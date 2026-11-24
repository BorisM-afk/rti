# RTI3 – L'essentiel (antisèche)

[← Retour au sommaire](README.md) · [Fiches complètes](fiches-completes.md) · [Méthodes GIMP](gimp.md)

## 1. Les formules, expliquées simplement

Pas besoin d'être matheux : pour presque tout, il suffit de retenir des correspondances. Les calculs à la calculatrice sont donnés au cas où.

### Les décibels (Tenacity : Effet › Amplifier)

Un décibel (dB) dit de combien le son devient plus fort ou plus faible. Retiens juste ceci :

| Tu ajoutes… | L'onde devient… | Exemple dans Tenacity |
| --- | --- | --- |
| **+6 dB** | **2 fois plus haute** | 0,25 → 0,5 |
| **+12 dB** | 4 fois (6 + 6) | 0,25 → 1 |
| **+20 dB** | **10 fois plus haute** | 0,05 → 0,5 |
| **−3 dB** | il en reste **environ 70 %** | 1 → 0,707 |
| **−6 dB** | 2 fois plus petite | 1 → 0,5 |
| **−20 dB** | **10 fois plus petite** | 1 → 0,1 |

Astuce pour les tableaux du TP3 : quand on multiplie, on **additionne** les dB.

- ×2 = 6 · ×4 = 12 · ×8 = 18 · ×16 = 24 · ×32 = 30 (on ajoute 6 à chaque fois)
- ×1 = 0 · ×10 = 20 · ×100 = 40 · ×1000 = 60 (on ajoute 20 à chaque fois)

Si on te demande le calcul : **20 × log(sortie ÷ entrée)**, avec la touche **log** de la calculatrice. Exemple : 0,5 ÷ 0,25 = 2, log(2) = 0,301, et 0,301 × 20 = 6 dB.

**Lire l'amplitude dans Tenacity** : regarde le haut de l'onde sur l'échelle à gauche de la piste (elle va de −1 à 1). Zoome verticalement si besoin.

### Les bits en audio (Édition › Préférences › Qualité)

Plus il y a de bits, plus l'écart possible entre le son le plus faible et le plus fort est grand. C'est la « gamme dynamique ». Règle simple : **chaque bit vaut environ 6 dB**.

- 16 bits × 6 ≈ **96 dB** (qualité CD, le réglage du TP)
- 8 bits × 6 ≈ **48 dB**

### Les filtres audio (Effet › Filtre passe-bas / passe-haut)

- **Fréquence de coupure** : la frontière entre ce qui passe et ce qui est coupé (1000 Hz par défaut dans Tenacity). Pile à cette fréquence, il reste environ 70 % du signal (−3 dB).
- **Bande passante** : toutes les fréquences qui passent sans presque rien perdre (il reste au moins 70 %).
- **« 6 dB par octave »** : chaque fois que la fréquence double après la coupure, l'onde est encore divisée par 2. **« 20 dB par décade »** veut dire la même chose : fréquence ×10, onde ÷10.
- Le **passe-bas** garde les graves et coupe les aigus. Le **passe-haut** fait l'inverse.

### Pixels, résolution et poids (Photoshop : Image › Taille de l'image)

- **Définition** = le nombre de pixels (par exemple 1920 × 1080). **C'est elle qui fait le poids du fichier.**
- **Résolution** (ppp ou dpi) = combien de pixels on met dans un pouce à l'impression. Sur un écran, elle ne change rien.
- **Taille en cm** = les pixels répartis selon la résolution. Plus de ppp donne une image plus petite en cm.
- Dans la fenêtre Taille de l'image :
  - **Rééchantillonnage coché** : changer la résolution ajoute ou retire des pixels, donc **le poids change**.
  - **Décoché** : seuls les cm changent, **le poids ne bouge pas**.
- Réponses du TP1 : même taille en cm et plus de ppp → plus de pixels → plus lourd. Mêmes pixels et plus de ppp → plus petit en cm, même poids.

### La matrice de convolution (Photoshop : Filtre › Divers › Autre)

- La grille de chiffres dit comment mélanger chaque pixel avec ses voisins. La case du milieu correspond au pixel lui-même.
- **Que des chiffres positifs** : on fait une moyenne avec les voisins, donc l'image devient **floue** (passe-bas).
- **Des positifs et des négatifs** : on calcule les différences entre voisins, donc on voit les **contours** (passe-haut).
- Case **Échelle** : mets la somme des chiffres de la grille (9 pour le moyenneur, 16 pour le gaussien), sinon l'image devient trop claire. Si la somme vaut 0 (Sobel, Laplace), mets 1.
- Case **Décalage** : ajoute de la luminosité. Avec Sobel ou Laplace, l'image sort presque noire ; mets 128 pour voir les contours sur un fond gris.

### Le codage entropique (le TD)

L'idée, comme en morse : **les lettres fréquentes ont un code court, les lettres rares un code long.**

- **Quantité d'information Q** = combien de bits mérite une lettre. À la calculatrice : **log(1 ÷ p) ÷ log(2)**. Exemple pour A (p = 0,42) : 1 ÷ 0,42 = 2,38 ; log(2,38) = 0,377 ; 0,377 ÷ 0,301 = **1,25**, qu'on arrondit au-dessus, donc 2 bits.
- **Entropie H** = le nombre moyen de bits idéal, qu'on ne peut pas battre. Pour chaque lettre, fais **p × Q**, puis additionne tout. Pour l'alphabet MMI : ≈ 2,45.
- **Longueur moyenne L** = la même chose avec ton vrai code. Pour chaque lettre, fais **p × le nombre de bits de son code**, puis additionne. Plus L est proche de H, meilleur est le code.

## 2. Image : ce qui retombe toujours

- **Histogramme** : abscisse noir (gauche) → blanc (droite). Le modifier ne crée pas d'information, il la rend plus visible.
- **Courbe tonale (LUT)** : diagonale = aucun changement · **au-dessus = plus clair** · **en dessous = plus foncé**.
- **Étirement** : ramener le min à 0 et le max à 255 → **plus de contraste** (Niveaux : rapprocher les curseurs noir et blanc).
- **Égalisation** : même nombre de pixels par niveau → plus de nuances.
- **Seuillage** : sous le seuil → noir, au-dessus → blanc → **image binaire**.
- **Inversion** : 255 − v → négatif, histogramme en miroir.
- **Passe-bas** = **flou / débruitage** (moyenneur 1/9 [1 1 1…], gaussien 1/16 [1 2 1 / 2 4 2 / 1 2 1]).
- **Passe-haut** = **contours / netteté** (Sobel [1 0 −1 / 2 0 −2 / 1 0 −1], Laplace [0 1 0 / 1 −4 1 / 0 1 0]). Si la somme des coefficients vaut 0, seuls les contours restent.

## 3. Images web : le réflexe

1. **Redimensionner** dans le logiciel (pas en HTML) : seuls les pixels comptent.
2. **Format** : photo → **JPEG ou WebP** · aplats et logos → **PNG ou GIF** · transparence → **PNG ou WebP** (jamais JPEG).
3. **Exporter pour le web** : régler la qualité et **supprimer les métadonnées**.

**Processeur d'images** : Fichier › Scripts › Processeur d'images → dossier source, dossier de sortie, JPEG + taille + qualité, **inclure le profil ICC**, Exécuter.

## 4. Vidéo : ce qui retombe toujours

- La compression supprime la **redondance spatiale** (dans l'image, comme M-JPEG) et la **redondance temporelle** (entre les images, comme MPEG).
- **I** = image complète (7:1) · **P** = prédite depuis l'image d'avant (20:1) · **B** = bidirectionnelle (50:1).
- **GOP M = 3, N = 12** → IBBPBBPBBPBB, soit 27:1 en moyenne.
- Les normes en une ligne : **MPEG-1** = CD, VHS · **MPEG-2** = DVD, TNT SD · **H.264/AVC** = −50 % vs MPEG-2, Blu-ray, TNT · **H.265/HEVC** = −50 % vs H.264, 4K.
- **Codec** = algorithme de compression (H.264) · **conteneur** = le fichier qui l'emballe (MP4, MOV, AVI, MKV).
- **Export web** : MP4, H.264, 720p, 24 i/s, VBR 2 passes, 2,5 à 5 Mbit/s, son AAC 48 kHz.

## 5. Codage entropique

- **Toujours trier par probabilité décroissante d'abord.**
- **Fano** (de haut en bas) : couper en 2 groupes de probabilité ≈ égale, 0 en haut, 1 en bas, recommencer.
- **Huffman** (de bas en haut) : fusionner les 2 plus petites (0 à la plus faible), retrier, recommencer. C'est le code **optimal**.
- Corrigé de l'alphabet MMI : **H ≈ 2,45** · fixe = 3 bits · direct ≈ 2,98 · Fano = Huffman = **2,49**.

## 6. Audio (Tenacity)

**S(t) = A·sin(2πft + φ)**

- **A** → dynamique (normaliser, noise gate, réduction de bruit)
- **f** → égalisation (EQ, filtres, preset Téléphone)
- **φ** → modulation (phaser, flanger, chorus)
- **t** → spatialisation (écho, delay, réverbération)

**Filtres** (fc = 1000 Hz, entrée 1 V) : à fc on obtient **0,707 V**. Le passe-bas laisse passer 100 Hz (≈ 1 V) et coupe 10 kHz (≈ 0,1 V). Le passe-haut fait l'inverse.

**Réduction de bruit** : 1) sélectionner du bruit seul et prendre le profil, 2) tout sélectionner et appliquer.

**Réglages** : 44 100 Hz, 16 bits, export WAV 16 bits PCM.

## 7. Antisèche de manip, TP par TP

Les noms des menus peuvent varier un peu selon ta version. Si tu ne trouves pas, tape le nom de l'outil dans la recherche (Ctrl+F dans Photoshop).

### TP1 : optimiser des images (Photoshop, GIMP, Word)

| Je veux… | Photoshop | GIMP |
| --- | --- | --- |
| Changer pixels, cm ou ppp | Image › Taille de l'image (Alt+Ctrl+I) | Image › Échelle et taille de l'image |
| Changer les cm **sans toucher aux pixels** | Décocher **Rééchantillonnage** | Image › Taille d'impression |
| Exporter pour le web | Fichier › Exporter › Enregistrer pour le web (hérité) (Alt+Maj+Ctrl+S) | Fichier › Exporter sous, puis choisir .jpg, .png ou .webp |
| Supprimer les métadonnées | Dans Enregistrer pour le web : Métadonnées = **Aucune** | Décocher EXIF, XMP et miniature à l'export |
| Traiter un dossier entier | Fichier › Scripts › **Processeur d'images** | Pas d'équivalent simple (BIMP ou XnConvert) |

- **Poids d'un fichier** : clic droit › Propriétés dans l'explorateur. Dans Enregistrer pour le web, le poids est affiché sous l'aperçu. Passe en vue **2 vignettes** pour comparer avant/après.
- **Exercice Word** : Insertion › Image, puis réduire à 10 cm. Le .docx reste lourd, car Word garde tous les pixels. Ensuite, réduire d'abord dans Photoshop et réinsérer : le .docx est beaucoup plus léger.
- **Optimisation selon le zoning** : regarde la taille en pixels de chaque emplacement, redimensionne l'image à cette taille, choisis le format (photo → JPEG qualité 60 à 80 ou WebP ; logo ou aplat → PNG-8 ou GIF ; transparence → PNG-24 ou WebP), puis note le poids obtenu pour justifier.
- **Processeur d'images, réglages du TP** : ① dossier BMP → ② dossier de sortie → ③ Enregistrer au format JPEG, Qualité 8, cocher Redimensionner : L 300, H 200 → ④ cocher Inclure le profil ICC → Exécuter. À commenter : l'image tient **dans** un cadre de 300 × 200 sans être déformée. Une image en portrait fera donc 133 × 200 et pas 300 × 200. Les fichiers sont tous beaucoup plus légers que les BMP.

### TP2 : traitement d'image (Photoshop, GIMP)

| Je veux… | Photoshop | GIMP |
| --- | --- | --- |
| Voir l'histogramme | Fenêtre › Histogramme (menu du panneau › Vue développée, puis choisir la couche) | Fenêtres › Fenêtres ancrables › Histogramme |
| Courbe tonale | Image › Réglages › **Courbes** (Ctrl+M) | Couleurs › Courbes |
| Niveaux | Image › Réglages › **Niveaux** (Ctrl+L) | Couleurs › Niveaux |
| Contraste automatique | Image › Contraste automatique | Couleurs › Auto › Étirer le contraste |
| Égaliser | Image › Réglages › **Égaliser** | Couleurs › Auto › Égaliser |
| Négatif | Image › Réglages › **Négatif** (Ctrl+I) | Couleurs › Inverser |
| Seuil | Image › Réglages › **Seuil** | Couleurs › Seuil |
| Convolution | Filtre › Divers › **Autre** | Filtres › Génériques › Matrice de convolution |

- **Courbes** : clique sur la diagonale pour créer un point, puis tire-le vers le haut (plus clair) ou vers le bas (plus foncé). Pour supprimer un point, fais-le glisser hors du graphique. Garde le panneau Histogramme ouvert pour voir les changements en direct.
- **Niveaux** : les 3 curseurs du haut sont les niveaux d'**entrée** (noir, gris, blanc). Ceux du bas sont les niveaux de **sortie**.
  - **Étirement (Tournesol2)** : tire le curseur noir jusqu'au début de la « montagne » et le curseur blanc jusqu'à sa fin. Le contraste augmente et l'histogramme s'étale sur toute la largeur. Compare ensuite avec Contraste automatique, qui doit donner un résultat proche.
  - Rapprocher les curseurs de sortie → moins de contraste, image plus terne.
- **Égaliser** : l'histogramme devient étalé et plus « plat ». **Négatif** : l'histogramme est retourné comme dans un miroir.
- **Trouver le seuil utilisé (Objets_dynamiques2)** : ouvre les deux images côte à côte. Sur Objets_dynamiques, ouvre Seuil et bouge le curseur jusqu'à obtenir exactement la même image, puis lis la valeur. Pour vérifier, mets le résultat en calque au-dessus de Objets_dynamiques2 avec le mode de fusion **Différence** : si tout est noir, c'est le bon seuil.
- **Convolution** : tape les chiffres dans la grille, la case du centre correspondant au pixel. Puis remplis Échelle et Décalage (voir partie 1) et coche Aperçu.
  - Moyenneur : neuf 1, Échelle 9 → flou.
  - Gaussien : 1 2 1 / 2 4 2 / 1 2 1, Échelle 16 → flou plus doux.
  - Sobel : 1 0 −1 / 2 0 −2 / 1 0 −1, Échelle 1, Décalage 128 → contours.
  - Laplace : 0 1 0 / 1 −4 1 / 0 1 0, Échelle 1, Décalage 128 → contours dans toutes les directions.

### TP3 : amplification et filtres (Tenacity)

| Je veux… | Menu Tenacity |
| --- | --- |
| Créer un son de test | Générer › **Tonalité** : forme Sinus, fréquence 1000 Hz, amplitude 0,25, durée 1 s |
| Amplifier | Sélectionner tout (Ctrl+A) › Effet › **Amplifier** → Amplification (dB) = 6 |
| Passe-bas ou passe-haut | Effet › **Filtre passe-bas** ou **Filtre passe-haut** → 1000 Hz, pente 6 dB |
| Annuler pour recommencer | Ctrl+Z |

- **Lire l'amplitude** : regarde le haut de l'onde sur l'échelle verticale à gauche (zoom avec Ctrl+molette sur l'échelle, ou clic sur la règle). Autre méthode : ouvre Effet › Amplifier sans valider. Tenacity propose l'amplification qui mènerait le pic à 0 dB (1). S'il propose environ 12 dB, ton pic vaut 0,25.
- Si Amplifier refuse de dépasser 1, coche **Permettre l'écrêtage**. Ce n'est normalement pas nécessaire dans ce TP.
- **Résultats attendus** : 0,25 + 6 dB = 0,5 ; 0,25 + 12 dB = 1 ; 0,05 + 20 dB = 0,5. Filtres sur 1 V : voir la partie 6.
- Fais toujours **un nouveau son par mesure** (Générer à nouveau), pour ne pas filtrer un signal déjà filtré.

### TP4 : effets audio (Tenacity, casque et micro)

| Je veux… | Menu Tenacity |
| --- | --- |
| Régler la qualité | Édition › Préférences › Qualité → 44 100 Hz, 16 bits |
| Nouvelle piste stéréo | Pistes › Ajouter nouvelle › Piste stéréo |
| Enregistrer | Bouton rouge ou touche **R**, puis **Espace** pour arrêter |
| Exporter en WAV | Fichier › Exporter › Exporter en WAV → type Signed 16-bit PCM, puis effacer les métadonnées dans la fenêtre suivante |
| Voir le spectre | Sélection › Analyse › **Tracer le spectre** → Hamming, taille 512, axe linéaire |
| Égaliser | Effet › **Courbe de filtrage EQ** (ou Égaliseur graphique) |
| Préréglage Téléphone | Dans Courbe de filtrage EQ : Gérer › Préréglages d'usine › Telephone |
| Normaliser | Effet › **Normaliser** → pic maximal (par exemple −1 dB, puis −6 dB pour comparer) |
| Réduction de bruit | Effet › **Réduction du bruit** (2 étapes, voir ci-dessous) |
| Modulation | Effet › **Phaser** |
| Spatialisation | Effet › **Écho**, **Delay**, **Réverbération** (prendre les préréglages) |

- **Réduction de bruit, pas à pas** : ① sélectionne un passage où l'on n'entend **que le bruit** (avant que tu parles) → ② Effet › Réduction du bruit › **Prendre le profil du bruit** → ③ Ctrl+A pour tout sélectionner → ④ rouvre Réduction du bruit › **Valider**. Compare ensuite le spectre avant et après.
- **Courbe EQ** : clique sur la ligne pour ajouter des points et tire-les vers le haut pour monter une zone de fréquences, vers le bas pour la baisser. Graves à gauche, aigus à droite.
- **À dire avec Téléphone** : il coupe les graves et les aigus et ne garde que les médiums. La voix devient nasillarde, comme au téléphone.
- **À dire avec Normaliser** : toute l'onde monte ou descend d'un coup, dans la même proportion. Le spectre garde la même forme, seul le niveau change.
- **Pour comparer** : écoute avec le bouton **Aperçu** de chaque effet avant de valider, et Ctrl+Z pour revenir à la phrase initiale.

### Réflexes pour le TP noté

- Pour chaque manip, note **ce que tu as fait**, puis **ce que tu observes** (image, histogramme, onde ou spectre), puis **la conclusion** en une phrase qui reprend le cours.
- Travaille sur une **copie** de l'image ou du son (Image › Dupliquer dans Photoshop, Ctrl+D dans Tenacity) pour pouvoir comparer.
- Prends des captures d'écran avant/après avec Win+Maj+S.
