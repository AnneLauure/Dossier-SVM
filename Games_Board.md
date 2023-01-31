<center><h1> Board Games </h1></center>

## I. Introduction

Notre étude s'appuie sur un dataset de jeux de société provenant du site Kaggle (https://www.kaggle.com/datasets/andrewmvd/board-games). Cet ensemble de données a été extrait à partir du site Web de BoardGameGeek (BGG) et comprend des données sur 16 598 jeux de société. Ces données ont été collectées en février 2021. BGG référence plus de 100 000 jeux de société différents. Ainsi, cela fait de lui, la plus grande collection en ligne de données de jeux de société. La base de données contient uniquement des jeux classés, c'est-à-dire des jeux qui ont obtenu au moins 30 votes. Pour chacun, nous retrouvons les caractéristiques suivantes :

- **ID**: Identifiant BoardGamesGeek
- **Name** : Nom du jeu de société
- **Year Published** : Année de publication du jeu sur BoardGamesGeek
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


