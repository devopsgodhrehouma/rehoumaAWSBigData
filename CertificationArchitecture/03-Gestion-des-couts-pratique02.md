### **Quiz : Gestion des Coûts AWS (avec corrections)**


#### **1. Vous gérez une application avec des pics de trafic imprévisibles. Vous souhaitez que cette application puisse évoluer automatiquement tout en optimisant les coûts. Quelle option est la plus appropriée ?**

A) Utiliser des instances EC2 Réservées  
B) Utiliser des instances EC2 On-Demand  
C) **Utiliser EC2 Auto Scaling avec des instances Spot** ✅  
D) Utiliser AWS Lambda

**Correction** : **EC2 Auto Scaling avec des instances Spot** vous permet d'ajuster dynamiquement la capacité en fonction de la demande, tout en profitant de réductions significatives en utilisant des instances Spot pour les pics de charge imprévisibles.

---

#### **2. Vous avez des données très importantes qui doivent être régulièrement sauvegardées et accessibles pendant une courte période, puis archivées pour plusieurs années. Quel service AWS est le plus économique pour cette situation ?**

A) **Amazon S3 avec une politique de cycle de vie vers Glacier** ✅  
B) Amazon EFS avec sauvegardes automatiques  
C) Utiliser uniquement Amazon RDS avec snapshots  
D) Amazon DynamoDB avec capacité provisionnée

**Correction** : **Amazon S3** avec une politique de cycle de vie est idéal pour cette situation. Vous pouvez d'abord stocker les données dans S3 Standard, puis les déplacer automatiquement vers **Amazon Glacier** pour un archivage à long terme et à moindre coût.

---

#### **3. Votre équipe IT gère une grande quantité d'instances EC2. Vous vous demandez si certaines instances sont sous-utilisées et vous voulez réduire les coûts. Quel service AWS peut vous fournir des recommandations pour optimiser vos ressources ?**

A) AWS Budgets  
B) AWS Cost Explorer  
C) **AWS Trusted Advisor** ✅  
D) AWS Pricing Calculator

**Correction** : **AWS Trusted Advisor** fournit des recommandations sur la façon d'optimiser vos ressources, notamment en identifiant les instances EC2 sous-utilisées ou surprovisionnées pour réduire les coûts.

---

#### **4. Vous avez plusieurs comptes AWS pour différents départements de votre organisation. Vous souhaitez surveiller les coûts de chacun de ces comptes depuis une seule interface. Quelle solution permet de gérer cela efficacement ?**

A) **Consolidated Billing via AWS Organizations** ✅  
B) AWS Cost Explorer avec des filtres  
C) Configurer des alertes via AWS Budgets pour chaque compte  
D) Utiliser un seul compte avec plusieurs IAM users

**Correction** : **Consolidated Billing via AWS Organizations** permet de regrouper plusieurs comptes AWS sous une seule facture consolidée, tout en surveillant les coûts pour chaque compte de manière indépendante.

---

#### **5. Vous utilisez des instances On-Demand EC2 pour une application à court terme et vous souhaitez anticiper le coût de cette application avant de lancer de nouvelles instances. Quel service AWS vous aide à estimer les coûts potentiels ?**

A) AWS Trusted Advisor  
B) **AWS Pricing Calculator** ✅  
C) AWS Budgets  
D) AWS Cost Explorer

**Correction** : **AWS Pricing Calculator** est un outil conçu pour estimer les coûts de différentes configurations AWS, y compris les instances EC2 On-Demand, avant de les déployer.

---

#### **6. Vous avez un serveur de base de données Amazon RDS qui est inactif la nuit. Quel est le meilleur moyen de réduire les coûts pendant ces périodes d'inactivité ?**

A) Passer à une instance RDS On-Demand  
B) **Automatiser les arrêts et redémarrages d'instances RDS en fonction des heures d'inactivité** ✅  
C) Utiliser Amazon RDS avec instances Réservées  
D) Migrer vers DynamoDB pour éviter l'inactivité

**Correction** : Il est possible d'**automatiser les arrêts et redémarrages** des instances Amazon RDS pendant les périodes d'inactivité, ce qui permet de réduire les coûts en ne payant pas pour des instances inutilisées la nuit.

---

#### **7. Vous travaillez pour une entreprise qui utilise intensivement Amazon Redshift pour des analyses de données. Vous souhaitez réduire les coûts tout en maintenant les performances. Quelle stratégie recommandez-vous ?**

A) Augmenter le nombre de nœuds dans le cluster  
B) Utiliser des snapshots pour optimiser l'espace disque  
C) **Utiliser la compression automatique et la mise en pause des clusters inactifs** ✅  
D) Passer à une instance RDS pour les analyses de données

**Correction** : **La compression automatique** dans Amazon Redshift réduit l'espace disque utilisé, tandis que la **mise en pause des clusters inactifs** permet de réduire les coûts en arrêtant les clusters lorsque les ressources ne sont pas utilisées.

---

#### **8. Votre entreprise gère un environnement de développement qui fonctionne 24/7, mais qui n'est utilisé qu'en journée par les développeurs. Comment optimiser les coûts tout en gardant cet environnement disponible ?**

A) Utiliser des instances EC2 Spot  
B) Utiliser des instances EC2 On-Demand  
C) **Automatiser l'arrêt des instances pendant les périodes d'inactivité** ✅  
D) Passer à des instances EC2 Réservées

**Correction** : Automatiser l'arrêt des instances pendant les périodes d'inactivité (comme la nuit ou les week-ends) permet d'économiser sur les coûts, tout en assurant la disponibilité pendant les heures de travail.

---

#### **9. Vous avez stocké des données dans Amazon S3. La plupart de ces données ne sont plus utilisées régulièrement, mais vous devez quand même les conserver. Quelle option vous permettrait de réduire les coûts tout en conservant ces données pour le futur ?**

A) Migrer les données vers un volume Amazon EBS  
B) **Déplacer les données vers Amazon S3 Glacier Deep Archive** ✅  
C) Supprimer toutes les données non utilisées pour éviter les coûts  
D) Compresser les données et les stocker sur EC2

**Correction** : **Amazon S3 Glacier Deep Archive** est l'option de stockage la plus économique pour les données rarement accédées. Elle est idéale pour conserver des données à long terme tout en réduisant considérablement les coûts.

---

#### **10. Vous souhaitez surveiller les coûts en temps réel et recevoir des notifications si les dépenses dépassent un certain seuil pour éviter les mauvaises surprises en fin de mois. Quel service AWS permet de configurer ce type d'alertes ?**

A) AWS Cost Explorer  
B) **AWS Budgets** ✅  
C) AWS Trusted Advisor  
D) AWS Billing Dashboard

**Correction** : **AWS Budgets** permet de définir des alertes lorsque les coûts ou l'utilisation atteignent ou dépassent un certain seuil. C'est l'outil idéal pour surveiller les dépenses en temps réel.

---

### **Réponses Résumées :**

1. C) Utiliser EC2 Auto Scaling avec des instances Spot  
2. A) Amazon S3 avec une politique de cycle de vie vers Glacier  
3. C) AWS Trusted Advisor  
4. A) Consolidated Billing via AWS Organizations  
5. B) AWS Pricing Calculator  
6. B) Automatiser les arrêts et redémarrages d'instances RDS en fonction des heures d'inactivité  
7. C) Utiliser la compression automatique et la mise en pause des clusters inactifs  
8. C) Automatiser l'arrêt des instances pendant les périodes d'inactivité  
9. B) Déplacer les données vers Amazon S3 Glacier Deep Archive  
10. B) AWS Budgets

