# Blog entry
Denne blog er skrevet af:

- Kristoffer Noga
- Christopher Borum



## Sikkerhed i store systemer spiller en altafgørende rolle

*Uden implementering af sikkerhed ved brug af MongoDB opstår der en enorm trussel overfor systemets data og dermed dets brugere. Sikkerhed spiller i dag en altafgørende rolle for store systemer i en verden, hvor brugernes data kan misbruges som aldrig før. Ved hjælp af en række praksisser og en ordentlig opsætning af MongoDB kan denne trussel formindskes kraftigt. Dette resulterer i en database, der er parat til nutidens mange trusler når systemet tages i brug.*

### Hvorfor er manglende sikkerhed et problem?

Vi lever i dag i en verden, hvor stort set alle har en mobiltelefon, en tablet eller en computer og det betyder, at mængden af software er stigende. Da flere og flere mennesker går rundt med enheder der er forbundet til internettet, betyder det også, at der findes mere og mere brugerdata. Det data skal alt sammen gemmes i en database, og disse databaser skal gemmes på et hav af servere. Det betyder med andre ord, at databaserne indeholder ekstremt værdifuld information om de brugere, der gør brug af et hav af forskellige apps som alle sammen samler på specifik data.

Det data som bliver gemt i databaserne har en stor værdi for hackere, som ved adgang til det data kan udnytte, misbruge og stjæle brugernes information. I og med mængden af software er stigende betyder det også, at mængden af malware, ransomware og generelle vira er stigende. Udover at brugerne kan blive misbrugt har det også alvorlige konsekvenser for udviklerne af systemet, hvis data bliver stjålet eller slettet fra systemet. Man kan derfor ikke længere komme uden om, at man må sikre sit system for denne type angreb, hvis man ønsker at udvikle systemer, der kræver at brugere oplyser deres personlige data.

> *Uden specifik opsætning er MongoDB på flere områder utrolig usikkert, og det kan betyde, at udefrakommende kan få nem adgang til databasen.*



### Hvorfor er MongoDB usikkert?

I januar 2017 kom antallet af Mongo-databaser ramt af ransomware op på 32.000. Disse type angreb går i al sin enkelthed ud på, at hackere får adgang til en mongo-database, kopiere al data i databasen og derefter sletter al data. Herefter får ejeren af databasen en besked om, at de imod betaling (ofte i Bitcoins) kan få data tilbage igen. 
MongoDB er i flere tilfælde blevet kritiseret for dets manglende sikkerhed og de nye tal taler for sig selv.

På trods af kritikken fra flere sikkerhedseksperter er MongoDB faktisk en sikker database. Men man skal dog vide noget om MongoDB’s nødvendige konfigurering for at opnå en tilfredsstillende sikkerhed. De primære syndere hos MongoDB er:

- At gøre brug af default porte
- At glemme at slå authentication til med det samme
- Ikke gøre brug af SSL på databasen
- Ikke formindske hvilke netværk/IP’er, der må snakke sammen med databasen

Der findes også flere specifikke områder man skal være opmærksom på, men det kommer meget an på den specifikke applikation. 
Det kan i den forbindelse diskuteres om hvorvidt MongoDB kan kategoriseres som usikker. Det kommer an på om man mener, at en open-source database som MongoDB bør komme med konfigureret sikkerhed out of the box. MongoDB har allerede taget højde for stort set alle aspekter af sikkerhed når det kommer til NoSQL-databaser, men det kræver at man som udvikler ved, hvad man installerer og hvordan man bruger det og sætter det op. 
Dette resulterer i, at mindre erfarne udviklere ikke tager højde for den manglende konfiguration, og det har derfor ofte konsekvenser. 

### Hvad er konsekvensen på den manglende konfigurering?

Denne problemstilling med manglende konfigurering på en MongoDB-database har vi selv erfaring med. Som en del af vores fag, Large System Development, udviklede vi et stort system, der gjorde brug af en MongoDB-database. 
Vi havde dog ikke taget højde for den manglende sikkerhedskonfigurering på MongoDB. I virkeligheden var vi faktisk slet ikke klar over, at det var nødvendigt at konfigurere MongoDB.

Der gik ikke længe før vi selv blev angrebet af ransomware og det lykkedes faktisk hackerne at få adgang til databasen og derefter slet al vores data:

![alt text](http://212.47.237.59:6001/test/blog/Screen%20Shot%202017-12-10%20at%2012.59.55.png)

Vi kan med andre ord skrive under på, at MongoDB er utrolig usikkert, hvis ikke det konfigureres korrekt. 
Heldigvis for os betød dette angreb ikke noget, da al vores data bestod af værdiløs testdata, men det er uden tvivl stadig et bevis på, hvor stor garanti der er for, at store systemer bliver sat under angreb konstant. 

Dette angreb er højest sandsynligt et resultat af, at vores MongoDB var sat op til at lytte på default porten 27017, og at vi ikke havde sat authentication op på databasen. Det har muligvis været en bot, der scraper porte på forskellige IP’er, og da den har ramt vores har den hurtigt fundet ud af, at vores MongoDB overhovedet ikke havde konfigureret sikkerhed. 
Da vi heller ikke have sat en firewall op, der bremsede udefrakommende forbindelser på den port er det ikke underligt, at det gik galt. 

### Hvad skal man være særlig opmærksom på?

Angrebet vi blev udsat for er på mange måder vores egen skyld, da det relativt nemt kunne forhindres. Som MongoDB selv gør opmærksom på i deres [Security Checklist](https://docs.mongodb.com/manual/administration/security-checklist/) er det relativ få ting man skal sætte op for at undgå et angreb som dette.
Det havde været en kæmpe fordel ikke at bruge MongoDB’s default port, da det havde gjort arbejdet for den bot der angreb os væsentligt sværere. Yderligere havde vi ikke sat authentication op på databasen ligesom vi heller ikke havde taget højde for, at vores firewall ikke blokerede ukendt udefrakommende trafik på vores MongoDB-port.

Vi har siden angrebet igen fået kontrol over databasen, selvom vi har mistet en masse testdata, som vi aldrig får igen. Havde det været et virkelig scenarie, hvor vi havde en masse specifik brugerdata liggende i databasen, havde det haft store konsekvenser. 
Vi har siden hen sat en firewall op, der blokerer på port 27017 således at vi ikke længere er modtagelige over for samme type angreb. Yderligere skal vi nu tage højde for authentication på databasen, og overveje om vi ikke skal skifte port. 
Der findes i øvrigt også et hav af andre konfigureringer som man bør tage et kig på, hvis man vil sikre sin database mod flest mulige angreb.

![alt text](http://212.47.237.59:6001/test/blog/Screen%20Shot%202017-12-10%20at%2012.55.37.png)

### Hvad kan vi tage med herfra?

Der kan ikke være tvivl om, at MongoDB er utrolig usikkert, hvis der ikke tages højde for den korrekte opsætning. Man bør altid sætte sig ind i dokumentationen når man vælger at bruge en open-source database. Generelt kan man konkludere, at databaser er utrolig udsat for angreb og at man derfor bør sikre sig mod angreb ligegyldig hvilken type database man bruger. 
Det er også en påmindelse om, at der findes mange trusler på nettet, når man udvikler et system. Vi havde overhovedet ikke overvejet dette scenarie da vi deployede vores database på en ekstern server. Det har heldigvis været en øjenåbner at få angrebet sin database, selvom systemet ingen brugere havde eller på nogen måde var kendt på nettet. Man bør derfor som det første have sikkerhed i tankerne når man overvejer, hvilken database man vælger at bruge. Yderligere skal man huske at sætte sikkerhed op på databasen før man tager den brug. Det absolut værste der kan ske for et system er en hacked database. 

Sammen med en ordentlig opsætning af ens database kan det også være en idé at tage regelmæssige backups af sin database. I vores tilfælde havde det betydet, at vi faktisk ikke havde mistet vores data, da vi altid kunne hente en backup i stedet. 
Vi havde faktisk en gammel backup af vores data, da vi på et tidspunkt skiftede fra en databaseudbyder til at sætte MongoDB op selv lokalt på den eksterne server. Vi havde med andre ord ikke mistet al data, men det var dog heller ikke meget vi havde reddet. Med regelmæssige backups kunne vi have minimeret den påvirkning angrebet havde. 


#### Kildehenvisning

- http://www.theregister.co.uk/2017/01/09/mongodb/ 
- https://www.theregister.co.uk/2017/01/11/mongodb_ransomware_followup/
- https://www.mongodb.com/blog/post/how-to-avoid-a-malicious-attack-that-ransoms-your-data#suggested-steps
- https://www.infoworld.com/article/3164504/security/the-essential-guide-to-mongodb-security.html
- https://docs.mongodb.com/manual/administration/security-checklist/
- https://www.mongodb.com/blog/post/update-how-to-avoid-a-malicious-attack-that-ransoms-your-data

