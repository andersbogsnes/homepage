---
title: "Machine Learning Isbjerget - de resterende 90%"
date: 2019-10-03T18:13:58+02:00
slug: "machine-learning-isbjerget"
description: "De resterende 90% af Machine Learning processen"
keywords: []
tags: []
toc: true
---

Beslutningen om at ansætte nogle datascientists og slippe dem løs på noget data virker ligetil - men til stor frustration for dine nye medarbejdere og resten af organisationen, så sker der ofte ikke så meget mere.

Der er ingen der seriøst ville overveje at investere i at bygge en webapp uden at vide hvordan den skulle sættes i produktion.

Så hvorfor hører vi så tit om modeller der forbliver på en datascientists laptop fordi det er for besværligt at produktionssætte den?

Undgå at være blandt dem der ikke har tænkt igennem de resterende 90% af Machine Learning isbjerget!

## Hvad er produktet?

Før man går i gang med at bygge modellen, er det viktigt at forstå hvad produktet er. Hvad vil vi opnå?
Vil vi bygge ny feature i vores app der gør kundens liv nemmere?

Vil vi lancere ny marketing kampagne skræddersyet til den enkelte kunde?

Vil vi hjælpe kunden med at prioritere blandt vores 1000 forskellige valgmuligheder?

At forstå produktet der skal bygges styre svarene på de resterende spørgsmål, og ikke mindst styre valg af algoritmer og metrikker

## Hvor skal modellen køre?

Modellen skal afvikles et sted, og det vil typisk være en server.

Har vi allerede en server vi kan bruge? Den skal nok tilpasses for at håndtere de mere komplicerede afhængigheder machine learning modeller typisk har.

Vil vi selv stå for opsætning så vi er sikre på at den passer til vores behov? Kan vi bruge containers? En af de mange cloud løsninger?

Alt er en tradeoff mellem customization og vedligehold og det skal matche teamets tekniske formåen og behov.

## Hvordan skal modellen anvendes?

Produktet skal anvende modellen på en eller anden måde. Skal den hente en flad fil ind? Så har vi brug for at sætte op en batchkørsel. Hvilket filformat giver mening i applikationen? Hvordan skal det skeduleres? Hvordan sikrer vi at vi får loadet filen ind i produktet? Eller er det en database integration?Det kræver en del integrationsarbejde at gå den vej.

Giver det mere mening at bygge et API?
Det er nemt at bygge et API ovenpå en model i Python og det er væsentlig nemmere at integrere og ikke mindst genbruge i andre produkter.

API vejen betyder dog skrappere krav til responstid - appen vil gerne have svaret fra modellen så hurtigt som mulig, så et par millisekunder ekstra responstid kan være forskellen på suksess og flop.

## Hvor er input data?

Modeller bliver ofte bygget udfra et udtræk fra kernesystemer, CRM osv. Men når modellen skal produktionssættes, så er krav til input data anderledes, og det er ofte ikke muligt at hive det direkte fra kilden.

Du bliver ikke populær hvis du lægger ned salgssystemet fordi din model spørger om data 60.000 gange om dagen!

Så hvor skal data trækkes fra? Skal der sættes op batch jobs der overfører data fra kernesystemer? Hvordan loades tredjepartsdata? Hvor meget af data er tilgængelig upfront, og hvad er kun tilgængelig i prædiktionsøjeblikket?

Måske skal input data opaggreres fra produktniveau til kundeniveau? Måske skal salget være summen af alle salgssystemer? Skal der laves custom ETL til at håndtere det?

## Hvordan dannes dine features?

Hvilke features man vælger at bruge i modellen ændrer sig hurtigt. For at følge med, skal man kunne tilføje nye features hurtigt, og smertefrit når datascientisten finder en ny måde at anvende data på.

Skal det ske i ETL pipelinen? Hvem skal bygge og servicere den ETL? Er det datascientistene selv? Er det en realtidstransformation i prædiktionsøjeblikket?

Hvordan undgår vi dobbeltarbejde? Hvad med håndtering af  inkonsistente begreber på tværs af modeller? Har vi brug for en central feature store?

## Hvor er output data?

Modellerne kommer til at generere en masse prædiktioner, og for at kunne følge op på dem, er vi nødt til at gemme output. Hvilke prediktioner, blev lavet hvornår af hvilken model? Til hvilket formål?

Vi er også nødt til at sørge for at udfaldet af vores prædiktion bliver gemt, så vi kan se om modellerne faktisk gør hvad de skal. Hvordan sørger vi for at resultatet bliver gemt hver gang, på en måde hvor vi kan koble dem til modellens prædiktion?

Skal vi også gemme input data, hvis din datakilde ikke sørger for historik? Er det personfølsomt? Hvordan skal tilgang til data styres? Hvordan skal metadata om hvilken model der genererede prædiktionen gemmes?

## Hvordan versionerer I modellerne?

En model er ikke one-and-done - vi arbejder videre med den, tuner den, finder nye datakilder og indarbejder feedback fra vores slutbrugere.

Hvad er planen for at opgradere modellen? Hvordan holder vi styr på versionering af dem? Hvordan ved vi hvilke prædiktioner der kommer fra hvilke modeller? Hvordan ruller vi tilbage til en ældre version hvis noget går galt? Kan vi AB teste dem før vi skifter over?

Med GDPR regler, er der endnu viktigere at vi kan spore en prædiktion til en given version af koden!

## Hvordan overvåges systemet?

Hvordan har vi tænkt at overvåge disse bevægelige dele? Hvis vi har en API, hvordan overvåges CPU & RAM forbrug, diskplads og responstid?

Hvordan ser vi hvilke SQL queries der forårsager stor load på databasen, så vi kan kigge på optimeringer?

Når input data dannes, hvordan holder vi øje med om der er fejl i data? Hvad hvis distributionerne i data har ændret sig siden modellen blev trænet? Måske har en marketingkampagne gjort at den gennemsnitlige alder har faldet markant i kundebasen? Eller har en nøglemetrik ændret definition?

Alt dette bør man ideelt kunne overvåge, da det vil påvirke modellen og kan potentiel gøre den ubrugelig fra en dag til den anden.

# Machine Learning er meget mere end en algoritme

Machine Learnings potentiale er stadig enormt stor i mange virksomheder, og det er naturligt at ville udforske det.

Men før man sætter igang det store Machine Learning initiativ, er det viktigt at man har taget stilling til det fundament der skal til for at høste gevinsten. Dette er kun et udvalg af spørgsmål man bør tænke over!

Husk, det er aldrig den synlige del af isbjerget der synker skibet!
