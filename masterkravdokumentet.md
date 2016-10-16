# Masterkravdokumentet

Detta dokument är en del av en rörlig process. Det innehåller motstridigheter
och är ostrukturerat men ger en bild av en generell riktning.

## Changelog

### Version 2

Gått igenom den tidiga konstituerande diskussionen i facebookgruppen och
lagt till saker därifrån.

La till några saker som nämndes under heldagsworkshopen i Skarpnäck 15 oktober.

### Version 1

Alfred: "Typ kvällen innan Mattermost-haveriet så satt jag och gick igenom det
som skrivits och försökte bara sammanfatta ner det i punkter. Jag minns inte om
jag blev helt färdig med det innan haveriet. Man kan väl betrakta det somxi
stödord för en sammanfattning av en spretig diskussion typ."

## Bakgrund

Aktivistplattformarna har varit relativt kortlivade och har ofta dött ut på
grund av att teknikbiten inte har fungerat i längden. Det har berott på en rad
saker:

* Att det är ett stort arbete bakom att ta fram webbsajter, och det har
underskattats, eller så har tiden inte räckt till.
* Det har varit en separat teknikgrupp som gör just tekniken (åtminstone
då och då), och att övriga blir typ en kravställningsorganisation gentemot
teknikgruppen. Känslan och deltaktighet och bla bla, kan har gjort det mindre
kul i längden.
* Även om tidigare plattformar byggt på öppen källkod, så har det fortfarande
varit ett ganska jobbigt tekniskt insteg för att kunna bidra, eftersom att
själva setupen varit tråkig och onödigt krånglig att göra.
* Mycket tråkig support och svajjig/testad miljö för Motkrafts bloggar, som
ledde till avhopp.

## Mål

### Politik

* Att ha en teknisk plattform för politisk aktivistiskt kodande på Internet.
* Att ha Motkraft på ovanstående plattform.
* Att Motkraft främst ska förmedla (verifierad) information, inte vara en
avsändare, men inte heller vara journalister.
* Att publicera AGPL-kod.

### Innehåll Motkraft

* Realtidsinfo från demonstrationer osv. (Tickers.) 
* Återpublicering av tidigare politiska plattformars material, för att skapa
en sammanhängande historia sedan Internet slog igenom.
* Streamande live-radio.
* Kalender/pepp.
* Insamlingar när det går åt skogen.
* Pokemon Go för autonoma.

### Arbetsprinciper

* Teknikdriven utveckling. Vi får inte hamna i fällan att teknikarbetet sker
på uppdrag från någon annan, utan tekniken ska göras för att den är
engagerande i sig.
* Steget att börja bidra med kod måste vara enkelt. Låga trösklar, inte bara
vad gäller själva kodandet, utan även för att sätta upp lokal utvecklingsmiljö
och deploy.
* All kod ska publiceras öppet och på ett sådant vis att det lockar fler att
bidra med utveckling.
* Vi ska använda oss av bra befintlig öppen mjukvara och inte uppfinna hjulet
bara för att det är roligt.
* Vi ska uppfinna hjulet igen eftersom att det är roligt och man lär sig saker.
* Issuetracker och task manager.
* Jobba med "minimum viable product (MVP)"-tänk kring Motkraft.
* Lära sig nya saker.
* Dokumentation och folkbildning.
* Bra kommunkationskanal som inte är Facebook.

### Teknikval etc.

#### Dokumentation

* Ordentlig plan för hur olika microservices sammanfogas.
* Ha träffar just för dokumentation och arkitektur/planering.

#### Utvecklingsmiljö

* Docker compose lokalt och Kubernetes deployat.

#### Tester

* Ska ska ha bra möjligheter till tester.
* Continous Integration/Delivery

#### Serverplattform

* Inrastruktur som kod. (Alltså att servrar och konfiguration är definierat
som versionshanterbar kod, och att setup osv alltså är automatiserat.)
* Ddos-skydd/CDN.
* Docker.
* Delvis garderobs-hostat kuberneteskluster.
* Rolling-restarts

#### Kod - generellt

* Versionshantering.
* Cykliska releaser ofta istället för stora versioner.

#### Kod - server side

* AMQP.

#### Kod - client side

* Mobile first och snålt.
* HTTP/2.
* Service workers och offline storage.
* Inte stödja gamla browsers.

#### Integration

* Flera kanaler att nå ut genom när saker går sönder (backupsajt, data ut på
sociala medier).

## Intressanta uppslag

* "gällande balansen "engagera genom locka med lära" vs "utgå från baseline
mnga kan och se resultat" så gjorde sparvnästet en deltagarstudie på typ såna
teman. ingick att gå på massa hackathons, geekups, aktivtiska hackerspaces o
allt vad det heter. tyckte det verkade som de producerade lite knowledge om hur
harnessa lärandet/självförverkligande för sociala ändamål."

## Konkretisering av teknisk plattform vid möte i Skarpnäck 2016-10-15

* WordPress som adminverktyg, men ej för att leverera själva sajten till
slutanvändare.
* Vi lyfter ut data ur WordPress med JSON-APIet.
* WordPress stöd för OEmbed är den integration vi har för inbäddning av
externt innehåll.
* En frontend är påbörjad för att ticks ska kunna distribueras genom RabbitMQ
och Socket.io.
* En wireframe framtagen under mötet för en första version med ticker.

## Credz

* Altemark för begreppet "masterkravdokumentet".


