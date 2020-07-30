---
title: "Tester Du?"
date: 2019-12-11T17:34:12+02:00
slug: "ml_testing"
description: "Are you testing your ML code?"
keywords: []
draft: true
tags: []
---

# Tester du?

Det var en gang når datascientists mødtes, at snakken gik på algoritmer - hvad var nyt og hot? I disse dage går snakken mere på software udfordringer. Hvordan får vi denne model i produktion? Hvordan sikrer vi datakvalitet? Hvordan overvåger vi modellerne? 

Algoritmerne er stadig en viktig del af hverdagen, men der er begyndt at demre at den bedste model er den der er i produktion!

# Datascience er nu en teamsport
Datascience er blevet mere modent - mere produktionssætning og mindre POC. Der er flere datascience teams fremfor ensomme ulve, noget der sætter krav til at man kan arbejde sammen i en kodebase. Det kan man ikke hvis kodebasen ikke er læsbar, testet og produktionsklar!

Uden automatiserede tests, er det svært at ændre i en delt kodebase - det er som at spille Klodsmajor, men man må ikke se tårnet når man trækker brikkerne ud.

Til trods for at alle er enige om at det er viktigt, så sker det alt for tit at datascience kode ender op med at ikke have tests.

For at komme igang, så vil jeg foreslå 4 forskellige teststrategier:
- Pipeline testing
- Runtime testing
- Property-based testing
- Example-based testing

# Pipeline testing
[Som jeg har skrevet om tidligere](https://pro.ing.dk/datatech/artikel/machine-learning-isbjerget-de-resterende-90-3861), så handler machine learning om meget mere end bare modellen. Hver eneste del af din pipeline op til modellen bør have unit-tests. 

Din transformationsfunktion der konverter fødselsdato til alder - den skal have en test. Din funktion der finder ud af hvornår en kunde sidst har købt baseret på solgte produkter - den skal have en test! 

Disse er typisk nemmere at teste - giv din funktion et input hvor du kender svaret, og tjek at resultatet af din funktion matcher det kendte svar. 

Jeg vil anbefale [pytest](https://docs.pytest.org/en/latest/) som test framework. Det er ved at være defacto standard for testing i Python efterhånden, og er nemt at komme igang med.

# Runtime testing
En machine learning pipeline handler typisk om at behandle meget data over tid - har I tests der tjekker om den nye data har ændret sig? 

Ville I opdage hvis der mangler en kolonne, en kolonne har skiftet datatype eller der mangler en dato? Hvad med dataens karakteristika? Har gennemsnittet ændret sig markant siden sidst load? Max og min værdier? Har nye kategorier sneget sig ind? Det er meget bedre at fange disse typer ændringer og fejl før man når hen til at træne modellen! 

Jeg vil anbefale [bulwark](https://bulwark.readthedocs.io/en/latest/index.html), [great expectations](https://docs.greatexpectations.io/en/latest/index.html) eller [pandera](https://pandera.readthedocs.io/en/stable/) afhængig af størrelsen af projektet - de er alle biblioteker lavet til validering af datapipelines, hvor `bulwark` og `pandera` er mere lightweight end `great expectations`, men har til gengæld ikke ligeså mange features. Kig på dem alle og vurder hvad der giver mening for jeres projekt!

# Property-based testing
Når man arbejder med kode hvor det ikke er nemt at vide nøjaktig hvad det forventede output burde være, så kan vi bruge property-based testing istedenfor "traditionel" testing (eller sammen med!). 

Property-based testing betyder at man definerer hvordan input data kan se ud - eks. så definerer vi input til modellen som et array med 6 kolonner, 5 tal og en string. 

Vi erklærer så hvilke "properties" dit output skal have efter at den har været gennem din funktion. Eksempelvis kunne din pipeline indeholde en skaleringsfunktion der skalerer alle numeriske værdier til mellem 0 og 1 og en One-Hot Encoding til strings. Så dit output vil have følgende "properties":

- output skal ikke indeholde strings
- output skal have flere værdier end input (pga One Hot encodingen)
- output skal ikke indeholde tal større end 1 og mindre en 0
- output skal kun indeholde floats


Et bibliotek som [hypothesis](https://hypothesis.readthedocs.io/en/latest/) definerer mange strategier for at erklære disse "properties"
og prøver at finde kombinationer af input værdier der gør at output ikke længer overholder vores definerede "properties". 

Den vil prøve sig frem med alle mulige mærkelige værdier, meget store tal, negative tal, unicode symboler osv, og når den finder en fejl, så vil [hypothesis](https://hypothesis.readthedocs.io/en/latest/) finde den simpleste version af inputs der skaber fejlen. Det er som at skrive 100 tests i et hug! 

Derved får man både en sikkerhed i at man har tænkt på hjørnetilfælde, men man slipper også for at "opfinde" testinput data der ligner reel data - man skal bare beskrive hvordan det ser ud og `hypothesis` går bare i gang.

# Example-based testing
Vi har nu fået en god testdækningsgrad på vores data pipeline, men hvad med modellen? Det er sværere at bruge traditionelle unit-testing metoder da modeller ofte ikke er deterministiske - der er randomiserede hændelser i træningen af en model.

For at teste en given model, kan man sammenligne resultater med kendte eksempler - når man forstår sin model, så har man også en ide om hvilke features der er udslagsgivende. 

Givet at vores model skal forudsige huspriser, så kunne en god test være at hvis input er et hus på 200 kvm på Frederiksberg, så skal output være over 5 millioner. 

Fra vores domæneviden, ved vi at det altid er tilfældet, så hvis vores model ikke kan fange den "nemme", så er der nok noget galt! Man kunne også have en test på at et hus i Frederiksberg er dyrere end hvis vi tager det samme hus, men ændrer kommune kolonnen til Lolland. Jeres domæneeksperter kan meget hurtigt finde nogle eksempler der er sund fornuft, som I kan inkorporere i din test-suite.

Man kan også inkludere tests på om modellens metrikker er bedre end en fast baseline - det kan være et af acceptkravene er at modellen skal være bedre end 50/50, eller skal den performe bedre end et menneske - eller måske bare bedre end den forrige model?


# Den første gør mest ondt
Vi har nu gennemgået 4 forskellige måder at teste datascience kode på. Det svære er at bygge det ind som en fast rutine i hverdagen og tvinge sig selv til at få skrevet de første par tests. 

Som med så meget andet godt i livet, så gør det lidt ondt i starten men når man ender op med en solid, produktionsklar kodebase man kan stole på, er det det hele værd!