---
title: Loggvidarebefordran - annan (manuell överföring)
description: Lär dig hur du manuellt överför CDN-loggar till Adobe S3-bucket för insamling av agentiska trafikdata i LLM Optimizer när du använder en CDN-provider som inte stöds.
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 0%

---


# Loggvidarebefordran: Annan (manuell överföring) {#log-forwarding-other}

Etableringsmetoden **Annan BYOCDN** är ett alternativ som fångar upp alla kunder som vill tillhandahålla CDN-loggar till LLM Optimizer när:

- **Manuella överföringar** är att föredra - till exempel exporterar projektteamen loggar och överför dem regelbundet.
- **Ad-hoc-automatiserade processer** används - engångskript, schemalagd export, serverlösa jobb.
- Kunden använder ett **CDN som inte stöds internt** av de inbyggda integreringarna för vidarebefordran av loggar.

Den här metoden imiterar modellen för kontinuerlig vidarebefordring: loggarna produceras och överförs till den förväntade S3-platsen och bearbetas sedan automatiskt av matningsledningarna.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På [LLM Optimizer](https://llmo.now/):

1. Gå till **Konfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. Klicka på **Kom igång**.

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)

1. Välj **Annan**.

   ![Välj annan](/help/overview/assets/log-forwarding/other/other-select.png)

1. Klicka på **Anonboard**.

<!--   ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## Steg 2: Förbered och överföra loggar {#step-2}

### Obligatoriskt loggformat (JSON-rader) {#log-format}

Loggar måste laddas upp som en nyradsavgränsad JSON (**ett JSON-objekt per rad**). Varje loggrad måste innehålla följande fält **exakt enligt anvisningarna nedan**.

#### Fältvis schema {#schema}

| Fält | Typ | Beskrivning | Exempel |
|---|---|---|---|
| **tidsstämpel** | Sträng | Tidsstämpeln för begäran som följer formatet **ISO 8601**. | `"2025-02-01T23:00:05Z"` |
| **värd** | Sträng | Den webbdomän som klienten begärde. | `"www.example.com"` |
| **url** | Sträng | **path** och **query-parametrarna** krävs, men domänen ska **inte** inkluderas. | `"/home?utm_source=google"` |
| **request_method** | Sträng | Metoden för HTTP-begäran, kallas ibland för HTTP-verb. | `"GET"` |
| **request_user_agent** | Sträng | HTTP User-Agent-begärandehuvudet. | `"Mozilla/5.0 (compatible; GPTBot/1.0"` |
| **request_reference** | Sträng | Rubriken för HTTP-referensbegäran (kan vara tom). | `"https://chatgpt.com"` |
| **response_status** | Heltal | Statuskoden för HTTP-svar. | `200` |
| **response_content_type** | Sträng | Svarshuvudet HTTP Content-Type. | `"text/html; charset=utf-8"` |
| **time_to_first_byte** | Heltal | Tiden mellan att skapa en anslutning till servern och att hämta innehållet på en webbsida i **millisekunder**. Ange noll om okänd eller otillgänglig. | `42` |

#### Exempel på loggrader {#example}

I följande exempel visas tre loggrader:

```json
{"timestamp":"2025-02-01T23:06:14Z","host":"www.example.com","url":"/products/llm-optimizer?utm_source=google","request_method":"GET","request_user_agent":"Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":198}
{"timestamp":"2025-02-01T23:19:32Z","host":"www.example.com","url":"/services/ai-consulting/overview","request_method":"GET","request_user_agent":"PerplexityBot/1.0 (+https://www.perplexity.ai/perplexitybot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":255}
{"timestamp":"2025-02-01T23:44:05Z","host":"www.example.com","url":"/products/pricing/enterprise?utm_medium=social","request_method":"GET","request_user_agent":"ClaudeBot/1.0 (+https://www.anthropic.com)","response_status":200,"request_referer":"","response_content_type":"application/pdf","time_to_first_byte":312}
```

### Kritisk ansvarsfriskrivning (stavning och typer) {#disclaimer}

Injektions- och aggregeringsledningarna är strikta för **fältnamn och datatyper**.

- Fältnamn måste matcha **exakt** (skiftläge och stavning).
- Datatyperna ska vara korrekta enligt följande:
   - **timestamp** måste vara en sträng med formatet **ISO 8601**. UNIX-liknande tidsstämplar fungerar kanske inte.
   - **response_status** måste vara ett heltal.
   - **time_to_first_byte** måste vara ett heltal och använda millisekunder.
   - Strängar måste vara giltiga JSON-strängar.
- Felformaterad JSON eller saknade/felaktiga fält kan göra att loggar hoppas över eller inte kan tolkas, vilket kan leda till att data saknas i rapporterna.

### Överför plats och bearbetningsgräns {#upload-location}

#### Sökvägsregel {#path-rule}

Överför loggar under rätt mappsökväg med formatet **`yyyy/mm/dd/`** (med snedstreck).

En exempellogg från 1 feb 2025 UTC: `ABC123AdobeOrg/raw/byocdn-other/2025/02/01/`

#### Bearbetar regel {#processing-rule}

- Loggar som har överförts under en angiven **UTC-dag** bearbetas av pipelines **nära slutet av den UTC-dagen** (daglig körning).
- Loggar som överförts till **föregående dagars mappar** (bakåtfyllnad) **upptäcks och bearbetas** inom 24 timmar.

## Scenarier {#scenarios}

### Scenario 1: Loggar in Splunk/Elasticsearch - exportera och ladda upp till S3 {#scenario-splunk}

**Mål**: Hämta loggar från befintliga observerbarhetsplattformar och leverera dem till S3-platsen.

- Extrahera de obligatoriska fälten från Splunk/Elastic Search-händelser.
- Omvandla varje händelse till ett JSON-objekt som följer schemat ovan (JSON-rader).
- Överför de resulterande filerna till angiven S3-bucket och **aktuell UTC-dag**-sökväg: `…/byocdn-other/yyyy/mm/dd/`
- Loggarna bearbetas automatiskt i slutet av UTC-dagen.

### Scenario 2: Lambda/Azure Function - format och överföring till S3 {#scenario-serverless}

**Mål**: Använd serverlös beräkning för att hämta/hämta CDN-loggar, normalisera dem och leverera dem till S3-platsen.

- Funktionen hämtar loggar från kundens källa (loggbutik, kö, blobblagring osv.).
- Funktionen mappar fält till det förväntade schemat och genererar **JSON-rader**.
- Funktionen överför utdata till: `…/byocdn-other/yyyy/mm/dd/`
- Loggarna bearbetas automatiskt i slutet av UTC-dagen.

## Snabb checklista {#checklist}

- **Ett JSON-objekt per rad** (JSON-rader)
- **Exakt fältstavning** enligt angiven
- Korrigera datatyper
- **time_to_first_byte** i millisekunder (heltal)
- Överför till lämplig UTC-mapp: **ååå/mm/dd/** under **byocdn-other**
