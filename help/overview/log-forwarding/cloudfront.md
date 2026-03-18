---
title: Loggvidarebefordran - CloudFront
description: Lär dig hur du vidarebefordrar CDN-loggar från CloudFront till Adobe S3-bucket för insamling av AGENT-trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 0%

---


# Loggvidarebefordran: CloudFront {#log-forwarding-cloudfront}

På den här sidan beskrivs hur du vidarebefordrar CDN-loggar från CloudFront till Adobe S3-bucket för insamling av autentiska trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När startprocessen är klar följer du stegen på den här sidan för att konfigurera vidarebefordran av loggar i kontrollpanelskonsolen för CloudFront.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På LLM Optimizer-sidan [https://llmo.now/](https://llmo.now/):

1. Gå till **instrumentpanelen för kundkonfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. Klicka på **Kom igång**.

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)

1. Ange ditt **AWS-konto**-ID.

   ![AWS konto-ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)

1. Välj **CloudFront (BYOCDN)**.

   ![Välj CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. Klicka på **Anonboard**.

   ![Knappen Inbyggt](/help/overview/assets/log-forwarding/common/onboard-button.png)

## Steg 2: Aktivera standardloggning (CloudFront-konsolen) {#step-2}

Om du vill aktivera standardloggning går du till [AWS Management Console](https://aws.amazon.com/console/):

1. Gå till [CloudFront-konsolen](https://console.aws.amazon.com/cloudfront/v4/home) och [uppdatera en befintlig distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html#HowToUpdateDistributionProcedure).

1. Öppna fliken **Loggning**.

1. Välj **Lägg till** och välj sedan den tjänst som ska ta emot loggar, i det här fallet **Amazon S3**.

1. Välj eller skapa resursen för **Mål**. Ange **bucket-namnet**, du kan kopiera värdet från LLM Optimizer CDN-konfigurationssidan.

   ![Namn på molnfrontpyts](/help/overview/assets/log-forwarding/cloudfront/cloudfront-bucket-name.png)

1. Konfigurera **Ytterligare inställningar**:

   - **Fältval** - välj loggfilsfälten. Se de obligatoriska fälten på LLM Optimizer CDN-konfigurationssidan.

     ![Markering av molnfältet ](/help/overview/assets/log-forwarding/cloudfront/cloudfront-field-selection.png)

   - **Partitionering** - kopiera **sökvägssuffixet** från LLM Optimizer konfigurationssida.

     ![CloudFront-partitionering](/help/overview/assets/log-forwarding/cloudfront/cloudfront-partitioning.png)

   - **Utdataformat** - formatet ska vara JSON.

     ![Utdataformat för CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-output-format.png)

1. Följ de här stegen för att uppdatera eller skapa distributionen.

1. Bekräfta att **Aktiverad** visas bredvid distributionen på sidan **Loggar**.

## Aktivera standardloggning för korsleverans {#cross-account}

**källkontot** (med CloudFront-distributionen) skickar åtkomstloggar till **målkontot** (S3-bucket visas på LLM Optimizer CDN-konfigurationssidan). Båda kontona måste ha rätt behörigheter.

Källkontot `111111111111` skickar till exempel loggar till en S3-bucket i målkontot `222222222222`. Du kan använda [AWS kommandoradsgränssnitt](https://aws.amazon.com/cli/).

>[!NOTE]
>
>Ersätt leveransdestinationens ARN-värde (`arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination`) med värdet för **leveransdestinationens ARN** från konfigurationssidan för LLM Optimizer med kommandona nedan.

![Leveransmål-ARN](/help/overview/assets/log-forwarding/cloudfront/cloudfront-delivery-destination-arn.png)

### Konfigurera källkontot {#source-account}

Sedan måste du konfigurera källkontot:

1. **Skapa en leveranskälla** - ersätt namnet och distributionen ARN:

   ```bash
   aws logs put-delivery-source --name s3-cf-delivery \
     --resource-arn arn:aws:cloudfront::111111111111:distribution/E1TR1RHV123ABC \
     --log-type ACCESS_LOGS
   ```

1. **Skapa leveransen** - länka källan till målet; använd mål-ARN från steget Konfigurera destinationskontot:

   ```bash
   aws logs create-delivery --delivery-source-name s3-cf-delivery \
     --delivery-destination-arn arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination
   ```

1. **Verifiera:**

   - I **source**-kontot: CloudFront-konsol > din distribution > fliken **Logging**. Under **Typ** bör du se leveransen av S3-korsloggen.
   - I **destination**-kontot: S3-konsol > Bucket. Du bör se prefixet (till exempel `MyLogPrefix`) och loggarna i den mappen.
