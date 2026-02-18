---
title: Optimera på Edge - Akamai (BYOCDN)
description: Lär dig hur du konfigurerar Akamai BYOCDN för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 2%

---


# Akamai (BYOCDN)

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar Akamai Property Manager-reglerna bör du kontrollera att du har:

* Åtkomst till Akamai Property Manager för din domän.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.

{{retrieve-byocdn-api-key}}

**Konfiguration**

Följande Akamai Property Manager-regel skickar användaragenter för LLM till Edge Optimize. Konfigurationen innehåller följande steg:

**1. Ange routningsvillkor (matchning av användaragent)**

Ange routning för följande användaragenter :image.png

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

{{verify-setup-byocdn}}

{{return-to-overview}}
