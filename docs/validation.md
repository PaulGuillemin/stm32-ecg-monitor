# Provenance et vérification du dépôt

## Période et sources

- Projet scolaire : 2025, rapport et présentation datés du 27 mars 2025.
- Préparation du portfolio : 19 octobre 2026.
- Auteurs du rapport : Paul Guillemin et Tristan Lecourtois.

Deux copies du firmware ont été comparées : `ECG_backup` dans le dossier scolaire et `ECG` dans l'ancien espace de travail STM32CubeIDE 1.17.0. La seconde a été retenue : elle contient notamment la mise au carré du signal et la mise à jour du texte BPM, alors que la copie scolaire comporte un message fixe `BPM 120` et un appel de mise à jour commenté.

Cette sélection ne prouve pas que la copie retenue soit exactement celle de la soutenance. Elle constitue la version de travail la plus représentative retrouvée pendant cette préparation. Les écarts entre rapport et code sont consignés dans la [note technique](technical-notes.md).

## Sélection des fichiers

Les sources `Core/`, `Drivers/`, les fichiers de projet `.project` et `.cproject`, la configuration `ECG.ioc` et le script de liaison ont été conservés. Les sources et configurations suivies ont été comparées aux originaux par SHA-256 après compilation ; les éventuelles modifications automatiques de l'IDE ont été vérifiées.

Les anciens binaires, objets, fichiers de lancement personnels, réglages locaux, caches et supports de cours non nécessaires ne sont pas inclus dans l'historique. Les documents de projet sélectionnés sont copiés sans réécriture. Les figures conservées ne sont ni des images générées ni de nouvelles acquisitions.

Les photos et captures sélectionnées proviennent du dossier ECG. `led-display.jpg` est extrait de la figure 18, page 16 du rapport. Les images du README illustrent le projet de 2025 ; les périodes d'acquisition et les conditions ne sont pas suffisamment établies pour en tirer une comparaison chiffrée du rapport signal/bruit.

## Compilation effectuée

| Contrôle | Résultat |
| --- | --- |
| Importation dans un espace CubeIDE distinct | Projet `ECG` reconnu |
| Configuration | Debug |
| IDE | STM32CubeIDE 1.19.0 |
| Compilateur | GNU Tools for STM32 13.3.rel1 |
| Cible | STM32L432KCUx, Cortex-M4F, ABI flottante matérielle |
| Compilation et édition de liens | Réussies, 0 erreur et 0 avertissement |
| Binaire produit | `ECG.elf` |
| Résumé `arm-none-eabi-size` | text : 32348, data : 576, bss : 2720 octets |

Ces chiffres sont ceux de la construction Debug du 19 octobre 2026. `text`, `data` et `bss` sont les catégories du résumé de l'outil, pas une mesure de consommation ni du besoin maximal de pile à l'exécution. Les produits de construction sont exclus du dépôt.

## Vérifications documentaires

Les fichiers de projet ont été lus, les chemins locaux du README et de la documentation contrôlés, les images sélectionnées inspectées et les notices des composants tiers conservées. Une recherche ciblée d'identifiants, de clés et de chemins personnels a été effectuée sur les fichiers retenus. Elle ne constitue pas une garantie exhaustive d'absence de données sensibles.

## Vérifications non effectuées

- Aucun flash ni essai avec une carte physique.
- Aucune nouvelle acquisition ECG.
- Aucune mesure nouvelle de cadence d'échantillonnage, de précision de détection ou de consommation.
- Aucune certification ou validation clinique.

La compilation réussie valide la construction de l'archive, pas son bon fonctionnement matériel ou numérique. Le dépôt présente ce travail passé et ses limites connues.
