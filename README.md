# SEGMENTATION-DES-OBJETS-EN-MOUVEMENT-AVEC-LE-FLOT-OPTIQUE
Ce projet consiste à utiliser le calcul du flow optique dense pour effectuer une segmentation des objets en mouvement dans une vidéo.
# ESTIMATION DU FLOT OPTIQUE DANS UNE SÉQUENCE D’IMAGES
L’estimation du flot optique consiste en l'analyse du déplacement des objets dans une vidéo en estimant la vitesse de déplacement de chacun des objets. Pour y arriver, on se base sur le frame courant et le frame précédent. Deux options sont possibles pour calculer le flot optique: par flot optique dense, ou par flot optique clairsemé (sparse optical flow en anglais). Le flot optique clairsemé donne le flux des vecteurs de certaines caractéristiques
intéressantes (quelques pixels représentant les bords ou les coins d’un objet) dans un frame tandis que le flot optique dense donne le flux des vecteurs de l’ensemble du frame(tous les pixels). Nous avons utiliser le flot optique dense qui est susceptible de plus de détails.
# Calcul du Flow Optique
La détermination du flot optique dense consiste à calculer le vecteur flux optique de tous les pixels de chaque frame. Bien que ce calcul puisse être plus lent, il donne un résultat plus précis adapté à plusieurs applications parmi lesquelles la segmentation d’une vidéo. Dans ce TP nous avons calculé le flot optique dense d’une séquence de frame provenant d’une vidéo prise en entrée. Il existe plusieurs implémentations du flot optique dense, mais
nous avons utilisé la méthode de Faneback disponible sur OpenCV. La méthode cv.calcOpticalFlowFarneback() prend principalement en entrée un frame
courant et son frame précédent. Chaque frame a été au préalable convertie en niveau de gris avant la détermination du flot optique.
# Estimation du flot optique en fonction de la norme
OpenCV permet également de calculer la norme et l’angle de chaque vecteur du flot optique grâce à la méthode cv.cartToPolar() qui prend en entrée les valeurs du flot sur deux canaux de l’espace couleur HSV. L’angle et la norme de chaque vecteur vitesse sont respectivement enregistrés sur les canaux Hue et représentation couleur de l’espace couleur HSV.
# SEGMENTATION DES OBJETS EN MOUVEMENT
La segmentation permet d’isoler les différents objets d’une image. Plusieurs techniques sont répertoriées pour permettre la segmentation. Nous venons de réaliser dans la première partie de notre travail que dans une vidéo, si les objets se déplacent à différentes vitesses, les normes du flot optique sont aussi différentes. Également, si les objets se déplacent dans des directions différentes, les flots optiques correspondants des ces objets ont des orientations différentes. Nous réalisons donc que le flot optique en utilisant ses caractéristiques (norme, angle ou les deux à la fois) peut aider à segmenter les objets en mouvement. 
# Segmentation en fonction de la norme avec seuillage
Le seuillage est une opération qui permet de transformer une image niveau de gris en image binaire (noir et blanc), l'image obtenue est appelée masque binaire. La segmentation avec la technique de seuillage va consister à fixer une ou plusieurs valeurs seuils. Si nous voulons diviser l’image en deux régions (objets et background), on utilise une seule valeur de seuil (seuillage global), mais si nous voulons en plus de deux régions, on utilise plusieurs valeurs de seuil(seuillage local). Dans le cas de la segmentation en fonction de la norme, les valeurs des pixels utilisées pour le seuillage sont des normes des vecteurs du flot optique.
# Objets à plus grande vitesse
Dans ce cas, nous avons utilisé une seule valeur de seuil. Déjà rassurés que les objets qui bougent plus rapidement ont leur norme de vitesse plus élevées, pour les segmenter nous avons utilisé ces normes comme valeur de pixels. cv.threshold prenant en paramètre
La méthode opencv suivante: le frame à segmenter, la valeur de seuil et la valeur maximum à assigner aux pixels ayant une valeur supérieure au seuil et la méthode cv.THRESH_BINARY nous ont permis de réaliser cette tâche. Dans le but d’enlever les bruits et de donner une forme propre à chaque objet en éliminant certains points noirs, nous avons appliqué les transformations morphologiques sur chaque frame binaire obtenue après segmentation. Nous avons particulièrement effectué le opening qui est une érosion suivie d’une dilatation et le closing qui est l’inverse du opening. La méthode de OpenCV qui nous permet de faire ces opérations est cv.morphologyEx () accompagné de cv.MORPH_OPEN pour le opening et cv.MORPH_CLOSE pour le closing, prenant en
paramètre le frame binaire et le kernel défini.
# Objets à différentes vitesses
le seuillage à plus grande vitesse limitait nos résultats aux seuls objets atteignant la vitesse prédéfinie, or dans une scène tous les objets n’ont pas la même vitesse, pour obtenir des résultats objectifs nous appliquons plusieurs seuils simultanément. Cependant nous sommes restés réaliste sur le fait que dans une scène il est extrêmement rare d’avoir un très grand nombre varié de vitesse. Nous avons également appliqué les opérations morphologiques mentionnées ci-dessus.

