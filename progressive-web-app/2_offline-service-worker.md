# Progressive Web App #PWA - Service worker by Silvia Righini - Offline by @granze

# Service worker
Worker che intercetta le richieste http, funziona in un thread separato e non ha accesso al dom.
Unico vincolo per utilizzare service worker, si possono registrare solo su server con https.

## Registrare un service worker
```javascript
if (navigator.serviceWorker) {
  navigator.serviceWorker.register('/sw.js', {
    scope: "/"
  });
}
```
Possiamo rimanere in ascolto di 3 diversi eventi come per esempio *fetch*, *push* e *sync*
```javascript
  self.addEventListener('fetch', event => {
      console.log(event.request)
    })
```
Il service worker verifica se una versione è differente da quella che ha salvato e nel caso modifica la pagina.

# Offline
SW: api di basso livello e event driven.
L'evento di fetch permette di controllare le richieste che vanno alla rete. Per esempio possiamo decidere di usare una versione cachata delle risorse.

2012 Application cache, api di alto livello non permette di controllare veramente le risorse che si vogliono rendere disponibili offline. **DEPRECATA**

Nuova **Cache Api** messa a disposizione dai service worker:
```javascript
const urlsToCache = ['/js/main.js', '/css/main.css']

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open('static-v1').then(cache => ...)
  )
})
```
## Perché "cacheare"
Per rendere disponibile una app offline, ma non solo per far credere a un utente che una app sia performante anche se non per forza lo è.
Page shell, scheletro dell'applicazione da mostrare all'untente prima che l'app si sia caricata.

## Strategie di cache
* Cache only
* Network only
* Cache first, falling back to network
* Network first, falling back to cache: caso d'uso se si hanno risorse da aggiornare frequentemente. Problema possibile di timeout.
* Cache then network: app di twitter

## sw-precache
Libreria di google per generare un sw data una lista di path di file da cacheare. Ogni risorsa aggiunta viene aggiornata una hash md5 per tenere conto della versione.

## sw-toolbox
Libreria che permette di gestire al meglio la nostra strategia di cache.

```javascript
importScripts()
```
ritorna un oggetto toolbox. Migliore debug, api per gestire una get con un timeout.

```javascript
toolbox.options.debug = true
toolbox.router.get('/api', toolbox.networkFirst)
```

Thank you @granze
![thank you @granze](https://pbs.twimg.com/media/ChnkVbdXIAE3Z5A.jpg:large)
