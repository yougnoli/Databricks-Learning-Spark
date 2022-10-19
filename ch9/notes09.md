# Building Reliable Data Lakes with Apache Spark

## The Importance of an Optimal Storage Solution
Queste sono le proprieta' fondamentali **desiderate** in una soluzione di storage:

- ### Scalabilita' e performance
La soluzione di storage dovrebbe essere in grado di scalare il volume dei dati e provvedere lettura e scrittura di questi senza latenza, in base ai diversi carichi di lavoro.

- ### Supporto nelle transazioni
Carichi di lavoro complessi riguardano spesso la lettura e scrittura in simultanea, quindi sarebbe essenziale il supporto per **ACID Transactions** per assicurare qualita' al risultato finale.

![image](https://user-images.githubusercontent.com/77077281/196636965-901d4f3f-03d5-4c5f-adc5-64ce3cbeb692.png)

**Atomicity**: una transazione o e' eseguita completamente con successo o non eseguita (quindi riportata allo stato precedente). Se il sistema fa crash in mezzo ad una operazione, una volta riacceso il database questo e' riportato allo stato prima che fosse iniziata l'operazione non conclusa.

**Consistency**: mantenere l'integrita' dei dati attraverso i *constraints* (PK, FK, etc.). Avere consistenza significa che se il database entra in uno stato non legale (violazione di un constraint), il processo verra' abortito e i cambiamenti non eseguiti riportando al loro stato precedente (uno stato legale).

**Isolation**: ogni lettura e scrittura non ha impatto su altre letture e scritture di separate transazioni sullo stesso database. (Esempio di ordinare nello stesso momento unbiglietto di un treno).

**Durability**: le operazioni portate a conclusione con successo, sopravviveranno in maniera permanente.

- ### Supporto per diversi tipi di formato di dati
La soluzione di storage dovrebbe avere la possibilita' di storare dati strutturati (tabellari), semistrutturati (JSON, etc.) e non strutturati (file di testo, immagini, etc.).

- ### Supporto per diversi carichi di lavoro
Come ad esempio supporto di carichi di lavoro **SQL** per analisi classiche di BI, supporto di flussi **ETL**, supporto per carichi di lavoro si **dati in Streaming**, supporto per carichi di lavoro di **ML** e **AI**.

- ### Apertura
Diversi carichi di lavoro vuol dire poter storare dati in formati aperti. Questo porta le aziende ad utilizzare i migliori strumenti.

Nel tempo le diverse **soluzioni di storage** si sono evolute, ognuna di queste con i propri vantaggi e svantaggi rispetto alle proprieta' appena elencate.

## Database
I Database sono lo strumento principe per storare i dati strutturati sotto forma di tabelle. Queste possono essere lette attraverso delle queries in SQL. I dati devono aderire a un certo *scehma* in maniera vincolante.

I carichi di lavoro SQL possono essere divisi in due larghe categorie:
- **OLTP: Online Transaction Processing**, come le transazioni in un conto corrente bancario, sono carichi di lavoro che leggono e aggiornano poche righe alla volta.
- **OLAP: Online Analytical Transactions**, carichi di lavoro su delle queries molto complesse (joins, group by, etc.) che richiedono molti scan su molte righe.

Gli engine di Big Data, come Apache Spark, e disegnato primariamente per carichi di lavoro OLAP.

### Limiti dei Database
Negli ultimi anni si sono visti 2 grandi trend nell'ambito dei carichi di lavoro analitici:
- **Crescita dei dati**: con l'avvento dei Big Data c'e' un trend globale di storare qualsiasi informazione. L'ammontare dei dati collezionati sta crescendo notevolmente (in una decina di anni si e' passati da GB a PetaBytes).
- **Crescita nei diversi tipi di analisi**: con l'aumento dei dati sono aumentate anche le diverse possibilita' di analisi da farci, come machine learning e deep learning.

Con questi nuovi trend i Database non sono del tutto adeguati per accomodare le richieste del mercato. Per via di queste limitazioni:
- **Sono costosi da scalare**: nonostante i database siano molto efficienti nel processare i dati su una singola macchina, la grande quantita' di dati e processi implica la necessita' di aumentare il numero di macchine su cui il database deve lavorare e far distribuire il carico in parallelo. Questo processo per i database e' pero non molto comune tra le industrie e dispendioso a livello economico.
- **Non supportano particolarmente bene carichi non-SQL**: come ad esempio carichi di lavoro di Machine Learning che hanno difficolta' ad accedere ai dati storati in tabelle SQL. 

Queste limitazioni hanno portato allo sviluppo di un nuovo modo di approcciare lo storing dei dati: *Data Lakes*.

## Data Lakes
A differenza dai Database i Data Lake sono un sistema di storage distribuito che puo' facilmente essere scalato.
I dati sono salvati come *file* in formato aperto (open format) in modo tale che ogni *engine* possa lavorarci attraverso le standard APIs. 
Le organizzazioni costruiscono i loro Data Lake in maniera indipendente scegliendo tra:
- **Sistema di Storage**: possono scegliere se usare HDFS su un cluster di macchine o utilizzare un'altro oggetto per lo storage sul cloud (AWS S3, Azure Data Lake Gen 2, Google Cloud Storage, etc.).
- **Formato dei files**: dati storati possono essere strutturati, semistrutturati o non strutturati.
- **Engine per processare i dati**: in base al carico di lavoro da eseguire si puo' scegleire un diverso engine per lavorare su quei dati storati.

Questi sono i maggiori vantaggi dei Data Lake sui Database, inoltre in alcuni casi offrono una soluzione piu' economica rispetto i Database.

### Reading from and Writing to Data Lakes using Apache Spark
Apache Spark e' un ottimo strumento per poter costruire un proprio Data Lake, perche' supporta:
- **Diversi carichi di lavoro**: operazioni ETL, SQL, Streaming, Machine Learning, etc.
- **Diversi formati di files**: strutturati, semi strutturati e non strutturati.
- **Diversi filesystem**: accesso a numerosi sistemi di storage che supportano HDFS.

### Limiti dei Data Lakes
I Data Lakes non sono lo strumento di storage perfetto, anche loro hanno delle limitazioni: la piu' importante e' che non garantisce le transazioni, nello specifico non garantisce transazioni ACID su:
- **Atomicity e Isolation**: Gli engine che lavorano con i dati scrivono sul Data Lake sottoforma di files in maniera distribuita. Se questa operazione fallisce, non c'e' meccanisco di *roll back* per i files che sono gia' stati scritti portando ad avere data corrotti. 
- **Consistency**: un errore molto comune e' avere nel Data Lake dei dati che sono stati scritti in un formato o in uno schema inconsistente con i dati gia' esistenti.

Per far fronte a questi problemi ci sono alcuni trucchetti da poter applicare:
- **Partizioni**: si e' soliti partizionare in cartelle e sotto cartelle dei file molto grandi per evitare modifiche o errori sull'intero file.
- **Scheduling dei job ETL**: esecuzioni di processi ETL o di queries sono spesso schedulate in maniera sfalsata per evitare inconsistsenza ed accesso multiplo sui dati.

## Lakehouses - Delta Lake
Il *Lakehouse* e' il nuovo paradigma per i migliori elementi del Database OLAP DataWarehouse e del Data Lake. Queste sono le sue principali caratteristiche:
- **Supporto nelle Transazioni**: come nei Database, Lakehouse grantisce ACID in presenza di carichi di lavoro simultanei. 
- **Schema**: Lakehouse previene l'inserimento di dati in una tabella con uno schema che non e' corretto. Lo schema puo' essere anche evoluto, in base ai nuovi dati in entrata.
- **Supporto per diversi data type e formati**: come i Data Lake, ma a differenza dei Database il Lakehouse puo' storare, analizzare, leggere e scrivere tutti i dati che siano strutturati, semi strutturati e non strutturati.
- **Supporto per diversi carichi di lavoro**: Lakehouse permette di lavorare con diversi carichi di lavoro: dalle tradizioni queries SQL al Machine Learning e Streaming.
- **Supporto per update e delete**: Lakehouse garantisce l'aggiornamento e l'eliminazione di dati anche se in simultanea.
- **Data Governance**: Lakehouse fornisce data integrity.

Sono presenti dei sistemi open source che permettono di costruire dei Lakehouse con queste proprieta', tra questi: **Delta Lake**.

### Delta Lake 
E' un progetto open source che supporta:
- **Streaming**: lettura e scrittura in tabelle per fonti dato i streaming.
- **Operations**: operazioni di *update*, *delete*, *merge* anche con diverse APIs.
- **Schema**: evoluzione dello schema alterandolo esplicitamente o implicitamente.
- **Time Travel**: permette l'interrogazione di tabelle di uno specifico snapshot (per ID) o nel tempo (per timestamp)
- **Rollback**: permette di tornare a versioni precedenti di tabelle per correggere gli errori.
- **Isolation**: isolazione su scritture in simultanea.

Delta Lake ha un'ottima integrazione con le fonti dato di Apache Spark.
