----
# **Projet Capstone en Data Engineering**
----

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

---

