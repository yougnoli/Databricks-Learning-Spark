# Introduction to Apache Spark: a Unified Analytics Engine

## Big Data and Distributed Computing at Google
Tutto e' nato dalla necessita' di Google, nel 2003, di avere un motore di ricerca che potesse indicizzare e trovare molto velocemente i dati testuali su internet. Gli storage tradizionali come i relational database management system (RDBMSs) e modalita' di programmazione imperativa non potevano permettersi di gestire la quantita' e la velocita' di risorse (di documenti indicizzati) che voleva Google. Questo porto' alla creazione del *Google Fyle System* (GFS), *MapReduce* (MR) e *BigTable*.

- **GFS** e' un filesystem tollerante agli errori e distribuito tra una famiglia di server che compongono un cluster di macchine.
- **BigTable** offre uno storage scalabile tra il filesystem GFS.
- **MR** permette un processo di dati di larga scala distribuito sul GFS e su BigTable.

Il funzionamento di **MapReduce** prevede che delle funzioni di mapping, rimescolamento, ordinamento e riduzione vengano eseguito la' dove i dati risiedono. I lavoratori nel cluster aggregano e riducono i dati e producono un output che verra' poi scritto e distribuito nello storage della mia applicazione.

![image](https://user-images.githubusercontent.com/77077281/195302719-2ed2d07c-6378-4dd4-b6b3-c9ad541d206b.png)

Questa idea porto' ad ispirare *Yahoo!* che proprio in quel periodo stava affrontando delle sfide simili in ambito big data per il suo motore di ricerca.

Le sfide di grandi calcoli distribuiti espressi grazie al GFS produsse un progetto autonomo chiamato *Hadoop File System* (HDFS) che includeva il MapReduce come struttura del calcolo distribuito. Donato nel 2006 alla *Apache Software Foundation* (ASF) divenne parte di quello che e' *Apache Hadoop*.

![image](https://user-images.githubusercontent.com/77077281/195305080-71a6a068-5d72-4643-bb06-c21bb4dc76f0.png)

Nonostante la sua grande diffusione al di fuori di Yahoo! MapReduce su HDFS aveva alcune problematiche:
- era difficile da gestire ed amministrare
- ogni task del MR era scritto su disco portando una grande dispersione di tempo per dei job molto grandi
- era difficile poter combiare diversi carichi di lavoro come machine learning, streaming e queries interattive di tipo SQL.

Ricercatori dell'Universita' di Berkeley in California accettarono la sfida di migliorare Hadoop MapRaduce con un progetto chiamato *Spark*. 

Oggi Spark prende in prestito l'idea di Hadoop del MapReduce, rendendolo:
- altamente tollerante agli errori
- supporta lo storage di memoria interna tra un risultato intermedio e l'altro delle computazioni di mapping e riduzione
- offre API per diverse tipologie di linguaggi.

Alcuni dei creatori e ricercatori del progetto Spark hanno poi fondato una compagnia chiamata *Databricks*. Databricks e sviluppatori della community open source hanno lavorato insieme per rilasciare **Apache Spark 1.0** a maggio del 2014.

## What is Apache Spark?
Apache Spark e' un motore designato per lavorare e processare con grosse quantita' di dati, che siano on premises o in cloud. Incorpora delle librerie per **Machine Learning (MLlib)**, per queries SQL interattive (**Spark SQL**), processi di streaming dati (**Structured Streaming**) per interagire con dati real time e processi per dataabse grafi (**GraphX**).

Spark ha 4 caratteristiche chiave:
- Velocita'
- Facilita'
- Modularita'
- Estensibilita'

### Velocita'
Spark costruisce la sua capacita' di calcolo come un grafico aciclico diretto (**DAG**). 

![image](https://user-images.githubusercontent.com/77077281/195355179-f5a0be11-73e4-4866-9c99-707315c44575.png)

Questo grafo computazionale e' solitamente scomposto in task minori che vengono eseguiti in parallelo tra i vari workers del cluster.

### Facilita'
Spark raggiunge la sua semplicita' grazie all'astrazione del dato strutturato sotto forma di Resilient Distributed Dataset (**RDD**). Ogni tipologia di dato strutturato come DataFrames o Datasets si appoggiano e sono costruiti al di sopra degli RDD.

### Modularita'
Le operazioni eseguibili in Spark possono essere fatte in diversi tipi di carichi di lavoro ed espressi in diversi linguaggi di programmazione: Scala, Java, R, Python, SQL. Unisce sotto un unico engine, diversi carichi di lavoro.

### Estensibilita'
A differenza di Apache Hadoop che includeva sia lo storage che la parte di calcolo, Spark puo' leggere dati provenienti da moltissime fonte dato. Spark si concentra su quello che e' calcolo computazionale in parallelo e veloce.

## Unified Analytics
E' possibile scrivere codice in Spark attraverso le diverse APIs che mette a disposizione: Scala, Java, R, Python, SQL. In realta' il codice che abbiamo scritto nel nostro linguaggio preferito avviene una scomposizione in *bytecode* che possa essere letto ed eseguito dai workers del nostro cluster (che sono in realta delle Java Virtual Machine).

![image](https://user-images.githubusercontent.com/77077281/195358506-a973d744-7e76-4ba7-9730-7a0ceb3b5ba2.png)

### SparkSQL
Lavora bene con dati strutturati. Possiamo leggere dati che sono storati in tabelle di RDBMS o da diversi tipi di formati di file (CSV, Parquet, Json, Avro, ORC, etc.) per poi costruire delle persistenti o temporanee in Spark. Possiamo combinare queries SQL in dati che si trovano dentro dei DataFrames Spark.

### Spark MLlib
Libreria che permette di avere diversi algoritmi di Machine Learning. E' possibile estrarre e trasformare features, costruire pipeline per il training e il testing, costruire modelli in maniera persistente per poi ricaricarli.

### Spark Structured Streaming
Per sviluppatori di Big Data che hanno bisogno di combinare e interrogare dati da fontiin tempo reale a tabelle che dinamicamente continuano a crescere. E' possibile utilizzare queste tabelle come se fossero tabelle strutturate e statiche.

### GrapX
Libreria costruita per manipolare i Grafi e performare calcoli su di essi in parallelo. Offre gli algoritmi standard per analisi e connessioni sui grafi.

## Apache Spark’s Distributed Execution

















