**Disclaimer:** La seguente presentazione è in continua evoluzione e miglioramento con sezioni da ampliare ancora molto, si cercherà di migliorarla quanto prima.
**Data ultima modifica:** 07/11/2026
## I modelli ISO/OSI e TCP/IP

### Introduzione
ISO/OSI e TCP/IP rappresentano due modelli di comunicazione tra dispositivi in una rete. Sono caratterizzati entrambi da una suddivisione in livelli.
Dividere in funzionalità logiche minori un problema o una tecnologia è una buona e fondamentale pratica nell'informatica per un facile mantenimento, uso ed espansibilità. La scelta della divisione in livelli si basa proprio su queste motivazioni:
- Separando le funzioni della rete in moduli logici più piccoli è più facile risolvere problemi tramite il concetto del *Divide et impera*;
- Attraverso la modularizzazione e un'interfaccia pulita si può avere una standardizzazione nelle interazioni tra dispositivi;
- Permette di cambiare un livello senza toccare gli altri (per esempio si può passare da Ethernet a Wi-Fi senza cambiare HTTP);
- La divisione in moduli inoltre permette di aggiungere facilmente nuovi livelli all'architettura.

Il modello OSI è stato definito dall'ISO come modello di riferimento ed è diviso in 7 livelli. Il modello TCP/IP invece è caratterizzato da 4 livelli (è dunque più semplice dell'ISO/OSI) ed è un modello pratico fondato su un'implementazione ampiamente utilizzata delle funzioni della rete.

### I 7 livelli del modello ISO/OSI

| Livello           | Funzione                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 7. Applicazione   | Costituisce l'interfaccia e fornisce i servizi di rete direttamente alle applicazioni software dell'utente. A questo livello operano i protocolli che definiscono le regole di comunicazione per i servizi di rete, come HTTP/HTTPS per il web e il DNS per la risoluzione dei nomi di dominio.                                                                                                                                                       |
| 6. Presentazione  | È il responsabile della formattazione delle informazioni (ad esempio attraverso compressioni e crittografia). Di questo livello fanno parte TLS, JPEG, MP4.                                                                                                                                                                                                                                                                                           |
| 5. Sessione       | Stabilisce, gestisce e termina le sessioni. Controlla il dialogo tra due programmi, aggiunge punti di controllo in caso di interruzione di rete e gestisce la chiusura della sessione in modo ordinato. In applicazioni reali possiamo trovare, appartenenti a questo livello, NetBIOS e RPC.                                                                                                                                                         |
| 4. Trasporto      | Consegna e controlla i messaggi da processo a processo dividendo i flussi di dati in pezzi più piccoli (*segment* per TCP o *datagram* per UDP) e decidendo come inviarli. Si occupa dell'affidabilità e della gestione dell'ordine tramite il protocollo TCP, oppure offre una consegna generalmente veloce ma non garantita tramite UDP. Gestisce inoltre il controllo di flusso e utilizza le porte per indirizzare correttamente i dati.          |
| 3. Rete (Network) | Si occupa dell'indirizzamento e del percorso. Determina il percorso logico migliore per far arrivare a destinazione i dati. In questo livello troviamo i router e l'indirizzo IP.                                                                                                                                                                                                                                                                     |
| 2. Data Link      | Organizza i bit in strutture dati e rileva eventuali alterazioni, scartando i frame danneggiati. I pacchetti vengono incapsulati in frame contenenti l'indirizzo MAC del mittente e del prossimo dispositivo sullo stesso collegamento. Gli indirizzi MAC di mittente e destinatario cambiano quando si attraversa un hop di livello 3, come può essere un router. Esempi per questo livello sono l'indirizzo MAC, l'Ethernet, il Wi-Fi e gli switch. |
| 1. Fisico         | Tratta il mezzo fisico attraverso il quale avviene il passaggio dei bit come segnali. Definisce inoltre i range fisici nel quale avvengono le trasmissioni. In questa categoria troviamo la fibra ottica, i cavi ethernet e le onde Wi-Fi.                                                                                                                                                                                                            |

#### L'ottavo e il nono livello
Questi livelli non appartengono al modello OSI definito dall'ISO, ma a un linguaggio informale. Nella loro informalità però presentano due elementi costantemente presenti nella rete che influenzano come essa viene gestita, meritando dunque un accenno a riguardo.
L'ottavo livello riguarda l'user e l'errore umano, come questo influenzi i sistemi reali e apra porte e considerazioni a nuovi rischi legati alla sicurezza informatica quali il phishing e il social engineering.
Il nono livello riguarda la politica e il management, ossia l'influenza e i limiti che le policy di un'organizzazione o delle leggi impongono alla rete.

### Il trasferimento dei dati
Quando un dispositivo invia i dati a un altro avviene un incapsulamento dei dati che avviene dall'alto verso il basso (dal livello applicativo al fisico). Una volta raggiunti a destinazione i dati vengono estratti risalendo il modello dal livello più basso (il fisico) fino al più alto (l'applicativo). Ogni livello può comunicare solo con un livello appena sopra o appena sotto durante l'incapsulamento o il decapsulamento dei dati mentre dialoga con un livello della stessa altezza nello scambio di dati tra due dispositivi.

#### I protocolli
Il modello è puramente teorico e generale, non definisce uno specifico metodo di comunicazione che viene invece definito dai vari protocolli. I vari esempi della tabella infatti sono applicazioni particolari del modello al web o ad altri contesti e non devono limitare il senso più ampio che il modello ISO/OSI deve fornire.

Un protocollo è una procedura e un formato standard che due dispositivi in comunicazione devono capire e accettare di usare per potersi parlare a vicenda.

#### Protocol Data Unit (PDU)
Quando i dati viaggiano tra i livelli vengono incapsulati. Il processo di incapsulamento crea un *Protocol Data Unit* (PDU) che è composto dai dati mandati insieme ai possibili header o footer aggiunti dal livello corrente e dai livelli superiori. Ogni livello incapsula la PDU ricevuta dal livello superiore aggiungendo, se previsto, le proprie informazioni di controllo.

Ad esempio, per una pagina web su Ethernet, i protocolli standard utilizzati sono:
- HTTP, usato per un primo incapsulamento a livello applicativo. I dati prendono il nome generico di *data* (dati).
- TCP, aggiunto al livello di trasporto. Il prodotto è chiamato *segment* (segmento).
- IP, usato nel livello di rete (network). I dati così incapsulati prendono il nome di *packet* (pacchetto).
- Ethernet, viene aggiunto al livello di Data Link. Si genera così un *frame*.
- A livello fisico i dati sono rappresentati dai *bit* trasmessi attraverso segnali radio, elettrici o ottici.

### Modello TCP/IP
TCP/IP fornisce un modello alternativo all'ISO/OSI per descrivere tutte le comunicazioni della rete. È un modello a 4 livelli basato su protocolli standard che internet ha sviluppato e il suo nome si riferisce ai due protocolli più largamente usati:
- TCP che sta per *Transmission Control Protocol*, protocollo già visto al livello del trasporto nel modello ISO/OSI.
- IP che significa *Internet Protocol* e viene utilizzato anche nel modello ISO/OSI al livello di rete.

#### I 4 livelli

| Livello TCP/IP | Corrispettivo ISO/OSI |
| ------- | -------- |
| Applicativo | Racchiude i livelli dal 7 al 5 |
| Trasporto | Corrisponde al quarto livello, quello livello di trasporto |
| Internet | Si tratta del corrispettivo del livello di rete |
| Network Access | Comprende i livelli di Data Link e fisico |

Come detto in precedenza il modello OSI definisce un ideale, nella realtà i processi gestiti dai livelli 5 e 6 vengono compiuti nel livello applicativo del modello TCP/IP.

#### Il modello client-server
Molti servizi su TCP/IP seguono il modello client-server per mettere in comunicazione due nodi della rete. È una relazione nella quale un programma (il client) richiede un servizio o una risorsa a un altro programma (il server). Il client deve necessariamente conoscere l'esistenza e l'indirizzo del server mentre quest'ultimo può ignorare la presenza del client prima di stabilire una connessione.

## Livelli ISO/OSI nel dettaglio

### Connessioni Fisiche e Data link
Esistono diverse tecnologie che mettono a contatto i dispositivi. Alcune ormai obsolete, altre di attuale impiego. Sebbene nel modello OSI il livello fisico e quello di data link siano divisi, nella pratica e nelle tecnologie moderne si trovano spesso uniti un unico tipo di collegamento.

#### Connessioni puramente fisiche
A livello puramente fisico i tipi di collegamento più importanti riguardano:
- Cavo coassiale
- Doppino telefonico
- Cavo Ethernet
- Fibra ottica

##### Cavo coassiale
È composto da quattro strati concentrici:
- Un filo metallico centrale (l'*anima*)
- Un dielettrico isolante (come un polietilene)
- Una calza metallica con funzione schermante
- Una guaina esterna di plastica
Il segnale viaggia sotto forma di campo elettromagnetico tra l'anima e la calza metallica. Quest'ultima funge da schermatura (gabbia di Faraday) mantenendo il campo dentro il cavo e bloccando l'ingresso di onde esterne che possono interferire. Alle estremità è applicato un tappo con la stessa impedenza del cavo in modo da impedire la formazione dell'onda riflessa formata dall'onda elettromagnetica giunta a fine percorso che può interferire con il segnale.
Uno degli usi più tipici era quello di collegare l'antenna alla tv o come prima forma di cavo per stabilire una connessione Ethernet. In particolare per quest'ultima funzione esistevano i cavi 10base5 (cavo coassiale spesso, presa a vampiro con vite perforatrice, massimo 500 metri) e 10base2 (cavo coassiale sottile, presa a "T" o BNC, massimo 185 metri). 

##### Doppino telefonico
Il doppino è composto da due cavi avvolti tra loro. Il suo connettore è chiamato RJ11.
È stato storicamente usato come cavo standard per collegare le abitazioni alla rete telefonica fissa.

##### Cavo ethernet
Si tratta di un'evoluzione del doppino telefonico ed è caratterizzato da 4 coppie di fili ritorti per un totale di 8 cavi. Il suo connettore è l'RJ45.
È un cavo di rete utilizzato per collegare dei dispositivi a una rete locale (LAN) o a Internet tramite un router o uno switch. Garantisce una connessione stabile, veloce e molto meno soggetta alle interferenze tipiche del Wi-Fi.
Esistono diverse tipologie per questo cavo. Le due differenze maggiori riguardano la velocità, definita in *bit rate*, ossia quanti bit passano al secondo, e la banda, cioè l’intervallo di frequenze che il cavo può trasportare bene. Una banda maggiore permette un bit rate maggiore. Queste caratteristiche dividono i cavi ethernet in diverse *cat*.

| Tipo di cavo | Velocità massima | Larghezza di banda massima |
| ------------ | ---------------- | -------------------------- |
| Cat 5        | 100 Mbps         | 100 MHz                    |
| Cat 5e       | 1.000 Mbps *     | 100 MHz                    |
| Cat 6        | 1.000 Mbps *     | 250 MHz                    |
| Cat 6a       | 10.000 Mbps      | 500 MHz                    |
| Cat 7        | 10.000 Mbps      | 600 MHz                    |
| Cat 7a       | 10.000 Mbps      | 1.000 MHz                  |

*\* Seguendo IEEE 802.3bz si può arrivare a 2,5 Gbps su Cat 5e e a 5 Gbps su Cat 6.*
Le velocità in tabella sono di riferimento per un collegamento di 100 metri, spostandoci questa misura le categorie possono avere velocità maggiori, intermedie o minori. Ad esempio un cavo Cat 6 può supportare fino a 10 Gbps su distanze ridotte, e il cavo Cat 8 è in grado di veicolare fisicamente velocità fino a 40 Gbps in condizioni adatte.
I cavi possono essere UTP, che sono cavi più semplici e comuni, o STP, che vengono schermati per limitare al minimo ogni interferenza.

##### Fibra ottica
Trasmette i dati sotto forma di impulsi luminosi (generalmente laser o LED) che viaggiano all'interno di un filo di vetro o plastica molto sottile, detto core, circondato da un materiale, chiamato cladding, con indice di rifrazione minore di quello del core. Quando la luce colpisce l'interfaccia tra il core e il cladding con un angolo superiore all'angolo critico, essa rimbalza invece di uscire, mantiene la luce all'interno del core e guidandola lungo il cavo. 
La fibra è superiore a un conduttore come il rame per la banda che è maggiore, per la minore attenuazione del segnale e l'immunità alle interferenze elettromagnetiche. È usata ad esempio nelle dorsali che collegano fisicamente i continenti oltreoceano o in delle LAN. La fibra può essere multimodale o monomodale. Quest'ultima è la più efficiente con un core molto fine (circa `9 * 10^-6 m`), un segnale laser alla sorgente, e una distanza che può arrivare a `100 km` di cavo.

#### Elementi appartenenti al primo e al secondo livello
Approfondiamo ora un dispositivo e due tecnologie ampiamente diffuse che uniscono le funzioni dei due livelli più bassi del modello ISO/OSI, ossia:
- Ethernet (standard IEEE 802.3)
- Wi-Fi (standard IEEE 802.11)
- La scheda di rete
Questi elementi della rete sono composti da una parte fisica (la trasmissione di un'onda o segnale in un mezzo fisico o un attacco fisico) e una logica (un protocollo). Proprio per questo motivo rientrano sia nel livello fisico che in quello di data link. Come detto in precedenza un livello può essere scambiato senza dover cambiare gli altri, permettendo, ad esempio, lo spostamento di un frame Ethernet attraverso la fibra o un cavo coassiale.

##### Ethernet

###### Introduzione
L'Ethernet dunque non si limita a essere il cavo descritto precedentemente, bensì è una tecnologia composta anche da un protocollo che gestisce e controlla i bit da trasmettere. Ethernet ha un MTU (Maximum Transmission Unit) di 1500 byte, ossia la dimensione massima che riesce a trasportare di un pacchetto senza che questo debba essere diviso. Questo limite è anche il più comune in internet.

###### Il frame
Il frame generato nel secondo livello da Ethernet è composto da 7 campi:
- Il preambolo, con valore fisso da 7 byte, serve a sincronizzarsi con il ricevente.
- Il secondo campo, SFD, è grande un byte e ha valore fisso, serve a comunicare che stanno arrivando dei dati.
- Il terzo e quarto campo occupano 6 byte ciascuno e contengono rispettivamente l'indirizzo MAC (Media Access Control) del destinatario e del mittente.
- Il quinto campo è grande due byte e indica la lunghezza del campo dati (secondo gli standard IEEE 802.3) oppure il tipo di protocollo del livello di rete in uso durante la trasmissione (secondo lo standard Ethernet II).
- I dati sono contenuti nel campo del payload (il sesto). Se i dati sono molto piccoli e non raggiungono i 46 byte viene aggiunto del *padding* (byte senza valore effettivo) per raggiungere il minimo richiesto.
- L'ultimo campo, *Frame Check Sequence* (FCS), è un numero lungo 4 byte che corrisponde al risultato di una funzione di checksum eseguita sul frame. È un dato fondamentale perché permette di controllare l'integrità dei frame. Il dispositivo ricevente calcola il checksum del frame e lo confronta con quello spedito: se i due valori coincidono il frame è integro e viene passato ai livelli superiori, altrimenti viene scartato.

##### Wi-Fi
Il Wi-Fi è un insieme di tecnologie per reti locali senza fili (WLAN) che permette a più dispositivi di essere connessi tra loro mediante onde radio. Le sue caratteristiche sono descritte nell'IEEE 802.11.
Data la natura fisica della sua trasmissione è un mezzo meno affidabile di una connessione cablata per via dei problemi legati alle interferenze. Per ridurre al minimo le conseguenze di queste ultime il Wi-Fi deve applicare diverse tecniche e subisce alcune restrizioni. Le più importanti misure sono:
-  Una stazione non può ascoltare mentre trasmette e viceversa.
- Prima di comunicare devono assicurarsi che il canale della frequenza su cui vogliono trasmettere i dati non sia già in uso (usare lo stesso canale causerebbe un'alta interferenza).
- Per evitare collisioni la comunicazione avviene usando il protocollo CSMA/CA.
- Grazie al campo contenente il checksum del frame è possibile controllarne l'integrità.
- Viaggiando nell'etere spesso si desidera una prima crittografia che avviene già al secondo livello del modello OSI, cosa che tipicamente non avviene prima del sesto livello (ad esempio con TLS). I protocolli più usati per la crittografia a questo livello sono WPA2 e WPA3.

###### Versioni
Il Wi-Fi ha diverse versioni che operano a tre diverse bande.

| Versione | Standard      | Banda       | Note                                                |
| -------- | ------------- | ----------- | --------------------------------------------------- |
| Wi‑Fi 8  | IEEE 802.11bn | 2,4/5/6 GHz | In elaborazione. Si stima verrà rilasciato nel 2028 |
| Wi‑Fi 7  | IEEE 802.11be | 2,4/5/6 GHz |                                                     |
| Wi‑Fi 6E | IEEE 802.11ax | 2,4/5/6 GHz |                                                     |
| Wi‑Fi 6  | IEEE 802.11ax | 2,4/5 GHz   |                                                     |
| Wi-FI 5  | IEEE 802.11ac | 5 GHz       |                                                     |
| Wi‑Fi 4  | IEEE 802.11n  | 2,4/5 GHz   |                                                     |

Aumentando la frequenza diminuisce la distanza raggiunta dalla connessione che però permette (insieme ad altri fattori) di ottenere maggiori prestazioni e velocità.
Frequenze più alte inoltre dispongono di più canali, rendendo più veloce la connessione (è più difficile che si debba aspettare che un canale si liberi) e limitano il rischio di interferenza con altre onde della stessa frequenza. Un dispositivo può trasmettere e ricevere onde di frequenze diverse. La maggior parte dei dispositivi moderni riescono a sfruttare le frequenze più alte fino ai 5/6 GHz mentre dispositivi più vecchi si fermano alle bande più basse.

##### La scheda di rete
La scheda di rete o NIC (acronimo di _Network Interface Card_) è un dispositivo di livello 1 e 2 che fornisce un attacco fisico ai cavi e un punto di collegamento per il WiFi.  
Questo dispositivo è il responsabile dell'esecuzione dei protocolli di livelli 2, ad esempio Ethernet definisce un protocollo e il suo frame ma la costruzione di quest'ultimo e i calcoli ad esso legati vengono fatti dal NIC. Controlla inoltre il traffico, le regole di trasmissione e protocolli come CSMA/CA (per il wifi) o CSMA/CD (per Ethernet).  
Questo dispositivo inoltre è il pezzo hardware che contiene l'indirizzo MAC del dispositivo.

#### Elementi del secondo livello
- Switches e Bridges
- L'indirizzo MAC
- CSMA/CD e CSMA/CA

##### Switches e Bridges
Questi dispositivi analizzano i frame ricevuti, trovano l'indirizzo MAC di destinazione e li spediscono alla porta appropriata.
I bridges e gli switches soddisfano gli stessi bisogni ma i secondi permettono di collegare moltissimi più dispositivi insieme, sono più veloci (lettura MAC a livello hardware e non software) e offrono un controllo migliore sulle collisioni.
Il termine porta quando si parla degli switch fa riferimento all'interfaccia fisica (il buco vero e proprio dove si collegano i cavi) che il singolo dispositivo identifica con un indirizzo logico (ad esempio un nome o un numero). Il termine perciò non deve essere confuso con quello delle porte puramente virtuali assegnate ai protocolli.
Per determinare come direzionare il traffico usano una tabella degli indirizzi MAC (*MAC address table*) salvata internamente allo switch.
Per facilitare il funzionamento di tali tabelle prendiamo in esame una rete composta da un server (MAC: 00:00:00:00:00:01, porta e3), un pc (MAC: 00:00:00:00:00:02, porta e2) e un laptop (MAC: 00:00:00:00:00:03, porta e1) dove il pc prova a comunicare col server.

All'inizio, quando non sono ancora presenti o sono scadute le associazioni tra la porta fisica dello switch e l'indirizzo MAC del dispositivo ad essa collegato, la tabella MAC è vuota.

| MAC | Porta |
| --- | ----- |
|     |       |

Quando uno dei dispositivi inizia a mandare dei frame (che come abbiamo visto contengono i MAC del mittente e del destinatario), lo switch li riceve e, non avendo salvato nella tabella le informazioni del primo dispositivo, crea una nuova riga contenente il suo indirizzo MAC e la porta a lui corrispondente.

| MAC               | Porta |
| ----------------- | ----- |
| 00:00:00:00:00:02 | e2    |

Siccome la tabella contiene solo il MAC del mittente lo switch non sa a che porta direzionare i dati in uscita e decide dunque di provare tutte le porte eccetto quella da cui provengono i dati. Questo processo è definito *unknown unicast flooding* o più semplicemente *flooding*.
Se il destinatario è collegato allo switch verrà trovato e, assumendo che voglia rispondere al mittente, manderà un nuovo frame contenente, come al solito, il MAC di destinazione e quello di partenza. Lo switch aggiungerà allora alla tabella MAC una riga che associa l'indirizzo del secondo dispositivo alla sua corrispondente porta fisica dello switch.

| MAC               | Porta |
| ----------------- | ----- |
| 00:00:00:00:00:02 | e2    |
| 00:00:00:00:00:01 | e3    |

Per inviare il frame di risposta al primo dispositivo lo switch ora non dovrà più inviarlo a tutte le porte, ma gli basterà guardare nella tabella per trovare subito la porta corretta.

Riassumendo lo switch a ogni trasferimento legge la tabella, se incontra:
- un dispositivo nuovo -> aggiunta di una riga MAC - porta
- un destinatario sconosciuto -> flooding
- dispositivi noti -> passa subito i dati attraverso la porta corretta

##### L'Indirizzo MAC (Media Access Control)
È un identificativo a 48 bit bruciato nella memoria della scheda di rete (è inciso nella scheda e non può essere cambiato fisicamente anche se a livello superiore può essere mascherato o randomizzato). I 48 bit del MAC sono suddivisi in 12 cifre esadecimali: le prime 6 cifre individuano l’organizzazione assegnataria e le successive sono assegnati da questa organizzazione.  
L'indirizzo è scritto come stringa di numeri esadecimali divisi in 6 coppie separate dai due punti o dal trattino centrale (es. `00:1A:2B:3C:4D:5E`).  
Identifica in modo univoco un NIC e viene usato nei processi delle reti locali. Generalizzando potremmo dire che identifica in modo univoco un dispositivo ma solo se questi ha solo una scheda di rete.

#### CSMA/CD e CSMA/CA
Questi due protocolli di secondo livello permettono di gestire le collisioni. In particolare il primo gestisce una collisione solo dopo che essa avviene, il secondo la previene. CSMA/CD è tipico delle connessioni cablate (ad esempio per il cavo coassiale) mentre CSMA/CA è usato dalle connessioni wireless (ad esempio è usato dal Wi-Fi).

La sigla CSMA significa:
- CS (_carrier sense_): controlla costantemente il canale per vedere se stanno già avvenendo delle comunicazioni.
- MA (_multiple access_): siccome molti dispositivi condividono lo stesso cavo o spettro di frequenza, definisce delle regole comuni e come gestirle.

Le due sigle finali invece significano:
- CD (*collision detection*): quando il mezzo è libero si aspetta un brevissimo tempo, si ricontrolla se il canale è libero e nel caso si manda il frame. Durante l'intera trasmissione l'adattatore monitora la rete e se non riceve segnali considera il frame spedito. Se l'adattatore invece riceve, durante una trasmissione, un segnale da un altro adattatore, arresta la trasmissione e trasmette un segnale di disturbo che avverte le altre stazioni dell'avvenuta collisione e bloccando la trasmissione. Dopo aver abortito la trasmissione le stazioni attendono un tempo aleatorio prima della ritrasmissione. CSMA/CD era un protocollo usato in passato in assenza di tecnologia più moderne, con tecniche quali la commutazione Ethernet e la comunicazione full-duplex si è persa la necessità di questo protocollo.
- CA (_collision avoidance_): se la rete è libera viene aspettato un brevissimo tempo e, se ancora libera, viene spedito il frame. Se si riceve un riscontro (ACK) dal destinatario il frame è arrivato con successo altrimenti si assume che il frame non è stato ricevuto correttamente (collisione, interferenza, destinazione fuori portata), e si ritrasmette dopo un backoff (tempo di attesa) casuale.

### Protocollo IP

#### Introduzione
Il protocollo IP (_internet protocol_) è l'esempio più significativo del livello 3 del modello OSI ed è lo standard per il trasferimento dei pacchetti attraverso internet.  
L'indirizzo IP è un'etichetta assegnata a ogni dispositivo connesso a una rete che usa questo protocollo per le comunicazioni. A differenza dell'indirizzo MAC questo numero cambia e può anche essere impostato, entro certi limiti, a scelta dall'utente. A differenza dell'indirizzo MAC, l'IP non è legato a un pezzo hardware ma a un'interfaccia, ciò significa che un singolo dispositivo può avere molteplici indirizzi IP a lui associati.
Esistono due versioni degli indirizzi IP: IPv4 e IPv6. Le due versioni non possono comunicare direttamente tra loro, ma coesistono sulla stessa rete e sullo stesso dispositivo con il dual stack; la comunicazione tra le due versioni richiede meccanismi di traduzione o di tunneling. Per il traffico IPv4–IPv4 e IPv6–IPv6 non serve alcuna traduzione.

#### IPv6
Si tratta della versione più recente, nata dalla necessità di avere di più indirizzi IP.
L'indirizzo è lungo 128 bit, permettendo la presenza di `2^128` indirizzi unici (circa `10^38`), un numero sufficiente per i dispositivi presenti oggigiorno (non più sostenibile invece dai limitati 4 miliardi di indirizzi utilizzabili con IPv4).
Gli indirizzi sono rappresentati da otto gruppi da quattro cifre esadecimali, separati dai due punti e accorciati secondo alcune regole per essere scritti nel modo più corto possibile. Ad esempio un indirizzo come `2001:0db8:0000:0000:0000:8a2e:0370:7334` diventa `2001:db8::8a2e:370:7334`.

#### IPv4
È la versione più utilizzata anche se si sta transizionando verso IPv6.
Essendo lungo 32 bit il protocollo può fornire al massimo `2^32` (circa 4 miliardi) indirizzi unici, ben sotto al numero di dispositivi connessi alla rete oggi e già al limite verso gli inizi degli anni '90.
L'indirizzo è diviso in gruppi da 8 bit andando a formare quattro ottetti trasformati poi in numeri decimali (ogni ottetto sarà compreso dunque tra 0 e 255) divisi da un punto.
Ad esempio l'indirizzo `11000000 10101000 01100100 11001000` diventa 
`192.168.100.200`.
Da qui in avanti tutti gli esempi saranno fatti con indirizzi IPv4 ma la maggior parte delle procedure e concetti sono simili per IPv6.

#### Netmask
L'indirizzo IP è composto di due parti: l'indirizzo di rete e l'indirizzo dell'host. L'indirizzo di rete è paragonabile a una via e l'indirizzo dell'host al numero civico. Come in una via ci sono più case, i primi tre ottetti di un indirizzo di rete possono essere uguali per molti dispositivi (ad esempio quelli interni a una LAN), che poi saranno resi unici dall'ultimo ottetto. Il compito della *netmask* è quello di dividere l'IP in queste due componenti. Ad esempio con una netmask /24 i primi tre ottetti indicano l'indirizzo di rete mentre l'ultimo l'indirizzo dell'host.
Facendo un AND logico tra l'indirizzo IP e la netmask si ottiene l'indirizzo di rete.

##### Default Netmask
Ci sono poi le netmask di default che hanno degli ottetti composti da tutti 1 o tutti 0. Le mask sono scritte come uno slash seguito dal numero di bit pieni. Avremmo dunque mask come /8, /16 o /24. Una mask /8 si legge "a 8 bit", una /16 "a 16 bit" e via dicendo.

| Classe dell'indirizzo | Bit per ID rete / ID host | Default subnet mask |
| --------------------- | ------------------------- | ------------------- |
| Classe A              | 8 / 24                    | 255.0.0.0           |
| Classe B              | 16 / 16                   | 255.255.0.0         |
| Classe C              | 24 / 8                    | 255.255.255.0       |

La divisione in classi presentata descrive il vecchio indirizzamento classful, uno schema storico oggi sostituito da CIDR. Il vantaggio di CIDR è quello di poter avere mask non composte da multipli di 8, come la /12.
Esistono poi le classi D e E, la prima riguarda gli indirizzi legati al multicast (di cui si parla nella prossima sezione), la seconda è usata solo in ambito sperimentale e di ricerca.

#### Indirizzi riservati
Esistono tre indirizzi riservati per operazioni speciali che non possono essere usati come nodi:
- L'indirizzo di rete: è il primo indirizzo della rete, dove i bit dell'host sono tutti 0. È usato per identificare la rete. Ad esempio `11000000 10101000 01100100 00000000` `192.168.100.0`
- L'indirizzo di broadcast: è l'ultimo indirizzo della rete, l'indirizzo dell'host è composto da tutti 1. Un pacchetto diretto all'indirizzo di broadcast viene inviato a tutti i nodi della rete IP. Un esempio è `11000000 10101000 01100100 11111111`, ossia `192.168.100.255`
- Indirizzo di multicast: sono indirizzi che possono essere associati a un gruppo di dispositivi scelti. A differenza del broadcast, che inoltra i dati a un'intera subnet, il multicast riguarda dispositivi diversi, magari appartenenti a reti o subnet diverse tra loro, che hanno aderito al gruppo. Gli indirizzi da `224.0.0.0` a `239.255.255.255` sono configurabili come indirizzi di multicast.

#### Indirizzi privati
Gli indirizzi IP privati non raggiungono e non sono raggiungibili pubblicamente e lavorano solo dentro a reti locali o private con la possibilità di comunicare con internet solo tramite terzi dispositivi e adeguate configurazioni. 
I tre range di indirizzi privati sono composti da:

| Blocchi CIDR   | Mask     | Range degli indirizzi            | Mask esplicita |
| -------------- | -------- | -------------------------------- | -------------- |
| 10.0.0.0/8     | Mask /8  | Da 10.0.0.0 a 10.255.255.255     | 255.0.0.0      |
| 172.16.0.0/12  | Mask /12 | Da 172.16.0.0 a 172.31.255.255   | 255.240.0.0    |
| 192.168.0.0/16 | Mask /16 | Da 192.168.0.0 a 192.168.255.255 | 255.255.0.0    |

#### IP routing
L'IP routing è il processo di mandare pacchetti da un host appartenente a una rete a un altro host che sta in una rete separata e non direttamente connessa. Un dispositivo come può essere un router, quando riceve un pacchetto, esamina l'IP di destinazione e tramite delle _routing tables_ determina l'indirizzo di hop successivo a cui il pacchetto deve essere inoltrato. L'indirizzo di hop è l'IP del router o del gateway successivo direttamente raggiungibile sul collegamento a cui il pacchetto è inviato nella sua tratta verso la destinazione.

### Router
È un dispositivo di livello 3 connesso a due o più linee di dati provenienti da reti IP diverse con lo scopo di inoltrare i pacchetti tra queste reti.

#### Routing tables
Queste tabelle vengono usate dai nodi della rete (i punti di collegamento tra reti, come può essere un router) per determinare il percorso da seguire per raggiungere la rete di destinazione. All'interno delle tabelle di routing troviamo i seguenti campi:
- Rete di destinazione e mask: serve a specificare i dispositivi destinatari della comunicazione. Grazie alla mask possiamo identificare un singolo dispositivo o una range di IP interessati.
- L'IP del next hop / gateway: è l'indirizzo del router usato per raggiungere la rete di nostro interesse.
- Interfaccia di uscita: rappresenta l'interfaccia attraverso la quale il pacchetto deve uscire per raggiungere la rete di destinazione.

### TCP e UDP
TCP e UDP sono i protocolli di livello 4 più comuni e presenti nella maggior parte delle tecnologie usate ogni giorno in internet.

#### TCP
- Crea una connessione tramite il *three-way handshake* prima che i messaggi vengano inviati.
- Controlla l'integrità di ogni segmento e se corrotto lo invia nuovamente
- Riordina i segmenti ricevuti in un ordine specifico. Con messaggi molto grandi i dati vengono divisi in segmenti più piccoli che a destinazione devono essere riordinati

##### TCP segment
Il segmento TCP è composto dai dati preceduti da un header. Al suo interno troviamo:
- Source port e Destination port  (16 bit per porta, 32 totali): il numero di porta sull'host del mittente e del destinatario associati alla connessione TCP.
- Sequence number (32 bit): conta i byte del payload a partire da un valore iniziale casuale (ISN). Se analizziamo un traffico in cui abbiamo un sequence number di 1000 e inviamo un payload da 500 byte il sequence number si aggiorna 1500. Questo permette di riordinare i pacchetti quando arrivano a destinazione e ad assicurarsi che non ci sono state perdite nel flusso.
- Acknowledgment number (32 bit): indica il prossimo byte atteso. Conferma la corretta ricezione dei segmenti se il numero qui contenuto è quello aspettato dall'host. È valido solo se nel segmento la flag `ACK` è impostata.
- Data offset (4 bit): rappresenta la lunghezza dell'header TCP in "parole" (una sequenza da 32 bit dell'header equivale a una parola). Varia in base alla dimensione del campo *Options* che vedremo dopo ed è un numero che va da un minimo di 5 a un massimo di 15.
- Reserved (4 bit): bit nulli.
- Flags (8 bit): esistono otto diverse flag (ognuna con valore 0 o 1), le più significative sono:
	- SYN: quando impostato a 1 significa che il mittente vuole aprire una connessione col destinatario.
	- ACK: rende valido l'*Acknowledgment number*.
	- FIN: quando impostato a 1 serve a chiudere una connessione.
- Window size (16 bit): rappresenta la quantità totale di dati (in byte) che l'host del destinatario è pronto a ricevere per non essere saturato. È un numero variabile basato sulle risorse a disposizione sul momento.
- Checksum (16 bit): risultato della funzione checksum sul segmento. Serve al destinatario per controllarne l'integrità e se necessario farselo rispedire.
- Urgent pointer (16 bit): usato solo se presenta la flag URG.
- Options (0 - 40 byte): è un campo facoltativo. Un dato significativo che può essere presente è la dimensione massima del segmento (MSS, *Maximum Segment Size*) esclusi gli header, un parametro che indica la quantità massima di dati (in byte) che un host è in grado di accettare in un singolo segmento. Si calcola togliendo all'MTU (di cui si è parlato nella sezione sull'Ethernet) la dimensione degli header IP e TCP. Tipicamente è di 1460 byte con MTU 1500, IPv4 e header TCP senza opzioni.
- Data (massimo MSS bytes): il campo contenente i dati o parte di essi se vengono spezzati in più segmenti.

##### Apertura: Three-way handshake
Il *three-way handshake* è un iniziale scambio di segmenti tra due dispositivi che precede la comunicazione dei dati.
- Il primo spedisce un segmento con flag `SYN` a 1 per controllare se l'altro dispositivo è acceso e in ascolto.
- Se il secondo dispositivo è acceso, in ascolto, e vuole rispondere, invia un segmento con le flags `SYN` e `ACK` a 1.
- Il primo dispositivo risponde con un segmento avente flag `ACK` a 1 e i due iniziano a comunicare i dati.

##### Chiusura: Four-way handshake e Three way handshake
Per chiudere una connessione TCP viene usato il *four-way handshake*, ogni tanto semplificato nel *three-way handshake*. Entrambi devono assicurare chiusure complete e pulite della connessione.
Nel primo caso il dispositivo manda un segmento `FIN`, a cui il destinatario risponde con un `ACK`. A questo punto i ruoli si invertono con il secondo dispositivo che invia un `FIN` a cui segue un `ACK` di risposta dal primo dispositivo.
In alcuni casi il secondo dispositivo unisce il suo segmento `ACK` con quello `FIN` in uno scambio simile a quello dell'apertura. Lo scambio inizia con un segmento FIN a cui segue un segmento `FIN/ACK` e si conclude con un segmento `ACK`.

#### UDP
- Manda direttamente i dati al destinatario senza stabilire prima una connessione. Questo permette di saltare un passaggio ma al tempo stesso non assicura che il dispositivo a cui stiamo mandando i dati sia acceso o in ascolto.
- Grazie al checksum permette di rilevare datagram corrotti che vengono scartati e non ritrasmessi.
- Non preserva l'ordine dei datagram.
- UDP è usato per servizi che possono permettersi di perdere dei pacchetti (a meno di implementazione di sistemi di livello superiore che lo impediscono) in cambio di velocità di trasmissioni maggiori. Tra questi servizi troviamo lo streaming di audio e video in tempo reale. Per ovviare al problema dell'ordinamento dei datagram vengono implementati degli algoritmi a livello superiore che si occupano di aggiungere a ogni datagram un numero identificativo per poter essere successivamente riordinati.
È importante ricordare che lo streaming on demand (YouTube, Netflix) si basa su HTTP (dunque su TCP nelle versioni di HTTP fino alla 2), le comunicazioni live invece si basano su UDP (VoIP, video-chiamate, giochi).

##### UDP datagram
Non dovendo eseguire tutti i controlli di integrità del TCP il datagram è molto più semplice e si compone di:
- Source port e Destination port  (16 bit per porta, 32 totali): il numero di porta sull'host del mittente e del destinatario associati alla connessione UDP.
- Length (16 bit): contiene la lunghezza totale in bytes del datagram.
- Checksum (16 bit): generalmente è opzionale con IPv4 e obbligatorio con IPv6 e contiene il risultato della funzione checksum sul datagram.
- Payload: contiene il messaggio.
Il datagram ha una dimensione limite di 65.535 byte, anche se è spesso ridotta da fattori quali l'OS (il sistema operativo) e il limite di trasporto imposto dal protocollo IP.

#### TCP vs UDP tabella riassuntiva

| Caratteristica            | TCP                                                                                    | UDP                                                                                    |
| ------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Connessione preventiva    | Si                                                                                     | No                                                                                     |
| Controllo sugli errori    | Si                                                                                     | Si                                                                                     |
| Ripristino degli errori   | Si                                                                                     | No                                                                                     |
| Ordinamento dei pacchetti | Si                                                                                     | No                                                                                     |
| Header                    | Grande e più complesso                                                                 | Piccolo e più semplice                                                                 |
| Velocità                  | È più lento perché deve fare diversi controlli, ordinamenti ed elaborare grandi header | È generalmente più veloce perché fa meno controlli e deve elaborare header più piccoli |
| Esempi applicativi        | Download                                                                               | Comunicazioni in tempo reale                                                           |

### Protocolli del livello applicativo

#### HTTP
*Hypertext Transfer Protocol*
Sta alla base dei web server e browser e ne definisce i comandi, intestazioni e processi. HTTP/1.1 e 2 usano TCP, HTTP/3 usa UDP con sopra QUIC che fornisce affidabilità e ordinamento. HTTP usa le porte 80 o 8080, HTTPS (la versione cifrata di HTTP grazie a TLS) usa la porta 443 o 8443.

#### DNS
*Domain Name System*
Questo protocollo permette di associare un indirizzo IP a un nome. Un dispositivo fa una richiesta al DNS con un nome (ad esempio il nome di dominio di un sito web) e riceve l'indirizzo corrispondente. I DNS usano sia UDP che TCP su porta 53.

#### SSH
*Secure Shell*
Permette di eseguire comandi e accedere a un computer da remoto. Cifra tutti i dati e le operazioni rendendoli sicuri.
Usa TCP su porta 22.

#### TFTP
*Trivial File Transfer Protocol*
È un protocollo semplice e leggero che consente il trasferimento di file. Non prevede alcuna misura di sicurezza e per questo viene tipicamente usato in reti locali o ambienti controllati. Usa UDP sulla porta 69.

#### FTP
*File Transfer Protocol*
È un protocollo che serve a trasferire dei file tra un cliente e un server. Solitamente richiede un username e una password e nella sua versione sicura, ossia FTPS, il traffico è crittografato. Usa TCP su porta 20 per il trasferimento dei file in modalità attiva e sulla 21 per l'esecuzione di comandi di controllo.

#### NFS
*Network File System*
Si tratta di un protocollo di rete che permette a un computer (client) di accedere a file e directory condivisi da un server remoto come se fossero memorizzati sul proprio disco locale. Sviluppato originariamente da Sun Microsystems nel 1984, è molto diffuso in ambienti **UNIX e Linux**, ma supportato anche da Windows Server. Come protocollo di trasporto storicamente usa UDP, nelle versioni più moderne (NFSv4) usa TCP.

#### SMTP
*Simple Mail Transfer Protocol*
È lo standard per l'invio e l'inoltro delle mail in internet. L'SMTP gestisce esclusivamente l'invio della posta in uscita e il passaggio dei messaggi tra server. Per la ricezione e la lettura si usano altri protocolli come IMAP o POP3.
Usa TCP, storicamente si affida alla porta 25.

#### Telnet
*Teletype Network*
Predecessore storico di SSH sostituito perché trasmetteva tutto in chiaro. Permette di eseguire comandi e accedere a un computer da remoto. Usa TCP su porta 23.

#### SNMP
*Simple Network Management Protocol*
Serve per il monitoraggio, la gestione e la configurazione di dispositivi connessi a una rete IP. Usa UDP sulle porte 161 e 162.

#### DHCP
*Dynamic Host Configuration Protocol*
Protocollo utilizzato per assegnare in modo dinamico gli indirizzi IP a un host. Con IPv4 si usa DHCP con UDP sulle porte 67 (server) e 68 (client). Per IPv6 una delle opzioni usate è il più moderno DHCPv6, anch'esso usa UDP ma sulle porte 546 (client) e 547 (server).

---

## Autore
Emanuele Canazza - [GitHub Profile](https://github.com/emanuele-c147)

## Fonti
- IEEE 802
	- In particolare vedere IEEE 802.3, IEEE 802.11
- [Documentazione RFC](https://www.rfc-editor.org/)
	- In particolare vedere RFC 791, RFC 768, RFC 1918, RFC 9114.
- ITU-T standards
	- In particolare vedere ITU-T X.200
- TIA standards
- Conoscenze apprese personalmente nel tempo, da ricerche online e durante il corso CyberChallenge.IT 2026 con l'università di Ferrara
