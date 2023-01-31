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





Les dates de publication des jeux de société sont entre -3500 et 2021.

Lorsque l'on observe les jeux pour lesquels la date est négative, on constate qu'il s'agit de jeux traditionnels, par exemple on y retrouve le jeu de go. Il ne s'agit donc pas nécessairement de valeur aberrante.

On fait le choix de ne conserver que les jeux contemporains. Pour ce faire, on supprime les jeux créés avant le 19ème siècle (1800).
