# État technique du firmware conservé

Le firmware a été copié sans modification fonctionnelle depuis une version du projet CubeIDE d'origine. Les constats ci-dessous décrivent cette archive ; ils ne remettent pas en scène un développement historique et ne constituent pas une validation médicale.

## Chaîne réellement appelée

Dans `Core/Src/main.c`, `HAL_ADC_ConvCpltCallback()` appelle :

```text
ADC → remove_spikes → remove_outliers → low_pass_filter
    → moving_average_filter → square_signal
    → conversion en uint16_t → detect_r_peak → UART
```

- ADC : résolution 12 bits, canal 15, déclenchement logiciel et mode continu, sans DMA.
- Passe-bas : récurrence entière `y = (15 × y_précédent + x) / 16`.
- Moyenne glissante : fenêtre de 25 échantillons.
- Détection : entrée supérieure à 45000, verrou et compteur réfractaire ; le changement de signe de dérivée intervient dans le réarmement.
- Intervalle : TIM7 configuré avec un prédiviseur de 8000 à partir de 80 MHz, soit 10000 ticks/s. La formule du code est `bpm = 600000 / compteur`.
- Affichage : texte BPM mis à jour toutes les trois détections. Le pilote MAX7219 utilise le timer 16 pour le défilement.
- Série : signal traité, une valeur par ligne, envoi bloquant depuis l'interruption ADC.

## Écarts avec le rapport

| Présentation du rapport | Implémentation conservée |
| --- | --- |
| Butterworth d'ordre 2 à 10 Hz pour 1 kHz d'échantillonnage | Passe-bas récursif du premier ordre et moyenne glissante ; acquisition non déclenchée par un timer périodique |
| Pic repéré par seuil et passage de dérivée positive à négative | Déclenchement sur seuil ; dérivée utilisée pour le réarmement |
| Transmission du signal et du rythme cardiaque | Callback série émettant seulement le signal traité |
| Affichage OLED évoqué dans certains titres | Démonstrateur final documenté sur matrice LED commerciale MAX7219 |

Le fichier MATLAB `docs/design/filtre.fda` est conservé comme document de conception. Sa présence ne prouve pas que les coefficients correspondants sont implémentés dans ce firmware.

## Limites à traiter avant une réutilisation robuste

1. **Mise au carré et largeur numérique.** `square_signal()` renvoie un entier 32 bits, affecté ensuite à `uint16_t`. Le résultat est tronqué lorsque la valeur dépasse 65535, ce qui peut modifier la forme du signal et la détection.
2. **Suppression des pics parasites.** `remove_spikes()` affecte `previous_value = input` avant de retourner cette même valeur lorsque le seuil de variation est dépassé. Ce chemin ne rejette donc pas le pic comme son nom le suggère.
3. **État partagé.** `previous_value` est utilisé à la fois dans `remove_spikes()` et pour calculer la dérivée du signal traité. Il ne représente pas simplement l'échantillon filtré précédent.
4. **Bornes des valeurs aberrantes.** Les limites calculées en flottant sont converties en entiers non signés sans saturation explicite ; une borne basse négative doit être traitée avant conversion.
5. **Temps et période réfractaire.** Le compteur réfractaire est décrémenté par appel ADC, pas en millisecondes. La durée dépend du débit effectif, lui-même affecté par le calcul et l'émission UART bloquante.
6. **Calcul du rythme.** Le compteur TIM7 peut être nul ou déborder ; la division n'est pas protégée et l'intervalle n'est pas moyenné. Les macros de limites BPM ne suffisent pas à filtrer ce résultat dans le chemin consulté.
7. **Périphériques résiduels.** TIM15 est initialisé pour une capture, mais n'est pas utilisé dans le traitement principal. Le pilote réécrit certains réglages du SPI et de TIM16.

Ces points ont été documentés pendant la préparation du portfolio. Aucun correctif algorithmique n'a été appliqué : une éventuelle version améliorée devra faire l'objet de changements datés du moment de leur réalisation et de tests distincts.
