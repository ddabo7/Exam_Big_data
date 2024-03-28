# R6.01 Big Data : enjeux, stockage et extraction

## TD2 

### HDFS

### 3.1 Prise en main Commandes HDFS
-> commande : 
```sh
hdfs
```
retourn : 
```sh
where COMMAND is one of:
  dfs                  run a filesystem command on the file systems supported in Hadoop.
  classpath            prints the classpath
  namenode -format     format the DFS filesystem
  secondarynamenode    run the DFS secondary namenode
  namenode             run the DFS namenode
  journalnode          run the DFS journalnode
  zkfc                 run the ZK Failover Controller daemon
  datanode             run a DFS datanode
  dfsadmin             run a DFS admin client
  envvars              display computed Hadoop environment variables
  haadmin              run a DFS HA admin client
  fsck                 run a DFS filesystem checking utility
  balancer             run a cluster balancing utility
  jmxget               get JMX exported values from NameNode or DataNode.
  mover                run a utility to move block replicas across
                       storage types
  oiv                  apply the offline fsimage viewer to an fsimage
  oiv_legacy           apply the offline fsimage viewer to an legacy fsimage
  oev                  apply the offline edits viewer to an edits file
  fetchdt              fetch a delegation token from the NameNode
  getconf              get config values from configuration
  groups               get the groups which users belong to
  snapshotDiff         diff two snapshots of a directory or diff the
                       current directory contents with a snapshot
  lsSnapshottableDir   list all snapshottable dirs owned by the current user
                                                Use -help to see options
  portmap              run a portmap service
  nfs3                 run an NFS version 3 gateway
  cacheadmin           configure the HDFS cache
  crypto               configure HDFS encryption zones
  storagepolicies      list/get/set block storage policies
  version              print the version
  ```

### -> commande : dfs 
retourn : 
```command not found```

### difference ? 
Il y en a une qui marche et l'autre non... 

### -> Commande : hadoop version / hdfs version
-> hadoop version 
retourne : 
```
Hadoop 2.7.3.2.6.5.0-292
Subversion git@github.com:hortonworks/hadoop.git -r 3091053c59a62c82d82c9f778c48bde5ef0a89a1
Compiled by jenkins on 2018-05-11T07:53Z
Compiled with protoc 2.5.0
From source with checksum abed71da5bc89062f6f6711179f2058
This command was run using /usr/hdp/2.6.5.0-292/hadoop/hadoop-common-2.7.3.2.6.5.0-292.jar
```
-> hdfs version
retourne : 
```
Hadoop 2.7.3.2.6.5.0-292
Subversion git@github.com:hortonworks/hadoop.git -r 3091053c59a62c82d82c9f778c48bde5ef0a89a1
Compiled by jenkins on 2018-05-11T07:53Z
Compiled with protoc 2.5.0
From source with checksum abed71da5bc89062f6f6711179f2058
This command was run using /usr/hdp/2.6.5.0-292/hadoop/hadoop-common-2.7.3.2.6.5.0-292.jar
```

///// Quelle est la version hadoop de sandbox 2.6.5?


### 3.2

1) La commande hdfs dfs -ls est utilisée pour lister l'ensemble des fichiers présents dans le répertoire utilisateur du système de fichiers distribué Hadoop (HDFS). Lorsque vous exécutez cette commande et qu'aucun fichier n'est présent dans le répertoire utilisateur spécifié, elle n'affiche aucun résultat, car il n'y a pas de fichier à lister.

2) Quel est la commande pour lire le contenue de /user? 
hdfs dfs -ls /user

 3) Que ce passe-il si vous refaite toute les commandes précedentes avec hadoop fs au lieux de hdfs dfs?
Error: Could not find or load main class fs

4) Créez localement un fichier texte monfichier.txt, modifiez son contenu, sauvegardez et quittez:
5. touch monfichier.txt
6. echo "Ceci est un test" >> monfichier.txt 

7) Copiez ce fichier sur HDFS par hdfs dfs -put monfichier.txt. Utilisez hdfs dfs -ls -R pour vérifier.
hdfs dfs -put monfichier.txt
hdfs dfs -ls -R

5) Si vous voulez envoyer vos données vers HDFS sans garder une copie en local :

hdfs dfs -moveFromLocal monfichier.txt

### 3.3 Manipulation des données dans HDFS

1) Affichez le contenu du fichier créer mais sur HDFS

hdfs dfs -cat monfichier.txt

2) Supprimer un fichier depuis le système de fichiers HDFS :

```hadoop fs -rm monfichier.txt``` 

3) Créez localement un dossier nommé data et envoyez-le sur HDFS.

```mkdir data``` : La commande `mkdir data` crée un répertoire appelé "data" dans le système de fichiers HDFS s'il n'existe pas déjà. Cela fournit un emplacement où stocker des données sur HDFS.
```hdfs dfs -put data``` : `hdfs dfs -put data` prend le contenu du répertoire local "data" (qui a été créé ou existe déjà localement) et le transfère vers le répertoire "data" sur HDFS. Cela permet de copier les données du système de fichiers local vers le système de fichiers distribué Hadoop pour une utilisation ultérieure dans un environnement distribué.
```hdfs dfs -ls -R``` : `hdfs dfs -ls -R` est une commande pour lister le contenu du répertoire courant de manière récursive sur HDFS. Cela affichera la structure et les fichiers présents dans le répertoire "data" ainsi que tout son contenu récursivement. Cela permet de vérifier que les données ont été correctement transférées sur HDFS et de voir leur disposition dans la structure du système de fichiers distribué. 

4) Copiez le fichier monfichier.txt dans le répertoire data à l’aide de la commande -cp (vérifiez).

 hdfs dfs -cp monfichier.txt data

5) Créez un dossier datasets dans le dossier data, puis déplacez monfichier.txt dans datasets à l’aide de la commande -mv, décrivez vos commandes.

hdfs dfs -mkdir /data/dataset

6) Créez un dossier datasets dans le dossier data, puis déplacez monfichier.txt dans datasets à l’aide de la commande -mv, décrivez vos commandes.

 hdfs dfs -mv data/monfichier.txt data/dataset

7) Créer une copie de monfichier.txt dans le répertoire data sous le nom copiedemonfichier.txt.

hdfs dfs -cp monfichier.txt data/copiedemonfichier.txt

8) Avant de lancer cette commande, il faut vérifier que l’espace local disponible est suffisant pour recevoir les données HDFS, décrivez vos commandes.

hdfs dfs -df -h

9) Si on veut supprimer un répertoire depuis le système de fichiers HDFS
    
hdfs dfs -rmdir test_d

10) Une commande qui vous permet de voir « l’état de santé » de votre HDFS (elle vérifie les incohérences : blocks manquants, nom de réplicas insufusants,…) : hdfs fsck /user

### 3.4 Manipulation de fichiers télécharger depuis un serveur

1) Pour ce faire il vous faut la commande wget. Si il y'a une erreur va apparaitre, décrivez comment vous avez pu faire pour télécharger le fichier.(Commentez la raison pour laqeulle l'erreur a lieu)

wget https://files.grouplens.org/datasets/movielens/ml-1m.zip

2) Décompressez le fichier zip
unzip ml-1m.zip

3) Créez un répértoire /datasets/movies en local et sur hdfs
   a) Local 

mkdir datasets
mkdir datasets/movies

    b) HDFS

hdfs dfs -mkdir datasets
hdfs dfs -mkdir datasets/movies

### Déroulez les étapes de création des deux dossier /datasets/movies et la copie du fichier rating.dat à partir du système local vers HDFS (dans movies).
### 1. Téléchargement du fichier 'ml_1m.zip' dans mon repertoire local datasets/movies
> ***Remarque :*** Après avoir nettoyé mon dossier local pour assurer un environnement de travail propre, j'ai téléchargé à nouveau le fichier ml_1m.zip. Cette fois-ci, je l'ai mis directement dans mon fichier local datasets/movies 

***-> Commande :***
```sh 
wget -P ~/datasets/movies https://files.grouplens.org/datasets/movielens/ml-1m.zip
```
***-> Réponse :***
```sh
[maria_dev@sandbox-hdp movies]$ ls ~/datasets/movies
ml-1m.zip
```
### 2. Dézipage du fichier 'ml_1m.zip'

> ***Remarque :*** Après avoir terminer avec cela, je dézipe le fichier

***-> Commande :***
```sh 
unzip /home/maria_dev/datasets/movies/ml-1m.zip -d /home/maria_dev/datasets/movies/
```
***-> Réponse :***
```sh
Archive:  /home/maria_dev/datasets/movies/ml-1m.zip
   creating: /home/maria_dev/datasets/movies/ml-1m/
  inflating: /home/maria_dev/datasets/movies/ml-1m/movies.dat
  inflating: /home/maria_dev/datasets/movies/ml-1m/ratings.dat
  inflating: /home/maria_dev/datasets/movies/ml-1m/README
  inflating: /home/maria_dev/datasets/movies/ml-1m/users.dat
```
### 3. copie du fichier rating.dat à partir du système local 'datasets/movies' vers HDFS (dans movies).

***-> Commande :***
```sh 
hdfs dfs -put /home/maria_dev/datasets/movies/ml-1m/ratings.dat /user/maria_dev/datasets/movies/
```
***-> Réponse :***
> Vérification : 'hdfs dfs -ls /user/maria_dev/datasets/movies/'

```sh
Found 1 items
-rw-r--r--   1 maria_dev hdfs   24594131 2024-03-27 14:34 /user/maria_dev/datasets/movies/ratings.dat
```

### 4. Affichez combien de blocs occupe le fichier avec la commande hdfs fsck [chemin vers votre fichier] -files -blocks (Commentez!)

***-> Commande :***
```sh 
hdfs fsck /user/maria_dev/datasets/movies/ratings.dat -files -blocks
```
***-> Réponse :***

> ***Remarque :*** Le système de fichiers distribué HDFS divise les fichiers en blocs pour une distribution et un stockage efficaces sur le cluster. Par défaut, la taille d'un bloc est de 128 Mo sur HDFS 2.x et plus. Lorsque nous utilisons la commande 'fsck', elle nous montre combien de blocs sont utilisés pour stocker le fichier 'ratings.dat'. Si le fichier est volumineux, il sera divisé en plusieurs blocs de taille égale ou inférieure à la taille de bloc définie par défaut.

```sh
Connecting to namenode via http://sandbox-hdp.hortonworks.com:50070/fsck?ugi=maria_dev&files=1&blocks=1&path=%2Fuser%2Fmaria_dev%2Fdatasets%2Fmovies%2Fratings.dat
FSCK started by maria_dev (auth:SIMPLE) from /172.18.0.2 for path /user/maria_dev/datasets/movies/ratings.dat at Wed Mar 27 14:37:05 UTC 2024
/user/maria_dev/datasets/movies/ratings.dat 24594131 bytes, 1 block(s):  OK
0. BP-243674277-172.17.0.2-1529333510191:blk_1073743070_2255 len=24594131 repl=1

Status: HEALTHY
 Total size:    24594131 B
 Total dirs:    0
 Total files:   1
 Total symlinks:                0
 Total blocks (validated):      1 (avg. block size 24594131 B)
 Minimally replicated blocks:   1 (100.0 %)
 Over-replicated blocks:        0 (0.0 %)
 Under-replicated blocks:       0 (0.0 %)
 Mis-replicated blocks:         0 (0.0 %)
 Default replication factor:    1
 Average block replication:     1.0
 Corrupt blocks:                0
 Missing replicas:              0 (0.0 %)
 Number of data-nodes:          1
 Number of racks:               1
FSCK ended at Wed Mar 27 14:37:05 UTC 2024 in 8 milliseconds
```

> ***Remarque :*** 
Ce fichier occupe une taille de 24 594 131 octets, ce qui équivaut à environ 23,45 Mo. Il est stocké dans un seul bloc, comme indiqué par la commande qui renvoie 'Total blocks (validated): 1 (taille moyenne de bloc : 24 594 131 octets)'.

5) Pour voir la décomposition d’un fichier en plusieurs blocs, récupérez le fichier zip MovieLens 25M Dataset.

a) Récuperez le fichiers qui se trouve
wget https://files.grouplens.org/datasets/movielens/ml-25m.zip

b) Décompression du fichier
unzip ml-25m.zip

c) Création du dossier d'accueil sur HDFS
hdfs dfs -ls datasets/movies25

d) Copie Local vers HDFS 
hdfs dfs -put ml-25m/ratings.csv datasets/movies25/

e) Nombre de blocks utiliser
hdfs fsck datasets/movies25/ratings.csv -files -blocks
6 blocs occupé par le fichier

### 3.5 Fichiers de configuration HDFS

1) Consultez le contenu de ce fichier.

a) Visualisation du fichier:
cat /etc/hadoop/conf/hdfs-site.xml

b) Valeur du paramètre dfs.replication.
La valeur de ce paramètre est 1
    <property>
      <name>dfs.replication</name>
      <value>1</value>
    </property>

c) Vous pouvez afficher la valeur de réplication directement par la commande
On obtient 1
1) La taille du bloc : HDFS stocke les fichiers dans le cluster en les décomposant en blocs de taille fixe.
La taille du bloc est 134217728 Bytes pour le paramètre dfs.blocksize:

    <property>
      <name>dfs.blocksize</name>
      <value>134217728</value>
    </property>

1) Nous pouvons voir la taille du bloc directement par une commande
hdfs getconf -confkey dfs.blocksize
On obtient 134217728

1) Vous pouvez changer la taille du bloc pour un fichier par la commande :

hdfs dfs -D dfs.blocksize=67108864 -put ml-25m/ratings.csv

hdfs fsck ratings.csv -files -blocks

### 4 Hadoop
On se met admin sudo su root 
## 4.1. Préparation de la vm (MrJob, Python ...)

### 4.1.1. Mise à jour de la SandBox HDP

```
sudo su root
```

> ***Explication :***
Pour installer les packages ci-dessous, nous devons passer par l'utilisateur root, car je n'ai pas les droits d'administration sur l'environnement maria_dev. L'utilisation de la commande sudo me permet d'exécuter temporairement cette commande en tant qu'utilisateur root, en fournissant le mot de passe associé à ce compte lorsque cela est requis. Cela me permet d'installer les packages nécessaires sans avoir à modifier les autorisations ou à demander une assistance supplémentaire à l'administrateur système.




```
yum-config-manager --save --setopt=HDP-SOLR-2.6-100.skip_if_unavailable=true

yum install https://repo.ius.io/ius-release-el7.rpm https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

> ***Explication :***
Nous procédons à une mise à jour de la sandbox mentionnée précédemment afin de permettre l'installation ultérieure de versions à jour de Python. Cette étape est nécessaire pour garantir la compatibilité et bénéficier des dernières fonctionnalités et correctifs de sécurité disponibles dans les versions récentes de Python.

### 4.1.2. Installation de python-pip:

```
yum install python-pip
```
> ***Explication :***
Nous installons python via cette commande ci-dessus.

### 4.1.3. Instalation de de MrJob

```
pip install pathlib
pip install mrjob==0.7.4
pip install PyYAML==5.4.1
```

> ***Explication :***
Ces commandes permettent d'installer MrJob, une bibliothèque Python qui facilite le développement et le test de programmes MapReduce. En utilisant MrJob, nous pouvons écrire et tester du code Python pour des tâches MapReduce localement, sans avoir besoin d'installer Hadoop, le framework habituellement utilisé pour exécuter des jobs MapReduce à grande échelle. Cette approche simplifie le processus de développement et de débogage des applications MapReduce en permettant aux développeurs de travailler sur des données de petite à moyenne taille sur leur propre machine, avant de les déployer sur un cluster Hadoop pour un traitement à grande échelle.

### 4.1.4 Installation de Nano

```
yum install nano
```

> ***Explication :***
Nous installons ici Nano qui est un éditeur de texte en ligne de commande, léger et facile à utiliser.
## 4.2 Execution du MapReduce en local
On revient sur l'utilisateur maria_dev

su maria_dev

### 4.2.1. Récuperation du code et des données

1) Telechargement du contenu:
   a) Télechargement du fichier python
wget https://github.com/juba-agoun/iut-hadoop/raw/main/filmEvaluation.py
    b) Télechargement du fichier des données
wget https://github.com/juba-agoun/iut-hadoop/raw/main/evaluation.data

2) MapReduce en Local
python filmEvaluation.py evaluation.data

### 4.3 Execution du MapReduce sur Hadoop
hdfs dfs -mkdir MapReduce

hdfs dfs -put *.data MapReduce

python filmEvaluation.py -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar hdfs:///user/maria_dev/MapReduce/evaluation.data

## 5 Mise en pratique (Examen)

1. Trouvez combien de tags chaque film possède?
On edite le fichier python via WinSCP 



hdfs dfs -mkdir MapReduce/Exam

hdfs dfs -put tagechantillon.data MapReduce/Exam
python [filmTag.py ](https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/filmTag_unique.py)-r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar hdfs:///user/maria_dev/MapReduce/Exam/tagechantillon.data

hdfs dfs -put ml-25m/tags.csv MapReduce/Exam/
python [filmTag.py ](https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/filmTag_unique.py)-r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar hdfs:///user/maria_dev/MapReduce/Exam/tags.csv -o MapReduce/Exam/out/tags

2. Trouvez combien de tags chaque utilisateur a ajoutés?
python [utilisateurTags.py](https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/utilisateurTags.py) -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar hdfs:///user/maria_dev/MapReduce/Exam/tags.csv -o MapReduce/Exam/out/tags_by_user

mkdir out/tags_by_user/

hdfs dfs -copyToLocal MapReduce/Exam/out/tags_by_user/* out/tags_by_user/

Resultat 
https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/out/tags_by_user/part-00000

3. Bloc
hdfs fsck MapReduce/Exam/tags.csv
a) défault

b) 64Mb


4. Tags and films
python [filmTag_unique.py](https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/filmTag_unique.py) -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar hdfs:///user/maria_dev/MapReduce/Exam/tags.csv -o MapReduce/Exam/out/tags_and_films

Resultat : 
https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/out/films_and_tags/part-00000

hdfs dfs -copyToLocal MapReduce/Exam/out/tags/* out/tags_by_films/

Resultat : 
https://github.com/ddabo7/Exam_Big_data/blob/fcc6d5b2a7d40e90172db925facf87490668a033/out/tags_by_films/part-00000