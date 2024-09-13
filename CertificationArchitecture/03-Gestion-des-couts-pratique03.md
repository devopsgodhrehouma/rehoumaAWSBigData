### **Quiz : Gestion des Coûts AWS (Niveau Avancé)**

---

#### **1. Vous gérez une infrastructure complexe avec plusieurs services AWS dans différents comptes de votre organisation. Vous avez remarqué une augmentation des coûts des instances EC2 dans plusieurs régions. Quelle stratégie permettrait d'analyser et d'optimiser les coûts dans ce contexte multi-comptes et multi-régions ?**

A) Utiliser AWS Trusted Advisor pour analyser chaque région individuellement  
B) **Mettre en place AWS Organizations avec des étiquettes de coût (cost allocation tags) et utiliser AWS Cost Explorer pour visualiser les coûts consolidés** ✅  
C) Configurer des snapshots automatiques pour toutes les instances EC2  
D) Créer des budgets distincts pour chaque région et recevoir des alertes par AWS Budgets

**Correction** : L'utilisation des **cost allocation tags** dans **AWS Organizations**, combinée avec **AWS Cost Explorer**, permet de visualiser et d'analyser les coûts sur plusieurs comptes et régions, offrant une vue consolidée pour mieux optimiser les ressources.

---

#### **2. Votre entreprise utilise AWS Lambda pour des microservices et observe une augmentation inattendue des coûts associés aux fonctions Lambda. Quelle approche vous permettrait de réduire ces coûts sans compromettre la performance ?**

A) Augmenter la mémoire allouée aux fonctions pour réduire le temps d'exécution  
B) **Optimiser la taille des paquets et réduire la durée d'exécution en ajustant la logique des fonctions** ✅  
C) Migrer les fonctions vers des instances EC2 Spot  
D) Passer à une instance EC2 On-Demand pour exécuter les fonctions manuellement

**Correction** : **Optimiser la taille des paquets et la logique des fonctions** permet de réduire la durée d'exécution, ce qui diminue directement les coûts liés à l'utilisation d'AWS Lambda, sans avoir à provisionner de capacité supplémentaire.

---

#### **3. Vous gérez une application qui utilise Amazon RDS avec des instances de bases de données constamment actives. Vous remarquez que la charge de travail est plus faible durant la nuit. Quelle approche permettrait d'optimiser les coûts de cette base de données ?**

A) Utiliser des snapshots fréquents et restaurer les instances RDS à la demande  
B) Passer à Amazon Aurora Serverless pour toute la journée  
C) **Automatiser l'arrêt et le redémarrage des instances RDS pendant les périodes d'inactivité** ✅  
D) Migrer la base de données vers DynamoDB pour des économies immédiates

**Correction** : **Automatiser l'arrêt des instances RDS** pendant les périodes de faible charge est la solution la plus efficace pour réduire les coûts lorsque les bases de données ne sont pas sollicitées.

---

#### **4. Votre entreprise stocke de grandes quantités de données dans Amazon S3 et observe des coûts croissants liés aux requêtes GET. Vous souhaitez réduire ces coûts tout en maintenant l'accessibilité des données. Quelle stratégie est la plus efficace ?**

A) Déplacer toutes les données vers S3 Glacier  
B) **Mettre en cache les objets fréquemment accédés avec Amazon CloudFront** ✅  
C) Activer S3 Intelligent-Tiering pour toutes les données  
D) Activer une stratégie de cycle de vie pour déplacer les objets rarement utilisés vers One Zone-IA

**Correction** : **Mettre en cache les objets fréquemment accédés** avec **Amazon CloudFront** réduit les coûts associés aux requêtes GET en diminuant le nombre d'appels directs à S3, tout en améliorant la performance d'accès.

---

#### **5. Votre entreprise a adopté une approche multi-cloud et utilise AWS avec d'autres fournisseurs de cloud. Vous souhaitez centraliser la gestion des coûts de plusieurs fournisseurs, y compris AWS. Quel outil AWS vous permettrait d'atteindre cet objectif ?**

A) AWS Trusted Advisor  
B) AWS Cost Explorer  
C) **AWS Cost and Usage Report (CUR) avec des intégrations externes pour unifier les données de coûts** ✅  
D) AWS CloudFormation

**Correction** : Le **AWS Cost and Usage Report (CUR)** permet de générer des rapports détaillés et d’intégrer des données avec des outils externes pour unifier la gestion des coûts, même dans un contexte multi-cloud.

---

#### **6. Votre entreprise exécute des charges de travail de calcul intensif qui varient de manière imprévisible. Quelle combinaison de services EC2 vous permettrait d’optimiser les coûts tout en répondant à ces besoins variables ?**

A) Utiliser uniquement des instances EC2 On-Demand  
B) **Utiliser une combinaison d’instances On-Demand, Spot et Réservées avec EC2 Auto Scaling** ✅  
C) Utiliser des instances On-Demand et réserver de la capacité avec AWS Outposts  
D) Configurer des instances EC2 On-Demand avec des snapshots automatiques

**Correction** : La meilleure solution consiste à utiliser une **combinaison d’instances On-Demand, Spot et Réservées** pour optimiser les coûts selon les besoins de charge variable, tout en automatisant l'ajustement de la capacité avec **EC2 Auto Scaling**.

---

#### **7. Vous souhaitez analyser vos coûts AWS actuels et prévoir les dépenses futures pour mieux planifier votre budget annuel. Quel service AWS vous permet d'obtenir cette vue prédictive ?**

A) AWS Billing Dashboard  
B) **AWS Cost Explorer avec l'outil de prévision des coûts** ✅  
C) AWS Budgets  
D) AWS CloudTrail

**Correction** : **AWS Cost Explorer** propose un outil de prévision des coûts basé sur l’historique d’utilisation et de dépenses, vous permettant de mieux planifier et anticiper vos coûts futurs.

---

#### **8. Votre entreprise utilise Amazon Redshift pour des analyses de données massives. Vous remarquez que certaines tables sont rarement consultées mais occupent beaucoup d'espace disque. Quelle stratégie peut réduire les coûts sans affecter les performances des requêtes ?**

A) Supprimer les tables rarement utilisées pour libérer de l’espace  
B) **Activer la compression des données et archiver les tables rarement consultées sur S3** ✅  
C) Migrer toutes les données vers Amazon S3 Glacier  
D) Augmenter le nombre de nœuds dans le cluster Redshift

**Correction** : **Activer la compression des données** dans Redshift permet d’économiser de l'espace disque, tandis que **l'archivage sur S3** pour les tables rarement consultées réduit les coûts de stockage tout en maintenant l'accessibilité via des requêtes externes.

---

#### **9. Votre équipe de développement déploie plusieurs environnements de test sur EC2. Ces environnements sont souvent laissés actifs même lorsqu'ils ne sont plus nécessaires. Quelle stratégie de gestion des coûts pouvez-vous mettre en place pour éviter les coûts inutiles ?**

A) Utiliser uniquement des instances On-Demand pour les environnements de test  
B) **Configurer des politiques de cycle de vie et d’arrêt automatique des instances inactives** ✅  
C) Passer à des instances Spot pour les environnements de test  
D) Utiliser des instances réservées pour les environnements de test

**Correction** : Configurer des **politiques de cycle de vie et d’arrêt automatique** des instances inactives permet de s’assurer que les instances inutilisées sont arrêtées, évitant ainsi les coûts inutiles.

---

#### **10. Votre entreprise envisage d'utiliser des volumes Amazon EBS pour une application de base de données à haute performance. Les volumes EBS utilisés sont volumineux et sous-utilisés. Quelle solution vous permettrait d'optimiser les coûts tout en maintenant les performances ?**

A) Migrer vers des volumes EBS General Purpose (gp2)  
B) **Utiliser des volumes EBS provisionnés avec IOPS (io1/io2) et ajuster le provisionnement en fonction de la demande** ✅  
C) Utiliser des snapshots pour réduire la taille des volumes  
D) Passer à des instances EC2 avec stockage d'instance

**Correction** : **Utiliser des volumes EBS provisionnés avec IOPS (io1/io2)** permet d'ajuster la capacité en fonction des besoins de performance, optimisant ainsi les coûts tout en maintenant des performances élevées pour les bases de données.

---

### **Réponses Résumées :**

1. B) Mettre en place AWS Organizations avec des étiquettes de coût et utiliser AWS Cost Explorer pour visualiser les coûts consolidés  
2. B) Optimiser la taille des paquets et réduire la durée d'exécution en ajustant la logique des fonctions  
3. C) Automatiser l'arrêt et le redémarrage des instances RDS pendant les périodes d'inactivité  
4. B) Mettre en cache les objets fréquemment accédés avec Amazon CloudFront  
5. C) AWS Cost and Usage Report (CUR) avec des intégrations externes pour unifier les données de coûts  
6. B) Utiliser une combinaison d’instances On-Demand, Spot et Réservées avec EC2 Auto Scaling  
7. B) AWS Cost Explorer avec l'outil de prévision des coûts  
8. B) Activer la compression des données et archiver les tables rarement consultées sur S3  
9. B) Configurer des politiques de cycle de vie et d’arrêt automatique des instances inactives  
10. B) Utiliser des volumes EBS provisionnés avec IOPS (io1/io2) et ajuster le provisionnement en fonction de la demande



