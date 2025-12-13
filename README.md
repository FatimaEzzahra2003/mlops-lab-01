## 1. Préparation de l’environnement Python
Cette étape permet de créer un environnement isolé afin d’exécuter le projet sans conflits de dépendances.

-Création de l’environnement virtuel + Activation de l’environnement virtuel :
 
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

## 3. Préparation des données et contrôles qualité

Cette étape prépare les données avant l’entraînement du modèle et vérifie leur qualité afin d’éviter des erreurs dans le pipeline MLOps.

**-Création du fichier de préparation :

<img width="640" height="184" alt="image" src="https://github.com/user-attachments/assets/20075172-bdce-4d5e-96dc-c83f239e2308" />

Dans ce fichier, un script a été développé. Ce module permet de :

 -charger le dataset brut data/raw.csv ;
 
 -nettoyer les données (valeurs négatives, format des variables catégorielles) ;
 
 -vérifier la qualité des données (schéma, valeurs manquantes, types) ;
 
 -générer un dataset prêt à l’entraînement.

**-Contrôles qualité appliqués**

Les règles de qualité implémentées sont :

 -vérification de la présence de toutes les colonnes attendues ;

 -refus du dataset si une colonne contient plus de 5 % de valeurs manquantes ;

 -vérification du type numérique des variables quantitatives.

**-Exécution du script**

<img width="838" height="64" alt="image" src="https://github.com/user-attachments/assets/9af58948-c613-4ee6-b5bc-fd59686ebae1" />

Après exécution du script, les fichiers suivants sont produits :

 -data/processed.csv : dataset nettoyé et prêt pour l’entraînement ;
 
 -registry/train_stats.json : statistiques (moyenne, écart-type) des variables numériques, utiles pour la normalisation.

## Entraîner, versionner et valider le modèle

Cette étape permet d’entraîner un modèle de churn, de versionner les modèles et de valider la qualité avant déploiement.

**-Création du fichier d’entraînement**

<img width="589" height="183" alt="image" src="https://github.com/user-attachments/assets/bcd6bf47-d1e7-4482-ac26-8e6423096918" />

Dans ce fichier, un script a été développé,ce module permet de :

 -charger le dataset prétraité data/processed.csv ;
 
 -séparer les features de la cible churn ;
 
 -construire un pipeline avec StandardScaler pour les variables numériques et OneHotEncoder pour les catégorielles ;
 
 -entraîner un modèle de régression logistique ;
 
 -évaluer les métriques (accuracy, precision, recall, F1) ;
 
 -comparer la F1 avec une baseline trivial ;
 
 -sauvegarder le modèle, les métadonnées et mettre à jour le modèle courant si le gate est passé.

**-Exécution de l’entraînement**

<img width="1342" height="250" alt="image" src="https://github.com/user-attachments/assets/a722a824-7d43-4b13-bdad-45ab3eb84d4f" />

Après exécution :

 -modèles sauvegardés dans models/ ;
 
 -métadonnées dans registry/metadata.json ;
 
 -modèle courant mis à jour dans registry/current_model.txt si le gate F1 est passé.





