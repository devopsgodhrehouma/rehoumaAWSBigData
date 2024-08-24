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


| **Service/Outil**        | **Rôle**                                                         | **Importance**                                                                                                                                                       |
|--------------------------|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AWS Cloud9 IDE**       | Environnement de développement en ligne.                         | Fournit un IDE complet et accessible depuis le navigateur, éliminant le besoin d'installation locale, avec une intégration directe avec les services AWS.            |
| **Amazon S3**            | Service de stockage d'objets dans le cloud.                      | Stocke les fichiers de données (CSV, Parquet) de manière sécurisée et évolutive. Permet un accès facile aux données par d'autres services AWS.                       |
| **Fichiers CSV**         | Format de stockage des données tabulaires.                       | Facile à manipuler et à transférer entre systèmes, mais inefficace pour le stockage de grandes quantités de données.                                                 |
| **Format Parquet**       | Format de fichier columnar optimisé pour l'analyse de données.   | Offre une meilleure performance de stockage et de requête pour les grands ensembles de données, réduisant la taille des fichiers et améliorant la vitesse des requêtes. |
| **AWS Glue**             | Service d'intégration et de préparation des données.             | Automatise la découverte, la transformation, et le catalogage des données, facilitant ainsi l'analyse et l'intégration des données avec d'autres services AWS.       |
| **AWS Glue Crawler**     | Composant d'AWS Glue pour explorer et cataloguer les données.    | Infère automatiquement le schéma des données et les organise dans un catalogue central, simplifiant l'accès aux données pour l'analyse avec Athena.                 |
| **Amazon Athena**        | Service de requêtes SQL sans serveur pour analyser les données.  | Permet d'exécuter des requêtes SQL directement sur des données stockées dans S3 sans besoin de configuration de base de données, offrant une solution rapide et flexible pour l'analyse de données. |
| **Amazon QuickSight**    | Outil de visualisation de données.                               | Crée des rapports et tableaux de bord interactifs pour interpréter et partager les résultats des analyses de données de manière visuelle et intuitive.              |
| **Amazon IAM**           | Gestion des identités et des accès.                              | Assure la sécurité en contrôlant qui peut accéder aux services et ressources AWS, permettant une gestion fine des permissions pour protéger l'environnement AWS.     |

### **Résumé de l'Importance**

- **AWS Cloud9 IDE** : Essentiel pour le développement rapide et l'intégration directe avec AWS.
- **Amazon S3** : Fondamental pour le stockage sécurisé et centralisé des données.
- **Fichiers CSV** : Utile pour la portabilité des données, mais limité en efficacité pour de grandes quantités de données.
- **Format Parquet** : Crucial pour l'optimisation du stockage et de l'analyse des données volumineuses.
- **AWS Glue** : Clé pour la préparation et le catalogage automatisés des données.
- **AWS Glue Crawler** : Vital pour découvrir et organiser les données automatiquement.
- **Amazon Athena** : Indispensable pour les analyses SQL rapides sans infrastructure supplémentaire.
- **Amazon QuickSight** : Important pour transformer les données en visualisations compréhensibles.
- **Amazon IAM** : Crucial pour assurer la sécurité et la gestion des accès dans AWS.



🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

----
*Partie4 - Explication des différents composants du projet Capstone, QUI FAIT QUOI*

----

### **Partie 3 : Explication des différents composants du projet Capstone**

Dans cette section, nous allons explorer en détail les différents composants du pipeline que vous avez mis en place. Nous allons expliquer à quoi sert chaque composant, quand l'utiliser, et ce qu'il fait concrètement. Cela vous aidera à mieux comprendre les technologies que vous utilisez et comment elles fonctionnent ensemble pour accomplir les tâches requises.

---

### **1. AWS Cloud9 IDE**

**À quoi ça sert ?**
- AWS Cloud9 est un environnement de développement intégré (IDE) en ligne qui vous permet d'écrire, d'exécuter, et de déboguer du code directement depuis votre navigateur. 

**Quand l'utiliser ?**
- Vous l'utilisez lorsque vous avez besoin d'un environnement de développement rapide et facile à configurer, sans avoir à installer des logiciels sur votre ordinateur local. 

**Ça fait quoi ?**
- Cloud9 offre un éditeur de code, un terminal, et des outils de débogage dans une seule interface. Il est également intégré avec AWS, ce qui facilite l'accès aux services AWS directement depuis l'IDE.

---

### **2. Amazon S3 (Simple Storage Service)**

**À quoi ça sert ?**
- Amazon S3 est un service de stockage d'objets dans le cloud qui vous permet de stocker et de récupérer n'importe quelle quantité de données à tout moment, de n'importe où.

**Quand l'utiliser ?**
- Vous l'utilisez pour stocker des fichiers tels que des jeux de données, des fichiers Parquet, et d'autres ressources nécessaires à votre projet. C'est particulièrement utile pour stocker des fichiers de grande taille ou pour partager des fichiers entre différents services AWS.

**Ça fait quoi ?**
- S3 vous permet de créer des "buckets" (des conteneurs de stockage) où vous pouvez uploader, organiser, et gérer vos fichiers. Il est hautement disponible, sécurisé, et évolutif, ce qui en fait un choix idéal pour le stockage de données dans le cloud.

---

### **3. CSV (Comma-Separated Values) et Parquet**

**À quoi ça sert ?**
- Les fichiers CSV sont un format de fichier simple utilisé pour stocker des données tabulaires sous forme de texte, où chaque ligne représente un enregistrement et chaque colonne est séparée par une virgule. Parquet est un format de fichier optimisé pour le stockage de données volumineuses.

**Quand l'utiliser ?**
- Les fichiers CSV sont utilisés pour la portabilité des données entre différents systèmes et outils, car ils sont largement supportés. Le format Parquet est utilisé lorsque vous avez besoin d'un stockage plus efficace et performant pour de grandes quantités de données.

**Ça fait quoi ?**
- Convertir des fichiers CSV en Parquet permet de réduire la taille des fichiers et d'améliorer les performances des requêtes sur ces données. Parquet est un format columnar, ce qui signifie qu'il stocke les données par colonne plutôt que par ligne, ce qui est plus efficace pour les requêtes analytiques.

---

### **4. AWS Glue**

**À quoi ça sert ?**
- AWS Glue est un service d'intégration de données qui facilite la préparation des données pour l'analyse. Il inclut des fonctionnalités pour découvrir, transformer, nettoyer, enrichir, et cataloguer les données.

**Quand l'utiliser ?**
- Vous utilisez AWS Glue lorsque vous avez besoin de préparer et de transformer des données stockées dans des sources multiples avant de les analyser ou de les visualiser. C'est également utile pour automatiser l'exploration et le catalogage des données.

**Ça fait quoi ?**
- AWS Glue utilise des "crawlers" pour explorer vos données, inférer leur schéma, et les organiser dans un catalogue central. Cela vous permet de facilement interroger vos données avec d'autres services comme Amazon Athena.

---

### **5. Amazon Athena**

**À quoi ça sert ?**
- Amazon Athena est un service de requêtes SQL sans serveur qui vous permet d'analyser vos données directement dans S3 en utilisant des commandes SQL.

**Quand l'utiliser ?**
- Vous l'utilisez pour exécuter des requêtes SQL sur des données stockées dans S3 sans avoir besoin de configurer des bases de données ou des clusters de traitement. C'est particulièrement utile pour les analyses ad hoc et les requêtes rapides sur de grandes quantités de données.

**Ça fait quoi ?**
- Athena lit les données depuis S3, les interprète en fonction du schéma défini (par exemple via AWS Glue), et exécute des requêtes SQL pour extraire des informations. Vous pouvez ainsi filtrer, agréger, et manipuler les données directement depuis votre navigateur.

---

### **6. Amazon QuickSight**

**À quoi ça sert ?**
- Amazon QuickSight est un service de visualisation de données qui permet de créer des tableaux de bord interactifs et des rapports visuels à partir de vos données.

**Quand l'utiliser ?**
- Vous l'utilisez pour transformer les résultats de vos analyses en visualisations claires et compréhensibles, que vous pouvez partager avec d'autres membres de votre équipe ou présenter aux parties prenantes.

**Ça fait quoi ?**
- QuickSight se connecte à vos sources de données (comme Athena), extrait les données, et vous permet de créer des graphiques, des cartes, et d'autres visualisations interactives. Il vous aide à tirer des conclusions significatives de vos données de manière visuelle.

---

### **7. Amazon IAM (Identity and Access Management)**

**À quoi ça sert ?**
- IAM est un service qui vous permet de gérer de manière sécurisée l'accès aux services et ressources AWS pour vos utilisateurs.

**Quand l'utiliser ?**
- Vous utilisez IAM pour contrôler qui peut accéder à vos ressources AWS, et pour définir les permissions nécessaires pour chaque utilisateur ou rôle.

**Ça fait quoi ?**
- IAM vous permet de créer des utilisateurs et des rôles avec des permissions spécifiques, afin de restreindre l'accès à certaines actions ou services AWS. Cela aide à sécuriser votre environnement AWS en s'assurant que chaque utilisateur ou service dispose uniquement des permissions dont il a besoin.

---

### **Pourquoi tout cela est important ?**

Chaque composant de ce pipeline a été choisi pour sa capacité à rendre votre projet plus efficace, sécurisé, et évolutif. En utilisant ces services ensemble, vous créez une infrastructure robuste qui peut traiter de grandes quantités de données, les analyser, et les visualiser de manière intuitive. C'est cette combinaison de technologies qui permet aux analystes de votre organisation de prendre des décisions éclairées basées sur des données précises et bien préparées.



