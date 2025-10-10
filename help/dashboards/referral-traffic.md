---
title: Hänvisningstrafik
description: Det här är artikelöversikten.
source-git-commit: c83b4929a82331534654fcfdccd41d91eefe6d92
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# Hänvisningstrafik

Referenstrafik visar hur besökare kommer till din webbplats från externa plattformar, AI-citat och hänvisningslänkar. Den spårar och analyserar trafikkällor, referensmönster och konverteringsvärden från externa webbplatser och plattformar. Detta hjälper er att förstå vilka källor, regioner och sidor som driver den mest engagerade trafiken. Data hämtas antingen från CDN-loggarna eller från AEM Operational Telemetry **(lägg till länk)**. Båda dessa källor bevarar integriteten och samlar inte in personuppgifter.

Det finns också anpassningsbara filter som hjälper dig att förfina de data som visas.

Den här sidan innehåller följande information:

* [Inställningar](#setup)
* [Filter](#filters)
* [Övergripande hänskjutningsresultat](#overall-performance)
* [Övre hänvisnings-URL](#top-referrals)
* [Hänvisningsinformation för trafik](#traffic-details)

## Inställningar {#setup}

Vid den första inloggningen kan kontrollpanelen för hänvisningstrafik visas tom. Om du vill visa dina data måste du konfigurera en hänvisningstrafikleverantör.

![Inställningar för hänskjutning](/help/dashboards/assets/referral-setup.png)

1. Välj din Source (antingen CDN-loggar eller AEM Operational Telemetry).
2. Ange en primär e-postadress till kontakten.
3. Klicka på **Begär aktivering** om du vill aktivera datainmatning.

När kontrollpanelen har aktiverats fylls den i med referensvärden för trafik.

## Filter {#filters}

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

**Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
**Plattform** - Välj en specifik trafikkälla som Google, OpenAI eller sociala medier.
**Kanal** - Filtrera mellan kanaler som Intjänad eller Betalad. Om du föredrar en blandning mellan de båda väljer du alternativet **Alla kanaler**.
**Kanal-Source** - Filtrera efter kanalens källa. bland annat: visning, sökning, hänvisningar, video och sociala medier. Du kan också använda **Alla plattformar** för att visa alla kanalkällor.
**Enhetstyp** - Analysera trafik efter besökarens enhetstyp, antingen på dator, mobil eller alla enheter.
**Region** - Visa hänvisningsmönster för olika platser.

När du har valt önskat filter klickar du på **Använd filter** för att använda markeringen på instrumentpanelen.

## Övergripande hänskjutningsresultat {#overall-performance}

Kontrollpanelen markerar hänvisningens övergripande prestanda genom att visa följande nyckeltal:

* **Total hänvisningstrafik** - den totala hänvisningstrafiken från alla källor.
* **Medgivandefrekvens** - Procentandel besökare som accepterar en fråga om medgivande.
* **Studsfrekvens** - Procentandel sessioner från hänvisningskällor som inte hade någon engagemangshändelse.

![Referenssida](/help/dashboards/assets/referral-traffic.png)

Förutom de övergripande prestandamätningarna ovan, delar panelen **De översta regionerna** upp trafiken per land. Under tiden visar panelen **De viktigaste referenskällorna** de plattformar som leder flest besök.

## Övre hänvisnings-URL {#top-referrals}

De översta hänvisnings-URL:erna visar webbplatsens mest besökta sidor från hänvisningar.

![URL:er för översta referensen](/help/dashboards/assets/top-url.png)

## Hänvisningsinformation för trafik {#traffic-details}

Tabellen Hänvisningsinformation hjälper dig att utvärdera både trafikvolym och kvalitet. Den ger en detaljerad fördelning efter källa, inklusive antal besök, avhoppsfrekvens och kanaltyp.

![Information om hänvisningstrafik](/help/dashboards/assets/traffic-details.png)

Tabellen innehåller följande kategorier:

**Source** - källan för hänvisningstrafiken.
**Besök** - Det totala antalet besök för varje källa.
**Studsfrekvens** - Procentandel sessioner från hänvisningskällan som inte hade någon engagemangshändelse.
**Kanal** - Källans kanal, antingen upparbetat, betalt eller både och.

Du kan använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera hänvisningstrafiken i chefsrapporteringen.
