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

**-Création du fichier de préparation :**

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

## 4.Entraîner, versionner et valider le modèle

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

## 5.Entraînement, évaluation et registry du modèle

   Cette étape permet d’entraîner un modèle de churn client, d’évaluer ses performances et de gérer l’enregistrement du modèle dans un registry, selon une logique MLOps.
**-Création du fichier d’entraînement et d’évaluation**

<img width="433" height="124" alt="image" src="https://github.com/user-attachments/assets/63c2aabb-16cf-4ff0-92df-4367bc33c37a" />

Le fichier src/evaluate.py contient un script qui :

 -charge le dataset prétraité data/processed.csv ;
 
 -applique un prétraitement (normalisation des variables numériques et encodage des variables catégorielles) ;
 
 -entraîne un modèle de régression logistique ;
 
 -sépare les données en ensembles d’entraînement et de test avec stratification.
 
 -évalue le modèle (accuracy, precision, recall, F1-score) ;

 -recherche un seuil optimal maximisant la F1-score ;

 -compare les performances à une baseline triviale ;

 -valide le modèle via un seuil minimal de F1 (gate F1).

**-Exécution du script**

 <img width="940" height="205" alt="image" src="https://github.com/user-attachments/assets/ef35551d-e82d-45d1-a1ec-778c85e3b7a9" />

 Après exécution :

 -le modèle entraîné est sauvegardé dans le dossier models/ ;
 
 -les métadonnées (version, métriques, date, seuil optimal) sont enregistrées dans registry/metadata.json ;
 
 -si le modèle passe le gate F1, il est déclaré comme modèle courant via registry/current_model.txt.

 ## 6.Création de l’API de prédiction

 Cette étape consiste à exposer le modèle de churn validé via une API afin de permettre des prédictions en temps réel.

 **-Création du fichier de l’API**

 <img width="393" height="103" alt="image" src="https://github.com/user-attachments/assets/e1869d72-25cb-4f43-b3f4-7bdb7b641571" />

 Le fichier src/api.py contient un script qui :

 -charge dynamiquement le modèle courant depuis registry/current_model.txt ;
 
 -expose un endpoint /health pour vérifier l’état de l’API et du modèle ;
 
 -expose un endpoint /predict pour prédire le churn d’un client ;
 
 -valide les données d’entrée à l’aide de Pydantic ;
 
 -applique une normalisation minimale des variables ;
 
 -retourne la prédiction, la probabilité associée et la latence ;
 
 -journalise chaque prédiction dans un fichier de logs.

 **-Lancement de l’API**

 <img width="544" height="158" alt="image" src="https://github.com/user-attachments/assets/3543b316-3697-4730-b227-43d96af35128" />

 **-Test de l’API**

 <img width="605" height="301" alt="image" src="https://github.com/user-attachments/assets/e59f465f-f131-43c6-9ccd-b6705c30fe05" />

 <img width="605" height="270" alt="image" src="https://github.com/user-attachments/assets/b0ecaa22-8830-4c7b-a93a-6c9ffbc520b2" />

 <img width="605" height="324" alt="image" src="https://github.com/user-attachments/assets/cc482459-7725-4a57-af11-44d4d7ce9d18" />

 <img width="605" height="332" alt="image" src="https://github.com/user-attachments/assets/1292d73a-6898-4df7-8fcc-f5c544a2e65c" />


Après exécution

 -l’API utilise automatiquement le modèle courant validé ;
 
 -chaque requête de prédiction est enregistrée dans logs/predictions.log ;
 
 -le service permet une inférence temps réel du modèle entraîné.

 ## 7.Détection de la dérive des données

   Cette étape permet de surveiller les données reçues en production et de détecter une éventuelle dérive par rapport aux données d’entraînement.

**-Création du script de monitoring**

<img width="478" height="146" alt="image" src="https://github.com/user-attachments/assets/563b34ae-5343-4973-8dee-8d9693fdc790" />


Le fichier src/monitor_drift.py contient un script qui :

 -charge les statistiques d’entraînement depuis registry/train_stats.json ;
 
 -charge les requêtes de prédiction depuis logs/predictions.log ;
 
 -extrait les variables numériques des logs ;
 
 -calcule la moyenne observée en production ;
 
 -compare ces moyennes à celles de l’entraînement via un score Z ;
 
 -déclenche une alerte si un seuil de dérive est dépassé.

**-Exécution du script**

<img width="443" height="81" alt="image" src="https://github.com/user-attachments/assets/c8e33349-1e75-4b84-8532-4a344e6af0cf" />

Après exécution

 -un résumé des scores Z est affiché dans la console ;
 
 -les variables présentant une dérive potentielle sont signalées ;
 
 -une alerte peut être envoyée vers un outil de monitoring externe si configuré ;
 
 -cette étape permet d’anticiper un retraining du modèle.

 ## 8.Gestion des versions et rollback du modèle

Cette étape permet de gérer les versions du modèle et de revenir à une version précédente en cas de problème.

**-Entraînement d’une nouvelle version**

<img width="718" height="133" alt="image" src="https://github.com/user-attachments/assets/fb4bdd1c-154e-46f9-bd20-f1681d86b690" />


Une nouvelle version du modèle peut être entraînée en modifiant le paramètre de version.

**-Création du script de rollback**

<img width="425" height="132" alt="image" src="https://github.com/user-attachments/assets/317f6b43-471f-462a-8752-4b683315f748" />


Le fichier src/rollback.py contient un script qui :

 -lit la liste des modèles enregistrés dans registry/metadata.json ;
 
 -permet de sélectionner un modèle spécifique ;
 
 -permet de revenir automatiquement au modèle précédent ;
 
 -met à jour le fichier registry/current_model.txt.

**-Exécution du rollback**

Rollback automatique vers la version précédente :

<img width="527" height="34" alt="image" src="https://github.com/user-attachments/assets/76f91c0d-31df-45e3-bab2-53439dfa736c" />

Rollback vers une version précise :

<img width="806" height="60" alt="image" src="https://github.com/user-attachments/assets/30d0f1d6-f138-4a9b-8dae-90d3ce3f3d5b" />

Après exécution

 -le modèle courant est mis à jour dans le registry ;
 
 -l’API utilise automatiquement la nouvelle version activée ;
 
 -cette étape permet un retour rapide en cas de déploiement défaillant.
 
  









