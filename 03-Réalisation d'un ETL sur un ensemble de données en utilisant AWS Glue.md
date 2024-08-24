# **Réalisation d'un ETL sur un Jeu de Données en Utilisant AWS Glue**

## **Aperçu du Lab et Objectifs**

Les problèmes liés au Big Data impliquent souvent un grand nombre de sources de données hétérogènes. En tant qu'analyste de données, il est possible que vous ne connaissiez pas le schéma de certaines sources de données. Cela correspond à l'aspect de la variété parmi les cinq V du Big Data (volume, variété, vélocité, véracité, et valeur). Dans ce lab, vous allez utiliser AWS Glue pour effectuer l'extraction, la transformation et le chargement (ETL) d'un jeu de données. Vous pouvez diriger AWS Glue vers une source de données, et il peut inférer un schéma basé sur les types de données qu'il découvre. AWS Glue construit ensuite un catalogue de données contenant les métadonnées des différentes sources de données.

AWS Glue est similaire à Amazon Athena dans le sens où les données que vous analysez restent dans la source de données. La principale différence est que vous pouvez créer un crawler avec AWS Glue pour découvrir le schéma, puis extraire les données du jeu de données. Vous pouvez également transformer le schéma, puis charger les données dans une base de données AWS Glue. Ensuite, vous pouvez analyser les données en utilisant des instructions SQL dans Athena.

Dans ce lab, vous apprendrez à utiliser AWS Glue pour importer un jeu de données depuis Amazon Simple Storage Service (Amazon S3). Vous allez ensuite extraire les données, transformer leur schéma, et charger le jeu de données dans une base de données AWS Glue pour une analyse ultérieure avec Athena.

Après avoir terminé ce lab, vous serez capable de :

1. Accéder à AWS Glue dans la console de gestion AWS et créer un crawler.
2. Créer une base de données AWS Glue avec des tables et un schéma en utilisant un crawler.
3. Interroger les données dans la base de données AWS Glue en utilisant Athena.
4. Créer et déployer un crawler AWS Glue en utilisant un modèle AWS CloudFormation.
5. Revoir une politique AWS Identity and Access Management (IAM) pour permettre aux utilisateurs d'exécuter un crawler AWS Glue et d'interroger une base de données AWS Glue dans Athena.
6. Confirmer qu'un utilisateur avec la politique IAM peut utiliser l'interface en ligne de commande AWS (AWS CLI) pour accéder à la base de données AWS Glue créée par le crawler.
7. Confirmer qu'un utilisateur peut exécuter le crawler AWS Glue lorsque les données sources changent.

---

## **Durée**

Ce lab nécessitera environ 90 minutes pour être complété.

---

## **Restrictions sur les Services AWS**

Dans cet environnement de lab, l'accès aux services AWS et aux actions de service peut être restreint à ceux nécessaires pour compléter les instructions du lab. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'effectuer des actions au-delà de celles décrites dans ce lab.

---

## **Scénario**

L'équipe de science des données vous a demandé de créer une série de preuves de concept (POC) pour utiliser les services AWS afin de répondre à de nombreux besoins en ingénierie et analyse des données de l'université. Mary, un membre de l'équipe de science des données, a vu ce qu'Athena peut faire pour créer des tables avec des schémas définis et en est impressionnée. Elle vous demande s'il est possible d'inférer automatiquement les colonnes et les types de données. La définition du schéma prend beaucoup de temps lorsqu'elle traite de grandes quantités de données variées. Vous souhaitez développer une POC pour utiliser AWS Glue, conçu pour des cas d'utilisation similaires.

Pour développer une POC, Mary suggère d'utiliser un jeu de données disponible publiquement, le **Global Historical Climatology Network Daily (GHCN-D)**, qui contient des résumés météorologiques quotidiens provenant de stations terrestres, remontant à 1763. Le jeu de données est disponible publiquement dans un bucket S3.

Mary explique que les paramètres les plus couramment enregistrés dans le jeu de données sont les températures quotidiennes, les précipitations et les chutes de neige. Ces paramètres sont utiles pour évaluer les risques de sécheresse, d'inondation, et de conditions météorologiques extrêmes. Les définitions de données utilisées dans ce lab sont disponibles sur la page du **NOAA Global Historical Climatology Network Daily (GHCN-D) Dataset**.

**Remarque :** Depuis octobre 2022, le jeu de données a été divisé en sous-ensembles, par année et par station. Tout au long de ce lab, vous utiliserez la version par année, qui se trouve à l'emplacement `s3://noaa-ghcn-pds/csv/by_year/`.

Lorsque vous démarrez le lab, l'environnement contiendra les ressources illustrées dans le diagramme suivant. Pour cet environnement de lab, la source de données originale est un bucket S3 situé dans un autre compte AWS.

---

## **Environnement du Lab**

L'environnement du lab est créé avec un modèle CloudFormation déployé lorsque vous lancez le lab. Le stack CloudFormation résultant crée deux buckets S3 intitulés `data-science-bucket` et `glue-1950-bucket`, une politique IAM intitulée `Policy-For-Data-Scientists`, et un rôle IAM intitulé `gluelab`.

---

## **Architecture du Lab**

1. Vous créerez et utiliserez un crawler AWS Glue nommé `weather` avec le rôle IAM `gluelab` pour accéder au jeu de données GHCN-D, qui se trouve dans un bucket S3 dans un autre compte AWS. Le crawler peuplera la base de données `weatherdata` dans le catalogue de données AWS Glue.
2. Vous utiliserez également la console Glue pour transformer la base de données en modifiant son schéma.
3. Lorsque le catalogue de données sera prêt, vous utiliserez Athena pour interroger la base de données et créer des tables. Les résultats des requêtes que vous exécuterez seront stockés dans le bucket S3 `data-science-bucket`.
4. Vous créerez une table de base de données AWS Glue en utilisant une autre requête qui n'inclut que les données depuis 1950, puis stockerez les résultats de la requête dans le bucket S3 `glue-1950-bucket`.
5. Vous créerez des vues et les utiliserez pour calculer la température moyenne de chaque année.
6. Vous utiliserez AWS CLI dans le terminal AWS Cloud9 pour créer un modèle CloudFormation. Le modèle créera le crawler en tant que `cfn-crawler-weather`. Les autres membres de l'équipe et d'autres départements universitaires pourront utiliser le modèle pour créer le crawler si nécessaire.
7. Vous examinerez la politique IAM `Policy-For-Data-Scientists` pour comprendre l'accès de l'équipe au workflow.
8. Vous testerez l'accès de Mary au crawler `cfn-crawler-weather` et l'exécuterez en utilisant ses informations d'identification.

---

## **Accès à la Console de Gestion AWS**

1. En haut de ces instructions, choisissez **Start Lab**.

   - La session du lab commence.
   - Un minuteur s'affiche en haut de la page et indique le temps restant dans la session.
   - **Astuce :** Pour rafraîchir la durée de la session à tout moment, choisissez **Start Lab** à nouveau avant que le minuteur n'atteigne 00:00.
   - Avant de continuer, attendez que l'icône circulaire à droite du lien **AWS** dans le coin supérieur gauche devienne verte.

2. Pour vous connecter à la console de gestion AWS, choisissez le lien **AWS** dans le coin supérieur gauche.

   - Un nouvel onglet du navigateur s'ouvre et se connecte à la console.
   - **Astuce :** Si un nouvel onglet ne s'ouvre pas, une bannière ou une icône apparaît généralement en haut de votre navigateur avec le message que votre navigateur empêche le site d'ouvrir des fenêtres pop-up. Choisissez la bannière ou l'icône, puis choisissez **Allow pop-ups**.

---

## **Tâche 1 : Utilisation d'un Crawler AWS Glue avec le Jeu de Données GHCN-D**

En tant qu'ingénieur ou analyste de données, vous ne connaissez peut-être pas toujours le schéma des données que vous devez analyser. AWS Glue est conçu pour cette situation. Vous pouvez diriger AWS Glue vers des données stockées sur AWS, et le service découvrira vos données. AWS Glue stockera ensuite les métadonnées associées (par exemple, la définition de la table et le schéma) dans le catalogue de données AWS Glue. Vous y parvenez en créant un crawler, qui inspecte la source de données et infère un schéma en fonction des données.

### **Étapes :**

1. **Configurer et créer le crawler AWS Glue :**

   - Dans la console de gestion AWS, dans la zone de recherche à côté de **Services**, recherchez et choisissez **AWS Glue** pour ouvrir la console AWS Glue.
   - Dans le panneau de navigation, sous **Bases de données**, choisissez **Tables**.
   - Choisissez **Ajouter des tables en utilisant un crawler**.
   - Pour **Nom**, entrez **Weather**.
   - Développez la section **Tags (facultatif)**.
   - Remarquez que c'est ici que vous pourriez ajouter des balises ou des configurations de sécurité supplémentaires. Conservez les paramètres par défaut.
   - Choisissez **Next** en bas de la

 page.
   - Choisissez **Ajouter une source de données** et configurez comme suit :
     - **Source de données** : Choisissez **S3**.
     - **Emplacement des données S3** : Choisissez **Dans un compte différent**.
     - **Chemin S3** : Entrez l'emplacement suivant du bucket S3 pour le jeu de données disponible publiquement : `s3://noaa-ghcn-pds/csv/by_year/`.
     - **Exécutions ultérieures du crawler** : Choisissez **Explorer tous les sous-dossiers**.
     - Choisissez **Ajouter une source de données S3**.
   - Choisissez **Next**.
   - Pour **Rôle IAM existant**, choisissez **gluelab**.
   - Ce rôle a été fourni dans l'environnement du lab pour vous. Pour référence, consultez le modèle CloudFormation du lab. Voici l'extrait YAML pour ce rôle :
     ```yaml
     GlueLab:
       Type: AWS::IAM::Role
       Properties:
         RoleName: "gluelab"
         Path: "/"
         AssumeRolePolicyDocument:
           Version: 2012-10-17
           Statement:
             - Effect: Allow
               Principal:
                 Service:
                   - glue.amazonaws.com
               Action:
                 - sts:AssumeRole
         ManagedPolicyArns:
           - arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole
           - arn:aws:iam::aws:policy/AmazonS3FullAccess
     ```
   - Choisissez **Next**.
   - Dans la section **Configuration de la sortie**, choisissez **Ajouter une base de données**.
     - Un nouvel onglet du navigateur s'ouvre.
     - Pour **Nom**, entrez **weatherdata**.
     - Choisissez **Créer une base de données**.
     - Retournez à l'onglet du navigateur qui est ouvert à la page **Définir la sortie et la planification** dans la console AWS Glue.
     - Pour **Base de données cible**, choisissez la base de données `weatherdata` que vous venez de créer.
     - **Astuce :** Pour rafraîchir la liste des bases de données disponibles, choisissez l'icône de rafraîchissement à droite de la liste déroulante.
   - Dans la section **Planification du crawler**, pour **Fréquence**, conservez le paramètre par défaut **À la demande**.
   - Choisissez **Next**.
   - Confirmez que la configuration de votre crawler est similaire à ce qui suit.
   - Cette image montre la configuration complète du crawler à l'étape 5 du processus de création du crawler, avant de créer réellement le crawler.
   - Choisissez **Créer un crawler**.
   - Pour effectuer les étapes d'extraction et de chargement du processus ETL, vous allez maintenant exécuter le crawler.
   - Vous pouvez créer des crawlers AWS Glue pour qu'ils s'exécutent à la demande ou selon un calendrier défini. Comme vous avez créé votre crawler pour s'exécuter à la demande, vous devez exécuter le crawler pour construire la base de données et générer les métadonnées.

2. **Exécuter le crawler :**

   - Sur la page **Crawlers**, sélectionnez le crawler **Weather** que vous venez de créer.
   - Choisissez **Exécuter**.
   - L'état du crawler passe à **Running**.
   - **Important :** Attendez que le statut passe à **Ready** avant de passer à l'étape suivante. Cela prendra environ 3 minutes.
   - AWS Glue crée une table pour stocker les métadonnées sur le jeu de données GHCN-D. Ensuite, vous allez inspecter les données qu'AWS Glue a capturées sur la source de données.

3. **Revoir les métadonnées créées par AWS Glue :**

   - Dans le panneau de navigation, choisissez **Bases de données**.
   - Choisissez le lien pour la base de données `weatherdata`.
   - Dans la section **Tables**, choisissez le lien `by_year`.
   - Passez en revue les métadonnées que le crawler `weather` a capturées, comme illustré dans la capture d'écran suivante. Le schéma liste les colonnes que le crawler a découvertes dans le jeu de données importé.

4. **Modifier le schéma :**

   - Dans le menu **Actions** dans le coin supérieur droit de la page, choisissez **Modifier le schéma**.
   - Modifiez les noms des colonnes selon le tableau suivant.
   - Pour changer un nom de colonne, sélectionnez la case à cocher pour l'élément que vous souhaitez modifier, puis choisissez **Modifier**.
   - Dans la fenêtre qui s'ouvre, changez la valeur pour le **Nom**, puis choisissez **Modifier**. Répétez ces étapes pour chaque nom de colonne.
     **Remarque :** AWS Glue prend en charge uniquement les noms de colonnes en minuscules.
     | Ancien nom  | Nouveau nom |
     |-------------|-------------|
     | id          | station     |
     | date        | date        |
     | element     | type        |
     | data_value  | observation |
     | m_flag      | mflag       |
     | q_flag      | qflag       |
     | s_flag      | sflag       |
     | obs_time    | time        |
   - Choisissez **Mettre à jour le schéma**.
   - Le schéma de la table ressemble maintenant à la capture d'écran suivante.

---

### **Résumé de la Tâche 1**

Dans cette tâche, vous avez utilisé la console pour créer un crawler dans AWS Glue. Vous avez dirigé le crawler vers des données stockées dans un bucket S3, et le crawler a découvert les données. Ensuite, le crawler a stocké les métadonnées associées (la définition de la table et le schéma) dans un catalogue de données. En utilisant un crawler dans AWS Glue, vous pouvez inspecter une source de données et en inférer le schéma.

L'équipe peut désormais utiliser des crawlers pour inspecter rapidement les sources de données et réduire les étapes manuelles nécessaires à la création de schémas de base de données à partir de sources de données dans Amazon S3. Vous partagez les résultats de cette POC avec Mary, et elle est satisfaite de la nouvelle fonctionnalité. Ensuite, elle souhaite pouvoir effectuer davantage d'analyses sur les données dans le catalogue de données AWS Glue.

---

## **Tâche 2 : Interrogation d'une Table en Utilisant Athena**

Maintenant que vous avez créé le catalogue de données, vous pouvez utiliser les métadonnées pour interroger les données plus en détail en utilisant Athena.

### **Étapes :**

1. **Configurer un bucket S3 pour stocker les résultats des requêtes Athena :**

   - Dans le panneau de navigation, sous **Bases de données**, choisissez **Tables**.
   - Choisissez le lien pour la table `by_year`.
   - Choisissez **Actions > Afficher les données**.
   - Lorsque la fenêtre contextuelle apparaît pour vous avertir que vous serez redirigé vers la console Athena, choisissez **Procéder**.
   - La console Athena s'ouvre. Remarquez le message d'erreur indiquant qu'un emplacement de sortie n'a pas été fourni. Avant de pouvoir exécuter une requête dans Athena, vous devez spécifier un bucket S3 pour conserver les résultats des requêtes.
   - Choisissez l'onglet **Paramètres**.
   - Choisissez **Gérer**.
   - À droite de **Emplacement du résultat de la requête**, choisissez **Parcourir S3**.
   - Choisissez le nom du bucket similaire à ce qui suit : `data-science-bucket-XXXXXX`.
   - **Important :** Ne choisissez pas le nom du bucket contenant `glue-1950-bucket`.
   - Sélectionnez **Choisir**.
   - Conservez les paramètres par défaut pour les autres options, et choisissez **Enregistrer**.

2. **Prévisualiser une table dans Athena :**

   - Choisissez l'onglet **Éditeur**.
   - Dans le panneau de données à gauche, remarquez que la source de données est **AwsDataCatalog**.
   - Pour **Base de données**, choisissez `weatherdata`.
   - Dans la section **Tables**, choisissez l'icône à trois points pour la table `by_year`, puis choisissez **Prévisualiser la table**.
   - **Astuce :** Pour voir les noms de colonnes et leurs types de données dans cette table, choisissez l'icône à gauche du nom de la table.
   - Les 10 premiers enregistrements de la table `weatherdata` s'affichent, similaires à la capture d'écran suivante.
   - Remarquez le temps d'exécution et la quantité de données scannées pour la requête. À mesure que vous développez des applications plus complexes, il est important de minimiser la consommation de ressources pour optimiser les coûts. Vous verrez des exemples de comment optimiser les coûts des requêtes Athena plus tard dans cette tâche.

3. **Créer une table pour les données après 1950 :**

   - D'abord, vous devez récupérer le nom du bucket créé pour vous permettre de stocker ces données.
   - Dans la zone de recherche à côté de **Services**, recherchez et choisissez **S3**.
   - Dans la liste des buckets, copiez le nom du bucket contenant `glue-1950-bucket` dans un éditeur de texte de votre choix.
   - Retournez à l'éditeur de requêtes Athena.
   - Copiez et collez la requête suivante dans

 un onglet de requête dans l'éditeur. Remplacez `<glue-1950-bucket>` par le nom du bucket que vous avez enregistré :
     ```sql
     CREATE table weatherdata.late20th
     WITH (
       format='PARQUET', external_location='s3://<glue-1950-bucket>/lab3'
     ) AS SELECT date, type, observation FROM by_year
     WHERE date/10000 between 1950 and 2015;
     ```
   - Choisissez **Exécuter**.
   - Après l'exécution de la requête, les valeurs de temps d'exécution et de données scannées sont similaires à ce qui suit :
     - **Temps en file d'attente :** 128 ms
     - **Temps d'exécution :** 1 min 8.324 sec
     - **Données scannées :** 98.44 GB
   - Pour prévisualiser les résultats, dans la section **Tables**, à droite de la table `late20th`, choisissez l'icône des trois points, puis choisissez **Prévisualiser la table**.
   - Les résultats sont similaires à la capture d'écran suivante.

4. **Exécuter une requête sur la nouvelle table :**

   - Tout d'abord, créez une vue qui inclut uniquement la valeur maximale de lecture de température, ou **TMAX**.
   - Exécutez la requête suivante dans un nouvel onglet de requête :
     ```sql
     CREATE VIEW TMAX AS
     SELECT date, observation, type
     FROM late20th
     WHERE type = 'TMAX';
     ```
   - Pour prévisualiser les résultats, dans la section **Vues**, à droite de la vue `tmax`, choisissez l'icône des trois points, puis choisissez **Prévisualiser la vue**.
   - Les résultats sont similaires à la capture d'écran suivante.

5. **Exécuter une requête pour calculer la température maximale moyenne pour chaque année :**

   - Exécutez la requête suivante dans un nouvel onglet de requête :
     ```sql
     SELECT date/10000 as Year, avg(observation)/10 as Max
     FROM tmax
     GROUP BY date/10000 ORDER BY date/10000;
     ```
   - Le but de cette requête est de calculer la température maximale moyenne pour chaque année dans le jeu de données.
   - Après l'exécution de la requête, les valeurs de temps d'exécution et de données scannées sont similaires à ce qui suit :
     - **Temps en file d'attente :** 0.211 sec
     - **Temps d'exécution :** 25.109 sec
     - **Données scannées :** 2.45 GB
   - Les résultats affichent la température maximale moyenne pour chaque année de 1950 à 2015. La capture d'écran suivante montre un exemple.

---

### **Résumé de la Tâche 2**

Dans cette tâche, vous avez appris à utiliser Athena pour interroger des tables dans une base de données créée par un crawler AWS Glue. Vous avez créé une table pour toutes les données après 1950 à partir du jeu de données original. Vous avez utilisé le format Apache Parquet pour optimiser vos requêtes Athena, ce qui a réduit le temps nécessaire pour compléter chaque requête, entraînant une réduction des coûts. Après avoir isolé ces données, vous avez créé une vue qui calculait la température maximale moyenne pour chaque année.

AWS Glue s'intègre avec les jeux de données originaux stockés dans Amazon S3. AWS Glue peut créer un crawler pour ingérer le jeu de données original dans une base de données et inférer le schéma approprié. Ensuite, vous pouvez rapidement passer à Athena pour développer des requêtes et mieux comprendre vos données. Cette intégration réduit le temps nécessaire pour tirer des insights de vos données et appliquer ces insights pour prendre de meilleures décisions.

---

## **Tâche 3 : Création d'un Modèle CloudFormation pour un Crawler AWS Glue**

Dans la tâche 1, vous avez utilisé la console pour créer un crawler AWS Glue pour inspecter la source de données et inférer un schéma. Cependant, l'équipe travaille avec de nombreux jeux de données dans différents comptes AWS, y compris pour le développement, les tests et la production. Par conséquent, il serait utile de réutiliser les crawlers à travers ces environnements, surtout lorsque de nouvelles données sont ajoutées aux jeux de données. Il serait également utile d'utiliser l'AWS CLI pour exécuter le crawler.

Si votre crawler s'exécute plus d'une fois, peut-être selon un calendrier, il recherche de nouveaux fichiers ou tables ou des fichiers ou tables modifiés dans votre magasin de données. La sortie du crawler inclut les nouvelles tables et partitions trouvées depuis la dernière exécution.

### **Étapes :**

1. **Trouver le Numéro de Ressource Amazon (ARN) pour le rôle IAM `gluelab` :**

   - Dans la zone de recherche à côté de **Services**, recherchez et choisissez **IAM** pour ouvrir la console IAM.
   - Dans le panneau de navigation, choisissez **Rôles**.
   - Choisissez le lien pour le rôle `gluelab`.
   - **Astuce :** Vous pouvez rechercher le rôle si nécessaire.
   - L'ARN est affiché sur la page dans la section **Résumé**.
   - Copiez l'ARN dans un éditeur de texte pour l'utiliser à l'étape suivante.

2. **Naviguer vers l'environnement intégré de développement (IDE) AWS Cloud9 :**

   - Dans la zone de recherche à côté de **Services**, recherchez et choisissez **Cloud9** pour ouvrir la console AWS Cloud9.
   - Les environnements AWS Cloud9 sont listés.
   - Pour l'environnement nommé **Cloud9 Instance**, choisissez **Ouvrir l'IDE**.
   - Un nouvel onglet du navigateur s'ouvre et affiche l'IDE AWS Cloud9.

3. **Créer un nouveau modèle CloudFormation :**

   - Dans l'IDE AWS Cloud9, choisissez **Fichier > Nouveau fichier**.
   - Enregistrez le fichier vide sous le nom **gluecrawler.cf.yml** mais laissez-le ouvert.
   - Copiez et collez le code suivant dans le fichier :
     ```yaml
     AWSTemplateFormatVersion: '2010-09-09'
     Parameters:
       CFNCrawlerName:
         Type: String
         Default: cfn-crawler-weather
       CFNDatabaseName:
         Type: String
         Default: cfn-database-weather
       CFNTablePrefixName:
         Type: String
         Default: cfn_sample_1-weather
     Resources:
       CFNDatabaseWeather:
         Type: AWS::Glue::Database
         Properties:
           CatalogId: !Ref AWS::AccountId
           DatabaseInput:
             Name: !Ref CFNDatabaseName
             Description: "AWS Glue container to hold metadata tables for the weather crawler"
       CFNCrawlerWeather:
         Type: AWS::Glue::Crawler
         Properties:
           Name: !Ref CFNCrawlerName
           Role: <GLUELAB-ROLE-ARN>
           Description: AWS Glue crawler to crawl weather data
           DatabaseName: !Ref CFNDatabaseName
           Targets:
             S3Targets:
               - Path: "s3://noaa-ghcn-pds/csv/by_year/"
           TablePrefix: !Ref CFNTablePrefixName
           SchemaChangePolicy:
             UpdateBehavior: "UPDATE_IN_DATABASE"
             DeleteBehavior: "LOG"
           Configuration: "{\"Version\":1.0,\"CrawlerOutput\":{\"Partitions\":{\"AddOrUpdateBehavior\":\"InheritFromTable\"},\"Tables\":{\"AddOrUpdateBehavior\":\"MergeNewColumns\"}}}"
     ```
   - **Remarque :** Ce bloc de code utilise l'exemple de modèle AWS CloudFormation pour un Crawler AWS Glue pour Amazon S3 provenant de la documentation AWS CloudFormation pour AWS Glue dans le guide du développeur AWS Glue.
   - Dans le fichier, remplacez `<GLUELAB-ROLE-ARN>` par l'ARN du rôle IAM `gluelab`. Recherchez la ligne commençant par **Role**, qui se trouve autour de la ligne 27.
   - Enregistrez les modifications dans le fichier modèle.
   - Examinez le code pour voir ce qui est créé. Ce modèle CloudFormation fait ce qui suit :
     - Définit un nom de ressource personnalisé pour le crawler AWS Glue.
     - Définit un nom de ressource personnalisé pour la base de données AWS Glue.
     - Définit un nom de ressource personnalisé pour la première table dans la base de données AWS Glue.
     - Crée un crawler pour explorer les données météorologiques dans un bucket S3 public.
     - Définit le rôle IAM (`gluelab`) que le crawler utilisera pour créer la base de données AWS Glue associée. Il s'agit du même rôle que vous avez utilisé pour créer le crawler manuellement. Notez que ce rôle IAM a été créé pour vous dans l'environnement du lab. Le rôle dispose de toutes les autorisations nécessaires pour créer et exécuter le crawler et créer la base de données.

4. **Valider le modèle CloudFormation :**

   - Exécutez la commande suivante dans le terminal AWS Cloud9 :
     ```bash
     aws cloudformation validate-template --template-body file://gluecrawler.cf.yml
     ```
   - **Remarque :** Si vous recevez une erreur indiquant que YAML n'est pas bien formé, vérifiez la valeur pour le nom du rôle `gluelab`. Vérifiez également les tabulations et l'espacement pour chaque ligne. Les documents YAML nécessitent un espacement exact

, et le parseur rencontrera des erreurs si l'espacement ne correspond pas.
   - Si le modèle est validé, la sortie suivante s'affiche :
     ```json
     {
       "Parameters": [
         {
           "ParameterKey": "CFNCrawlerName",
           "DefaultValue": "cfn-crawler-weather",
           "NoEcho": false
         },
         {
           "ParameterKey": "CFNTablePrefixName",
           "DefaultValue": "cfn_sample_1-weather",
           "NoEcho": false
         },
         {
           "ParameterKey": "CFNDatabaseName",
           "DefaultValue": "cfn-database-weather",
           "NoEcho": false
         }
       ]
     }
     ```
   - **Important :** Ne passez pas à l'étape suivante avant que le modèle soit validé.

5. **Créer la pile CloudFormation :**

   - Pour créer la pile CloudFormation, exécutez la commande suivante :
     ```bash
     aws cloudformation create-stack --stack-name gluecrawler --template-body file://gluecrawler.cf.yml --capabilities CAPABILITY_NAMED_IAM
     ```
   - **Remarque :** La commande inclut le paramètre `--capabilities` avec la capacité `CAPABILITY_NAMED_IAM`. Cela est dû au fait que vous créez les ressources suivantes avec des noms personnalisés, ce qui affecte les permissions :
     - Un crawler AWS Glue nommé `cfn-crawler-weather`.
     - Une base de données AWS Glue nommée `cfn-database-weather`.
     - Une table nommée `cfn_sample_1-weather` au sein de la base de données AWS Glue.

6. **Vérifier la création de la base de données AWS Glue dans la pile :**

   - Pour vérifier que la base de données AWS Glue a été créée dans la pile, exécutez la commande suivante :
     ```bash
     aws glue get-databases
     ```
   - La sortie est similaire à ce qui suit :
     ```json
     {
       "DatabaseList": [
         {
           "Name": "cfn-database-weather",
           "Description": "AWS Glue container to hold metadata tables for the weather crawler",
           "CreateTime": 1649267047.0,
           "CatalogId": "034140262343"
         },
         {
           "Name": "weatherdata",
           "CreateTime": 1649263434.0,
           "CatalogId": "034140262343"
         }
       ]
     }
     ```

7. **Vérifier que le crawler a été créé dans la pile :**

   - Pour vérifier que le crawler a été créé, exécutez la commande suivante :
     ```bash
     aws glue list-crawlers
     ```
   - La sortie est similaire à ce qui suit :
     ```json
     {
       "CrawlerNames": [
         "Weather",
         "cfn-crawler-weather"
       ]
     }
     ```

8. **Récupérer les détails du crawler :**

   - Pour récupérer les détails du crawler, exécutez la commande suivante :
     ```bash
     aws glue get-crawler --name cfn-crawler-weather
     ```
   - La sortie est similaire à ce qui suit :
     ```json
     {
       "Crawler": {
         "Name": "cfn-crawler-weather",
         "Role": "gluelab",
         "Targets": {
           "S3Targets": [
             {
               "Path": "s3://noaa-ghcn-pds/csv/by_year/"
             }
           ]
         },
         "DatabaseName": "cfn-database-weather",
         "Description": "AWS Glue crawler to crawl weather data",
         "SchemaChangePolicy": {
           "UpdateBehavior": "UPDATE_IN_DATABASE",
           "DeleteBehavior": "LOG"
         },
         "State": "READY"
       }
     }
     ```

---

### **Résumé de la Tâche 3**

Dans cette tâche, vous avez appris à intégrer un crawler AWS Glue dans un modèle CloudFormation. Vous avez également appris à utiliser l'AWS CLI dans le terminal AWS Cloud9 pour valider et déployer le modèle afin de créer le crawler. Avec le modèle, vous pouvez réutiliser le crawler dans d'autres comptes AWS. Ensuite, vous avez appris à confirmer que les ressources ont été créées avec le crawler (la base de données AWS Glue et les tables associées).

---

## **Tâche 4 : Revue de la Politique IAM pour l'Accès à Athena et AWS Glue**

Maintenant que vous avez créé le crawler en utilisant CloudFormation, examinez la politique IAM pour le crawler afin de vous assurer que d'autres utilisateurs peuvent l'utiliser en production.

**Remarque :** La politique IAM pour le crawler a été créée pour vous ; vous n'avez pas la capacité de créer des politiques IAM dans l'environnement du lab.

### **Étapes :**

1. **Revoir la politique `Policy-For-Data-Scientists` dans IAM :**

   - Dans la zone de recherche à droite de **Services**, recherchez et choisissez **IAM** pour ouvrir la console IAM.
   - Dans le panneau de navigation, choisissez **Utilisateurs**.
   - Notez que `mary` est l'un des utilisateurs IAM répertoriés. Cet utilisateur fait partie du groupe IAM `DataScienceGroup`.
   - Choisissez le lien pour le groupe IAM `DataScienceGroup`.
   - Sur la page de détails du groupe `DataScienceGroup`, choisissez l'onglet **Permissions**.
   - Dans la liste des politiques attachées au groupe, choisissez le lien pour la politique `Policy-For-Data-Scientists`.
   - La page de détails de la politique `Policy-For-Data-Scientists` s'ouvre. Passez en revue les permissions associées à cette politique. Remarquez que les permissions fournissent un accès limité uniquement pour les services Athena, AWS Glue, et Amazon S3.
   - **Astuce :** Pour examiner de plus près les détails de la politique IAM, choisissez **{} JSON**. Dans le fichier de politique JSON, vous pouvez voir les actions autorisées et refusées, y compris les ressources sur lesquelles les utilisateurs peuvent effectuer des actions.

---

### **Résumé de la Tâche 4**

Dans cette tâche, vous avez examiné la politique IAM pour le groupe `DataScienceGroup`. La politique contient des permissions pour un accès limité à Amazon S3, AWS Glue, et Athena. La politique pourrait être utilisée comme une politique exemple pour les utilisateurs qui ont l'intention de réutiliser les crawlers construits par l'équipe des opérations. Comme avec tous les services dans AWS, les utilisateurs IAM doivent avoir les permissions appropriées appliquées pour pouvoir effectuer des actions.

---

## **Tâche 5 : Confirmation que Mary Peut Accéder et Utiliser le Crawler AWS Glue**

Maintenant que vous avez examiné la politique IAM, vous allez l'utiliser pour tester l'accès d'un autre utilisateur au crawler AWS Glue. Vous testerez également la capacité de l'utilisateur à utiliser le crawler pour extraire, transformer, et charger des données d'un jeu de données stocké dans Amazon S3 dans une base de données AWS Glue.

### **Étapes :**

1. **Récupérer les informations d'identification de l'utilisateur IAM `mary`, et les stocker comme variables bash :**

   - Dans la zone de recherche à côté de **Services**, recherchez et choisissez **CloudFormation**.
   - Dans le panneau de navigation, choisissez **Stacks**.
   - Choisissez le lien pour le stack qui a créé l'environnement du lab. Le nom du stack inclut une chaîne de lettres et de chiffres aléatoires, et le stack devrait avoir la plus ancienne date de création.
   - Sur la page de détails du stack, choisissez l'onglet **Outputs**.
   - **Remarque :** Lorsque vous créez un modèle CloudFormation, vous pouvez choisir de sortir des informations sur les ressources que le modèle créera. Le modèle CloudFormation qui a créé les ressources dans votre environnement du lab a sorti la clé d'accès et la clé d'accès secret pour l'utilisateur `mary`.
   - Copiez la valeur de `MarysAccessKey` dans votre presse-papiers.
   - Retournez au terminal AWS Cloud9.
   - Pour créer une variable pour la clé d'accès, exécutez la commande suivante. Remplacez `<ACCESS-KEY>` par la valeur de votre presse-papiers :
     ```bash
     AK=<ACCESS-KEY>
     ```
   - Retournez à la console CloudFormation, et copiez la valeur de `MarysSecretAccessKey` dans votre presse-papiers.
   - Retournez au terminal AWS Cloud9.
   - Pour créer une variable pour la clé d'accès secret, exécutez la commande suivante. Remplacez `<SECRET-ACCESS-KEY>` par la valeur de votre presse-papiers :
     ```bash
     SAK=<SECRET-ACCESS-KEY>
     ```

2. **Tester l'accès de `mary` au crawler AWS Glue :**

   - Pour tester si l'utilisateur `mary` peut exécuter la commande `list-crawlers`, exécutez la commande suivante :
     ```bash
     AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws glue list-crawlers
     ```
   - La sortie est similaire à ce qui suit et ressemble à la sortie affichée après avoir exécuté la commande plus tôt :
     ```json
     {
       "CrawlerNames": [
         "Weather",
         "cfn-crawler-weather"
       ]
     }
     ```

3. **Tester si `mary` peut exécuter la commande `get-crawler` :**

   - Pour tester si l'utilisateur `mary` peut exéc

uter la commande `get-crawler`, exécutez la commande suivante :
     ```bash
     AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws glue get-crawler --name cfn-crawler-weather
     ```
   - La sortie est similaire à ce qui suit et ressemble à la sortie affichée après avoir exécuté la commande plus tôt. Remarquez que l'état du crawler est **READY**, mais aucune information de statut n'est affichée. C'est parce que le crawler n'a pas encore été exécuté.
     ```json
     {
       "Crawler": {
         "Name": "cfn-crawler-weather",
         "Role": "gluelab",
         "Targets": {
           "S3Targets": [
             {
               "Path": "s3://noaa-ghcn-pds/csv/by_year/"
             }
           ]
         },
         "DatabaseName": "cfn-database-weather",
         "Description": "AWS Glue crawler to crawl weather data",
         "State": "READY"
       }
     }
     ```

4. **Tester que `mary` peut exécuter le crawler :**

   - Exécutez la commande suivante :
     ```bash
     AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws glue start-crawler --name cfn-crawler-weather
     ```
   - Si le crawler s'exécute avec succès, le terminal ne montre aucune sortie.
   - Pour observer le crawler en cours d'exécution et ajoutant des données à la table, naviguez vers la console AWS Glue.
   - Dans le panneau de navigation, choisissez **Crawlers**.
   - Ici, vous pouvez voir les informations de statut pour le crawler, comme illustré dans la capture d'écran suivante.
   - Lorsque le statut passe à **Ready**, le crawler a terminé son exécution. Cela peut prendre quelques minutes.
   - Retournez au terminal AWS Cloud9.
   - Pour confirmer que le crawler a terminé son exécution, exécutez la commande suivante :
     ```bash
     AWS_ACCESS_KEY_ID=$AK AWS_SECRET_ACCESS_KEY=$SAK aws glue get-crawler --name cfn-crawler-weather
     ```
   - La sortie est similaire à ce qui suit :
     ```json
     {
       "Crawler": {
         "Name": "cfn-crawler-weather",
         "Role": "gluelab",
         "State": "READY",
         "LastCrawl": {
           "Status": "SUCCEEDED"
         }
       }
     }
     ```
   - Remarquez que la section `LastCrawl` est incluse, et que le statut dans cette section est `SUCCEEDED`. Cela signifie que `mary` a pu exécuter le crawler avec succès.

---

### **Résumé de la Tâche 5**

Ce résultat confirme que `mary` a accès au crawler AWS Glue que vous avez créé et déployé avec CloudFormation. Cela est dû au fait que les permissions dans la politique IAM lui permettent de lister, obtenir, et récupérer les métadonnées pour le crawler. Les autres permissions associées à la politique incluent les éléments suivants :

- Pour AWS Glue : Lister, lire, et marquer les ressources. Exécuter un crawler déployé avec CloudFormation, mais ne pas créer ou supprimer des ressources ou gérer des ressources.
- Pour Athena : Lister, lire, et marquer les ressources, mais ne pas créer ou supprimer des ressources spécifiques. (Par exemple, cette politique ne fournit pas les permissions pour créer ou supprimer une requête nommée ou un catalogue de données, que votre utilisateur a les permissions de faire.)
- Pour Amazon S3 : Accéder aux buckets, lister le contenu des buckets, et lire les objets, mais ne pas créer un bucket et limiter l'accès à des buckets spécifiques, comme `DataScienceBucket` que nous avons créé pour vous.

---

## **Félicitations !**

Vous avez appris à créer un crawler AWS Glue manuellement et en utilisant CloudFormation afin de pouvoir le déployer pour des utilisateurs avec une politique IAM sécurisée. Comme le crawler est dans un modèle CloudFormation, vous pouvez réutiliser le modèle pour créer et déployer le crawler dans n'importe quel compte AWS et changer les paramètres selon vos besoins.

---

### **Mise à Jour de l'Équipe**

L'équipe est heureuse de ce que vous avez appris et démontré en utilisant Athena, AWS Glue, et CloudFormation. Maintenant que vous avez partagé ce que vous avez appris, l'équipe pourra simplifier ses charges de travail et utiliser AWS de manière à suivre les meilleures pratiques.

---

## **Soumettre Votre Travail**

1. Pour enregistrer votre progression, choisissez **Submit** en haut de ces instructions.
2. Lorsqu'on vous le demande, choisissez **Oui**.
3. Après quelques minutes, le panneau de notes apparaît et vous montre combien de points vous avez obtenus pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez **Grades** en haut de ces instructions.
   - **Important :** Certains contrôles effectués par le processus de soumission dans ce lab ne vous donneront du crédit que s'il s'est écoulé au moins 5 minutes depuis que vous avez effectué l'action. Si vous ne recevez pas de crédit la première fois que vous soumettez, vous devrez peut-être attendre quelques minutes et soumettre à nouveau pour recevoir le crédit pour ces éléments.
   - **Astuce :** Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez **Submit** à nouveau. Votre dernière soumission est enregistrée pour ce lab.
4. Pour trouver des commentaires détaillés sur votre travail, choisissez **Rapport de soumission**.

---

**Lab terminé**

**Félicitations !** Vous avez terminé le lab.

- En haut de cette page, choisissez **End Lab**, puis choisissez **Oui** pour confirmer que vous souhaitez terminer le lab.
- Un panneau de message indique que le lab se termine.
- Pour fermer le panneau, choisissez **Fermer** dans le coin supérieur droit.

---

**Ressources supplémentaires**

Pour plus d'informations sur les services et concepts abordés dans ce lab, consultez les ressources suivantes :

- Getting Started with AWS Glue
- Scheduling an AWS Glue Crawler
- Apache Parquet
- How Crawlers Work
- CreateStack Request in CloudFormation
