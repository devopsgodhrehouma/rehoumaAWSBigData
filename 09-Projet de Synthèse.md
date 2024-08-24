----
# **Projet Capstone en Data Engineering**
----

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
----
*Partie1* 

----

# **Aperçu et objectifs**

Au cours de ce projet Capstone, vous allez créer une solution utilisant plusieurs services AWS que vous connaissez, sans recevoir des instructions détaillées étape par étape. Certaines sections de cet exercice sont conçues pour être difficiles. Ce projet vous mettra au défi de :

- Lancer et configurer un environnement de développement intégré (IDE) AWS Cloud9.
- Transformer des fichiers de données au format CSV en format Apache Parquet et les télécharger sur Amazon S3.
- Créer un crawler AWS Glue pour déduire la structure des données.
- Utiliser Amazon Athena pour interroger les données.
- Créer une vue Athena.
- Utiliser Amazon QuickSight pour visualiser les données.

# **Durée et suivi de votre budget**

Ce projet devrait nécessiter environ 4 heures pour être complété. L'environnement est persistant : lorsque le minuteur de la session atteint 0:00, la session se termine, mais toutes les données et ressources créées dans le compte AWS sont conservées. Vous pouvez relancer la session à tout moment avant la fin du temps imparti en choisissant "Start Lab".

# **Accès aux services AWS**

L'accès aux services et actions AWS peut être restreint aux besoins de ce projet. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'exécuter des actions non décrites dans ce projet.

# **Le jeu de données**

Le site Sea Around Us propose un ensemble de données avec des informations historiques détaillées sur les pêcheries mondiales, de 1950 à 2018. Les données incluent les captures de poissons par pays, les tonnes pêchées, et la valeur de ces captures en dollars américains de 2010.

# **Scénario**

Vous devez créer l'infrastructure pour héberger ces données afin que les analystes puissent créer des rapports sur l'impact de la pêche en haute mer. Vous testerez cette infrastructure en utilisant trois fichiers de données du site Sea Around Us.

# **Accès à la console de gestion AWS**

1. **Lancer l'environnement de lab** :
   - Cliquez sur "Start Lab".
   - Attendez que l'icône en haut à gauche devienne verte avant de continuer.

2. **Connexion à la console de gestion AWS** :
   - Cliquez sur le lien AWS en haut à gauche pour ouvrir la console dans un nouvel onglet.

# **Tâche 1 : Configuration de l'environnement de développement**

1. **Observer les détails du rôle IAM CapstoneGlueRole**.
2. **Créer un environnement AWS Cloud9** :
   - Nommer l'environnement `CapstoneIDE`.
   - Créer une nouvelle instance EC2 t2.micro dans le VPC Capstone, sous-réseau public Capstone.
3. **Créer deux buckets S3** :
   - Dans la région us-east-1.
   - Nommer les buckets `data-source-#####` et `query-results-#####` où ##### est un numéro aléatoire.

4. **Télécharger les fichiers source .csv** dans l'IDE Cloud9 en utilisant les commandes `wget`.

5. **Observer les données** du fichier `SAU-GLOBAL-1-v48-0.csv` avec la commande `head -6 SAU-GLOBAL-1-v48-0.csv`.

6. **Convertir le fichier CSV en format Parquet** en utilisant pandas et pyarrow.

7. **Télécharger le fichier Parquet dans le bucket S3 data-source**.

# **Tâche 2 : Utilisation d’un crawler AWS Glue et requêtes avec Athena**

1. **Configurer un crawler AWS Glue** :
   - Créer une base de données `fishdb` et un crawler `fishcrawler`.
   - Configurer le crawler pour utiliser le rôle IAM `CapstoneGlueRole` et explorer le bucket `data-source`.

2. **Exécuter des requêtes SQL avec Athena** sur les données découvertes.

# **Tâche 3 : Transformation et ajout de nouveaux fichiers**

1. **Analyser la structure du fichier SAU-EEZ-242-v48-0.csv**.
2. **Modifier les noms de colonnes** pour les aligner avec les autres fichiers et convertir en format Parquet.
3. **Télécharger le fichier transformé dans le bucket S3**.
4. **Mettre à jour la table dans AWS Glue** en réexécutant le crawler.

# **Tâche 4 : Visualisation des résultats dans QuickSight**

1. **Créer un compte QuickSight** et configurer l'accès à Athena.
2. **Créer un jeu de données QuickSight** basé sur la vue `MackerelsCatch`.
3. **Créer un graphique dans QuickSight** représentant les tonnes de maquereaux pêchées par année et par pays.

# **Diagramme ASCII du Pipeline**

```
    +--------------------+
    | Lancer AWS Cloud9  |
    +---------+----------+
              |
              v
    +--------------------+
    | Créer S3 Buckets   |
    +---------+----------+
              |
              v
    +---------------------------+
    | Télécharger CSV dans Cloud9|
    +---------+------------------+
              |
              v
    +-----------------------------+
    | Convertir CSV en Parquet    |
    +---------+-------------------+
              |
              v
    +---------------------------+
    | Télécharger Parquet sur S3 |
    +---------+------------------+
              |
              v
    +------------------------+
    | Configurer Glue Crawler|
    +---------+--------------+
              |
              v
    +-----------------------+
    | Requêtes avec Athena  |
    +---------+-------------+
              |
              v
    +------------------------+
    | Visualiser dans QuickSight |
    +------------------------+
```

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
----
*Partie2 - Pipeline détaillé du projet Capstone en Data Engineering**

----



# **1. Lancer et configurer l'environnement de développement**

**Objectif :** Créer un environnement de développement intégré (IDE) sur AWS Cloud9 pour travailler sur ce projet.

1. **Créer un environnement Cloud9 :**
   - **Pourquoi ?** AWS Cloud9 est un IDE basé sur le cloud qui vous permet d'écrire, d'exécuter et de déboguer votre code à partir de votre navigateur.
   - **Comment faire ?**
     - Allez sur la console AWS, puis recherchez "Cloud9".
     - Cliquez sur "Create environment".
     - Donnez un nom à votre environnement, par exemple `CapstoneIDE`.
     - Sélectionnez "Create a new EC2 instance" et choisissez une instance `t2.micro`, qui est gratuite dans le cadre de l'offre gratuite d'AWS.
     - Assurez-vous que l'instance est déployée dans le VPC Capstone et dans le sous-réseau public Capstone.
     - Gardez toutes les autres options par défaut et cliquez sur "Create environment".

2. **Vérifier les rôles IAM :**
   - **Pourquoi ?** Le rôle IAM `CapstoneGlueRole` permet à différents services AWS de communiquer en toute sécurité et d'accéder aux ressources nécessaires.
   - **Comment faire ?**
     - Dans la console AWS, recherchez "IAM".
     - Allez dans "Roles" et recherchez `CapstoneGlueRole`.
     - Vérifiez les permissions associées à ce rôle pour vous assurer qu'il peut accéder aux ressources AWS nécessaires.

# **2. Créer et configurer les buckets S3**

**Objectif :** Stocker les fichiers de données dans Amazon S3, un service de stockage d'objets sécurisé et évolutif.

1. **Créer deux buckets S3 :**
   - **Pourquoi ?** Les buckets S3 vous permettent de stocker et d'organiser les fichiers de données utilisés dans ce projet.
   - **Comment faire ?**
     - Dans la console AWS, recherchez "S3".
     - Cliquez sur "Create bucket".
     - Nommez le premier bucket `data-source-#####` (remplacez `#####` par un numéro aléatoire).
     - Nommez le deuxième bucket `query-results-#####`.
     - Sélectionnez la région `us-east-1` pour les deux buckets.
     - Gardez les paramètres par défaut et cliquez sur "Create bucket".

# **3. Télécharger les fichiers de données CSV**

**Objectif :** Obtenir les fichiers de données CSV nécessaires au projet et les examiner.

1. **Télécharger les fichiers CSV :**
   - **Pourquoi ?** Ces fichiers contiennent des données historiques sur la pêche, que vous allez transformer et analyser.
   - **Comment faire ?**
     - Ouvrez le terminal dans votre environnement Cloud9.
     - Exécutez les commandes suivantes pour télécharger les fichiers CSV :

       ```bash
       wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-GLOBAL-1-v48-0.csv
       wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-HighSeas-71-v48-0.csv
       wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-EEZ-242-v48-0.csv
       ```

2. **Examiner les données :**
   - **Pourquoi ?** Comprendre la structure des fichiers CSV est essentiel pour les prochaines étapes de transformation.
   - **Comment faire ?**
     - Pour afficher les premières lignes d'un fichier, utilisez la commande `head` :

       ```bash
       head -6 SAU-GLOBAL-1-v48-0.csv
       ```

     - Cette commande affiche les en-têtes de colonnes et les premières lignes de données, vous donnant un aperçu de ce que contient le fichier.

# **4. Conversion des fichiers CSV en format Parquet**

**Objectif :** Convertir les fichiers CSV en format Parquet, un format de stockage optimisé pour les gros volumes de données.

1. **Installer les outils nécessaires :**
   - **Pourquoi ?** Vous aurez besoin de bibliothèques Python spécifiques pour effectuer la conversion.
   - **Comment faire ?**
     - Dans le terminal de Cloud9, installez les bibliothèques avec la commande suivante :

       ```bash
       sudo pip3 install pandas pyarrow fastparquet
       ```

2. **Convertir le fichier CSV en Parquet :**
   - **Pourquoi ?** Le format Parquet est plus efficace pour le stockage et la requête de données volumineuses.
   - **Comment faire ?**
     - Démarrez une session Python interactive dans le terminal :

       ```bash
       python
       ```

     - Utilisez le code suivant pour lire le fichier CSV et le convertir en Parquet :

       ```python
       import pandas as pd
       df = pd.read_csv('SAU-GLOBAL-1-v48-0.csv')
       df.to_parquet('SAU-GLOBAL-1-v48-0.parquet')
       exit()
       ```

# **5. Téléchargement des fichiers Parquet sur S3**

**Objectif :** Stocker les fichiers convertis dans le bucket S3.

1. **Télécharger les fichiers Parquet sur S3 :**
   - **Pourquoi ?** Les fichiers Parquet doivent être disponibles dans S3 pour être analysés avec les autres services AWS.
   - **Comment faire ?**
     - Utilisez la commande AWS CLI suivante dans le terminal Cloud9 pour télécharger le fichier Parquet :

       ```bash
       aws s3 cp SAU-GLOBAL-1-v48-0.parquet s3://data-source-#####/
       ```

# **6. Utilisation d'un crawler AWS Glue et requêtes avec Athena**

**Objectif :** Configurer un crawler AWS Glue pour explorer les données et utiliser Amazon Athena pour interroger les données.

1. **Configurer un crawler AWS Glue :**
   - **Pourquoi ?** AWS Glue vous permet de cataloguer et préparer vos données pour l'analyse.
   - **Comment faire ?**
     - Créez une base de données `fishdb` et un crawler `fishcrawler` dans AWS Glue.
     - Configurez le crawler pour utiliser le rôle IAM `CapstoneGlueRole` et explorer le bucket `data-source`.

2. **Exécuter des requêtes avec Athena :**
   - **Pourquoi ?** Athena vous permet de faire des requêtes SQL directement sur vos données stockées dans S3.
   - **Comment faire ?**
     - Accédez à Athena via la console AWS, configurez l'éditeur de requêtes pour stocker les résultats dans votre bucket `query-results`.
     - Exécutez une requête pour vérifier que vos données sont correctement cataloguées.

# **7. Visualisation des résultats avec Amazon QuickSight**

**Objectif :** Créer des visualisations pour analyser les résultats de vos requêtes.

1. **Créer un compte QuickSight :**
   - **Pourquoi ?** QuickSight est un outil de visualisation de données qui vous permet de créer des rapports interactifs.
   - **Comment faire ?**
     - Allez sur la console QuickSight, inscrivez-vous et configurez l'accès à Athena.

2. **Créer des visualisations :**
   - **Pourquoi ?** Pour transformer les données en graphiques et rapports compréhensibles.
   - **Comment faire ?**
     - Créez un nouveau jeu de données à partir de la vue `MackerelsCatch`.
     - Créez un graphique représentant les tonnes de maquereaux pêchées par année et par pays.

# **Diagramme du Pipeline**


```
      +-------------------------------------+
      | 1. Lancer AWS Cloud9 IDE            |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 2. Créer S3 Buckets                 |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 3. Télécharger fichiers CSV         |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 4. Conversion CSV -> Parquet        |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 5. Télécharger Parquet sur S3       |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 6. Configurer AWS Glue Crawler      |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 7. Requêtes SQL avec Athena         |
      +------------------+------------------+
                         |
                         v
      +-------------------------------------+
      | 8. Visualisation avec QuickSight    |
      +-------------------------------------+
```

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
----
*Partie3 - Explication des différents composants**

----

