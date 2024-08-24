
---

# **Lab : Interrogation des données avec Athena**

---

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

---

### **Aperçu du Lab et Objectifs**

Dans ce lab, vous allez explorer comment utiliser Amazon Athena pour interroger des données stockées dans Amazon S3, optimiser ces requêtes, et configurer des politiques IAM pour un accès sécurisé aux données. À la fin de ce lab, vous serez capable de :

1. Utiliser l'éditeur de requêtes Athena pour créer une base de données et une table AWS Glue.
2. Définir le schéma de la base de données AWS Glue en utilisant la fonctionnalité d'ajout en masse de colonnes d'Athena.
3. Optimiser les requêtes Athena pour améliorer les performances et réduire les coûts.
4. Créer des vues dans Athena pour simplifier l’analyse des données.
5. Utiliser AWS CloudFormation pour créer et partager des requêtes nommées dans Athena.
6. Revoir les politiques IAM pour assurer un accès sécurisé à Athena et AWS Glue.

---

### **Durée**

Ce lab prendra environ 90 minutes à compléter.

---

### **Scénario**

Vous allez endosser le rôle d'un membre de l'équipe de science des données travaillant sur une preuve de concept (POC) pour interroger et analyser des données en utilisant Athena. L'objectif est de construire une solution évolutive qui peut être partagée avec d'autres membres de l'équipe et départements tout en assurant la sécurité des données et une performance efficace.

---

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

---

### **Aperçu des Tâches du Lab**

#### **Tâche 1 : Création et Interrogation d'une Base de Données et Table AWS Glue avec Athena**

- **Objectif :** Configurer une base de données et une table AWS Glue en utilisant l'éditeur de requêtes Athena, et charger des données d'exemple depuis Amazon S3.
- **Étapes :**
  1. Créer une base de données AWS Glue nommée `taxidata` en utilisant l'éditeur de requêtes Athena.
  2. Créer une table dans la base de données `taxidata` et définir le schéma en utilisant la fonctionnalité d'ajout en masse de colonnes.
  3. Charger les données d'exemple des trajets de taxi pour 2017 depuis un bucket S3 dans la table.
  4. Prévisualiser les données dans la table AWS Glue.

#### **Tâche 2 : Optimisation des Requêtes Athena en Utilisant le Bucketing**

- **Objectif :** Expérimenter le bucketing des données pour améliorer la performance des requêtes et réduire les coûts.
- **Étapes :**
  1. Créer une table nommée `jan` pour les données de janvier 2017.
  2. Comparer la performance des requêtes et les données scannées pour l’ensemble des données par rapport aux données bucketisées de janvier.

#### **Tâche 3 : Optimisation des Requêtes Athena en Utilisant les Partitions**

- **Objectif :** Utiliser les partitions pour optimiser les requêtes sur les champs avec une faible cardinalité.
- **Étapes :**
  1. Créer une table partitionnée nommée `taxidata.creditcard` pour les trajets de taxi payés par carte de crédit.
  2. Comparer la performance des requêtes sur des données non partitionnées versus des données partitionnées.

#### **Tâche 4 : Utilisation des Vues dans Athena**

- **Objectif :** Simplifier l'analyse des données en créant et utilisant des vues dans Athena.
- **Étapes :**
  1. Créer une vue pour calculer le total des tarifs payés par carte de crédit.
  2. Créer une vue pour calculer le total des tarifs payés en espèces.
  3. Joindre les données des deux vues pour comparer le revenu total provenant des paiements par carte de crédit par rapport aux paiements en espèces.

#### **Tâche 5 : Création de Requêtes Nommées Athena en Utilisant CloudFormation**

- **Objectif :** Automatiser la création et le partage de requêtes Athena en utilisant AWS CloudFormation.
- **Étapes :**
  1. Créer un template CloudFormation pour générer une requête nommée Athena.
  2. Déployer le template et confirmer que la requête a été créée avec succès.

#### **Tâche 6 : Revue de la Politique IAM pour l'Accès à Athena et AWS Glue**

- **Objectif :** Assurer un accès sécurisé à Athena et AWS Glue en révisant et appliquant les politiques IAM.
- **Étapes :**
  1. Revoir la politique IAM `Policy-For-Data-Scientists` et confirmer ses permissions.
  2. Tester l'efficacité de la politique en accédant à la requête nommée en utilisant les identifiants de l'utilisateur IAM `mary`.

#### **Tâche 7 : Confirmation de l'Accès pour les Utilisateurs IAM**

- **Objectif :** Vérifier que d'autres utilisateurs peuvent accéder et utiliser la requête nommée Athena créée dans la Tâche 5.
- **Étapes :**
  1. Récupérer les identifiants pour l'utilisateur IAM `mary`.
  2. Tester la capacité de `mary` à exécuter la requête nommée en utilisant l'interface en ligne de commande AWS (CLI).

---

### **Code de Création de la Base de Données et de la Table**

#### **Création de la Base de Données AWS Glue :**

```sql
CREATE DATABASE taxidata;
```

#### **Création de la Table dans la Base de Données `taxidata` :**

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS taxidata.yellow (
    `vendor` string,
    `pickup` timestamp,
    `dropoff` timestamp,
    `count` int,
    `distance` int,
    `ratecode` string,
    `storeflag` string,
    `pulocid` string,
    `dolocid` string,
    `paytype` string,
    `fare` decimal,
    `extra` decimal,
    `mta_tax` decimal,
    `tip` decimal,
    `tolls` decimal,
    `surcharge` decimal,
    `total` decimal
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
    'serialization.format' = ',',
    'field.delim' = ','
) LOCATION 's3://aws-tc-largeobjects/CUR-TF-200-ACDSCI-1/Lab2/yellow/'
TBLPROPERTIES ('has_encrypted_data'='false');
```

---

### **Diagramme du Workflow**

```
+-----------------------------------------------------------+
|               Téléchargement des fichiers CSV             |
|                          (S3)                             |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|    Transformation des CSV en Parquet via AWS Cloud9        |
|           (Interaction avec S3 pour lire et écrire)       |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|    Exploration et Catalogage des fichiers Parquet          |
|       via AWS Glue Crawler (Interaction avec S3)           |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|    Requêtes SQL sur les données cataloguées via Athena     |
|            (Interaction avec S3 et AWS Glue)              |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|  Visualisation des résultats via Amazon QuickSight         |
|        (Interaction avec Athena pour les données)         |
+-----------------------------------------------------------+
```

---

### **Explication des Composants**

#### **AWS Cloud9 IDE**

**À quoi ça sert ?**
- AWS Cloud9 est un environnement de développement intégré (IDE) en

 ligne qui permet d'écrire, d'exécuter et de déboguer du code directement depuis votre navigateur.

**Quand l'utiliser ?**
- Utilisé pour un environnement de développement rapide et facile à configurer.

**Ça fait quoi ?**
- Cloud9 offre un éditeur de code, un terminal, et des outils de débogage dans une seule interface, intégrée avec AWS.

#### **Amazon S3**

**À quoi ça sert ?**
- Amazon S3 est un service de stockage d'objets dans le cloud, utilisé pour stocker des fichiers comme des jeux de données CSV ou Parquet.

**Quand l'utiliser ?**
- Utilisé pour stocker et organiser les fichiers de données de manière sécurisée et évolutive.

**Ça fait quoi ?**
- S3 permet de créer des buckets où les fichiers sont uploadés, organisés, et gérés.

#### **AWS Glue**

**À quoi ça sert ?**
- AWS Glue est un service d'intégration de données qui facilite la préparation des données pour l'analyse.

**Quand l'utiliser ?**
- Utilisé pour explorer, transformer, et cataloguer des données stockées dans Amazon S3.

**Ça fait quoi ?**
- AWS Glue utilise des crawlers pour explorer les fichiers, inférer leur schéma, et les organiser dans une base de données pour les rendre accessibles via d'autres services comme Athena.

#### **Amazon Athena**

**À quoi ça sert ?**
- Amazon Athena est un service de requêtes SQL sans serveur pour analyser les données stockées dans S3.

**Quand l'utiliser ?**
- Utilisé pour exécuter des requêtes SQL sur des données stockées dans S3.

**Ça fait quoi ?**
- Athena permet de filtrer, agréger, et manipuler les données directement depuis le navigateur en utilisant des commandes SQL.

#### **Amazon QuickSight**

**À quoi ça sert ?**
- Amazon QuickSight est un outil de visualisation de données.

**Quand l'utiliser ?**
- Utilisé pour créer des rapports et des tableaux de bord interactifs à partir des données analysées.

**Ça fait quoi ?**
- QuickSight extrait les données de services comme Athena et les transforme en visualisations compréhensibles.

#### **Amazon IAM**

**À quoi ça sert ?**
- IAM est un service qui gère de manière sécurisée l'accès aux services et ressources AWS.

**Quand l'utiliser ?**
- Utilisé pour contrôler l'accès aux ressources AWS et définir les permissions des utilisateurs.

**Ça fait quoi ?**
- IAM permet de créer des utilisateurs et des rôles avec des permissions spécifiques pour sécuriser l'environnement AWS.

