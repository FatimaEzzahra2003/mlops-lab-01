## 1. Préparation de l’environnement Python
Cette étape permet de créer un environnement isolé afin d’exécuter le projet sans conflits de dépendances.

-**Création de l’environnement virtuel**
 Activation de l’environnement virtuel :
 
<img width="561" height="49" alt="image" src="https://github.com/user-attachments/assets/e0841bc5-d5f1-4521-be3b-037b5f609b95" />

-**Mise à jour de pip**

<img width="1116" height="231" alt="image" src="https://github.com/user-attachments/assets/5cb43d37-bb34-47df-adef-e70eb341420b" />

-**Installation des dépendances**

Les bibliothèques nécessaires au projet sont installées avec la commande suivante :

Ces dépendances permettent :

 -la manipulation des données

 -l’entraînement et l’évaluation du modèle

 -l’exposition du modèle via une API

 -la sauvegarde et le chargement des modèles
 
<img width="923" height="425" alt="image" src="https://github.com/user-attachments/assets/27044a01-0764-4537-a330-bd3f5853eb11" />

## 2. Génération du dataset

Cette étape consiste à créer un jeu de données synthétique de churn client. Le dataset généré servira de base pour l’entraînement et l’évaluation des modèles.

-**Création du fichier de génération**

<img width="637" height="189" alt="image" src="https://github.com/user-attachments/assets/176ac453-db53-4956-9ae0-72f88abb58a7" />

Dans ce fichier, un script a été développé pour générer un jeu de données synthétique de churn client.

Ce script permet de générer des données réalistes décrivant le comportement des clients, telles que l’ancienneté, le nombre de plaintes et le temps d’utilisation. Il calcule ensuite la probabilité de churn à l’aide d’un modèle logistique simple, puis génère une variable cible binaire indiquant si le client quitte le service ou non.

-**Exécution du script**

<img width="719" height="43" alt="image" src="https://github.com/user-attachments/assets/860dee40-a916-47f0-95d0-0b2b11608e96" />

Après exécution, un fichier CSV est créé automatiquement : data/raw.csv




