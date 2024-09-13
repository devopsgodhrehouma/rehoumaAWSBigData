# **Quiz : Gestion des Coûts AWS (avec corrections)**

---

#### **1. Vous gérez plusieurs instances EC2 qui fonctionnent 24/7, et vous constatez que ces instances ont un usage prévisible. Quelle option est la plus économique pour réduire les coûts sur ces instances ?**

A) Utiliser des instances Spot  
B) Passer à des instances On-Demand  
C) **Utiliser des instances Réservées (RI)**  ✅  
D) Laisser les instances telles quelles

**Correction** : Les instances **Réservées (RI)** sont idéales pour des charges de travail stables et prévisibles fonctionnant en continu. Elles permettent de réduire les coûts jusqu'à 75 % par rapport aux instances On-Demand, ce qui les rend très économiques pour ce type de besoin.

---

#### **2. Votre entreprise utilise des instances EC2 pour des charges de travail temporaires, et vous souhaitez réduire les coûts de ces instances, qui ne nécessitent pas de disponibilité continue. Quelle est la meilleure solution ?**

A) Utiliser des instances Réservées (RI)  
B) **Utiliser des instances Spot** ✅  
C) Utiliser des instances On-Demand  
D) Utiliser AWS Lambda

**Correction** : Les **instances Spot** permettent d’utiliser la capacité EC2 inutilisée à des prix réduits (jusqu’à 90% moins cher que les instances On-Demand). Elles sont idéales pour des charges de travail temporaires qui peuvent tolérer des interruptions.

---

#### **3. Votre équipe de développement travaille sur des applications sans serveur. Vous souhaitez surveiller et réduire les coûts associés à ces applications. Quel service vous permet de facturer uniquement les ressources consommées sans provisionner de capacité en avance ?**

A) **AWS Lambda** ✅  
B) Amazon EC2 Auto Scaling  
C) Amazon RDS  
D) Amazon DynamoDB On-Demand

**Correction** : **AWS Lambda** est facturé uniquement pour le temps d'exécution des fonctions et la quantité de mémoire utilisée. Il ne nécessite pas de provisionnement de serveurs, ce qui en fait un choix économique pour les applications sans serveur.

---

#### **4. Vous avez stocké une grande quantité de données dans Amazon S3 et vous remarquez que les données sont rarement accédées. Quelle approche peut réduire vos coûts de stockage ?**

A) Migrer les données vers des instances EC2  
B) Créer des snapshots des données S3  
C) **Configurer des politiques de cycle de vie pour déplacer les données vers S3 Intelligent-Tiering ou Amazon Glacier** ✅  
D) Compresser les données avant de les télécharger sur S3

**Correction** : Configurer des **politiques de cycle de vie** dans **Amazon S3** permet de déplacer automatiquement les objets rarement accédés vers des classes de stockage moins coûteuses comme **S3 Intelligent-Tiering** ou **Amazon Glacier**, réduisant ainsi les coûts.

---

#### **5. Votre entreprise a défini un budget strict pour l'utilisation d'AWS. Quel service vous permet de définir des seuils budgétaires et de recevoir des alertes lorsque les coûts approchent ou dépassent les limites définies ?**

A) AWS CloudTrail  
B) **AWS Budgets** ✅  
C) AWS Cost Explorer  
D) AWS Trusted Advisor

**Correction** : **AWS Budgets** permet de définir des budgets pour les coûts, l’utilisation ou les réservations de services. Vous pouvez recevoir des alertes lorsque vous atteignez ou dépassez les seuils budgétaires définis.

---

#### **6. Votre organisation souhaite migrer une application vers AWS et prévoit d'utiliser une base de données relationnelle. Afin de gérer efficacement les coûts pour des charges de travail variables, quelle option de base de données AWS est la plus économique ?**

A) Amazon RDS avec instances On-Demand  
B) Amazon RDS avec instances Réservées  
C) **Amazon Aurora Serverless** ✅  
D) Amazon Redshift

**Correction** : **Amazon Aurora Serverless** ajuste automatiquement la capacité en fonction des besoins de l'application. Elle est économique pour des charges de travail imprévisibles ou irrégulières, car elle ne facture que la capacité utilisée.

---

#### **7. Vous avez besoin d'analyser les coûts d'utilisation sur plusieurs mois et d'identifier les services qui génèrent le plus de dépenses dans votre compte AWS. Quel service AWS est le plus adapté à cette tâche ?**

A) AWS Budgets  
B) AWS CloudTrail  
C) **AWS Cost Explorer** ✅  
D) AWS Billing Dashboard

**Correction** : **AWS Cost Explorer** est l'outil le plus adapté pour analyser les tendances de coûts et d'utilisation sur plusieurs mois. Il permet de créer des rapports personnalisés et d'identifier les services AWS les plus coûteux.

---

#### **8. Vous avez des serveurs de production qui fonctionnent uniquement pendant les heures de bureau. Quel est le meilleur moyen de réduire les coûts de ces instances EC2 ?**

A) Passer à des instances Réservées  
B) Passer à des instances Spot  
C) **Utiliser AWS Auto Scaling pour arrêter les instances hors des heures de bureau** ✅  
D) Configurer des snapshots des instances pour sauvegarder les données

**Correction** : **AWS Auto Scaling** permet d’ajuster dynamiquement le nombre d’instances en fonction des besoins, y compris d'arrêter ou démarrer des instances selon les heures de bureau, ce qui permet de réaliser des économies en dehors des heures de pointe.

---

#### **9. Votre entreprise veut utiliser une approche hybride pour archiver des données rarement utilisées, mais souhaite limiter les coûts de stockage. Quel service AWS peut vous aider à réduire les coûts tout en conservant l'accessibilité des données à long terme ?**

A) **Amazon S3 Glacier** ✅  
B) Amazon RDS  
C) Amazon EFS  
D) Amazon EBS avec snapshots

**Correction** : **Amazon S3 Glacier** est une solution de stockage d'archivage à long terme, avec des coûts très réduits par rapport à d'autres options. Il est idéal pour stocker des données rarement utilisées tout en les gardant accessibles à long terme.

---

#### **10. Votre équipe souhaite tester une nouvelle application qui nécessite une importante capacité de calcul pour une courte durée. Comment pouvez-vous minimiser les coûts pour ce projet ?**

A) **Utiliser des instances EC2 Spot pour exécuter la capacité de calcul temporaire** ✅  
B) Utiliser des instances EC2 On-Demand pour gérer la charge  
C) Utiliser des instances EC2 Réservées pour anticiper des tests futurs  
D) Configurer Auto Scaling pour augmenter la capacité en fonction de la charge

**Correction** : **Les instances Spot** permettent de bénéficier d’une importante capacité de calcul à un coût réduit, parfait pour des charges de travail à court terme ou temporaires. Les instances Spot offrent des réductions allant jusqu’à 90 % par rapport aux instances On-Demand.

