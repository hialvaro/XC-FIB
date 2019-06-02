# XC - Primer Parcial

## <u>TEMA 1- INTRODUCCIÓ</u>

## Model OSI de ISO

Divideix el conjunt de protocols que formen part d'una xarxa de computadors en 7 nivells; cada un independent dels altrs i amb funcions específiques.

Els **nodes terminals** representen els dos sistemes que es comuniquen a través de la xarxa (p.e: client i servidor). Els **nodes intermedis** tenen la funció d'encaminar l'informació (p.e: els routers). En aquest model, cada nivell és independent dels altres, ofereix serveis al nivell superior i fa servir el nivell inferior (excepte el nivell físic) per a implementar els seus serveis. Cada nivell utilitza na estructura de dades anomenada *Protocol Data Unit* **(PDU)** per a intercanviar informació amb el seu nivell parell.

Cada nivell estableix un diàleg al corresponent amb les línies discontínues.

![](/img/Osi.PNG)

- Nivell 1 - **Físic**: Defineix les característiques físiques i elèctriques d'un dispositiu de xarxa. Per exemple, l'estàndard RS-232 és un estàndard de nivell físic. Aquest estàndard especifica el protocol que fa servir el port sèrie d'un PC (voltatges, velocitats de transmissió, com s'envien els bits per la línia de transmissió, tipus de connector i pins, etc.) En aquest nivell només es parla de bits o caràcters.
- Nivell 2 - **Enllaç** *(Data link)*: Fa d'interfície entre el nivell de xarxa i el físic. Les PDUs que utilitza s'anomenen "trames" *(frames)*. Entre les seves funcions hi pot haver:
  - *Framing*: Per a descobrir on comença i on acaba una trama dintre del flux de bits o caràcters que llegeix del nivell físic.
  - Detecció d'errors: Per a detectar si la trama rebuda té algun error.
  - Recuperació d'errors: per exemple, demanant la retransmissió d'una trama errònia al seu nivell parell.
- Nivell 3 - **Xarxa** *(Network)*: Les PDUs que fa servir s'anomenen "paquets" (datagrames per Internet). La seva principal funció és l'encaminament de PDUs: És a dir, aconseguir que les PDUs segueixin el camí correcte entre la font i la destinació.
- Nivell 4 - **Transport**: Les PDUs utilitzades es diuen "segments". És un nivell extrem-a-extrem. La seva funció és establir un canal entre les dues aplicacions que es comuniquen. Pot oferir serveis de segmentació o recuperació d'errors.
- Nivell 5 - **Sessió**: La seva funció és proporcionar la gestió d'una sessió entre les dues aplicacions. Per exemple, *login*, permetre tornar a un punt conegut després d'un tall de connexió, etc.
- Nivell 6 - **Presentació**: La seva funció és proporcionar protocols de presentació de dades (ASCI, MPEG, etc.)
- Nivell 7 - **Aplicació**: Són els protocols de les aplicacions que fan servir la xarxa. Per exemple: http, smtp, ftp, telnet, etc.

### Arquitectura TCP/IP

L'arquitectura, o nivells, en què estan organitzats els protocols d'internet sol estar presentat com a la següent figura:

![](/img/arquitecturatcpip.png)

És a dir, el nivell d'aplicació engloba els nivells 5, 6 i 7 del model OSI. Els **protocols més importants** d'Internet es corresponen amb el nivell de **xarxa: el *Internet Protocol* (IP)** i el nivell de **transport: El *Transmission Control Protocol* (TCP)**. Per ser més exactes, pel que fa al **transort hi ha dos protocols**: **TCP,** que implementa una transmissió fiable ( assegura una transmissio lliure d'errors), i el ***User Datagram Protocol* (UDP)**, que no assegura el lliurament correcte de l'informació. Per sota del nivell IP, hi ha una "interfície de xarxa" que depèn de la xarxa física on es connecta cada dispositiu. En l'argot d'Internet, els **nodes terminals s'anomenen *hosts* i els nodes intermedis *routers*.**

### Encapslament de l'informació

En la transmissió d'informació a través de la xarxa, **cada nivell afegeix una capçalera amb l'infromació necessària per a comunicar-se amb el nivell parell**. Cada nivell afegeix una capçalera abans de passar la PDU al nivell inferior i **elimina la capçalera aband de passar la PDU al nivell superior**. Aquest procés s'anomena **encapsulament**:

![](/img/paradigmaclientservidor.png)

## Paradigma Client-Servidor

És el **model utilitzat per a les aplicacións a Internet per intercanviar-se informació**.

L'aplicació que **inicia l'intercanvi** d'informació s'anomena "**client**". Així doncs, el client envia el primer datagrama cap al servidor. Tal com hem vist, **el nivell de xarxa (IP) porta els datagrames fins a la destinació**. **Un cop a la destinació, cal lliurar l'informació a l'aplicació on van dirigits** (p.e: un servidor web). El responsable d'aquesta tasca és el nivell de transport (TCP o UDP). És lògic que sigui així, perquè el nivell de transport és el primer nivell que hi ha extrem-a-extrem.

Per poder **identificar les aplicacions** que es comuniquen, TCP/UDP fan servir els anomenats "**ports**":
Un port identifica l'aplicació client, i l'altra aplicació servidor. Aquests ports **formen part de la capçalera de protocol TCP o UDP**.

Considerem una màquina UNIX; les aplicacións són els processos que s'executen en la màquina. El servidor és un *daemon* que s'executa contínuament. Aquest **servidor té associat un port *well-known***. Cada port *well-known* **identifica un servei estàndard d'internet**, com ara ftp (port 21), telnet (port 23), web (port 80), etc. Els ports *well-known* tenen un valor en l'interval [0, ..., 1023] i estan estandarditzats per un organisme d'Internet anomenat IANA.
Quan el **client** inicia l'intercanvi d'informació, el sistema operatiu li assigna un **port "efímer"** (que té un valor >= 1024). Aquest port identifica el client mentre dura l'intercanvi d'informació. D'aquesta manera, el servidor pot contestar al client quan rep la seva petició. Així doncs, per identificar completament una connexió en TCP/IP es fa servir la tupla: (port font, adreça IP font; port destinació, adreça IP destinació). Les **adreces IP identifiquen els hosts, els ports identifiquen les aplicacions dels hosts**.



## <u>TEMA 2 - Xarxes IP</u>

A continuació estudiarem l'*Internet Protocol* (IP) i alguns altres protocols relacionats amb la configuració i funcionament del nivell de xarxa d'una xarxa TCP/IP.

El protocol IP va ser dissenyat per tal de poder interconnectar xarxes heterogènies. Això s'aconsegueix amb routers que "parlen" el mateix protocol: IP. La funció dels routers és encaminar les unitats de dades (PDUs en OSI) que fa servir el protocol IP: els "datagrames". La descripció del funcionament bàsic dels routers ens permet descriure les característiques més importants del protocol IP. La seguent figura mostra l'arquitectura bàsica d'un router:

Un router té dues o més "interfícies" connectades a xarxes diferents. Físicament, les interfícies estàn formades per targes de comunicacions (*Network Interface Card*, NIC) que permeten transmetre o rebre informació a través d'una xarxa física específica: una LAN o WAN.

![](/img/estructrouter.PNG)



Quan un router rep un datagrama d'una interfície, segueix el següent procés:

1. El datagrama passa a la **funció *ip_input***, que **mira si va dirigit al mateix router**. En cas afirmatiu, el passa als nivells superiors, **altrament el passa a la funció *ip_output***. El pas de *ip_input* a *ip_output* es coneix com a ***IP forwarding*** i és la diferència entre un host i un router. En el as d'un *host*, l'*IP forwarding* està desactivat, i si el datagrama no va dirigit al mateix *host* es descarta.
2. La funció ***ip_output* s'encarrega de l'encaminament**. Per a l'encaminament el router fa servir una **"taula d'encaminament"**. En aquesta taula **hi ha les xarxes destinació on el router sap arribar.** Per a cada xarxa destinació la taula diu la interfície **per on ha denviar-se el datagrama**.
3. Després de consultar la taula, ***ip_output* assa el datagrama al *driver* que controla la NIC** per on ha d'enviar-se i aquest el guarda en un **"*buffer* de sortida"** a l'espera que la NIC l'agafi i l'enviï a la **xarxa física** on està connectada.

El procediment de **rebre els datagrames, processar-los i emmagatzemar-los fins que es transmeten **es coneix com a ***store and forward***. Cal destacar que les **taules d'encaminament del router han d'estar inicialitzades**. Si el router rep un datagrama dirigit a una **direcció desconeguda, el descarta**. A més, **si arriben més datagrames dirigits a una mateixa NIC del que és capaç de transmetre, el buffer de sortida s'omplirà i el router començarà a descartar datagrames**.

Característiques del protocol IP deduïbles:

- No està orientat a la connexió *connectionless*: Abans de començar a encaminar datagrames, un router no ha de registrar l'establiment de connexió. Això no és així en altres xarxes com ara la xarxa telefòncia, on abans de començar a parlar hem d'establir una connexió i esperar que l'altre extrem respongui (*connection oriented*).
- Sense estat (*stateless*): És a dir, no guarda informació de les connexions en curs.
- No fiable: Per exemple, els routers descarten datagrames si el buffer per on han d'encaminar-lo està ple. Aquest tipus de servei on els routers "fan el millor que poden" per encaminar el datagrama, i si no poden el descartes s'anomenen *best effort*.

### Capçalera IP-RFC 791 [19]

![](/img/capçaleradatagramaip.PNG)

- *Version*: Versió del protocol: 4.
- IHL: ***IP Header Length***, mida de la capçalera **en words de 32 bits** (Si no té opcions val 5, és a dir, té 5*4 = 20bytes).
- ***Type of service***: Preferència d'encaminament. El format és xxxdtrc0, on xxx permeten indicar una precedència. Només té significat a l'interiror d'una mateixa xarxa i no arreu d'internet (no sol utilitzar-se). L'últim bit es posa a 0 (no es fa servir). Els bits *dtrc* signifiquen:
  - d: *delay*, optimitzar el retard
  - t: *throughput*, optimitzar la velocitat eficaç
  - r: *reliabiliti*, optimitzar la fiabilitat
  - c: *cost*, optimitzar el cost (econòmic)
- ***Total Length***: Mida total del datagrama en bytes
- *Identification, Flags, Fragment Offset*: Utilitzats en la fragmentació.
- ***Time to Live* (TTL)**: Temps de vida. Els **routers decrementen aquest camp i descarten el datagrama si arriba a 0**, és a dir fan: `if(--TTL == 0) descartar el datagrama`. El motiu és **evitar que hi hagi datagrames donant voltes indefinidament per Internet**. Això podria passar si hi hagués algun *bug* en una taula d'encaminament, o si s'enviés un datagrama a una adreça inexistent.
- ***Protocol***: Multiplexació del protocol de nivell superior. Els números de protocol estan estandarditzats en el RFC 1700. En una màquina UNIX es poden consultar en el fitxer `/etc/protocols`
- ***Header Checksum***: Permet la detecció d'errors. El checksum és només de la capçalera. L'algoristme de Checksum és el complement a 1 de la suma en complement a 1 de la informació a protegir. És a dir, pel càlcul del checksum es posen els bits del camp de checksum a la capçalera a 0, s'agrupen els bits de la capçalera en words de 16 bits, se sumen en complement a 1 i es fa el complement a 1 de la suma. La suma en complement a 1 consisteix en fer la suma en binari natural i tornar a sumar el possible excés que es pugui produïr. El complement a 1 consisteix en canviar els 1 per 0 i viceversa:

![](/img/checksum.PNG)

- ***Source Adress, Destination Adress***: adreça font i destinació del datagrama.
- ***Options***: Pot portar 1 o més opcións. **Normalment no en porta cap**. Algunes de les definides són:
  - *Record Route*: Amb aquesta opció, els routers afegeixen l'adreça IP de l'interfície per on encaminen el datagrama.
  - *Loose Source Routing*: Especifica una llista d'adreces IP de routers que ha de travessar el datagrama (pot travessar també altres routers que no siguin a la llista).
  - *Strict Source Routing*: Adreces IP dels únics routers que pot travessar el datagrama.

### Fragmentació

IP **pot fragmentar** un datagrama quan la *Maximum Transfer Unit* (MTU) del nivell d'enllaç on ha d'encaminar-se té una **mida menor que la del datagrama.** Això sol passar en els següents casos:

- Quan un router ha d'encaminar un datagrama per una xarxa amb MTU menor que la d'on ha arribat.
- Quan es fa servir UDP i l'aplicació fa una escriptura major que l'MTU de la xarxa.

Quan es produeix la fragmentació, el reensamblatge es fa en la destinació dels datagrames. Per poder fer el reensamblatge es fan servir els camps:

- ***Identification***: El nivell IP del *host* que genera els datagrames hi posa el valor d'un comptador que incrementa cada cop que es genera un nou datagrama. **Aquest número permet identificar fragments d'un mateix datagrama.**
- ***Flags***: Són **0DM**. El primer bit no s'utilitza, els altres són:
  - D: Flag de *Don't fragment*. Quan està a '1' el datagrama no es pot fragmentar. Si un router necessita fer-ho el descartarà i enviarà un missatge ICMP d'error. Es fa servir en el mecanisme MUT *path discovery*.
  - M: Flag de ***More fragments***. Si està a 1 vol dir que **hi ha més fragments**. Tots els datagrames el porten a '1' excepte l'últim.
- ***Offset***: **Posició del primer byte del fragment en el datagrama original** (el primer val '0'). Es compta en unitats de 8 bytes (per poder comptar fins a 12<sup>16-1</sup> amb 13 bits).

## MTU Path Discovery - RFC 1191 [31]

El nivell de transport **TCP agrupa els bytes que escriu l'aplicació fins a tenir un segment de mida òptima i després l'envia**. Normalment la mida òptima és la que permet enviar els segments de mida igual a l'MTU menor de les xarxes que han de travessar fins a la destinació. Això permet enviar segments de mida gran (i així l'*overhead* de les capçaleres serà petit), però no tant com perquè s'hagin de fragmentar. La fragmentació no és desitjable perquè:

1. Relantitza els routers.
2. Pot provocar que hi hai fragments de mida petita que redueixen l'eficiència de la xarxa.
3. Si es perd un sol fragent, s'han de descartar tots els altres quan arriben a la destinació.

Per aconseguir que la **mida dels segments sigui òptima**, les implementacións actuals de TCP fan el mecanisme **MTU Path Discovery, que consisteix en**:

Primer proven d'**adaptar la mida dels segments a l'MTU de la xarxa on està connectat el *host***. Els datagrames **s'envien amb el bit de *Don't fragment* activat**. Si un router intermedi ha d'enviar-los per una **xarxa d'MTU menor, descartaran el datagrama** i enviaran un missatge ICMP d'**error *fragmentation needed but DF set***. Una de les informacions que aporta aquest missatge es la mida de l'MTU que l'ha provocat. **Quan el *host* rep el missatge d'error, TCP redueix la mida dels segments per adaptar-los en aquesta MTU**.

### Exemple de fragmentació

Suposem que una aplicació d'un *host* genera un **datagrama UDP de 1980 bytes** que ha d'enviar-se per una xarxa Ethernet.  El nivell **IP construirà un datagrama de 2000 bytes: 20 de capçalera IP + 1980 de *payload***. Quan IP vegi que el datagrama ha d'encaminar-se per una interfície d'**MTU = 1500 bytes**, haurà de fragmentar-lo. El ***payload* màxim dels fragments** (en <u>unitats de 8 bytes</u>) serà:
$$
mida \ dels \ fragments\ =\Bigl\lfloor\dfrac{1500-20}{8}\Bigr\rfloor\qquad = 185 \ unitats \ de \ 8 \ bytes
$$
1500 és la mida del MTU, 20 serà la mida de la capçalera; per tant el *payload* del fragment serà de 1480 bytes. Ho dividim entre 8 per a obtenir el nombre de unitats de 8 bytes que tenim en el fragment; així obtenim quin és el payload efectiu del fragment en unitats de 8 bytes.

És a dir, 185*8 = 1480 bytes de *payload* en el fragment, per tant, es generaràn 2 fragments: El primer portarà els primers 1480 bytes del payload del datagrama original. El segon portarà els 500 bytes restants. Al fragmentar-se, es copiarà la capçalera del datagrama original en cada fragment i es canviaràn els camps:

- 1r fragment: offset = 0, M = 1
- 2n fragment: offset = 185, M = 0

A més dels camps anterirors, IP també haurà de canviar convenient-ment el camp *total length* i el *checksum*.

## Adreces IP

Tenen **32 bits** (4 bytes). La següent figura mostra el format d'una adreça IP.

![Adreça IP](/img/ipadr.PNG)

***Netid* identifica la xarxa i el *hostid* identifica un *host* dintre la xarxa**. El límit entre el netid i el hostid és variable. La notació que es fa servir es coneix com a *notació amb punts* i consisteix a expressar els **4 bytes de l'adreça en decimal separats per punts**, per exemple 147.83.34.25.

Hi ha diferents classes de ip; la classe D és per adreces *multicast* (per exemple la 224.0.0.1 identifica "*All systems on this Subnet*") i la classe E són adreces reservades. Tipus d'adreces:

![](/img/tiupsips.PNG)

L'assignació d'aquestes adreces es fa tenint en compte que:

- **Una adreça identifica una interfície.**
- Totes les adreces d'una **mateixa xarxa IP** tenen el **mateix netid.**
- Totes les **adreces** assignades han de ser **diferents entre elles**

No totes les adreces es poden fer servir per numerar les interfícies. A continuació veiem les adreces especials:

![](/img/ipsespecials.PNG)

A continuació veiem un exemple d'assignació d'adreces, destaquem que:

- Totes les interfícies d'una mateixa xarxa tenen el mateix prefix (netid).
- No es poden fer servir adreces especials per a numerar interfícies. Cada xarxa en té dues: la de la xarxa (amb el hostid tot a '0', que és la primera adreça disponible en el rang d'adreces de la xarxa) i la del broadcast en la xarxa (amb el hostid tot a '1', que és la ultima disponible en el rang d'adreces de la xarxa).
- El router ha de tenir assignada una adreça en cada interfície. Cada adreça ha de tenir el netid de la xarxa on està connectada la interfície.

![](/img/assignadr.PNG)

Les **adreces han de ser úniques en tot internet**. Per això, l'organisme IANA assigna blocs d'adreces als *Regional Internet Registers* (RIR): RIEPE (EUR), ARIN (USA), APNIC (ASIA), LACNIC (LATAM). A la vegada els RIR assignen blocs d'adreces als ISP i aquests als seus abonats. Aquestes adreces s'anomenen públiques, globals o registrades.

### Adreces Privades - RFC 1918

Per a les xarxes (o *hosts*) que fan servir TCP/IP i que no tenen una adreça pública, s'han reservat els blocs d'adreces següents que no són enrutables a internet:

- 1 adreça de la classe A :10.0.0.0 -> 10.255.255.255
- 16 adreces de la classe B: 172.16.0.0 -> 172.31.255.255
- 256 adreces de la classe C: 192.168.0.0 -> 192.168.255.255

## Subnetting

La divisió en classes és massa rígida i no permet aprofitar bé les adreces. Solució: Deixar que el límit entre el netid i hostid sigui variable. Això permet tenir més xarxes de mida diferent. El nombre d'adreces d'una xarxa, però haurà de continuar sent múltiple de 2.

Per a expressar quina és la mida del netid es fa servir una màscara. La màscara és un número de 32 bits amb els bits més significatius que es corresponen amb el netid a '1' i els altres a '0'.

La màscara es representa amb una de les següents notacions: *adreça-IP/bits-a-1-de-la-mascara* (p.e: 147.83.24.0/24), o amb la notació amb punts (p.e: 255.255.255.0).

Les motivacións que porten la divisió d'una xarxa en subxarxes és *l'eficiència* i la *seguretat*. A continuació veiem un exemple de divisió en subxarxes:

Suposem que una empresa contracta l'adreça de classe C 200.200.200.0/24 a l'ISP (l'adreça de partida per a fer subnetting l'anomenarem *adreça base*). Suposem que l'empresa vol tenir 4 subxarxes. Per aconseguir-ho, agafem 2 bits del hostid (2<sup>2</sup> = 4) per a fer el subnetting (aquests bits s'anomenen subnetid i han de ser els més significatius del hostid).

![](/img/subnettingres.PNG)

Els pesos dels bits del subnetid són 2<sup>7</sup> = 128 i 2<sup>6</sup> = 64. A la taula es fa servir B = 200.200.200. La columna d'adreces disponibles compte el nombre d'adreces que queden al descomptar les adreces especials (la de la xarxa i la de broadcast). Per saber quants *hosts* es podrien connectar en cada subxarxa també hauríem de descomptar les adreces que consumeixen els routers.

![](/img/divisoensubxarxes.PNG)

