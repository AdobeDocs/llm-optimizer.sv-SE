---
title: Optimera på Edge - CloudFront (BYOCDN)
description: Lär dig hur du konfigurerar CloudFront BYOCDN för optimering på Edge i LLM Optimizer.
feature: Opportunities
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '2265'
ht-degree: 0%

---


# CloudFront (BYOCDN)

Den här konfigurationen dirigerar agens-trafik (begäranden från AI-bots och LLM-användaragenter) till Edge Optimize-backend-tjänsten (`live.edgeoptimize.net`). Besökare på människor och SEO-botar fortsätter att betjänas som vanligt utifrån ditt ursprung. Om du vill testa konfigurationen söker du efter rubriken `x-edgeoptimize-request-id` i svaret när konfigurationen är klar.

**Förutsättningar**

Innan du konfigurerar CloudFront bör du kontrollera att du har:

* En befintlig CloudFront-distribution som betjänar din webbplats.
* AWS IAM-behörigheter för att skapa Lambda-funktioner, IAM-roller, CloudFront-distributioner och cacheprinciper.
* LLM Optimizer introduktionsprocess har slutförts.
* CDN-loggen har vidarebefordrats till LLM Optimizer.
* En Edge Optimize API-nyckel har hämtats från LLM Optimizer användargränssnitt.
* (Valfritt) En mellanlagringsnyckel för Edge Optimize API om du först testar routning på ett mellanlagringsvärdnamn.

{{retrieve-byocdn-api-key}}

{{retrieve-staging-edge-optimize-api-key}}

**Steg 1: Skapa Edge Optimera ursprung**

**Navigering:** AWS Console > CloudFront > Distributioner > [Din distribution] > fliken Original

1. Klicka på **Skapa ursprung**.

2. Konfigurera ursprung:
   * **Ursprunglig domän:** `live.edgeoptimize.net`
   * **Namn:** `EdgeOptimize_Origin`

3. Lämna alla andra fält med sina standardvärden.

4. Lägg till anpassade rubriker:

   | Sidhuvud | Värde |
   |--------|-------|
   | `x-edgeoptimize-api-key` | Din API-nyckel |
   | `x-forwarded-host` | `www.example.com` |

   Ersätt `www.example.com` med den faktiska webbplatsdomänen och `Your API key` med API-nyckeln för Edge Optimize som tillhandahålls av din Adobe-representant.

5. Klicka på **Skapa ursprung**.

![Skapa molnstartpunkt](/help/assets/optimize-at-edge/cloudfront-origin-creation.png)

**Steg 2: Skapa funktion för visningsprogrambegäran**

**Navigering:** AWS Console > CloudFront > Funktioner

1. Klicka på **Skapa funktion**.

2. Konfigurera:
   * **Namn:** `edgeoptimize-routing`
   * **Körningsmiljö:** `cloudfront-js-2.0`

3. Ersätt standardkoden med koden från [viewer-request.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/cloudfront-function/viewer-request.js).

   Anpassa följande värden i koden före publicering:

   * `YOUR_DEFAULT_ORIGIN` - Ersätt med namnet på ditt befintliga standardursprung (finns i CloudFront > Distribution > [Din distribution] > fliken Original).
   * `TARGETED_PATHS` - Ange som `null` om du vill ange alla HTML-sidor som mål eller ange en array med specifika sökvägar, till exempel `['/', '/products', '/about']`.

4. Klicka på **Spara ändringar** > **Publiceringsfunktion**.

![Skapa molnfunktion](/help/assets/optimize-at-edge/cloudfront-function-creation.png)


**Steg 3: Konfigurera cacheprincip**

**Navigering:** AWS Console > CloudFront > Distribution > [Din distribution] > Beteenden

Kontrollera den cacheprincip som är kopplad till ditt beteende. Klicka på **Redigera** om ditt beteende och titta i avsnittet **Cachenyckel och ursprungsförfrågningar** för att identifiera ditt scenario:

* **Scenario A (äldre):** En alternativknapp med namnet **Äldre cacheinställningar** visas markerad. Det finns ingen listruta för principnamn - i stället visas den infogade TTL- och rubrikinställningarna.
* **Scenario B (anpassad princip):** Du ser **Cacheprincipen** markerad med ett principnamn som du eller ditt team har skapat (inte en princip som tillhandahålls av AWS).
* **Scenario C (hanterad princip):** Du ser **Cacheprincipen** markerad med ett namn från AWS som `CachingOptimized`, `CachingDisabled` eller `CachingOptimizedForUncompressedObjects` - dessa kan inte redigeras.

**Scenario A: Äldre cacheinställningar**

Om ditt beteende använder äldre cacheinställningar:

1. Under **Cachenyckel och ursprungsbegäran** visas **Äldre cacheinställningar** markerade.

2. Lägg till `x-edgeoptimize-config` och `x-edgeoptimize-url` i tillåtelselista för **rubriker**:

   * Välj **Ta med följande rubriker** i listrutan.
   * Lägg till `x-edgeoptimize-config` och `x-edgeoptimize-url`.
     ![Cloudfront Cache Legacy](/help/assets/optimize-at-edge/cloudfront-cache-policy-legacy.png)

   Om du redan har markerat **Alla** i listrutan Sidhuvuden hoppar du över det här steget - alla sidhuvuden vidarebefordras automatiskt till startpunkten.

3. Kontrollera inställningen för **objektcache**:

   * Om värdet är **Anpassa** rekommenderar vi att du anger **Minsta TTL** till `0`. Om din nuvarande lägsta TTL är mycket kort kanske du inte behöver ändra den.
   * Om värdet är **Använd rubriker i ursprungscache** - ingen ändring behövs.

4. Klicka på **Spara ändringar**.

**Scenario B: Inte äldre med en anpassad cacheprincip**

Om ditt beteende redan använder en anpassad cacheprincip (en som du har skapat, inte en princip som hanteras av AWS):

**Navigering:** AWS Console > CloudFront > Principer > Cache

1. Klicka på din befintliga profil.

2. Klicka på **Redigera**.

3. Du bör ange **Minsta TTL** till `0`. Om din nuvarande lägsta TTL är mycket kort kanske du inte behöver ändra den.
   ![TTL-inställningar för cacheprincip](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

4. Lägg till `x-edgeoptimize-config` och `x-edgeoptimize-url` under **Inställningar för cachenycklar** > **Sidhuvuden**, tillsammans med dina befintliga inkluderingar.   > Sidhuvuden.
   ![Policyrubriker för cache](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

5. Klicka på **Spara ändringar**.

**Scenario C: Icke-äldre med en hanterad (AWS) cacheprincip**

Om ditt beteende använder en AWS-hanterad cacheprincip (till exempel `CachingOptimized`) kan du inte redigera den. Du måste skapa en ny anpassad cacheprincip som replikerar de befintliga inställningarna för den hanterade profilen och lägger till Edge Optimize-rubrikerna överst.

**Del 1: Anteckna dina aktuella inställningar för principer för hanterad cache**

**Navigering:** AWS Console > CloudFront > Principer > Cache

1. Sök efter och klicka på den hanterade cacheprincipen som är kopplad till ditt beteende (till exempel `CachingOptimized`).

2. Observera alla befintliga inställningar:
   * Minsta TTL, Maximal TTL, Standard TTL
   * Rubriker som ingår i cachenyckeln
   * Cookies som ingår i cachenyckeln
   * Frågesträngar som ingår i cachenyckeln
   * Komprimeringsstöd (Gzip, Brotli)

**Del 2: Skapa en ny anpassad cacheprincip med samma inställningar + Edge Optimize-konfigurationen**

**Navigering:** AWS Console > CloudFront > Principer > Cache

1. Klicka på **Skapa cacheprincip**.

2. **Namn:** `edgeoptimize-cache`

   ![Cacheprincipnamn](/help/assets/optimize-at-edge/cloudfront-cache-policy-name.png)

3. Replikera alla inställningar som anges i del 1 med följande ändringar:

   * Du bör ange **Minsta TTL** till `0`. Om din nuvarande lägsta TTL är mycket kort kanske du inte behöver ändra den.

   ![TTL-inställningar för cacheprincip](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

   * Under **Inställningar för cachenycklar** > **Huvuden** inkluderar du allt som den hanterade principen hade, plus lägg till `x-edgeoptimize-config` och `x-edgeoptimize-url`.

   ![Policyrubriker för cache](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

4. Klicka på **Skapa**.

5. Gå tillbaka till ditt beteende och koppla den nya profilen:

   **Navigering:** AWS Console > CloudFront > Distribution > [Din distribution] > Beteenden

   1. Redigera ditt beteende.
   2. Välj **Cacheprincip** under **Cachenyckel- och ursprungsförfrågningar**.
   3. Välj `edgeoptimize-cache` i listrutan.
   4. Klicka på **Spara ändringar**.

**Steg 4: Skapa Lambda@Edge (ursprunglig begäran och svar)**

>[!IMPORTANT]
>Lambda@Edge funktioner **måste skapas i regionen `us-east-1` (N. Virginia)**. Detta är ett krav från AWS. Även om funktionen har skapats i `us-east-1`, replikerar AWS den automatiskt till alla platser i CloudFront Edge världen över, så den körs på den närmaste kantpositionen till visningsprogrammet. Kontrollera att du befinner dig i regionen `us-east-1` i AWS Console innan du fortsätter.

**Skapa Lambda-funktionen**

**Navigering:** AWS Console > Lambda

1. Klicka på **Skapa funktion**.

2. Välj **Författare från grunden**.

3. Konfigurera:
   * **Funktionsnamn:** `edgeoptimize-origin`
   * Lämna alla andra fält med sina standardvärden.

4. Klicka på **Skapa funktion**.

5. Ersätt standardkoden med koden från [origin-request-response.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/origin-request-response.js) i kodredigeraren.

6. Klicka på **Distribuera** för att spara koden.

7. Observera det **körningsrollnamn** som visas under **Konfiguration** > **Behörigheter** (till exempel `edgeoptimize-origin-role-xxxxx`). Du behöver detta i följande steg.

**Uppdatera körningsrollens förtroendeprincip**

Den automatiskt skapade rollen litar bara på `lambda.amazonaws.com`. För Lambda@Edge måste du även lägga till `edgelambda.amazonaws.com`.

**Navigering:** AWS Console > IAM > Roller > [din roll från föregående steg] > Fliken Pålitlighetsrelationer

1. Klicka på **Redigera förtroendeprincip**.

2. Ersätt profilen med innehållet från [trust-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/trust-policy.json).

3. Klicka på **Uppdatera princip**.

>[!WARNING]
>Tjänsthuvudkontot `edgelambda.amazonaws.com` är **obligatoriskt** för Lambda@Edge. Utan det kan CloudFront inte anropa funktionen på kantplatser.

**Åtgärda behörighetsprincipen CloudWatch-loggar**

Rollen som skapas automatiskt har en `AWSLambdaBasicExecutionRole`-princip som är konfigurerad för Lambda som har fel region och logggruppsnamn för Lambda@Edge. Du måste uppdatera den.

**Navigering:** AWS Console > IAM > Roller > [din roll] > fliken Behörigheter > klicka på det bifogade principnamnet (till exempel `AWSLambdaBasicExecutionRole-xxxx`)

1. Klicka på **Redigera**.

2. Ersätt profilen med innehållet från [cloudwatch-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/cloudwatch-policy.json).

   I JSON ersätter du `ACCOUNT_ID` med ditt faktiska AWS-konto-ID (finns i det övre högra hörnet av AWS Console) och `FUNCTION_NAME` med namnet på din Lambda-funktion (till exempel `edgeoptimize-origin`).

3. Klicka på **Spara ändringar**.

>[!WARNING]
>Regionen i ARN måste vara `*` - Lambda@Edge körs vid den närmaste kantpositionen för visningsprogrammet, så loggar skrivs till CloudWatch i kantplatsens region (till exempel `ap-south-1`, `eu-west-1`), inte nödvändigtvis `us-east-1`. Logggruppen använder ett regionsprefixnamn: `/aws/lambda/us-east-1.FUNCTION_NAME`, där `us-east-1` alltid är funktionens hemregion.

**Publicera en version**

1. På funktionssidan klickar du på **Åtgärder** (överst till höger) > **Publicera ny version**.

2. Lägg till en beskrivning.

3. Klicka på **Publicera**.
   ![Lambda-publicering](/help/assets/optimize-at-edge/cloudfront-lambda-publish.png)

4. Kopiera eller anteckna nedåt i **funktionen ARN** - du behöver det här i nästa steg.
   ![Lambda ARN](/help/assets/optimize-at-edge/cloudfront-lambda-arn.png)

**Steg 5: Associera funktionerna och cacheprincipen med beteendet**

**Navigering:** AWS Console > CloudFront > Distribution > [Din distribution] > Beteenden

1. Redigera ditt beteende.

2. Om du skapade en ny cacheprincip i steg 3 (scenario C) anger du **cacheprincipen** till `edgeoptimize-cache`.
   ![Cacheprincip](/help/assets/optimize-at-edge/cloudfront-behaviour-cache.png)

3. Konfigurera under **Funktionsassociationer**:

   * **Visningsprogramförfrågan:** `edgeoptimize-routing`
   * **Ursprungsbegäran:** Versionerad funktion ARN från steg 4 (i **Publicera en version**)
   * **Ursprungligt svar:** Versionerad funktion ARN från steg 4 (i **Publicera en version**)

   ![Konfiguration av funktionsassociationer](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. Klicka på **Spara ändringar**.

**Steg 6: Testa konfigurationen**

**1. Testa starttrafik (bör optimeras)**

Skicka en begäran med en agent för båda. På den **första begäran** kan Edge Optimize returnera ett proxiderat (ej optimerat) svar medan det bearbetar och cachelagrar sidan. Du kan identifiera detta med rubriken `x-edgeoptimize-proxy: 1` i svaret.

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

**4. Mellanlagringsdomän (valfritt)**

Om du använder ett mellanlagringsvärdnamn och en mellanlagrings-API-nyckel från LLM Optimizer ska du distribuera samma CloudFront-konfiguration på din **mellanlagringsdistribution** med API-nyckeln **staging**. Verifiera sedan starttrafiken på mellanlagringsvärden:

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

Ersätt `https://staging.example.com/page.html` med din faktiska mellanlagrings-URL och sökväg. Ett godkänt svar innehåller rubriken `x-edgeoptimize-request-id`.

{{verify-routing-status-in-ui}}

**5. Verifiera att loggarna flödar korrekt**

När du har kört testförfrågningarna ovan kontrollerar du att loggarna skrivs för både funktionen CloudFront och funktionen Lambda@Edge.

*Funktionsloggar för CloudFront (`edgeoptimize-routing`)*

**Navigering:** AWS Console > CloudWatch > Logggrupper (i `us-east-1` eller den region där din CloudFront-distribution är konfigurerad)

1. Sök efter en logggrupp med namnet `/aws/cloudfront/function/edgeoptimize-routing`.
2. Öppna den senaste loggströmmen.
3. För autentiska förfrågningar bör du se bl.a. följande:
   * `Adding origin group for userAgent: chatgpt-user`
   * `Routing to Edge Optimize origin for userAgent: chatgpt-user`
4. För icke-autentiska förfrågningar bör du se:
   * `Routing to Default origin for userAgent: ...`

Du kan även kontrollera fliken **Metrisk** under **AWS Console > CloudFront > Funktioner > edgeoptimize-routing** för att visa antalet anrop och felfrekvensen.

*Lambda@Edge loggar (`edgeoptimize-origin`)*

>[!IMPORTANT]
>Lambda@Edge loggar skrivs till CloudWatch i **regionen på kantplatsen** som betjänade begäran, inte `us-east-1`. Kontrollera CloudWatch i den AWS-region som ligger närmast den plats där du körde kommandot curl.

**Navigering:** AWS Console > CloudWatch > Logggrupper (kontrollera att du är i rätt region)

1. Sök efter en logggrupp med namnet `/aws/lambda/us-east-1.edgeoptimize-origin`.
2. Öppna den senaste loggströmmen.
3. För autentiska förfrågningar bör du se bl.a. följande:
   * `Calling Edge Optimize Origin for agentic requests` - primär sökväg
   * `Calling Default Origin in case of failover for agentic requests` - redundanssökväg
   * `Failover Triggered for agentic requests` - identifiering av redundans för ursprungligt svar

Om logggruppen inte finns kontrollerar du att IAM-behörigheterna uppdaterades korrekt i steg 4. Kontrollera även andra närliggande AWS-regioner - den kantplats som betjänade din begäran kan skilja sig från vad du förväntar dig.

**Felsökning**

| Problem | Möjlig orsak | Lösning |
|-------|----------------|----------|
| Inget `x-edgeoptimize-request-id` som svar på giltiga begäranden | Origin-gruppen dirigerar inte till Edge Optimize | Kontrollera att `YOUR_DEFAULT_ORIGIN` har ersatts korrekt i CloudFront-funktionskoden (steg 2). |
| 403 fel på autentiska begäranden | Ogiltig eller saknad API-nyckel | Kontrollera rubrikvärdet `x-edgeoptimize-api-key` i anpassade rubriker för Edge Optimize origin (steg 1). |
| Det går inte att hitta CloudWatch-loggar för Lambda@Edge | Fel IAM-behörigheter | Kontrollera att behörighetsprincipen för CloudWatch-loggar har uppdaterats (steg 4). Obs! Lambda@Edge loggar visas i den kantregion som betjänade begäran, inte nödvändigtvis `us-east-1`. |
| Cache stöder inte `cache-control: no-store` | Minsta TTL kan vara för hög | Ange lägsta TTL till `0` i cacheprincipen (steg 3). Om din lägsta TTL redan är mycket kort kanske detta inte är problemet. |
| Regelbunden (icke-agentisk) trafik bruten efter konfiguration | Felkonfiguration av cacheprincip | Om du har skapat en ny cacheprincip (scenario C) måste du kontrollera att du har replikerat alla inställningar från den ursprungliga hanterade principen. |

**Inaktivera och återaktivera optimering på Edge**

Funktionen Lambda@Edge (`edgeoptimize-origin`) är kopplad till händelserna origin request och origin response i ditt CloudFront-beteende. Eftersom det går i linje med alla förfrågningar som skickas genom det beteendet - både mänskligt och agentiskt - kommer ett Lambda@Edge att påverka all livstrafik, inte bara autentiska förfrågningar. Om du upptäcker ett Lambda@Edge-avbrott tar du bort funktionsassociationerna omedelbart för att återställa det normala trafikflödet till standardstartpunkten.

**Identifiera ett Lambda@Edge**

* **AWS tjänsthälsoinstrumentpanel** - Kontrollera [AWS tjänsthälsoinstrumentpanel](https://health.aws.amazon.com/health/status) för att se om det finns några aktiva incidenter som påverkar **Amazon CloudFront** eller **AWS Lambda**. Ett globalt eller regionalt driftstopp som rapporteras här är det snabbaste sättet att bekräfta att problemet finns på AWS infrastruktursida snarare än i din konfiguration.
* **Lambda@Edge fel** — Navigera till **AWS Console > CloudFront > Övervakning > [Din distribution]**. Öppna fliken **Lambda@Edge fel** och kontrollera om diagrammet **Körningsfel** innehåller körningsfel. Om de här är höga kan det hända att Lambda@Edge är nere.

**Kopplar loss funktionen Lambda@Edge**

**Navigering:** AWS Console > CloudFront > Distribution > [Din distribution] > Beteenden

1. Klicka på **Redigera** om ditt beteende.

2. Bläddra ned till avsnittet **Funktionsassociationer**.

3. Ange följande associationer till **Ingen association**:

   | Händelse | Ändra till |
   |---|---|
   | Begäran om visningsprogram | Ingen association |
   | Ursprungsbegäran | Ingen association |
   | Ursprungligt svar | Ingen association |

   ![Konfiguration av funktionsassociationer](/help/assets/optimize-at-edge/cloudfront-no-function-association.png)

4. Klicka på **Spara ändringar**.

5. Vänta tills distributionen av CloudFront är klar. Statusen ändras från **Distribuerar** till det senast ändrade datumet, vanligtvis inom några minuter.

Alla trafikvägar direkt till standardkällan efter distributionen. Ingen konfiguration tas bort. Lambda-funktionen och dess associationer kan återställas när som helst.

**Kopplar om funktionen Lambda@Edge**

**Navigering:** AWS Console > CloudFront > Distribution > [Din distribution] > Beteenden

1. Klicka på **Redigera** om ditt beteende.

2. Bläddra ned till avsnittet **Funktionsassociationer**.

3. Återställ associationerna:

   | Händelse | Ange till |
   |---|---|
   | Begäran om visningsprogram | `edgeoptimize-routing` (funktionen CloudFront) |
   | Ursprungsbegäran | Versionerat Lambda ARN från steg 4 |
   | Ursprungligt svar | Versionerat Lambda ARN från steg 4 |

   Använd den version av **funktionen ARN** som du angav i steg 4 (i **Publicera en version**).

   ![Konfiguration av funktionsassociationer](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. Klicka på **Spara ändringar**.

5. Vänta tills distributionen är klar och kontrollera sedan att autentiska begäranden returnerar rubriken `x-edgeoptimize-request-id` enligt beskrivningen i steg 6.

{{return-to-overview}}
