# Progressive Web App #PWA - Making it installable and Instant loading Gilberto Cocchi

## Installare la web app sul proprio dispositivo
Il Manifest viene usato anche per definire come salvare sul proprio device android la web app. File json di configurazione.

* Utente va nel sito, clicca sul menu di chrome add to home screen e scarica il Manifest
* Gli viene chiesta conferma
* Un icona viene installa sulla home screen

La web app a questo punto si apre in full screen senza dover aprire il browser. Si può abilitare o meno la barra degli indirizzi.

Si può fare l'auto prompt che permette all'utente di salvare il sito sull'home screen senza passare dalle impostazioni di chrome. Questo è abilitato di default su chrome mobile se si naviga su un sito per almeno 2 volte con una distanza di non più di 5 minuti(??).

Lato sviluppo questa cosa si può controllare per renderla più user friendly.

## Instant loading
Ogni step che un utente deve fare fa perdere il 20% di essi

* Compression: minificazione, immagini webp(funziona solo su chrome praticamente)
* Round Trips: domini esterni, link attribute rel, prefetch etc, Caricamento di javascript, attributi *defer* o *async*
* Strategia di Cache: header del server last-modified, etag, if-modified-since, if-none-match, cache-control, expires
  Come definire **cache-control** e definire una max age?
  Usare un framework che pone un hash sul file di modo da settare max age su years. index.html con master-validate quindi cache breve
* CDNs

HTTP/2 esperimento di google engeneers
In http si può fare una connessione tcp alla volta e il canale non si può utilizzare se è già presente una connessione tcp.
Http/2 utilizza una sola connessione ma le richieste diventano degli stream, tante richieste di file piccoli vengono concatenati in un unica connessione. Si possono inviare dati che hanno origine diversa.
Multiplexing della stessa connessione che a loro volta vengono divisi in frames con diversi headers, gli headers ripetuti fra loro vengono salvati in cache e non vengono ripetuti.
Retrocompatibile con http.
