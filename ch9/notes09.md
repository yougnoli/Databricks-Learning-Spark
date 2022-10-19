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
I Database sono lo strumento pricipe per storare i dati strutturati sotto forma di tabelle. Queste possono essere lette attraverso delle queries in SQL. I dati devono aderire a un certo *scehma* in maniera vincolante.

I carichi di lavoro SQL possono essere divisi in due larghe categorie:
- **OLTP: Online Transaction Processing**, come le transazioni in un conto corrente bancario, sono carichi di lavoro che leggono e aggiornano poche righe alla volta.
- **OLAP: Online Analytical Transactions**, carichi di lavoro su delle queries molto complesse (joins, group by, etc.) che richiedono molti scan su molte righe.

Gli engine di Big Data, come Apache Spark, e disegnato primariamente per carichi di lavoro OLAP.

### Limiti dei Database
Negli ultimi anni si sono visti 2 grandi trend nell'ambito dei carichi di lavoro analitici:
- **Crescita dei dati**: con l'avvento dei Big Data c'e' un trend globale di storare qualsiasi informazione. L'ammontare dei dati collezionati sta crescendo notevolmente (in una decina di anni si e' passati da GB a PetaBytes).
- **Crescita nei diversi tipi di analisi**: con l'aumento dei dati sono aumentate anche le diverse possibilita' di analisi da farci, come machine learning e deep learning.

Con questi nuovi trend i Database non sono del tutto adeguati per accomodare le richieste del mercato. Per via di queste limitazioni:
- **Sono costosi da scalare**:
- **Non supportano particolarmente bene carichi non-SQL**:

Queste limitazioni hanno portato allo sviluppo di un nuovo modo di approcciare lo storing dei dati: *Data Lakes*.

## Data Lakes



## Lakehouses - Delta Lake






















