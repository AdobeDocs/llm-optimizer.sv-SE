---
title: Loggvidarebefordran - Imperva
description: Lär dig hur du vidarebefordrar CDN-loggar från Imperva till Adobe S3-bucket för insamling av AGENT-trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---


# Loggvidarebefordran: Imperva {#log-forwarding-imperva}

I den här guiden beskrivs hur du vidarebefordrar CDN-loggar från Imperva till Adobe S3-bucket för insamling av AGENT-trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När startprocessen är klar följer du stegen på den här sidan för att konfigurera vidarebefordran av loggar från Imperva-webbkonsolen.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På LLM Optimizer-sidan [https://llmo.now/](https://llmo.now/):

1. Gå till **Konfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)
1. Klicka på **Kom igång**.
1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)
1. Välj **Imperva (BYOCDN)**.

   ![Välj Imperva](/help/overview/assets/log-forwarding/imperva/imperva-select.png)
1. Klicka på **Anonboard**.

## Steg 2: Konfigurera vidarebefordran av loggar i Imperva {#step-2}

På [Imperva-konsolen](https://my.imperva.com):

>[!NOTE]
>
>Loggar bör skickas dagligen.

1. Logga in på ditt Imperva-konto på [https://my.imperva.com](https://my.imperva.com).

2. Gå till **Loggar** > **Logginställningar** (eller **Loggintegrering**) i sidofältet.

3. Välj **Amazon S3 ARN** som anslutningstyp (loggmål).

4. Ange följande:

   | Fält | Beskrivning | Anteckning |
   |---|---|---|
   | **Anslutningsnamn** | Ett beskrivande namn för anslutningen (till exempel Production S3-loggar). Du kan byta namn på standardinställningen. | |
   | **Sökväg** | Platsen för mappen där loggfilerna ska lagras. Använd formatet `<Amazon S3 bucket name>/<log folder>`. Till exempel: `MyBucket/MyImpervaLogFolder`. | `Amazon S3 bucket name` är **Bucket Name** från LLM Optimizer konfigurationssida. ![Bucket Name](/help/overview/assets/log-forwarding/imperva/imperva-bucket-name.png) Loggmappen är **Path** från LLM Optimizer konfigurationssida. ![Sökväg ](/help/overview/assets/log-forwarding/imperva/imperva-path.png) |

5. Klicka på **Testa anslutningen**. Imperva kör ett fullständigt test där en testfil (inga verkliga data) skickas till den angivna mappen och sedan tas bort när överföringen är klar.

   - **Tillgänglig** - lagringsinformationen är giltig. Du kan konfigurera loggar så att den här anslutningen används.
   - **Odefinierad** - antingen saknas nödvändig information eller så misslyckades testet.

6. Klicka på **Spara** för att spara konfigurationen.

7. Konfigurera loggalternativ (loggtyper, loggnivå, format, komprimering) och **loggnivåer**. Du kan hämta värdena från konfigurationssidan för LLM Optimizer.

   | Fält | Anteckning |
   |---|---|
   | Loggintegrationsläge | ![Loggintegrationsläge](/help/overview/assets/log-forwarding/imperva/imperva-log-integration-mode.png) |
   | Leveranssätt | ![Leveransmetod](/help/overview/assets/log-forwarding/imperva/imperva-delivery-method.png) |
   | Loggtyper | ![Loggtyper](/help/overview/assets/log-forwarding/imperva/imperva-log-types.png) |
   | Loggnivå | ![Loggnivå](/help/overview/assets/log-forwarding/imperva/imperva-log-level.png) |
   | Format | ![Format](/help/overview/assets/log-forwarding/imperva/imperva-format.png) |
   | Komprimera loggar | ![Komprimera loggar](/help/overview/assets/log-forwarding/imperva/imperva-compress-logs.png) |
