# TILL UTVÄRDERINGEN

## Allmänt spånande:

* Bildmaterial postat på egen twitterfeed bör läsas in och återpubliceras
  av oss själva. Kanske även video, under förutsättning att vi kan få ut
  materialet med rimlig encoding osv.
* Ticks gick ut olika snabbt på olika ställen, sannolikt för att olika
  Cloudfront-edgeservrar har varit olika snabba på att rensa cache. Vore
  intressant att samla statistik på hur snabbt det har gått. Google Analytics-
  events, kanske?
* Bättre hantering av bilder i olika format. Ha flera designmallar för ticks som
  bättre presenterar bilder i mindre storlek, stående osv?
* Vore intressant att veta hur länge folk är inne på tickern, hur långa
  sessionerna är.
* När man har scrollat ner så vill man få en indikator som syns någonstans
  om det finns nya meddelanden i flödet. (Kanske rentav klickbar med scroll upp
  till senaste inläggen?)
* Man skulle vilja få en "pling" när det hänt nått nytt? Det finns ju nått
  notifikationssystem på iPhone som 0x4d snackade om. Chrome och Firefox har
  och notifikationssystem. What 'bout Android?
* När JSON-blobben blivit för stor så blev renderingen långsam. Implementera
  inladdning och rendering i flera steg. (Initial state bla bla.)
* Förslag: Omedelbar feedback på att grejer har gått ut, direkt till en
  särskild kanal i Slack? Kanske rentav publicering från Slack också?
* Ha en bra kontaktväg, lite typ Wikileaks style kanske? D.v.s. att "garantera"
  anonymitet. Så att folk kan skicka tips om grejer utan att använda personliga
  konton på twitter osv.
* Snabba upp uppdateringarna av info. Twitter stream-API istället för polling
  som det nu blev i sista minuten, websocket från AWS IoT/MQTT, osv.
* Kanske skippa Wordpresskopplingen? Bara Twitter (+ Slack) som backend för
  ticker.

