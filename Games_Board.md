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

*Graphique N°1 : Boxplot des variables quantitatives*
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

- Il y a 137 jeux de Turn order. Cela représente 1.473 % des jeux de la base de données. Il y a trop peu de jeux de turn order pour que cela soit pertinent de créer une variable pour cette mécanique.

- Les jeux possédant une mécanique contenant le mot “Action” représentent 14.61 % des jeux de la base de données. Ainsi, nous créons une variable binaire intitulée *Action*

- Il y a 508 jeux d'enchères, soit 5.46 % des jeux de la base de données. Il y a trop peu de jeux d'enchères pour que cela soit pertinent de créer une variable pour cette mécanique

- Il y a 1193 jeux de cartes. Cela représente 12.82 % des jeux de la base de données. On crée une variable binaire *Map*. Cette variable prend la valeur de 1 si le jeu à une mécanique contenant le mot “Card”.

Finalement pour la variable *Mechanics* Nous avons créé 9 variables binaires ainsi qu'une variable quantitative représentant le nombre de mécaniques par jeu. 

- **Domains**

La variable *Domains* indique les genres des jeux de société. Tout comme pour la variable *mécanique* on créer une variable quantitative représentant le nombre de domaines par jeu. Un jeu peut avoir plusieurs domaines différents (au maximum 3). Plus de 75 % des jeux de sociétés, ne possèdent qu’un seul domaine. Il y a  8 domaines différents parmi les jeux de la base de données : *Abstract Games*, *Children's Games*, *Customizable Games*, *Family Games*, *Party Games*, *Strategy Games*, *Thematic Games* et *Wargames*. Parmi les jeux de notre dataset on retrouve une majorité de jeux de guerres suivis de jeux de stratégie et de jeux familiaux. Nous créons ainsi une variable binaire pour chacun de ses 8 domaines.

### D) Corrélation entre les variables quantitatives

Afin de voir les liens entre les différentes variables explicatives, nous avons représenté une matrice des corrélations de spearman :

*Figure N°1 : Matrice de corrélation*
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

*Figure N°2 : Matrice de corrélation*
<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/correlation_y.jpeg" alt="correlation_y" style="width:400px;"/>

La variable *BGG_rank*, est la variable la plus corrélée avec la note moyenne d’un jeu. Plus, le jeu est positionné en haut du classement, plus la note moyenne est élevée. La complexité ainsi que l’âge minimal recommandé pour y jouer ont un impact positif relativement important sur la note moyenne du jeu. En revanche, les variables concernant le nombre de joueurs maximum ou minimum sont peu corrélées avec la note moyenne d'un jeu de société.

On s'intéresse également à la distribution des notes selon les modalités des diverses variables qualitatives. On constate qu'il semble y avoir peut de variations liées aux mécaniques et aux domaines. On peut toutefois noter qu'il semble y avoir une différence de moyenne entre les jeux pour enfant et ceux qui ne le sont pas. Les jeux pour enfant (appartenant au domaine "Children") obtiennent une note plus faible en moyenne sur notre échantillon.
*Figure N°3 : cc*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/Children.jpeg" alt="Children" style="width:400px;"/>


## III. Modélisation

L'objectif de notre modélisatoin est de prédire la note moyenne des jeux de société. Pour ce faire on estime deux types de modèle, dans un premier temps on applique un SVR puis on applique un réseau de neurones.  

Préalablement à la modélisation, on procède à un rééchantillonnage. On divise le jeu de données en deux échantillons : 80% des données sont aléatoirement attribuées au jeu d’entraînement et les 20% restant forment le jeu test. Les features quantitatives sont standardisées.

### A/Support Vector Regression

Le premier modèle que nous appliquons est un *SVR* (Support Vector Regression). Les SVR sont des modèles qui appartiennent à la famille de l’apprentissage supervisé. Les SVR permettent de modéliser des relations linéaires mais aussi non linéaires. Le SVM pour la régression consiste, non pas à maximiser les points en dehors des marges comme pour un problème de classification, mais à faire en sorte que le maximum de points se trouvent entre les marges. Le paramètre epsilon permet de faire varier la largeur des marges autour de l’hyperplan. Le paramètre C est un paramètre de régularisation qui indique l’importance attribuée à l’erreur par l’algorithme. Il est également possible de choisir le type de kernel à appliquer pour que les données soient linéairement séparables.

*Figure N°3 : Evolution de l'erreur en cross validation*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/folds.png" alt="folds" style="width:600px;"/>

Dans un premier temps, nous entraînons un LinearSVR puis 3 SVR avec un kernel linéaire, polynomial et rbf. Les 4 modèles sont estimés avec pour paramètres C=100 et epsilon=0.5. Afin de comparer la performance des modèles sur le jeu d’entraînement, on réalise une cross validation en 5 folds. Le score que nous utilisons est le mean square error. Le SVR avec kernel rbf est celui qui permet d’obtenir les erreurs les plus faibles en moyenne sur l’ensemble des folds. On réalise une prévision à partir de ces 4 modèles que l’on compare au jeu test, ce qui permet de constater que le modèle avec kernel rbf est celui qui apporte le mse le plus faible. 

  On cherche à optimiser les hyperparamètres du SVR avec kernel rbf à l’aide d’un GridSearch. Le score mesuré par le GridSearch est le R2 du modèle. On obtient le modèle le plus performant avec les hyperparamètres suivants : C=10 et epsilon=0.1. 

*Figure N°4 : Evolution du score selon les paramètres*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/params.png" alt="params" style="width:600px;"/>

  On trace les courbes d’apprentissage associée au modèle. Cela permet de constater que plus on a de données mieux le modèle apprend. EN effet, on constate qu’à mesure que l’on rajoute des données, l’erreur du jeu de validation diminue et se rapproche de celle du jeu d’apprentissage.

*Figure N°5 : Courbes d'apprentissage*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/learning%20curve.png" alt="learning%20curve" style="width:600px;"/>


  On fit le modèle à partir des paramètres issus du GridSearch. On mesure le score réalisé par le modèle sur les deux jeux de données à l’aide du R2. Le score obtenu sur le jeu d’entraînement est de 0,93 et celui obtenu sur le jeu test est de 0,83. Le mse obtenu est de 0.11, soit un mse plus faible que celui obtenu avant de tuner les hyperparamètres. L’application du GridSearch a donc permis d’améliorer le modèle.

### B/Réseau de neurones

  On applique une deuxième méthode afin de prédire la note des jeux de société en estimant un réseau de neurones. On construit un premier réseau de neurones composé d’une couche cachée avec 100 neurones en entrée. On utilise la fonction softmax en sortie pour contraindre les prévisions à être positives car on cherche à prédire des notes qui sont comprises entre 0 et 10. Le jeu d’entraînement est divisé en un jeu d’entraînement et un jeu de validation afin d’évaluer l’évolution de l’erreur au cours des itérations. L’erreur du jeu d’entraînement décroît très vite et se stabilise rapidement. L’erreur sur le jeu de validation fluctue beaucoup sur les premières epochs. Au fur et à mesure des epochs, l’erreur en validation décroît et se rapproche de l’erreur en entraînement. L’algorithme continue donc d’apprendre à mesure qu’il observe de nouvelles données. On peut souligner qu’il y a un peu d’overfitting puisque l’erreur sur le jeu d’entrainement est plus faible que celle du jeu de validation. 
  
*Figure N°6 : Evolution de la fonction de perte en fonction du nombre d'epochs*

<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/ANN1.jpeg" alt="ANN1" style="width:600px;"/>

  
  On évalue ensuite la performance du modèle sur un échantillon test. On obtient un mse de 0,103, soit une erreur plus faible que celle obtenue avec le SVR. On cherche ensuite à améliorer les performances de ce réseau de neurones en en modifiant les paramètres à l’aide d’un GridSearch. Le modèle ayant 2 couches cachées avec 200 neurones chacunes ainsi qu’un learning rate de 0.001 est  le plus performant en termes de MSE. Sur l’échantillons test on obtient une erreur MSE légèrement plus faible (0,0936)


