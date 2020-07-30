---
title: "Data Science Kode != Produktionskode"
date: 2019-01-22T17:41:42+02:00
slug: "framework"
description: "Data science kode er ofte ikke klar for produktion"
keywords: []
toc: true
tags: []
---

# Datascience kode != Produktionskode

Det er en kendt problemstilling for mange der arbejder med datascience - der ligger utallige flotte modeller på laptops rundt om i det vide land der aldrig kom over den sidste og viktigste skridt - produktionssætning.

Men hvorfor er et så svært at få ting i produktion?
En af hovedårsagene er at datascience processen ikke typisk resulterer i produktionsklar kode!

Der er et par forskellige årsage til hvorfor den typiske datascientist har svært ved at skrive produktionskode.

### Problem 1: Python/R != Java/C++
IT er et godt sted at starte for at lære best practice indenfor software engineering. Udfordringen er at IT arbejder typisk ikke i samme sprog som dine datascientists - det er typisk Java, C++ eller lignenede "enterprise" sprog, mens machine learning typisk sker i Python eller R. Man kan selvfølgelig lære best practice på tværs af sprog, men det er sin egen disciplin at skrive veloptimeret numerisk kode i f.eks Python.

### Problem 2: Datascience != Software Engineering
Mange datascientists kommer fra en akademikerbaggrund - statistikere, forskere, geologer osv. De har ikke nødvendigvis en software engineering baggrund og fokus er mere på optimere modellen end at gøre koden produktionsklar.

Best practice ifht funktioner, DRY, SOLID, unittesting, CI og andre flotte forkortelser som software engineers tager som en selvfølge er ikke nødvendigvis fulgt. Man mister fordelerne ved at kunne refactore sin kode fordi man har et solidt unittest fundament, eller muligheden for at arbejde i større teams fordi man har styr på version kontrol og CI.

### Problem 3: Datascientist != Datascience Team
Mange datascientist er ikke nødvendigvis vandt til at arbejde i teams og er derfor heller ikke vandt til at arbejde med kode genbrug og at bygge delte kodebiblioteker. Der er meget kode jeg har set der genopfinder den dype tallerken. Kode for at plotte ROC kurver eller gemme resultater fra en gridsearch bliver genskrevet om og om igen, i lidt forskellige varianter. Det kan skade udviklingshastigheden og kodekvalitet hvis man "opfinder" sine egne kodesnippets hver gang man starter et nyt projekt

## DIY - Byg dit eget framework
Vi har valgt at løse disse 3 problemer ved at bygge vores eget ML framework. Det har flere fordele både for datascience teamet, men også for produktionssætning af modeller.

###  Hele stacken i Python
Vores framework standardiserer hele ML stacken i Python, så modellerne kan deployes så nemt som at pip installere fra vores interne PyPi repository. Frameworket standardiserer interface, så alle modeller trænes, gemmes og prædikterer på samme måde. Det gør det meget nemt for os at bygge en REST API rundt en given model og sætte det i produktionspipelinen via Docker.

### Incentiver produktionsklar kode
Ved at bygge vores eget framework så incentiveres datascience teamet til at gøre sin kode genbrugbar, testet og klar til produktion.

Når vi tester f.eks en ny metode for feature selection, så testes det på "Data science måden" - eksperimenter for at afgøre om det skaber en reel forbedring i modelperformance. 

Når vi er enige i at det er den korrekte måde at lave feature selection på, så opdaterer vi frameworket til at indeholde denne nye funktionalitet. Nu er vi i "produktionskodemodus" hvor vi sørger for at skrive en omfattende testsuite, vi skriver dokumentation, vi kigger på optimeringer som parallelisering og det arbejdes ind i et sammenhængende API, så det er genkendeligt og kompatibelt med resten af vores kodebase. 

### Incentiver teamwork
Det er datascience teamet selv der skriver og vedligeholder vores framework. Det betyder at det er slutbrugeren der bestemmer hvordan skal APIet se ud, hvilke features der skal prioriteres og hvilke bugs der bør løses først. 

Det giver også en forståelse for software engineering verden, så datascientist teamet får et bedre fundament indenfor kodning og hvordan det kan gøres bedre. De får viden om hvordan man koder i et større projekt sammen med andre, hvordan man bruger git og CI, hvordan man laver code reviews og mange andre værktøjer til at arbejde bedre sammen i et team.

# En stor investering
Det lyder som en stor investering at sætte tid af til at kode sit eget bibliotek - der skal investeres tid til at lære alle disse nye værktøj og jeg ville anbefale at inddrage IT til at hjælpe med best practice.

Det er dog en investering der giver hurtig afkast. Første gang nogen på teamet bare kan kalde "plot_roc_curve" funktionen uden at skulle tage stilling til axer, farver og titler, eller når "train" funktionen også automatisk skriver resultatet i en logfil, så mærker man værdien.

Ved at opfordre til genbrug af kode i et testet framework får vi det bedste af begger verdener - den hurtige eksperimentering, men også den bundsolide implementering.