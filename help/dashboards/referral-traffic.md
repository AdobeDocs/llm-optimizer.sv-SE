---
title: Hänvisningstrafik
description: Lär dig hur du använder kontrollpanelen för hänvisningstrafik för att se hur besökare kommer till din webbplats från externa plattformar, AI-citat och hänvisningslänkar.
feature: Referral Traffic
source-git-commit: e50c87e8e5a669526f3c10855c1629ce82b67aef
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---


# Hänvisningstrafik

Referenstrafik visar hur besökare kommer till din webbplats från externa plattformar, AI-citat och hänvisningslänkar. Den spårar och analyserar trafikkällor, referensmönster och konverteringsvärden från externa webbplatser och plattformar. Detta hjälper er att förstå vilka källor, regioner och sidor som driver den mest engagerade trafiken. <!--Data is sourced from the CDN logs, a privacy-preserving source that does not capture personal user data.--> Det finns också anpassningsbara filter som hjälper dig att förfina de data som visas.

![Referenssida](/help/dashboards/assets/referral-traffic.png)

Den här sidan innehåller följande information:

* [Inställningar](#setup)
* [Filter](#filters)
* [Övergripande hänskjutningsresultat](#overall-performance)
* [Övre hänvisnings-URL](#top-referrals)
* [Hänvisningsinformation för trafik](#traffic-details)

## Inställningar {#setup}

Vid den första inloggningen kan kontrollpanelen för hänvisningstrafik visas tom. Om du vill visa dina data måste du konfigurera [CDN-loggvidarebefordran](/help/dashboards/customer-configuration.md#cdn-configuration) genom att välja **Gå till konfiguration**.

![Inställningar för hänskjutning](/help/dashboards/assets/referral-setup1.png)

<!--- 1. Select your Source (either CDN logs or AEM Operational Telemetry).
2. Enter a primary contact email.
3. Click **Request activation** to enable data ingestion. Hiding this until confirmation from PM-->

När kontrollpanelen har aktiverats fylls den i med referensvärden för trafik.

## Filter {#filters}

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

* **Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
* **Plattform** - Välj en specifik trafikkälla som Google, OpenAI eller sociala medier.
* **Sidåtergivning** - Filtrera hänvisningstrafik efter användaråtergivning.
* **Kanal-Source** - Filtrera efter kanalens källa. är bland annat LLM, förtjänade, betalda eller blandade hänvisningskanaler.
* **Enhetstyp** - Analysera trafik efter besökarens enhetstyp, antingen på dator, mobil eller alla enheter.
  **Region** - Visa hänvisningsmönster för olika platser.

När du har valt önskat filter klickar du på **Använd filter** för att använda markeringen på instrumentpanelen.

## Övergripande hänskjutningsresultat {#overall-performance}

Kontrollpanelen markerar hänvisningens övergripande prestanda genom att visa viktiga värden, bland annat:

* **Total hänvisningstrafik** - den totala hänvisningstrafiken från alla källor.
* **Referenstrafik från LLM:s** - den totala hänvisningstrafiken från LLM:er.
* **Medgivandefrekvens** - Procentandel besökare som accepterar en fråga om medgivande.
* **Studsfrekvens** - Procentandel sessioner från hänvisningskällor som inte hade någon engagemangshändelse.

![Referenssida](/help/dashboards/assets/referral-traffic.png)

Förutom de övergripande prestandamätningarna ovan finns det ytterligare tre paneler som visar trafikfördelningen mellan olika marknader, hänvisningskällor och kategorier för sidavsikt <!-- the **Top Regions** panel breaks down traffic by geography. Meanwhile, the **Top Referral Sources** panel shows the platforms driving the most visits. Trend indicators for the metrics show how these values are changing over time compared to the previous period.-->

<!--## Top Referral URLs {#top-referrals}

The Top Referral URLs list surfaces your site's most visited pages from referrals.

![Top Referral URLs](/help/dashboards/assets/top-url.png)-->

## Information om referenskällor och URL-prestandaanalys {#traffic-details}

Med tabellerna Händelsekällsinformation och URL-prestandaanalys kan du utvärdera både trafikvolym och kvalitet. Klicka på varje flik nedan för mer information:

![Information om hänvisningstrafik](/help/dashboards/assets/traffic-details.png)

>[!BEGINTABS]

>[!TAB Information om referenskällor]

Vyn Information om referenskällor delar upp trafik från olika plattformar som OpenAI, Microsoft, Google och Perplexity. Här visas viktiga mätvärden som besök, studsfrekvens och kanaltyp, vilket hjälper dig att förstå vilka AI och sökkällor som driver den mest engagerade trafiken till din webbplats.

* **Source** - källan för hänvisningstrafiken.
* **Besök** - Det totala antalet besök för varje källa.
* **Studsfrekvens** - Procentandel sessioner från hänvisningskällan som inte hade någon engagemangshändelse.
* **Kanal** - Källans kanal, antingen upparbetat, betalt eller både och.

>[!TAB URL-prestandaanalys]

Vyn för URL-prestandaanalys rangordnar sidor som presterar bäst baserat på hänvisningstrafikvolym från LLM och andra källor. Den visar på mätvärden som trafik, studsfrekvens, medgivande och sidavsikt, och hjälper dig att identifiera vilka sidor som lockar och behåller de mest engagerade besökarna från AI-drivna hänvisningar. Tabellen har ett sökfält för snabb åtkomst till ämnen.

>[!ENDTABS]

På båda tabellerna kan du använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellerna i den verkställande rapporten. För båda tabellerna kan du dessutom anpassa vilka mätvärden som visas genom att klicka på knappen **Konfigurera kolumner** .
