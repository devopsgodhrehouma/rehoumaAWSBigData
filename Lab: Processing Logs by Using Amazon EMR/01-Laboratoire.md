### Traitement des journaux à l'aide d'Amazon EMR

#### Aperçu et objectifs du laboratoire

Apache Hadoop est un framework open-source qui prend en charge le traitement massif de données sur un cluster d'instances. Hadoop peut s'exécuter sur une seule instance Amazon Elastic Compute Cloud (Amazon EC2) ou sur des milliers d'instances. Hadoop utilise le système de fichiers distribué Hadoop (HDFS) pour stocker les données sur plusieurs instances, que Hadoop appelle des nœuds. Hadoop peut également lire et écrire des données vers et à partir d'Amazon Simple Storage Service (Amazon S3). Le service Amazon EMR regroupe le noyau de Hadoop et d'autres projets liés à Hadoop dans chaque version. Vous verrez une liste de ces services lors de la création d'un cluster EMR dans ce laboratoire.

Dans ce laboratoire, vous allez utiliser Amazon EMR pour traiter des journaux stockés dans un compartiment S3. Pour traiter les données dans Amazon EMR, vous utiliserez Apache Hive. Hive est un cadre d'infrastructure d'entrepôt de données open-source construit au-dessus de Hadoop, souvent utilisé pour traiter de grands ensembles de données. Avec Hive, vous pouvez interroger de grands ensembles de données avec des requêtes de type SQL. Lorsque vous exécutez une requête Hive, la demande est traduite en tâche YARN (Yet Another Resource Negotiator). YARN est la partie du logiciel Hadoop qui coordonne la planification des tâches.

Après avoir terminé ce laboratoire, vous serez en mesure de :
- Lancer un cluster EMR via la console de gestion AWS.
- Exécuter Hive de manière interactive.
- Utiliser des commandes Hive pour créer des tables à partir de données de journaux stockées dans Amazon S3.
- Utiliser des commandes Hive pour joindre des tables et stocker la table jointe dans Amazon S3.
- Utiliser Hive pour interroger des tables stockées dans Amazon S3.

#### Durée
Ce laboratoire prendra environ 90 minutes à compléter.

#### Restrictions de services AWS
Dans cet environnement de laboratoire, l'accès aux services et actions AWS pourrait être limité aux éléments nécessaires pour compléter les instructions du laboratoire. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'effectuer des actions non mentionnées dans ce laboratoire.

---

### Scénario

Un autre département universitaire a demandé à votre groupe de rechercher des tendances dans les données publicitaires en ligne contenues dans un ensemble de fichiers de journaux web. Les journaux sont répartis dans plusieurs fichiers en raison du processus de rotation des journaux utilisé pour les collecter et les stocker. Deux types de journaux vous ont été fournis :
- **Journaux d'impressions** : Chaque entrée indique qu'une publicité a été affichée à un utilisateur.
- **Journaux de clics** : Chaque entrée indique qu'un utilisateur a cliqué sur une publicité.

Votre défi est de développer une preuve de concept (POC) pour trouver des tendances montrant quelles impressions ont conduit à des clics publicitaires.

Après avoir discuté du défi avec vos coéquipiers, vous avez décidé qu'Amazon EMR serait un bon choix pour relever ce défi de données. Vous savez qu'Amazon EMR peut traiter de grands ensembles de données et qu'il peut lire à partir et écrire vers Amazon S3. De plus, vous avez appris que vous pouvez utiliser Hive (un outil inclus avec Amazon EMR) pour construire un entrepôt de données. Ensuite, vous pouvez exécuter des requêtes de type SQL sur les données.

Lorsque vous démarrez le laboratoire, l'environnement contiendra les ressources illustrées dans le diagramme suivant. Pour cet environnement de laboratoire, la source de données d'origine est un compartiment S3 situé dans un autre compte AWS.

---

### Tâche 1 : Lancer un cluster Amazon EMR

1. Accédez à la console EMR via la recherche dans la barre des services AWS.
2. Cliquez sur **Créer un cluster**.
3. Configurez les options suivantes :
   - **Nom du cluster** : `Hive EMR cluster`
   - **Version EMR** : `emr-5.29.0`
   - Sélectionnez les applications : Hadoop 2.8.5, Hive 2.3.6.
   - **Type d'instance principale et de nœud** : `m4.large`
   - Sélectionnez le réseau **Lab VPC** et le sous-réseau **Lab subnet**.
4. Désactivez l'option de protection de terminaison du cluster et choisissez un compartiment S3 pour stocker les résultats Hive.
5. Sélectionnez une paire de clés SSH (vockey).
6. Créez le cluster et attendez qu'il soit en état **Running**.

---

### Tâche 2 : Connexion au nœud principal via SSH

1. Récupérez l'adresse DNS publique du nœud principal dans l'onglet **Résumé du cluster**.
2. Connectez-vous à AWS Cloud9 et ouvrez un terminal bash.
3. Téléchargez la clé privée SSH (`labsuser.pem`) sur Cloud9 et modifiez ses permissions avec la commande :
   ```bash
   chmod 400 labsuser.pem
   ```
4. Connectez-vous au nœud principal avec SSH :
   ```bash
   ssh -i labsuser.pem hadoop@<PUBLIC-DNS>
   ```
5. Vous êtes maintenant connecté au nœud principal du cluster.

---

### Tâche 3 : Exécution de Hive de manière interactive

1. Configurez un répertoire pour les logs Hive :
   ```bash
   sudo chown hadoop -R /var/log/hive
   mkdir /var/log/hive/user/hadoop
   ```
2. Exécutez Hive avec les paramètres appropriés pour lire et écrire des données dans S3 :
   ```bash
   hive -d SAMPLE=s3://aws-tc-largeobjects/... -d OUTPUT=s3://<HIVE-BUCKET-NAME>/output/
   ```

---

### Tâche 4 : Créer des tables à partir des données sources

1. Créez une table externe `impressions` :
   ```sql
   CREATE EXTERNAL TABLE impressions (
       requestBeginTime string,
       adId string,
       impressionId string,
       referrer string,
       userAgent string,
       userCookie string,
       ip string)
   PARTITIONED BY (dt string)
   ROW FORMAT  serde 'org.apache.hive.hcatalog.data.JsonSerDe'
   LOCATION '${SAMPLE}/tables/impressions';
   ```
2. Mettez à jour la partition avec la commande :
   ```bash
   MSCK REPAIR TABLE impressions;
   ```
3. Répétez le processus pour la table `clicks`.

---

### Tâche 5 : Jointure des tables avec Hive

1. Créez une table `joined_impressions` pour stocker les résultats de la jointure :
   ```sql
   CREATE EXTERNAL TABLE joined_impressions (
       requestBeginTime string,
       adId string,
       impressionId string,
       referrer string,
       userAgent string,
       userCookie string,
       ip string,
       clicked Boolean)
   PARTITIONED BY (day string, hour string)
   STORED AS SEQUENCEFILE
   LOCATION '${OUTPUT}/tables/joined_impressions';
   ```
2. Créez des tables temporaires `tmp_impressions` et `tmp_clicks` pour stocker des données intermédiaires.
3. Exécutez une jointure externe gauche entre `tmp_impressions` et `tmp_clicks` pour remplir la table `joined_impressions`.

---

### Tâche 6 : Interrogation des résultats

1. Exécutez des requêtes Hive pour analyser les données :
   ```sql
   SELECT adid, count(*) AS hits FROM joined_impressions WHERE clicked = true GROUP BY adid ORDER BY hits DESC LIMIT 10;
   ```
2. Exportez les résultats vers un fichier texte :
   ```bash
   hive -e "SELECT referrer, count(*) as hits FROM joined_impressions WHERE clicked = true GROUP BY referrer ORDER BY hits DESC LIMIT 10;" > result.txt
   ```

---

### Soumission du travail

Cliquez sur **Soumettre** pour enregistrer vos progrès et obtenir votre évaluation finale.

Félicitations, vous avez terminé ce laboratoire !


# Annexe : 

La commande Hive suivante est une commande `hive` avec plusieurs options `-d`, qui définissent des variables dynamiques. Ces variables sont utilisées dans les requêtes Hive qui vont être exécutées. Voici une explication détaillée des différents éléments de cette commande :

### Structure générale de la commande :
```bash
hive -d SAMPLE=s3://aws-tc-largeobjects/CUR-TF-200-ACDENG-1/emr-lab \
     -d DAY=2009-04-13 \
     -d HOUR=08 \
     -d NEXT_DAY=2009-04-13 \
     -d NEXT_HOUR=09 \
     -d OUTPUT=s3://<HIVE-BUCKET-NAME>/output/
```

1. **hive** :
   - C'est le binaire ou la commande principale pour exécuter une requête Hive. Apache Hive est un système de data warehousing utilisé pour lire, écrire et gérer de grandes bases de données stockées dans des systèmes distribués comme Hadoop.

2. **`-d`** : 
   - Cette option permet de définir des variables dynamiques. Ces variables peuvent ensuite être référencées dans une requête Hive à l’aide de la syntaxe `${VARIABLE_NAME}`. 

### Détails des variables :

1. **`-d SAMPLE=s3://aws-tc-largeobjects/CUR-TF-200-ACDENG-1/emr-lab`** :
   - Cette variable `SAMPLE` définit un chemin vers un dossier ou un fichier stocké dans un bucket S3 d’Amazon (c'est un service de stockage d'objets).
   - Ce chemin sera utilisé dans la requête Hive pour spécifier où trouver l’échantillon de données à traiter.
   
2. **`-d DAY=2009-04-13`** :
   - La variable `DAY` représente une date spécifique. Elle pourrait être utilisée pour filtrer des données par date dans une requête Hive, par exemple dans une condition de type `WHERE date = '${DAY}'`.

3. **`-d HOUR=08`** :
   - La variable `HOUR` représente une heure spécifique de la journée, ici `08` (8 heures du matin). Cela peut servir à filtrer des données qui ont été collectées à cette heure-là.

4. **`-d NEXT_DAY=2009-04-13`** :
   - La variable `NEXT_DAY` indique un jour suivant ou simplement une autre date, mais dans ce cas particulier, elle est la même que `DAY`. Cela pourrait être utilisé pour comparer ou filtrer des événements entre deux jours différents ou dans une plage de temps.

5. **`-d NEXT_HOUR=09`** :
   - De même, la variable `NEXT_HOUR` indique l’heure suivante (`09`, soit 9 heures du matin). Cela pourrait être utilisé pour définir une plage horaire dans une requête Hive.

6. **`-d OUTPUT=s3://<HIVE-BUCKET-NAME>/output/`** :
   - La variable `OUTPUT` spécifie un chemin de sortie dans un bucket S3. C'est là où les résultats de la requête Hive vont être stockés. Le `<HIVE-BUCKET-NAME>` est un espace réservé qui doit être remplacé par le nom réel du bucket dans S3.

### Utilisation dans une requête Hive :

Dans une requête Hive, tu pourrais voir ces variables référencées comme suit :

```sql
INSERT OVERWRITE DIRECTORY '${OUTPUT}'
SELECT * 
FROM some_table
WHERE sample_path = '${SAMPLE}'
  AND day = '${DAY}'
  AND hour >= '${HOUR}' AND hour < '${NEXT_HOUR}';
```

Ici, la commande `INSERT OVERWRITE` stocke les résultats de la requête dans le chemin S3 spécifié par `OUTPUT`. Le reste de la requête filtre les données en fonction du chemin du fichier (`SAMPLE`), de la date (`DAY`), et de l'heure (`HOUR` et `NEXT_HOUR`).

### Résumé :
- **SAMPLE** : Chemin des données d'entrée sur S3.
- **DAY, HOUR** : Date et heure de départ pour le filtrage des données.
- **NEXT_DAY, NEXT_HOUR** : Date et heure de fin de filtrage.
- **OUTPUT** : Chemin de sortie pour les résultats de la requête sur S3.

Chaque variable `-d` définit une valeur qui sera utilisée dans une requête Hive pour filtrer les données ou pour spécifier où les résultats seront stockés.


----

La commande Hive  définit plusieurs variables dynamiques avec l'option `-d`, utilisées dans une requête Hive pour filtrer des données et spécifier où les résultats seront stockés :

- **SAMPLE** : chemin S3 des données source.
- **DAY, HOUR** : date et heure de départ.
- **NEXT_DAY, NEXT_HOUR** : date et heure de fin.
- **OUTPUT** : chemin S3 pour stocker les résultats.

Ces variables sont ensuite utilisées dans une requête Hive pour filtrer et traiter des données sur une plage horaire spécifique et exporter les résultats dans S3.

