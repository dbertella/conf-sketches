# Progressive Web App #PWA - Push notification by @ludomagno

Siti affidabili, veloci e coinvolgenti, attirare l'attenzione dell'utente e migliorare l'interazione con esso.
**Notification Api**

## Come strutturare le notifiche per interagire con l'utente
Sette punti simili sia per app che per Web. Una push notification ruba spazio allo schermo quindi si deve utilizzare con cautela.
* Personali, devono rivolgersi all'untente che la riceve, ex. nome della persona che riceve la notifica.
* In tempo reale
* Rilevante per l'utente
* Actionable: deve generare una azione per l'utente
* Concisa
* Funzionare in offline
* Non spammare, no ads in notifiche

## Set up
Ci sono due set up diversi, in firefox si ha bisogno solo dell'endpoint, in chrome c'è del setup da fare.
 - Developer console, creare un nuovo progetto per avere l'id
 - Attivare le api tramite la console e recuperare la browser key
 - Nel manifest si andrà ad aggiungere la key
In più serve avere abilitato il service worker.

## Processo di subscription
Informare l'utente della subscription creando una storia ad hoc per lui, informarli di come le può limitare o rimuovere. Non fare la subscription alla prima visita del sito ma in un momento chiave come nell'invio di un ordine di un carrello o nel momento della prenotazione di un aereo. Deve essere un valore aggiunto e non spam.
