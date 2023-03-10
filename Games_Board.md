<center><h1> Board Games </h1></center>

## I. Introduction

Notre étude s'appuie sur un dataset de jeux de société provenant du site Kaggle (https://www.kaggle.com/datasets/andrewmvd/board-games). Cet ensemble de données a été extrait à partir du site Web de BoardGameGeek (BGG) et comprend des données sur 16 598 jeux de société. Ces données ont été collectées en février 2021. Le site BoardGameGeek est un site qui recense de nombreux jeux de sociétés et permet à ses utilisateurs de partager leur expérience avec ces jeux au travers de commentaires et de notes. BGG référence plus de 100 000 jeux de société différents. Ainsi, cela fait de lui, la plus grande collection en ligne de données de jeux de société. La base de données contient uniquement des jeux classés, c'est-à-dire des jeux qui ont obtenu au moins 30 votes. Pour chacun, nous retrouvons les caractéristiques suivantes :

- **ID**: Identifiant BoardGamesGeek
- **Name** : Nom du jeu de société
- **Year Published** : Année de publication du jeu 
- **Min Players** : Nombre minimum de joueurs suggérés
- **Max Players** : Nombre maximum de joueurs suggérés
- **Play Time** : Temps de jeu moyen suggéré par les créateurs de jeux
- **Min Age** : Age minimum suggéré pour jouer au jeu
- **Users Rated** : Nombre d'utilisateurs qui ont évalué le jeu
- **Rating Average** : Note moyenne du jeux sur BGG (noté sur 10)
- **BGG Rank** : Classement BoardGamesGeek
- **Complexity Average** : Note de complexité (noté sur 5)
- **Owned Users** : Nombre d'utilisateurs qui ont déclaré posséder le jeu
- **Mechanics** : Mécanique de jeu
- **Domains** : Sous-genre de jeu

L’objectif de notre étude est de prédire la note moyenne du jeu de société (*Rating Average*) en fonction de ses différentes caractéristiques à l’aide de divers algorithmes.

Voici un aperçu de notre base de données :

*Tableau N°1 :  les 3 premières lignes de notre base de données*

|ID|	Name	|Year Published	|Min Players|	Max Players	|Play Time|	Min Age|	Users Rated	|Rating Average	|BGG Rank|Complexity Average|	Owned Users|	Mechanics |Domains|
|-:|-------:|--------------:|----------:|------------:|--------:|-------:|-------------:|--------------:|-------:|-----------------:|-----------:|-----------:|------:|
|174430.0|Gloomhaven|2017.0|1|4	|120|	14|	42055|	8.79|	1|	3.86|	68323.0	|Action Queue, Action Retrieval, Campaign / Bat...	|Strategy Games, Thematic Games|
|161936.0|	Pandemic Legacy: Season 1|	2015.0|	2|	4|	60|	13|	41643|	8.61|2	|2.84	|65294.0|	Action Points, Cooperative Game, Hand Manageme...|	Strategy Games, Thematic Games|
|224517.0|	Brass: Birmingham|	2018.0|	2|	4|	120|	14	|19217|	8.66|	3	|3.91|	28785.0|	Hand Management, Income, Loans, Market, Networ...|	Strategy Games|

On ne conserve pas la variable *ID*, cette variable n'est pas pertinente pour la prédiction de la variable *Rating Average*. En effet, l'identifiant n'a aucun impact sur la note d'un jeu de société. 

## II. Analyse Exploratoire

### A) Analyse des valeurs manquantes

Nous avons tout d'abord regardé les valeurs manquantes pour chacune des variables. Nous constatons qu'il manque des valeurs pour 4 des variables : *Yr_Published, Owned_users Mechanics* ainsi que la variable *Domains*. 

Il manque une seule valeur pour la variable d'année de publication. C'est pour le jeu 'Hus' que cette information n'est pas renseignée. On remarque également qu'il manque d'autres informations, dont le nombre d'utilisateurs qui possèdent le jeu, la mécanique et le domaine. Il ne semble donc pas pertinent de concerver ce jeu, on le supprime de notre base de données.

On constate que pour les observations pour lesquelles il manque le nombre d'utilisateurs qui possède le jeu, il manque également le domaine et pour la plupart la mécanique de jeu. On supprime donc les jeux pour lesquels cette variable n'est pas renseignée (22 valeurs).

En ce qui concerne la variable *Domains*, il manque plus de 10 000 observations soit quasiment la moitié des données. Nous faisons tout de même le choix de conserver cette dernière, puisque nous avons un assez grand nombre de données et que cette variable pourrait être pertinente afin de prédire la note moyenne du jeu. En effet, il pourrait par exemple être intéressant pour un éditeur de jeu de société de connaître les genres de jeux les plus populaires. Nous supprimons ainsi les jeux pour lesquels le domaine n'est pas renseigné.  

Nous faisons de même pour les 475 valeurs manquantes de la variable *Mechanics*. Il aurait été intéressant d'imputer ses valeurs manquantes par le mode. Néanmoins, cela ne serait pas forcément pertinent puisqu'un jeu peut avoir plusieurs mécaniques différentes. 

Finalement, nous obtenons une base constituée de 9 703 jeux de sociétés.

### B) Analyse des valeurs atypiques

Nous nous intéressons ensuite à la présence de valeurs potentiellement atypiques pour les variables quantitatives. Pour ce faire, nous avons tracé les boxplot de ces variables : 

*Figure N°1 : Boxplot des variables quantitatives*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/fig_2_intuition_svm.jpeg" alt="fig_2_intuition_svm" style="width:1400px;"/>

- **Year_Published**

Les dates de publication des jeux de société sont entre -3500 et 2021. Lorsque l'on observe les jeux pour lesquels la date est négative, on constate qu'il s'agit majoritairement de jeux traditionnels, par exemple, on y retrouve le jeu de go. Il ne s'agit donc pas nécessairement de valeur aberrante. On fait le choix de ne conserver que les jeux contemporains. Pour ce faire, on supprime les jeux créés avant le 19ème siècle (1800).

Afin de facilité l'interprétation des variables, on fait le choix de créer une nouvelle variable qui représente l'age du jeu que l'on nomme *Age* à partir de l'année de pblication. Les données ayant été collectées en février 2021, on calcul l'âge du jeu à la date où les données ont été collectées. L'âge moyen des jeux de société est de 18 ans. Le Perudo ainsi que le Casino sont les jeux les plus anciens de la base de données.

- **Max_players et Min_players**

On constate qu'il existe des jeux dans la base de données pour lesquels le nombre de joueurs minimum et le nombre de joueurs maximum sont nuls. On supprime ces jeux car il s'agit de valeurs aberrantes. En effet, pour jouer à un jeu de société, il faut au minimum être 1 joueur.

Il y a une observation pour laquelle le nombre de joueurs minimum est de 10. Il s'agit d'un jeu (haggle) qui demande effectivement d'être nombreux pour pouvoir y jouer (cf bgg). Même si cette valeur peut être considérée comme atypique, on fait le choix de la garder, car cela peut avoir un impact important sur la note du jeu. Il faut en moyenne au minimum 2 joueurs pour jouer aux jeux de la base de données.

On constate qu'il y a deux jeux (Pit Fighter: Fantasy Arena	 et Black Powder: Second Edition) pour lesquels le maximum de joueurs est de plus de 100. Ces sont tous les deux des jeux de Guerres. Lorsqu'un jeu n'a pas de maximum de joueurs indiqué, il est souvent considéré que le maximum est de 99 ou 100. On décide donc de corriger ces valeurs en leur attribuant la valeur de 100.

- **Play_Time**

Le temps de jeu minimum est nul. Les jeux avec un temps de jeu nul sont probablement des erreurs de saisies ou des informations manquantes. On supprime ainsi les jeux pour lesquels le temps de jeu est nul. Le temps de jeu correspond au temps moyen suggéré par les créateurs des jeux. Le temps de jeu peut être défini comme un intervalle entre le temps minimimum et le temps maximum de jeu. Lorsque le temps de jeu est indiqué sous forme d'intervalle, la valeur que l'on retrouve dans la base de données est la borne maximale. 

La variable *Play_Time* présente beaucoup d'observations qui semblent atypiques. Le temps de jeu maximal est de 60 000 minutes, soit 1 000 heures. C'est bien supérieur au temps des 75 % des jeux les moins longs qui est de 120 minutes. De plus, la moyenne est plus de 2 fois supérieures à la médiane.

Lorsqu'il y a un temps de jeu maximum, c'est celui-ci qui est indiqué dans la base de données. C'est pourquoi certains jeux ont des temps de jeu particulièrement élevé. On remarque que les jeux avec les temps de jeu les plus élevés sont majoritairement des jeux de la catégorie wargames. Lorsque l'on s'intéresse à la description de certains de ces jeux sur le site BoardGameGeek, les jeux de guerres sont généralement des jeux de plateaux qui peuvent être parfois très long. L'ensemble des temps de jeux très longs ne sont donc pas des valeurs aberrantes. On peut toutefois soupçonner des valeurs comme 60 000 d'être aberrantes. On fait donc le choix de supprimer uniquement les jeux qui durent plus de 24h soit 1440 minutes.

- **Min_age**

On constate que l'âge minimum n'est pas renseigné pour un certains nombre de jeux. On fait le choix d'imputer ces valeurs manquantes par la médiane. On leur attribut un âge en fonction de leur domaine. La majorité des jeux dont l'âge minimum est manquant possèdent un seul domaine. Par conséquent, on leur attribut l'âge médian du domaine auquel ils appartiennent. Pour les jeux pour lesquels il reste des âges minimum à 0, ils combinent plusieurs domaines de jeu. On leur attribue donc la médiane de l'ensemble des jeux du dataset.

- **Users_rated**

La variable *Users_rated* correspond au nombre d'utilisateurs qui ont évalué le jeu. On observe une grande différence entre la moyenne et la médiane, cependant, on décide dans un premier temps de conserver l'ensemble de ces observations. En effet, il y a quelques variables qui ont beaucoup plus de votes que la médiane, mais il s'agit de jeux très célèbre dont on ne souhaite pas se séparer. Il nous semble intéressant de voir si la renommée d'un jeu permet d'avoir une note plus importante par exemple.

- **Rating_avg**

Les jeux sont notés de 0 à 10. On remarque que la moyenne et la médiane sont relativement proches. On décide de conserver l'ensemble des observations de cette variable. Il nous semble pertinent de chercher à comprendre les valeurs tant faibles qu'élevées de cette variable. C'est également la variable que l'on souhaite prédire.

- **Complexity_avg**

Les jeux se voient attribuer un niveau de complexité par les utilisateurs du site BGG. La complexité du jeu dépend de plusieurs facteurs notamment le nombre de règles, la durée de jeux, la part de chance ou encore les connaissances prérequises pour jouer. Les jeux sont notés sur une échelle de 0 à 5, 5 correspondant aux jeux particulièrement complexes. La variable complexity average est une moyenne du score de complexité attribué par les utilisateurs de BGG à un jeu. Il y a 16 jeux pour lesquels le niveau de complexité est de 0. Il s'agit de jeux pour lesquels les utilisateurs de BGG n'ont pas noté la complexité donc on supprime ces jeux. La complexité moyenne des jeux de notre dataset est de 2.31.

- **Owned_users**

On fait le choix de garder l'ensemble des observations. 

Suite à ces premières étapes de nettoyage de la base de données, celle-ci se compose de 9 302 observations.

### C) Traitement des variables qualitatives

Notre dataset contient 2 variables qualitatives : *Mechanics* et *Domains*.

- **Mechanics**

La variable *Mechanics* indique les différentes mécaniques de jeu qui peuvent être utilisées lors d'une partie. Par exemple, la mécanique indique s'il s'agit d'un jeu de cartes ou d'un jeu de dé. Certains jeux ont plus d'une mécanique qui intervient. Il y a 182 mécaniques de jeu différentes. Le nombre maximum de mécaniques observées pour un même jeu est de 19.

Dans un premier temps on crée une variables correspondant au nombre de mécaniques par jeu. On peut supposer que c'est une variable qui pourrait avoir un impact sur la popularité du jeu, soit parce qu'un jeu avec trop de mécaniques serait difficile d'accès pour un large public soit parce qu'il permet d'avoir différentes dynamiques au cours d'une partie qui rendent attrayant ce jeu.

Dans un second temps on crée des variables binaires pour les mécaniques les plus communes pour savoir si le jeu appartient ou non à une mécanique. Il ne paraît pas pertinent de construire une variable par mécanique, cela représenterait trop de variables et certaines mécaniques ne sont pas suffisamment représentées. Nous gardons ainsi les mécaniques de jeux dont la fréquence d'apparition est supérieure à 10 % du nombre de jeux total dans notre dataset. Les mécaniques de jeux les plus communes sont les suivantes : *Dice Rolling*, *Hexagon Grid*, *Hand Management*, *Simulation*, *Variable Player Powers*, *Set Collection* ainsi que *Grid Movement*. Nous créons ainsi une variable binaire pour chacune de ses mécaniques, soit un total de 7 nouvelles variables. 

Nous avons également cherché si l’on pouvait procéder à certains regroupements de mécaniques. Par exemple, les jeux d'enchères sont séparés dans différentes catégories donc nous regardons combien de jeux sont dans ces catégories afin de voir s'il peut être pertinent de les regrouper pour en faire une catégorie unique.

- Nous avons regroupé les mécaniques pour lesquelles le mot “map” apparaît, soit 74 jeux, cela représente 0.796 % des jeux de la base de données. Ainsi il y a trop peu de jeux utilisant des cartes (map) pour qu'il soit pertinent de créer une variable pour ce type de jeu.

- Au total, 3 437 jeux utilisent un dé, soit 36.95 % des jeux de la base de données. Comme nous avons déjà créé une variable “Dice Rolling” auparavant nous ajoutons à cette dernière les autres mécaniques contenant le mot “Dice”. On renomme cette variable *Dice*.

- Les jeux possédant une mécanique contenant le mot “Action” représentent 14.61 % des jeux de la base de données. Ainsi, nous créons une variable binaire intitulée *Action*

- Il y a 1193 jeux de cartes. Cela représente 12.82 % des jeux de la base de données. On crée une variable binaire *Map*. Cette variable prend la valeur de 1 si le jeu à une mécanique contenant le mot “Card”.

Finalement pour la variable *Mechanics* Nous avons créé 9 variables binaires ainsi qu'une variable quantitative représentant le nombre de mécaniques par jeu. 

- **Domains**

La variable *Domains* indique les genres des jeux de société. Tout comme pour la variable *mécanique* on créer une variable quantitative représentant le nombre de domaines par jeu. Un jeu peut avoir plusieurs domaines différents (au maximum 3). Plus de 75 % des jeux de sociétés, ne possèdent qu’un seul domaine. Il y a  8 domaines différents parmi les jeux de la base de données : *Abstract Games*, *Children's Games*, *Customizable Games*, *Family Games*, *Party Games*, *Strategy Games*, *Thematic Games* et *Wargames*. Parmi les jeux de notre dataset on retrouve une majorité de jeux de guerres suivis de jeux de stratégie et de jeux familiaux. Nous créons ainsi une variable binaire pour chacun de ses 8 domaines.

### D) Corrélation entre les variables quantitatives

Afin de voir les liens entre les différentes variables explicatives, nous avons représenté une matrice des corrélations de spearman :

*Figure N°2 : Matrice de corrélation*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/correlation.jpeg" alt="correlation.jpeg" style="width:1400px;"/>

On note 4 couples de variables fortement corrélées : 

- La complexité moyenne (*Complexity_avg*) et le temps de jeu (*Play_Time*) sont corrélés positivement. On peut en effet supposer qu'un jeu de société complexe demandera davantage de réflexion et par conséquent le temps de jeu sera plus important. 

- La variable *Bgg_rank* et *Users_rated* sont corrélés négativement. Ainsi, plus le nombre de joueurs ayant déclaré posséder le jeu est important plus la position de celui-ci dans le classement BoardGamesGeek semble élevé. 
 
- La variable *Bgg_rank* et *Owned_users* sont également corrélés négativement. Les jeux se trouvant en haut du classement BoardGamesGeek sont les jeux de société les plus possédés par les utilisateurs du site.  

- Le nombre de joueurs qui déclarent posséder le jeu (*Owned_users*) et le nombre de joueurs qui ont attribué une note au jeu (Users_rated) sont très fortement corrélés (0,96). On fait donc le choix de ne conserver que le nombre de joueurs qui possèdent le jeu. On écarte le nombre de votants, car c'est la variable la moins pertinente des deux à conserver puisqu'on peut supposer que le nombre de votants intervient dans le calcul de la variable que nous cherchons à prédire qui est la note moyenne.

### E) Statistiques descriptives de *Rating_avg* 

*Tableau N°2 :  Statistiques descriptives de la variable Rating_avg*

|Stats|valeurs|
|-:|-------:|
|count|    9302.000000|
|mean|       6.626050|
|std  |       0.835887|
|min|    2.830000|
|25%    |     6.110000|
|50%    |     6.650000|
|75%    |     7.200000|
|max     |    9.340000| 

En moyenne, les jeux de société de notre dataset ont une note moyenne de 6,62/10. Le jeu de guerre *Company of Heroes* à obtenu la note moyenne maximale. À l’inverse, *Quidditch : The Game* (jeu pour enfant) possède la note moyenne minimum. De plus, on constate que la moyenne et la médiane sont très proches. 

### F) Corrélation des variables quantitatives avec la variable *Rating_avg*

*Figure N°3 : Matrice de corrélation*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/correlation_y.jpeg" alt="correlation_y" style="width:400px;"/>

La variable *BGG_rank*, est la variable la plus corrélée avec la note moyenne d’un jeu. Plus, le jeu est positionné en haut du classement, plus la note moyenne est élevée. La complexité ainsi que l’âge minimal recommandé pour y jouer ont un impact positif relativement important sur la note moyenne du jeu. En revanche, les variables concernant le nombre de joueurs maximum ou minimum sont peu corrélées avec la note moyenne d'un jeu de société.

On s'intéresse également à la distribution des notes selon les modalités des diverses variables qualitatives. On constate qu'il semble y avoir peut de variations liées aux mécaniques et aux domaines. On peut toutefois noter qu'il semble y avoir une différence de moyenne entre les jeux pour enfant et ceux qui ne le sont pas. Les jeux pour enfant (appartenant au domaine "Children") obtiennent une note plus faible en moyenne sur notre échantillon.

*Figure N°4 : Distribution de la note moyenne en fonction de la variable Children*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/Children.jpeg" alt="Children" style="width:400px;"/>

## III. Régression

L'objectif de notre modélisation est de prédire la note moyenne des jeux de société. Pour ce faire, on estime deux types de modèle, dans un premier temps, on applique différents SVR puis on applique un réseau de neurones.  

Préalablement, à la modélisation, on procède à un rééchantillonnage. On divise le jeu de données en deux échantillons : 80 % des données sont aléatoirement attribuées au jeu d’entraînement et les 20 % restantes forment le jeu test. Les features quantitatives sont standardisées.

### A/Support Vector Regression

Le premier modèle que nous appliquons est un *SVR* (Support Vector Regression). Les SVR sont des modèles qui appartiennent à la famille de l’apprentissage supervisé. Les SVR permettent de modéliser des relations linéaires, mais aussi non-linéaires. Le SVM pour la régression consiste, non pas à maximiser les points en dehors des marges comme pour un problème de classification, mais à faire en sorte que le maximum de points se trouve entre les marges. Le paramètre epsilon permet de faire varier la largeur des marges autour de l’hyperplan. Le paramètre C est un paramètre de régularisation qui indique l’importance attribuée à l’erreur par l’algorithme. Il est également possible de choisir le type de kernel à appliquer pour que les données soient linéairement séparables.

*Figure N°5 : Evolution de l'erreur en cross validation*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/folds.png" alt="folds" style="width:600px;"/>

Dans un premier temps, nous entraînons un LinearSVR puis 3 SVR avec un kernel linéaire, polynomial et rbf. Les 4 modèles sont estimés avec pour paramètres C=100 et epsilon=0.5. Afin de comparer la performance des modèles sur le jeu d’entraînement, on réalise une cross validation en 5 folds. Le score que nous utilisons est le mean square error (MSE). Le SVR avec kernel rbf est celui qui permet d’obtenir les erreurs les plus faibles en moyenne sur l’ensemble des folds. On réalise une prévision à partir de ces 4 modèles que l’on compare au jeu test, ce qui permet de constater que le modèle avec kernel rbf est celui qui apporte le mse le plus faible. 

On cherche à optimiser les hyperparamètres du SVR avec kernel rbf à l’aide d’un GridSearch. Le score mesuré par le GridSearch est le R2 du modèle. On obtient le modèle le plus performant avec les hyperparamètres suivants : C=10 et epsilon=0.1. 

*Figure N°6 : Evolution du score selon les paramètres*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/params.png" alt="params" style="width:600px;"/>

On trace les courbes d’apprentissage associées au modèle. Cela permet de constater que plus on a de données mieux le modèle apprend. EN effet, on constate qu’à mesure que l’on rajoute des données, l’erreur du jeu de validation diminue et se rapproche de celle du jeu d’apprentissage.

*Figure N°7 : Courbes d'apprentissage*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/learning%20curve.png" alt="learning%20curve" style="width:600px;"/>

On fit le modèle à partir des paramètres issus du GridSearch. On mesure le score réalisé par le modèle sur les deux jeux de données à l’aide du R2. Le score obtenu sur le jeu d’entraînement est de 0,93 et celui obtenu sur le jeu test est de 0,83. Le mse obtenu est de 0.11, soit un mse plus faible que celui obtenu avant de tuner les hyperparamètres. L’application du GridSearch a donc permis d’améliorer le modèle.

### B/Réseau de neurones

On applique une deuxième méthode afin de prédire la note des jeux de société en estimant un réseau de neurones. On construit un premier réseau de neurones composé d’une couche cachée avec 100 neurones en entrée. On utilise la fonction softmax en sortie pour contraindre les prévisions à être positives, car on cherche à prédire des notes qui sont comprises entre 0 et 10. Le jeu d’entraînement est divisé en un jeu d’entraînement et un jeu de validation afin d’évaluer l’évolution de l’erreur au cours des itérations. L’erreur du jeu d’entraînement décroît très vite et se stabilise rapidement. L’erreur sur le jeu de validation fluctue beaucoup sur les premières epochs. Au fur et à mesure des epochs, l’erreur en validation décroît et se rapproche de l’erreur en entraînement. L’algorithme continue donc d’apprendre à mesure qu’il observe de nouvelles données. On peut souligner qu’il y a un peu d’overfitting puisque l’erreur sur le jeu d’entraînement est plus faible que celle du jeu de validation. 
  
*Figure N°8 : Evolution de la fonction de perte en fonction du nombre d'epochs*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/ANN1.jpeg" alt="ANN1" style="width:600px;"/>

On évalue ensuite la performance du modèle sur un échantillon test. On obtient un mse de 0,103, soit une erreur plus faible que celle obtenue avec le SVR. On cherche ensuite à améliorer les performances de ce réseau de neurones en modifiant les paramètres à l’aide d’un GridSearch. Le modèle ayant 2 couches cachées avec 200 neurones chacune ainsi qu’un learning rate de 0.001 est le plus performant en termes de MSE. Sur l’échantillon test, on obtient une erreur MSE légèrement plus faible (0,0936)

*Figure N°9 : Evolution de la fonction de perte en fonction du nombre d'epochs pour le modèle obtenue à l'aide du GridSearch*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/pp.jpeg.jpeg" alt="ANN1" style="width:600px;"/>

Tout comme pour le réseau de neurones précédent, l’erreur du jeu d'entraînement est importante au début puis se stabilise par la suite. L’erreur sur le jeu de validation suit la même allure. La encore, il y a un peu d’overfitting puisque l’erreur sur le jeu d'entraînement est plus faible que celle du jeu de validation.

Le réseau de neurones après optimisation des hyperparamètres permet de prédire au mieux la note moyenne des jeux de société comparativement aux SVR estimés.

## IV. Classification

Notre objectif dans cette seconde partie est d’appliquer des méthodes de classification pour identifier les jeux appartenant ou non à la catégorie des jeux de stratégie. Nous nous intéressons donc aux caractéristiques qui peuvent faire d’un jeu un jeu de stratégie.

Les jeux de stratégie ne représentent que 23 % de la base de données, nous devons donc appliquer une méthode pour rééquilibrer les classes dans l’échantillon. Nous appliquons une méthode d’undersampling. L’échantillon original de 9 302 jeux est ainsi réduit à 4 358 jeux. On retire la base de données, les différentes variables crées précédemment concernant le domaine auquel appartient le jeu à l’exception de la variable « Strategy » qui est la variable que l’on cherche à prédire. En effet, ce ne serait pas pertinent d'inclure ces variables, car l'appartenance à un domaine est quasi exclusive. 

Dans un premier temps, nous appliquons des méthodes de SVM. L’objectif des SVM est de séparer les classes par un hyperplan en maximisant l’écart entre les marges qui entourent cette frontière de décision. Nous estimons d’abord des SVM linéaires que nous comparons également à une régression logistique. Les SVM linéaires sont estimés avec les paramètres par défaut. On procède à une cross validation afin d’identifier le modèle le plus performant selon l’accuracy. 

*Figure N°10 : Evolution de l'accuracy selon les folds en cross validation*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/accuracy_l.png" alt="accuracy_l" style="width:600px;"/>

En moyenne, le SVC avec un kernel linéaire donne la meilleure accuracy (0,8477) et il est par ailleurs relativement stable selon les folds avec un écart-type de 0,006. On applique donc un GridSearch sur ce modèle pour tuner les hyperparamètres. On obtient un SVC avec un kernel linéaire dont le paramètre de régularisation C, est de 1000. La valeur de C élevée indique que le modèle est plus performant en autorisant la présence de moins d’outliers. L’accuracy est de 0,8482. Le GridSearch a donc bien permis d’augmenter la performance du modèle. On évalue la performance de ce modèle out of sample. Le training score est de 0,8482 tandis que le score sur le jeu test est de 0,8303. On peut noter que les deux scores sont proches. Le modèle apprend donc des données sans pour autant être en sur-ajustement.

L’application d’un SVC linéaire permet d’observer l’influence des variables sur le modèle. On peut ainsi noter que le niveau de complexité moyen d’un jeu plus élevé est davantage associé à un jeu de stratégie. De même les jeux faisant intervenir une mécanique de lancé de dés ont un impact positif sur le modèle. En revanche, les jeux avec des mécaniques de management et de Variable Player Powers ont un effet négatif sur le modèle.

*Figure N°11 : Importance des variables sur le SVC linéaire*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/importance.png" alt="importance" style="width:600px;"/>

On applique ensuite des méthodes de SVM non linéaire en utilisant des kernel tricks. On estime des SVM avec trois kernel différents (polynomial, rbf et linéaire) et les paramètres par défaut. En cross validation, c’est le modèle avec un kernel rbf qui permet d’obtenir l’accuracy la plus élevée. On procède à un GridSearch pour tuner les hyperparamètres. On obtient comme meilleur modèle un SVC avec un kernel rbf, un paramètre de régularisation C=100 et un gamma=0,01. L’accuracy sur le jeu d’entraînement est de 0,911 et elle est de 0,8773 sur le jeu test. On peut noter un léger sur-ajustement du modèle, cela est notamment dû au fait que le SVC non linéaire présente davantage de paramètres à estimer par rapport au SVC linéaire. Le fait de recourir à un SVC non linéaire permet d'améliorer l'accuracy.

*Figure N°12 : Matrice de confusion du SVC avec kernel rbf*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/cm_rbf.png" alt="cm_rbf" style="width:400px;"/>

On propose également une classification à l’aide d’un réseau de neurones afin de comparer les performances des différents modèles. On construit un premier réseau de neurones avec une couche cachée comprenant 100 neurones en entrée. On obtient un modèle avec une accuracy de 0,8853, ce qui signifie que le modèle est bien capable de prédire les classes. On peut noter que le modèle présente du sur-ajustement car l’erreur en validation est plus faible que l’erreur sur le jeu d’entraînement et ces deux erreurs ne convergent pas au fil des epochs.

*Figure N°13 : Evolution de la fonction de perte et de l'accuracy*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/courbes_accuracy.png" alt="courbes_accuracy" style="width:500px;"/>

On utilise un GridSearch pour tuner les paramètres du réseau de neurones. On obtient ainsi un réseau de neurones avec 2 couches cachées, chacune ayant 50 neurones en entrée. L’accuracy (0,8899) de ce modèle est plus élevée que celle du premier réseau de neurones. L’optimisation par GridSearch a donc bien permis d’améliorer les capacités prédictives du modèle.

*Tableau N°3 : Tableau de comparaison des indicateurs de performance*

|Spécificité|Recall|F1-score|AUC|
|-:|-------:|------:|------:|
|LinearSVC| 0,8227272|0,843|0,843|0,843|
|RBF SVC| 0,857143|0,885|0,884|0,885|
|Réseaux de neurones | 0,860674|0,886|0,885|0,886|
|Réseaux de neurones (Avec GridSearch)| 0,860310|0,890|0,891|0,890|

On compare les modèles que l’on a estimés à partir de différents indicateurs. À l’exception de la spécificité, tous les indicateurs indiquent que le réseau de neurones dont on a tuné les paramètres apporte les mêmes résultats. Cela implique que le réseau de neurones avec les paramètres par défaut parvient mieux à prédire les cas négatifs que les autres modèles. En revanche, le réseau de neurones optimisé parvient mieux à identifier les vrais cas positifs puisqu’il a un recall plus élevé. Un AUC de 1 indique que le modèle est capable de bien prédire les classes. Cela correspond à l’aire sous la courbe ROC, c’est-à-dire à un arbitrage entre un taux de vrais positifs élevés et un taux de faux positifs qui ne soit pas augmenté par la même occasion.

## V. Conclusion 

Notre étude se découpe finalement en 3 grandes parties. Dans une première partie, nous avons effectué une analyse exploratoire des données. Cela nous a permis de bien comprendre les différentes variables ainsi que les liens qui existent entre chacune d’entre elles. Dans un second temps, notre objectif était de prédire la note moyenne obtenue pour un jeu de société. Nous avons opté pour 2 catégories de modèles : les SVR ainsi que les réseaux de neurones. Afin d’identifier le modèle le plus performant en termes de mean square error (MSE), nous avons effectué une cross validation. Le réseau de neurones après optimisation des hyperparamètres est plus performant afin de prédire la note moyenne d’un jeu. 

Par la suite, notre second objectif était de prédire si un jeu était un jeu de stratégie ou non. Notre variable étant déséquilibrée (plus de 0 que de 1), nous avons tout d'abord appliqué une méthode d’undersampling afin de rééquilibrer les classes. Nous avons appliqué tout comme pour la partie précédente, des méthodes de SVM ainsi que des réseaux de neurones en ne prenant pas en compte les variables binaires concernant le domaine du jeu. Nous comparons les différents modèles à l’aide de plusieurs indicateurs tels que la spécificité, le F1-score, le recall ainsi que l’AUC. Finalement, le réseau de neurones optimisé à l’aide d’un Grid Search est le modèle le plus performant afin de prédire si un jeu est un jeu de stratégie ou non. 

Enfin, on peut noter que l'échantillon sur lequel on travaille est particulier. En effet, les utilisateurs du site BoardGaesGeek dont sont extraites les données sont probablement des personnes qui jouent très régulièrement aux jeux de société et recherchent à échanger avec une communauté de passionnés. Ces personnes ne sont pas nécessairement représentatives de l'ensemble de la population intéressée par les jeux de société. On peut par exemple le percevoir dans la répartition des domaines de jeux avec principalement des jeux de guerre et à l'inverse beaucoup moins de jeux familiaux. Ainsi, même si nous obtenons des modèles performants, on peut supposer qu'ils ne s'appliqueraient pas nécessairement bien si l'on s'intéressait à un autre public.
