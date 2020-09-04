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

Featurestoren er et nyt værktøj i værktøjskassen til datascience teams. Uber skrev om sin Featurestore ["Michelangelo"](https://eng.uber.com/michelangelo-machine-learning-platform/) tilbage i 2017, det samme år som Airbnb begyndte at skrive om sin Featurestore ["Zipline"](https://medium.com/airbnb-engineering/using-machine-learning-to-predict-value-of-homes-on-airbnb-9272d3d4739d). Der er nu en hel masse virksomheder der har indset at
Featurestores er en central del af deres ML platform - Facebooks ["FBLearner"](https://engineering.fb.com/core-data/introducing-fblearner-flow-facebook-s-ai-backbone/), Netflix ["Metaflow"](https://metaflow.org/), Google's ["Feast"](https://cloud.google.com/blog/products/ai-machine-learning/introducing-feast-an-open-source-feature-store-for-machine-learning) og mange andre. Der er også platforme der leverer Featurestore-as-a-service som [Hopsworks](https://www.logicalclocks.com/) og [Tecton](https://www.tecton.ai/) men trenden er helt klart at bygge sit eget.

## Hvad er en Featurestore?

En featurestore er, kort fortalt, et centralt sted hvor datascientists kan tilgå prædefinerede features til brug i deres modeller.
Features der ligger i featurestoren er typisk gennemtestet, monitoreret og bliver genereret automatisk med et eller andet interval

En typisk workflow for en datascientist er at starte fra rå data, enten et SQL udtræk eller nogle filer. Når man har fået sit udtræk begynder man at danne sig et overblik over hvilke data der kan bruges, og hvordan det kan tranformeres. Fødselsdatoer bliver konverteret til aldre, en større beregning for at finde hvornår kunden sidste gang købte noget og man renser lige nogle kolonner der ikke så helt riktige ud. Populært sagt, så er at arbejdet med data 80% af Machine Learning.

Hvad sker der så til næste projekt? Best case, så husker det nye projekt at nogen andre har noget kode der lignede det man ville, og går tilbage og copy-paster. Hvad er sandsynligheden for at det er veldokumenteret, testet og vedligeholdt? Hvad er sandsynligheden for at resten af teamet er enige i måden beregningen er lavet - givet at de kan gennemskue nuancerne i det? Hvad er sandsynligheden for at det er plug-n-play?

I BI verdenen har man vidst længe at det er spild af tid at lave rapporter fra scratch hver gang. De metrics som går igen bliver beregnet daglig og persisteret i tabeller, så en rapport udvikler har et "bibliotek" af metrics de kan trække på. Det sikrer at begreber bruges ens på tværs af rapportering og bygger tillid til rapporterne, da man kan genkende tallene på tværs af rapporter. Den klassiske "en sandhed".

I Machinelearning verdenen er vi begyndt at indse at det er ret smart at have et "bibliotek" af input features til vores modeller. At have tilgang til 200+ features der bliver genereret hver dag, gennemtestet, monitoreret og dokumenteret er en kæmpe tidsbesparing for datascientists. Hvis man yderligere kan nemt generere nye features der bliver produktionssat på samme måde, så andre kan genbruge ens features, så har vi en force-multiplier der vil noget.

## Features i produktion

Når vi skal sætte vores modeller i produktion, tænker man meget over hvordan vi skal få selve modellen med ind - vi laver Model Registries hvor vi gemmer vores modelartifakter og så kan vi loade dem i vores APIer og hvor de nu ellers skal bruges. Men som allerede nævnt, så er dataen 80% af arbejdet - hvordan sikrer vi os at den data vi giver til modellen i produktion er korrekt? 

## Featurestore komponenterne

En featurestore består typisk af følgende

- Et Compute lag, hvor feature transformationerne sker
- Et "offline" storage lag hvor vi gemmer de historiske data til model træning
- Et "online" storage lag hvor vi gemmer de mest up-to-date data til prædiktioner
- Et overvågningslag hvor vi monitorerer data for data-drift
- Et GUI lag til discovery og dokumentation

