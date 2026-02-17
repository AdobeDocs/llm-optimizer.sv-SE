---
title: Optimera på Edge
description: Lär dig leverera optimeringar i LLM Optimizer i CDN-kanten utan att behöva göra några redigeringsändringar.
feature: Opportunities
source-git-commit: ae37ef578f279eae6ea51fd8aed5c6b91c8e1088
workflow-type: tm+mt
source-wordcount: '4843'
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

Som vägledning för konfigurationsprocessen, som presenteras nedan, är exempelkonfigurationer för ett antal CDN-inställningar. Tänk på att dessa exempel bör anpassas till den aktuella livekonfigurationen. Vi rekommenderar att du gör ändringar i de lägre miljöerna först.

>[!BEGINTABS]

>[!TAB Hanterad CDN för tjänsten AEM Cloud (snabbt)]

**Edge Optimize - AEM Cloud Service Managed CDN (snabbt)**

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Så här börjar du dirigera agrotisk trafik till Edge Optimize:

1. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

   ![Navigera till Kundkonfiguration](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. Markera kryssrutan **Distribuera optimeringar till AI-agenter** under **AI-trafikroutning för att distribuera optimeringar**. Adobe-teamet hanterar routningskonfigurationen åt dig.

   ![Tick Deploy Optimizations to AI Agents](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. När du har aktiverat kryssrutan visas statusen för inställningen. Adobe-teamet kommer att slutföra routningskonfigurationen åt dig.

   ![Konfigurering av AI-trafik pågår](/help/assets/optimize-at-edge/prereq-traffic-routing-progress.png)

   När routningen är konfigurerad och aktiv uppdateras statusen till en grön bock som anger att routning har aktiverats. Du behöver inte göra något mer.

Om du behöver hjälp med ovanstående steg kontaktar du ditt Adobe-kontoteam eller `llmo-at-edge@adobe.com`.

**Självbetjäningsroutning via Cloud Manager Pipeline**

Om du föredrar att konfigurera routningen själv via Cloud Manager Pipeline följer du stegen nedan. Cirkulationskonfigurationen görs med en [originSelector CDN-regel](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors). Förutsättningarna är följande:

* Bestäm vilken domän som ska dirigeras.
* Bestäm vilka banor som ska dirigeras.
* Bestäm vilka användaragenter som ska dirigeras (rekommenderad regex).

För att kunna distribuera regeln måste du:

* Skapa en [konfigurationspipeline](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/operations/config-pipeline).
* Genomför konfigurationsfilen `cdn.yaml` i din databas.
* Kör konfigurationsflödet.

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

**Verifiera installationen**

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

>[!TAB Snabbt (BYOCDN)]

**Edge Optimize - Fast (BYOCDN)**

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar VCL-reglerna snabbt måste du se till att du har:

* Snabb åtkomst till din domän.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

**Steg för att hämta API-nyckeln:**

1. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

   ![Navigera till Kundkonfiguration](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. Markera kryssrutan **Distribuera optimeringar till AI-agenter** under **AI-trafikroutning för att distribuera optimeringar**.

   ![Tick Deploy Optimizations to AI Agents](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. Kopiera API-nyckeln och fortsätt med stegen för routningskonfiguration nedan.

   ![Kopiera API-nyckeln](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >I det här skedet kan statusen visa ett rött kryss som anger att installationen inte har slutförts än. Detta förväntas - när du har slutfört routningskonfigurationen nedan och AI-robottrafiken börjar flöda uppdateras statusen till en grön bock som bekräftar att routning har aktiverats.

Om du behöver hjälp med ovanstående steg kontaktar du ditt Adobe-kontoteam eller `llmo-at-edge@adobe.com`.

**Konfiguration**

Lägg till följande tre VCL-kodfragment till tjänsten Snabbt. Dessa fragment hanterar cirkulerande autentiska förfrågningar till Edge Optimize, cachenyckelseparation och failover till din standardursprung.

![Snabbt VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![Lägg till VCL-fragment](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv-kodfragment**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
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

**Redundans**

`vcl_deliver`-fragmentet hanterar automatiskt växling vid fel. Om Edge Optimize returnerar ett `4XX`- eller `5XX`-fel startas begäran om och dirigeras tillbaka till ditt standardursprung så att slutanvändaren fortfarande får ett svar. Redundanssvar innehåller rubriken `x-edgeoptimize-fo: 1`.

| Scenario | Beteende |
| --- | --- |
| Edge Optimize returnerar `2XX` | Klienten får optimerat svar. |
| Edge Optimize returnerar `4XX` eller `5XX` | Begäran startas om och hanteras från standardkällan. |
| Redundanssvar | Innehåller rubriken `x-edgeoptimize-fo: 1`. |

**Verifiera installationen**

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Akamai (BYOCDN)]

**Edge Optimize - Akamai (BYOCDN)**

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar Akamai Property Manager-reglerna bör du kontrollera att du har:

* Åtkomst till Akamai Property Manager för din domän.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

**Steg för att hämta API-nyckeln:**

1. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

   ![Navigera till Kundkonfiguration](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. Markera kryssrutan **Distribuera optimeringar till AI-agenter** under **AI-trafikroutning för att distribuera optimeringar**.

   ![Tick Deploy Optimizations to AI Agents](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. Kopiera API-nyckeln och fortsätt med stegen för routningskonfiguration nedan.

   ![Kopiera API-nyckeln](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >I det här skedet kan statusen visa ett rött kryss som anger att installationen inte har slutförts än. Detta förväntas - när du har slutfört routningskonfigurationen nedan och AI-robottrafiken börjar flöda uppdateras statusen till en grön bock som bekräftar att routning har aktiverats.

Om du behöver hjälp med ovanstående steg kontaktar du ditt Adobe-kontoteam eller `llmo-at-edge@adobe.com`.

**Konfiguration**

Följande Akamai Property Manager-regel skickar användaragenter för LLM till Edge Optimize. Konfigurationen innehåller följande steg:

**1. Ange routningsvillkor (matchning av användaragent)**

Ange routning för följande användaragenter:

```
 *AdobeEdgeOptimize-AI*,
 *ChatGPT-User*,
 *GPTBot*,
 *OAI-SearchBot*,
 *PerplexityBot*,
 *Perplexity-User*
```

![Ange routningsvillkor](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. Ange ursprung och SSL-beteende**

Ange ursprung som `live.edgeoptimize.net` och matcha SAN till `*.edgeoptimize.net`

![Ange ursprung och SSL-beteende](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. Ange cachenyckelvariabel**

Ange cachenyckelvariabeln `PMUSER_EDGE_OPTIMIZE_CACHE_KEY` till `LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![Ange cachenyckelvariabel](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. Cachelagrar regler**

![Cachelagra regler](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. Ändra inkommande begärandehuvuden**

Ange följande rubriker för inkommande begäran:
`x-edgeoptimize-api-key` till API-nyckeln som hämtats från LLMO
`x-edgeoptimize-config` to `LLMCLIENT=TRUE;`
`x-edgeoptimize-url` to `{{builtin.AK_URL}}`

![Ändra rubriker för inkommande begäran](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. Ändra inkommande svarshuvuden**

![Ändra inkommande svarshuvuden](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. Ändring av cache-ID**

![Ändra cache-ID](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. Ändra rubriker för utgående begäran**

Ange `x-forwarded-host`-rubriken till `{{builtin.AK_HOST}}`

![Ändra rubriker för utgående begäran](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. Webbplatsredundans**

![Webbplatsredundans](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![Redundansbeteenden](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

![Redundansregler](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

Om Edge Optimize returnerar ett fel av typen `4XX` eller `5XX` dirigeras begäran automatiskt tillbaka till ditt standardursprung så att slutanvändaren fortfarande får ett svar.

| Scenario | Beteende |
| --- | --- |
| Edge Optimize returnerar `2XX` | Klienten får optimerat svar. |
| Edge Optimize returnerar `4XX` eller `5XX` | Begäran dirigeras tillbaka till standardkällan. |

**Verifiera installationen**

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Cloudflare (BYOCDN)]

**Edge Optimize - Cloudflare (BYOCDN)**

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar Cloudflare Worker-routningsreglerna måste du se till att du har:

* Ett CloudFlare-konto med arbetare aktiverade på din domän.
* Åtkomst till domänens DNS-inställningar i CloudFlare.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

**Steg för att hämta API-nyckeln:**

1. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

   ![Navigera till Kundkonfiguration](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. Markera kryssrutan **Distribuera optimeringar till AI-agenter** under **AI-trafikroutning för att distribuera optimeringar**.

   ![Tick Deploy Optimizations to AI Agents](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. Kopiera API-nyckeln och fortsätt med stegen för routningskonfiguration nedan.

   ![Kopiera API-nyckeln](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >I det här skedet kan statusen visa ett rött kryss som anger att installationen inte har slutförts än. Detta förväntas - när du har slutfört routningskonfigurationen nedan och AI-robottrafiken börjar flöda uppdateras statusen till en grön bock som bekräftar att routning har aktiverats.

Om du behöver hjälp med ovanstående steg kontaktar du ditt Adobe-kontoteam eller `llmo-at-edge@adobe.com`.

**Så här fungerar routning**

När den är korrekt konfigurerad fångas en begäran till din domän (till exempel `www.example.com/page.html`) från en autentisk användaragent upp av Cloudflare Worker och dirigeras till Edge Optimize-backend. Backend-begäran innehåller de huvuden som krävs.

**Testar backend-begäran**

Du kan verifiera routningen genom att göra en direkt begäran till Edge Optimize-backend.

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

**Obligatoriska rubriker**

Följande huvuden måste anges på begäranden till Edge Optimize-backend:

| Sidhuvud | Beskrivning | Exempel |
|--------|-------------|---------|
| `x-forwarded-host` | Den ursprungliga värddatorn för begäran. Krävs för att identifiera webbplatsdomänen. | `www.example.com` |
| `x-edgeoptimize-url` | Den ursprungliga URL-sökvägen och frågesträngen för begäran. | `/page.html` eller `/products?id=123` |
| `x-edgeoptimize-api-key` | Den API-nyckel som tillhandahålls av Adobe för din domän. | `your-api-key-here` |
| `x-edgeoptimize-config` | Konfigurationssträng för cachenyckeldifferentiering. | `LLMCLIENT=TRUE;` |

**Steg 1: Skapa molnarbetare**

1. Logga in på din Cloudflare-kontrollpanel.
2. Navigera till **Arbetare och sidor** i sidlisten.
3. Klicka på **Skapa program** och sedan på **Skapa arbetare**.
4. Namnge din arbetare (till exempel `edge-optimize-router`).
5. Klicka på **Distribuera** för att skapa arbetaren med standardkoden.

![Kontrollpanelen för CloudFlow-arbetare](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

**Steg 2: Lägg till arbetarkoden**

När du har skapat arbetaren klickar du på **Redigera kod** och ersätter standardkoden med följande:

```javascript
/**
 * Edge Optimize BYOCDN - Cloudflare Worker
 *
 * This worker routes requests from agentic bots (AI/LLM user agents) to the
 * Edge Optimize backend service for optimized content delivery.
 *
 * Features:
 * - Routes agentic bot traffic to Edge Optimize backend
 * - Failover to origin on Edge Optimize errors (any 4XX or 5XX errors)
 * - Loop protection to prevent infinite redirects
 * - Human visitors and SEO bots are served from the origin as usual
 */

// List of agentic bot user agents to route to Edge Optimize
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User'
];

// Targeted paths for Edge Optimize routing
// Set to null to route all HTML pages, or specify an array of paths
const TARGETED_PATHS = null; // e.g., ['/', '/page.html', '/products']

// Failover configuration
// Failover on any 4XX client error or 5XX server error from Edge Optimize
const FAILOVER_ON_4XX = true; // Failover on any 4XX error (400-499)
const FAILOVER_ON_5XX = true; // Failover on any 5XX error (500-599)

export default {
  async fetch(request, env, ctx) {
    return await handleRequest(request, env, ctx);
  },
};

async function handleRequest(request, env, ctx) {
  const url = new URL(request.url);
  const userAgent = request.headers.get("user-agent")?.toLowerCase() || "";

  // Check if request is already processed (loop protection)
  const isEdgeOptimizeRequest = !!request.headers.get("x-edgeoptimize-request");

  // Construct the original path and query string
  const pathAndQuery = `${url.pathname}${url.search}`;

  // Check if the path matches HTML pages (no extension or .html extension)
  const isHtmlPage = /(?:\/[^./]+|\.html|\/)$/.test(url.pathname);

  // Check if path is in targeted paths (if specified)
  const isTargetedPath = TARGETED_PATHS === null
    ? isHtmlPage
    : (isHtmlPage && TARGETED_PATHS.includes(url.pathname));

  // Check if user agent is an agentic bot
  const isAgenticBot = AGENTIC_BOTS.some((ua) => userAgent.includes(ua.toLowerCase()));

  // Route to Edge Optimize if:
  // 1. Request is NOT already from Edge Optimize (prevents infinite loops)
  // 2. User agent matches one of the agentic bots
  // 3. Path is targeted for optimization
  if (!isEdgeOptimizeRequest && isAgenticBot && isTargetedPath) {

    // Build the Edge Optimize request URL
    const edgeOptimizeURL = `https://live.edgeoptimize.net${pathAndQuery}`;

    // Clone and modify headers for the Edge Optimize request
    const edgeOptimizeHeaders = new Headers(request.headers);

    // Remove any existing Edge Optimize headers for security
    edgeOptimizeHeaders.delete("x-edgeoptimize-api-key");
    edgeOptimizeHeaders.delete("x-edgeoptimize-url");
    edgeOptimizeHeaders.delete("x-edgeoptimize-config");

    // x-forwarded-host: The original site domain
    // Use environment variable if set, otherwise use the request host
    edgeOptimizeHeaders.set("x-forwarded-host", env.EDGE_OPTIMIZE_TARGET_HOST ?? url.host);

    // x-edgeoptimize-api-key: Your Adobe-provided API key
    edgeOptimizeHeaders.set("x-edgeoptimize-api-key", env.EDGE_OPTIMIZE_API_KEY);

    // x-edgeoptimize-url: The original request URL path and query
    edgeOptimizeHeaders.set("x-edgeoptimize-url", pathAndQuery);

    // x-edgeoptimize-config: Configuration for cache key differentiation
    edgeOptimizeHeaders.set("x-edgeoptimize-config", "LLMCLIENT=TRUE;");

    try {
      // Send request to Edge Optimize backend
      const edgeOptimizeResponse = await fetch(new Request(edgeOptimizeURL, {
        headers: edgeOptimizeHeaders,
        redirect: "manual", // Preserve redirect responses from Edge Optimize
      }), {
        cf: {
          cacheEverything: true, // Enable caching based on origin's cache-control headers
        },
      });

      // Check if we need to failover to origin
      const status = edgeOptimizeResponse.status;
      const is4xxError = FAILOVER_ON_4XX && status >= 400 && status < 500;
      const is5xxError = FAILOVER_ON_5XX && status >= 500 && status < 600;

      if (is4xxError || is5xxError) {
        console.log(`Edge Optimize returned ${status}, failing over to origin`);
        return await failoverToOrigin(request, env, url);
      }

      // Return the Edge Optimize response
      return edgeOptimizeResponse;

    } catch (error) {
      // Network error or timeout - failover to origin
      console.log(`Edge Optimize request failed: ${error.message}, failing over to origin`);
      return await failoverToOrigin(request, env, url);
    }
  }

  // For all other requests (human users, SEO bots), pass through unchanged
  return fetch(request);
}

/**
 * Failover to origin server when Edge Optimize returns an error
 * @param {Request} request - The original request
 * @param {Object} env - Environment variables
 * @param {URL} url - Parsed URL object
 */
async function failoverToOrigin(request, env, url) {
  // Build origin URL
  const originURL = `${url.protocol}//${env.EDGE_OPTIMIZE_TARGET_HOST}${url.pathname}${url.search}`;

  // Prepare headers - clean Edge Optimize headers and add loop protection
  const originHeaders = new Headers(request.headers);
  originHeaders.set("Host", env.EDGE_OPTIMIZE_TARGET_HOST);
  originHeaders.delete("x-edgeoptimize-api-key");
  originHeaders.delete("x-edgeoptimize-url");
  originHeaders.delete("x-edgeoptimize-config");
  originHeaders.delete("x-forwarded-host");
  originHeaders.set("x-edgeoptimize-request", "fo");

  // Create and send origin request
  const originRequest = new Request(originURL, {
    method: request.method,
    headers: originHeaders,
    body: request.body,
    redirect: "manual",
  });

  const originResponse = await fetch(originRequest);

  // Add failover marker header to response
  const modifiedResponse = new Response(originResponse.body, {
    status: originResponse.status,
    statusText: originResponse.statusText,
    headers: originResponse.headers,
  });
  modifiedResponse.headers.set("x-edgeoptimize-fo", "1");
  return modifiedResponse;
}
```

Klicka på **Spara och distribuera** för att publicera arbetaren.

![Kodredigeraren för Cloudflare Worker](/help/assets/optimize-at-edge/cloudflare-worker-editor.png)

**Steg 3: Konfigurera miljövariabler**

Miljövariabler lagrar känslig konfiguration som din API-nyckel säkert.

1. Navigera till **Inställningar** > **Variabler** i arbetarens inställningar.
2. Klicka på **Lägg till variabel** under **Miljövariabler**.
3. Lägg till följande variabler:

   | Variabelnamn | Beskrivning | Obligatoriskt |
   |---------------|-------------|----------|
   | `EDGE_OPTIMIZE_API_KEY` | Din Adobe-medföljande Edge Optimize API-nyckel. | Ja |
   | `EDGE_OPTIMIZE_TARGET_HOST` | Målvärden för Edge Optimize-begäranden (skickas som `x-forwarded-host`-huvud) och ursprungsdomänen för redundans. Måste vara domänen utan protokoll (till exempel `www.example.com`, inte `https://www.example.com`). | Ja |

4. För API-nyckeln klickar du på **Kryptera** för att lagra den säkert.
5. Klicka på **Spara och distribuera**.

![CloudFlare-miljövariabler](/help/assets/optimize-at-edge/cloudflare-env-variables.png)

**Steg 4: Lägg till en väg till din domän**

Så här aktiverar du arbetaren på din domän:

1. Gå till din arbetares **inställningar** > **Utlösare**.
2. Klicka på **Lägg till flöde** under **Routningar**.
3. Ange ditt domänmönster (till exempel `www.example.com/*` eller `example.com/*`).
4. Välj din zon i listrutan.
5. Klicka på **Spara**.

Du kan också konfigurera vägar på zonnivå:

1. Navigera till din domän i Cloudflare.
2. Gå till **Arbetsvägar**.
3. Klicka på **Lägg till flöde** och ange mönster och arbetare.

![Cloudflare Worker-vägar](/help/assets/optimize-at-edge/cloudflare-worker-routes.png)

**Steg 5: Verifiera installationen**

Efter distributionen testar du konfigurationen genom att göra en begäran med en autentisk användaragent.

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

Ett godkänt svar innehåller rubriken `x-edgeoptimize-request-id`:

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

Du kan även verifiera att normal trafik fortsätter att fungera:

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0"
```

Den här begäran ska hanteras från ditt ursprung utan rubriken `x-edgeoptimize-request-id`.

**Verifierar failover-beteende**

Om Edge Optimize inte är tillgängligt eller returnerar ett fel, kommer arbetaren automatiskt att gå över till ditt ursprung. Redundanssvar innehåller rubriken `x-edgeoptimize-fo`:

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

Du kan övervaka failover-händelser i loggar för Cloudflare-arbetare för att felsöka problem.

**Förstå arbetslogiken**

Cloudflare Worker implementerar följande logik:

1. **Identifiering av användaragent:** Kontrollerar om användaragenten för den inkommande begäran matchar någon av de definierade officiella boten (skiftlägesokänslig).

2. **Sökvägsmål:** Du kan också filtrera begäranden baserat på målsökvägar. Som standard dirigeras alla HTML-sidor (URL:er som slutar med `/`, inget tillägg eller `.html`). Du kan ange specifika sökvägar med arrayen `TARGETED_PATHS`.

3. **Loopskydd:** Rubriken `x-edgeoptimize-request` förhindrar oändliga loopar. När Edge Optimize skickar tillbaka begäranden till ditt ursprung anges rubriken till `"1"`, och arbetaren skickar begäran genom begäran utan att vidarebefordra den till Edge Optimize.

4. **Huvudsäkerhet:** Innan du anger Edge Optimize-huvuden tar arbetaren bort alla befintliga `x-edgeoptimize-*` huvuden från den inkommande begäran för att förhindra huvudinmatningsattacker.

5. **Mappning av sidhuvud:** Arbetaren anger de sidhuvuden som krävs för Edge Optimize:
   * `x-forwarded-host` - Identifierar den ursprungliga webbplatsdomänen.
   * `x-edgeoptimize-url` - Bevarar den ursprungliga sökvägen och frågesträngen för begäran.
   * `x-edgeoptimize-api-key` - Autentiserar begäran med Edge Optimize.
   * `x-edgeoptimize-config` - Tillhandahåller cachenyckelkonfiguration.

6. **Redundanslogik:** Om Edge Optimize returnerar en felstatuskod (4XX klientfel eller 5XX serverfel) eller om begäran misslyckas på grund av ett nätverksfel, misslyckas arbetaren automatiskt över till ditt ursprung med `EDGE_OPTIMIZE_TARGET_HOST`. Redundanssvaret innehåller rubriken `x-edgeoptimize-fo: 1` som anger att redundans inträffade.

7. **Omdirigeringshantering:** Med alternativet `redirect: "manual"` ser du till att omdirigeringssvar från Edge Optimize skickas till klienten utan att arbetaren följer dem.

**Anpassa konfigurationen**

Du kan anpassa arbetsbeteendet genom att ändra konfigurationskonstanterna högst upp i koden:

**Agentrobotlista**

Ändra `AGENTIC_BOTS`-arrayen för att lägga till eller ta bort användaragenter:

```javascript
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User',
  // Add additional user agents as needed
  'ClaudeBot',
  'Anthropic-AI'
];
```

**Målsökvägar**

Som standard dirigeras alla HTML-sidor till Edge Optimize. Om du vill begränsa routning till specifika sökvägar ändrar du `TARGETED_PATHS`-arrayen:

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

**Konfiguration för växling vid fel**

Som standard misslyckas arbetaren med alla 4XX- eller 5XX-fel från Edge Optimize. Anpassa detta beteende:

```javascript
// Default: failover on any 4XX or 5XX error
const FAILOVER_ON_4XX = true;
const FAILOVER_ON_5XX = true;

// Failover only on 5XX server errors (not 4XX client errors)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = true;

// Disable automatic failover (not recommended)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = false;
```

**Viktiga överväganden**

* **Redundansbeteende:** Arbetaren övergår automatiskt till ditt ursprung om Edge Optimize returnerar ett fel (4XX- eller 5XX-statuskoder) eller om begäran misslyckas på grund av ett nätverksfel. Redundans använder `EDGE_OPTIMIZE_TARGET_HOST` som ursprungsdomän (liknande Fastly `F_Default_Origin` eller CloudFront `Default_Origin`). Redundanssvar innehåller rubriken `x-edgeoptimize-fo: 1` som du kan använda för övervakning och felsökning.

* **Cachelagring:** Cloudflare cachelagrar svar baserat på URL-adressen som standard. Eftersom agens trafik tar emot annat innehåll än mänsklig trafik bör du kontrollera dina cachekonfigurationskonton för detta. Använd cache-API eller cacherubriker för att differentiera cachelagrat innehåll. Rubriken `x-edgeoptimize-config` ska inkluderas i cachenyckeln.

* **Hastighetsbegränsning:** Övervaka din Edge Optimera-användning och överväg att implementera hastighetsbegränsning för autentisk trafik om det behövs.

* **Testar:** Testa alltid konfigurationen i en staging-miljö innan du distribuerar till produktion. Verifiera att både den autentiska och den mänskliga trafiken beter sig som förväntat. Testa failover-beteendet genom att simulera Edge Optimize-fel.

* **Loggning:** Aktivera loggning av Cloudflare-arbetare för att övervaka förfrågningar och felsöka problem. Navigera till **Arbetare** > **din arbetare** > **Loggar** om du vill visa realtidsloggar. Arbetaren loggar redundanshändelser i felsökningssyfte.

**Felsökning**

| Problem | Möjlig orsak | Lösning |
|-------|----------------|----------|
| Inget `x-edgeoptimize-request-id`-huvud i svaret | Worker-vägen matchar inte, eller så finns inte användaragenten i listan med giltiga botar. | Kontrollera att ruttmönstret matchar begärande-URL:en. Kontrollera att användaragenten finns i `AGENTIC_BOTS`-arrayen. |
| 401- eller 403-fel från Edge Optimize | Ogiltig eller saknad API-nyckel. | Kontrollera att `EDGE_OPTIMIZE_API_KEY` har angetts korrekt i miljövariabler. Kontakta Adobe för att bekräfta att API-nyckeln är aktiv. |
| Oändliga omdirigeringar eller slingor | Loopskyddshuvudet är inte inställt eller korrekt markerat. | Kontrollera att rubrikkontrollen `x-edgeoptimize-request` är på plats. |
| Infektion hos människan | Worker-routningslogiken är för bred. | Kontrollera att logiken för matchning av användaragent är korrekt och skiftlägeskänslig. Kontrollera att `TARGETED_PATHS` har konfigurerats korrekt. |
| Långsamma svarstider | Nätverksfördröjning till Edge Optimize-backend. | Detta förväntas för den första begäran. Efterföljande begäranden cachelagras på Edge Optimize. |
| `x-edgeoptimize-fo: 1`-huvud i svar | Edge Optimize returnerade ett fel och en växling till fel inträffade. | Kontrollera loggarna för Cloudflare Workers för den specifika felkoden. Verifiera Edge Optimize-tjänstens status med Adobe. |
| Redundans fungerar inte | Redundansflaggor är inaktiverade eller fel i redundanslogiken. | Verifiera att `FAILOVER_ON_4XX` och `FAILOVER_ON_5XX` är inställda på `true`. Sök efter felmeddelanden i arbetarloggarna. |
| Vissa banor optimeras inte | Sökvägen matchar inte målsökvägarna eller HTML sidmönster. | Kontrollera att sökvägen är i `TARGETED_PATHS` (om den anges) och matchar HTML sidans regex-mönster. |
| Misslyckade begäranden med ogiltig värd | `EDGE_OPTIMIZE_TARGET_HOST` innehåller protokoll (till exempel `https://`). | Använd bara domännamnet utan protokoll (till exempel `example.com`, inte `https://example.com`). |
| 530-fel vid växling vid fel | Cloudflare kan inte ansluta till ursprungsläget, eller så har redundansbegäran ogiltiga huvuden. | Kontrollera att failover-funktionen tar bort Edge Optimize-huvuden. Kontrollera att du kan komma åt källan och att DNS är korrekt konfigurerat. |

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

[Adobe LLM Optimizer: Kan din webbsida redigeras?](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome-tillägget visar hur mycket av webbsidans innehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration.

Med ett enda klick kan du utvärdera vilken dator som kan läsas på en webbplats. Du kan visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad människor ser och uppskatta hur mycket innehåll som kan återställas med hjälp av LLM Optimizer. Ser du [Kan AI läsa din webbplats?](https://business.adobe.com/se/blog/introducing-the-llm-optimizer-chrome-extension) sida för mer information.

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

Vi levererar den optimerade versionen av sidan från cacheminnet så länge som den underliggande källsidan inte har ändrats. Men när källan ändras för **Återskapa innehållets synlighet** uppdateras systemet automatiskt så att AI-agenter alltid får det senaste innehållet. Det beror på att vi använder låg cachetid för att göra inställningar (i minutordning) så att alla innehållsuppdateringar på webbplatsen utlöser en ny optimering i det fönstret. För innehållsmöjligheter som **Lägg till vänliga LLM-sammanfattningar** övervakar LLM Optimizer källsidan för ändringar. Om en ändring upptäcks pausar vi optimeringen och flaggar den för mänsklig granskning för att förhindra att innehållet växlar mellan den agentsynliga sidan och den mänskligt synliga sidan.
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

F. Optimerar Edge endast för webbplatser som använder Adobe Edge Delivery Service (EDS)?

Nej. Optimera på Edge är CDN-agnostiker och fungerar med alla frontendarkitekturer, inte bara de som används i Adobe EDS Stack.

F. Hur skiljer sig Optimera vid Edge-förrendering från traditionell SSR-rendering (server-side rendering)?

Båda problemen kan lösas och fungera tillsammans. Traditionell SSR återger innehåll på serversidan, men inkluderar inte innehåll som läses in senare i webbläsaren. Optimera vid Edge-förrendering fångar upp sidan när data från JavaScript och klientsidan har lästs in, vilket skapar den fullständigt monterade versionen vid CDN-kanten. SSR fokuserar på att förbättra människors upplevelse och på Optimera på Edge förbättrar webbupplevelsen för LLM.

F. Vad händer om jag distribuerar optimeringar för vissa URL:er i min domän, men inte för alla?

Endast de URL:er som du uttryckligen optimerar ändras. För URL:er med distribuerade möjligheter får AI-agenter den optimerade versionen. För URL-adresser utan distribuerade möjligheter proxyanpassar vår tjänst helt enkelt originalsidan i befintligt skick utan att tillämpa ändringar eller lagra den i vårt optimeringslager för cache. På så sätt kan du selektivt driftsätta optimeringar utan att påverka resten av webbplatsen.
