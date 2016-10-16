# Spec för formgivning

Detta dokument ska läsas vid sidan av wireframe gjord i HTML som finns i
repositoryt [https://github.com/revinfrastruct/ticker-wireframe](ticker-wireframe).

## Funktion på första versionen av sajten

* Live-ticker.
* Radiospelare (i princip en playknapp).

## Beskrivning av wireframen

* Wireframen är responsiv.
* En header med logotyp. I wireframen så har headern samma storlek för både
desktop och mobil, och så kan det vara, men är inte ett krav.
* Potentiell slogan "Från information till aktion", om det formmässigt passar
och är lämpligt.
* Radioplayknapp relativt högt upp på sidan, alltså utan att man behöver
scrolla någonstans.
* Ingen meny behövs (än), men i senare versioner av sajten så kommer det att
behövas. Man kan tänka med det redan nu, eller så blir det en senare grej att
mecka med.
* En rubrik för tickerflödet, som är en hashtag.
* En toggle/checkbox/e.dyl. för att slå av/på live-uppdatering av tickerflödet.
* En beskrivning av vad tickerflödet gäller.
* Ett flöde av ticks.
* Ingen info typ "vad är motkraft?", eller dylikt. Det behövs inte. Vi finns
till för rörelsen och är du i rörelsen så vet du redan.
* Inga kontaktuppgifter, du ringer inte oss, utan vi ringer dig.
* Tickerflödet är i denna första version inte paginerad, eftersom att vi
inbillar oss att det inte behövs.

### Ticks (ticker-meddelanden):

Innehåller alltid:

* En text. Förhållandevis kort. Texten han innehålla länkar och viss stilning
(typ fet/kursiv om sådant behov skulle uppstå).
* En tid för när tickermeddelandet publicerades. Detta ska publiceras i
formatet: "För X sekunder/minuter sedan" när den är ganska närliggande i tid,
och efter några timmar bli ett datum och klockslag.

En tick kan innehålla inbäddad content från externa sajter, typ en tweet
eller instagraminbäddning.

Vi lägger inte själva upp bildmaterial eller video och ljud, utan sådant
bäddas in ifrån extern social media om det ska publiceras.

Grafiska effekter:

* Någonting som markerar för ögat att ett tickermeddelande är nytt i flödet.
Någonting som sätter fokus men som kan fade:a bort (typ?) efter någon minut.
Bakgrundsfärg? Något grafiskt element?
* Någonting som markerar att ett tickermeddelande är viktigare än andra. Vi
vill kunna markera ett meddelande som viktigare att läsa än övriga, men att det
ändå befinner sig i flödet. Här kan man tänka sig att dessa meddelanden
formmässigt/typografiskt ser annorlunda ut, kanske större? Eller har de något
grafisk element som pekar ut dem som extra viktiga?

## Några tankar och principer

* Responsiv sajt. Mobilupplevelsen viktigast. Först och främst är vi ett stöd
till aktivisterna på gatan, i andra hand nått som sopor på Twitter kan dela
till varandra.
* Liten header i Mobil, så att man omedelbart får en överblick över de senaste
grejerna i tickern när man surfar in i mobilen, utan att scrolla (beror
naturligtvis på hur blaffig den senaste tickern är, men ja...)

## Känsla

* Intern. Vi måste komma bort ifrån att vara mediapersonligheter och att nästa
karriärsteg efter aktivist är journalist.
* Inte för mörk.

## Hur formen kommer att användas

När formgivning är färdig så kommer vi att göra den till ett "pattern lib"
med atomiska komponenter.

Därefter kommer vi att faktiskt bygga sajten Motkraft med de komponenter och
patterns som vi definierat.

## Changelog

### Version 2

Städat upp och lyft in några saker ifrån mötet 15 oktober 2016 i Skarpnäck.

### Version 1

Kopierat spån-spec från plankapaddokument.
