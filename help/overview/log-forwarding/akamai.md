---
title: Loggvidarebefordran - Akamai
description: Lär dig hur du vidarebefordrar CDN-loggar från Akamai till Adobe S3-bucket för insamling av AGENT-trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---


# Loggvidarebefordran: Akamai {#log-forwarding-akamai}

På den här sidan beskrivs hur du vidarebefordrar CDN-loggar från Akamai till Adobe S3-bucket för insamling av agentiska trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När startprocessen är klar följer du stegen på den här sidan för att konfigurera vidarebefordran av loggar på Akamai-kontrollpanelen.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På LLM Optimizer-sidan [https://llmo.now/](https://llmo.now/):

1. Gå till **instrumentpanelen för kundkonfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. Klicka på **Kom igång**.

   <!--![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)

1. Välj **Akamai (BYOCDN)**.

   ![Välj Akamai](/help/overview/assets/log-forwarding/akamai/akamai-select.png)

1. Klicka på **Anonboard**.

   <!--![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## Steg 2: Skapa en ström i Akamai {#step-2}

På Akamai-kontrollpanelen [https://control.akamai.com/](https://control.akamai.com/) följer du stegen från den officiella Akamai-dokumentationen för att [skapa en ström](https://techdocs.akamai.com/datastream2/docs/create-stream).

## Steg 3: Välj dataparametrar {#step-3}

När du har skapat dataströmmen klickar du på Nästa på Akamai-kontrollpanelen för att fortsätta till fliken **Datauppsättningar**. Följ stegen från den officiella Akamai-dokumentationen för att välja [dataparametrarna](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters). Följande fält från LLM Optimizer-konfigurationen behövs:

![LLMO-konfigurationsfält](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

Mappningen ska vara följande:

* **Logginformation**
reqTimeSec -> Begärandetid
* **Geo-data**
land -> Land/region
* **Meddelandeutbytesdata**
reqHost -> Begär värd
reqPath -> Sökväg till begäran
queryStr -> Frågesträng
reqMethod -> Request method
ua -> User-Agent
statusCode -> HTTP-statuskod
rspContentType -> Response Content-Type
* **Begär rubrikdata**
reference -> Referer
* **Nätverksprestandadata**
timeToFirstByte -> Tid till första byte

Akamai-datauppsättningsfälten (inklusive ID) är följande:

1100, # reqTimeSec -> Begärandetid
2012, # country -> Country/Region
1011, # reqHost -> Begär värd
1013, # reqPath -> Sökväg för begäran
2009, # queryStr -> Frågesträng
1012, # reqMethod -> Request method
1017, # ua -> User-Agent
1008, # statusCode -> HTTP-statuskod
1032, # reference -> Referer
1016, # rspContentType -> Content-Type för svar
2025 # timeToFirstByte -> Tid till första byte

## Steg 4: Konfigurera mål {#step-4}

När du har skapat dataströmmarna och valt de parametrar som du behöver för att konfigurera målet. Så här konfigurerar du målet:

1. Välj **S3** i **Mål**.
2. Ange en läsbar beskrivning för målet i **Namn**.
3. I **Bucket** kopierar du **Bucket Name** från LLM Optimizer konfigurationssida.

   ![Bucket-namn](/help/overview/assets/log-forwarding/common/bucket-name.png)

4. Kopiera **Sökvägen** från konfigurationssidan för LLM Optimizer i **Mappsökvägen**.

   ![Sökvägskonfiguration](/help/overview/assets/log-forwarding/akamai/akamai-path-config.png)

5. I **Region** kopierar du **Region** från LLM Optimizer konfigurationssida.

   <!--![Region](/help/overview/assets/log-forwarding/common/region.png)-->

6. Kopiera båda värdena från LLM Optimizer konfigurationssida i **Åtkomstnyckel-ID** och **Hemlig åtkomstnyckel**.

   ![Åtkomstnycklar](/help/overview/assets/log-forwarding/common/access-keys.png)

7. Klicka på **Verifiera och spara** för att validera anslutningen till målet och spara den angivna informationen. Som en del av den här valideringsprocessen använder systemet den angivna åtkomstnyckelidentifieraren och den hemliga åtkomstnyckeln för att skapa en verifieringsfil i S3-mappen, med en tidsstämpel i filnamnet i formatet `Akamai_access_verification_[TimeStamp].txt`. Du kan bara se den här filen om valideringsprocessen lyckas och du har tillgång till den Amazon S3-bucket och den mapp som du försöker skicka loggar till.

8. Redigera fältet **Filnamn** på menyn **Leveransalternativ** enligt följande:

   a. Ändra **prefixet**. Kopiera värdet från LLM Optimizer konfigurationssida under **Loggfilsprefix**:

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. Ändra **suffixet**. Kopiera värdet från LLM Optimizer konfigurationssida under **Loggfilssuffix**.

9. Ändra **penselfrekvensen**. Kopiera värdet från LLM Optimizer konfigurationssida under **Loggintervall**.

   ![Loggintervall](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. Klicka på **Nästa** för att slutföra processen.

Före den slutliga valideringen bör konfigurationen se ut ungefär som i det här exemplet:

![Konfigurationsvalidering](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
