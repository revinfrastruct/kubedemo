# Vad är docker?

Random anteckningar och snippets inför demonstration av Docker och Kubernetes.

## 1. Att bygga och köra en container:

Docker är ett sätt att köra containrar (typ "virtuella maskiner").

Man bygger sin kontainer med en `Dockerfile`. Ett mycket enkelt exempel:

	FROM debian:jessie
	CMD ls /

`FROM` anger en image som man utgår ifrån.

`CMD` anger vilket kommando som ska köras på denna image.

Man bygger och kör en image med:

	$ docker build .
	$ docker run <hash>

## 2. Att anpassa sin container:

Man kan även anpassa sin image innan den körs genom `RUN`-kommandon:

	FROM debian:jessie
	RUN touch /hej
	RUN touch /hejsan
	CMD ls /

Varje variant får en egen hash. Det går fortfarande att köra den hash som gjordes för första versionen av Dockerfile, trots att Dockerfile är förändrad.

## 3. Infrastruktur som kod

Infrastruktur som kod betyder att man scriptar och automatiserar hela sin servermiljö. Fördelar:

* Går att återskapa miljön.
* Versionshantering av allt. Det går att se hur en servermiljö såg ut tidigare.
* Det går att enkelt skapa identiska testmiljöer. Du kan köra en miljö lokalt och sedan lägga hela miljön på servern.
* Man sprider möjligheten för andra att sätta sig in i hur en miljö ser ut. Hur servrar är uppsatta hänger ofta på väldigt personliga preferenser, precis som programmeringskod. Men programmeringskod är mycket mer överblickbar än en servermiljö. Om allt är scriptat på servern så får man samma överblickbarhet och större möjlighet för andra att överta eller sätta sig in i en servermiljö.

## 4. Något mer användbart

	FROM debian:jessie
	RUN apt-get -y update && apt-get -y upgrade
	RUN apt-get -y install nginx
	CMD service nginx start

...eller inte.

Docker är gjort för att en process per container ska köras i förgrunden. `service nginx start` kommer att göra nginx till en bakgrundsprocess och avsluta, och då avslutas även containern.

Därför måste vi göra nginx till en förgrundsprocess.

## 5. Vi gör nginx till en förgrundsprocess och testar igen

	FROM debian:jessie
	RUN apt-get -y update && apt-get -y upgrade
	RUN apt-get -y install nginx
	RUN echo "daemon off;" >> /etc/nginx/nginx.conf
	CMD service nginx start

Nu gick det betydligt snabbare att bygga! De två `apt-get`-raderna kördes inte ens, utan de hämtades från en cache! Varje `RUN` leder till att en filsystemsimage sparas med de förändringar som skett under respektive `RUN`. Det gör att man slipper sitta och vänta på att samma saker ska byggas om och om igen under utvecklingstiden.

Dessutom så startar nginx i förgrunden.

Nu kan vi se att processen kör:

	$ docker ps

## 6. Skaffa fram IP-adressen till den container som körs

Man kan se mer info om varje container genom

	$ docker inspect <hash>

Istället för `<hash>` så kan man använda det "name" som finns i kolumenen längst till höger när man gör en `docker ps`.

Tittar man igenom inspect-infon så hittar man IP-adressen och kan sufa till den.

## 7. Att skaffa en shell i en körande container

Istället för att använda `docker inspect` så kan man även få fram IP-adressen genom att öppna ett shell i containern.

Det är skillnad på den <hash> som containern har och den <hash> som imagen har. En image kan köras i flera instanser. Den hash som vi ska använda för att öppna ett shell är inte samma som vi använder för att starta en image.

	$ docker exec -it <hash>

`-it` använder vi för att vi vill att det ska fungera med ett interaktivt shell.

## 8. Att få containrar att hitta varandra genom /etc/hosts

Vi skapar en ny container:

	FROM debian:jessie
	RUN apt-get -y update && apt-get -y upgrade
	RUN apt-get -y install nodejs
	CMD echo "var counter = 0; var server = (require('http')).createServer(function(req, res) { res.end('' + (counter++)); }); server.listen(80);" | nodejs

Det är ingen vacker historia, men det är helt enkelt en liten http-server i node som räknar upp en siffra för varje inkommande request.

Vi kan namnge den container som vi startar denna app i:

	docker run --name app <hash>

Om man nu gör en `docker ps` så ser man att containern heter "app".

Om ni nu skapar ytterligare en container från denna Dockerfile:

	FROM debian:jessie
	CMD cat /etc/hosts

Och jämför resultaten mellan dessa två kommandon:

	$ docker run <hash>
	$ docker run --link app <hash>

Så ser vi att om vi lägger in en `--link` till en container som körs, så kommer vår hosts-fil att resolva containernamnet till ett IP-nummer. På det viset så får vi en enkel form av "service discovery" mellan containrar som dependar på varandra.

## 9. Att få containrar att hitta varandra genom miljövariabler

Om vi lägger till en `EXPOSE 80` i vår nodejs-server:

	FROM debian:jessie
	RUN apt-get -y update && apt-get -y upgrade
	RUN apt-get -y install nodejs
	EXPOSE 80
	CMD echo "var counter = 0; var server = (require('http')).createServer(function(req, res) { res.end('' + (counter++)); }); server.listen(80);" | nodejs

Och testar att göra en länk från en sådan container:

	FROM debian:jessie
	CMD set

Då ser vi att att en länk även skapar en rad miljövariabler, som även de kan användas för "service discovery".

## 10. Koppla ihop nginx-containern och app-containern

Nu kan vi göra en liten anpassning av nginx-container och få den att vara reverse proxy för nodejs-containern.

	FROM debian:jessie
	RUN apt-get -y update && apt-get -y upgrade
	RUN apt-get -y install nginx
	RUN echo "daemon off;" >> /etc/nginx/nginx.conf
	COPY default-nginx-site /etc/nginx/sites-available/default
	CMD service nginx start

Med kommandot `COPY` så lägger vi in en ny konfigurationsfil för nginx.

Filen `default-nginx-site` lägger vi i samma katalog som `Dockerfile`, och den kan se ut så här:

	server {
		location / {
			# First attempt to serve request as file, then
			# as directory, then fall back to displaying a 404.
			proxy_pass http://app:80;
		}
	}

Vi kommer att få "502 Bad Gateway" från nginx om vi kör denna nya nginx-image så här:

	$ docker run <hash>

Men om vi skapar en länk så fungerar vår reverse proxy:

	$ docker run --link app <hash>

## 11. Att publicera images till ett registry

Den där `FROM`-grejen överst i `Dockerfile` är namnet på en image som ligger på ett "Docker registry".

Vi kan själva publicera våra images till sådana registrys.

När vi bygger en dockerimage så kan vi tagga den med ett namn. Det innebär även att man slipper använda hasharna som vi har använt tidigare. Så här kan vi göra med vår nodejs-räknare:

	$ docker build -t node-counter:v1.0.0 .

Vi kan lista alla images som vi har lokalt, och då hittar vi version 1.0.0 av node-counter:

	$ docker images

För att publicera en image så behöver vi ett "docker registry", vilket vi enkelt sparkar igång lokalt för att experimentera:

	$ docker run -d -p 5000:5000 --restart=always --name registry registry:2

Detta lokala repository har nu inget SSL-cert, så man måste konfigurera sin docker-daemon att inte använda HTTPS när det snackar med detta registry.

Det finns även publika registrys som man kan publicera till.

Därefter lägger vi vår image på detta repository:

	$ docker tag node-counter:v1.0.0 192.168.99.1:5000/node-counter:v1.0.0
	$ docker push 192.168.99.1:5000/node-counter:v1.0.0

## 12. Att köra en grej per container

Vi har sett att docker vill att det finns en förgrundprocess och att containern dör när förgrundsprocessen försvinner.

Man kan köra hur många bakgrundsprocesser och servrar som helst i sin container, men det är god sed att man har en container per "grej", och kopplar ihop sina containrar, exempelvis genom att använda `--link`.

Fördelen är att man få en "separation of concerns", så att olika saker är tydligt separerade och tydligt interface (IP-paket).

## 13. `docker-compose`

`docker-compose` är ett verktyg som gör att man kan definiera hur flera olika containrar länkar ihop med varandra. `docker-compose` startar allt på en gång.

## 14. Cirkulära dependencies med `--link`

Ett problem med `--link` är att man inte kan ha cirkulära dependencies. Du kan inte länka till en container som inte kör, så den första containern som startar kan inte ha någon länk till någon annan container, och kan därför inte initiera kontakt till någon annan. (Eller jo, den kan ju, men den måste ha ett annat sätt än `--link` för service discovery.

Ett alternativt sätt för dependencies är så som Kubernetes löser cirkulära dependencies-problemet:

1. Att flera containrar kan dela nätverksinterface. Det innebär att containrarna når varandra på genom localhost (127.0.0.1) på olika portar. Det kräver alltså att man inte får lyssna på samma port i flera olika contrainrar.
2. Genom att namnen till olika containrar slås upp genom en "centraliserad" (typ) DNS-server istället för genom `/etc/hosts`. Det innebär att containrar som startat sent kan kollas upp av containrar som startat tidigt.

## 15. Orkestrering

"Orchestration" är ett begrepp som innebär att man har en automatiserad deploy och skalning av containers. Jag säger till mitt orkestreringsverktyg att jag har en särskild container som jag vill köra. Orkestreringen ser till att:

* containern körs någonstans i mitt cluster
* att den är nåbar genom någon slags "service discovery"
* att den startar igen någonstans om den kraschar eller om någon snubblar på elsladden till den fysiska maskinen som containern körde på
* att den skalar upp till flera instanser som kan lastbalansera jobbet om det behövs.
* Orkestreringsverktygen skulle även kunna starta upp flera maskiner att köra klustret på om resurserna tar slut, typ genom att starta nya maskiner på Amazon genom API-anrop.

Det finns ett antal olika orkestreringsverktyg, men det som jag främst har meckat med är Kubernetes.

Den process som avgör hur många instanser och var någonting ska köra (och eventuell när) brukar kallas för "scheduler".

Den process som kan starta upp nya maskiner när klustret behöver det brukar kallas för "provisioner".

## 16. minikube

Minikube är ett lokalt kubernetes-kluster som man kan använda som utvecklingsmiljö.

Efter installation av Minikube och VirtualBox (eller hur man nu gör om man inte kör Linux) så kör vi:

	$ minikube start --insecure-registry 192.168.99.1:5000

`kubectl` heter det verktyg som man använder för att administrera sitt Kubernetes-kluster, och med det kan vi konstatera att minikube är igång:

	$ kubectl config use-context minikube
	$ kubectl get nodes

Den första raden anger att kubectl ska jobba mot minikube-klustret. När man har flera olika kluster så behöver man byta context (läs kluster) ibland.

Den andra raden listar vilka noder som är igång i klustret.

## 17. Begrepp i kubernetessvängen:

* "Nodes" är en maskin som Kubernetes kör på. Det kan vara en fysisk dator eller en virtuell.
* "Pods" är ett bregrepp för en samling av containrar. När vi snackade om att flera containrar delar ett och samma nätverksinterface, så talade vi om en "pod". En pod är alltså en grupp containrar som sitter ihop i en enhet. När kubernetes startar, skalar och stoppar saker som körs i klustret, så gör den det på hela "poddar". En container inom en "podd" kan inte köra utan att de andra containrarna i samma podd också körs.
* "Namespaces" är ett sätt att skapa olika scopes inom ett kluster. Inom ett namespace så måste alla resurser i clustret vara unika. Kan vara rimligt att ha ett namespace per sajt om man kör flera sajter inom ett cluster, till exempel.

## 18. Kubernetes dashboard

För att starta kubernetes dashboard i default-webbläsaren så kör man

	$ minikube dashboard

## 19. Låt oss deploya vår nodejs-app till kubernetes

Nu kan vi deploya denna image till kubernetes:

	$ kubectl run mycounter --image=192.168.99.1:5000/node-counter:v1.0.0 --port=80
	$ kubectl expose deployment mycounter --type=NodePort
	$ minikube service mycounter --url

Den första raden gör att kubernetes laddar in dockerimagen och kör den (som en "pod").

Den andra raden gör så att podden blir en "service" inom Kubernetesclustret. Det innebär att de får ett lastbalanserande internt IP-nummer som kommer att peka trafik till en podd som kör vår counter. Om den fysiska maskinen i ett cluster dör eller om vår container kraschar, så kommer kubernetes att starta vår podd någon annanstans, och den får då ett nytt ip, men det interna service-IP:t kommer inte att ändras. Och om Kubernetes skalar upp vår counter så att flera poddar körs i klustret, så kommer service-IP:t att lastbalansera mellan de olika poddarna.

Den sista raden gör så att minikube ger oss en adress utanför klustret där vi kan nå vår service.

Nu kan man ssh:a in på vår virtuella maskin som kör minikube med

	$ minikube ssh

och därefter titta runt lite:

	$ docker ps 

## 20. Skala upp vår pod

Här ser man antalet poddar som kör:

	kubectl get deployments

Nu väljer vi att skala upp:

	$ kubectl scale --replicas=3 deployment/mycounter

## Mer vokabulär

* Pause container - Den container som körs i varje podd för att skapa det nätverksinterface som alla andra containrar i samma podd använder sig av.
* Manifest - en fil (i JSON eller YAML) som definierar en resurs i Kubernetes. En resurs kan exempelvis vara en pod eller en service.
* Kube addon manager - en process som kollar om det dyker upp nya manifestfiler på en node och i sådana fall kör dessa.
* PetSet - en "podd" är stateless, ska kunna dö närsomhelst och då köras igång någon annan stans. En "pet" är däremot någonting som har ett mer beständigt state och som inte kan vara lika rörligt i klustret. De kräver en mer manuell hantering för provisioning av storage t.ex. (Namnet kommer från Pets vs Cattle.) Man bör undvika pets.
* Replica Sets - underliggande teknik unde "Deployments". Ser till att ett visst antal av en podd körs.
* Replication Controller - En äldre version av Replica Sets.
* Deployments - ett deklarativt interface för kontrollera replica sets och göra rollouts and rollbacks.
* Daemon sets - En resurs som säkerställer att en viss pod körs på alla noder. Alltså saker som man vill ska köras i lokal kopia överallt definieras med daemon sets. (Tänk loggning, nått delat filsystem, eller nått dylikt.)
* Jobs - någonting som inte ska köras hela tiden, utan ett engångsjobb som startas en gång inom klustret och sedan avslutas (och då inte startas upp igen av Kubernetes).
* Ingress - Tar en anslutning som kommer utifrån och pekar den till rätt Service internt i klustret. Gör lastbalansering.
* Service - Intern lastbalansering och service discovery i klustret.
