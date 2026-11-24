# TP RTI3 avec GIMP – méthodes pas à pas

[← Retour au sommaire](README.md) · [Antisèche](antiseche.md)

Écrit pour **GIMP 2.10.34** (la version des postes de l'IUT). Si un menu n'est pas là où je l'indique, tape **/** dans GIMP : ça ouvre la recherche d'actions, et tu peux taper « seuil », « courbes », « convolution »…

## Avant de commencer

| Réflexe | Où |
|---|---|
| Travailler sur une copie pour comparer avant/après | Image › **Dupliquer** (Ctrl+D) |
| Vérifier que l'image est en RVB | Image › Mode › **RVB** |
| Afficher l'histogramme en permanence | Fenêtres › Fenêtres ancrables › **Histogramme** |
| Lire la valeur d'un pixel | Fenêtres › Fenêtres ancrables › **Pointeur** |
| Annuler / tout recommencer | Ctrl+Z / Fichier › Rétablir |

**Spécial 2.10 : linéaire ou perceptuel.** Les fenêtres Histogramme, Niveaux et Courbes ont deux petits boutons en haut à droite : **linéaire** et **perceptuel**. Choisis **perceptuel** pour avoir le même histogramme et les mêmes valeurs (0 à 255) que dans le cours et dans Photoshop. En linéaire, l'histogramme a une autre forme et les valeurs ne correspondent plus.

---

## TP1 – Optimisation des images

### Rappels théoriques : résolution, définition, poids

Deux fenêtres différentes dans GIMP, c'est ce qui rend la notion claire :

- **Image › Échelle et taille de l'image** : change les **pixels** (la définition). Si tu changes la résolution ici, GIMP ajoute ou retire des pixels → **le poids change**.
- **Image › Taille d'impression** : change seulement les **cm** et la **résolution**, sans toucher aux pixels → **le poids ne change pas**.

Pour répondre aux questions :

1. *Taille constante, je change la résolution* → Échelle et taille de l'image, unité en **cm**, garde 10 cm et change la résolution. Les pixels augmentent (résolution × 2 = pixels × 2 en largeur et en hauteur = **poids × 4**).
2. *Définition constante, je change la résolution* → Taille d'impression. Les cm diminuent, les pixels et le **poids restent identiques**.
3. Conclusion : **le poids ne dépend que du nombre de pixels.**

Calculs utiles :
- Pixels = cm ÷ 2,54 × ppp (ex. 10 cm à 72 ppp = 283 px)
- Poids d'une image RVB non compressée (BMP) = largeur × hauteur × 3 octets

### Retour sur la SAÉ 204 (image dans Word)

1. Dans Word/LibreOffice : Insertion › Image › tournesol.bmp, la réduire à 10 cm de haut, enregistrer en .docx, noter le poids (clic droit › Propriétés).
2. Dans GIMP : ouvrir tournesol.bmp › **Image › Échelle et taille de l'image** → unité **cm**, hauteur **10**, chaîne de proportions fermée, **résolution inchangée** › Mise à l'échelle.
3. **Fichier › Exporter sous** › nom en **.bmp** › Exporter.
4. Insérer ce nouveau BMP dans un nouveau .docx et comparer : **beaucoup plus léger**. Word garde tous les pixels même si on réduit l'image à la souris.

### Optimiser les images selon le zoning

Pour chaque image :

1. **Redimensionner** : Image › Échelle et taille de l'image › unité **px**, mets la largeur de la zone (la hauteur suit) › Interpolation **Cubique** ou **NoHalo** › Mise à l'échelle.
   - Si le format ne correspond pas à la zone : **Image › Taille du canevas** ou l'outil **Découpage** (Maj+C) avec un ratio fixe.
2. **Exporter** : Fichier › **Exporter sous** (Maj+Ctrl+E) › tape le nom **avec l'extension voulue** (.jpg, .png, .webp, .gif) : c'est l'extension qui choisit le format.

#### Photo → JPEG

Dans la fenêtre d'export JPEG :
- **Qualité** : 60 à 80.
- Coche **« Afficher l'aperçu dans la fenêtre d'image »** : GIMP affiche la **taille du fichier** en direct. Baisse la qualité tant que tu ne vois pas de défauts.
- Ouvre **Options avancées** : coche **Progressif**, décoche **Enregistrer les données EXIF, XMP, IPTC** et **Enregistrer la vignette** (= supprimer les métadonnées).

#### Logo, aplats, peu de couleurs → PNG-8 ou GIF

1. **Image › Mode › Couleurs indexées** › « Générer une palette optimale », nombre max de couleurs (256, ou moins : 64, 32… tant que ça reste propre) › Convertir. C'est ce qui fait un **PNG-8**.
2. Exporter en **.png** : niveau de compression **9**, décoche les métadonnées (EXIF, XMP, commentaire, vignette).
- Pour un **PNG-24** (transparence douce, qualité parfaite) : reste en mode RVB, ne passe pas en couleurs indexées.

#### WebP

Exporter en **.webp** : choisir **Avec perte** + qualité 75–80 pour une photo, ou **Sans perte** pour un graphisme. Décoche les métadonnées.

#### Justifier ses choix (à écrire dans le compte rendu)

Pour chaque image : **définition finale** (px), **format** + pourquoi (photo / aplat / transparence), **qualité**, **poids avant → après**.

### Le processeur d'images (traitement par lot)

**GIMP n'a pas de processeur d'images intégré.** Selon ce qui est installé sur le poste :

1. **Photoshop dispo ?** Fais cette partie sur Photoshop : Fichier › Scripts › Processeur d'images (voir l'[antisèche](antiseche.md#tp1--optimiser-des-images-photoshop-gimp-word)).
2. **Plugin BIMP** dans GIMP 2.10, s'il est installé (menu Fichier › Batch Image Manipulation…) : Add › Resize (300 × 200, garder les proportions) + Change format (JPEG, qualité ≈ 80) › choisir le dossier de sortie › Apply.
3. **XnConvert / IrfanView** (souvent installés) : IrfanView › Fichier › Conversion/Renommage par lot › Format JPG, Options qualité ≈ 80, « Utiliser options avancées » › Redimensionner 300 × 200 en gardant les proportions.

À commenter : l'image tient **dans** 300 × 200 sans être déformée (300 × 200 pour une photo 3:2, 300 × 169 pour du 16:9…), et les JPEG sont beaucoup plus légers que les BMP. La qualité 8 de Photoshop (sur 12) correspond à peu près à 70–80 % ailleurs.

---

## TP2 – Traitement des images

### Histogramme

**Fenêtres › Fenêtres ancrables › Histogramme**

- Menu déroulant en haut : **Valeur** (luminosité), **Rouge**, **Vert**, **Bleu**, **RVB** (les 3 superposés).
- Boutons à droite : échelle **linéaire** ou **logarithmique**.
- En bas : **Moyenne**, **Écart-type**, **Médiane**, **Pixels**, comme dans le cours.
- Voiliers : image sombre → pics à gauche, image claire → pics à droite, tons moyens → au milieu.

### Courbe tonale

**Couleurs › Courbes**

- **Canal** : Valeur (ou un canal R, V, B).
- Clique sur la diagonale pour créer un point, tire-le : **au-dessus de la diagonale = plus clair**, **en dessous = plus foncé**. Courbe en **S** = plus de contraste.
- Pour enlever les points : bouton **Réinitialiser le canal** (flèche circulaire).
- Coche **Aperçu**, et regarde la fenêtre Histogramme qui bouge en direct.

### Niveaux

**Couleurs › Niveaux**

- **Niveaux d'entrée** : 3 triangles sous l'histogramme (noir, gris = gamma, blanc).
- **Niveaux de sortie** : 2 triangles en bas (noir, blanc).
- Rapprocher les triangles de **sortie** → moins de contraste, image terne (histogramme resserré).
- Les triangles d'**entrée** noir et blanc : tout ce qui est avant le noir devient noir, tout ce qui est après le blanc devient blanc.

**Étirement sur Tournesol2** : dans Niveaux, amène le triangle **noir** au début de la « montagne » de l'histogramme et le triangle **blanc** à sa fin › Valider. L'histogramme s'étale sur toute la largeur = plus de contraste.

**Comparer avec l'automatique** : **Couleurs › Auto › Étirer le contraste** (l'équivalent de « Contraste automatique » de Photoshop). Le bouton **Auto** dans la fenêtre Niveaux fait la même chose.

### Égalisation

**Couleurs › Auto › Égaliser**

L'histogramme devient étalé et « en peigne » : on tend vers le même nombre de pixels par niveau → plus de nuances visibles.

### Inversion (négatif)

**Couleurs › Inverser**

L'histogramme est retourné en miroir (ce qui était à gauche passe à droite). Chaque valeur devient 255 − v.

### Seuillage – trouver le seuil d'Objets_dynamiques2

**Couleurs › Seuil**

Laisse le **Canal** sur **Valeur**.

Dans GIMP le seuil a **deux valeurs** (bas et haut) : les pixels **entre** les deux deviennent blancs, les autres noirs. C'est donc directement le « double seuillage » du cours.

- **Seuillage simple** : laisse la valeur haute à **255** et déplace seulement la valeur **basse**.
- **Méthode pour trouver le seuil** :
  1. Ouvre Objets_dynamiques et Objets_dynamiques2 côte à côte.
  2. Sur Objets_dynamiques : Couleurs › Seuil, coche Aperçu, change la valeur basse jusqu'à obtenir exactement la même image › Valider.
  3. **Vérification** : sur Objets_dynamiques2, Ctrl+A puis Ctrl+C. Reviens sur ton image seuillée, **Édition › Coller comme › Nouveau calque**. Dans la fenêtre **Calques**, mets le **Mode** de ce calque sur **Différence**.
  4. Si l'image est **entièrement noire** (histogramme : moyenne = 0), c'est le bon seuil. S'il reste des pixels blancs, ajuste de ±1 et recommence.
- Valeur trouvée par d'autres : **≈ 180 sous GIMP** (≈ 190 sous Photoshop). À vérifier avec la méthode ci-dessus.

### Filtrage – matrice de convolution

**Filtres › Génériques › Matrice de convolution**

- La grille fait **5 × 5** : utilise seulement le **carré 3 × 3 du centre**, laisse le reste à 0. La case centrale = le pixel traité.
- **Diviseur** = l'« Échelle » de Photoshop : la somme des coefficients.
- **Décalage** : ajouté au résultat. **Dans GIMP 2.10, il va de −1 à 1** (et non de 0 à 255) : pour l'équivalent du décalage 128 de Photoshop, mets **0,5**.
- **Normaliser** : si coché, GIMP calcule le diviseur tout seul (pratique pour les flous). **Décoche-le** pour Sobel et Laplace.
- **Bordure** : Étendre (ou Recadrer). Coche **Aperçu**.

| Filtre | Grille 3 × 3 (lignes) | Diviseur | Décalage | Résultat |
|---|---|---|---|---|
| Moyenneur (passe-bas) | 1 1 1 / 1 1 1 / 1 1 1 | 9 | 0 | Flou, débruitage |
| Gaussien (passe-bas) | 1 2 1 / 2 4 2 / 1 2 1 | 16 | 0 | Flou plus doux |
| Sobel horizontal (passe-haut) | 1 0 −1 / 2 0 −2 / 1 0 −1 | 1 | 0,5 | Contours verticaux |
| Laplace (passe-haut) | 0 1 0 / 1 −4 1 / 0 1 0 | 1 | 0,5 | Contours dans toutes les directions |

- Sans décalage, Sobel et Laplace donnent une image **presque noire** avec les contours en clair : c'est normal, la somme des coefficients vaut 0.
- Pour comparer : applique sur une copie (Image › Dupliquer) et mets les deux fenêtres côte à côte.
- Conclusion à écrire : **passe-bas = flou / lissage du bruit**, **passe-haut = contours / accentuation**.
- Filtres tout faits pour aller plus loin : Filtres › Flou › Flou gaussien ; Filtres › Détection de bord › Sobel / Laplace ; Filtres › Amélioration › Netteté (masque flou).

---

## Le menu Couleurs (courbes et histogrammes)

Dans GIMP 2.10, presque tout ce qui touche à l'histogramme est dans le menu **Couleurs** :

| Menu | Ce que ça fait | Effet sur l'histogramme |
|---|---|---|
| Couleurs › **Info › Histogramme** | Ouvre la fenêtre Histogramme | Lecture seule (Moyenne, Écart-type, Médiane, Pixels) |
| Couleurs › **Niveaux** | Curseurs d'entrée et de sortie | Entrée resserrée → étiré ; sortie resserrée → compressé |
| Couleurs › **Courbes** | Courbe tonale (la LUT du cours) | Au-dessus de la diagonale → décalé à droite (plus clair) ; en dessous → à gauche |
| Couleurs › **Luminosité-Contraste** | Réglage simple | Luminosité : tout l'histogramme glisse ; contraste : il s'étale ou se resserre |
| Couleurs › **Exposition** | Corrige une photo sur- ou sous-exposée | Glisse vers la droite ou la gauche |
| Couleurs › **Inverser** | Négatif (255 − v) | Miroir |
| Couleurs › **Seuil** | Binarisation | Il ne reste que 2 pics : noir et blanc |
| Couleurs › **Postériser** | Réduit le nombre de niveaux | Quelques barres séparées |
| Couleurs › **Auto › Égaliser** | Égalisation | Étalé, « en peigne » |
| Couleurs › **Auto › Étirer le contraste** | Étirement automatique | Le min va à 0 et le max à 255 |

Astuces dans les fenêtres **Niveaux** et **Courbes** (2.10) :

- En haut, mets **Canal = Valeur** pour la luminosité, ou R, V, B pour une seule couleur. Coche **Aperçu**.
- Les deux boutons **linéaire / perceptuel** : prends **perceptuel**.
- Dans Niveaux, le bouton **« Modifier ces paramètres en tant que courbes »** convertit le réglage en courbe tonale. C'est pratique pour montrer qu'un réglage de niveaux **est** une courbe (lien niveaux ↔ courbe tonale du cours).
- Dans Niveaux, les **pipettes** (noir, gris, blanc) fixent le point noir et le point blanc en cliquant sur l'image.
- Le menu **Préréglages** permet d'enregistrer un réglage pour le réappliquer.
- **Courbes : les formes à connaître**

| Forme de la courbe | Opération du cours |
|---|---|
| Diagonale | Aucune modification |
| Au-dessus de la diagonale | Éclaircir |
| En dessous de la diagonale | Assombrir |
| En S | Plus de contraste |
| Droite plus pentue, coupée en bas et en haut | Étirement d'histogramme |
| Diagonale descendante | Inversion (négatif) |
| Marche verticale (0 puis 255 d'un coup) | Seuillage |

---

## Résumé des menus GIMP

| Manip | Menu GIMP |
|---|---|
| Changer pixels / résolution (poids change) | Image › Échelle et taille de l'image |
| Changer cm / résolution (poids identique) | Image › Taille d'impression |
| Recadrer | Outil Découpage (Maj+C) ou Image › Taille du canevas |
| Exporter (JPEG, PNG, WebP, GIF, BMP) | Fichier › Exporter sous (Maj+Ctrl+E) |
| PNG-8 / GIF (≤ 256 couleurs) | Image › Mode › Couleurs indexées |
| Histogramme | Fenêtres › Fenêtres ancrables › Histogramme |
| Courbes | Couleurs › Courbes |
| Niveaux | Couleurs › Niveaux |
| Étirement automatique | Couleurs › Auto › Étirer le contraste |
| Égaliser | Couleurs › Auto › Égaliser |
| Négatif | Couleurs › Inverser |
| Seuil | Couleurs › Seuil |
| Convolution | Filtres › Génériques › Matrice de convolution |
| Mode Différence (vérifier le seuil) | Fenêtre Calques › Mode › Différence |
| Chercher une commande | Touche **/** |
