**Il file è in fase di scrittura**

## I modelli ISO/OSI e TCP/IP
### Introduzione
ISO/OSI e TCP/IP rappresentano due modelli di comunicazione tra dispositivi in una rete. Sono caratterizzati entrambi da una suddivisione in livelli.
Dividere in funzionalità logiche minori un problema o una tecnologia è una buona e fondamentale pratica nell'informatica per un facile mantenimento, uso ed espansibilità. La scelta della divisione in livelli si basa proprio su queste motivazioni:
- Separando le funzioni della rete in moduli logici più piccoli è più facile risolvere problemi tramite il concetto del *Divide et impera*;
- Attraverso la modularizzazione e un'interfaccia pulita si può avere una standardizzazione nelle interazioni tra dispositivi;
- Permette di cambiare un livello senza toccare gli altri (per esempio si può passare da Ethernet a Wi-Fi senza cambiare HTTP);
- La divisione in moduli inoltre permette di aggiungere facilmente nuovi livelli all'architettura.

Il modello ISO è stato definito dall'ISO come modello di riferimenti ed è diviso in 7 livelli. Il modello TCP/IP invece è caratterizzato da 4 livelli (è dunque più semplice dell'ISO/OSI) ed è un modello pratico fondato su un'implementazione ampiamente utilizzata delle funzioni della rete.

### I 7 livelli del modello ISO/OSI

| Livello        | Funzione                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Applicazione   | Costituisce l'interfaccia e fornisce i servizi di rete direttamente alle applicazioni software dell'utente. A questo livello operano i protocolli che definiscono le regole di comunicazione per i servizi di rete, come HTTP/HTTPS per il web e il DNS per la risoluzione dei nomi dei dominio.                                                                                                                                                       |
| Presentazione  | È il responsabile della formattazione delle informazioni (ad esempio attraverso compressioni e crittografia). Di questo livello fanno parte SST/TLS, JPEG, MP4.                                                                                                                                                                                                                                                                                        |
| Sessione       | Stabilisce, gestisce e termina le sessioni. Controlla il dialogo tra due programmi, aggiunge punti di controllo in caso di interruzione di rete e gestisce la chiusura della sessione in modo ordinato. In applicazioni reali possiamo trovare, appartenenti a questo livello, NetBIOS e RPC.                                                                                                                          |
| Trasporto      | Consegna e controlla i messaggi da processo a processo dividendo i flussi di dati in pezzi più piccoli (*segment* per TCP o *datagram* per UDP) e decidendo come inviarli. Si occupa dell'affidabilità e della gestione dell'ordine tramite il protocollo TCP, oppure offre una consegna veloce ma non garantita tramite UDP. Gestisce inoltre il controllo di flusso e utilizza le porte per indirizzare correttamente i dati. |
| Rete (Network) | Si occupa dell'indirizzamento e del percorso. Determina il percorso logico migliore per far arrivare a destinazione i dati. In questo livello troviamo i router e l'indirizzo IP.                                                                                                                                                                                                                                                                      |
| Data Link      | Organizza i bit in strutture dati e rileva eventuali alterazioni, scartando i frame danneggiati. I pacchetti vengono incapsulati in frame contenenti l'indirizzo MAC del mittente e del prossimo dispositivo sullo stesso collegamento (che cambiano passando da un dispositivo a un altro.). Esempi per questo livello sono l'indirizzo MAC, l'Ethernet, il Wi-Fi e gli switch.                                               |
| Fisico         | Si tratta del mezzo fisico attraverso il quale avviene il passaggio dei bit come segnali. In questa categoria troviamo la fibra ottica, i cavi ethernet e le onde Wi-Fi.                                                                                                                                                                                                                                                                               |

### Il trasferimento dei dati
Quando un dispositivo invia i dati a un altro avviene un incapsulamento dei dati che avviene dall'alto verso il basso (dal livello applicativo al fisico). Una volta raggiunti a destinazione i dati vengono estratti risalendo il modello dal livello più basso (il fisico) fino al più alto (l'applicativo). Ogni livello può comunicare solo con un livello appena sopra o appena sotto durante l'incapsulamento o il decapsulamento dei dati mentre dialoga con un livello della stessa altezza nello scambio di dati tra due dispositivi.

#### I protocolli
Il modello è puramente teorico e generale, non definisce uno specifico metodo di comunicazione che viene invece definito dai vari protocolli. I vari esempi della tabella infatti sono applicazioni particolari del modello al web o ad altri contesti e non devono limitare il senso più ampio che il modello ISO/OSI deve fornire.

Un protocollo è una procedura e un formato standard che due dispositivi in comunicazione devono capire e accettare di usare per potersi parlare a vicenda.

#### Protocol Data Unit (PDU)
I livelli si scambiano i dati tramite l'incapsulamento dei dati.
Ogni livello è responsabile dell'aggiunta di un header o di un footer ai dati che vengono trasferiti. Il processo di incapsulamento crea un *Protocol Data Unit* (PDU) che include i dati mandati insieme agli header o ai footer aggiunti dal livello corrente e da quelli precedenti.

Ad esempio, per una pagina web su Ethernet, i protocolli standard utilizzati sono:
- HTTP, usato per un primo incapsulamento a livello applicativo. I dati prendono il nome generico di *data* (dati).
- TCP, aggiunto al livello di trasporto. Il prodotto è chiamato *segment* (segmento).
- IP, usato nel livello di rete (network). I dati così incapsulati prendono il nome di *packet* (pacchetto).
- Ethernet, viene aggiunto al livello di Data Link. Si genera così un *frame*.
