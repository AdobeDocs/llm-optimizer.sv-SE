---
title: Varumärkesnärvaro
description: Lär dig använda kontrollpanelen för varumärkesnärvaro för att förstå hur ert varumärke uppfattas på nivån för AI-genererade svar.
source-git-commit: e8ea9ae0d6592ea3d1e9945ec117f852112ba9d7
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 0%

---


# Varumärkesnärvaro {#brand-presence}

Instrumentpanelen Varumärkesnärvaro ger en översikt över hur ert varumärke uppfattas på nivån för AI-genererade svar. Det visar var, hur ofta och i vilket sammanhang ert varumärke omnämns. Du kan använda kontrollpanelen för att mäta synlighet, spåra citat, jämföra konkurrenter och utforska attitydtrender. Instrumentpanelen är uppdelad i flera avsnitt, där alla innehåller olika insikter. Det finns också anpassningsbara filter som hjälper dig att förfina de data som visas.

![Varumärkesnärvaro](/help/dashboards/assets/brand-main.png)

Den här sidan innehåller följande information:

* [Filter](#filters)
* [Översikt, mått](##key-metrics)
* [Jämförelse av konkurrenter](##competitor-comparison)
* [Känslotrend](#sentiment-trend)
* [Datainsikter](#data-insights)

## Filter {#filters}

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

* **Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
* **Kategori** - Filtrera de resultat som visas efter fördefinierade kategorier. Du kan också lägga till egna kategorier i det här fältet (**SR**-how?).
* **Plattform** - Välj vilken AI-motor som ska analyseras.
* **Region** - Filtrera resultaten efter geografi. Alla regioner är inte tillgängliga vid lanseringen.

När du har valt önskat filter klickar du på **Använd filter** för att använda markeringen på instrumentpanelen.

## Översikt, mått {#overview-metrics}

Kontrollpanelen markerar tre viktiga mätvärden högst upp på sidan: synlighetspoäng, omnämnanden och citat. Ju lägre antal mätvärden desto värre blir ert varumärke, och ni bör agera för att förbättra ert varumärke. som **SR - Lägg till optimeringslänk här**. Nedan visas en kort beskrivning av varje mätvärde och vad det representerar.

![Översiktsmått](/help/dashboards/assets/overview-metrics.png)

### Synlighetspoäng {#visibility-score}

Synlighetspoängen består av faktorer som omnämnanden, citat, känslouttryck och rangordning. Varje faktor har en viss &quot;vikt&quot; som läggs till i den slutliga poängen. Exempel: omnämnande av&quot;vikt more&quot; eftersom synlighet bara spelar roll om varumärket anges.

### Omnämnanden {#mentions}

Detta mått visar det totala antalet gånger som ert varumärke eller era kategorier omnämns i alla AI-prompter i urvalet. Du har till exempel varumärket&quot;Kaffe B&quot;, med kategorierna&quot;Maskiner&quot; och&quot;Tillbehör&quot;, och det här måttet räknar det totala antalet gånger som de visas i de AI-svar som du tar prov på.

### Citat {#citations}

Det här måttet anger hur många gånger platsen refererades som en källa.

Trendindikatorer för varje nyckelmätvärde visar hur dessa värden ändras över tiden jämfört med föregående period.

## Jämförelse av konkurrenter {#competitor-comparison}

I jämförelseavsnittet kan du välja upp till fem konkurrenter och jämföra deras omnämnanden och citat med ert varumärke. På så sätt kan ni visa och testa hur ni presterar i förhållande till konkurrenterna.

![Jämförelse av konkurrenter](/help/dashboards/assets/competitor-comparison.png)

Konkurrenterna väljs i listrutan och diagrammen uppdateras när du klickar på **Använd filter**. Diagrammen visar veckovisa omnämnanden och veckocitat sida vid sida. Du kan också hålla muspekaren längs diagrammet för att se datautvecklingen under veckotidsperioden.

## Sentiment - trendanalys {#sentiment-trend}

I avsnittet för analys av attitydtrender kan du spåra hur ert varumärke uppfattas i de AI-svar som ingår i urvalet. Måttet för senhetstrend kan vara antingen positivt, neutralt eller negativt. Det kan till exempel vara positivt om svaren lyfter fram produktkvaliteten eller negativt om de talar om dålig service. Trenddiagrammet visar förändringarna i vecka för varumärkesuppfattning över vecka. Avsnittet fylls bara i efter att ert varumärke har omnämnts.

![Känslotrend](/help/dashboards/assets/sentiment-trend.png)

## Data Insights and Share of Voice {#data-insights}

Vi har två viktiga tabeller - datainsikter och delning av röst - som rundar upp kontrollpanelen. Den information som presenteras i dessa tabeller hjälper er att identifiera var ert varumärke är starkt och var optimering behövs.  Genom att använda tabellen **datainsikter** kan du utforska ämnen och användarfrågor för att utvärdera och optimera innehållets effekt. Resultaten är detaljerade efter ämnen och uppmaningar. Samtidigt jämförs varumärkesuttrycket med andra konkurrenter i tabellen **andel av röst**, vilket hjälper dig att identifiera brister och prioritera framtida ämnen.

![Datainsikter](/help/dashboards/assets/data-insights.png)

Båda tabellerna har ett sökfält som ger snabb åtkomst till ämnen. Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.

Klicka på flikarna nedan om du vill ha mer information om varje tabell och tillhörande mått.

>[!BEGINTABS]

>[!TAB Datainsikter]

Tabellen med datainsikter hjälper dig att utforska ämnen och användaruppmaningar för att utvärdera och optimera innehållets effekt. Här visas följande mått:

* **Kategori** - Ämneskategorin representerar SEO-nyckelord och användarfrågor som rör ditt varumärke. Du kan klicka för att expandera varje ämne och se enskilda frågor analyserade för att se om det finns ett varumärke. Varje ämne och knapp har en **Detaljer** -knapp när du håller muspekaren över det. Om du klickar på knappen visas ett separat fönster med mer information.
* **Popularitet** - popularitetskategorin representerar sökvolymen för det här ämnet i förhållande till alla andra ämnen i analysen. Värdet kan vara antingen Hög, Medium eller Låg.
* **Synlighetspoäng** - Synlighetspoäng för det ämnet. Den återspeglar viktade faktorer som omnämnanden, citat, känslor och rankning.
* **Omnämnanden** - Det antal gånger ditt varumärke omnämns i AI-svar för det här ämnet eller den här kombinationen av ämne och fråga.
* **Sentiment** - Varumärkesuppfattningen i AI-svar vad gäller respektive ämne. Uppskattningsvärdet kan vara antingen positivt, neutralt eller negativt.
* **Position** - Hur tidigt ditt varumärke visas i AI-svaret, beräknat som ett genomsnitt för alla veckor.
* **Alla källhänvisningar** - Antalet unika källor som anges i AI-svar för det här ämnet eller den här kombinationen av ämne och kommando (inklusive egna citat).
* **Ägda citat** - Det antal gånger ditt varumärke citerades i AI-svar för det här nyckelordet eller den här nyckelords-/frågekombinationen.

>[!TAB Delning av röst]

Tabellen **andel av röst** jämför varumärkesuttrycket med andra konkurrenter i alla ämnen. Här visas följande mått:

* **Ämne** - Det analyserade ämnet.
* **Popularitet** - Sökvolymen för ämnet i förhållande till alla andra ämnen i din analys.
* **Omnämnanden** - Antal gånger ditt varumärke omnämns i AI-svar för ämnet eller för kombinationen ämne/fråga.
* **Rankning** - rankningen av ert varumärkes Voice i förhållande till alla identifierade konkurrenter.
* **Andel av röst** - Den procentandel av tiden som ett varumärke anges i förhållande till alla omnämnanden i AI-svar.
* **De fem främsta konkurrenterna** - De fem främsta konkurrenterna ordnade efter deras andel av röst (högst till lägst).

>[!ENDTABS]

### Använda Data Insights-tabellen {#using-data-insights}

Tabellen Data Insights hjälper er att gå från mått till åtgärder genom att bryta ned prestanda på ämne- och promptnivå.

Viktiga sätt att använda tabellen:

* Prioritera ämnen med stor popularitet och låg synlighet - fokusoptimering där målgruppens efterfrågan är stark men där varumärkets närvaro är svag.
* Spåra känslomässig förändring - hitta ämnen där omnämnanden är negativa eller neutrala och koordinera ert svar.
* Jämför citat mot egna citat - identifiera frågor där ert varumärke nämns, men där konkurrentens innehåll anges, vilket signalerar ett innehållslucka.
* Utvärdera positionsintervall - övervaka om ditt varumärke visas tidigt i AI-svar (position 1-3) eller längre ned (6-10).
