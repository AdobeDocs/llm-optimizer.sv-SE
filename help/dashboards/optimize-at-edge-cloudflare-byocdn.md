---
title: Optimera på Edge - Cloudflare (BYOCDN)
description: Lär dig hur du konfigurerar Cloudflare BYOCDN för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '1250'
ht-degree: 0%

---


# Cloudflare (BYOCDN)

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar Cloudflare Worker-routningsreglerna måste du se till att du har:

* Ett CloudFlare-konto med arbetare aktiverade på din domän.
* Åtkomst till domänens DNS-inställningar i CloudFlare.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

{{retrieve-byocdn-api-key}}

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

{{verify-setup-byocdn}}

{{return-to-overview}}
