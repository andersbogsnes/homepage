---
title: "Det sværeste problem i machine learning"
date: 2020-07-30T17:43:06+02:00
slug: "deployment"
description: "Hvordan løser man det sværeste problem i Machine Learning?"
keywords: []
toc: true
tags: []
---

En af de sværeste problemer indenfor machine learning er ikke hvordan man træner den nyeste "state of the art" Deep Learning model eller hvordan man tuner sin gamle churn model - det er hvordan man tager noget der lever på en laptop og laver et produkt ud af det.

Jeg kommer til at gennemgå den "typiske" deployment og sammenligne med den løsning vi arbejder med i Alm Brand.

## "Over muren" deployment modellen

Den "typiske" workflow starter med at dine datascientists finder et problem de synes er spændende. De har fundet noget nyt data, bruger en måned på at tune, træne og teste og ender op med en model de er tilfreds med. Så bliver den smidt "over muren" til IT der får opgaven med at produktionssætte modellen.
Denne fremgangsmåde har et par problemer:

### Problem 1: Sprogbarrieren

IT arbejder typisk ikke i samme sprog som dine datascientists - det er typisk Java, C++ eller lignenede "enterprise" sprog, mens machine learning typisk sker i Python eller R. Så for at kunne produktionssætte modellen, så skal modellen kodes om til noget der passer ind i ITs deployment processer, vel og mærke uden at ændre i hvordan modellen fungerer.

### Problem 2: Produktionsklar kode

Mange datascientists kommer fra en akademikerbaggrund - statistikere, forskere, geologer osv. De har ikke nødvendigvis en software engineering baggrund og fokus er mere på optimere modellen end at gøre koden produktionsklar.

Best practice ifht funktioner, DRY, SOLID, unittesting, CI og andre flotte forkortelser som software engineers tager som en selvfølge er ikke nødvendigvis fulgt. Før modellen kan sættes i produktion, så er man nødt til at implementere disse best practice, ellers risikerer man at ende op med en kodebase ingen kan navigere rundt i.

### Problem 3: Udviklingshastighed

Datascience er en iterativ process og man kan hele tiden finde små forbedringer til modellerne. IT investerer en masse ressourcer i at omkode modellen i Java, implementeret unittests og ellers gjort koden produktionsklar.Måneden efter kommer datascience teamet tilbage og siger at de nu har forbedret modellen med 0.5 ROCAUC og kan vi ikke implementere denne nye model? Det bør være en positiv ting at man forbedrer sine modeller løbende, men det bliver hurtig en negativ oplevelse hvor man skal hele tiden afveje om forbedringen er værd omkostningen.

## Alm Brands deployment model

Vi har valgt at angribe denne problemstilling fra nogle forskellige vinkler:

### Løsning 1: Hele stacken i Python

En af fordelene ved at vi koder modeller i Python er at vi har tilgang til mange "produktionsressourser". Det betyder at vi kan kode hele stacken i Python, fra model til API. Ved at bruge Docker, er det nemt at levere en færdig indpakket model til produktion, hvor datascience teamet kan styre hele processsen fra start til slut - IT skal bare starte en ny container, de behøver ikke at bekymre sig om hvad der er indeni. Datascience teamet tager nu ejerskab for modellen og dens udvikling mens IT tager ejerskab for drifting af server og lignende.

### Løsning 2: Byg vores eget ML framework

For at drive best practice, så har vi bygget vores eget framework der standardiserer best practice for teamet.

Vores framework er bygget ovenpå scikit-learn, så det er nemt og kendt materiale for datascientists og indeholder mange af de læringer vi har gjort indenfor machine learning.

Når vi tester f.eks en ny metode for feature selection, så testes det på "Data science måden" - eksperimenter for at afgøre om det skaber en reel forbedring i modelperformance. Når vi er enige i at det er den korrekte måde at lave feature selection på, så opdaterer vi frameworket til at indeholde denne nye funktionalitet. Nu er vi i "produktionskodemodus" hvor vi sørger for at skrive en omfattende testsuite, vi skriver dokumentation, vi kigger på optimeringer som parallelisering og det arbejdes ind i et sammehængende API, så det er genkendeligt og kompatibelt med resten af vores kodebase.

Den sidste viktige komponent er at det er datascience teamet selv der skriver og vedligeholder vores framework. Det er slutbrugeren der bestemmer hvordan skal APIet se ud, hvilke features der skal prioriteres og hvilke bugs der bør løses først. Det giver også en forståelse for software engineering verden, så datascientist teamet får et bedre fundament indenfor kodning og hvordan det kan gøres bedre.

På denne måde får vi det bedste af begger verdener - den hurtige eksperimentering, men også den bundsolide implementering.

### Løsning 3: Standardisering

Ved at bruge vores eget framework får vi også den kæmpe fordel at vi standardiserer API til modellerne. Det gør at når vi skal produktionssætte vores modeller, så starter vi ikke forfra i at definere hvordan modellen skal tilgå data eller hvordan REST apiet skal spørge om en prædiktion - det er det samme hver gang. Det gør at vi kan standardisere resten af vores pipeline og give kontrollen end-to-end til datascience teamet. Så når teamet næste måned laver en bedre model, så behøver vi ikke balancere gevinsten af den bedre model mod kosten af implementation - vi pusher bare den nye model til master og CI serveren bygger en ny model og sætter den i produktion!

# Konklusion

For at undgå "Over Muren" deployment modellen anbefaler vi følgende:

## Inddrag datascience teamet i at bygge sine egne værktøj

Ved at inddrage datascience teamet i at bygge sine egne værktøjer og bruge ITs ekspertise til at standardisere og drifte bliver modelprocessen bliver smidigere og vi kan agere hurtigere. Best practice for modellering bliver standardiseret og testet ved at samles i frameworket, så man ikke genimplementerer den dybe tallerken på fem forskellige måder.

## Byg end-to-end i et sprog datascience teamet forstår

Datascience teamet skal ikke være software engineers, men en forståelse for best practice og end-to-end processen vil gøre produktionssætning meget smidigere og modellerne mere robuste.

## Standardiser løsninger

Jo mere der kan standardiseres i deployment processen, jo hurtigere kan man iterere og prøve ting af - det bedste eksperiment gøres live!
