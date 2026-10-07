# Project Short — LTX Video sur Kaggle

Ce dépôt contient un workflow propre et reproductible pour transformer une image de référence en courte vidéo verticale sur Kaggle.

## Fichier principal

Utiliser :

`Project_Short_LTX_Kaggle_FIXED.ipynb`

L'ancien notebook `notebook9a4597f817.ipynb` est conservé uniquement comme historique.

## Pourquoi le notebook a été refait

L'ancien notebook mélangeait deux méthodes différentes et créait plusieurs problèmes :

- installation de versions incompatibles de `transformers`, `huggingface_hub` et `diffusers` ;
- ancien chemin `lina-t` réécrit après la détection correcte de `lina-testt` ;
- chargement complet du pipeline LTX sur une Tesla T4, provoquant `CUDA out of memory` ;
- cellules de patch mémoire ajoutées au fur et à mesure ;
- téléchargement manuel de fichiers qui n'était pas nécessaire avec le nouveau workflow.

Le nouveau notebook utilise une seule méthode : Diffusers + LTX Image-to-Video avec offload vers le CPU.

## Ordre des cellules

1. Vérification du GPU.
2. Installation des dépendances compatibles.
3. Chargement de LTX avec gestion mémoire.
4. Détection automatique de l'image dans Kaggle.
5. Paramètres du plan.
6. Génération et affichage du MP4.

## Première génération recommandée

Pour valider que tout fonctionne sur Tesla T4 :

- largeur : 320 ;
- hauteur : 576 ;
- frames : 33 ;
- FPS : 16 ;
- durée approximative : 2,06 s.

Après validation, augmenter progressivement la résolution ou la durée.

## Image actuelle

Le notebook recherche d'abord une image dans :

`/kaggle/input/datasets/pathemandiouba/lina-testt/`

S'il ne trouve rien, il recherche automatiquement la première image disponible dans `/kaggle/input/`.

## Nouvelle génération dans la même session

Il n'est pas nécessaire de recharger le modèle.

Modifier uniquement la cellule **PARAMÈTRES DU PLAN** :

- `PROMPT`
- `NEGATIVE_PROMPT`
- `WIDTH`
- `HEIGHT`
- `NUM_FRAMES`
- `FPS`
- `OUTPUT_PATH`

Puis relancer la cellule **GÉNÉRATION**.

## Après un redémarrage Kaggle

Relancer toutes les cellules dans l'ordre. Les fichiers déjà présents dans le cache Kaggle peuvent être réutilisés pendant la session, mais les variables Python doivent être recréées.
