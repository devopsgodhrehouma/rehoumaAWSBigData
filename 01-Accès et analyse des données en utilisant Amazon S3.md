# **Projet AWS S3 et IAM : Accès et Analyse de Données**
----

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
----
*Partie 1*

----

# **Aperçu et objectifs**

Dans ce projet, vous apprendrez à manipuler des données dans Amazon S3, y compris l'envoi, l'encryptage, la compression, et la sécurisation de fichiers au format .csv. Vous utiliserez également la fonctionnalité S3 Select pour interroger des données directement dans Amazon S3 à l'aide de requêtes SQL, sans nécessiter la mise en place d'une base de données.

### **Objectifs du projet :**
- Créer un modèle AWS CloudFormation pour déployer un bucket S3.
- Charger des données dans un bucket S3.
- Utiliser S3 Select pour interroger un fichier .csv stocké dans S3.
- Modifier les propriétés de chiffrement et le type de stockage d'un objet S3.
- Télécharger et compresser un jeu de données dans Amazon S3.
- Examiner une politique IAM et tester l'accès restreint d'un membre de l'équipe à l'aide d'AWS CLI.

# **Durée**

Ce projet nécessite environ 60 minutes pour être complété.

# **Restrictions des services AWS**

L'accès aux services et actions AWS peut être limité aux besoins de ce projet. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'exécuter des actions non décrites dans ce projet.

# **Scénario**

Vous êtes membre de l'équipe Data Science et votre objectif est de créer une preuve de concept (POC) en utilisant Amazon S3 pour accomplir certaines tâches d'analyse de données. Vous allez expérimenter avec S3 Select pour effectuer des requêtes SQL sur de grands fichiers .csv compressés et encryptés, directement dans S3.

# **Accès à la console AWS**

1. **Lancer l'environnement de lab** :
   - Cliquez sur "Start Lab".
   - Attendez que l'icône en haut à gauche devienne verte avant de continuer.

2. **Connexion à la console de gestion AWS** :
   - Cliquez sur le lien AWS en haut à gauche pour ouvrir la console dans un nouvel onglet.

# **Tâche 1 : Création d'un modèle et d'une pile CloudFormation**

1. **Créer un modèle CloudFormation** :
   - Créez un fichier nommé `create_bucket.yml` dans AWS Cloud9.
   - Ajoutez le code YAML pour créer un bucket S3 avec chiffrement SSE-S3.

2. **Valider le modèle** :
   - Utilisez la commande AWS CLI pour valider le modèle : 
     ```bash
     aws cloudformation validate-template --template-body file://create_bucket.yml
     ```

3. **Créer la pile CloudFormation** :
   - Utilisez la commande AWS CLI pour créer la pile :
     ```bash
     aws cloudformation create-stack --stack-name ade-my-bucket --template-body file://create_bucket.yml
     ```

4. **Vérifier la création du bucket** :
   - Listez les buckets S3 pour vérifier la création du bucket `ade-my-bucket`.

5. **Supprimer la pile** :
   - Supprimez la pile pour détruire le bucket et les ressources associées :
     ```bash
     aws cloudformation delete-stack --stack-name ade-my-bucket
     ```

# **Tâche 2 : Chargement de données dans S3**

1. **Télécharger et examiner un fichier .csv** :
   - Utilisez `wget` pour télécharger un fichier CSV exemple et `cat` pour visualiser son contenu.

2. **Copier le fichier dans S3** :
   - Utilisez AWS CLI pour copier le fichier dans le bucket S3 :
     ```bash
     aws s3 cp lab1.csv s3://<LAB-BUCKET-NAME>
     ```

# **Tâche 3 : Interrogation des données avec S3 Select**

1. **Utiliser S3 Select pour interroger les données** :
   - Dans la console S3, utilisez S3 Select pour exécuter des requêtes SQL sur le fichier .csv chargé.

2. **Ajuster les requêtes SQL** :
   - Modifiez la requête SQL pour afficher uniquement les prénoms des trois premières lignes :
     ```sql
     SELECT "First Name" FROM s3object s LIMIT 3
     ```

# **Tâche 4 : Modification des propriétés de chiffrement et de stockage**

1. **Modifier la classe de stockage** :
   - Changez la classe de stockage du fichier dans S3 pour "Intelligent-Tiering".

# **Tâche 5 : Compression et interrogation des fichiers**

1. **Compresser le fichier avec GZIP** :
   - Utilisez la commande `gzip` pour compresser le fichier :
     ```bash
     gzip -v lab1.csv
     ```

2. **Télécharger et interroger le fichier compressé** :
   - Téléchargez le fichier compressé dans S3 et utilisez S3 Select pour le requêter.

# **Tâche 6 : Gestion des accès restreints avec IAM**

1. **Examiner la politique IAM** :
   - Consultez la politique IAM attachée au groupe de l'équipe Data Science.

2. **Tester les permissions d'un utilisateur** :
   - Utilisez AWS CLI avec les credentials d'un utilisateur spécifique pour tester les accès :
     ```bash
     AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws s3api get-object --bucket <LAB-BUCKET-NAME> --key lab1.csv --region us-east-1 pulled_lab.csv
     ```

# **Diagramme du Workflow**

```
+-----------------------------------------------------+
|            Création d'un Bucket S3 avec             |
|             CloudFormation et chargement            |
+--------------------------+--------------------------+
                           |
                           v
+-----------------------------------------------------+
|   Interrogation des données avec S3 Select sur S3   |
+--------------------------+--------------------------+
                           |
                           v
+-----------------------------------------------------+
|    Modification des Propriétés et Compression       |
+--------------------------+--------------------------+
                           |
                           v
+-----------------------------------------------------+
|         Gestion des accès restreints avec IAM       |
+-----------------------------------------------------+
```

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

----
*Partie 7 - Révision des Composants AWS et de leurs Rôles*

----

| **Composant AWS**        | **Rôle**                                                         | **Importance**                                                                                                                                                       |
|--------------------------|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AWS CloudFormation**    | Automatisation de la création d'infrastructure.                 | Permet de déployer et gérer l'infrastructure AWS de manière reproductible à l'aide de modèles.                                                                         |
| **Amazon S3**             | Stockage d'objets dans le cloud.                                | Stocke les fichiers de données de manière sécurisée et évolutive.                                                                                                      |
| **S3 Select**             | Interrogation directe des fichiers dans S3.                     | Permet d'effectuer des requêtes SQL sur des fichiers stockés dans S3, sans nécessiter une base de données.                                                           |
| **GZIP**                  | Compression de fichiers.                                        | Réduit la taille des fichiers pour optimiser les coûts de stockage dans S3.                                                                                          |
| **Amazon IAM**            | Gestion des identités et des accès.                             | Contrôle l'accès aux ressources AWS en définissant des permissions détaillées pour les utilisateurs et les rôles.                                                    |

### **Conclusion**

Chaque composant AWS est essentiel pour atteindre les objectifs de ce projet, en assurant une gestion efficace des données et en garantissant leur sécurité et leur disponibilité. En comprenant et en maîtrisant ces outils, vous pouvez optimiser vos opérations de Data Science et réduire les erreurs humaines liées à la manipulation manuelle des données.

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

🥇🥇🥇🥇🥇
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇


