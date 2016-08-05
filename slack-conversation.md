
Matteo Ronchi [10:16 AM]  
@gianmarcotoso @michele @lucacolonnellocrweb e chiunque altro abbia input utili, ho una domanda per voi:

[10:19]  
Supponete di avere un manager/oggetto che ha dei valori che sono esposti via `context` ai children. Ma vorreste che alcuni rami usassero delle diverse configurazioni di valori. Ovviamente se ogni componente che usa questi valori ha nozione della chiave che deve chiedere all’oggetto nel context funziona tutto ma supponiamo che io volessi definire solo nel nodo ​*X*​ un nuovo set di valori e imporre a tutti i children/descendant questo set di valori ma senza passare una chiave esplicita ad ogni componente descendant…
L’uso del `context` è opzionale se ci sono altri approcci che funzionano

Luca Colonnello [10:19 AM]  
Spara

Matteo Ronchi [10:19 AM]  
per riassumere la radice ha 4 figli e 3 ricevono il colore rosso, ma uno e tutti is uoi children voglio ricevano blue

[10:20]  
definendolo solo in quel nodo padre di quel ramo

Michele Bertoli [10:20 AM]  
bhe' il context funziona gia' cosi'

[10:20]  
nel senso di sovrascrivere i valori per i figli (edited)

Matteo Ronchi [10:21 AM]  
si ma è globale no?

[10:21]  
cioè se sovrascrivo anche gli altri rami ricevono la modifica

[10:21]  
o clona ad ogni step (ammetto che non ho verificato)

[10:21]  
ma mi sembra molto costoso in termini di risorse clonare il context...

[10:21]  
anche lo facesse solo `shallow`

Luca Colonnello [10:23 AM]  
ho capito

[10:23]  
al di là del modo, il context ha un problema a riguardo

[10:23]  
il figlio deve definire cosa vuole che i suoi figli vedano

[10:23]  
quindi fondamentalmente per come funziona ora con l’attuale context di react penso ci sia troppo effort da distribuire

[10:23]  
se invece creassi un container?

Matteo Ronchi [10:24 AM]  
aspe

[10:24]  
il context fa quello che mi serve

Luca Colonnello [10:24 AM]  
certo

Matteo Ronchi [10:24 AM]  
la radice definisce nel context `color`

[10:24]  
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

[10:25]  
​_click_​

Michele Bertoli [10:25 AM]  
e' questo?

Matteo Ronchi [10:25 AM]  
leggo un sec

Luca Colonnello [10:26 AM]  
ma tu vuoi che color sia diverso solo da un certo nodo in poi e non vuoi specificarlo in quel nodo ma nel padre di tutti giusto?

Michele Bertoli [10:26 AM]  
> e non vuoi specificarlo in quel nodo ma nel padre di tutti

[10:26]  
ah ok non avevo capito

Matteo Ronchi [10:26 AM]  
@michele si l’idea è quella

Michele Bertoli [10:26 AM]  
ah

[10:26]  
bene

Matteo Ronchi [10:27 AM]  
immaginatevi lo scenario

[10:27]  
parliamo di un theme-manager per esempio

[10:27]  
anche se si può applicare a tanti casi (edited)

[10:27]  
l’app usa il tema `white`

[10:27]  
ma voglio che una modale e tutto il suo contenuto usi il tema `red`

[10:27]  
però non tutti i nodi della modale sono consapevoli del `theme-manager`

[10:28]  
quindi non posso semplicemente passare dalla modale ai figli che il tema è `red` e cmq passare la prop esplicitamente non mi piace (edited)

[10:28]  
l’esempio di Michele è nella direzione che desidero

[10:29]  
ora devo vedere se funziona anche con oggetti e non solo plain props

[10:29]  
però questo espone il fatto che il context viene ricalcolato potenzialmente ad ogni render

[10:29]  
e non mi piace molto

[10:29]  
@michele: fighissimo react.run :slightly_smiling_face:

Luca Colonnello [10:29 AM]  
ok allora io ti prppongo questo

Michele Bertoli [10:29 AM]  
http://www.react.run/rJ4F2T-t/3 (edited)

Matteo Ronchi [10:30 AM]  
vai

Michele Bertoli [10:30 AM]  
funziona anche con oggetti

Luca Colonnello [10:30 AM]  
crei un themeManager locale

Matteo Ronchi [10:30 AM]  
@michele quindi ogni ramo può sovrascrivere valori senza impatatre gli altri rami (edited)

Luca Colonnello [10:30 AM]  
o meglio un themeManager globale che in pattern factory possa essere ricreato a partire dall’oggetto base con config differenti

Matteo Ronchi [10:30 AM]  
questo non lo sapevo

Michele Bertoli [10:30 AM]  
@cef62: yep

Luca Colonnello [10:30 AM]  
beh questo si

Matteo Ronchi [10:31 AM]  
@lucacolonnellocrweb: una cosa, io non posso sapere sempre che c’è un override eslicito

[10:31]  
mi spiego

Michele Bertoli [10:31 AM]  
l'ho scoperto qui: https://twitter.com/ryanflorence/status/717597674040467456
 Ryan Florence @ryanflorence
@MicheleBertoli cool, but I think you are now limited to a single Provider in the app, no? The second you provide twice it shadows __data.
TwitterApril 6th at 8:19 AM 

Luca Colonnello [10:31 AM]  
:slightly_smiling_face:

Matteo Ronchi [10:31 AM]  
la dialog che usa `red` contiene altri container e alcuni dei loro children accedono al theme mangare, per fare override dovrebbero essere consapevoli del cambio di tema

Luca Colonnello [10:31 AM]  
mitico Ryan

Matteo Ronchi [10:31 AM]  
quindi deov cmq usare il context

Luca Colonnello [10:32 AM]  
no beh certo

[10:32]  
ma se il themeManager è un istanza inserita nel context

[10:32]  
che tutti usano e da cui recuperano variabili tipo il color

[10:32]  
basta solo reistanziare il theme manager con config differenti nel nodo dal quale vuoi si applichi il tema red

Matteo Ronchi [10:33 AM]  
figo @michele!

Luca Colonnello [10:33 AM]  
e tutti i figli lo useranno indipendentemente da quello che il padre ha impostato

[10:33]  
sbaglio?

Matteo Ronchi [10:33 AM]  
mmmm io mi immagino (visto che michele ha sdoganato l’override x nodi)

[10:33]  
che la radice inietta un unico them manager nell'app

[10:33]  
e ha un default theme

Luca Colonnello [10:33 AM]  
si

[10:34]  
esatto

Matteo Ronchi [10:34 AM]  
poi il nodo dialog modifica il valore del theme da `white` a `red`

Luca Colonnello [10:34 AM]  
esatto

[10:34]  
quello intendevo

Matteo Ronchi [10:34 AM]  
e tutti i suoi figli, che accedono all’unico theme manager

Luca Colonnello [10:34 AM]  
forse mi sono spiegato male

Matteo Ronchi [10:34 AM]  
useranno red

[10:34]  
:+1:

Luca Colonnello [10:34 AM]  
il problema è che devi creare un nuovo theme manager

[10:34]  
perchè se no le modifiche hanno effetti anche sui padri

[10:34]  
quindi il figlio dovrebbe fare tipo

Matteo Ronchi [10:34 AM]  
no non è vero, basta che il them manager supporti + temi

[10:35]  
e tu gli chiedi il tema tramite la chiave

roberto diana [10:35 AM]  
... of topic ... ma il context non verrà abbandonato in futuro ? ..

Matteo Ronchi [10:35 AM]  
abbandonato difficilmente

[10:35]  
potrebbero cambiare le API

Luca Colonnello [10:35 AM]  
added a JavaScript/JSON snippet 
​
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
Add Comment Collapse

Matteo Ronchi [10:35 AM]  
se tolgono la DI in react perdono accesso a tutte le cose fighe che si stanno producendo

[10:36]  
ah ok luca

[10:36]  
in realtà preferisco una singola istanza che tiene una mappa di temi

[10:36]  
e cambiare solo la chiave attiva del tema

[10:36]  
però il risultato è simile

[10:36]  
:wink:

[10:36]  
Figata cmq grazie a entrambi!

Luca Colonnello [10:36 AM]  
però così facendo vuol dire che i figli devono sapere che tema richiedere al themeManager

Matteo Ronchi [10:36 AM]  
questo mi apre delle possibilità molto fighe sul progetto che sto sviluppando (edited)

Luca Colonnello [10:36 AM]  
come dicevo io è implicito per i figli (edited)

Matteo Ronchi [10:37 AM]  
vero luca

[10:37]  
ma se i figli devono solo fare:

[10:37]  
`const themmanager = this.context.thememanager(this.context.theme)`

[10:38]  
è un male minore

[10:38]  
però si la tua soluzione risulta più pulita

[10:38]  
in realtà mi preoccupa poco la differenza perchè sarà tutto astratto in un HoC

[10:38]  
quello che vorrei evitare è di avere più di un istanza del theme manager

[10:39]  
ci lavoro su cmq

[10:39]  
poi vi faccio sapere!

Luca Colonnello [10:39 AM]  
un altra soluzione potrebbe essere utilizzare una prop

Matteo Ronchi [10:40 AM]  
no non è sostenibile

[10:40]  
non tutti i componenti sono a conoscenza del theme-manager

Luca Colonnello [10:40 AM]  
ma la prop serve solo a definire quale tema vuoi da li in poi, però per propagare la cosa ai figli deve essere implicita secondo me...

Matteo Ronchi [10:40 AM]  
intendo che se la dialog riceve come prop il tema `red`

Luca Colonnello [10:40 AM]  
si usando il context potrebbe andare

[10:40]  
ma ovviamente hai il problema al contrario

[10:41]  
la variabile theme nel context diventa una dipendenza esplicita

[10:41]  
può fungere lo stesso però

Matteo Ronchi [10:41 AM]  
lo dovrebbe passare esplicitamente a tutti i children, ma cosa succede se un children non sa cosa farsene della prop `them` e non la passa au suo descendant? (edited)

[10:41]  
beh si un tradeof c’è sempre

Luca Colonnello [10:41 AM]  
no infatti intendevo di passarlo solo al primo padre che deve sovrascrivere

[10:42]  
però facendo così stai generando un istanza del theme manager ad ogni componente

[10:42]  
che lo usa

Matteo Ronchi [10:42 AM]  
esatto

[10:42]  
vorrei evitarlo

Luca Colonnello [10:42 AM]  
invece mettendolo nel context solo chi lo cambia lo sostituisce

[10:43]  
però una soluzione potrebbe essere non reinstanziare ma cambiare a runtime il tema dal padre al figlio

[10:43]  
senza riprodurre oggetti

[10:43]  
un po come immutable (edited)

[10:43]  
immagina che il themeManager non è un instanza pura

[10:44]  
ma una rappresentazione dei metodi pubblici

[10:44]  
quindi un oggetto plain

Matteo Ronchi [10:44 AM]  
yep

Luca Colonnello [10:44 AM]  
che all’interno si riferisce alla vera istanza di themeManager

[10:44]  
ma usa le config dell’oggetto per lavorare

[10:44]  
tipo

[10:44]  
red:mainColor

[10:44]  
è il modo in cui recuperi il colore

[10:44]  
ma red non lo usano i componenti (edited)

[10:44]  
usano solo mainColor

[10:44]  
chiamando funzioni di un oggetto plain

[10:45]  
che conosce sia red che il themeManager

[10:45]  
e in reflection chiama

[10:45]  
themeManager.get(`${this.theme}:${propName}`)

[10:45]  
quindi themeManager pubblico è una factory di oggetti di questo tipo

[10:45]  
e così il padre può semplicemente fare come ti dicevo

[10:45]  
ma non ricrea tutto

[10:46]  
ma solo il POJO pubblico

[10:46]  
che ha 3 cose

[10:46]  
o forse bastano 2

[10:46]  
il get e il tema attuale

[10:46]  
:slightly_smiling_face:

Matteo Ronchi [10:46 AM]  
si può funzionare, molto dipende da come saràà internamente il themmanager (edited)

[10:46]  
ma l’idea è questa

[10:47]  
la API sono 3 imho: get, set, theme

[10:47]  
in realtà 4

[10:47]  
ci vuole anche register

[10:47]  
per aggiungere nuovi temi

Luca Colonnello [10:48 AM]  
esatto

[10:48]  
così da quello ne crei altri nei figli che vogliono sovrascrivere

Matteo Ronchi [10:48 AM]  
sarebbe figo farlo senza neanche il context

Luca Colonnello [10:49 AM]  
e questo potrebbe non essere un themeManager ma un wrapper per la DI con il context

Matteo Ronchi [10:49 AM]  
ma vorrebbe dire creare un meccanismo di DI ad-hoc

Luca Colonnello [10:49 AM]  
una roba tipo Pimple in PHP ma per React

[10:49]  
no no sempre con il context

[10:49]  
serve solo a definire questo meccanismo dinamico ma pulito

Matteo Ronchi [10:49 AM]  
> e questo potrebbe non essere un themeManager ma un wrapper per la DI con il context
spiega melgio (edited)

Luca Colonnello [10:50 AM]  
se io genero un wrapper che ingloba un oggetto, e gli passo una factory con impostazioni (utile all’inizio e a fare anche il register)

[10:50]  
può wrappare qualsiasi oggetto

[10:50]  
le api le definisco io

[10:50]  
lui definisce il meccanismo

[10:51]  
se io creo get e gli faccio fare quello che dicevamo

[10:51]  
è tutto flessibile e generico

[10:51]  
e va bene per qualunque cosa

[10:51]  
lui definisce solo l’interfaccia pubblica

[10:51]  
che però è risultato di una factory che crei tu

[10:51]  
quindi può fare qualunque cose secondo questo meccanismo

[10:51]  
anche un sistema di settaggi differente da un tema

[10:51]  
o di oggetti

[10:51]  
veri e propri derivati da classi

Matteo Ronchi [10:52 AM]  
gotcha, si infatti il theme manager era solol’esempio più facile su cui discutere (edited)

Luca Colonnello [10:53 AM]  
:slightly_smiling_face:

Matteo Ronchi [10:54 AM]  
poi non so quanto abbia senso fare un sistema generico (nel mio caso attuale) ma vedremo… sett prox ci lavoro attivamente :slightly_smiling_face:

[10:55]  
grazie a entrambi ragazzi!

Luca Colonnello [10:57 AM]  
:slightly_smiling_face:

roberto diana [10:57 AM]  
raga ma che ce relazione fra context e DI ? o forse no c'è relazione ... ? (edited)

Matteo Ronchi [10:58 AM]  
il `context` è l’unico meccanismo nativo che React offre per avere qualcosa di simile alla DI

[10:59]  
concettualmente parlando non è la stessa cosa ma offre la possibilità di accedere a dati non forniti direttamente dal parent container

roberto diana [10:59 AM]  
ah!

[10:59]  
ok

[10:59]  
non riuscivo a collegare con la  DI (edited)

Matteo Ronchi [10:59 AM]  
in `angular` tu inietti un servizio

roberto diana [10:59 AM]  
in angular è chiarissimo in react meno

[10:59]  
(per me ... obv)

Matteo Ronchi [11:00 AM]  
in `react` passi props, ma se devi passare un servzio ail pronipote dovresti passare il servizio anche al figlio e al nipote

[11:00]  
il context ti permette di passare al pronipote senza passare esplicitmaente tra i nodi intermedi

roberto diana [11:01 AM]  
che comunque non è proprio DI ma ho capito meglio adesso :smile:

Matteo Ronchi [11:01 AM]  
esatto :wink:

[11:02]  
React, a quanto mi risulta, non usa Reflection o altre tecniche per iniettare il context, semplicemente lo gestisce per te lungo l’albero di componenti

roberto diana [11:02 AM]  
lo clona ? o usa reference o qualche meccanismo simile ?

Matteo Ronchi [11:04 AM]  
se leggi il thread sopra ci sono gli esempi di michele che mostrano che ogni volta che un nodo definisce qualcosa nel context viene prodotto un nuovo context che mantiene relazione con ciò che è definito dai livelli superiori ma che permette un `safe` override da quel punto in poi per un dato ramo

[11:04]  
questo http://www.react.run/rJ4F2T-t/3

roberto diana [11:05 AM]  
missato ora leggo grazie

Matteo Ronchi [11:05 AM]  
:wink:

Luca Colonnello [4:33 PM]  
Ragazzi volevo porvi una soluzione che ho trovato ad un problema

[4:34]  
voglio creare container redux che permettano di sovrascrivere il dumb che usano (non usando props dall'esterno)

[4:34]  
con un default dumb nel caso in cui non si voglia sovrascrivere

[4:34]  
ho pensato a questo

Luca Colonnello [4:34 PM]  
added a JavaScript/JSON snippet: withRender 
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
Add Comment Click to expand inline 45 lines

Luca Colonnello [4:35 PM]  
una utility withRender, curryed, che ricevuta una classe, la estende aggiungendo il render

[4:35]  
ditemi che ne pensate e se è chiaro

[4:35]  
:slightly_smiling_face:

Matteo Ronchi [4:39 PM]  
ma quale sarebbe il caso d’uso?

[4:41]  
cioè è chiaro che ti permette di definire un wrapper redux e di riutilizzarlo più volte

[4:41]  
ma in che scenario lo vedi utile/necessario?

Luca Colonnello [4:49 PM]  
creo un toolkit di componenti che usano redux per separare logica delle interazioni rispetto a grafica

[4:49]  
voglio permettere all’esterno a chi lo usa di ridefinire solo la view

[4:49]  
quindi il container non voglio debba essere ricreato ogni volta

[4:49]  
ma creo il mio componente tab

[4:51]  
e chi lo usa usa la grafica che offro o se la riscrive

[4:51]  
pensi possa essere una cazzata?

[4:51]  
chiedo eh… condivido per quello :slightly_smiling_face:

Matteo Ronchi [4:53 PM]  
non basta creare una factory che riceve il componente e lo restituisce wrappato dal container? Cioè stilisticamente è figo ma non vedo perchè dovrei usare `withRender` quando posso semplicemente esportare dei moduli che fanno la stessa cosa.. l’unico vantaggio è il default, ma perchè dovrei usare un default?

[4:54]  
non so se mi sono spiegato

[4:58]  
non critico l’approccio semplicemente cerco di capire che reale vantaggio potrei avere rispetto a creare moduli `es` che esportano una funzione che accetta un componente e lo wrappa con connect

[4:59]  
con il tuo componente devo introdurre una dipendenza esterna che mi risparmia poche righe

Luca Colonnello [5:00 PM]  
si ma manca il default

[5:00]  
questo serve a standardizzare l’approccio in FP

Matteo Ronchi [5:01 PM]  
si ma no vedo un caso reale in cui il default possa servire

Luca Colonnello [5:01 PM]  
se tu fai un toolkit

[5:01]  
o cmq componenti riutilizzabili

[5:01]  
il container ha già la grafica

Matteo Ronchi [5:01 PM]  
si ma chi mai userebbe il default?

Luca Colonnello [5:01 PM]  
???

[5:01]  
se prendo il tab di material ui è fatto così

Matteo Ronchi [5:01 PM]  
cioè se lo uso vuol dire che il default è il mio componente reale

[5:01]  
si ma loro ti danno un component con UI e il default tab serve

Luca Colonnello [5:02 PM]  
esatto

Matteo Ronchi [5:02 PM]  
il default con un connect non serve

[5:02]  
perchè non lo useresti mai

Luca Colonnello [5:02 PM]  
se tu esporti un componente che ha una parte redux e una parte react

[5:02]  
la parte redux è connect + reducer + actions + selectors (edited)

[5:02]  
la parte react è il dumb + il container del connect (edited)

Matteo Ronchi [5:02 PM]  
vero ma continuo a non aver bisogno del default

[5:03]  
crei una factory che accetta il componente e lo wrappa con tutta la parte redux

[5:03]  
hai anche test + facili

Luca Colonnello [5:03 PM]  
si ma il tuo toolkit non offre quindi un default render??

[5:03]  
me lo devo fare ogni volta  a mano (edited)

[5:04]  
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

[5:04]  
tu non hai la mia UI, non sai cosa ci metto dentro e non usi di sicuro le mie CSS

[5:05]  
quindi prendo la tua utility e gli passo un mio componente

[5:05]  
al che mi chiedo

[5:05]  
perchè uso questa lib, se gli devo fornire tutti i building blocks?

[5:05]  
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

[5:07]  
questa è un esigenza specifica

[5:07]  
cmq non è solo redux il punto

[5:07]  
quello era per dimostrare che con la composition si può fare anche con il connect

[5:07]  
ma se io voglio creare come con recompose

[5:08]  
dei comp stateless ma che hanno con degli HoC dei comportamenti legati al componente stesso

[5:08]  
gestione dello stato magari

Matteo Ronchi [5:08 PM]  
se mi dici che tu offri una lib, la cui logica è redux-based e che vuoi permettere ai tuoi user di cambiare la dumb part del componente, mantenendo ovviamente le props che tu gli passi, allora lo snippet ha senso

Luca Colonnello [5:08 PM]  
così posso sovrascrivere il render ma avere stato e altri HoC

[5:08]  
si si esatto

Matteo Ronchi [5:09 PM]  
anche se non so se userei una lib che mi impone i suoi reducer per funzionare, anche se ha un suo perchè)

[5:09]  
però ci dovrei riflettere, a pelle non me gusta

[5:09]  
gli UI comp dovrebbero essere completamente isolati

Luca Colonnello [5:09 PM]  
si ma come ti scrivevo

Matteo Ronchi [5:09 PM]  
personalmente non vedo male lo stato di un componente react se è specifico ad azioni locali

Luca Colonnello [5:09 PM]  
possono anche non essere vincolati a redux

[5:09]  
si si

[5:09]  
ma lo fai uguale

[5:10]  
immagina recompose

[5:10]  
ho il dumb e con gli HoC creo lo stato e i reducer dello stato

Matteo Ronchi [5:10 PM]  
però aspetta se togli il layer redux e lasci cambiare il componente praticamente chiede all’utente di rifare ilt uo lavoro

[5:10]  
solo x componenti davvero banali funzionerebbe la sostituzione così com'è

Luca Colonnello [5:10 PM]  
se il componente è stateful no

Matteo Ronchi [5:10 PM]  
si ma a quel punto devi incastrarti con lemeccaniche di aggiornamento dello stato

[5:11]  
è figo ma complesso

Luca Colonnello [5:11 PM]  
se vai in composition è esattamente come il concetto di dumb e container

[5:11]  
come funge anche recompose

Matteo Ronchi [5:12 PM]  
si ma non vedo come per esempio puoi gestire il custom rendere di una  select, sarebbe mooolto complicato

Luca Colonnello [5:12 PM]  
scusa non ho capito

Matteo Ronchi [5:13 PM]  
supponi che uno dei tuoi componenti è una `select` (edited)

Luca Colonnello [5:13 PM]  
si

[5:13]  
una select è stateless

[5:13]  
non ha bisogno di tale logica

Matteo Ronchi [5:13 PM]  
se io voglio cambiare la UI devo scrivere uno sproposito di codice

[5:14]  
mmm statelss +o-

[5:14]  
se ha multiselezione, elementi interattivi etc...

Luca Colonnello [5:14 PM]  
nel caso di un ui toolkit dovrebbe essere stateless

Matteo Ronchi [5:14 PM]  
guarda react-select

Luca Colonnello [5:14 PM]  
certo in quel caso funzionerebbe

[5:14]  
devi scrivere molto codice

Matteo Ronchi [5:14 PM]  
si ma per fare un renderer ci impiego una vita

Luca Colonnello [5:14 PM]  
ma la tua esigenza è proprio quella di rifarlo da 0 se vuoi cambiare il render

[5:15]  
sarebbe peggio rifare tutto da 0, grafica + logica!!

Matteo Ronchi [5:15 PM]  
si ma vuol dire che la tua logica deve andarmi perfettamente bene

Luca Colonnello [5:15 PM]  
se così non fosse non usi quel componente

[5:15]  
ma se fai un tab ma il render deve essere modificato

[5:16]  
così lo puoi fare

Matteo Ronchi [5:16 PM]  
sisi è chiaro il vantaggio

[5:16]  
semplicemente, ma è molto personale come punto di vista, se uso la tua lib e la personalizza in maniera forte molto probabilmente la riscrivo come serve a me

[5:17]  
l’effort è maggiore ma non dipendo dalle tue scelte di design (del codice) e di utilizzo

Luca Colonnello [5:17 PM]  
si ma se non ti capita questo caso, non usi la lib

[5:17]  
:slightly_smiling_face:

[5:17]  
o meglio

[5:17]  
se l’implementazione è molto differente nella logica e nel render certamente non ha senso

[5:18]  
è un caso limite magari, ma facendolo in FP è compatibile con molte altre cose

[5:18]  
come appunto recompose o aprhodite

Matteo Ronchi [5:18 PM]  
quello assolutamente

[5:19]  
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
(edited)

[5:20]  
cosa intendi?

[5:21]  
volevo usare aphrodite per la stessa ragione, fare qualcosa del genere con withStyle tipo

Matteo Ronchi [5:22 PM]  
no spe forse mi confondo

[5:22]  
ne sto guardando troppe assieme

Luca Colonnello [5:22 PM]  
ahahahah

[5:22]  
è quella di Khan Dods (edited)

Matteo Ronchi [5:22 PM]  
è radium che wrappa la rende function

[5:22]  
sorry

Luca Colonnello [5:22 PM]  
si si

[5:22]  
esatto

[5:23]  
infatti non me gusta molto

Matteo Ronchi [5:23 PM]  
non so tra iniettare css e usare stili inline quale mi piace di più :stuck_out_tongue:

Luca Colonnello [5:23 PM]  
preferisco iniettare css onestamente

[5:23]  
lo stile inline ha troppe limitazioni

Matteo Ronchi [5:24 PM]  
beh in realtà sono tutte aggirabili (vedi radium)

Luca Colonnello [5:24 PM]  
eh ma un hover fatto in css non è un hover fatto in js

[5:24]  
perdi after e before e animation

[5:24]  
hai solo transition

[5:25]  
se non mi sbaglio eh

[5:25]  
...

Matteo Ronchi [5:26 PM]  
beh si molto dipende da cosa devi fare

[5:26]  
personalmente non sono molto preoccupato lato animation e simili

Luca Colonnello [5:26 PM]  
beh si certo dipende

Matteo Ronchi [5:26 PM]  
anche le mediaquery le gestiamo cmq via JS perchè carico proprio layout applicativi diversi

Luca Colonnello [5:26 PM]  
beh si

fabiobiondi [5:27 PM]  
ma voi non lavorate mai? :smile:

Matteo Ronchi [5:27 PM]  
certo è quello che facciamo reglarmente

fabiobiondi [5:27 PM]  
potreste fare delle chat audio su sta roba.. sarebbero interessantissime (edited)

[5:28]  
: P

Michele Bertoli [5:28 PM]  
no ma infatti ragazzi questa roba va salvata

[5:28]  
purtroppo non riesco a partecipare perche' YPlan e' nel caos (poi vi racconto)

[5:29]  
> ma voi non lavorate mai?

fabiobiondi [5:29 PM]  
urca.. buon lavoro raga

[5:29]  
cmq sono tutte info utili per il lavoro

[5:29]  
era una battuta  :smile:

Michele Bertoli [5:29 PM]  
esatto

[5:29]  
sisisi chiaro

fabiobiondi [5:29 PM]  
:wink:

Michele Bertoli [5:29 PM]  
grazie a questa community faccio meglio il mio lavoro

fabiobiondi [5:30 PM]  
sono d’accordo 100%.. ma volevo rompere le scatole a Luca e Mat : P

Michele Bertoli [5:30 PM]  
anzi nelle mie prossime presentazioni

[5:30]  
posso mettere

[5:30]  
member of FEVR, vero?

fabiobiondi [5:30 PM]  
in realtà dovresti citare angularjs developers italiani e react italia….

Michele Bertoli [5:30 PM]  
(anche se non sono mai venuto ad un evento live :( - a parte le conf)

fabiobiondi [5:30 PM]  
sono le nostre : P

[5:30]  
sono i canali in cui promuoviamo slack (edited)

[5:30]  
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

Luca Colonnello [5:46 PM]  
piccola osservazione: se usi recompose questo withRender non serve, in quanto basta comporre tutto con recompose e passare tutto a connect

[5:46]  
recompose offre l’enhancer che fa una roba simile

[5:47]  
connect sarebbe una funzione composta come le altre

[5:47]  
e il default component sarebbe un componente che è già stato decorato con l'enhancer

[5:47]  
e non crei lib ulteriori e rimani standard

[5:47]  
se hai classe invece ti serve una roba come questa (edited)

[5:47]  
scelte implementativa

[5:48]  
:slightly_smiling_face:

Matteo Ronchi [5:49 PM]  
scusate ero in un meeting flash!

[5:49]  
ora vi saluto miei cari :wink: buona continuazione

fabiobiondi [5:52 PM]  
ciao mat!!

Luca Colonnello [5:53 PM]  
ciao Matteee

[5:53]  
@fabiobiondi secondo me la cosa di recompose potrebbe interessarti

[5:53]  
:slightly_smiling_face:

Matteo Ronchi [5:53 PM]  
:slightly_smiling_face:

new messages
fabiobiondi [6:23 PM]  
@lucacolonnellocrweb: scusa temporale.. saltato tutto. Non ho seguito la discussione a dire il vero.. non tutta

[6:23]  
se mi mandi un link me lo guardo...

[6:23]  
di che lib parlavi?

Luca Colonnello [6:25 PM]  
Recompose js

fabiobiondi [6:31 PM]  
grazie doc
