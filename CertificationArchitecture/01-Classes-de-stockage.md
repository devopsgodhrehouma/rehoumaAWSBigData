#  Table comparative des services de stockage et de gestion des données d'AWS (Amazon Web Services) :

| **Service**           | **Type de Stockage**          | **Cas d'utilisation principal**                                   | **Durabilité**                       | **Performance**                      | **Modèle de tarification**          | **Chiffrement**                      |
|-----------------------|-------------------------------|-------------------------------------------------------------------|--------------------------------------|--------------------------------------|-------------------------------------|--------------------------------------|
| **Amazon S3**          | Stockage d'objets             | Stockage de fichiers statiques, sauvegarde, big data               | Très élevée (11 9s)                  | Haute (classes Standard, IA, Glacier) | Par Go, par requêtes et par classe  | Au repos et en transit              |
| **Amazon EFS**         | Stockage de fichiers partagé  | Systèmes de fichiers partagés pour plusieurs instances EC2         | Haute (multi-AZ)                     | Ajustable (Standard/Inférieure)      | Par Go utilisé par mois             | Au repos et en transit              |
| **Amazon EBS**         | Stockage de blocs             | Stockage de blocs pour une instance, bases de données, OS          | Haute (basée sur les snapshots)      | Très haute (IOPS avec SSD)           | Par Go provisionné                  | Au repos et en transit              |
| **Amazon RDS**         | Base de données managée       | Bases de données relationnelles managées (MySQL, PostgreSQL, etc.) | Haute (réplication automatique)      | Performances adaptées au moteur DB   | Par instance et par stockage utilisé | Au repos et en transit              |
| **Amazon Aurora**      | Base de données relationnelle | Base de données relationnelle compatible MySQL/PostgreSQL optimisée | Très élevée (6 copies, multi-AZ)     | Haute (performances optimisées)      | Par instance et par volume utilisé  | Au repos et en transit              |
| **Amazon DynamoDB**    | Base de données NoSQL         | Applications à grande échelle, faible latence (NoSQL)              | Très élevée (réplication multi-région)| Très haute (sous millisecondes)      | Par capacité provisionnée et stockage | Au repos et en transit              |
| **Amazon Redshift**    | Data warehouse (entrepôt de données) | Entrepôt de données analytique pour le traitement de big data      | Très élevée (réplication automatique)| Performances élevées (analyse rapide)| Par nœud et par stockage            | Au repos et en transit              |
| **Amazon Glacier**     | Archivage de données          | Archivage de données à long terme                                 | Très élevée (multi-région)           | Faible (récupération lente)          | Très faible coût par Go             | Au repos et en transit              |
| **Amazon FSx**         | Stockage de fichiers Windows/Lustre | Stockage de fichiers hautes performances pour applications Windows ou HPC (Lustre) | Haute (multi-AZ)                    | Très haute (optimisé pour Windows ou HPC) | Par Go utilisé                     | Au repos et en transit              |
| **AWS Backup**         | Sauvegarde unifiée            | Sauvegarde centralisée de divers services AWS (EBS, RDS, DynamoDB, etc.) | Très élevée                          | N/A                                  | Par Go sauvegardé                   | Au repos et en transit              |
| **AWS Snowball**       | Transfert de données physiques| Transfert de données hors-ligne pour migration massive vers AWS    | Très élevée (dispositif sécurisé)    | N/A                                  | Par job et par Go transféré         | Au repos sur l'appareil             |

### Remarques :
- **Amazon S3** propose plusieurs classes de stockage (Standard, Intelligent-Tiering, Glacier) qui influencent le coût et la performance selon la fréquence d'accès.
- **Amazon EFS** est idéal pour les applications nécessitant un stockage partagé entre plusieurs instances.
- **Amazon EBS** est principalement utilisé pour les volumes de disques attachés à des instances EC2, offrant une haute performance pour les applications exigeantes comme les bases de données.
- **Amazon RDS** et **Amazon Aurora** sont des solutions de bases de données managées, simplifiant la gestion et la scalabilité des bases de données relationnelles.
- **Amazon DynamoDB** est optimisé pour des charges de travail NoSQL massives avec des temps de réponse très rapides.
- **Amazon Redshift** est conçu pour les analyses massives de données dans un entrepôt de données, avec des performances élevées pour les requêtes complexes.
- **Amazon Glacier** est une solution d'archivage à long terme, où le coût est faible mais l'accès aux données est plus lent.

- Ces services peuvent être combinés en fonction des besoins spécifiques de votre architecture cloud pour maximiser les performances, la sécurité et l'efficacité des coûts.

# QUIZ

Voici un ensemble de questions à choix multiples (QCM) basées sur des cas d’utilisation pour **on-premises**, **cloud**, et **hybride**, dans le style des examens de certification AWS. Ces questions couvrent divers scénarios d'architecture cloud et de migration.

---

### 1. **Vous gérez une infrastructure existante sur site qui subit des pannes fréquentes en raison de la capacité de stockage limitée. Vous envisagez de migrer certaines parties vers AWS. Quelle architecture convient le mieux pour minimiser les interruptions de service tout en optimisant les coûts ?**

A) Migrer complètement vers AWS et utiliser Amazon S3 pour stocker toutes vos données.

B) Utiliser une architecture hybride en connectant votre centre de données on-premises à AWS à l'aide de **AWS Direct Connect**, tout en stockant les données rarement utilisées dans **Amazon Glacier**.

C) Augmenter la capacité de stockage sur site et utiliser **Amazon EC2** pour la haute disponibilité.

D) Utiliser uniquement **AWS Snowball** pour migrer vos données vers AWS et décommissionner le stockage sur site.

---

### 2. **Votre entreprise utilise un environnement hybride, avec des serveurs de bases de données critiques sur site, mais souhaite augmenter la capacité de calcul temporaire pour un pic de trafic. Quel service AWS est le plus approprié pour ajouter cette capacité sans migrer entièrement vers le cloud ?**

A) Utiliser **AWS Lambda** pour traiter toutes les nouvelles requêtes.

B) Déployer des instances **Amazon EC2** et les connecter à votre infrastructure sur site via **AWS VPN**.

C) Migrer les bases de données vers **Amazon RDS** et utiliser **Amazon EC2 Auto Scaling** pour gérer les variations de charge.

D) Utiliser **AWS Snowball Edge** pour ajouter de la capacité de calcul à votre centre de données.

---

### 3. **Vous avez une application qui doit respecter des exigences de conformité strictes en matière de traitement et de stockage des données dans une région spécifique. Vous souhaitez maintenir certains systèmes sur site pour des raisons de sécurité et de conformité. Quel modèle de déploiement AWS convient le mieux ?**

A) Utiliser une solution entièrement cloud en stockant toutes les données dans **Amazon S3** avec chiffrement.

B) Utiliser une architecture hybride, avec des instances sur site pour le traitement initial des données, et AWS pour les sauvegardes et l'archivage dans **Amazon Glacier**.

C) Migrer toutes les données dans une **instance EC2** dans la région AWS souhaitée.

D) Utiliser uniquement des solutions de stockage AWS comme **Amazon EFS** pour tout stockage de données.

---

### 4. **Votre organisation possède un centre de données sur site et a récemment migré une application critique vers AWS. Vous souhaitez garantir une communication rapide et sécurisée entre votre centre de données et AWS pour des mises à jour fréquentes. Quelle solution vous permettra d'atteindre cet objectif ?**

A) Utiliser **AWS VPN** pour connecter le centre de données à AWS.

B) Utiliser **AWS Direct Connect** pour une connexion dédiée et fiable.

C) Configurer des instances **Amazon EC2** dans une région proche pour réduire la latence.

D) Utiliser **Amazon CloudFront** pour acheminer tout le trafic vers AWS.

---

### 5. **Une entreprise prévoit de migrer progressivement une application de son infrastructure on-premises vers AWS. Elle souhaite commencer par déplacer le stockage de fichiers vers AWS tout en continuant à utiliser ses serveurs sur site pour traiter les données. Quelle option répond le mieux à cette situation ?**

A) Migrer toute l'application vers **Amazon EC2** et décommissionner l'infrastructure on-premises.

B) Utiliser **Amazon S3** pour stocker les fichiers, et configurer un accès direct depuis les serveurs on-premises via **AWS Storage Gateway**.

C) Déployer des serveurs **Amazon EC2** et **Amazon EBS** pour stocker les fichiers à la place de l'infrastructure existante.

D) Utiliser uniquement **AWS Snowball** pour migrer les fichiers vers AWS, et ne plus utiliser l'on-premises.

---

### 6. **Votre entreprise a besoin d'une solution de sauvegarde pour ses serveurs on-premises. Les données doivent être facilement accessibles pendant 30 jours, puis archivées pour une durée indéfinie. Quelle stratégie est la plus rentable ?**

A) Sauvegarder les données sur **Amazon S3** et utiliser la classe **S3 Intelligent-Tiering** pour gérer automatiquement les transitions vers des classes de stockage à moindre coût.

B) Sauvegarder les données sur **Amazon EBS** avec des snapshots fréquents, puis utiliser **Amazon Glacier** après 30 jours.

C) Utiliser **AWS Storage Gateway** pour sauvegarder les données sur **Amazon S3**, puis configurer une politique de cycle de vie pour les déplacer vers **Amazon Glacier** après 30 jours.

D) Utiliser uniquement **Amazon Glacier** pour stocker toutes les sauvegardes dès la première journée.

---

### 7. **Votre entreprise prévoit de mettre en œuvre une stratégie de reprise après sinistre pour ses applications critiques hébergées sur site. L'objectif est de pouvoir redémarrer l'infrastructure dans AWS en cas de défaillance totale du centre de données. Quelle solution est la plus adaptée ?**

A) Configurer des sauvegardes régulières des serveurs sur **Amazon S3** et restaurer manuellement les instances en cas de sinistre.

B) Utiliser **Amazon EC2** avec **Auto Scaling** pour héberger une réplique complète de l'infrastructure on-premises, prête à démarrer à tout moment.

C) Mettre en place une solution **pilot light**, où des instances critiques sont préconfigurées dans AWS mais ne sont démarrées qu'en cas de sinistre.

D) Utiliser uniquement **AWS Backup** pour sauvegarder toutes les instances on-premises vers AWS.

---

### 8. **Vous gérez un environnement de développement sur site qui doit fréquemment cloner et tester de nouvelles versions d'une application. Vous souhaitez accélérer ces tâches tout en optimisant les coûts. Quel service AWS peut améliorer l'efficacité de ces processus tout en minimisant les coûts ?**

A) Utiliser **AWS Elastic Beanstalk** pour automatiser le déploiement des applications.

B) Utiliser **Amazon EC2 Spot Instances** pour déployer des environnements de test temporaires.

C) Migrer toute l'infrastructure vers AWS et utiliser **Amazon RDS** pour stocker les bases de données.

D) Utiliser **AWS Lambda** pour gérer les processus de développement.

---

### 9. **Votre entreprise a un environnement on-premises lourd, et vous envisagez de migrer certaines applications vers AWS pour tester leur comportement dans le cloud. Vous souhaitez une approche qui minimise les changements de code et la migration complète de l'infrastructure. Quel modèle de migration convient le mieux ?**

A) Utiliser **Lift-and-Shift** en migrant les applications directement vers **Amazon EC2** sans modification.

B) Refactoriser les applications pour les adapter aux services cloud natifs comme **AWS Lambda** et **Amazon RDS**.

C) Migrer partiellement les bases de données vers **Amazon Aurora** tout en gardant les serveurs sur site.

D) Utiliser **AWS Elastic Beanstalk** pour héberger automatiquement l'application sans besoin de modifications.

---

### 10. **Votre organisation souhaite adopter une approche "hybride cloud" pour améliorer la flexibilité et réduire les coûts de l'infrastructure. Quel service AWS facilitera le mieux la gestion des ressources on-premises et cloud ensemble ?**

A) **AWS CloudFormation** pour provisionner l'infrastructure sur site et dans le cloud.

B) **AWS Outposts** pour exécuter des services AWS dans votre propre centre de données.

C) **AWS Lambda** pour orchestrer l'ensemble des tâches cloud et on-premises.

D) **AWS Fargate** pour exécuter les conteneurs on-premises.


---

### 1. **Vous gérez une infrastructure existante sur site qui subit des pannes fréquentes en raison de la capacité de stockage limitée. Vous envisagez de migrer certaines parties vers AWS. Quelle architecture convient le mieux pour minimiser les interruptions de service tout en optimisant les coûts ?**

**Réponse : B) Utiliser une architecture hybride en connectant votre centre de données on-premises à AWS à l'aide de AWS Direct Connect, tout en stockant les données rarement utilisées dans Amazon Glacier.**

---

### 2. **Votre entreprise utilise un environnement hybride, avec des serveurs de bases de données critiques sur site, mais souhaite augmenter la capacité de calcul temporaire pour un pic de trafic. Quel service AWS est le plus approprié pour ajouter cette capacité sans migrer entièrement vers le cloud ?**

**Réponse : B) Déployer des instances Amazon EC2 et les connecter à votre infrastructure sur site via AWS VPN.**

---

### 3. **Vous avez une application qui doit respecter des exigences de conformité strictes en matière de traitement et de stockage des données dans une région spécifique. Vous souhaitez maintenir certains systèmes sur site pour des raisons de sécurité et de conformité. Quel modèle de déploiement AWS convient le mieux ?**

**Réponse : B) Utiliser une architecture hybride, avec des instances sur site pour le traitement initial des données, et AWS pour les sauvegardes et l'archivage dans Amazon Glacier.**

---

### 4. **Votre organisation possède un centre de données sur site et a récemment migré une application critique vers AWS. Vous souhaitez garantir une communication rapide et sécurisée entre votre centre de données et AWS pour des mises à jour fréquentes. Quelle solution vous permettra d'atteindre cet objectif ?**

**Réponse : B) Utiliser AWS Direct Connect pour une connexion dédiée et fiable.**

---

### 5. **Une entreprise prévoit de migrer progressivement une application de son infrastructure on-premises vers AWS. Elle souhaite commencer par déplacer le stockage de fichiers vers AWS tout en continuant à utiliser ses serveurs sur site pour traiter les données. Quelle option répond le mieux à cette situation ?**

**Réponse : B) Utiliser Amazon S3 pour stocker les fichiers, et configurer un accès direct depuis les serveurs on-premises via AWS Storage Gateway.**

---

### 6. **Votre entreprise a besoin d'une solution de sauvegarde pour ses serveurs on-premises. Les données doivent être facilement accessibles pendant 30 jours, puis archivées pour une durée indéfinie. Quelle stratégie est la plus rentable ?**

**Réponse : C) Utiliser AWS Storage Gateway pour sauvegarder les données sur Amazon S3, puis configurer une politique de cycle de vie pour les déplacer vers Amazon Glacier après 30 jours.**

---

### 7. **Votre entreprise prévoit de mettre en œuvre une stratégie de reprise après sinistre pour ses applications critiques hébergées sur site. L'objectif est de pouvoir redémarrer l'infrastructure dans AWS en cas de défaillance totale du centre de données. Quelle solution est la plus adaptée ?**

**Réponse : C) Mettre en place une solution pilot light, où des instances critiques sont préconfigurées dans AWS mais ne sont démarrées qu'en cas de sinistre.**

---

### 8. **Vous gérez un environnement de développement sur site qui doit fréquemment cloner et tester de nouvelles versions d'une application. Vous souhaitez accélérer ces tâches tout en optimisant les coûts. Quel service AWS peut améliorer l'efficacité de ces processus tout en minimisant les coûts ?**

**Réponse : B) Utiliser Amazon EC2 Spot Instances pour déployer des environnements de test temporaires.**

---

### 9. **Votre entreprise a un environnement on-premises lourd, et vous envisagez de migrer certaines applications vers AWS pour tester leur comportement dans le cloud. Vous souhaitez une approche qui minimise les changements de code et la migration complète de l'infrastructure. Quel modèle de migration convient le mieux ?**

**Réponse : A) Utiliser Lift-and-Shift en migrant les applications directement vers Amazon EC2 sans modification.**

---

### 10. **Votre organisation souhaite adopter une approche "hybride cloud" pour améliorer la flexibilité et réduire les coûts de l'infrastructure. Quel service AWS facilitera le mieux la gestion des ressources on-premises et cloud ensemble ?**

**Réponse : B) AWS Outposts pour exécuter des services AWS dans votre propre centre de données.**

---
