# WebProgramming-2014-project1
Java Servlets,JSp, Filters

# Introduzione

Il primo approccio al compito del gruppo è stato quello di cominciare a capire quali fossero i task
fondamentali che andavano implementati all’interno del sito che dovevamo creare.
Quindi come primo step abbiamo dovuto affrontare la creazione del database, problema molto complesso
perché ci siamo trovati a discutere su come salvare nella nostra base di dati tutta la mole di informazioni
che andranno poi a riempire il forum. Il problema verrà poi discusso più dettagliatamente in seguito.
Finita la creazione del database siamo passati alla programmazione pura dove abbiamo trovato molto utili
le slide fornite durante il corso, grazie alle quali siamo riusciti a risolvere alcuni problemi che avevamo
riscontrato nella realizzazione del sito.
Una volta terminata la parte di codice il gruppo si è dedicato alla parte grafica, sistemando alcune pagine ed
aggiungendo dei fogli di stile css.

# Scelte database

La prima parte del tempo dedicato al progetto, in cui tutti tre i componenti hanno lavorato assieme
portando ognuno le proprie idee e motivazioni, è stata trascorsa nella ricerca della migliore soluzione nella
creazione del database, ovvero quella che poi ci avrebbe reso più semplice l’utilizzo dei dati all’interno del
codice.
Le scelte su cui il gruppo ha dovuto discutere maggiormente sono state relative ai campi dove vengono
indicate le date (data del post, data dell’ultimo login dell’utente), che abbiamo deciso di salvare come un
dato timestamp per semplificare le operazioni che avremmo dovuto compiere con quei dati, mentre la data
di creazione del gruppo abbiamo deciso di lasciarla di tipo date in quanto non ci serviva per nessuna
operazione.
Altre scelte importanti sono state su come implementare la partecipazione di un utente ad un gruppo e
l’invito da parte di un amministratore, in entrambi i casi è stato deciso di creare un’entità a sé stante con al
loro interno un campo booleano che riporta, rispettivamente, se l’utente è stato eliminato dal gruppo
oppure ha accettato l’invito; tutto questo per tenere traccia di tutte le attività degli utenti all’interno del
sito, infatti nel caso un partecipante venga eliminato dal gruppo, nel database risulta ancora presente il loro
legame, anche per evitare che una persona indesiderata venga aggiunta nuovamente, magari per errore,
dall’admin.

# Scrittura codice

La parte di scrittura del codice è stata divisa tra i vari componenti del gruppo ed è stata la parte che ha
occupato il maggior spicchio di tempo di tutto il progetto.
Tutto il progetto è contenuto in diverse servlet, come chiesto, che contengono quindi, sia la parte grafica,
sia la parte nascosta, ma più importante del progetto, ovvero il codice.
Scelte importanti nella fase di coding sono state relative al quick display nella home page, infatti il gruppo
ha deciso di tenere traccia nel database dell’ultimo login di un utente, così da semplificare il controllo sugli
aggiornamenti ai gruppi a cui si è iscritti.Un’altra scelta fatta più per che comodità è stata quella relativa al nome univoco dei file caricati in un post,
qui il gruppo ha scelto di aggiungere l’id del file prima del nome del file stesso, così da evitare che un file
abbia lo stesso nome di uno caricato precedentemente.
Servlet:
Le classi del progetto sono suddivise all’interno dei seguenti package:
- Filter – contiene le servlet per gestire i filtri
- DB – contiene le servlet per comunicare con il database
- Listeners – contiene la servlet per aprire la conessione al database
- Visualizza – contiene le servlet “grafiche”, ovvero che contengono le istruzioni HTML
- Servlets – contiene le servlet usate dal processo che non rientrano in nessuna delle
 precedenti categorie

All’interno di questi package sono contenute le seguenti servlet; nel seguito ne viene descritto il
funzionamento, ne viene spiegata la scelta di programmazione effettuata; tra parentesi viene mostrato in
quale package sono contenute.
ViewLog(visualizza): è la servlet che viene utilizzata per mostrare il login, che viene effettuato tramite
username e password. Premendo il tasto “Entra” viene invocata la servlet Logged.
Logged(servlets): controlla che il login effettuato sia corretto e setta i valori al cookie.
Home(visualizza): contiene la pagina principale del forum, in alto viene visualizzato l’ultimo accesso
dell’utente, tramite cookie, e il suo avatar, poi i bottoni per la creazione di un nuovo
gruppo(CreaGruppoView) e per mostrare i gruppi a cui si è iscritti(HomeGroups); più in basso è presente il
quick display con gli aggiornamenti dall’ultima visita ai propri gruppi e gli inviti ricevuti, con la possibilità di
accettare(AccettaInvito) o di rifiutare(RifiutaInvito), il tutto tramite due pulsanti.
Nella parte inferiore troviamo i pulsanti per gestire l’upload di un nuovo avatar e un link per fare il logout.
CreaGruppoView(visualizza): premendo il tasto Crea Gruppo si viene indirizzati a questa servlet che mostra
solamente una TextBox e un bottone per inserire un nuovo gruppo, il quale richiama la CreaGr.
CreaGr(servlets): dopo aver controllato che il parametro inserito nella TextBox non sia nullo, in tal caso
viene mostrata nuovamente la CreaGruppo, viene invocata una query che inserisce il gruppo e la relazione
tra lo stesso e il creatore all’interno del database, viene poi visualizzata la ViewGroup.
HomeGroups(visualizza): premendo dalla Home il tasto “I miei gruppi” si viene indirizzata a questa servlet
che mostra in un elenco puntato i gruppi a cui l’utente è iscritto.
ViewGroup(visualizza): questa servlet mostra in alto il nome del gruppo scelto, poi inseriti in una tabella
sono presenti i seguenti dati: data e ora (in formato TimeStamp, avatar, utente e testo del post. Più sotto
sono presenti la TextBox, e i bottoni per inserire il post(InsPost) e gli eventuali file(UploadFile) caricati
dall’utente all’interno della discussione. Sul fondo della pagina sono visualizzati gli utenti partecipanti alla
discussione e due pulsanti, uno che manda alla gestione del gruppo, visibile solamente per l’amministratore
e un per tornare alla Home.
Nel momento di caricare il testo dei post viene effettuato il parsing, sia per i file già caricati, sia per gli
indirizzi esterni.
InsPost(servlets): inserisce il post(testo, data e ora, etc) all’interno del database. Nel caso del caricamento
di uno o più files alla fine del messaggio aggiunge del testo dove mostra tutti i file caricati per quel post e
richiama la ViewGroup.
Upload(servlets): crea una nuova cartella “File” nella directory build del progetto, qualora non sia già
presente, salva all’interno il file caricato ed inserisce nel database il percorso.Download(servlets): cliccando sul nome del file, quest’ultimo viene salvato nella directory predefinita dei
download.
ImpostazioniAdmin(visualizza): è la pagina riassuntiva del gruppo che può essere aperta solamente
dall’amministratore, in cui si possono vedere gli utenti presenti nel gruppo, con la possibilità di eliminarli
tramite bottone(EliminaUtentiGruppi), gli utenti che possono essere invitati(InvitaUtenti), una TextBox con
relativo bottone per rinominare il gruppo(Rinomina), un tasto per creare il PDF riassuntivo(CreaPDF) del
gruppo e un tasto per tornare alla ViewGroup.
EliminaUtentiGruppi(servlets): modifica il campo “Eliminato” all’interno del record nel database, così da
escludere l’utente dalla discussione.
InvitaUtenti(servlets): esegue la query che aggiunge alla tabella “Invito” un record, in attesa della risposta
dell’utente invitato.
Rinomina(servlets): aggiorna il record relativo al gruppo nel database
CreaPDF(servlets): crea una nuova cartella “PDF” nella directory build del progetto, qualora non sia già
presente, e salva il file in cui sono contenuti, gli utenti partecipanti al gruppi con avatar, il numero dei post
e la data dell’ultimo commento inserito.
AccettaInvito(servlets): setta il campo booleano “Accettato” come true ed inserisce la relazione tra utente e
gruppo .
RifiutaInvito(servlets): cancella dalla tabella “Invito” il record ed impedisce un nuovo invito.
UploadAvatar(servlets): come l’Upload, solamente che crea una cartella “Avatar”, nel caso non esista, e
controlla che il tipo del file caricato sia JPG.
LogOut(servlets): torna alla ViewLog e chiude la sessione.
DBManager(DB): servlet che contiene tutte le query necessarie
Gruppo-Post-Utente(DB): servlet che rimappano le tabelle del database.
FilterGroup(filter): chiama una query in cui controlla che l’utente faccia parte del gruppo passato come
parametro dall’URL, in caso contrario chiama una pagina presa come attributo della sessione(la pagina
precedente).
FilterLogin(filter): controlla se la sessione è attiva e l’utente è loggato, in caso contrario rimanda alla
ViewLog.
filterautenticazione(filter): chiama una query in cui controlla se l’utente passato come parametro dall’URL
sia uguale a un attributo della sessione settato al login, in caso contrario chiama una pagina presa come
attributo della sessione(la pagina precedente).
WebappContextListener(listeners): è un listener che ha il compito di accedere e di chiudere il database.

# Parte grafica

La parte della grafica non è stata molto curata dal gruppo, in parte per mancanza di tempo, in parte perché
non pensavamo fosse parte fondamentale del progetto.
Nonostante tutto abbiamo deciso di utilizzare per tutto il sito lo stesso stile, ovvero stesso colore di sfondo,
stessi bottoni e stesso carattere, tutto questo è contenuto in diversi fogli di stile.
ConclusioniGrazie alle slide del corso, ad altre informazioni reperite su siti internet specializzati e soprattutto alla
cooperazione tra i componenti questa parte, è stata ovviamente impegnativa, ma i principali task del sito
sono stati svolti senza grossi problemi.
Le maggiori difficoltà sono derivate da alcuni obbiettivi che non avevamo capito in pieno o da problemi
oggettivamente più complicati, come la gestione degli avatar e dei file oppure la traduzione del testo
tramite il parsing.
Per questo progetto il gruppo ha lavorato in maniera coesa e tutti i componenti hanno fornito il proprio
apporto.
