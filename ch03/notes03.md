# Apache Spark's Structured API's

## What are RDDs?
RDD sta per **Resilient Distributed Dataset** e ha diverse caratteristiche fondamentali.
- sono un'**astrazione distribuita di dati** su un intero cluster. Ad esempio possiamo avere un dataset molto grande che puo' essere diviso e partizionato (le partizioni possono essere di diverse dimensioni). Su ogni partizione possiamo eseguirci funzioni lambda, SQL queries, trasformazioni che verranno eseguite in parallelo.

![image](https://user-images.githubusercontent.com/77077281/195409023-5522657d-916f-4d08-a3f1-3db4eb0e2fc0.png)

- sono **resilienti ed immutabili**, resiliente perche' un RDD puo' essere ricreato in qualsiasi momento. La creazione di un RDD che poi viene trasformato attraverso delle operazioni in un altro RDD e in un altro ancora crea una *discendenza* che viene registrata. Se qualcosa va storto l'RDD ha la possibilita' di ricrearsi. Immutabile perche' ogni RDD viene creato da un RDD precedente: non viene modificato l'originale anzi rimane inalterato.

![image](https://user-images.githubusercontent.com/77077281/195409369-25ac7839-d030-4f06-aed3-ff176a9fd14b.png)

- sono **type safe**, perche' esistono specifici RDD di integers, di strings or binary.
- possono essere **strutturati, semistrutturati o non strutturati**.
- sono **lazy**, cioe' non vengono materializzati fino a quando non avviene una certa *azione* su un particolare RDD. Spark riesce quindi a fondere insieme tutte le precedenti *trasformazioni* e solo con un'azione materializza veramente l'RDD.

![image](https://user-images.githubusercontent.com/77077281/195411179-93f82a46-1dfe-4f7c-a578-5eb13d818dd2.png)

## Why Use RDDs?
- Agli sviluppatori piace controllare esattamente quello che sta succedendo, quindi offre controllo e flessibilita'
- basso livello di utilizzo di API 
- non ho la necessita' di utilizzare le performance che offre Spark
- non mi interessa lo schema o la struttura dei dati con cui sto lavorando
- incoraggiano il *come fare le cose* direttamente a Spark

### Codice per costruire un RDD
![image](https://user-images.githubusercontent.com/77077281/195412173-f661452b-0023-4c0f-ba6a-7780532eb0d7.png)

## What is the problem?
- Sto dicendo a Spark su come fare le cose e non **quello che deve** fare!
- per il motivo precedente non sto utilizzando le ottimizzazioni di Spark
- RDDs non sono particolarmente efficienti con il linguaggio Python
- Spark non sa quello che deve fare perche' non sto utilizzando delle API di alto livello

Sapendo quello che **deve fare** attraverso un alto livello di API, Spark riesce a utilizzare in maniera ottimale e performante le funzioni o le query su quei dati che stiamo lavorando.

## Spark: What's Underneath an RDD?
Gli RDD hanno 3 vitali caratteristiche:
- **Dependencies**: creare una discendenza di trasformazioni, che permette di arrivare dal punto A al punto B perche' e' stata registrata una serie di trasformazioni.
- **Partitions** e il luogo dove queste sono state storate.
- A un RDD e' associata una **compute function** che presa una particolare partizione, crea un iteratore che svolgera' tutte le funzioni o trasformazioni in parallelo su tutte le partizioni del cluster, per poi restituire tutti i risultati.

Quindi in ognuna di queste caratteristiche degli RDDs ci sono delle zone d'ombra per Spark: 
- Spark non sa quello che avviene all'interno delle compute functions
- Spark non sa il datat type dell'iteratore, sa solo che e; un generico oggetto di Python
- Spark, porpio perche' non sa tutte queste cose non riesce ad ottimizzare le sue performance

**Allora quale e' la soluzione?**

## Structuring Spark
Da Spark 2.x sono stati implementati delle espressioni di computazioni comuni nel mondo dell'analisi dati: filtering, selecting, counting, grouping, etc. Questo aggiunge chiarezza e semplicita'. Attraverso i linguaggi supportati in Spark (Java, Python, Spark, R, e SQL) riesce a capirequello che deve fare con i dati e come risultato un ottimo piano di esecuzione. Inoltre la possibilita' di riarrangiare i dati sottoforma tabulare. Questa nuova struttura restituisce migliori performance e miglior gestione dello spazio tra i vari componenti di Spark. 

Ad esempio se volessimo aggregare tutte le eta' per ogni nome e poi farne la media. Usando un RDD API il codice sarebbe cosi:
![image](https://user-images.githubusercontent.com/77077281/195423023-e7afa6bb-1881-4ce6-a5a4-22013ada030c.png)

Questo codice dice a Spark **come fare** per aggregare le chiavi e fare il calcolo della media con le funzioni lambda. E' criptico e difficile.

Questo e' il codice usando un DataFrame API, instruendo Spark su quello che **deve fare**.
![image](https://user-images.githubusercontent.com/77077281/195423442-c20ee204-a6f7-487b-9737-9244b92f16ce.png)
![image](https://user-images.githubusercontent.com/77077281/195423474-4f018aae-a259-4d1b-accc-1f5c08de0b57.png)

In questo modo Spark puo' ispezionare il nostro codice e capire quello che vogliamo fare, portando l'esecuzione a miglior ottimizzazione e performance.
