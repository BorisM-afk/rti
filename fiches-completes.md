# Fiches RTI3 – Devaux (version complète)

[← Retour au sommaire](README.md) · [Antisèche](antiseche.md)

Résumé des 10 supports du cours : Traitement des images, Compression vidéo, Optimisation images web, Traitement par lot, TD Codage entropique, TP1 à TP4, article de 1994.

## Fiche 1 – Traitement des images

**Définition** : ensemble des techniques pour modifier une image afin de l'améliorer ou d'en extraire de l'information (imagerie médicale, satellite, vidéosurveillance, contrôle qualité, robotique…).

### Histogramme

- Graphique du **nombre de pixels par niveau d'intensité**. Abscisse : du plus foncé (gauche) au plus clair (droite).
- Infos affichées : Moyenne = luminosité moyenne ; Écart-type = étendue des valeurs ; Médiane ; Pixels = nombre de pixels comptés.
- Image RVB : 1 histogramme de luminance + 3 histogrammes (R, V, B).
- Image foncée → détails dans les tons foncés ; claire → tons clairs ; tons moyens → tons moyens.
- Modifier l'histogramme **n'ajoute ni ne supprime d'information**, ça la rend plus ou moins visible.

### Courbe tonale (LUT)

- Abscisse = valeur d'entrée, ordonnée = valeur de sortie. Fonction de transfert stockée dans une **LUT** (Look Up Table).
- **Diagonale** = aucune modification.
- Courbe **au-dessus** de la diagonale → image **éclaircie** ; **en dessous** → **assombrie**.
- Alternative : jouer directement sur les **Niveaux** (curseurs d'entrée noir/blanc et niveaux de sortie).

### Les opérations à connaître

| Opération | Principe | Effet / courbe |
| --- | --- | --- |
| Égalisation | Répartir les pixels pour avoir ~le même nombre par niveau | Plus de nuances |
| Étirement (expansion de dynamique) | Ramener la valeur mini à 0 et la maxi à 255 | Plus de contraste ; courbe = droite plus pentue |
| Seuillage simple | < seuil → 0 (noir), > seuil → max (blanc) | Image binaire (binarisation), fait ressortir les formes ; courbe en marche |
| Seuillage à 2 bornes | Entre seuil bas et haut → blanc, le reste → noir | Isoler une plage |
| Inversion | Valeurs inversées autour de la moyenne (255 − v) | Négatif ; histogramme retourné en miroir ; courbe = diagonale descendante |

En TP : étirement = rapprocher les curseurs d'entrée noir et blanc des bords de l'histogramme (≈ « Contraste automatique »).

### Filtrage (convolution)

- Filtre = matrice (noyau) dont le centre = pixel traité. Nouvelle valeur = **somme des voisins × coefficients** (produit de convolution), souvent divisée par la somme des coefficients.
- Photoshop : Filtre › Divers › Autre. GIMP : Filtres › Génériques › Matrice de convolution.
- **Passe-bas** : atténue les hautes fréquences → **lissage / débruitage**, image **plus floue**.
  - Moyenneur : 1/9 × [1 1 1 / 1 1 1 / 1 1 1]
  - Binomial (gaussien) : 1/16 × [1 2 1 / 2 4 2 / 1 2 1]
- **Passe-haut** : atténue les basses fréquences → **accentuation**, **détection de contours**.
  - Sobel horizontal : 1/4 × [1 0 −1 / 2 0 −2 / 1 0 −1]
  - Roberts : [0 −1 / 1 0]
  - Prewitt : 1/3 × [1 0 −1 / 1 0 −1 / 1 0 −1]
  - Laplace : [0 1 0 / 1 −4 1 / 0 1 0] (contours dans toutes les directions)
- **Bruit** = pixels dont l'intensité diffère fortement des voisins (capteur, environnement, échantillonnage).
- Astuce : un noyau dont la somme des coefficients = 0 donne un fond noir et ne garde que les contours.

## Fiche 2 – Optimisation des images web et traitement par lot

### Les formats

| Format | Compression | Usage | Transparence |
| --- | --- | --- | --- |
| GIF | Sans perte si ≤ 256 couleurs | Icônes, logos, aplats, petites animations | 1 niveau |
| JPEG | **Avec perte**, taux réglable | **Photos** | Aucune |
| PNG-8 / PNG-24 | **Sans perte** (1 à 48 bits) | Graphismes, images HD avec transparence | 1 niveau en 256 coul., multiple sinon |
| WebP | Avec ou sans perte | Remplace JPEG et PNG, souvent plus léger | Oui |

### Méthode en 3 étapes

1. **Dimensionner** : pour le web **seul le nombre de pixels compte** (la résolution en ppp ne sert à rien). Redimensionner dans le logiciel, pas avec width/height en HTML ou dans le CMS.
2. **Choisir le format** : aplats ou peu de couleurs → GIF/PNG ; photo → JPEG (ou WebP) ; PNG-24 = parfait mais lourd.
3. **Compresser** : Photoshop « Exporter / Enregistrer pour le web » : format, taille, qualité, **suppression des métadonnées**. Comparer qualité visuelle et poids.

Outils : TinyPNG, Kraken, JPEGmini, ImageOptim, pngquant… ⚠️ Les services en ligne peuvent réutiliser tes images (entraînement d'IA).

### Résolution, définition, taille, poids (TP1)

- Définition = pixels (L × H). Résolution = ppp. Taille physique = définition / résolution.
- **Taille constante, résolution ↑** → définition ↑ → poids ↑.
- **Définition constante, résolution ↑** → taille physique ↓, **poids inchangé**.
- Conclusion : le poids dépend de la définition (et du codage), pas de la résolution.
- Image réduite dans Word avec la souris = toujours aussi lourde dans le .docx. Réduire d'abord dans le logiciel d'image = .docx bien plus léger.

### Traitement par lot (Photoshop)

Fichier › Scripts › **Processeur d'images** :

1. Dossier source
2. Dossier de sortie
3. Format (JPEG) + qualité + redimensionnement (TP1 : 300 × 200, qualité 8)
4. Préférences : cocher **Inclure le profil ICC**
5. **Exécuter**

Alternatives : ImageMagick, IrfanView, FastStone, XnView/ImBatch.

## Fiche 3 – Compression vidéo

**Principe** : supprimer la **redondance** (spatiale, temporelle, subjective, statistique).

### Réduire le débit avant compression

- On garde la partie utile de l'image en 8 bits : 4:2:2 SD passe de 270 à **166 Mbit/s** (−38,5 %).
- 4:2:0 / 4:1:1 = **124 Mbit/s** (DVD, DV…).
- **SIF** = 4:2:0 progressif, définition ÷ 2 en largeur et en hauteur → **360 × 288, 25 i/s, 31 Mbit/s** = format d'entrée du MPEG-1.

### Compression spatiale ou temporelle

- **Spatiale (intra)** : chaque image compressée comme un JPEG → **M-JPEG** (format DV). ✅ Montage à l'image près. ❌ Taux faible (2:1 à 5:1). M-JPEG 2000 = cinéma numérique (10:1).
- **Temporelle unidirectionnelle** : on code la différence entre n et n−1 (**image delta**) → Cinepak, Indeo. Il faut insérer régulièrement des images I, d'autant plus que la séquence est animée.
- **Temporelle bidirectionnelle** (n−1, n, n+1) + **estimation/compensation de mouvement** = base du MPEG :
  1. Chercher les blocs semblables
  2. Calculer les vecteurs de mouvement
  3. Construire l'image prédite
  4. Image résiduelle = réelle − prédite
  5. Coder les vecteurs + le résidu

### GOP (Group Of Pictures)

| Image | Codage | Taux moyen |
| --- | --- | --- |
| **I** (Intra) | Comme un JPEG, sans référence ; point d'entrée du GOP | 7:1 |
| **P** (Prédite) | À partir de la I ou P précédente | 20:1 |
| **B** (Bidirectionnelle) | Interpolée entre les I/P passées et futures | 50:1 |

- **M** = distance entre deux P, **N** = distance entre deux I. Valeurs classiques : **M = 3, N = 12** → IBBPBBPBBPBB.
- Répartition : 1/12 de I, 1/4 de P, 2/3 de B → **27:1 en moyenne** (les B, les plus nombreuses, sont les plus compressées).
- Macrobloc = 6 blocs 8×8 (4 Y + 1 Cb + 1 Cr).
- Chaîne MPEG-1 : 124 Mbit/s → SIF 31 Mbit/s (4:1) → ×27 → **1,15 Mbit/s** (total **108:1**), compatible CD.

### Les normes

| Norme | Année | Points clés |
| --- | --- | --- |
| **MPEG-1** | 1992 | Stockage CD, ~1,5 Mbit/s (1,15 vidéo + 350 k audio), SIF, 4:2:0, qualité ≈ VHS |
| **MPEG-2** | 1994 | SD → HD, entrelacé et progressif, 4:2:2 possible. **Profils** (outils : Simple = I+P, Main = I+P+B, 4:2:2) × **Niveaux** (définition : Low = SIF, Main = SD, High 1440, High = 1920×1080). DVD = MP@ML, GOP de 15 |
| **MPEG-4 Part 2** | 2000 | ASP = **DivX**, seulement +20 % vs MPEG-2, vite dépassé |
| **MPEG-4 Part 10 = AVC / H.264** | 2003 | **−50 % de débit vs MPEG-2**, calcul ×2 à ×4. Prédiction intra 9 directions, blocs 4×4, macroblocs variables, ¼ de pixel, 32 images de référence, slices, **CABAC** (ou CAVLC), filtre anti-blocs. TNT française depuis le 6/04/2016 |
| **HEVC / H.265** | 2013 | **−50 % vs H.264, −75 % vs MPEG-2**, 35 directions, codeur ×5 à ×10 plus lourd. Profil Main 10 (10 bits grand public). Ultra HD 15 à 25 Mbit/s, BD UHD, TNT allemande (2017) |

MPEG = Moving Picture Experts Group (1988). ISO + IEC (et ITU pour les H.26x).

### Codec et conteneur, à ne pas confondre

- **Codec** = algorithme de compression (MPEG-2, H.264, H.265…).
- **Conteneur** = fichier qui réunit la vidéo, l'audio et les métadonnées (codec, fps, débit, sous-titres) : **AVI** (Microsoft), **MOV** (Apple QuickTime), MP4, MKV, FLV, OGG.

### Usages

DVD = MPEG-2 · Blu-ray = MPEG-2, MPEG-4 ou VC1 · BD UHD = HEVC · Cinéma = M-JPEG 2000 · Streaming = MPEG-4 ou HEVC.

### Export web recommandé (et sobriété)

**MP4 + H.264, 720p (1280×720 progressif), 24 i/s, VBR 2 passes, 5 ou 2,5 Mbit/s. Son : AAC 320 kbit/s, 48 kHz, stéréo.**

La vidéo représente 60 à 90 % du trafic internet. Se demander à quoi elle sert et si un autre média suffit, faire court (< 1 min 30 en marketing), prévoir plusieurs formats, optimiser en post-production, privilégier YouTube ou Vimeo.

## Fiche 4 – Codage entropique (TD)

### Formules

- Quantité d'information : **Q = log₂(1/p)** bits, avec log₂ x = log₁₀ x / log₁₀ 2 (≈ log x / 0,301).
- Entropie : **H = −Σ pᵢ log₂ pᵢ** = nombre minimum de bits moyen par symbole.
- Longueur moyenne : **L = Σ pᵢ × lᵢ**. Efficacité = H / L.

### Les 3 algorithmes (toujours trier par probabilité décroissante d'abord)

- **Direct** : nombre de bits = Q arrondi au supérieur, puis on choisit des codes dont aucun n'est le début d'un autre (code préfixe).
- **Shannon-Fano** (descendant) : couper en 2 groupes de probabilités les plus proches possibles, 0 pour le plus probable, 1 pour l'autre, et recommencer.
- **Huffman** (ascendant) : fusionner les 2 plus faibles (0 au plus faible, 1 à l'autre), retrier, recommencer, puis lire le code de la racine à la feuille. C'est un code **optimal**.

### Corrigé : alphabet MMI

| Lettre | p | Q (bits) | Direct | Fano | Huffman |
| --- | --- | --- | --- | --- | --- |
| A | 0,42 | 1,25 | 2 bits | 0 | 0 |
| C | 0,18 | 2,47 | 3 | 100 | 111 |
| G | 0,13 | 2,94 | 3 | 101 | 101 |
| E | 0,09 | 3,47 | 4 | 1100 | 1101 |
| H | 0,07 | 3,84 | 4 | 1101 | 1100 |
| B | 0,05 | 4,32 | 5 | 1110 | 1000 |
| F | 0,04 | 4,64 | 5 | 11110 | 10011 |
| D | 0,02 | 5,64 | 6 | 11111 | 10010 |

- **H ≈ 2,45 bits**
- Cas équiprobable : 8 symboles → **3 bits**, L = 3
- Direct : **L ≈ 2,98** · Fano : **L = 2,49** · Huffman : **L = 2,49** (98 % d'efficacité)
- Conclusion : Fano et Huffman approchent l'entropie et battent le code fixe (−17 %). Huffman est toujours optimal ; ici Fano fait aussi bien. Les codes exacts de Huffman peuvent varier selon l'arbre, mais pas les longueurs.

## Fiche 5 – Audio : amplification et filtres (TP3, Tenacity)

### Gain

- **Gv = Vs / Ve** (sans unité). **G(dB) = 20 log₁₀(Vs/Ve)**.
- **+6 dB = amplitude × 2** · **+20 dB = × 10** · −3 dB = × 0,707 · −6 dB = ÷ 2 · −20 dB = ÷ 10.
- Les dB s'ajoutent quand les gains se multiplient.

| G | 2 | 4 | 8 | 16 | 32 |
| --- | --- | --- | --- | --- | --- |
| dB | 6 | 12 | 18 | 24 | 30 |

| G | 1 | 10 | 100 | 1000 |
| --- | --- | --- | --- | --- |
| dB | 0 | 20 | 40 | 60 |

Résultats attendus : S1 = 0,25 V +6 dB → **0,5 V**, +12 dB → **1 V**. S2 = 0,05 V +20 dB → **0,5 V**.

### Filtres

- **Bande passante** : plage où G ≥ Gmax − 3 dB. **Fréquence de coupure fc** : là où G = Gmax − 3 dB.
- Pente 6 dB/octave = 20 dB/décade.
- Tenacity par défaut : fc = 1000 Hz, 6 dB/oct. Avec une entrée de 1 V :

| Fréquence | Passe-bas | Passe-haut |
| --- | --- | --- |
| 100 Hz | ≈ 1 V (passe) | ≈ 0,1 V (−20 dB) |
| 1000 Hz | ≈ 0,707 V (−3 dB) | ≈ 0,707 V (−3 dB) |
| 10 000 Hz | ≈ 0,1 V (−20 dB) | ≈ 1 V (passe) |

Passe-bas : laisse passer les graves, coupe les aigus. Passe-haut : l'inverse.

## Fiche 6 – Audio : les effets (TP4, Tenacity)

**S(t) = A·sin(2πft + φ)** : chaque paramètre correspond à une famille d'effets.

| Paramètre | Famille | Effets |
| --- | --- | --- |
| **A** amplitude | Dynamique | Normaliser, amplifier, compresseur, noise gate, réduction de bruit |
| **f** fréquence | Égalisation | EQ, filtres, préréglage « Téléphone » |
| **φ** phase | Modulation | Phaser, flanger, chorus, trémolo |
| **t** temps | Spatialisation | Écho, delay, réverbération |

- Réglages : Édition › Préférences › Qualité → **44 100 Hz, 16 bits**. Export **WAV signé 16 bits PCM**.
- Spectre : Analyse › Spectre (Hamming, 512, linéaire).
- **Égalisation** = modifier la réponse en fréquence (filtre). Préréglage Téléphone = coupe les graves et les aigus, ne garde qu'une bande d'environ 300 à 3400 Hz : son nasillard.
- **Gamme dynamique max = 20 log₁₀(2ⁿ − 1)** → **16 bits ≈ 96 dB**, **8 bits ≈ 48 dB** (≈ 6 dB par bit).
- **Normaliser** : même correction d'amplitude sur tout le signal pour atteindre un pic cible. Exploite tous les bits disponibles et harmonise les niveaux.
- **Noise gate** : tout ce qui est sous un seuil devient silence.
- **Réduction de bruit en 2 étapes** : 1) sélectionner un passage de bruit seul et « Prendre le profil du bruit », 2) tout sélectionner et appliquer. Alternative : « Réduction et isolation vocale ».
- **Écho** : répétition atténuée. **Delay** : écho calé sur le tempo. **Réverbération** : nombreuses copies retardées et atténuées, pour simuler une pièce (utiliser les préréglages).
- Rappel du prof : une bonne prise de son vaut mieux que tous les effets, et on ne peut souvent pas la refaire.

## Fiche 7 – Article « La compression favorise l'espace » (1994)

- 1994 voit exploser les techniques de compression. Ancêtre : la **modulation delta** de Philips (après 1945), qui ne transmet que les différences entre points.
- **JPEG** : blocs 8×8, gain de 10 à 20 sans différence visible. **MPEG** : images transmises, prédites et interpolées, taux proche de 100. Les fractales laissaient espérer plus de 1000.
- Effet sur le satellite : on passe d'environ 20 à 140 ou 150 programmes par satellite (Direct TV aux États-Unis, Astra en 1995). Le coût de l'information baisse et de nouveaux marchés s'ouvrent.

## Questions probables du TP noté

- Lire un histogramme (image sous- ou surexposée) et le corriger avec les Niveaux ou les Courbes.
- Retrouver le seuil d'une image seuillée (TP2 : comparer avec Seuil dans Photoshop).
- Appliquer un filtre de convolution et dire s'il est passe-bas ou passe-haut.
- Optimiser des images selon un zoning, en justifiant le format, la définition et le poids.
- Lancer le processeur d'images (JPEG 300×200, qualité 8, profil ICC).
- Calculer des gains en dB et mesurer une amplitude après filtrage dans Tenacity.
- Associer chaque effet audio à sa famille et calculer une gamme dynamique.
- Expliquer I/P/B, le GOP, codec et conteneur, et classer les normes vidéo.
