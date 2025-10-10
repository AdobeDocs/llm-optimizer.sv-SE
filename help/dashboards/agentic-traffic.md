---
title: Myndighetstrafik
description: Lär dig hur du använder kontrollpanelen för AI-trafik för att se hur AI-agenter interagerar med din webbplats.
source-git-commit: e8ea9ae0d6592ea3d1e9945ec117f852112ba9d7
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 0%

---


# Myndighetstrafik {#agentic-traffic}

På kontrollpanelen för AGATIVE Traffic visas hur AI-agenter (crawler och chatbots) interagerar med din webbplats. Genom att använda den här vyn kan du spåra det totala antalet förfrågningar och allmänna prestandarelaterade mått. Du kan också visa fördelningen av trafik mellan marknader, kategorier, sidor och agenter. De data som används av den här instrumentpanelen hämtas från CDN-loggarna, så du måste konfigurera **CDN-loggvidarebefordran** för att kunna visa mätvärden. Det finns också anpassningsbara filter som hjälper dig att förfina de data som visas.

Den här sidan innehåller följande information:

* [Filter](#filters)
* [Inställningar för CDN](#cdn-setup)
* [Trafikdistribution](#traffic-distribution)
* [Myndigheter - Trafiktrender](#agentic-trends)
* [Övre och nedre flyttningar](#top-bottom-movers)
* [Prestandaanalys för användaragent och URL](#user-url-performance)

## Inställningar för CDN {#cdn-setup}

När du loggar in för första gången är kontrollpanelen för AGAL-trafik tom. Om du vill visa agentiska interaktioner måste du konfigurera **CDN-loggvidarebefordran**. **TBD-pekar på CDN-konfigurationen vid snabbstart/introduktion?**

![CDN-inställningar](/help/dashboards/assets/ag-log-forward.png)

1. Välj din CDN-leverantör (till exempel Akamai, Adobe-hanterad Fastly, Fastly, AWS Cloudfront, Azure CDN, Cloudflare eller Annan).
2. Ange en primär e-postadress till kontakten.
3. Klicka på **Begär aktivering** om du vill aktivera vidarebefordran av loggar.

När loggarna har aktiverats hämtas de in och kontrollpanelen fylls i med mätvärden som total agentinteraktion, framgångsgrad, träffar per marknad, användaragentanalys och URL-nivåprestanda.

## Filter {#filters}

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

* **Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
* **Kategori** - Filtrera de resultat som visas efter fördefinierade kategorier. Du kan också lägga till egna kategorier i det här fältet (**SR**-how?).
* **Plattform** - Välj vilken AI-motor som ska analyseras.
* **Agenttyp** - Filtrera efter den typ av AI-agent som interagerade med din plats. Du kan filtrera mellan crawler, chattbotar eller alla agenter.
* **Slutförandefrekvens** - Filtrera efter interaktionskvalitet (hög, medel eller låg). Det här måttet visar hur många procent lyckade HTTP-begäranden, inklusive både direkta lyckade svar och omdirigeringar.
* **Innehållstyp** - Filtrera efter innehållstyp, antingen HTML eller text.

När du har valt önskat filter klickar du på **Använd filter** för att använda markeringen på instrumentpanelen.

## Trafikdistribution {#traffic-distribution}

Trafikdistributionsvyn visar hur agenttrafiken sprids mellan marknader, kategorier och sidtyper. Den här vyn hjälper dig att avgöra vilka geografiska områden, produktområden eller innehållsformat som AI-agenterna mest använder när de interagerar med webbplatsen.

![Trafikdistribution](/help/dashboards/assets/ag-main.png)

Överst på sidan finns det tre viktiga mätvärden som du måste känna till:

* **Agentinsk interaktion** - Det här måttet representerar det totala antalet förfrågningar från AI-agenter till din webbplats. Detta omfattar all trafik från sökmotorer, chattbottar och annan icke-mänsklig trafik.
* **Slutförandefrekvens** - Det här måttet representerar procentandelen lyckade HTTP-begäranden, både direkta lyckade svar och omdirigeringar.
* **Genomsnittlig TTFB** - Tid till första byte (TTFB) mäter den tid det tar för den första byten data att tas emot från servern. Lägre värden innebär snabbare svarstider på servern.

Trendindikatorer för varje nyckelmätvärde visar hur dessa värden ändras över tiden jämfört med föregående period.

## Myndigheter - Trafiktrender {#agentic-trends}

Använd diagrammet&quot;Agentic Traffic Trends&quot; för att följa upp antalet lyckade, misslyckade och övergripande träffar per vecka. På så sätt kan du övervaka ändringar i agentaktivitet och prestanda över tid. Du kan också hålla muspekaren längs diagrammet för att se datautvecklingen under veckotidsperioden.

![Agenttrafiktrender](/help/dashboards/assets/ag-trends.png)

## Övre och nedre flyttningar {#top-bottom-movers}

Dessa två mätvärden sorterar URL:erna enligt följande:

* **De översta topparna** - URL:er med den största ökningen av agentiell trafik från den äldsta till den senaste veckan.
* **Bottom Movers** - URL:er med den största minskningen av den autentiska trafiken från den äldsta till den senaste veckan.

## Prestandaanalys för användaragent och URL {#user-url-performance}

Vyerna för användaragenten och URL-prestandaanalys ger ytterligare datauppdelningar om hur crawler och chattbots interagerar med webbplatsen. Klicka på flikarna nedan om du vill ha detaljerade beskrivningar.

![Prestandaanalys för användaragent och URL](/help/dashboards/assets/user-agent.png)

>[!BEGINTABS]

>[!TAB Analys av användaragent]

Tabellen för användaragentanalys innehåller en beskrivning av trafiken per sidtyp och agenttyp (till exempel crawler jämfört med chattbots). På det här sättet är det lätt att förstå vilka AI-agenter som crawlar vilka delar av din webbplats som ska användas. Den innehåller följande kategorier:

* **Sidtyp** - Sidtyp.
* **Agenttyp** - AI-agenten crawlar sidan, antingen en crawler eller en chattbot.
* **träffar** - Det totala antalet begäranden som AI-agenter har gjort för den specifika sidtypen.

Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela agentanalysen med ditt team eller inkludera den i chefsrapporter.

>[!TAB URL-prestandaanalys]

Tabellen URL-prestandaanalys visar en detaljerad vy över enskilda URL:er. Detta inkluderar träffar, unika agenter, toppmedarbetare, antal lyckade inköp och kategorier. På så sätt kan du identifiera värdefulla sidor, upptäcka kryphål och optimera innehåll för AI-motorer. URL:erna rangordnas efter trafikvolym. Tabellen innehåller följande kategorier:

* **URL** - Den undersökta URL:en.
* **Totalt antal träffar** - Totalt antal begäranden från AI-agenter till URL:en.
* **Unika agenter** - Antalet olika AI-agenter som använde URL:en.
* **Top Agent** - AI-agenten som genererade mest trafik till URL:en.
* **Agenttyp** - Den typ av AI-agent som genererade mest trafik till den här URL:en.
* **Slutförandefrekvens** - Procentandel lyckade HTTP-begäranden, inklusive både direkta lyckade svar och omdirigeringar.
* **Kategori** - Den kategori som bäst matchar sidans innehåll.

I URL-prestandatabellen finns ett sökfält som ger snabb åtkomst till URL:er. Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.

>[!ENDTABS]
