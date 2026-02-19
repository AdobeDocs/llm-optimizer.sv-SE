---
title: Optimera på Edge - snabbt (BYOCDN)
description: Lär dig hur du konfigurerar BYOCDN för optimering snabbt på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---


# Snabbt (BYOCDN)

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar VCL-reglerna snabbt måste du se till att du har:

* Snabb åtkomst till din domän.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

{{retrieve-byocdn-api-key}}

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

När installationen är klar kontrollerar du att både trafik dirigeras till Edge Optimize och att mänsklig trafik inte påverkas.

**1. Testa starttrafik (bör optimeras)**

Simulera en AI-robotbegäran med en agentisk användaragent:

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

Ett svar innehåller rubriken `x-edgeoptimize-request-id` som bekräftar att begäran har vidarebefordrats via Edge Optimize:

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. Testa mänsklig trafik (bör INTE påverkas)**

Simulera en vanlig webbläsarbegäran:

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

Svaret ska **inte** innehålla rubriken `x-edgeoptimize-request-id`. Sidinnehållet och svarstiden ska vara identiska med innan Optimera aktiveras på Edge.

**3. Så här skiljer du mellan de två scenarierna**

| Sidhuvud | Punkttrafik (optimerad) | Humantrafik (ej påverkad) |
|---|---|---|
| `x-edgeoptimize-request-id` | Presentera - innehåller ett unikt begärande-ID | Frånvarande |
| `x-edgeoptimize-fo` | Finns bara om redundans inträffade (värde: `1`) | Frånvarande |

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

{{return-to-overview}}
