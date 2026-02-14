---
title: Optimera på Edge
description: Lär dig leverera optimeringar i LLM Optimizer i CDN-kanten utan att behöva göra några redigeringsändringar.
feature: Opportunities
source-git-commit: eb8bdf9144aebb85171a529a3cc25034be5b076e
workflow-type: tm+mt
source-wordcount: '2291'
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

* Generera en API-nyckel.
* Lägg till Optimize på Edge routningsregler i CDN.
* Tillåtslista användardefinierade sökvägar för hela domänen.
* Lägg till en användardefinierad lista med användaragenter för LLM i målgruppen.
* Kontrollera att `robots.txt` inte blockerar några användaragenter som är avsedda att vara målanvändare.
* Bekräfta Optimera vid Edge-routning i LLM Optimizer gränssnitt.

Som vägledning för konfigurationsprocessen, som presenteras nedan, är exempelkonfigurationer för ett antal CDN-inställningar. Tänk på att dessa exempel bör anpassas till den aktuella livekonfigurationen. Vi rekommenderar att du gör ändringar i de lägre miljöerna först.

>[!BEGINTABS]

>[!TAB Hanterad CDN för Adobe]

**Hanterad CDN för Adobe**

Syftet med den här konfigurationen är att konfigurera begäranden med agentiska användaragenter som dirigeras till optimeringstjänsten (`live.edgeoptimize.net` backend). Testa konfigurationen genom att söka efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

```
curl -svo page.html https://frescopa.coffee/about-us --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Cirkulationskonfigurationen görs med en [originSelector CDN-regel](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors). Förutsättningarna är följande:

* bestämma vilken domän som ska dirigeras
* bestämma vilka banor som ska dirigeras
* bestämma vilka användaragenter som ska dirigeras (rekommenderad regex)

För att kunna distribuera regeln måste du:

* skapa en [konfigurationspipeline](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/operations/config-pipeline)
* implementera konfigurationsfilen `cdn.yaml` i din databas
* köra konfigurationsflödet


```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Edge Optimize backend
  originSelectors:
    rules:
      - name: route-to-edge-optimize-backend
        when:
          allOf:
            - reqHeader: x-edgeoptimize-request
              exists: false # avoid loops when requests comes from Edge Optimize
            - reqHeader: user-agent
              matches: "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: edge-optimize-backend
    origins:
      - name: edge-optimize-backend
        domain: "live.edgeoptimize.net"
```

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo page.html https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

>[!TAB Snabbt (BYOCDN)]

**Edge Optimize BYOCDN - Fast - VCL**

![Snabbt VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![Lägg till VCL-fragment](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv-kodfragment**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-fowarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=true"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash-kodfragment**

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

**vcl_deliver snippet**

```
if (req.http.x-edgeoptimize-config && resp.status >= 400) {
  set req.http.x-edgeoptimize-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-edgeoptimize-config && req.http.x-edgeoptimize-request == "failover") {
  set resp.http.x-edgeoptimize-fo = "1";
}
```

>[!TAB Akamai (BYOCDN)]

**Edge Optimize BYOCDN - Akamai**

Syftet med den här konfigurationen är att dirigera begäranden från agentiska användaragenter till tjänsten Edge Optimize (`live.edgeoptimize.net` backend). Testa konfigurationen genom att söka efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.


**Följande Akamai Property Manager JSON-regel skickar användaragenter för LLM till Edge Optimize:**

Konfigurationen innehåller följande steg:

**1. Ange routningsvillkor (matchning av användaragent)**

![Ange routningsvillkor](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. Ange ursprung och SSL-beteende**

![Ange ursprung och SSL-beteende](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. Ange cachenyckelvariabel**

![Ange cachenyckelvariabel](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. Cachelagrar regler**

![Cachelagra regler](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. Ändra inkommande begärandehuvuden**

![Ändra rubriker för inkommande begäran](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. Ändra inkommande svarshuvuden**

![Ändra inkommande svarshuvuden](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. Ändring av cache-ID**

![Ändra cache-ID](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. Webbplatsredundans**

![Webbplatsredundans](/help/assets/optimize-at-edge/akamai-step8-failover.png)

![Redundansbeteenden](/help/assets/optimize-at-edge/akamai-step8-failover-behaviors.png)

![Redundansregler](/help/assets/optimize-at-edge/akamai-step8-failover-rules.png)

![Beteendesvar](/help/assets/optimize-at-edge/akamai-step8-behaviors-response.png)

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo page.html https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

>[!ENDTABS]

>[!NOTE]
>Kontakta `llmo-at-edge@adobe.com` om du vill hjälpa dina IT-/CDN-team med att komma igång för andra CDN-leverantörer. När konfigurationerna är klara kan du driftsätta förslag för Optimera på Edge i LLM Optimizer.

## Möjligheter

I följande tabell visas möjligheter som kan förbättra den agentiska webbupplevelsen och stöds av Optimize på Edge.

| Möjligheter | Typ | Identifiera automatiskt | Föreslå automatiskt | Automatisk optimering |
|---------|----------|----------|----------|----------|
| Återskapa innehållets synlighet | Teknisk GEO | Identifierar sidor där kritiskt innehåll är dolt för AI-agenter. Visar URL:er som påverkas och förväntat innehåll som kan återställas. | Markerar innehåll som kan göras tillgängligt för AI-agenter och rekommenderar att förrendering aktiveras för dessa sidor. | Skapar en helåtergiven, AI-vänlig HTML-ögonblicksbild av den autentiska trafik som återställer det tidigare dolda innehållet. |
| Optimera rubriker för budskap om livslångt lärande | Optimering av innehåll | Söker igenom rubriker för att upptäcka tomma, duplicerade, saknade eller tvetydiga rubriker som kan minska maskinläsbarheten. | Föreslår en renare rubrikhierarki och förbättrade etiketter och visar en förhandsvisning av den uppdaterade strukturen för varje sida. | Infogar den förbättrade rubrikstrukturen för AI-agenter och bevarar den visuella designen samtidigt som sidan blir mer läsbar för LLM-program. |
| Lägg till LLM-vänliga sammanfattningar | Optimering av innehåll | Identifierar långa eller komplexa sidor som saknar koncisa sammanfattningar på sid- eller avsnittsnivå, vilket gör dem svårare för AI att snabbt skanna och förstå. | Rekommenderar korta, AI-genererade sammanfattningar på sid- och avsnittsnivå som hämtar nyckelinnehåll. | Infogar sammanfattningarna i de relevanta HTML-avsnitten, vilket förbättrar hur modellerna tolkar och beskriver sidinnehållet. |
| Lägg till relevanta frågor | Optimering av innehåll | Identifierar mellanrum i det befintliga sidinnehållet som skulle kunna dra nytta av vanliga frågor. | Föreslår AI-genererat FAQ-innehåll som är anpassat till användaravsikten och befintliga ämnen. | Inför innehåll med vanliga frågor i HTML, vilket gör sidorna mer identifierbara och relevanta i AI-drivna svar. |
| Förenkla komplext innehåll | Optimering av innehåll | Flaggar sidor med komplex text som kan förhindra AI-förståelsen. | Innehåller AI-genererade förenklade versioner av komplex text samtidigt som den ursprungliga innebörden bevaras. | Skriver om komplexa avsnitt på sidan, vilket förbättrar AI-läsbarheten. |

### Ytterligare verktyg

[Adobe LLM Optimizer: Kan din webbsida redigeras?Tillägget &#x200B;](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome visar hur mycket av ditt webbsidesinnehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration.

Med ett enda klick kan du utvärdera vilken dator som kan läsas på en webbplats. Du kan visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad människor ser och uppskatta hur mycket innehåll som kan återställas med hjälp av LLM Optimizer. Se [Kan AI läsa din webbplats?](https://business.adobe.com/se/blog/introducing-the-llm-optimizer-chrome-extension) sida för mer information.

## Detaljerade affärsmöjligheter

I följande avsnitt kan du visa ytterligare information för varje affärsmöjlighet som stöds av Optimize på Edge.

### Återskapa innehållets synlighet

Den här affärsmöjligheten flaggar sidor där nyckelinnehåll döljs för AI-agenter på grund av klientåtergivning. För varje identifierad sida visar den exakt vilket innehåll som saknas i AI-agentvyn, markerar luckor i synligheten och gör det möjligt att direkt tillämpa ändringar för att återskapa det dolda innehållet. När du distribuerar denna möjlighet med Optimize på Edge får LLM-användaragenterna en förrenderad, AI-optimerad version av sidan så att de kan komma åt den fullständiga kontexten utan att köra Javascript.
Detta garanterar att sidan först är helt synlig för AI-agenter. Ytterligare förbättringar tillämpas ovanpå den förrenderade HTML.

>[!IMPORTANT]
>Denna förrenderingsfunktion gäller automatiskt alla möjligheter som presenteras nedan när den används med Optimera på Edge för att säkerställa att sidan är helt synlig för AI-agenter.

### Optimera rubriker för budskap om livslångt lärande

Denna möjlighet identifierar sidor där rubrikstrukturen gör det svårt för AI-agenter att förstå sidan på grund av tomma, duplicerade, saknade eller tvetydiga rubriker. För varje berörd sida kommer affärsmöjligheten att visa de underoptimala rubrikerna och rekommenderar en tydligare hierarki. När de används med Optimera i Edge används de förbättrade rubrikerna i HTML för agell trafik. Detta gör att datorn blir läsbar samtidigt som den mänskliga motstående layouten förblir densamma.

### Lägg till LLM-vänliga sammanfattningar

Denna möjlighet identifierar sidor som kan dra nytta av koncisa sammanfattningar för att hjälpa webbdesigners att snabbt förstå vad sidinnehållet handlar om. För varje sida identifierar affärsmöjligheten var en sammanfattning behövs och skapar AI-genererade sammanfattningar på sidnivå eller avsnittsnivå. När du distribuerar med Optimera på Edge infogas dessa sammanfattningar i HTML som AI-agenter hämtar, vilket förbättrar chanserna att ditt innehåll beskrivs mer korrekt.

### Lägg till relevanta frågor

Den här affärsmöjligheten flaggar sidor där ytterligare frågor och svar-innehåll bättre kan matcha användaravsikten och frågar vid AI-driven identifiering. För varje sida föreslås AI-genererade FAQ-block kopplade till användaravsikt och innehåll på sidan. Med Optimize på Edge injiceras dessa frågor och svar i HTML, vilket gör sidan mer AI-vänlig och ökar sannolikheten för att AI-svaren direkt speglar din vägledning.

### Förenkla komplext innehåll

Här hittar du sidor med långa, komplexa stycken som kan minska förståelsen av AI. För varje sida som överskrider läsbarhetströskeln skapas AI-genererat innehåll som är enklare och enklare att läsa in samtidigt som den ursprungliga innebörden bevaras. När innehållet distribueras i framkant hjälper det förenklade innehållet i den autentiska trafiken att tolka och sammanfatta innehållet på ett trognare sätt.

## Optimera automatiskt på Edge

För varje affärsmöjlighet kan du förhandsgranska, redigera, driftsätta, visa direkt och återställa optimeringarna.

>[!VIDEO](https://video.tv.adobe.com/v/3477988/?captions=swe&learn=on&enablevpops)

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

Vi levererar den optimerade versionen av sidan från cache så länge som den underliggande källsidan inte har ändrats. Men när källan ändras uppdateras vårt system automatiskt så att AI-agenter alltid får det senaste innehållet. Det beror på att vi använder låg cachetid för att göra inställningar (i minutordning) så att alla innehållsuppdateringar på webbplatsen utlöser en ny optimering i det fönstret. <!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

F. Optimerar Edge endast för webbplatser som använder Adobe Edge Delivery Service (EDS)?

Nej. Optimera på Edge är CDN-agnostiker och fungerar med alla frontendarkitekturer, inte bara de som används i Adobe EDS Stack.

F. Hur skiljer sig Optimera vid Edge-förrendering från traditionell SSR-rendering (server-side rendering)?

Båda problemen kan lösas och fungera tillsammans. Traditionell SSR återger innehåll på serversidan, men inkluderar inte innehåll som läses in senare i webbläsaren. Optimera vid Edge-förrendering fångar upp sidan när data från JavaScript och klientsidan har lästs in, vilket skapar den fullständigt monterade versionen vid CDN-kanten. SSR fokuserar på att förbättra människors upplevelse och på Optimera på Edge förbättrar webbupplevelsen för LLM.
