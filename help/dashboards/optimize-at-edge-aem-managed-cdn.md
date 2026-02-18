---
title: Optimera på Edge - AEM Cloud Service Managed CDN (snabbt)
description: Lär dig hur du konfigurerar AEM Cloud Service Managed CDN (snabbt) för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 8cdd15413555057165f69ea4d5a15b243ab9098d
workflow-type: tm+mt
source-wordcount: '329'
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

Om du föredrar att konfigurera routningen själv via Cloud Manager Pipeline följer du stegen nedan. Cirkulationskonfigurationen görs med en [originSelector CDN-regel](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors). Förutsättningarna är följande:

* Bestäm vilken domän som ska dirigeras.
* Bestäm vilka banor som ska dirigeras.
* Bestäm vilka användaragenter som ska dirigeras (rekommenderad regex).

För att kunna distribuera regeln måste du:

* Skapa en [konfigurationspipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/config-pipeline).
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

{{verify-setup-adobe-aem-cs-cdn}}

{{return-to-overview}}
