Curiosità: come gestite scenari in cui avete una factory che produce oggetti e volete verificare che gli oggetti siano del tipo che volete?

1. usate semplicemente un campo pesudo privato tipo `obj.isMyFuckingObj = true`
2. usate un campo pubblico con un Symbol come valore `obj.isMyFuckingObj = Symbol('myFuckingObjKey')`
3. usate una classe per sfruttare `instanceof`
4. controllate che l’oggetto abbia i metodi/fields che vi aspettate e quindi rispetti l’interfaccia pubblica che vi aspettate

agos [9:29 AM]  
5. uso un linguaggio con un check statico dei tipi? :stuck_out_tongue:
6. scrivo un test (edited)

cef62 [9:34 AM]  
> scrivo un test
assolutamente :wink:

> 5. uso un linguaggio con un check statico dei tipi
non è un opzione :wink:

rickdrink [9:51 AM]  
gli oggetti a @cef62 li prepara Samuel L. Jackson

agos [9:55 AM]  
non aggiungerei un campo, piuttosto testerei il comportamento dell'oggetto

cef62 [9:57 AM]  
concordo, infatti se non ho istanze di solito tendo a controllare l’interfaccia dell'oggetto
solo in dev-mode però non in produzione

gcanti [10:17 AM]  
@cef62 userei una classe + `instanceof` (metodo nominale) oppure una libreria per il type checking (metodo strutturale)
quindi 3) o 4)

cef62 [10:17 AM]  
+:+1:  la tua lib è in top alla lista per scenari complessi :slightly_smiling_face: (edited)

dej611 [10:19 AM]  
duck typing (4) (edited)

cef62 [10:19 AM]  
per le classi preferirei evitare di avere istanze se possibile
quindi `instanceof` è molto utile ma se posso seguo alternative :wink:

gcanti [10:20 AM]  
lo strutturale è più costoso ma lo puoi eseguire solo in dev

cef62 [10:20 AM]  
assolutamente
è un check che mi interessa solo in sviluppo
in produzione lo ignoro in ogni caso

gcanti [10:21 AM]  
se già usi babel e non vuoi lasciare "nessun segno" nella codebase puoi dare un'occhiata ai plugin di babel che fanno type checking

cef62 [10:22 AM]  
si li conosco ma preferisco qualcosa di esplicito, semplicemente rimuovo le espressioni condizionali con NODE_ENV === ‘development'
sto solo indagando le preferenze e le diverse tecniche :slightly_smiling_face:
è sempre interessante vedere come altri affrontano situazioni comuni ma non per forza banali :wink:

dej611 [10:27 AM]  
ieri hanno pubblicato un articolo su come integrare `flow` in un progetto che già utilizza babel

cef62 [10:27 AM]  
questo dici? 
https://medium.com/@thejameskyle/using-flow-with-babel-c04fdca8d14d#.v2eo3chr3

dej611 [10:27 AM]  
si

cef62 [10:30 AM]  
si è molto interessante

dej611 [10:32 AM]  
un qualcosa così ti risolverebbe le cose meglio del duck typing o dell’`instanceof` (che può essere facilmente ingannato)

cef62 [10:33 AM]  
verissimo
ma come team è stato deciso di non usare un type checker al momento
quindi per ora è un opzione che lascio da parte

pigoz [10:34 AM]  
al momento io sto usando tcomb, ma penso che presto farò il salto
tenendo comunque tcomb con il plugin

cef62 [10:34 AM]  
devo essere sincero che scrivere JS definendo i tipi mi lascia perplesso :confused:
non per i vantaggi

pigoz [10:35 AM]  
la cosa che non mi era piaciuta di flow al tempo, era che non c'era check a runtime

cef62 [10:35 AM]  
ma a pelle XD

dej611 [10:35 AM]  
ma flow funziona anche senza definizione dei tipi
con le definizioni funziona meglio, ma il motore sotto, almeno prima, inferiva il tipo

pigoz [10:36 AM]  
infatti anche a me lascia perplesso, ma infatti tcomb non è proprio un sistema di tipi, ma piu' un sistema di contratti

cef62 [10:36 AM]  
ma con le dipendenze di 3° parti come funziona ora?

pigoz [10:36 AM]  
e lo trovo estremamente utile

cef62 [10:36 AM]  
devi dichiararle? o le ignora se non le mappi

dej611 [10:37 AM]  
c’è spiegato nell’articolo

cef62 [10:37 AM]  
:wink: non lo ho ancora letto era nei preferiti, tnx
flow è parecchi mesi che non lo guardo

dej611 [10:37 AM]  
puoi o definirle tu, o scaricarle da un repo

cef62 [10:37 AM]  
ma se non le voglio?

dej611 [10:37 AM]  
ma immagino che altrimenti il motore cerchi di capirlo
non saprei

cef62 [10:38 AM]  
tnx :wink: mi informerò

gcanti [10:39 AM]  
attenzione però che per come funziona l'inferenza di flow potreste avere risultati sorprendenti a prima vista, occorre metterle le type annotation, almeno in punti strategici
questo ad esempio non solleva errori `const a = [1, 2, 3]; a.push('s')`
perchè flow, in assenza di annotazioni, inferisce il tipo dall'uso. Sopra `a` ha tipo `Array<number | string>`

cef62 [10:41 AM]  
interessante

gcanti [10:42 AM]  
mentre questo non passa `const a: Array<number> = [1, 2, 3]; a.push('s')`

dej611 [10:43 AM]  
nel primo caso non è sbagliato
è che se vuoi solo interi devi restringere il controllo giustamente

gcanti [10:45 AM]  
per typescript è un errore il primo esempio
per flow no

cef62 [10:49 AM]  
devo davvero trovare il tempo di approfondire i 2 sistemi e le differenze
grazie :wink:

gcanti [10:51 AM]  
flow è più adatto ad essere aggiunto ad una codebase senza annotazioni, ma contemporaneamente ha un type system più esigente quando inizi a metterle

gcanti [11:03 AM]  
semplificando le cose l'algoritmo funziona così: flow si passa tutto il codice (compreso i node_modules), e tagga ogni variabile con un tipo "aperto", ovvero continua ad aggiungere union inferendo dall'uso nel codice, se non trova inconsistenze non solleva errori
quindi se lanci flow su una codebase non annotata e non solleva errori non significa che non ci sono bug ma perlomeno sei sicuro che non ci siano inconsistenze
per inconsistenza intendo per esempio mappare la `a` del primo esempio con una funzione che accetta solo numeri
questo solleva un errore `const a = [1, 2, 3]; a.push('s'); a.map(x => 2 * x)`

gcanti [11:09 AM]  
quindi lanciare flow sulla tua codebase, anche se non sei disposto ad aggiungere annotazioni, può essere comunque utile
le annotazioni possono essere aggiunte anche tramite commenti, il procedimento quindi può essere molto graduale e poco invasivo

dej611 [11:14 AM]  
questo è quello che ho fatto qui dove sono. Nessun inconsistenza trovata
Però il rendere tutto più restrittivo dipende molto da quel che devi produrre, a volte devi convivere anche con robe legacy che non ti permettono di chiudere i cancelli alle stringhe negli array :disappointed:

sa_su_ke [11:28 AM]  
@cef62: Io ho sempre usato linguaggi dinamici. È una questione di abitudine. Io oramai metto i tipi ovunque non ci fai neanche caso. Arrivi a passare più tempo a combattere contro il compilatore che contro il browser :-) ti fa risparmiare molto tempo

cef62 [11:35 AM]  
:slightly_smiling_face: ho ben presente
qualcuno con cui litigare c’è sempre XD

zanza00 [11:39 AM]  
in teoria il compilatore dovrebbe essere più furbo del browser

sa_su_ke [11:42 AM]  
Si nel senso che molti errori che prima te li segnalava il browser te li triggera' il compilatore. Passi più tempo nel compilatore che nel browser
