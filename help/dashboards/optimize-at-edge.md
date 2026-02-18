---
title: Optimera på Edge
description: Lär dig leverera optimeringar i LLM Optimizer i CDN-kanten utan att behöva göra några redigeringsändringar.
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '2172'
ht-degree: 0%

---


# Optimera på Edge

På den här sidan finns en detaljerad översikt över hur du kan leverera optimeringar vid CDN-kanten utan att behöva göra några redigeringsändringar. Det handlar om introduktionsprocessen, tillgängliga optimeringsmöjligheter och hur ni kan optimera automatiskt vid behov.

>[!NOTE]
>Den här funktionen är för närvarande i tidig åtkomst. Du kan läsa mer om Tidig åtkomst-program [här](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs).

## Vad är Optimize på Edge?

Optimize at Edge är en edge-baserad driftsättningsfunktion i LLM Optimizer som ger AI-vänliga ändringar av användaragenter för livslångt lärande. I det aktuella sammanhanget innebär&quot;Edge&quot; att optimeringen används i CDN-lagret. Eftersom det levererar optimeringar i CDN-lagret behövs inga redigeringsändringar i Content Management System (CMS), vilket innebär att CMS inte ändras. Tack vare den här separationen kan du förbättra LLM-synligheten utan att ändra befintliga publiceringsarbetsflöden. Den riktar sig endast till agell trafik och påverkar varken mänskliga användare eller SEO-robotar. När LLM Optimizer upptäcker möjligheter att optimera en sida kan man driftsätta korrigeringar direkt vid CDN-kanten.

Optimera på Edge är ett snabbare och smidigare alternativ till traditionella korrigeringar som kräver komplexa konstruktionssatsningar. När du har slutfört en engångskonfiguration behöver du som sagt inte göra några plattformsändringar eller längre utvecklingscykler för att ändringarna ska börja gälla. Ni kan publicera förbättringar på några minuter utan att behöva något engagemang från utvecklarna. Det är ett kodfritt sätt att optimera webbplatsen för AI-agenter.

Optimera på Edge är utformat för företagsanvändare i team som arbetar med marknadsföring, sökmotoroptimering, innehåll och digital strategi. Det kan göra det möjligt för företagsanvändare att slutföra hela kundresan i LLM Optimizer: identifiera möjligheter, förstå förslag och enkelt implementera korrigeringarna. Med Optimize på Edge kan man förhandsgranska ändringarna, snabbt driftsätta dem vid CDN-kanten och validera att optimeringarna är aktiva. Prestanda kan spåras i LLM Optimizer ekosystem.

### Viktiga fördelar

* **Leverans endast för AI:** Serverar optimerade HTML endast för AI-agenter utan påverkan på vare sig besökare eller SEO-robotar.
* **Snabbare cykler:** Publicera ändringar på några minuter, inte veckor. Inga plattformsändringar eller långa konstruktionscykler krävs.
* **Reversibel:** Stöds med en enklicksåterställning som kan återställa sidan på några minuter.
* **Ingen prestandapåverkan:** Edge-baserade optimeringar och cachelagring förhindrar att webbplatsens latens påverkas.
* **CDN och CMS-agnostiker:** Fungerar med alla CDN-konfigurationer och frontendkonfigurationer oavsett Content Management System.

### Vilka möjligheter stöds av Optimize på Edge?

Möjligheter som kan förbättra den autentiska webbupplevelsen stöds av Optimize på Edge. Läs mer om varje affärsmöjlighet både på sidan [Affärsmöjligheter](/help/dashboards/opportunities.md) och i avsnittet om affärsmöjligheter på den aktuella sidan.

## Onboarding

Du bör kontakta antingen ditt Adobe-kontoteam eller FDE-teamet för att starta introduktionsprocessen. IT- eller CDN-teamet måste också slutföra installationen. Du kan även kontakta `llmo-at-edge@adobe.com` om du vill ha mer hjälp med introduktionen.

Krav för att kunna utnyttja Optimera på Edge:

* Slutför introduktionsprocessen till LLM Optimizer.
* Slutför vidarebefordringsprocessen för CDN-loggarna.

Krav för IT-avdelningen:
* Lägg till `*AdobeEdgeOptimize/1.0*` användaragent i Tillåtelselista i webbplatsens robots.txt-fil eller bot-trafik-hanteringsregler.
* Kontrollera att sidorna inte blockeras på domän- eller CDN-nivå.
* Lägg till Optimize på Edge routningsregler i CDN.
* Bekräfta Optimera vid Edge-routning i LLM Optimizer gränssnitt.

Om du vill vägleda installationsprocessen väljer du din CDN-leverantör nedan och följer motsvarande konfigurationsguide. Tänk på att dessa exempel bör anpassas till den aktuella livekonfigurationen. Vi rekommenderar att du gör ändringar i de lägre miljöerna först.

### CDN-konfigurationsguider

| CDN-provider | Typ | Guide |
|---|---|---|
| Hanterad CDN för tjänsten AEM Cloud (snabbt) | Adobe Managed | [Visa installationsguiden](/help/dashboards/optimize-at-edge-aem-managed-cdn.md) |
| Snabbt (BYOCDN) | Ta med din egen CDN | [Visa installationsguiden](/help/dashboards/optimize-at-edge-fastly-byocdn.md) |
| Akamai (BYOCDN) | Ta med din egen CDN | [Visa installationsguiden](/help/dashboards/optimize-at-edge-akamai-byocdn.md) |
| Cloudflare (BYOCDN) | Ta med din egen CDN | [Visa installationsguiden](/help/dashboards/optimize-at-edge-cloudflare-byocdn.md) |

>[!NOTE]
>Om din CDN-leverantör inte finns med i listan ovan, eller om du inte hittar din domän eller e-postadress i LLM Optimizer-gränssnittet, ber vi dig kontakta `llmo-at-edge@adobe.com` för att få hjälp med introduktionen. När konfigurationerna är klara kan du driftsätta förslag för Optimera på Edge i LLM Optimizer.

Varje installationsguide för CDN ovan innehåller detaljerade verifieringssteg i slutet för att bekräfta att agell trafik dirigeras korrekt och att mänsklig trafik inte påverkas.

## Möjligheter

I följande tabell visas möjligheter som kan förbättra den agentiska webbupplevelsen och stöds av Optimize på Edge.

| Möjligheter | Typ | Identifiera automatiskt | Föreslå automatiskt | Automatisk optimering |
|---------|----------|----------|----------|----------|
| Återskapa innehållets synlighet | Teknisk GEO | Identifierar sidor där kritiskt innehåll är dolt för AI-agenter. Visar URL:er som påverkas och förväntat innehåll som kan återställas. | Markerar innehåll som kan göras tillgängligt för AI-agenter och rekommenderar att förrendering aktiveras för dessa sidor. | Skapar en helåtergiven, AI-vänlig HTML-ögonblicksbild av den autentiska trafik som återställer det tidigare dolda innehållet. |
| Lägg till LLM-vänliga sammanfattningar | Optimering av innehåll | Identifierar långa eller komplexa sidor som saknar koncisa sammanfattningar på sid- eller avsnittsnivå, vilket gör dem svårare för AI att snabbt skanna och förstå. | Rekommenderar korta, AI-genererade sammanfattningar på sid- och avsnittsnivå som hämtar nyckelinnehåll. | Infogar sammanfattningarna i de relevanta HTML-avsnitten, vilket förbättrar hur modellerna tolkar och beskriver sidinnehållet. |
| Lägg till relevanta frågor | Optimering av innehåll | Identifierar mellanrum i det befintliga sidinnehållet som skulle kunna dra nytta av vanliga frågor. | Föreslår AI-genererat FAQ-innehåll som är anpassat till användaravsikten och befintliga ämnen. | Inför innehåll med vanliga frågor i HTML, vilket gör sidorna mer identifierbara och relevanta i AI-drivna svar. |
| Förenkla komplext innehåll | Optimering av innehåll | Flaggar sidor med komplex text som kan förhindra AI-förståelsen. | Innehåller AI-genererade förenklade versioner av komplex text samtidigt som den ursprungliga innebörden bevaras. | Skriver om komplexa avsnitt på sidan, vilket förbättrar AI-läsbarheten. |

### Ytterligare verktyg

[Adobe LLM Optimizer: Kan din webbsida redigeras?](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome-tillägget visar hur mycket av webbsidans innehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration.

Med ett enda klick kan du utvärdera vilken dator som kan läsas på en webbplats. Du kan visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad människor ser och uppskatta hur mycket innehåll som kan återställas med hjälp av LLM Optimizer. Ser du [Kan AI läsa din webbplats?](https://business.adobe.com/se/blog/introducing-the-llm-optimizer-chrome-extension) sida för mer information.

## Detaljerade affärsmöjligheter

I följande avsnitt kan du visa ytterligare information för varje affärsmöjlighet som stöds av Optimize på Edge.

### Återskapa innehållets synlighet

Den här affärsmöjligheten flaggar sidor där nyckelinnehåll döljs för AI-agenter på grund av klientåtergivning. För varje identifierad sida visar den exakt vilket innehåll som saknas i AI-agentvyn, markerar luckor i synligheten och gör det möjligt att direkt tillämpa ändringar för att återskapa det dolda innehållet. När du distribuerar denna möjlighet med Optimize på Edge får LLM-användaragenterna en förrenderad, AI-optimerad version av sidan så att de kan komma åt den fullständiga kontexten utan att köra Javascript.
Detta garanterar att sidan först är helt synlig för AI-agenter. Ytterligare förbättringar tillämpas ovanpå den förrenderade HTML.

>[!IMPORTANT]
>Denna förrenderingsfunktion gäller automatiskt alla möjligheter som presenteras nedan när den används med Optimera på Edge för att säkerställa att sidan är helt synlig för AI-agenter.

### Lägg till LLM-vänliga sammanfattningar

Denna möjlighet identifierar sidor som kan dra nytta av koncisa sammanfattningar för att hjälpa webbdesigners att snabbt förstå vad sidinnehållet handlar om. För varje sida identifierar affärsmöjligheten var en sammanfattning behövs och skapar AI-genererade sammanfattningar på sidnivå eller avsnittsnivå. När du distribuerar med Optimera på Edge infogas dessa sammanfattningar i HTML som AI-agenter hämtar, vilket förbättrar chanserna att ditt innehåll beskrivs mer korrekt.

### Lägg till relevanta frågor

Den här affärsmöjligheten flaggar sidor där ytterligare frågor och svar-innehåll bättre kan matcha användaravsikten och frågar vid AI-driven identifiering. För varje sida föreslås AI-genererade FAQ-block kopplade till användaravsikt och innehåll på sidan. Med Optimize på Edge injiceras dessa frågor och svar i HTML, vilket gör sidan mer AI-vänlig och ökar sannolikheten för att AI-svaren direkt speglar din vägledning.

### Förenkla komplext innehåll

Här hittar du sidor med långa, komplexa stycken som kan minska förståelsen av AI. För varje sida som överskrider läsbarhetströskeln skapas AI-genererat innehåll som är enklare och enklare att läsa in samtidigt som den ursprungliga innebörden bevaras. När innehållet distribueras i framkant hjälper det förenklade innehållet i den autentiska trafiken att tolka och sammanfatta innehållet på ett trognare sätt.

## Optimera automatiskt på Edge

För varje affärsmöjlighet kan du förhandsgranska, redigera, driftsätta, visa direkt och återställa optimeringarna.

>[!VIDEO](https://video.tv.adobe.com/v/3477983/?learn=on&enablevpops)

### Förhandsgranska

Med **Förhandsgranska** kan du se effekten av ett förslag innan det publiceras. Den visar på en skillnad sida vid sida mellan den aktuella sidan och den AI-optimerade versionen som förväntas när förslaget har tillämpats. I den här vyn används samma Optimera-funktion på Edge som i direkttrafiken, men i ett isolerat förhandsvisningsläge. Detta påverkar inte Live-trafik eftersom det är en skrivskyddad simulering för granskning.

![Förhandsgranska](/help/assets/optimize-at-edge/preview.png)

### Redigera

Med **Redigera** kan du förfina eller skriva om det automatiskt genererade förslaget helt innan du distribuerar det. Istället för att godkänna förslaget behåller du full kontroll genom redigeringsarbetsflödet. Vyn visar föreslagna ändringar i en strukturerad redigerare, där du kan ändra texten så att den bättre matchar den ursprungliga metoden. Den redigerade versionen skickas sedan till AI-agenter när de har distribuerats.

![Redigera](/help/assets/optimize-at-edge/edit.png)

### Distribuera

**Distribuera** publicerar de valda förslagen så att de optimerade upplevelserna kan hanteras från kanten till AI-agenter. Om CDN är helt dirigerad visas vanligtvis alla sidor i domänen med de nya ändringarna inom några minuter. Om routning har konfigurerats för enbart valda sökvägar, är det bara de tillåtslista sidorna som får publiceras med optimeringarna.

![Distribuera](/help/assets/optimize-at-edge/deploy.png)

### Visa Live

Med **Visa Live** kan du verifiera att optimeringen är aktiv och fungerar som förväntat för autentisk trafik, en vy som annars skulle vara svår att komma åt. Du kan visa den aktiva sidan under Fasta förslag, som återger sidan så som den visas för AI-agenter.

![Visa Live](/help/assets/optimize-at-edge/view-live.png)

### Återställning

Återställning återställer säkert en tidigare distribuerad optimering. Sidans AI-version återgår vanligtvis till det tidigare läget inom några minuter, vilket gör det enkelt att experimentera med optimeringar när det behövs.

![Återställning](/help/assets/optimize-at-edge/rollback.png)

## Vanliga frågor

F. Vilken typ av LLM riktar du dig till med Optimize på Edge?

Listan med användaragenter som ska användas som mål definieras av dig under introduktionsprocessen.

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

F. Vad händer om jag inte har börjat optimera på Edge än?

Om du klickar på **Distribuera optimeringar** innan du slutför den nödvändiga konfigurationen kommer ingenting att tillämpas på din plats. I stället visas en popup-dialogruta där du uppmanas att kontakta vårt team på `llmo-at-edge@adobe.com` för att få hjälp med introduktionen. Tills introduktionen är klar kan du fortfarande utforska de identifierade möjligheterna och förslagen, men arbetsflödet för distribution med ett klick förblir inaktivt.

F: Vad händer när innehållet uppdateras vid källan?

Vi levererar den optimerade versionen av sidan från cacheminnet så länge som den underliggande källsidan inte har ändrats. Men när källan ändras för **Återskapa innehållets synlighet** uppdateras systemet automatiskt så att AI-agenter alltid får det senaste innehållet. Det beror på att vi använder låg cachetid för att göra inställningar (i minutordning) så att alla innehållsuppdateringar på webbplatsen utlöser en ny optimering i det fönstret. För innehållsmöjligheter som **Lägg till vänliga LLM-sammanfattningar** övervakar LLM Optimizer källsidan för ändringar. Om en ändring upptäcks pausar vi optimeringen och flaggar den för mänsklig granskning för att förhindra att innehållet växlar mellan den agentsynliga sidan och den mänskligt synliga sidan.
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

F. Optimerar Edge endast för webbplatser som använder Adobe Edge Delivery Service (EDS)?

Nej. Optimera på Edge är CDN-agnostiker och fungerar med alla frontendarkitekturer, inte bara de som används i Adobe EDS Stack.

F. Hur skiljer sig Optimera vid Edge-förrendering från traditionell SSR-rendering (server-side rendering)?

Båda problemen kan lösas och fungera tillsammans. Traditionell SSR återger innehåll på serversidan, men inkluderar inte innehåll som läses in senare i webbläsaren. Optimera vid Edge-förrendering fångar upp sidan när data från JavaScript och klientsidan har lästs in, vilket skapar den fullständigt monterade versionen vid CDN-kanten. SSR fokuserar på att förbättra människors upplevelse och på Optimera på Edge förbättrar webbupplevelsen för LLM.

F. Vad händer om jag distribuerar optimeringar för vissa URL:er i min domän, men inte för alla?

Endast de URL:er som du uttryckligen optimerar ändras. För URL:er med distribuerade möjligheter får AI-agenter den optimerade versionen. För URL-adresser utan distribuerade möjligheter proxyanpassar vår tjänst helt enkelt originalsidan i befintligt skick utan att tillämpa ändringar eller lagra den i vårt optimeringslager för cache. På så sätt kan du selektivt driftsätta optimeringar utan att påverka resten av webbplatsen.
