## Introduction à Amazon SageMaker

Amazon SageMaker est un service de machine learning entièrement géré qui permet aux développeurs et aux scientifiques des données de créer, d'entraîner et de déployer des modèles de machine learning rapidement et en toute confiance. Il offre une interface utilisateur pour exécuter des workflows de machine learning, rendant les outils de SageMaker accessibles à travers plusieurs environnements de développement intégrés (IDE)[4].

## Principales Fonctionnalités

**Création et Entraînement de Modèles**

- **Blocs-notes Jupyter** : SageMaker propose des blocs-notes Jupyter intégrés pour explorer et visualiser les données, ce qui facilite le développement de modèles de machine learning[3].
- **Entraînement Distribué** : Il permet l'entraînement de modèles sur de grandes quantités de données en utilisant des algorithmes ML gérés, optimisés pour fonctionner efficacement dans un environnement distribué[4].

**Déploiement et Gestion**

- **Déploiement en Production** : Les modèles peuvent être déployés dans un environnement sécurisé et évolutif directement depuis la console SageMaker, ce qui simplifie le passage de l'entraînement au déploiement[4].
- **Environnements Personnalisables** : SageMaker supporte l'utilisation de vos propres algorithmes et frameworks, offrant ainsi des options d'entraînement flexibles qui s'adaptent à vos workflows spécifiques[4].

**Optimisation des Coûts**

- **Tarification à l'Usage** : Vous payez uniquement pour les ressources de calcul, de stockage et de traitement des données que vous utilisez, sans frais minimums ni engagements initiaux[1].
- **Entraînement Spot Géré** : Cette fonctionnalité permet de réduire les coûts d'entraînement en utilisant des instances Spot AWS, qui sont des capacités de calcul excédentaires proposées à un tarif réduit[1].

## Utilisation de SageMaker

**Pour les Débutants**

- **Configuration** : SageMaker offre des guides pour vous aider à configurer et à utiliser le service selon vos besoins[4].
- **Options No-Code/Low-Code** : Pour simplifier les workflows de machine learning, SageMaker propose des options no-code et low-code qui automatisent les tâches de machine learning et génèrent des notebooks pour chaque tâche automatisée[4].

**Exemples et Tutoriels**

- **Exemples de Notebooks** : SageMaker propose des notebooks d'exemples qui démontrent comment construire, entraîner et déployer des modèles de machine learning, comme l'utilisation de l'algorithme k-means pour le clustering d'images MNIST[3].

En résumé, Amazon SageMaker est un outil puissant pour les professionnels du machine learning, offrant une plateforme intégrée pour développer, entraîner et déployer des modèles tout en optimisant les coûts et en simplifiant les workflows.

# Annexe : 


```
        +--------------+
        |  Data Input  |
        +--------------+
              |
              v
        +--------------+
        | Data Process  |
        | (Preprocessing)|
        +--------------+
              |
              v
        +--------------+
        |   Model Train |
        +--------------+
              |
              v
        +--------------+
        | Model Evaluate|
        +--------------+
              |
              v
        +--------------+
        |  Model Deploy  |
        +--------------+
```

Ce diagramme illustre les étapes typiques d'une pipeline de machine learning dans SageMaker :

1. **Data Input** : Collecte et importation des données nécessaires pour l'entraînement du modèle.
2. **Data Process (Preprocessing)** : Prétraitement des données, incluant le nettoyage, la normalisation et la transformation des données pour les rendre aptes à l'entraînement.
3. **Model Train** : Entraînement du modèle de machine learning en utilisant les données prétraitées.
4. **Model Evaluate** : Évaluation du modèle pour vérifier ses performances et sa précision.
5. **Model Deploy** : Déploiement du modèle dans un environnement de production pour effectuer des prédictions en temps réel ou par batch.

Ce flux de travail est typique dans les projets de machine learning et peut être automatisé et géré à grande échelle avec Amazon SageMaker Pipelines[1][5].


# Table: 

Voici un tableau récapitulatif des services AWS mentionnés dans le diagramme :

| Service                  | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **AWS Cloud9 IDE**       | Environnement de développement intégré utilisé pour écrire et exécuter du code. |
| **VPC (Virtual Private Cloud)** | Réseau virtuel dédié pour isoler les ressources AWS.                         |
| **Public Subnet**        | Sous-réseau public pour héberger des ressources accessibles depuis Internet.   |
| **Data-source Bucket**   | Bucket S3 où les données sources sont stockées.                              |
| **AWS Glue Crawler**     | Explore les données et génère des métadonnées.                               |
| **AWS Glue Table Metadata** | Stocke les métadonnées des tables générées par AWS Glue.                     |
| **AWS Glue Data Store**  | Stockage des données géré par AWS Glue.                                      |
| **Query-results Bucket** | Bucket S3 pour stocker les résultats des requêtes.                           |
| **Athena**               | Service qui permet d'exécuter des requêtes SQL sur des données stockées dans S3. |
| **QuickSight**           | Outil de visualisation de données d'Amazon.                                  |
| **CapstoneGlueRole**     | Rôle IAM utilisé par AWS Glue pour accéder aux ressources nécessaires.       |
| **aws-quicksight-service-role-v0** | Rôle IAM qui permet à QuickSight d'accéder aux ressources AWS.                |

Ces services collaborent pour former une pipeline de données complète, allant de l'ingestion des données à leur analyse et visualisation.


Voici une table récapitulative des services AWS mentionnés dans les images :

| **Catégorie**                      | **Service**                             | **Description**                                                                 |
|------------------------------------|-----------------------------------------|---------------------------------------------------------------------------------|
| **Workflow Services**              | SageMaker                               | Service de machine learning pour créer, entraîner et déployer des modèles.      |
|                                    | Amazon EMR                              | Service pour traiter de grandes quantités de données avec Hadoop et Spark.      |
|                                    | AWS Batch                               | Exécute des travaux batch à n'importe quelle échelle.                           |
|                                    | Amazon EKS                              | Service pour exécuter des applications Kubernetes.                              |
|                                    | DLAMI                                   | Amazon Machine Images pour le deep learning.                                    |
|                                    | Deep Learning Containers                | Conteneurs optimisés pour le deep learning.                                     |
|                                    | AWS ParallelCluster                     | Gestion de clusters HPC (High-Performance Computing).                           |
|                                    | Amazon ECS                              | Service de conteneurisation pour exécuter des applications en conteneurs.       |
| **Frameworks**                     | TensorFlow, PyTorch, MXNet, Keras, Gluon, Horovod | Frameworks populaires pour le développement de modèles de machine learning.    |
| **Compute, Networking, Storage**   | EC2 P3, P4, G4, Inf1 instances          | Instances de calcul optimisées pour le machine learning et le deep learning.    |
|                                    | Elastic Inference                       | Ajoute de la capacité d'inférence à vos instances EC2.                          |
|                                    | Elastic Fabric Adapter                  | Réseau haute performance pour les applications HPC.                             |
|                                    | Amazon S3                               | Stockage d'objets scalable et sécurisé.                                         |
|                                    | Amazon EFS                              | Système de fichiers scalable et élastique pour le stockage partagé.             |
|                                    | Amazon EBS                              | Stockage de blocs pour les instances EC2.                                       |
|                                    | Amazon FSx                              | Systèmes de fichiers pour les applications HPC et Windows.                      |
|                                    | Outposts                                | Exécute des services AWS sur site pour une expérience hybride.                  |

Cette table résume les services utilisés pour le machine learning, le traitement de données, et l'infrastructure cloud sur AWS.

Citations:
[1] https://pplx-res.cloudinary.com/image/upload/v1724451149/user_uploads/rznieqbws/image.jpg
[2] https://pplx-res.cloudinary.com/image/upload/v1724451448/user_uploads/bulguntio/image.jpg


---
# Citations:
<hr/>

[1] https://aws.amazon.com/fr/sagemaker/faqs/

[2] https://www.youtube.com/watch?v=TZNDerXVjvM

[3] https://github.com/aws/amazon-sagemaker-examples/blob/main/README.md?plain=1

[4] https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html
