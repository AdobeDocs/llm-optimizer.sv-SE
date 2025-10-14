---
title: Myndighetstrafik
description: Lär dig hur du använder kontrollpanelen för AI-trafik för att se hur AI-agenter interagerar med din webbplats.
feature: Agentic Traffic
source-git-commit: c6e37395362262eb5fe8366473e76086e36d77e9
workflow-type: tm+mt
source-wordcount: '1100'
ht-degree: 0%

---


# Myndighetstrafik {#agentic-traffic}

Kontrollpanelen för AGATIVE Traffic visar hur AI-agenter (crawler och chattbots) interagerar med din webbplats. Genom att använda den här vyn kan du spåra det totala antalet förfrågningar och allmänna prestandarelaterade mått. Du kan också visa fördelningen av trafik mellan marknader, kategorier, sidor och agenter. De data som används av den här instrumentpanelen hämtas från CDN-loggarna, så du måste konfigurera **CDN-loggvidarebefordran** för att kunna visa mätvärden. Det finns också anpassningsbara filter som hjälper dig att förfina de data som visas.

![Trafikdistribution](/help/dashboards/assets/ag-main.png)

Den här sidan innehåller följande information:

* [Filter](#filters)
* [Inställningar för CDN](#cdn-setup)
* [Trafikdistribution](#traffic-distribution)
* [Myndigheter - Trafiktrender](#agentic-trends)
* [Övre och nedre flyttningar](#top-bottom-movers)
* [Prestandaanalys för användaragent och URL](#user-url-performance)

## CDN-loggvidarebefordran {#cdn-setup}

Utan **CDN-loggvidarebefordran** är instrumentpanelen för agenttrafik tom. Om du vill visa agentiska interaktioner måste du konfigurera **CDN-loggvidarebefordran**.  När du loggar in första gången visas ett meddelande enligt bilden nedan.

![CDN-inställningar](/help/dashboards/assets/ag-log-forward1.png)

Välj **Gå till konfiguration** så navigerar du automatiskt till fliken **CDN-konfiguration** på kontrollpanelen för [kundkonfiguration](/help/dashboards/customer-configuration.md).

![Inbyggt CDN-installationsprogram](/help/dashboards/assets/ag-log-forward2.png)

Välj **Inbyggt CDN** på den här fliken. CDN-providerfönstret visas.

<!-- [CDN Provider](/help/dashboards/assets/ag-log-forward3.png)-->
I fönstret **Onboard CDN Provider**:

1. Välj din CDN-leverantör (till exempel Akamai, Adobe-hanterad Fastly, Fastly, AWS Cloudfront, Azure CDN, Cloudflare eller Annan).
2. Klicka på **Anonboard** om du vill aktivera vidarebefordran av loggar.

Om du väljer **Annan** måste du kontakta llmo-now@adobe.com för att få hjälp.

När loggarna har aktiverats hämtas de in och kontrollpanelen fylls i med mätvärden som total agentinteraktion, framgångsgrad, träffar per marknad, användaragentanalys och URL-nivåprestanda.

## Filter {#filters}

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

* **Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
* **Kategori** - Filtrera de resultat som visas efter fördefinierade kategorier eller anpassade kategorier.
* **Plattform** - Välj vilken AI-motor som ska analyseras.
* **Agenttyp** - Filtrera efter den typ av AI-agent som interagerade med din plats. Du kan filtrera mellan crawler, chattbotar eller alla agenter.
* **Slutförandefrekvens** - Filtrera efter interaktionskvalitet (hög, medel eller låg). Det här måttet representerar procentandelen lyckade HTTP-begäranden, inklusive både direkt slutförda svar (2xx-statuskoder) och omdirigeringar (3xx-statuskoder).
* **Innehållstyp** - Visa autentisk interaktion för olika innehållstyper, som HTML, PDF och så vidare.

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

I vyn Top and Bottom Movers (Översta och understa flytten) markeras URL:er med de största vecko-över-vecka-förändringarna i autentisk trafik - besök eller träffar från AI-system som använder ditt innehåll. De översta topparna visar vilka sidor som blir synliga eller engagerande, medan de nedersta flyttarna visar URL:er med de brantaste minskningarna. På så sätt kan ni snabbt identifiera vilket innehåll som trasar uppåt, vilket kan behöva åtgärdas och var de AI-drivna identifieringsmönstren förskjuts.

![Övre och nedre flyttningar](/help/dashboards/assets/movers.png)

## Prestandaanalys för användaragent och URL {#user-url-performance}

Vyerna för användaragenten och URL-prestandaanalys ger ytterligare datauppdelningar om hur crawler och chattbots interagerar med webbplatsen. Klicka på flikarna nedan för detaljerade beskrivningar.

![Prestandaanalys för användaragent och URL](/help/dashboards/assets/user-agent.png)

>[!BEGINTABS]

>[!TAB Analys av användaragent]

Tabellen för användaragentanalys innehåller en beskrivning av trafiken per sidtyp och agenttyp (till exempel crawler jämfört med chattbots). På det här sättet är det lätt att förstå vilka AI-agenter som crawlar vilka delar av din webbplats som ska användas. Den innehåller följande kategorier:

* **Sidtyp** - Sidtyp.
* **Agenttyp** - AI-agenten crawlar sidan, antingen en crawler eller en chattbot.
* **träffar** - Det totala antalet begäranden som AI-agenter har gjort för den specifika sidtypen.

>[!TAB URL-prestandaanalys]

Tabellen URL-prestandaanalys visar en detaljerad vy över enskilda URL:er. Detta inkluderar träffar, unika agenter, toppmedarbetare, antal lyckade inköp och kategorier. På så sätt kan du identifiera värdefulla sidor, upptäcka kryphål och optimera innehåll för AI-motorer. URL:erna rangordnas efter trafikvolym. Tabellen innehåller följande kategorier:

* **URL** - Den undersökta URL:en.
* **Totalt antal träffar** - Totalt antal begäranden från AI-agenter till URL:en.
* **Unika agenter** - Antalet olika AI-agenter som använde URL:en.
* **Top Agent** - AI-agenten som genererade mest trafik till URL:en.
* **Agenttyp** - Den typ av AI-agent som genererade mest trafik till den här URL:en.
* **Slutförandefrekvens** - Procentandel lyckade HTTP-begäranden, inklusive både direkta lyckade svar och omdirigeringar.
* **Kategori** - Den kategori som bäst matchar sidans innehåll.

I URL-prestandatabellen finns ett sökfält som ger snabb åtkomst till URL:er. Du kan också visa ytterligare information för varje URL genom att klicka på informationsikonen i slutet av varje rad.

![URL-information](/help/dashboards/assets/details.png)

Vyn URL-detaljer ger en helhetsbild av en sidas prestanda - som visar hur ofta den anges, känslan av AI-svar där den nämns, ämnen och uppmaningar som den visas i samt trender för faktisk och hänvisningstrafik över tid.

>[!ENDTABS]

På båda tabellerna kan du använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.
