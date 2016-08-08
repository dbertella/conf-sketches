
Matteo Ronchi [10:16 AM]  
@gianmarcotoso @michele @lucacolonnellocrweb e chiunque altro abbia input utili, ho una domanda per voi:
Supponete di avere un manager/oggetto che ha dei valori che sono esposti via `context` ai children. Ma vorreste che alcuni rami usassero delle diverse configurazioni di valori. Ovviamente se ogni componente che usa questi valori ha nozione della chiave che deve chiedere all’oggetto nel context funziona tutto ma supponiamo che io volessi definire solo nel nodo ​*X*​ un nuovo set di valori e imporre a tutti i children/descendant questo set di valori ma senza passare una chiave esplicita ad ogni componente descendant…
L’uso del `context` è opzionale se ci sono altri approcci che funzionano

**TL,DR;** per riassumere la radice ha 4 figli e 3 ricevono il colore rosso, ma uno e tutti is uoi children voglio ricevano blue definendolo solo in quel nodo padre di quel ramo

Michele Bertoli [10:20 AM]  
bhe' il context funziona gia' cosi' nel senso di sovrascrivere i valori per i figli 

Matteo Ronchi [10:21 AM]  
si ma è globale no?
cioè se sovrascrivo anche gli altri rami ricevono la modifica
o clona ad ogni step (ammetto che non ho verificato)
ma mi sembra molto costoso in termini di risorse clonare il context...
anche lo facesse solo `shallow`

Luca Colonnello [10:23 AM]  
ho capito
al di là del modo, il context ha un problema a riguardo
il figlio deve definire cosa vuole che i suoi figli vedano
quindi fondamentalmente per come funziona ora con l’attuale context di react penso ci sia troppo effort da distribuire
se invece creassi un container?

Matteo Ronchi [10:24 AM]  
aspe
il context fa quello che mi serve

Luca Colonnello [10:24 AM]  
certo

Matteo Ronchi [10:24 AM]  
la radice definisce nel context `color`
e tutta l’app li riceve

Luca Colonnello [10:24 AM]  
si

Matteo Ronchi [10:24 AM]  
se poi un nodo aggiunge altri valori solo i suoi figli li vedranno (edited)

Luca Colonnello [10:24 AM]  
certo

Michele Bertoli [10:24 AM]  
http://www.react.run/rJ4F2T-t/2 (edited)

Matteo Ronchi [10:25 AM]  
ma `color` lo vedranno sempre

Luca Colonnello [10:26 AM]  
ma tu vuoi che color sia diverso solo da un certo nodo in poi e non vuoi specificarlo in quel nodo ma nel padre di tutti giusto?

Michele Bertoli [10:26 AM]  
> e non vuoi specificarlo in quel nodo ma nel padre di tutti
ah ok non avevo capito

Matteo Ronchi [10:26 AM]  
@michele si l’idea è quella
immaginatevi lo scenario
parliamo di un theme-manager per esempio
anche se si può applicare a tanti casi (edited)
l’app usa il tema `white`
ma voglio che una modale e tutto il suo contenuto usi il tema `red`
però non tutti i nodi della modale sono consapevoli del `theme-manager`
quindi non posso semplicemente passare dalla modale ai figli che il tema è `red` e cmq passare la prop esplicitamente non mi piace (edited)
l’esempio di Michele è nella direzione che desidero
ora devo vedere se funziona anche con oggetti e non solo plain props
però questo espone il fatto che il context viene ricalcolato potenzialmente ad ogni render
e non mi piace molto
@michele: fighissimo react.run :slightly_smiling_face:

Michele Bertoli [10:29 AM]  
http://www.react.run/rJ4F2T-t/3 (edited)
funziona anche con oggetti
 
Matteo Ronchi [10:30 AM]  
@michele quindi ogni ramo può sovrascrivere valori senza impatatre gli altri rami (edited) 
questo non lo sapevo

Michele Bertoli [10:30 AM]  
@cef62: yep
l'ho scoperto qui: https://twitter.com/ryanflorence/status/717597674040467456
 Ryan Florence @ryanflorence
@MicheleBertoli cool, but I think you are now limited to a single Provider in the app, no? The second you provide twice it shadows __data.
TwitterApril 6th at 8:19 AM 

Matteo Ronchi [10:33 AM]  
figo @michele!

Luca Colonnello [10:30 AM]  
ok allora io ti prppongo questo:  
crei un themeManager locale
o meglio un themeManager globale che in pattern factory possa essere ricreato a partire dall’oggetto base con config differenti

Matteo Ronchi [10:31 AM]  
@lucacolonnellocrweb: una cosa, io non posso sapere sempre che c’è un override eslicito
mi spiego
la dialog che usa `red` contiene altri container e alcuni dei loro children accedono al theme mangare, per fare override dovrebbero essere consapevoli del cambio di tema
quindi deov cmq usare il context

Luca Colonnello [10:32 AM]  
no beh certo
ma se il themeManager è un istanza inserita nel context
che tutti usano e da cui recuperano variabili tipo il color
basta solo reistanziare il theme manager con config differenti nel nodo dal quale vuoi si applichi il tema red
e tutti i figli lo useranno indipendentemente da quello che il padre ha impostato
sbaglio?

Matteo Ronchi [10:33 AM]  
mmmm io mi immagino (visto che michele ha sdoganato l’override x nodi)
che la radice inietta un unico them manager nell'app
e ha un default theme

Luca Colonnello [10:33 AM]  
si
esatto

Matteo Ronchi [10:34 AM]  
poi il nodo dialog modifica il valore del theme da `white` a `red`

Luca Colonnello [10:34 AM]  
esatto
quello intendevo

Matteo Ronchi [10:34 AM]  
e tutti i suoi figli, che accedono all’unico theme manager

Luca Colonnello [10:34 AM]  
forse mi sono spiegato male

Matteo Ronchi [10:34 AM]  
useranno red

Luca Colonnello [10:34 AM]  
il problema è che devi creare un nuovo theme manager
perchè se no le modifiche hanno effetti anche sui padri
quindi il figlio dovrebbe fare tipo

Matteo Ronchi [10:34 AM]  
no non è vero, basta che il them manager supporti + temi
e tu gli chiedi il tema tramite la chiave

roberto diana [10:35 AM]  
... of topic ... ma il context non verrà abbandonato in futuro ? ..

Matteo Ronchi [10:35 AM]  
abbandonato difficilmente
potrebbero cambiare le API
se tolgono la DI in react perdono accesso a tutte le cose fighe che si stanno producendo

Luca Colonnello [10:35 AM]  

```
	class First extends React.Component {
	    getChildContext() {
	        return { themeManager: this.context.themeManager.clone({
				// settings
				theme: 'red',
			}) }
	    }
	    render() {
	        return <AnotherChild />
	    }
	}
```

Matteo Ronchi [10:35 AM]  
ah ok luca
in realtà preferisco una singola istanza che tiene una mappa di temi
e cambiare solo la chiave attiva del tema
però il risultato è simile
Figata cmq grazie a entrambi!

Luca Colonnello [10:36 AM]  
però così facendo vuol dire che i figli devono sapere che tema richiedere al themeManager

Matteo Ronchi [10:36 AM]  
questo mi apre delle possibilità molto fighe sul progetto che sto sviluppando (edited)

Luca Colonnello [10:36 AM]  
come dicevo io è implicito per i figli (edited)

Matteo Ronchi [10:37 AM]  
vero luca
ma se i figli devono solo fare:
`const themmanager = this.context.thememanager(this.context.theme)`
è un male minore
però si la tua soluzione risulta più pulita
in realtà mi preoccupa poco la differenza perchè sarà tutto astratto in un HoC
quello che vorrei evitare è di avere più di un istanza del theme manager
ci lavoro su cmq
poi vi faccio sapere!

Luca Colonnello [10:39 AM]  
un altra soluzione potrebbe essere utilizzare una prop

Matteo Ronchi [10:40 AM]  
no non è sostenibile
non tutti i componenti sono a conoscenza del theme-manager

Luca Colonnello [10:40 AM]  
ma la prop serve solo a definire quale tema vuoi da li in poi, però per propagare la cosa ai figli deve essere implicita secondo me...

Matteo Ronchi [10:40 AM]  
intendo che se la dialog riceve come prop il tema `red`

Luca Colonnello [10:40 AM]  
si usando il context potrebbe andare
ma ovviamente hai il problema al contrario
la variabile theme nel context diventa una dipendenza esplicita
può fungere lo stesso però

Matteo Ronchi [10:41 AM]  
lo dovrebbe passare esplicitamente a tutti i children, ma cosa succede se un children non sa cosa farsene della prop `them` e non la passa au suo descendant? (edited)
beh si un tradeof c’è sempre

Luca Colonnello [10:41 AM]  
no infatti intendevo di passarlo solo al primo padre che deve sovrascrivere
però facendo così stai generando un istanza del theme manager ad ogni componente
che lo usa

Matteo Ronchi [10:42 AM]  
esatto
vorrei evitarlo

Luca Colonnello [10:42 AM]  
invece mettendolo nel context solo chi lo cambia lo sostituisce
però una soluzione potrebbe essere non reinstanziare ma cambiare a runtime il tema dal padre al figlio
senza riprodurre oggetti
un po come immutable (edited)
immagina che il themeManager non è un instanza pura
ma una rappresentazione dei metodi pubblici
quindi un oggetto plain

Matteo Ronchi [10:44 AM]  
yep

Luca Colonnello [10:44 AM]  
che all’interno si riferisce alla vera istanza di themeManager
ma usa le config dell’oggetto per lavorare
tipo
red:mainColor
è il modo in cui recuperi il colore
ma red non lo usano i componenti (edited)
usano solo mainColor
chiamando funzioni di un oggetto plain
che conosce sia red che il themeManager
e in reflection chiama
themeManager.get(`${this.theme}:${propName}`)
quindi themeManager pubblico è una factory di oggetti di questo tipo
e così il padre può semplicemente fare come ti dicevo
ma non ricrea tutto
ma solo il POJO pubblico
che ha 3 cose
o forse bastano 2
il get e il tema attuale

Matteo Ronchi [10:46 AM]  
si può funzionare, molto dipende da come sarà internamente il themmanager (edited)
ma l’idea è questa
la API sono 3 imho: get, set, theme
in realtà 4
ci vuole anche register
per aggiungere nuovi temi

Luca Colonnello [10:48 AM]  
esatto
così da quello ne crei altri nei figli che vogliono sovrascrivere

Matteo Ronchi [10:48 AM]  
sarebbe figo farlo senza neanche il context

Luca Colonnello [10:49 AM]  
e questo potrebbe non essere un themeManager ma un wrapper per la DI con il context

Matteo Ronchi [10:49 AM]  
ma vorrebbe dire creare un meccanismo di DI ad-hoc

Luca Colonnello [10:49 AM]  
una roba tipo Pimple in PHP ma per React
no no sempre con il context
serve solo a definire questo meccanismo dinamico ma pulito

Matteo Ronchi [10:49 AM]  
> e questo potrebbe non essere un themeManager ma un wrapper per la DI con il context
spiega melgio (edited)

Luca Colonnello [10:50 AM]  
se io genero un wrapper che ingloba un oggetto, e gli passo una factory con impostazioni (utile all’inizio e a fare anche il register)
può wrappare qualsiasi oggetto
le api le definisco io
lui definisce il meccanismo
se io creo get e gli faccio fare quello che dicevamo
è tutto flessibile e generico
e va bene per qualunque cosa
lui definisce solo l’interfaccia pubblica
che però è risultato di una factory che crei tu
quindi può fare qualunque cose secondo questo meccanismo
anche un sistema di settaggi differente da un tema
o di oggetti
veri e propri derivati da classi

Matteo Ronchi [10:52 AM]  
gotcha, si infatti il theme manager era solol’esempio più facile su cui discutere (edited)

Luca Colonnello [10:53 AM]  
:slightly_smiling_face:

Matteo Ronchi [10:54 AM]  
poi non so quanto abbia senso fare un sistema generico (nel mio caso attuale) ma vedremo… sett prox ci lavoro attivamente :slightly_smiling_face:
grazie a entrambi ragazzi!

Luca Colonnello [10:57 AM]  
:slightly_smiling_face:

roberto diana [10:57 AM]  
raga ma che ce relazione fra context e DI ? o forse no c'è relazione ... ? (edited)

Matteo Ronchi [10:58 AM]  
il `context` è l’unico meccanismo nativo che React offre per avere qualcosa di simile alla DI
concettualmente parlando non è la stessa cosa ma offre la possibilità di accedere a dati non forniti direttamente dal parent container

roberto diana [10:59 AM]  
ah! ok
non riuscivo a collegare con la  DI (edited)

Matteo Ronchi [10:59 AM]  
in `angular` tu inietti un servizio

roberto diana [10:59 AM]  
in angular è chiarissimo in react meno
(per me ... obv)

Matteo Ronchi [11:00 AM]  
in `react` passi props, ma se devi passare un servzio ail pronipote dovresti passare il servizio anche al figlio e al nipote
il context ti permette di passare al pronipote senza passare esplicitmaente tra i nodi intermedi

roberto diana [11:01 AM]  
che comunque non è proprio DI ma ho capito meglio adesso :smile:

Matteo Ronchi [11:01 AM]  
esatto :wink:
React, a quanto mi risulta, non usa Reflection o altre tecniche per iniettare il context, semplicemente lo gestisce per te lungo l’albero di componenti

roberto diana [11:02 AM]  
lo clona ? o usa reference o qualche meccanismo simile ?

Matteo Ronchi [11:04 AM]  
se leggi il thread sopra ci sono gli esempi di michele che mostrano che ogni volta che un nodo definisce qualcosa nel context viene prodotto un nuovo context che mantiene relazione con ciò che è definito dai livelli superiori ma che permette un `safe` override da quel punto in poi per un dato ramo
questo http://www.react.run/rJ4F2T-t/3

roberto diana [11:05 AM]  
missato ora leggo grazie

Matteo Ronchi [11:05 AM]  
:wink:

-----------

Luca Colonnello [4:33 PM]  
Ragazzi volevo porvi una soluzione che ho trovato ad un problema
voglio creare container redux che permettano di sovrascrivere il dumb che usano (non usando props dall'esterno)
con un default dumb nel caso in cui non si voglia sovrascrivere
ho pensato a questo

Luca Colonnello [4:34 PM]  
```
import React, { Component } from 'react';
import { compose } from 'redux';
import { connect } from 'react-redux';
​
// supposed to receive a class and return a new function (curryed) that
// if called add a render function to the received class,
// rendering the received component passing down all the props
import { withRender } from './utils';
​
// container class with event handler and action creator dispatch
class AContainer extends Component {
  onBtnClick() {}
  componentDidMount(){}
}
​
// factory of AContainer, composed by redux connect and withRender
export const AContainerWithRender = compose(
  connect(() => {}, {}),
  withRender(AContainer)
)
​
// default version using A as dumb component
export default AContainerWithRender(A);
​
​
​
// usage in another file with default render
import React from 'react';
import AContainer from './AContainer';
​
React.render(<AContainer />, ...);
​
​
​
// usage in another file with custom render
import React from 'react';
import { AContainerWithRender } from './AContainer';
​
const AContainer = AContainerWithRender((props) => (
  <div>My custom renderer</div>
));
​
React.render(<AContainer />, ...);
​
```

Luca Colonnello [4:35 PM]  
una utility withRender, curryed, che ricevuta una classe, la estende aggiungendo il render
ditemi che ne pensate e se è chiaro

Matteo Ronchi [4:39 PM]  
ma quale sarebbe il caso d’uso?
cioè è chiaro che ti permette di definire un wrapper redux e di riutilizzarlo più volte
ma in che scenario lo vedi utile/necessario?

Luca Colonnello [4:49 PM]  
creo un toolkit di componenti che usano redux per separare logica delle interazioni rispetto a grafica
voglio permettere all’esterno a chi lo usa di ridefinire solo la view
quindi il container non voglio debba essere ricreato ogni volta
ma creo il mio componente tab
e chi lo usa usa la grafica che offro o se la riscrive
pensi possa essere una cazzata?
chiedo eh… condivido per quello :slightly_smiling_face:

Matteo Ronchi [4:53 PM]  
non basta creare una factory che riceve il componente e lo restituisce wrappato dal container? Cioè stilisticamente è figo ma non vedo perchè dovrei usare `withRender` quando posso semplicemente esportare dei moduli che fanno la stessa cosa.. l’unico vantaggio è il default, ma perchè dovrei usare un default?
non so se mi sono spiegato
non critico l’approccio semplicemente cerco di capire che reale vantaggio potrei avere rispetto a creare moduli `es` che esportano una funzione che accetta un componente e lo wrappa con connect
con il tuo componente devo introdurre una dipendenza esterna che mi risparmia poche righe

Luca Colonnello [5:00 PM]  
si ma manca il default
questo serve a standardizzare l’approccio in FP

Matteo Ronchi [5:01 PM]  
si ma no vedo un caso reale in cui il default possa servire

Luca Colonnello [5:01 PM]  
se tu fai un toolkit
o cmq componenti riutilizzabili
il container ha già la grafica

Matteo Ronchi [5:01 PM]  
si ma chi mai userebbe il default?

Luca Colonnello [5:01 PM]  
???
se prendo il tab di material ui è fatto così

Matteo Ronchi [5:01 PM]  
cioè se lo uso vuol dire che il default è il mio componente reale
si ma loro ti danno un component con UI e il default tab serve

Luca Colonnello [5:02 PM]  
esatto

Matteo Ronchi [5:02 PM]  
il default con un connect non serve
perchè non lo useresti mai

Luca Colonnello [5:02 PM]  
se tu esporti un componente che ha una parte redux e una parte react
la parte redux è connect + reducer + actions + selectors (edited)
la parte react è il dumb + il container del connect (edited)

Matteo Ronchi [5:02 PM]  
vero ma continuo a non aver bisogno del default
crei una factory che accetta il componente e lo wrappa con tutta la parte redux
hai anche test + facili

Luca Colonnello [5:03 PM]  
si ma il tuo toolkit non offre quindi un default render??
me lo devo fare ogni volta  a mano (edited)
chiaramente è un caso specifico

Matteo Ronchi [5:04 PM]  
facciamo un caso reale

Luca Colonnello [5:04 PM]  
ok

Matteo Ronchi [5:04 PM]  
io prendo il tuo toolkit e lo uso nella mia app

Luca Colonnello [5:04 PM]  
si

Matteo Ronchi [5:04 PM]  
per quale motivo dovrei usare il tuo default renderer?
tu non hai la mia UI, non sai cosa ci metto dentro e non usi di sicuro le mie CSS
quindi prendo la tua utility e gli passo un mio componente
al che mi chiedo
perchè uso questa lib, se gli devo fornire tutti i building blocks?
non posso farmi io un modulo che accetta il componente e che internamente ha i building block che comunque mi devo scrivere (actions, connector, etc..) (edited)

Luca Colonnello [5:06 PM]  
l’obiettivo del toolkit dovrebbe essere offrire già tutti i copmonenti funzionanti, personalizzabili nello stile

Matteo Ronchi [5:06 PM]  
aspetta

Luca Colonnello [5:06 PM]  
ok

Matteo Ronchi [5:06 PM]  
ma parliamo di uno UI toolkit? (edited)

Luca Colonnello [5:06 PM]  
beh si

Matteo Ronchi [5:06 PM]  
e perchè mai uno dovrebbe usare uno UI toolkit vincolato a redux?

Luca Colonnello [5:06 PM]  
è una scelta implementativa

Matteo Ronchi [5:07 PM]  
le mie critiche erano legate al fatto che pensavo che tu fornissi solo le utilities :wink:

Luca Colonnello [5:07 PM]  
ah no no
questa è un esigenza specifica
cmq non è solo redux il punto
quello era per dimostrare che con la composition si può fare anche con il connect
ma se io voglio creare come con recompose
dei comp stateless ma che hanno con degli HoC dei comportamenti legati al componente stesso
gestione dello stato magari

Matteo Ronchi [5:08 PM]  
se mi dici che tu offri una lib, la cui logica è redux-based e che vuoi permettere ai tuoi user di cambiare la dumb part del componente, mantenendo ovviamente le props che tu gli passi, allora lo snippet ha senso

Luca Colonnello [5:08 PM]  
così posso sovrascrivere il render ma avere stato e altri HoC
si si esatto

Matteo Ronchi [5:09 PM]  
anche se non so se userei una lib che mi impone i suoi reducer per funzionare, anche se ha un suo perchè)
però ci dovrei riflettere, a pelle non me gusta
gli UI comp dovrebbero essere completamente isolati

Luca Colonnello [5:09 PM]  
si ma come ti scrivevo

Matteo Ronchi [5:09 PM]  
personalmente non vedo male lo stato di un componente react se è specifico ad azioni locali

Luca Colonnello [5:09 PM]  
possono anche non essere vincolati a redux
si si
ma lo fai uguale
immagina recompose
ho il dumb e con gli HoC creo lo stato e i reducer dello stato

Matteo Ronchi [5:10 PM]  
però aspetta se togli il layer redux e lasci cambiare il componente praticamente chiede all’utente di rifare ilt uo lavoro
solo x componenti davvero banali funzionerebbe la sostituzione così com'è

Luca Colonnello [5:10 PM]  
se il componente è stateful no

Matteo Ronchi [5:10 PM]  
si ma a quel punto devi incastrarti con lemeccaniche di aggiornamento dello stato
è figo ma complesso

Luca Colonnello [5:11 PM]  
se vai in composition è esattamente come il concetto di dumb e container
come funge anche recompose

Matteo Ronchi [5:12 PM]  
si ma non vedo come per esempio puoi gestire il custom rendere di una  select, sarebbe mooolto complicato

Luca Colonnello [5:12 PM]  
scusa non ho capito

Matteo Ronchi [5:13 PM]  
supponi che uno dei tuoi componenti è una `select` (edited)

Luca Colonnello [5:13 PM]  
si
una select è stateless
non ha bisogno di tale logica

Matteo Ronchi [5:13 PM]  
se io voglio cambiare la UI devo scrivere uno sproposito di codice
mmm statelss +o-
se ha multiselezione, elementi interattivi etc...

Luca Colonnello [5:14 PM]  
nel caso di un ui toolkit dovrebbe essere stateless

Matteo Ronchi [5:14 PM]  
guarda react-select

Luca Colonnello [5:14 PM]  
certo in quel caso funzionerebbe
devi scrivere molto codice

Matteo Ronchi [5:14 PM]  
si ma per fare un renderer ci impiego una vita

Luca Colonnello [5:14 PM]  
ma la tua esigenza è proprio quella di rifarlo da 0 se vuoi cambiare il render
sarebbe peggio rifare tutto da 0, grafica + logica!!

Matteo Ronchi [5:15 PM]  
si ma vuol dire che la tua logica deve andarmi perfettamente bene

Luca Colonnello [5:15 PM]  
se così non fosse non usi quel componente
ma se fai un tab ma il render deve essere modificato
così lo puoi fare

Matteo Ronchi [5:16 PM]  
sisi è chiaro il vantaggio
semplicemente, ma è molto personale come punto di vista, se uso la tua lib e la personalizza in maniera forte molto probabilmente la riscrivo come serve a me
l’effort è maggiore ma non dipendo dalle tue scelte di design (del codice) e di utilizzo

Luca Colonnello [5:17 PM]  
si ma se non ti capita questo caso, non usi la lib
o meglio
se l’implementazione è molto differente nella logica e nel render certamente non ha senso
è un caso limite magari, ma facendolo in FP è compatibile con molte altre cose
come appunto recompose o aprhodite

Matteo Ronchi [5:18 PM]  
quello assolutamente
tra l’altro tu susi aphrodite?

Luca Colonnello [5:19 PM]  
si cmq è un utility che fornisce un approccio per risolvere questo problema che si ha spesso se sviluppi ui toolkit in react

Matteo Ronchi [5:19 PM]  
l’approccio è interessante ma non mi piace molto la sintassi da usare e neanche il fatto che vada a intercettare tutte le mie render function

Luca Colonnello [5:19 PM]  
spesso non hanno logica complessa ma il fatto di non poter toccare la grafica è brutto

Matteo Ronchi [5:19 PM]  
concordo

Luca Colonnello [5:19 PM]  
> il fatto che vada a intercettare tutte le mie render function

cosa intendi?
volevo usare aphrodite per la stessa ragione, fare qualcosa del genere con withStyle tipo

Matteo Ronchi [5:22 PM]  
no spe forse mi confondo
ne sto guardando troppe assieme

Luca Colonnello [5:22 PM]  
ahahahah
è quella di Khan Dods (edited)

Matteo Ronchi [5:22 PM]  
è radium che wrappa la rende function
sorry

Luca Colonnello [5:22 PM]  
si si
esatto
infatti non me gusta molto

Matteo Ronchi [5:23 PM]  
non so tra iniettare css e usare stili inline quale mi piace di più :stuck_out_tongue:

Luca Colonnello [5:23 PM]  
preferisco iniettare css onestamente
lo stile inline ha troppe limitazioni

Matteo Ronchi [5:24 PM]  
beh in realtà sono tutte aggirabili (vedi radium)

Luca Colonnello [5:24 PM]  
eh ma un hover fatto in css non è un hover fatto in js
perdi after e before e animation
hai solo transition
se non mi sbaglio eh

Matteo Ronchi [5:26 PM]  
beh si molto dipende da cosa devi fare
personalmente non sono molto preoccupato lato animation e simili

Luca Colonnello [5:26 PM]  
beh si certo dipende

Matteo Ronchi [5:26 PM]  
anche le mediaquery le gestiamo cmq via JS perchè carico proprio layout applicativi diversi

Luca Colonnello [5:26 PM]  
beh si

Luca Colonnello [5:46 PM]  
piccola osservazione: se usi recompose questo withRender non serve, in quanto basta comporre tutto con recompose e passare tutto a connect
recompose offre l’enhancer che fa una roba simile
connect sarebbe una funzione composta come le altre
e il default component sarebbe un componente che è già stato decorato con l'enhancer
e non crei lib ulteriori e rimani standard
se hai classe invece ti serve una roba come questa (edited)
scelte implementativa

----------

fabiobiondi [5:27 PM]  
ma voi non lavorate mai? :smile:

Matteo Ronchi [5:27 PM]  
certo è quello che facciamo reglarmente

fabiobiondi [5:27 PM]  
potreste fare delle chat audio su sta roba.. sarebbero interessantissime (edited)

Michele Bertoli [5:28 PM]  
no ma infatti ragazzi questa roba va salvata

Michele Bertoli [5:29 PM]  
grazie a questa community faccio meglio il mio lavoro

fabiobiondi [5:30 PM]  
sono d’accordo 100%.. ma volevo rompere le scatole a Luca e Mat : P

Michele Bertoli [5:30 PM]  
anzi nelle mie prossime presentazioni
posso mettere
member of FEVR, vero?

fabiobiondi [5:30 PM]  
in realtà dovresti citare angularjs developers italiani e react italia….

Michele Bertoli [5:30 PM]  
(anche se non sono mai venuto ad un evento live :( - a parte le conf)

fabiobiondi [5:30 PM]  
sono le nostre : P
sono i canali in cui promuoviamo slack (edited)
il FEVR ci ospita

Michele Bertoli [5:34 PM]  
okappa

[5:34]  
pero' non diro' mai angular in una mio talk ok?

[5:34]  
:D

fabiobiondi [5:34 PM]  
però penso che non possano esser contenti se citi il FEVR

[5:34]  
aahahhahahahah

[5:34]  
ahahhaha

[5:34]  
(non sei così aperto come pensavo allora : P )

Michele Bertoli [5:34 PM]  
ahahahahahahahahahahah

fabiobiondi [5:35 PM]  
:angular:

Michele Bertoli [5:35 PM]  
:wat:

fabiobiondi [5:36 PM]  
ahahah

Luca Colonnello [5:37 PM]  
ahahaha Fabio io oggi sono di formazione

[5:37]  
abbiamo fatto questo :slightly_smiling_face:

[5:38]  
@michele quando racconti taggami che non voglio perdermelo

[5:38]  
:slightly_smiling_face:

Michele Bertoli [5:38 PM]  
;)

----------------


Matteo Ronchi [9:10 AM]  
domandona del lunedì mattina: cosa ne pensate di `aphrodite` (no pun) @michele @lucacolonnellocrweb @danieleb @gianmarcotoso

Luca Colonnello [9:11 AM]  
Per me è favoloso, ma non l'ho ancora provato.. l'idea di poter creare pacchetti altamente modificabili da css è stupenda.. bisogna vedere un po' per il debug che magari può diventare morboso, ma alla fine non è proprio un compilato quindi quello che vedi in console può bastare

Michele Bertoli [9:12 AM]  
Aphrodite è una delle soluzioni più sensate
Anche perchè è arrivata quando l'universo dei css in js era stato già esplorato
E altri progetti erano falliti

Matteo Ronchi [9:13 AM]  
si infatti il concetto mi piace molto, unica cosa che non mi esalta è che ogni volta che cambi una prop css crei una classe nuova

Michele Bertoli [9:13 AM]  
Mette insieme la comodità degli oggetti js per gli stili, con l'output di vero css

Matteo Ronchi [9:13 AM]  
mi piace molto di più degli inline styles puri

Michele Bertoli [9:14 AM]  
Yes
Una feature molto interessante

Matteo Ronchi [9:14 AM]  
vedevo che loro consigliano di usare `react-look` come alternativa che ora è deprecated in favore di `react-fela`

Michele Bertoli [9:14 AM]  
È che inietta solo gli stili attualmente in uso
L'autore di fela
È co-maintainer del mio repo

Matteo Ronchi [9:15 AM]  
:open_mouth:
e cosa cambia / ne pensi tra fela e aphrodite?

Michele Bertoli [9:15 AM]  
È un tipo molto smart

Matteo Ronchi [9:16 AM]  
una cosa che ha peso per aphrodite è anche che è sviluppata appositamente per paypal quindi dovrebbe avere una longevità/continuità più stabile (edited)
anche se non so su quali app la usano
ah no vedo che lo usano in produzione

Michele Bertoli [9:19 AM]  
Sisi la usano in prod in paypal
Infatti
Secondo me fela ha delle feature anche più interessanti
Es. Plugins
Ma non diventerà mai così popolare
Una soluzione da tenere d'occhio
È https://github.com/geelen/css-components-demo
Sta nascendo in sti giorni ma ha un grande potenziale

Matteo Ronchi [9:22 AM]  
interessante!
però come x radium non  mi esalta il fatto che decori/modifichi il comportamento/output di react

Michele Bertoli [9:24 AM]  
Giusto

Matteo Ronchi [9:30 AM]  
quindi per fare tirare le somme: 
* `aphrodite` è usata da progetti grandi, non pare avere particolari drawback se non in scenari dove generi davvero tanti nuovi stili in continuazione. Non è legata solo a React
* `radium` anch’essa usata da grandi progetti ha + limiti verso media queries e altri aspetti css ma non ha altri grandi limiti. E’ legata a react e wrappa il `render` method
* `react-fela` è un pò un aphrodite on steroids ma non ha usi reali in grandi progetti ed è mantenuta da un singolo dev. Come radium si interpone/modifica i comportamenti di React per funzionare

Luca Colonnello [9:56 AM]  
a me onestamente non preoccupa molto il casso della generazione live dello style
penso che faccia memoizzazione

Matteo Ronchi [9:57 AM]  
si fa memoization
però la chiave è la serializzazione in JSON dell’intero oggetto passato alla lib
quindi imho va gestito con attenzione quando generi/rigeneri gli stili usando `css()` (edited)
se no è vero che non genera nuove classi css ma fai cmq serializzare uno sproposito di oggetti ad ogni render

Michele Bertoli [10:00 AM]  
eccomi in ufficio

Matteo Ronchi [10:00 AM]  
:slightly_smiling_face:

Michele Bertoli [10:00 AM]  
ottime conclusioni @cef62
radium tra l'altro non sono sicuro sia utilizzato davvero in progetti grossi
(a parte le cose che fa formidable labs)

Matteo Ronchi [10:00 AM]  
loro sul sito dicono che è usato da FB stessa :stuck_out_tongue:

Michele Bertoli [10:00 AM]  
un'altra alternativa
che mi dimentico sempre perche' il tizio mi sta antipatico
e' https://github.com/cssinjs/jss

Matteo Ronchi [10:01 AM]  
si l’ho vista non mi ha convinto però
m isembra che non sia usata in progetti reali importanti o sbalgio?

Michele Bertoli [10:05 AM]  
quello non lo so (percio' penso di no)
pero' e' la piu' matura (edited)
es. quando ho iniziato con il mio repo c'era gia'
c'e' questa che funziona in modo super strano
https://github.com/threepointone/glamor
e siccome l'ha twittata mj sta ricevendo un sacco di attenzione
ma se vuoi andare sul sicuro aphrodite e' quello che fa per te

Matteo Ronchi [10:07 AM]  
si ho visto quella di threepointone
fa cose molto cool
però è troppo nuova
si infatti aphrodite mi sembra il miglior compromesso
anche se un sistema di plugins sarebbe figo

Michele Bertoli [10:08 AM]  
confermo

Matteo Ronchi [10:43 AM]  
ragazzi idee su come supportareflexbox per ie11 con aphrodite?

Michele Bertoli [10:49 AM]  
https://github.com/rofrischmann/inline-style-prefixer
sembra che aphrodite usi questa
dovresti essere a posto

Matteo Ronchi [10:59 AM]  
si infatti stavo guardando proprio ora!
grazie mille! sei il mio salvavita

https://github.com/Khan/aphrodite/issues/100
questo è inerente, se ti può interessare

