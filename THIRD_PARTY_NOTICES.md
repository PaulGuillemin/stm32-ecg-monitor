# Auteurs, composants et provenance

## Travail étudiant

Le rapport et la présentation de mars 2025 sont signés **Paul Guillemin et Tristan Lecourtois**, Mines Saint-Étienne. Leur contenu collectif et les documents techniques réutilisés ne sont pas placés automatiquement sous une nouvelle licence par la préparation de ce dépôt.

La répartition individuelle des travaux n'est pas détaillée dans les documents conservés. Les crédits du README distinguent le travail du binôme des composants réutilisés.

## Logiciel

| Composant | Attribution présente dans les sources | Notice conservée |
| --- | --- | --- |
| HAL STM32L4 | STMicroelectronics | `firmware/Drivers/STM32L4xx_HAL_Driver/LICENSE.txt` |
| CMSIS | ARM et contributeurs indiqués dans les fichiers | `firmware/Drivers/CMSIS/LICENSE.txt` |
| Support du composant STM32L4 | STMicroelectronics | `firmware/Drivers/CMSIS/Device/ST/STM32L4xx/LICENSE.txt` |
| Code d'initialisation généré | STMicroelectronics, en-têtes des fichiers `Core/` | Mentions originales conservées |
| Pilote matrice MAX7219 | Khaled Magdy / DeepBlueMbedded, en-têtes de `DOT_MATRIX.c`, `.h` et fichiers `cfg` | Attribution originale conservée ; aucune licence supplémentaire identifiée dans ces fichiers |

Ce dépôt n'attribue pas une licence globale nouvelle à des fichiers tiers. Les notices et conditions propres à chaque composant restent à examiner pour toute redistribution envisagée.

## Images et documents

- Les captures de signaux, la photo de fabrication et le dessin de carte support proviennent des archives du projet ECG.
- La photo `docs/media/led-display.jpg` est extraite de la figure 18 du rapport collectif.
- Le rapport contient un schéma de carte ECG portant l'attribution François Bernier / EMSE / ID-Fab. Ce schéma n'est pas revendiqué comme une conception originale de Paul ou de Tristan.
- Les références bibliographiques et illustrations tierces du rapport conservent les attributions portées dans ce document.

Les autorisations de publication du travail collectif et des éléments tiers doivent être établies avant une diffusion publique. La présence locale de ces fichiers ne constitue pas à elle seule une autorisation de les relicencier.
