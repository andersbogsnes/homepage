---
title: "Featurestores - Skaler din data til datascience"
date: 2020-09-03T10:42:36+02:00
slug: "featurestores"
description: "Featurestore - Datascientistens datakatalog"
keywords: []
draft: true
tags: []
math: false
toc: true
---

Featurestoren er et nyt værktøj i værktøjskassen til datascience teams. Uber skrev om sin Featurestore [Michelangelo](https://eng.uber.com/michelangelo-machine-learning-platform/) tilbage i 2017, det samme år som Airbnb begyndte at skrive om sin Featurestore [Zipline](https://medium.com/airbnb-engineering/using-machine-learning-to-predict-value-of-homes-on-airbnb-9272d3d4739d). Der er nu en lang række virksomheder der har indset at
Featurestores er en central del af deres ML platform - Facebooks [FBLearner](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/), Netflix [Metaflow](https://metaflow.org/), Google's [Feast](https://cloud.google.com/blog/products/ai-machine-learning/introducing-feast-an-open-source-feature-store-for-machine-learning) og mange andre. Der er også platforme der leverer *Featurestore-as-a-service* som [Hopsworks](https://www.logicalclocks.com/) og [Tecton](https://www.tecton.ai/) men trenden er helt klart at bygge sit eget.

## Hvad er en Featurestore?

En featurestore er, kort fortalt, et centralt sted hvor datascientists kan tilgå prædefinerede features til brug i deres modeller.
Features der ligger i featurestoren er typisk gennemtestet, monitoreret og bliver genereret automatisk med et eller andet interval

En typisk workflow for en datascientist er at starte fra rå data, enten et SQL udtræk eller en datadump fil. Når udtrækket lander, begynder man at danne sig et overblik over hvilke data der kan bruges, og hvordan det kan transformeres. Fødselsdatoer bliver konverteret til aldre, der køres måske en større beregning for at finde hvornår kunden sidst købte noget og man renser lige nogle kolonner der ikke så helt riktige ud. Populært sagt, så er datadelen 80% af Machine Learning.

Hvad sker der så i næste projekt? Best case, så husker det nye projekt at nogen andre har noget kode der lignede det man har brug for, finder det gamle frem og copy-paster. Hvad er sandsynligheden for at det er veldokumenteret, testet og vedligeholdt? Hvad er sandsynligheden for at resten af teamet er enige i måden beregningen er lavet - givet at de overhodet kan gennemskue nuancerne i det? Hvad er sandsynligheden for at det er plug-n-play?

I BI verdenen har man længe vidst at det er spild af tid at lave rapporter fra scratch hver gang. De metrics som går igen bliver beregnet daglig og persisteret i tabeller, så en rapportudvikler har et "bibliotek" af metrics de kan trække på. Det sikrer at begreber bruges ens på tværs af rapportering og bygger tillid til rapporterne, da man kan genkende tallene på tværs af rapporter. Den klassiske "en sandhed".

I ML verdenen er vi begyndt at indse at det er ret smart at have et "bibliotek" af input features til vores modeller. At have tilgang til 200+ features der bliver genereret hver dag, der er gennemtestet, monitoreret og dokumenteret er en kæmpe tidsbesparing for datascientists. Hvis man yderligere nemt kan generere nye features der bliver produktionssat på samme måde, så andre kan genbruge ens features, så har man en force-multiplier der vil noget.

## Features i produktion

Når modeller udvikles, så er dataen 80% af arbejdet - men hvordan sikrer man sig at den data der skal bruges til modellen i produktion er korrekt? Det er sjældent at brugeren af APIen kan sende al den data der er brug for til prædiktionen - ideelt så sender man APIen et kundenr som input og den henter selv den data der er brug for. Det simplificerer også API kontrakten - Input er et kundenr, alt bagved kan laves om uden at informere nogen.

Udfordringen er at sikre sig mod Train/Serve Skew - hvordan sikrer man at data er transformeret på samme måde i prædiktionsøjeblikket som når modellen blev trænet? Kodes logikken to gange? Hvis ja, hvordan opdateres det? Hvad med latency krav? Der er andre latency krav til produktionsapier end der er til modeltræning, så det skal der også tages højde for.

Featurestores løser Train/Serve Skew problemet ved at isolere featurelogikken som en separat service der kan genbruges til træning og prædiktion. Ved at isolere det komponent, så kan dataopslaget optimeres efter usecase, hvor prædiktion bruger en lav latency key-value store mens træning kan bruge mere beregningstunge backends.  

## Featurestore komponenterne

En featurestore består typisk af følgende

- Et *Compute* lag, hvor feature transformationerne sker
- Et *Offline* storage lag hvor man gemmer de historiske data til model træning
- Et *Online* storage lag hvor man gemmer de mest up-to-date data til prædiktioner
- Et *Monitoreringslag* hvor man monitorerer data for data-drift
- Et *GUI* lag til discovery og dokumentation
- Et *Client* lag som datascientisten skal bruge i sin kode til at snakke med featurestoren

### Compute

Feature transformationer skal afvikles et sted - ideelt et compute lag som datascientists kan tilgå direkte for at undgå omskrivning af logik. I dette lag bruges et pipeline/orchestrating værktøj, som Airflow eller Luigi for at koordinere beregningen af de forskellige features. Ideelt, så er det datascientisterne selv der skriver de forskellige transformationer, så man undgår "smid-over-muren" problemer ved at have forskellige afdelinger til det.

### Offline Storage

Typisk en Datalake/S3/SQL database. Offline storage er hvor man opbevarer den historiske data der skal bruges til træning af modeller. Kriterierne her at det skal være søgbart for en given tidsperiode - typisk via snapshotting. I machine learning modeller vil man gerne kunne se verden som den så ud på et givent tidspunkt, så man undgår datalækage til target.

### Online Storage

Det data der skal bruges til træning og prædiktion har forskellige krav. Det nyeste udseende af data er det man gerne vil prædiktere på. Ofte udstilles modeller som APIer, så latency krav er markant højere end i offline storage. Når der prædikteres, så er historik ikke vigtig, og man kan bruge Key-Value stores som Redis eller Memcache for at lave lookups på f.eks kundenr meget hurtigt.

### Monitorering

Machine Learning projekter vil altid være en større udfordring end andre software projekter af den simple årsag at ML kan fejle fordi verden ændrer sig (fail quietly). Når man monitorer ML APIer og ETL, så skal man ikke kun holde øje med datafejl, man skal også holde øje med om verden har ændret sig. Airbnb's udleje data vil f.eks se meget anderledes ud under Corona end før. Tilsvarende, hvis marketing kører en kampagne på Audi biler, så vil bestanden have flere Audi biler end før.

ML modeller er trænet på historik, så hvis verden nu er anderledes end historikken tilsiger, så vil modellen komme med forkerte prædiktioner, *selvom koden er korrekt*. Det er et andet paradigme end traditionel software, så featurestores skal både kunne monitorere om verden nu er anderledes og samtidig kunne opdage fejl i grunddatata. I praksis betyder det at man kan se udvikling over tid på parametre som gennemsnit, og kunne sende advarsler hvis gennemsnit ændrer sig +- 1-2 standard afvigelser.

### GUI

Discovery er et meget vigtig koncept i en featurestore - hvis det nye ML projekt skal genbruge features fra tidligere projekter, så sker det ikke uden discoverability. Data Catalogues er en hel artikel for sig, men konceptet er det samme - Datascientisten skal kunne gå ind på en hjemmeside/GUI og få et overblik over hvilke features er tilgængelige, kunne læse dokumentation på dem, og kigge på nogle visualiseringer af det. Hvis de ikke kan det, så mister man hurtigt genbrugsfaktoren og tilliden til kvaliteten af features.

### Client

I sidste ende skal datascientisten nemt kunne hente en feature fra featurestoren til at bruge i sin model. Hvis det ikke er en nem, sømløs process, så kommer det ikke til at se. Datascientisten skal ikke bekymre sig om hvilke joins der skal til, hvor data ligger henne fysisk eller hvordan det beregnes. Client laget skal derfor oversætte et ønske om at få feature X, feature Y og feature Z for et givet tidspunkt til en forespørgsel, der inkluderer Authentication, query parametre og alt det andet der skal til, for at hente data fra forskellige backends, lave de korrekte joins og servere det til datascientisten i et format de kan arbejde videre med - typisk en DataFrame af en art.

## Do-it-yourself eller køb en platform?

Hvert komponent der er beskrevet her er ikke nødvendigvist kompleks, afhængig af hvilke krav man har til data. Hvert komponent er også unik til den specifikke dataplatform man arbejder på og de krav man har til frekvens, latency osv. Det er hovedårsagen til at man ser at mange virksomheder vælger at lave deres egen løsning tilpasset deres behov, fremfor at købe en samlet platform. Løsningen er typisk styret af andre infrastrukturvalg, som hvilken cloud man er i, hvilke teknologier man er komfortabel med og diverse andre krav man har til sin løsning.

Uanset hvilken tilgang man vælger, hvis du vil give dit team en tidlig julegave, begynd at investere i en featurestore!
