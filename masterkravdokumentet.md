# Masterkravdokumentet

Sammanfattning av diskussion och idéer under första konstituerande tiden på
Mattemost-chatten (och Facebookgruppen kanske?).

## Bakgrund

Aktivistplattformarna har varit relativt kortlivade och har ofta dött ut på
grund av att teknikbiten inte har fungerat i längden. Det har berott på en rad
saker:

* Att det är ett stort arbete bakom att ta fram webbsajter, och det har
underskattats, eller så har tiden inte räckt till.

## Mål

### Politik

* Att ha en teknisk plattform för politisk aktivistiskt kodande på Internet.
* Att ha Motkraft på ovanstående plattform.
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




