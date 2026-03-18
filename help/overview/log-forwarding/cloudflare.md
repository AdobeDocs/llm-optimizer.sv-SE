---
title: Loggvidarebefordran - Cloudflare
description: Lär dig hur du vidarebefordrar CDN-loggar från Cloudflare till Adobe S3-bucket för insamling av AGENT-trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# Loggvidarebefordran: Cloudflare {#log-forwarding-cloudflare}

På den här sidan beskrivs hur du vidarebefordrar CDN-loggar från Cloudflare till Adobe S3-bucket för insamling av agrotiska trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När startprocessen är klar följer du de steg som anges på den här sidan för att konfigurera vidarebefordran av loggar i Cloudflare-konsolen.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På LLM Optimizer-sidan [https://llmo.now/](https://llmo.now/):

1. Gå till **instrumentpanelen för kundkonfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. Klicka på **Kom igång**.

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png) -->

1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)

1. Välj **Cloudflare (BYOCDN)**.

   ![Välj CloudFlare](/help/overview/assets/log-forwarding/cloudflare/cloudflare-select.png)

1. Klicka på **Anonboard**.

   <!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## Steg 2: Skapa ett Logpush-jobb i Cloudflare {#step-2}

Följ de här stegen på [CloudFlare-kontrollpanelen](https://dash.cloudflare.com/login):

1. Gå till sidan **Logpush** på nivån **Domän (zon)**.
1. Välj **Skapa ett loggpush-jobb**.
1. I **Välj ett mål** väljer du **Amazon S3**.
1. Ange följande målinformation:

   - **Bucket** - S3-bucket-namnet. Kopiera värdet från LLM Optimizer CDN-konfigurationssidan.

     ![Bucket-namn](/help/overview/assets/log-forwarding/common/bucket-name.png)

   - **Sökväg** - bucket-platsen i lagringsbehållaren. Kopiera värdet från LLM Optimizer CDN-konfigurationssidan.

     ![Cloudflare-sökväg](/help/overview/assets/log-forwarding/cloudflare/cloudflare-path.png)

   - **Ordna loggar i dagliga undermappar** (rekommenderas).

     ![Dagliga undermappar](/help/overview/assets/log-forwarding/cloudflare/cloudflare-daily-subfolders.png)

   - **Bucket-region** - kopiera värdet från LLM Optimizer CDN-konfigurationssidan.

     <!-- ![Region](/help/overview/assets/log-forwarding/cloudflare/cloudflare-region.png)-->

   - Om kryptering på serversidan inte behövs låter du bli omarkerad.

   När du har slutfört stegen ovan väljer du **Fortsätt**.

1. För att bevisa ägarskap skickar Cloudflare en fil till det avsedda målet. Leta reda på token genom att klicka på knappen **Öppna** på fliken **Översikt** i ägarskapsutmaningsfilen. Kopiera ägarskapstoken från LLM Optimizer CDN-konfigurationssidan och klistra in den i Cloudflare-kontrollpanelen för att verifiera din åtkomst till bucket. Ange ägarskapstoken och välj **Fortsätt**.

   <!--![Ownership token](/help/overview/assets/log-forwarding/cloudflare/cloudflare-ownership-token.png)-->

1. Välj datauppsättningen **HTTP-begäranden** som ska skickas till lagringstjänsten.

1. Konfigurera ditt loggpush-jobb:

   - Ange **jobbnamnet**.

   - I **Skicka följande fält** finns värdena på konfigurationssidan för LLM Optimizer.

     ![Loggpush-fält](/help/overview/assets/log-forwarding/cloudflare/cloudflare-logpush-fields.png)

   - **Loggformat**: JSON.

     <!--![JSON format](/help/overview/assets/log-forwarding/cloudflare/cloudflare-json-format.png)-->

1. I **Avancerade alternativ**:

   - Välj format för tidsstämpelfält i loggarna: `RFC3339`.

     ![Tidsstämpelformat](/help/overview/assets/log-forwarding/cloudflare/cloudflare-timestamp-format.png)

   - Välj **Alla loggar** för samplingsfrekvenser.

     ![Samplingsfrekvens](/help/overview/assets/log-forwarding/cloudflare/cloudflare-sampling-rate.png)

1. Välj **Skicka** när du har konfigurerat loggpush-jobbet.
