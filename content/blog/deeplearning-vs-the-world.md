---
title: "Når Deep Learning ikke er svaret"
date: 2019-04-09T17:55:21+02:00
slug: "deeplearning-vs-the-world"
description: "Når Deep learning ikke er svaret"
keywords: []
tags: []
toc: true
---

Man kan ikke bevæge sig langt ude på Internettet uden at falde over artikler med der snakker om hvordan [Deep Learning diagnosticerer kræftpatienter](https://ai.googleblog.com/2018/10/applying-deep-learning-to-metastatic.html) eller [Deep Learning der skriver nyhedsartikler](https://www.forbes.com/sites/nicolemartin1/2019/02/08/did-a-robot-write-this-how-ai-is-impacting-journalism/#6d38d5297795). Det lyder besnærende at det eneste du behøver at gøre for at løse alle dine problemer er at smide al din data ind i en Deep Learning model, men som med alt andet i Machine Learning, så skal man være opmærksom på de tradeoffs der er.

## Det har aldrig været nemmere at lave Deep Learning

En af grundene til denne eksplosion af Deep Learning er at det har aldrig været nemmere at træne en Deep Learning model. Frameworks som [Pytorch](https://pytorch.org/) og [Tensorflow](https://www.tensorflow.org/) klarer det tunge arbejde med differentialer og GPU programmering, og abstraktionsframeworks som [Keras](
https://keras.io/) og [fastai](https://docs.fast.ai/) giver dig tilgang til prækodede dele så du ikke engang behøver at kode lagene selv!

## Vi har været her før

Dette er ikke første gang vi ser noget lignende. Før frameworks som [Scikit-learn](https://scikit-learn.org/stable/) blev introduceret, skulle man både kunne kode en Machine Learning algoritme og håndtere Data Science delen. Det betød at man skulle være en Software Engineer der tænker optimiseringer til algoritmekoden, formulere tests, håndtere edge cases, designe en API og arkitektur og sørge for at det var nemt at vedligeholde.
Samtidig skulle man også være Data Scientist og tænke over anvendelsen af modellen - hvilke features vil vi bruge, hvordan skal de transformeres, tune hyperparametre, forstå strukturen i data, tage højde for class imbalance og alt det andet arbejde der er i at lave en god model.

## Frameworkenes Indtræden

[Scikit-learn](https://scikit-learn.org/stable/) leverede optimerede versioner af alle de mest brugte algoritmer. Pludselig behøvede man ikke at tænke over hele algoritme-implementationsdelen! Nu skulle man bare skrive `import RandomForestClassifier` og kunne istedenfor fokusere på Data Science delen - en kæmpe gevinst!

Bagsiden af medaljen var dog at pludselig blev det meget nemt at træne en model på noget data, uden at tænke over alt det andet der stadig hørte med til Data Science delen. Jeg tør ikke tænke over hvor mange der har blindt smidt sin Churn data ind i en Random Forest og stolt præsenteret til sin chef at modellen har en accuracy på 98%! (Hvis kun 2% af dine kunder churner, så vil modellen ramme riktig 98% af gangene ved at forudsige at ingen churner!)

Det vi skal lære fra denne omvæltning er at det viktigste i Machine Learning processen er at det handler om at træffe aktive valg. Alt er en tradeoff og modeller giver værdi når vi som Data Scientists træffer gode valg ved at forstå disse tradeoffs. Hvis man ikke forstår sine tradeoffs, så ender man op med at produktionsætte Churn modellen med accuracy 98%.

## Start Simple!

Så hvordan kan man undgå at bygge Deep Learning versionen af Churn modellen? Det største misbrug af Deep Learning sker tit i de projekter der hopper direkte til Deep Learning uden at have prøvet simplere modeller først.

### Start med at etablere baseline

Den første model vi skal huske at afprøve, er ingen model!
Hvis vi skal prediktere noget i dag, hvordan performer vi uden en model? Er baseline at vi gætter, 50/50?
Har vi nogle forretningserfaringer der gør at vi måske er oppe på 60/40? Hvis du ikke kender din baseline, så ved du ikke om din model faktisk er bedre end at ikke have en model overhodet!

Når du har etableret din baseline, så kan du begynde at få en ide om hvor god modellen skal være før den kan anvendes. Hvis baseline er et tilfældig gæt, så skaber modellen jo værdi ved at gå fra 50/50 til 55/45!

### Start med den mest simple model

Når du ved hvad du skal løbe efter, start med den mest simple model. Hvis en Linear Regression kan løfte din baseline med 5-10%, så tag dem først! En simpel model har også mange andre gode egenskaber.

En Linear Regression kan:

- implementeres i SQL direkte, så du behøver ikke en kæmpe DevOps pipeline
- er nem at forstå, så du kan nemt dokumentere hvad der sker i hver prediction
- giver dig de viktigste features, så du kan videregive insights om data
- behøver ikke en supercomputer for at trænes - no GPU required!

Kom i produktion, høst dine gevinster og arbejd videre på baggrund af dine learnings. Udover at du har skabt værdi hurtigere, Så har du også lært om alt der kan gå galt i deployment processen, har noget mere træningsdata og har løftet din baseline.

Når det er sagt, så tror jeg mange vil blive overrasket hvor langt man kan komme med en Linear/Logististisk Regression + lidt kreativ feature engineering!

## Deep Learning har sin plads

Det skal ikke forstås sådan at Deep Learning ikke giver værdi eller aldrig bør bruges. Deep Learning er et kraftigt værktøj at have i sin værktøjskasse. Det er dog lidt som cykeludstyr - behøver man en karboncykel for at cykle til supermarkedet? Vent med at købe 20.000 kr cykelen til du finder ud af at du skal cykle Tour de France og start med at undersøge om klassiske Machine Learning modeller kan give værdi før du hopper direkte på at træne en 512-lags Inception model!
