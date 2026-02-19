---
title: Optimera på Edge - Akamai (BYOCDN)
description: Lär dig hur du konfigurerar Akamai BYOCDN för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '587'
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

Konfigurationen för växling vid fel på plats har två delar: failover-funktionen (konfigurerad inuti den huvudsakliga routningsregeln för optimering på plats) och en separat huvudregel för redundanstestning.

**9a. Beteende vid växling vid fel på plats (i den huvudsakliga routningsregeln för optimering vid gräns)**

Inuti huvudroutningsregeln konfigurerar du beteendet Webbplatsväxling vid fel och det avancerade XML-fragmentet enligt följande:

![Webbplatsredundans](/help/assets/optimize-at-edge/akamai-step9-failover.png)

Lägg till begärandehuvudet `x-edgeoptimize-request` med värdet `fo` via avancerad XML:

```
<forward:availability.fail-action2>
<add-header>
<status>on</status>
<name>x-edgeoptimize-request</name>
<value>fo</value>
</add-header>
</forward:availability.fail-action2>
```

![Redundansbeteenden](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

**9b. Regel för testhuvud för växling vid fel (jämställd)**

>[!IMPORTANT]
>
>Skapa regeln **EdgeOptimize Failover - Test Header** som en **jämställd** (på samma nivå) för routningsreglerna - **inte** kapslad i dem. I Akamai Property Manager-regelträdet ska hierarkin se ut så här:
>
>```
>▼ Parent Rule
>   ▶ Optimize at Edge Routing     ← routing rule
>       EdgeOptimize Failover - Test Header       ← sibling, same level
>```
>
>Detta garanterar att huvudregeln för redundanstestning utvärderas för **alla**-routningsregler, inte bara en.

Om begärandehuvudet `x-edgeoptimize-request` är `fo` anger du det utgående svarshuvudet `x-edgeoptimize-fo` till `true`.

![Redundansregler](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

Om Edge Optimize returnerar ett fel av typen `4XX` eller `5XX` dirigeras begäran automatiskt tillbaka till ditt standardursprung så att slutanvändaren fortfarande får ett svar.

| Scenario | Beteende |
| --- | --- |
| Edge Optimize returnerar `2XX` | Klienten får optimerat svar. |
| Edge Optimize returnerar `4XX` eller `5XX` | Begäran dirigeras tillbaka till standardkällan. |

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
