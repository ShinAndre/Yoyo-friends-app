# YoYo - Friends
 
Web app in **Vue 3** per raccogliere e condividere i video della community di YoYo, la piattaforma che rende lo yoga accessibile a tutti, indipendentemente dalle capacità fisiche.
 
Progetto realizzato per il percorso start2impact.
 
## Funzionalità
 
- **Home "Friends"**: elenco dei video della community, caricati da un'API tramite Axios.
- **Scroll infinito**: i video compaiono 3 alla volta; quando si arriva in fondo alla lista ne vengono caricati altri 3, fino alla fine.
- **Video page**: player YouTube incorporato, con titolo, autore, visualizzazioni e descrizione.
- **Commenti**: sotto al video c'è un'area commenti. Il commento scritto nel form compare nell'elenco sopra al form. I commenti sono separati per video.
- **Responsive**: progettata mobile-first, con la griglia delle card che si adatta anche agli schermi desktop.
## Tecnologie
 
| Tecnologia | Uso |
| --- | --- |
| Vue 3 (`<script setup>`) | Framework dell'app |
| Axios | Chiamata AJAX all'API dei video |
| Bootstrap Vue Next | Componenti UI (card, bottoni) e layout |
| vue3-youtube | Embed dei video YouTube |
| IntersectionObserver (API del browser) | Rilevare la fine della lista per lo scroll infinito |
 
## API
 
I dati arrivano dal file `videos.json` del repository [yoyo-friends-api](https://anita-liberatore.github.io/yoyo-friends-api/).
Ogni video ha questi campi: `id`, `title`, `author`, `videoId`, `thumbnail`, `duration`, `views`, `description`.
 
L'API restituisce tutti i video in una volta sola, quindi la suddivisione "3 alla volta" è fatta lato client.
 
## Struttura del progetto
 
```
src/
├── App.vue                      # stato globale, header, scelta della schermata
└── components/
    ├── VideoList.vue            # card dei video + scroll infinito
    ├── intersectionObserver.vue # sentinella: segnala quando è visibile
    ├── VideoPage.vue            # player e dettagli del video
    └── CommentSection.vue       # elenco commenti e form
```
 
Sono 5 componenti in tutto, ciascuno ampiamente sotto le 400 righe.
 
### Responsabilità dei componenti
 
- **App.vue**: carica i dati con Axios, tiene lo stato (`videos`, `selectedVideo`, `comments`) e decide se mostrare la lista o la video page.
- **VideoList.vue**: mostra le card dei video visibili e aumenta il loro numero quando la sentinella lo richiede.
- **intersectionObserver.vue**: osserva un elemento vuoto in fondo alla lista ed emette un evento quando entra nello schermo. Non conosce i dati.
- **VideoPage.vue**: mostra il player e le informazioni del video selezionato.
- **CommentSection.vue**: mostra i commenti e gestisce il form di inserimento.
### Flusso degli eventi
 
I dati scendono dai componenti genitori ai figli tramite **props**, gli eventi salgono dai figli ai genitori tramite **emit**.
 
| Evento | Emesso da | Ricevuto da | Effetto |
| --- | --- | --- | --- |
| `visible` | intersectionObserver | VideoList | mostra altri 3 video |
| `select` | VideoList | App | imposta `selectedVideo` e apre la video page |
| `back` | VideoPage | App | azzera `selectedVideo` e torna alla lista |
| `submit` | CommentSection | VideoPage | inoltra il testo del commento |
| `comment` | VideoPage | App | salva il commento per il video corrente |
 
## Scelte per mantenere il codice semplice
 
- Nessun router e nessuno store: con due sole schermate basta un `ref` (`selectedVideo`) per decidere cosa mostrare.
- Tutto lo stato condiviso vive in `App.vue`, così c'è un solo posto in cui cercare i dati.
- I componenti figli ricevono props ed emettono eventi, senza modificare direttamente lo stato dei genitori.
- La lista visibile non è un secondo array da tenere aggiornato: è una `computed` che taglia l'array originale con `slice`.
- L'embed YouTube è affidato a una libreria, invece di gestire a mano l'API del player.
