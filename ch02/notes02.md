# Understanding Spark Application Concepts

#### *Application*
Programma di un utente creato su Spark per usare le sue Api. Consiste nel driver e degli esecutori nel cluster.

#### *SparkSession*
Punto di ingresso per usufruire delle funzionalità di Spark e permette di programmare con le sue API. In una Spark shell interattiva viene creata automaticamente dal driver Spark, in un'applicazione Spark la devo creare io stesso l'oggetto SparkSession.

#### *Job*
Un calcolo eseguito in parallelo formato da più compiti che viene generato in risposta ad un'azione di Spark.

#### *Stage*
Ogni Job viene diviso in piccoli pezzetti di compiti da svolgere che sono collegati l'uno all'altro.

#### *Task*
Una singola unità di lavoro che verrà inviata a uno Spark executor.

## SparkApplication and SparkSession
Al centro di ogni applicazione Spark c'è il **driver** che crea un oggetto **SparkSession**.

![image](https://user-images.githubusercontent.com/77077281/196006602-a5c2d389-80a4-4d14-b71e-7001161fbbf7.png)

Una volta che c'è la SparkSession si può programmare usando le API per performare delle operazioni Spark.

## Spark Jobs
