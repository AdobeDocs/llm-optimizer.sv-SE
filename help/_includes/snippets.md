---
source-git-commit: beae935e7a34f5bccbe21578fa9a928912958710
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 1%

---
# Fragment

## Verifiera installationen - Adobe hanterat CDN {#verify-setup-adobe-aem-cs-cdn}

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

## Verifiera installationen - BYOCDN {#verify-setup-byocdn}

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

## Hämtningssteg för API-nyckel {#retrieve-byocdn-api-key}

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

## Återgå till översikt {#return-to-overview}

Om du vill veta mer om Optimera på Edge, inklusive tillgängliga möjligheter, automatiska optimeringsarbetsflöden och vanliga frågor och svar, går du tillbaka till [Optimera på Edge-översikt](/help/dashboards/optimize-at-edge.md).
