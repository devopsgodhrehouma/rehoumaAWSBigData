# Projet de Clôture en Ingénierie des Données Académie

#### Vue d'ensemble et objectifs

Tout au long de ce cours, vous avez réalisé des laboratoires pratiques où vous avez utilisé les fonctionnalités de différents services AWS pour vous entraîner à ingérer de grands ensembles de données, à les transformer et à en extraire des informations.

Dans ce projet de clôture, vous êtes mis au défi de construire une solution qui utilise de nombreux services AWS qui vous sont familiers, sans recevoir de directives pas à pas. Certaines sections de l'exercice sont conçues pour être difficiles.

Ce projet de clôture vous mettra au défi de faire ce qui suit :

- Lancer et configurer une instance d’environnement de développement intégré (IDE) AWS Cloud9.
- Transformer des fichiers de données au format CSV en format Apache Parquet et les télécharger sur Amazon S3.
- Créer un robot d'exploration AWS Glue pour inférer la structure des données.
- Utiliser Amazon Athena pour interroger les données.
- Créer une vue Athena.
- Utiliser Amazon QuickSight pour visualiser les données.

#### Durée et suivi de votre budget

Ce projet est estimé à environ 4 heures de travail.

Cet environnement est à long terme. Lorsque le minuteur de la session atteint 0:00, la session se termine, mais toutes les données et ressources que vous avez créées dans le compte AWS seront conservées. Si vous lancez une nouvelle session ultérieurement (par exemple, le jour suivant), vous constaterez que votre travail est toujours présent dans l'environnement du laboratoire. De plus, à tout moment avant que le minuteur de la session n'atteigne 0:00, vous pouvez choisir "Start Lab" à nouveau pour prolonger le temps de la session.

**Important :** Surveillez votre budget de laboratoire dans l'interface du laboratoire. Lorsque vous avez une session de laboratoire active, les informations sur le budget restant s'affichent en haut de cet écran. Ces données proviennent d'AWS Budgets, qui se met généralement à jour toutes les 8 à 12 heures. Par conséquent, le budget restant que vous voyez peut ne pas refléter votre activité de compte la plus récente. Si vous dépassez votre budget de laboratoire, votre compte de laboratoire sera désactivé et tout progrès et toutes les ressources seront perdus. Il est donc important de gérer vos dépenses.

#### Restrictions des services AWS

Dans cet environnement de laboratoire, l'accès aux services et aux actions des services AWS peut être limité à ceux nécessaires pour accomplir les instructions du laboratoire. Vous pourriez rencontrer des erreurs si vous essayez d'accéder à d'autres services ou d'effectuer des actions au-delà de celles décrites dans ce laboratoire.

#### L'ensemble de données

Le site Sea Around Us propose un ensemble de données avec des informations historiques détaillées sur les pêcheries dans toutes les parties de chaque océan à l'échelle mondiale. Les données incluent des informations sur les captures de pêcheries annuelles de 1950 à 2018.

Les données peuvent être téléchargées au format CSV à partir du site Sea Around Us. L'ensemble de données comprend des colonnes d'informations pour chaque année, y compris les pays qui ont capturé quels types de poissons dans quelles zones. Les données indiquent également combien de tonnes de poissons ont été capturées et quelle était la valeur de la capture, mesurée en dollars américains de 2010.

Pour comprendre les données, il sera utile de comprendre ce que l'on entend par zones de haute mer et zones économiques exclusives (ZEE) :

- **Haute mer :** Zones de l'océan situées à au moins 200 milles marins de la côte d'un pays. Les ressources, y compris les poissons, dans ces zones ne sont généralement pas attribuées à un seul pays.
- **Zones Économiques Exclusives (ZEE) :** Zones situées à moins de 200 milles marins de la côte d'un pays. Chaque pays revendique généralement un accès exclusif aux ressources dans ces zones, y compris les poissons qui s'y trouvent.

Les données de Sea Around Us sont (sauf indication contraire) sous licence Creative Commons Attribution Non-Commercial 4.0 International License (https://creativecommons.org/licenses/by-nc/4.0/). Les avis concernant les droits d'auteur (© Université de la Colombie-Britannique), la licence et la clause de non-responsabilité sont disponibles à l'adresse http://www.seaaroundus.org/terms-and-conditions/.

#### Scénario

Vous avez pour mission de créer l'infrastructure pour héberger des données sur la pêche afin que les analystes de données de votre organisation puissent créer des rapports sur l'impact de la pêche en haute mer. Vous avez décidé de construire l'infrastructure dans votre compte AWS et de la tester en utilisant trois fichiers de données provenant de l'ensemble de données Sea Around Us.

Dans ce projet de clôture, vous travaillerez avec trois fichiers de données du site Sea Around Us :

- Le premier fichier contient des données provenant de toutes les zones de haute mer.
- Le deuxième fichier contient des données provenant d'une seule zone de haute mer dans le Pacifique, connue sous le nom de Pacific, Western Central, qui n'est pas loin des Fidji et de nombreux autres pays.
- Le troisième fichier contient des données provenant de la ZEE d'un seul pays (les Fidji), proche de la zone de haute mer Pacific, Western Central.

En travaillant avec cette variété de fichiers de données, vous pourrez tester si la solution que vous construisez peut prendre en charge un ensemble de données beaucoup plus grand.

Lorsque vous commencez le projet de clôture, l'environnement contiendra les ressources montrées dans le diagramme suivant.

*Architecture au début du projet de clôture.*

À la fin du projet de clôture, vous aurez créé une architecture similaire à celle montrée dans le diagramme suivant.

*Architecture à la fin du projet de clôture.*

#### Accéder à la console de gestion AWS

1. En haut de ces instructions, choisissez **Start Lab**.
   - La session de laboratoire commence.
   - Un minuteur s'affiche en haut de la page et montre le temps restant dans la session.
   - **Conseil :** Pour rafraîchir la durée de la session à tout moment, choisissez **Start Lab** à nouveau avant que le minuteur n'atteigne 0:00.
   - Avant de continuer, attendez que l'icône de cercle à droite du lien **AWS** dans le coin supérieur gauche devienne verte.
2. Pour vous connecter à la console de gestion AWS, choisissez le lien **AWS** dans le coin supérieur gauche.
   - Un nouvel onglet de navigateur s'ouvre et vous connecte à la console.
   - **Conseil :** Si un nouvel onglet ne s'ouvre pas, une bannière ou une icône se trouve généralement en haut de votre navigateur avec le message que votre navigateur empêche le site d'ouvrir des fenêtres pop-up. Choisissez la bannière ou l'icône, puis choisissez **Autoriser les pop-ups**.

#### Tâche 1 : Configurer l'environnement de développement

Dans cette première partie du projet de clôture, vous configurerez votre environnement de développement.

1. Observez les détails du rôle AWS Identity and Access Management (IAM) **CapstoneGlueRole** qui a été créé pour vous.
2. Créez un environnement AWS Cloud9 avec les paramètres suivants :
   - Nommez l'environnement **CapstoneIDE**.
   - Créez une nouvelle instance EC2 pour l'environnement et utilisez une instance t2.micro.
   - Déployez l'instance pour prendre en charge les connexions SSH au VPC Capstone, dans le sous-réseau public Capstone.
   - Conservez tous les autres paramètres par défaut.
3. Créez deux buckets S3 avec les paramètres suivants :
   - Créez les buckets dans la région us-east-1.
   - Nommez le premier bucket **data-source-#####** où ##### est un nombre aléatoire.
   - Nommez le deuxième bucket **query-results-#####** où ##### est également un nombre aléatoire.
   - Conservez tous les autres paramètres par défaut.

4. Pour télécharger les trois fichiers source .csv, exécutez les commandes suivantes dans le terminal de votre IDE AWS Cloud9 :
   ```bash
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-GLOBAL-1-v48-0.csv
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-HighSeas-71-v48-0.csv
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-EEZ-242-v48-0.csv
   ```
5. Pour observer la ligne d'en-tête des colonnes et les cinq premières lignes de données dans le fichier SAU-GLOBAL-1-v48-0.csv, exécutez la commande suivante :
   ```bash
   head -6 SAU-GLOBAL-1-v48-0.csv
   ```
   **Analyse :** Entre autres détails, chaque ligne de données de cet ensemble comprend :
   - L'année de la pêche
   - Le pays (fishing_entity) qui a pêché
   - Les tonnes de poissons capturées cette année-là par ce pays
   - La valeur en dollars américains de 2010 (landed_value) des poissons capt

urés cette année-là par ce pays

   **Remarque :**
   - Cet ensemble de données contient 561 675 lignes.
   - **Conseil :** Pour confirmer cela, exécutez `wc -l <nom_du_fichier>`.

6. Convertissez le fichier SAU-GLOBAL-1-v48-0.csv en format Parquet.
   - Tout d'abord, vous devez installer quelques outils sur votre IDE AWS Cloud9. Exécutez la commande suivante :
   ```bash
   sudo pip3 install pandas pyarrow fastparquet
   ```
   - Ensuite, pour convertir le fichier en format Parquet, exécutez le code suivant :
   ```python
   # Démarrer l'interpréteur Python interactif 
   python
   
   # Utiliser pandas pour convertir le fichier en format parquet
   import pandas as pd
   df = pd.read_csv('SAU-GLOBAL-1-v48-0.csv')
   df.to_parquet('SAU-GLOBAL-1-v48-0.parquet')
   exit()
   ```
   **Remarque :** Pandas est un outil utile pour travailler avec des fichiers de données. Pour plus d'informations, consultez le site web de pandas.
7. Pour télécharger le fichier SAU-GLOBAL-1-v48-0.parquet vers le bucket data-source, utilisez une commande AWS CLI dans votre terminal AWS Cloud9.

#### Tâche 2 : Utiliser un robot d'exploration AWS Glue et interroger plusieurs fichiers avec Athena

La requête que vous avez exécutée dans la tâche précédente fonctionne bien pour un seul fichier de données. Cependant, que faire si vous devez interroger un ensemble de données plus large composé de plusieurs fichiers ?

Dans cette deuxième partie du projet de clôture, vous interrogez des données stockées dans plusieurs fichiers. Pour ce faire, vous configurez un robot d'exploration AWS Glue pour découvrir la structure des données, puis utilisez Athena pour interroger les données.

1. Pour observer la ligne d'en-tête des colonnes et les premières lignes de données du fichier SAU-HighSeas-71-v48-0.csv, utilisez la commande `head`.
   - Rappelez-vous que vous avez déjà téléchargé le fichier sur votre IDE AWS Cloud9. 
   - Le fichier contient les mêmes colonnes que le fichier SAU-GLOBAL-1-v48-0.csv, mais il comporte des colonnes supplémentaires. Le tableau suivant montre les colonnes contenues dans chaque fichier.

**Analyse :**
- Comme l'ensemble de données SAU-GLOBAL-1-v48-0 que vous avez déjà téléchargé sur Amazon S3 et interrogé, l'ensemble de données SAU-HighSeas-71-v48-0 décrit également les captures de poissons en haute mer. Cependant, l'ensemble de données HighSeas comprend uniquement des données provenant d'une seule zone de haute mer, connue sous le nom de Pacific, Western Central.

   - Parmi les colonnes supplémentaires de l'ensemble de données HighSeas, deux sont particulièrement intéressantes :
     - La colonne `area_name` contient la valeur "Pacific, Western Central" dans chaque ligne.
     - La colonne `common_name` contient des valeurs qui décrivent certains types de poissons (par exemple, "Mackerels, tunas, bonitos").

2. Convertissez le fichier SAU-HighSeas-71-v48-0.csv en format Parquet et téléchargez-le dans le bucket data-source.

3. Créez une base de données AWS Glue et un robot d'exploration AWS Glue avec les paramètres suivants :
   - Nommez la base de données **fishdb**.
   - Nommez le robot d'exploration **fishcrawler**.
   - Configurez le robot pour utiliser le rôle IAM **CapstoneGlueRole** pour explorer le contenu du bucket S3 data-source.
   - Exportez les résultats du robot d'exploration vers la base de données fishdb.
   - Réglez la fréquence du robot sur **À la demande**.

4. Exécutez le robot pour créer une table contenant des métadonnées dans la base de données AWS Glue.
   - Vérifiez que la table attendue est créée.

5. Pour confirmer que la table a correctement catégorisé les données, utilisez Athena pour exécuter des requêtes SQL sur chaque colonne de la nouvelle table.
   - **Important :** Avant d'exécuter la première requête dans Athena, configurez l'éditeur de requêtes Athena pour exporter les données vers le bucket query-results.
   - **Exemple de requête :**
   ```sql
   SELECT DISTINCT area_name FROM fishdb.data_source_xxxxx;
   ```
   **Remarque :** La requête d'exemple renvoie deux résultats. Pour cette colonne, chaque ligne de l'ensemble de données contient soit la valeur "Pacific, Western Central" (pour les lignes extraites de SAU-HighSeas-71-v48-0.parquet), soit une valeur nulle (pour les lignes extraites de SAU-GLOBAL-1-v48-0.parquet). 

6. Maintenant que votre table de données est définie, exécutez des requêtes pour confirmer qu'elle fournit des résultats utiles.
   - Pour trouver la valeur en dollars US de tous les poissons capturés par le pays Fidji dans la zone de haute mer Pacific, Western Central depuis 2001, organisés par année, utilisez la requête suivante (assurez-vous de remplacer `<FMI_1>` et `<FMI_2>` par les valeurs appropriées) :
   ```sql
   SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValuePacificWCSeasCatch
   FROM <FMI_1>
   WHERE area_name LIKE '%Pacific%' and fishing_entity='Fiji' AND year > <FMI_2>
   GROUP BY year, fishing_entity
   ORDER By year
   ```
   **Remarque :** La partie `CAST(CAST(sum(landed_value) AS DOUBLE) AS DECIMAL(38,2))` de la requête garantit que le format des données retournées depuis la colonne `landed_value` s'affiche dans un format facile à lire (dollars et centimes) au lieu du format scientifique.

   - **Défi :** Trouvez la valeur en dollars US de tous les poissons capturés par le pays Fidji dans toutes les zones de haute mer depuis 2001, organisés par année. Dans vos résultats, nommez la colonne de valeur en dollars US **ValueAllHighSeasCatch**.

   **Conseils utiles pour le défi :**
   - Votre clause WHERE devrait inclure deux mots-clés AND.
   - Pour retourner les lignes qui ne contiennent pas d'entrée pour une colonne particulière, utilisez IS NULL.

7. Après avoir créé et exécuté la requête correcte et visualisé les résultats affichés, créez une vue basée sur la requête :
   - Choisissez **Create** > **View from query**.
   - Nommez la vue **challenge**.

#### Tâche 3 : Transformer un nouveau fichier et l'ajouter à l'ensemble de données

Dans cette partie du projet de clôture, vous allez ajouter le fichier de données **SAU-EEZ-242-v48-0.csv** à votre ensemble de données dans Amazon S3.

Ce fichier a quelques noms de colonnes qui ne correspondent pas aux données que vous avez déjà ajoutées à votre bucket S3. Cependant, les données de ces colonnes sont alignées avec les données existantes. Vous devrez modifier les noms des colonnes avant d'ajouter les données à votre bucket.

1. Analysez la structure des données du fichier **SAU-EEZ-242-v48-0.csv**. Comparez les colonnes qu'il contient avec celles des autres fichiers de données.

   **Conseil :** La plupart des noms de colonnes correspondent entre les trois fichiers. Cependant, deux des noms de colonnes dans le fichier ZEE ne sont pas une correspondance exacte.

2. Utilisez la bibliothèque d'analyse de données Python appelée **pandas** pour corriger les noms de colonnes. De plus, convertissez le fichier ZEE au format Parquet.

   Pour accomplir ces tâches, exécutez toutes les commandes du bloc de code suivant. Cependant, avant d'exécuter la commande `df.rename`, remplacez les espaces réservés `<FMI_#>` par les valeurs correctes.

   **Conseils :**
   - Par exemple, `<FMI_1>` doit être défini sur l'un des noms de colonnes que vous souhaitez changer et `<FMI_2>` doit être défini sur ce que vous voulez changer.
   - Il sera plus facile de lire la sortie des lignes de print si vous élargissez autant que possible la fenêtre de votre navigateur.

   ```python
   # Faire une sauvegarde du fichier avant de le modifier sur place
   cp SAU-EEZ-242-v48-0.csv SAU-EEZ-242-v48-0-old.csv

   # Démarrer l'interpréteur Python interactif
   python

   import pandas as pd

   # Charger la version sauvegardée du fichier
   data_location = 'SAU-EEZ-242-v48-0-old.csv'

   # Utiliser Pandas pour lire le fichier CSV dans un dataframe
   df = pd.read_csv(data_location)

   # Afficher les noms de colonnes actuels
   print(df.head(1))

   # Changer les noms des colonnes 'fish_name' et 'country' pour correspondre aux noms de colonnes où ces données apparaissent dans les autres fichiers de données déjà dans votre bucket data-source
  

 df.rename(columns = {"<FMI_1>": "<FMI_2>", "<FMI_3>": "<FMI_4>"}, inplace = True)

   # Vérifier que les noms de colonnes ont été changés
   print(df.head(1))

   # Écrire les changements sur disque
   df.to_csv('SAU-EEZ-242-v48-0.csv', header=True, index=False)
   df.to_parquet('SAU-EEZ-242-v48-0.parquet')
   exit()
   ```

3. Téléchargez le nouveau fichier de données ZEE dans le bucket data-source.

4. Pour mettre à jour les métadonnées de la table avec les colonnes supplémentaires qui font maintenant partie de votre ensemble de données, exécutez à nouveau le robot d'exploration AWS Glue.

5. Exécutez quelques requêtes dans Athena.
   **Remarque :** Pour toutes ces requêtes, remplacez `data_source_#####` par le nom de votre table `data_source`.

   - Pour vérifier les valeurs dans la colonne `area_name`, comme vous l'avez fait précédemment, utilisez la requête suivante :
   ```sql
   SELECT DISTINCT area_name FROM fishdb.data_source_#####;
   ```
   Avec l'ajout du fichier ZEE à l'ensemble de données, cette requête renvoie maintenant trois résultats, y compris le résultat pour les lignes où la colonne `area_name` ne contient aucune donnée. (Rappelez-vous que cette requête ne renvoyait que deux résultats précédemment.)

   - Pour trouver la valeur en dollars US de tous les poissons capturés par Fidji dans les eaux de haute mer depuis 2001, organisés par année, utilisez la requête suivante :
   ```sql
   SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueOpenSeasCatch 
   FROM fishdb.data_source_#####
   WHERE area_name IS NULL AND fishing_entity='Fiji' AND year > 2000
   GROUP BY year, fishing_entity
   ORDER By year
   ```

   - Pour trouver la valeur en dollars US de tous les poissons capturés par Fidji dans la ZEE de Fidji depuis 2001, organisés par année, utilisez la requête suivante :
   ```sql
   SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueEEZCatch
   FROM fishdb.data_source_#####
   WHERE area_name LIKE '%Fiji%' AND fishing_entity='Fiji' AND year > 2000
   GROUP BY year, fishing_entity
   ORDER By year
   ```

   - Pour trouver la valeur en dollars US de tous les poissons capturés par Fidji dans la ZEE de Fidji ou en haute mer depuis 2001, organisés par année, utilisez la requête suivante :
   ```sql
   SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueEEZAndOpenSeasCatch 
   FROM fishdb.data_source_#####
   WHERE (area_name LIKE '%Fiji%' OR area_name IS NULL) AND fishing_entity='Fiji' AND year > 2000
   GROUP BY year, fishing_entity
   ORDER By year
   ```

   **Analyse :** Si vos données sont bien formatées et que le robot d'exploration AWS Glue a correctement mis à jour la table de métadonnées, les résultats que vous obtenez des deux premières requêtes de cette étape devraient correspondre aux résultats que vous obtenez de la troisième requête.

   Par exemple, si vous additionnez la valeur `ValueOpenSeasCatch` de 2001 et la valeur `ValueEEZCatch` de 2001, le total devrait être égal à la valeur `ValueEEZAndOpenSeasCatch` de 2001. Si vos résultats sont cohérents avec cette description, c'est un bon indicateur que votre solution fonctionne comme prévu.

6. Créez une vue dans Athena, qui sera utile pour visualiser les données dans la dernière partie de ce projet de clôture.
   - Exécutez la requête suivante. Remplacez `data_source_#####` par le nom de votre table `data_source` :
   ```sql
   CREATE OR REPLACE VIEW MackerelsCatch AS
   SELECT year, area_name AS WhereCaught, fishing_entity as Country, SUM(tonnes) AS TotalWeight
   FROM fishdb.data_source_#####
   WHERE common_name LIKE '%Mackerels%' AND year > 2014
   GROUP BY year, area_name, fishing_entity, tonnes
   ORDER BY tonnes DESC
   ```
   - Pour vérifier que la vue contient des données, dans le panneau **Data**, sous **Views**, choisissez l'icône des ellipses (trois points) à droite de la vue `mackerelscatch`, puis choisissez **Preview View**.

#### Tâche 4 : Visualiser les résultats dans QuickSight

Dans cette dernière partie du projet de clôture, vous serez mis au défi d'utiliser QuickSight pour créer un graphique à barres à partir de votre ensemble de données. Vous modifierez également la requête sur laquelle le graphique est basé, ce qui ajustera ce qui s'affiche dans le graphique.

1. Créez un compte QuickSight.
   - Accédez à la console QuickSight.
   - Choisissez **Sign up for QuickSight**.
   - En bas de la page, là où il est indiqué **Sign up for Standard Edition here**, choisissez **here**.
   - **Important :** Un message indique que vous n'avez peut-être pas les autorisations nécessaires pour vous inscrire à QuickSight. C'est prévu dans l'environnement de laboratoire. Ignorez le message et continuez à suivre ces instructions pour créer le compte.
   - Sur la page **Create your QuickSight account**, configurez les éléments suivants :
     - **Nom du compte QuickSight :** Entrez un nom, par exemple `capstone-#####`, où ##### est un nombre aléatoire.
     - **Adresse e-mail de notification :** Entrez une adresse e-mail à laquelle vous pouvez accéder.
     - Dans la zone **QuickSight access to AWS services** :
       - Vérifiez qu'Athena est sélectionné. Si ce n'est pas déjà le cas, sélectionnez-le maintenant.
       - Désélectionnez tous les autres services.
   - Choisissez **Finish**.
   - **Important :** Vous verrez à nouveau un message indiquant que vous n'avez peut-être pas toutes les autorisations nécessaires. Vous pouvez ignorer ce message. La création du compte prendra une ou deux minutes.
   - Choisissez **Go to Amazon QuickSight**.
   - La page **QuickSight Analyses** s'affiche. Laissez cet onglet de navigateur ouvert. Vous y reviendrez dans un instant.
   - Dans un nouvel onglet de navigateur, accédez à la console IAM.
     - Choisissez **Roles**, puis choisissez le rôle `aws-quicksight-service-role-v0`.
     - Dans le menu **Add permissions**, choisissez **Attach policies**.
     - Recherchez la politique `AmazonS3FullAccess`, puis sélectionnez-la en cochant la case à côté.
     - Choisissez **Add permissions**.
     - Dans la page du rôle `aws-quicksight-service-role-v0`, vérifiez que la politique `AmazonS3FullAccess` apparaît maintenant dans l'onglet **Permissions**.
   - Revenez à l'onglet du navigateur où QuickSight est ouvert.

2. Créez un nouvel ensemble de données QuickSight.
   - Dans le volet de navigation, choisissez **Datasets**.
   - Choisissez **New dataset**.
   - Choisissez **Athena**, puis configurez les éléments suivants :
     - **Nom de la source de données :** Entrez `MackerelsView`.
     - **Athena workgroup :** Choisissez `[primary]`.
   - Choisissez **Validate connection**.
   - Choisissez **Create data source**.
   - Configurez les éléments suivants :
     - **Catalog :** Choisissez `AwsDataCatalog`.
     - **Database :** Choisissez `fishdb`.
     - **Tables :** Choisissez `mackerelscatch`.
   - Choisissez **Select**.
   - Choisissez **Directly query your data**, puis choisissez **Visualize**.

3. Créez un graphique dans QuickSight.
   - Si un panneau **New sheet** s'affiche, gardez **Interactive sheet** sélectionné et choisissez **CREATE**.
   - Dans la liste **Fields** à gauche, faites glisser `year` vers le graphique vide.
   - Pour agrandir la zone **Field wells**, choisissez l'icône de flèche dans le coin supérieur droit de la page.
   - Les paramètres **Y axis**, **Value**, et **Group/Color** sont affichés pour le graphique que vous construisez.
   - Vérifiez que `year` se trouve dans le champ **Y axis**. Si ce n'est pas le cas, faites-le glisser maintenant.
   - Dans la liste **Fields**, faites glisser `country` vers le champ **Group/Color**.
   - Faites glisser `totalweight` vers le champ **Value**.
   - Ajustez les paramètres du graphique :
     - Choisissez l'icône des deux flèches dans le coin supérieur droit du graphique pour en agrandir la taille.
     - Double-cliquez sur le titre du graphique, **Sum of Totalweight by Year

 and Country**. 
     - Placez votre curseur dans le champ de texte qui indique **Default**, et entrez le titre suivant comme nouveau titre du graphique : **Tonnes de maquereaux capturées par année par pays dans la ZEE des Fidji et dans la zone de haute mer Pacific, Western Central**.
     - Choisissez **Save**.
     - Sur le côté gauche du graphique, choisissez l'icône de flèche (>) au-dessus de `year`. Choisissez **Format**, puis choisissez le format de nombre qui ne contient ni virgules ni points.
     - Choisissez l'icône de flèche sur l'onglet **Sheet 1**, au-dessus du graphique, choisissez **Rename**, et renommez la feuille en **Fish Data**.

4. Réduisez la quantité de données affichées dans le graphique.
   - Revenez à l'éditeur de requêtes Athena, et modifiez la vue que vous avez créée pour qu'elle ne comprenne que les données pour les six entités de pêche qui ont capturé le plus grand poids total de maquereaux dans une année entre 2013 et 2018.
   - **Conseils :**
     - La requête SQL suivante identifiera les entités de pêche avec la plus grande capture de maquereaux chaque année :
     ```sql
     SELECT year, Country, MAX(TotalWeight) AS Weight
     FROM fishdb.mackerelscatch 
     GROUP BY year, Country  
     ORDER BY year, Weight DESC;
     ```
     - **Remarque :** L'ordre des entités de pêche ayant capturé le plus de poissons chaque année varie. Cependant, même si un pays a capturé le troisième plus grand nombre de poissons une année, et le quatrième plus grand nombre une autre année, vous devriez observer que ce sont toujours les mêmes six entités de pêche qui sont listées chaque année dans les six premières positions pour les poissons capturés.
     - Dans la requête que vous avez exécutée précédemment pour créer la vue `MackerelsCatch` (la requête commence par **CREATE OR REPLACE VIEW MackerelsCatch**), modifiez la clause WHERE pour ajuster les entités de pêche et les années qui apparaîtront dans l'ensemble de données. Vous pourriez inclure **AND fishing_entity IN** comme partie de votre clause WHERE.
   - Revenez à l'onglet du navigateur où vous avez le graphique **Fish Data** ouvert dans QuickSight, et rafraîchissez la page du navigateur.
   - L'affichage des données a changé et n'inclut maintenant que les années et les pays spécifiés, comme illustré dans l'image suivante :

5. **Soumettre votre travail**
   - Pour enregistrer vos progrès, choisissez **Submit** en haut de ces instructions.   
   - Lorsque vous y êtes invité, choisissez **Yes**.
   - Après quelques minutes, le panneau des notes apparaît et vous montre combien de points vous avez obtenus pour chaque tâche. Si les résultats ne s'affichent pas après quelques minutes, choisissez **Grades** en haut de ces instructions.
   - **Conseil :** Vous pouvez soumettre votre travail plusieurs fois. Après avoir modifié votre travail, choisissez **Submit** à nouveau. Votre dernière soumission est enregistrée pour ce laboratoire.
   - Pour trouver des commentaires détaillés sur votre travail, choisissez **Submission Report**.

6. **Terminer votre session**
   - **Rappel :** Il s'agit d'un environnement de laboratoire à long terme. Les données sont conservées jusqu'à ce que vous utilisiez soit le budget alloué, soit la date de fin du cours (selon ce qui survient en premier).
   - Pour préserver votre budget lorsque vous avez terminé pour la journée, ou lorsque vous avez terminé de travailler activement sur la tâche, faites ce qui suit :
     1. En haut de cette page, choisissez **End Lab**, puis choisissez **Yes** pour confirmer que vous voulez terminer le laboratoire.
     2. Un panneau de message indique que le laboratoire est en train de se terminer.
     3. **Remarque :** Choisir **End Lab** dans cet environnement de projet de clôture ne supprimera pas la ressource que vous avez créée. Elles seront toujours là la prochaine fois que vous choisirez **Start Lab** (par exemple, un autre jour).
     4. Pour fermer le panneau, choisissez **Close** dans le coin supérieur droit.

