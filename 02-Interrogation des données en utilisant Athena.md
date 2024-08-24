---

# **Projet AWS : Interrogation des Données avec Athena**

---

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇

---

### **Aperçu du Lab et Objectifs**

Dans ce lab, vous allez explorer comment utiliser Amazon Athena pour interroger des données stockées dans Amazon S3, optimiser ces requêtes, et configurer des politiques IAM pour un accès sécurisé aux données. À la fin de ce lab, vous serez capable de :

1. Utiliser l'éditeur de requêtes Athena pour créer une base de données AWS Glue et une table.
2. Définir le schéma pour la base de données AWS Glue et les tables associées en utilisant la fonctionnalité d'ajout en masse de colonnes d'Athena.
3. Configurer Athena pour utiliser un dataset situé dans Amazon S3.
4. Optimiser les requêtes Athena contre un dataset d'exemple.
5. Créer des vues dans Athena pour simplifier l'analyse des données pour d'autres utilisateurs.
6. Créer des requêtes nommées Athena en utilisant AWS CloudFormation.
7. Revoir une politique IAM qui peut être assignée aux utilisateurs qui souhaitent utiliser des requêtes nommées dans Athena.
8. Confirmer qu'un utilisateur peut accéder à une requête nommée Athena en utilisant l'AWS Command Line Interface (AWS CLI) dans le terminal AWS Cloud9.

---

### **Durée**

Ce lab prendra environ 90 minutes à compléter.

---

### **Scénario**

Vous allez endosser le rôle d'un membre de l'équipe de science des données. Vous allez construire une preuve de concept (POC) en utilisant AWS Glue et Athena pour analyser les données stockées dans Amazon S3. Vous souhaitez expérimenter avec Athena pour accéder aux données brutes dans un bucket S3, construire une base de données AWS Glue, et interroger cette base de données en utilisant Athena.

Vous voulez également déterminer si cette solution peut être étendue pour que d'autres membres de l'équipe puissent y accéder. Mary fait partie du groupe IAM dans AWS pour l'équipe de science des données et dispose d'un accès similaire au bucket S3, à la base de données AWS Glue, et à Athena. Cependant, vous avez limité son accès pour empêcher la création inutile de ressources. Cela suit le principe du moindre privilège, où un administrateur limite l'accès aux services et ressources AWS en fonction des permissions nécessaires pour accomplir une tâche spécifique.

Vous demandez à Mary si elle a quelques datasets d'exemple que vous pouvez utiliser pour vos expériences. Mary vous donne accès à un dataset contenant des données sur les trajets de taxi pour l'année 2017. Vous utiliserez ces données pour votre POC.

---

### **Tâches du Lab**

#### **Tâche 1 : Création et Interrogation d'une Base de Données et d'une Table AWS Glue dans Athena**

- **Objectif :** Configurer une base de données et une table AWS Glue en utilisant l'éditeur de requêtes Athena, et charger des données d'exemple depuis Amazon S3.
- **Étapes :**
  1. Spécifiez un bucket S3 pour les résultats de requêtes.
  2. Créez une base de données AWS Glue nommée `taxidata` en utilisant l'éditeur de requêtes Athena.
  3. Créez une table dans la base de données `taxidata` et importez les données.
  4. Prévisualisez les données dans la table AWS Glue.

#### **Tâche 2 : Optimisation des Requêtes Athena en Utilisant le Bucketing**

- **Objectif :** Expérimenter le bucketing des données pour améliorer la performance des requêtes et réduire les coûts.
- **Étapes :**
  1. Créez une table nommée `jan` pour les données de janvier 2017.
  2. Comparez le temps nécessaire pour exécuter une requête sur les données bucketisées pour janvier 2017 par rapport à l'ensemble des données de janvier 2017.

#### **Tâche 3 : Optimisation des Requêtes Athena en Utilisant les Partitions**

- **Objectif :** Utiliser les partitions pour optimiser les requêtes sur des champs avec une faible cardinalité.
- **Étapes :**
  1. Créez une table partitionnée nommée `taxidata.creditcard` pour les transactions payées par carte de crédit.
  2. Comparez la performance des requêtes sur des données non partitionnées et des données partitionnées.

#### **Tâche 4 : Utilisation des Vues dans Athena**

- **Objectif :** Simplifier l'analyse des données en créant et utilisant des vues dans Athena.
- **Étapes :**
  1. Créez une vue pour calculer le total des tarifs payés par carte de crédit.
  2. Créez une vue pour calculer le total des tarifs payés en espèces.
  3. Joignez les données des deux vues pour comparer le revenu total provenant des paiements par carte de crédit et en espèces.

#### **Tâche 5 : Création de Requêtes Nommées Athena en Utilisant CloudFormation**

- **Objectif :** Automatiser la création et le partage de requêtes Athena en utilisant AWS CloudFormation.
- **Étapes :**
  1. Créez un template CloudFormation pour générer une requête nommée Athena.
  2. Déployez le template et confirmez que la requête a été créée avec succès.

#### **Tâche 6 : Revue de la Politique IAM pour l'Accès à Athena et AWS Glue**

- **Objectif :** Assurer un accès sécurisé à Athena et AWS Glue en révisant et appliquant les politiques IAM.
- **Étapes :**
  1. Revoyez la politique IAM `Policy-For-Data-Scientists` et confirmez ses permissions.
  2. Testez l'efficacité de la politique en accédant à la requête nommée en utilisant les identifiants de l'utilisateur IAM `mary`.

#### **Tâche 7 : Confirmation de l'Accès pour les Utilisateurs IAM**

- **Objectif :** Vérifier que d'autres utilisateurs peuvent accéder et utiliser la requête nommée Athena créée dans la Tâche 5.
- **Étapes :**
  1. Récupérez les identifiants pour l'utilisateur IAM `mary`.
  2. Testez la capacité de `mary` à exécuter la requête nommée en utilisant l'interface en ligne de commande AWS (CLI).

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
|     Configuration d'une base de données et d'une table    |
|                 via AWS Glue et Athena                    |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|    Exécution de requêtes SQL pour interroger les données  |
|                  et créer des vues dans Athena            |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|      Automatisation de la création de requêtes via        |
|            CloudFormation et partage de celles-ci         |
+-----------------------------------------------------------+
                          |
                          v
+-----------------------------------------------------------+
|  Validation de l'accès des utilisateurs via IAM et AWS CLI |
+-----------------------------------------------------------+
```

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
TBLPRO

PERTIES ('has_encrypted_data'='false');
```

---

### **Explication des Composants AWS Utilisés**

| **Composant AWS**       | **Rôle**                                                          | **Importance**                                                                 |
|-------------------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------|
| **Amazon S3**           | Stockage des datasets CSV pour le projet                          | Permet un stockage sécurisé et scalable des fichiers de données                 |
| **AWS Glue**            | Catalogage et transformation des données                         | Facilite l'exploration et l'analyse des données non structurées                 |
| **Amazon Athena**       | Requête des données cataloguées avec SQL                         | Permet de réaliser des requêtes SQL directement sur des données stockées dans S3|
| **AWS CloudFormation**  | Automatisation de la création et gestion des ressources AWS      | Permet de déployer et gérer l'infrastructure AWS de manière reproductible       |
| **IAM**                 | Gestion des accès et des permissions aux ressources AWS          | Contrôle l'accès aux ressources AWS en définissant des permissions précises     |

---

### **Conclusion**

Ce lab vous a permis de découvrir comment utiliser Athena et AWS Glue pour interroger des données stockées dans S3, tout en optimisant les requêtes et en sécurisant l'accès via des politiques IAM. Vous êtes maintenant prêt à appliquer ces compétences à d'autres projets de science des données, en utilisant les bonnes pratiques pour la gestion des coûts et la sécurisation des données.

---

🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇  
🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇🥇
