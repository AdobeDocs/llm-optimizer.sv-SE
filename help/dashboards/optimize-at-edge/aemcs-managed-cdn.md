---
title: Optimera på Edge - AEM Cloud Service Managed CDN (snabbt)
description: Lär dig hur du konfigurerar AEM Cloud Service Managed CDN (snabbt) för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---


# Hanterad CDN för tjänsten AEM Cloud (snabbt)

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

![AI-trafikroutningsstatus med routning aktiverat](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

{{return-to-overview}}
