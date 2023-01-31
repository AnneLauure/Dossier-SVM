<center><h1> Board Games </h1></center>

## I. Introduction

Notre étude s'appuie sur un dataset de jeux de société provenant du site Kaggle (https://www.kaggle.com/datasets/andrewmvd/board-games). Cet ensemble de données a été extrait à partir du site Web de BoardGameGeek (BGG) et comprend des données sur 16 598 jeux de société. Ces données ont été collectées en février 2021. BGG référence plus de 100 000 jeux de société différents. Ainsi, cela fait de lui, la plus grande collection en ligne de données de jeux de société. La base de données contient uniquement des jeux classés, c'est-à-dire des jeux qui ont obtenu au moins 30 votes. Pour chacun, nous retrouvons les caractéristiques suivantes :

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

Nous avons tout d'abord regarder les valeurs manquantes pour chacune de nos variables. Nous constatons qu'il manque des valeurs pour 4 de nos variables : *Yr_Published, Owned_users Mechanics* ainsi que la variable *Domains*. 

Il manque une seule valeur pour la variable d'année de publication. C'est pour le jeu 'Hus' que cette information n'est pas renseigné. On remarque également qu'il manque d'autres informations, dont le nombre d'utilisateurs qui ont le jeu, la mécanique et le domaine. Il ne semble donc pas pertinent de concerver ce jeu, on le supprime notre base de données.

On constate que pour les observations pour lesquelles il manque le nombre d'utilisateurs qui possède du jeu, il manque également le domaine et pour la plupart la mécanique de jeu. On supprime donc les jeux pour lesquels cette valeur n'est pas renseignée (22 valeurs).

En ce qui concerne la variable *Domains*, il manque plus de 10 000 observations soit quasiment la moitié des données. Nous faisons tout de même le choix de conserver cette dernière, puisque nous avons un assez grand nombre donné et que cette variable pourrais être pertinente afin de prédire la note moyenne du jeu. Nous supprimons ainsi les jeux pour lesquels le domaine n'est pas renseigné.  

Nous faisons de même pour les 475 valeurs manquantes de la variable *Mechanics*. Il aurait été intéressant d'imputer ses valeurs manquantes par le mode. Néanmoins, cela ne serait pas forcément pertinent puisqu'un jeu peut avoir plusieurs mécaniques différentes. 

Finalement, nous obtenons une base constituée de 9 703 jeux de sociétés.

### B) Analyse des valeurs atypiques

Regardons maintenant s'il existe des valeurs atypiques pour les variables quantitatives. Pour ce faire, nous avons tracer les boxplot de ces variables : 

*Graphique N°1 : Boxplot des variables qauntitatives*
<img src="https://github.com/AnneLauure/Dossier-SVM/blob/main/Image/fig_2_intuition_svm.jpeg" alt="fig_2_intuition_svm" style="width:1400px;"/>

- **Yr_Published**

Les dates de publication des jeux de société sont entre -3500 et 2021. Lorsque l'on observe les jeux pour lesquels la date est négative, on constate qu'il s'agit de jeux traditionnels, par exemple, on y retrouve le jeu de go. Il ne s'agit donc pas nécessairement de valeur aberrante. On fait le choix de ne conserver que les jeux contemporains. Pour ce faire, on supprime les jeux créés avant le 19ème siècle (1800).

L'année de publication, avec des valeurs négatives est assez difficile à interpréter. Nous avons donc fait le choix de créer une nouvelle variable qui représente l'age du jeu que l'on nomme *Age*. Les données ayant été collectées en février 2021, on calcul l'âge du jeu à la date où les données ont été collectées. L'âge moyen des jeux de société est de 18 ans. Le Perudo ainsi que le Casino sont les jeux contemporains les plus anciens de notre base de données.

- **Max_players et Min_players**

On constate qu'il existe des jeux dans la base de données pour lesquels le nombre de joueurs minimum et le nombre de joueurs maximum sont nuls. On supprime ces jeux car il s'agit de valeurs aberrantes. En effet, pour jouer à un jeu de société,il faut au minimum être 1 joueur.

Il y a une observation pour laquelle le nombre de joueurs minimum est de 10. Il s'agit d'un jeu (haggle) qui demande effectivement d'être nombreux pour pouvoir y jouer (cf bgg). Même si cette valeur peut être considérée comme atypique, on fait le choix de la garder, car cela peut avoir un impact important sur la note du jeu. Il faut en moyenne au minimum 2 joueurs pour jouer aux jeux de notre base de données.

On constate qu'il y a deux jeux (Pit Fighter: Fantasy Arena	 et Black Powder: Second Edition) pour lesquels le maximum de joueurs est de plus de 100. Ces sont tous les deux des jeux de Guerres. Lorsqu'un jeu n'a pas de maximum de joueurs indiqué, il est souvent considéré que le maximum est de 99 ou 100. On décide donc de corriger ces valeurs en leur attribuant la valeur de 100.

- **Play_Time**

Le temps de jeu minimum est de 0. Ce sont probablement des erreurs de saisies ou des informations manquantes. Le temps de jeu correspond au temps moyen suggéré par les créateurs des jeux. On supprime ainsi les jeux pour lesquels le temps de jeu est nul.

La variable *Play_Time* présente beaucoup d'observations qui semblent atypiques. Le temps de jeu maximal est de 60 000 minutes, soit 1 000 heures. C'est bien supérieur au temps de 75 % des jeux qui est de 120 minutes. De plus, la moyenne est plus de 2 fois supérieures à la médiane.

Lorsqu'il y a un temps de jeu maximum, c'est celui-ci qui est indiqué dans la base de données. C'est pourquoi certains jeux ont des temps de jeu particulièrement élevé. On remarque que les jeux avec les temps de jeu les plus élevés sont des jeux de la catégorie wargames. Les jeux de guerres sont généralement des jeux de plateaux qui peuvent être parfois très long. L'ensemble des temps de jeux très longs ne sont donc pas des valeurs aberrantes. On peut toutefois soupçonner des valeurs comme 60 000 d'être aberrantes. On fait donc le choix de supprimer les jeux qui durent plus de 24h soit 1440 minutes.

- **Min_age**

On constate que l'âge minimum n'est pas renseigné pour un certains nombre de jeux. On leur attribut un âge en fonction de leur domaine. La majorité des jeux dont l'âge minimum est manquant possèdent un seul domaine. Par conséquent, on leur attribut l'âge médian du domaine auquel ils appartiennent. Pour les jeux pour lesquels il reste des âges minimum à 0, ils combinent plusieurs domaines de jeu. On leur attribue donc la médiane de l'ensemble des jeux du dataset.

- **Users_rated**

La variable *Users_rated* correspond au nombre d'utilisateurs qui ont évalué le jeu. On observe une grande différence entre la moyenne et la médiane, cependant, on décide dans un premier temps de conserver l'ensemble de ces observations. En effet, il y a quelques variables qui ont beaucoup plus de votes que la médiane, mais il s'agit de jeux très populaires (Catan par exemple) dont on ne souhaite pas se séparer.

- **Rating_avg**

Les jeux sont notés de 0 à 10. On remarque que la moyenne et la médiane sont relativement proches. On décide de conserver l'ensemble des observations de cette variable. Il nous semble pertinent de chercher à comprendre les valeurs tant faibles qu'élevées de cette variable. C'est également la variable que l'on souhaite prédire.

- **Complexity_avg**

Les jeux sont notés de 0 à 5. Il y a 16 jeux pour lesquels le niveau de complexité est de 0. Il s'agit de jeu pour lequel les utilisateurs de bgg n'ont pas noté la complexité donc on supprime ces jeux. La complexité moyenne des jeux de notre dataset est de 2.31.

- **Owned_users**

Après réflexion, même si la moyenne et la médiane sont assez éloigné on fait le choix de garder l'ensemble de ses observations. 
