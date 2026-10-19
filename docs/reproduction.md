# Reproduire la compilation et préparer un essai

## Environnement

| Élément | Valeur identifiée |
| --- | --- |
| Carte déclarée dans `ECG.ioc` | NUCLEO-L432KC |
| Microcontrôleur | STM32L432KCUx, Cortex-M4F |
| Fréquence système configurée | 80 MHz |
| CubeMX enregistré dans le projet | 6.13.0 |
| Paquet STM32Cube enregistré | STM32Cube FW_L4 V1.18.1 |
| IDE utilisé pour la vérification du dépôt | STM32CubeIDE 1.19.0 sous Windows |
| Compilateur utilisé | GNU Tools for STM32 13.3.rel1 |

Les sources HAL et CMSIS nécessaires sont incluses dans `firmware/Drivers/`. Il n'est pas nécessaire de régénérer le projet avec CubeMX pour compiler l'archive. Une régénération pourrait modifier le code ou des configurations ; travailler sur une copie si elle est souhaitée.

## Compilation dans l'IDE

1. Créer ou ouvrir un espace de travail distinct des archives originales.
2. Importer `firmware/` avec **File → Import → General → Existing Projects into Workspace**.
3. Vérifier que le projet importé se nomme `ECG` et sélectionner la configuration `Debug`.
4. Lancer **Project → Build Project**.
5. Vérifier que `firmware/Debug/ECG.elf` a été produit et consulter les diagnostics.

Les fichiers `.project`, `.cproject`, `ECG.ioc`, `Core/`, `Drivers/` et le script de liaison doivent rester ensemble dans `firmware/`. Les sorties `Debug/` sont générées et ne sont pas suivies par Git.

## Compilation sans interface

STM32CubeIDE fournit `headless-build.bat` sous Windows. Avec les chemins de son installation et d'un espace de travail dédié, utiliser :

```powershell
& 'C:\ST\STM32CubeIDE_1.19.0\STM32CubeIDE\headless-build.bat' `
  -data "$env:TEMP\ecg-build-workspace" `
  -import (Join-Path $PWD 'firmware') `
  -build 'ECG/Debug'
```

Exécuter depuis la racine du dépôt et adapter le chemin d'installation. Vérifier le récapitulatif de compilation et la création de l'ELF ; le code de sortie du lanceur seul ne suffit pas à établir la réussite.

## Interfaces relevées dans le firmware

Cette table décrit les broches du microcontrôleur, pas les numéros de connecteur d'une carte support.

| Signal | Broche MCU | Usage |
| --- | --- | --- |
| Entrée analogique ECG conditionnée | PB0 / ADC1_IN15 | ADC 12 bits, conversions continues |
| Indicateur de détection | PB1 | Sortie GPIO nommée `LED` |
| USART1 TX | PA9 | Envoi du signal traité vers l'ordinateur |
| USART1 RX | PA10 | Broche configurée pour l'USART1 |
| SPI3 SCK | PB3 | Horloge vers le MAX7219 |
| SPI3 MOSI | PB5 | Données vers le MAX7219 |
| Sélection matrice | PA15 | Le pilote configure cette broche comme sortie GPIO |
| SWD | PA13 / PA14 | Programmation et débogage |

L'USART1 est réglé à **115200 bauds, 8 bits, sans parité, 1 bit d'arrêt**, sans contrôle de flux. Le firmware émet une valeur entière du signal traité par ligne, terminée par `\r\n`. Il n'émet pas une seconde colonne BPM. La broche PA2 marquée `VCP_TX` appartient à l'USART2 : sa présence ne signifie pas que l'USART1 est automatiquement raccordé au port virtuel du ST-Link.

Le pilote MAX7219 reconfigure le SPI avec un diviseur de 256, après l'initialisation générée. Les paramètres de l'IOC ne décrivent donc pas à eux seuls tout l'état d'exécution. Le rapport décrit aussi une adaptation des niveaux logiques par TXS0108 entre le STM32 et le driver d'affichage.

## Préparer un essai sur banc

Aucun branchement ni flash de carte n'a été effectué pour préparer ce dépôt.

1. Retrouver le montage et les alimentations de la carte analogique d'origine ; la table de broches ne remplace pas son schéma électrique.
2. Utiliser d'abord une source de signal de banc adaptée à l'entrée ADC, sans personne reliée au montage, et contrôler les niveaux avant raccordement. Le dépôt ne fournit pas un protocole validé de mesure sur une personne.
3. Programmer la carte avec une configuration ST-Link créée pour le matériel effectivement disponible.
4. Raccorder l'USART1 à une interface série adaptée, puis configurer SerialPlot pour une colonne numérique en ASCII à 115200 bauds.
5. Vérifier la réception de valeurs, l'affichage et l'indicateur de détection. Mesurer l'intervalle d'acquisition effectif et comparer le rythme estimé à celui du signal de banc.

Ne pas annoncer une précision, une fréquence d'échantillonnage de 1 kHz ou une période réfractaire de 300 ms sans les mesurer : l'archive ne garantit pas ces propriétés. Consulter la [note technique](technical-notes.md) avant un essai du détecteur.
