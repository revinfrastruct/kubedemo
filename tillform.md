# Spec för formgivning

## Minimikrav:

### Funktionalitet på sidan:

* Live-ticker.

### Några tankar och principer:

* Responsiv sajt. Mobilupplevelsen viktigast. Först och främst är vi ett stöd till aktivisterna på gatan, i andra hand nått som sopor på Twitter kan dela till varandra.
* Liten header i Mobil, så att man omedelbart får en överblick över de senaste grejerna i tickern när man surfar in i mobilen, utan att scrolla (beror naturligtvis på hur blaffig den senaste tickern är, men ja...)

### Ett tickermeddelande ska alltid ha:

* En text. Förhållandevis kort. Det är rimligt om ett tickermeddelande aldrig är längre än 300 tecken?
* En tid för när tickermeddelandet publicerades. Dessa bör publiceras i formatet: "För X sekunder/minuter sedan".

### Dessutom kan ett tickermeddelande innehålla:

* En eller flera media. Video, bild eller ljud.

Ingen info typ "vad är motkraft?", eller dylikt. Det behövs inte. Vi finns till för rörelsen och är du i rörelsen så vet du redan.
Inga kontaktuppgifter, du ringer inte oss, utan vi ringer dig.
Kanske rimligt med ett kort mission statement dock, typ sammanfattning i en mening. För den oinsatte som skulle kunna vara lockad. :)

## Om tid finns-grejer:

### Extra features i tickermeddelanden:

* Share-knapp och då dyker en länk upp som man kan kopiera för at dela på Fejjan eller Twitter osv.
* Ovanstående share-knapp kräver även en landningsvy för varje tickermeddelande. Det bör vara samma sida som normalt, men med ett mörkt halvtransparent lager ovanpå som skuggar ut vanliga sidan, och sedan en (centrerad?) presentation av ett enskilt ticker-meddelande. Med en kryssknapp uppe till höger för att stänga det enskilda tickermeddelanden och så att hela tickerflödet visas igen.

### Grafiska effekter:

* Någonting som markerar för ögat att ett tickermeddelande är nytt i flödet. Någonting som sätter fokus men som kan fade:a bort (typ?) efter någon minut. Bakgrundsfärg? Något grafiskt element?
* Någonting som markerar att ett tickermeddelande är viktigare än andra. Vi vill kunna markera ett meddelande som viktigare att läsa än övriga, men att det ändå befinner sig i flödet. Här kan man tänka sig att dessa meddelanden formmässigt/typografiskt ser annorlunda ut, kanske större? Eller har de något grafisk element som pekar ut dem som extra viktiga?

## Några spångrejer:

* Slogan "Från information till aktion".
* Känsla: Intern. Vi måste komma bort ifrån att vara mediapersonligheter och att nästa karriärsteg efter aktivist är journalist.

## UI/UX

* Sidan bör vara aggressivt mobile first, dvs att utgångspunkten för all design och gränssnitt är mobila enheter. Förutom formen så innebär det en del saker rent tekniskt.
* Sidan måste levereras läsbar direkt från servern. Innan stilmallar, script och bilder laddas måste texten gå att läsa, såklart då även om dessa andra saker mot förmodan inte laddar.
* Sidan ska servas över HTTP/2.
* Alla statiska resurser som stilmallar, script och bilder ska ha oändlig cache och versionshanteras via filnamn.
* Stilmallar ska vara responsiva för olika skärmstorlekar och enheter.
* Bilder ska laddas progressivt och vid behov.
* Responsiva bilder bör användas för UI-element.
* SVG-bilder, möjligtvis inbäddade i stilmallar, bör användas för UI-element.
* Innehållet måste utgå ifrån högupplösta skärmar, snarare än lågupplösta desktopskärmar.

## Changelog

### Version 1

Kopierat spån-spec från plankapaddokument.

