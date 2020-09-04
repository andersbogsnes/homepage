---
title: "Featurestores"
date: 2020-09-03T10:42:36+02:00
slug: ""
description: "Featurestore - Datascientistens datakatalog"
keywords: []
draft: true
tags: []
math: false
toc: true
---

# Featurestores - Skaler din data til datascience

Featurestoren er et nyt værktøj i værktøjskassen til datascience teams. Uber skrev om sin Featurestore [Michelangelo](https://eng.uber.com/michelangelo-machine-learning-platform/) tilbage i 2017, det samme år som Airbnb begyndte at skrive om sin Featurestore [Zipline](https://medium.com/airbnb-engineering/using-machine-learning-to-predict-value-of-homes-on-airbnb-9272d3d4739d). Der er nu en lang række virksomheder der har indset at
Featurestores er en central del af deres ML platform - Facebooks [FBLearner](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/), Netflix [Metaflow](https://metaflow.org/), Google's [Feast](https://cloud.google.com/blog/products/ai-machine-learning/introducing-feast-an-open-source-feature-store-for-machine-learning) og mange andre. Der er også platforme der leverer *Featurestore-as-a-service* som [Hopsworks](https://www.logicalclocks.com/) og [Tecton](https://www.tecton.ai/) men trenden er helt klart at bygge sit eget.

## Hvad er en Featurestore?

En featurestore er, kort fortalt, et centralt sted hvor datascientists kan tilgå prædefinerede features til brug i deres modeller.
Features der ligger i featurestoren er typisk gennemtestet, monitoreret og bliver genereret automatisk med et eller andet interval

En typisk workflow for en datascientist er at starte fra rå data, enten et SQL udtræk eller en datadump fil. Når man har fået sit udtræk begynder man at danne sig et overblik over hvilke data der kan bruges, og hvordan det kan transformeres. Fødselsdatoer bliver konverteret til aldre, en større beregning for at finde hvornår kunden sidste gang købte noget og man renser lige nogle kolonner der ikke så helt riktige ud. Populært sagt, så er at arbejdet med data 80% af Machine Learning.

Hvad sker der så til næste projekt? Best case, så husker det nye projekt at nogen andre har noget kode der lignede det man ville, og går tilbage og copy-paster. Hvad er sandsynligheden for at det er veldokumenteret, testet og vedligeholdt? Hvad er sandsynligheden for at resten af teamet er enige i måden beregningen er lavet - givet at de kan gennemskue nuancerne i det? Hvad er sandsynligheden for at det er plug-n-play?

I BI verdenen har man vidst længe at det er spild af tid at lave rapporter fra scratch hver gang. De metrics som går igen bliver beregnet daglig og persisteret i tabeller, så en rapport udvikler har et "bibliotek" af metrics de kan trække på. Det sikrer at begreber bruges ens på tværs af rapportering og bygger tillid til rapporterne, da man kan genkende tallene på tværs af rapporter. Den klassiske "en sandhed".

I Machinelearning verdenen er vi begyndt at indse at det er ret smart at have et "bibliotek" af input features til vores modeller. At have tilgang til 200+ features der bliver genereret hver dag, gennemtestet, monitoreret og dokumenteret er en kæmpe tidsbesparing for datascientists. Hvis man yderligere kan nemt generere nye features der bliver produktionssat på samme måde, så andre kan genbruge ens features, så har vi en force-multiplier der vil noget.

## Features i produktion

Når vi skal sætte vores modeller i produktion, tænker man meget over hvordan vi skal få selve modellen med ind - vi laver Model Registries hvor vi gemmer vores modelartifakter og så kan vi loade dem i vores APIer og hvor de nu ellers skal bruges. Men som allerede nævnt, så er dataen 80% af arbejdet - hvordan sikrer vi os at den data vi giver til modellen i produktion er korrekt? Det er sjældent at vi kan bede brugeren af vores API til at sende al den data vi har brug for til vores prædiktion - vi vil gerne kunne tage som input et kundenr og hente den data vi har brug. Det simplificerer også vores API kontrakt - vi skal bruge et kundenr, så kan vi lave om i alt det vi vil bagved APIet.

Udfordringen er at sikre sig mod Train/Test Skew - er data transformeret på samme måde i prædiktionsøjeblikket som det var når vi trænede modellen? Koder vi logikken to gange? Hvordan opdateres det? Der er også andre latency krav til vores produktionsapier end der er til vores modeltræning, så det skal vi også tage højde for. Featurestores løser Train/Test Skew problemet ved at isolere feature logikken som en separat service som kan genbruges til træning og prædiktion. Ved at isolere det komponent, så kan man også optimere dataopslaget, så prædiktion kan bruge en lav latency key-value store mens træning kan bruge mere beregningstunge backends.  

## Featurestore komponenterne

En featurestore består typisk af følgende

- Et Compute lag, hvor feature transformationerne sker
- Et "offline" storage lag hvor vi gemmer de historiske data til model træning
- Et "online" storage lag hvor vi gemmer de mest up-to-date data til prædiktioner
- Et monitoreringslag hvor vi monitorerer data for data-drift
- Et GUI lag til discovery og dokumentation
- Et client lag som datascientisten skal bruge


### Compute lag

Vi skal have et sted at afvikle vores feature transformationer - ideelt et compute lag som datascientists kan tilgå direkte for at undgå omskrivning af logik. I dette lag har vi brug for et pipeline/orchestrating værktøj, som Airflow eller Luigi for at koordinere beregningen af de forskellige features

### Offline Storage

Typisk en datalake/S3/SQL database. Offline storage er hvor man opbevarer den historiske data der skal bruges til træning af modeller. Kriterierne her at det skal være søgbart for en tidsperiode - typisk via snapshotting. I machine learning modeller vil man gerne kunne se verden som den så ud på et givent tidspunkt, så vi kan undgår leakage til target.

## Online Storage

Når man laver sit ETL i Compute laget, så differentierer man på det nyeste udseende af data og det historiske data. Det nyeste udseende af data er det man gerne vil prediktere på. Ofte udstilles modeller som APIer, så latency krav er markant højere end i offline storage. HFordi historik ikke er vigtig, kan man bruge Key-Value stores som Redis eller Memcache for at lave lookups på f.eks kundenr meget hurtigt.

## Monitorering

Machine Learning vil altid være en større udfordring end andre software projekter af den simple årsag at ML kan fejle fordi verden ændrer sig. Når man monitorer ML APIer og ETL, så skal man ikke kun holde øje med datafejl, man skal også monitorere at verden har ændret sig. ML modeller er trænet på historik, så hvis verden nu er anderledes end historikken tilsiger, så vil modellen komme med forkerte prædiktioner, *selvom koden er korrekt*. Det er et andet paradigme end Software Engineering, så featurestores skal også monitorere om verden nu er anderledes udover at den skal opdage fejl i grunddatata. I praksis betyder det at man skal kunne se udvikling over tid på parametre som gennemsnit, og kunne sende advarsler hvis gennemsnit nu er anderledes +- 1-2 standard afvigelser. Det skulle gerne opdaget at man har kørt en marketingkampagne på Audi biler, så nu er vores bestandssammensætning væsentlig anderledes end modellerne forventer.

## GUI lag

Discovery er et meget vigtig koncept i en featurestore - hvis det nye ML projekt skal genbruge features fra tidligere projekter, så sker det ikke uden discoverability. Data Catalogues er en hel artikel for sig, men konceptet er det samme - Datascientisten skal kunne gå ind på en hjemmeside og få en oversigt over hvilke features er tilgængelige, kunne læse dokumentation på dem, og kigge på nogle visualiseringer af det. Hvis de ikke kan det, så mister man hurtigt genbrugsfaktoren og tilliden til kvaliteten af features.

## Client laget

I sidste ende skal datascientisten nemt kunne hente en feature fra featurestoren til at bruge i sin model. Hvis det ikke er en nem, sømløs process, så sker det ikke. Datascientisten skal ikke bekymre sig om hvilke joins der skal til, hvor data ligger henne fysisk eller hvordan det beregnes. Client laget skal derfor oversætte et ønske om at få feature X, feature Y og feature Z for et givet tidspunkt til en forespørgsel, der inkluderer Authentication, query parametre og alt det andet der skal til, for at hente data fra forskellige backends, lave de korrekte joins og serverer det til datascientisten i et format de kan arbejde videre med - typisk en DataFrame af en art.

## DYI eller platform

Hvert komponent der er beskrevet her er ikke nødvendigvist kompleks, afhængig af hvilke krav man har til data. Hvert komponent er også unik til den specifikke dataplatform man arbejder på og de krav man har til frekvens, latency osv. Det er også derfor man ser at virksomheder vælger at lave deres egen løsning for at passe til deres behov, fremfor at købe en samlet platform. Det er typisk styret af andre infrastrukturvalg, som hvilken cloud man er i, hvilke teknologier man er komfortabel med og hvilke krav man har til sin løsning.

