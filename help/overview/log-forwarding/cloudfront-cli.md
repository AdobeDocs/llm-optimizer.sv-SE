---
title: Loggvidarebefordran - CloudFront (AWS CLI)
description: Vidarebefordra CloudFront CDN loggar till Adobe S3-bucket med hjälp av AWS CLI för leveranskonfiguration och drift.
feature: Agentic Traffic
source-git-commit: 3277e7f7f2e0c5e4693e40473d595b12d9e5f2e8
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---


# Loggvidarebefordran: CloudFront (AWS CLI) {#log-forwarding-cloudfront-cli}

På den här sidan finns information om hur du vidarebefordrar CDN-loggar från CloudFront till Adobe S3-bucket för insamling av agrotiska trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När startprocessen är klar följer du stegen på den här sidan för att konfigurera vidarebefordran av loggar med hjälp av [AWS Command Line Interface](https://aws.amazon.com/cli/) i [steg 2](#step-2-cli).

>[!NOTE]
>
> I den här guiden beskrivs hur du konfigurerar vidarebefordran av loggar med hjälp av [AWS kommandoradsgränssnitt](https://aws.amazon.com/cli/). Om du vill konfigurera vidarebefordran av loggar med hjälp av **gränssnittet i molnet** läser du [Loggvidarebefordran: CloudFront](/help/overview/log-forwarding/cloudfront.md).

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

<!--  ![AWS Account ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)-->

1. Välj **CloudFront (BYOCDN)**.

   ![Välj CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. Klicka på **Anonboard**.

<!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## Steg 2: Konfigurera CDN-loggvidarebefordran med AWS CLI {#step-2-cli}

Konfigurera CDN-loggvidarebefordran med AWS CLI enligt följande:

### Konfigurera AWS CLI-autentiseringsuppgifter

Konfigurera MAC för AWS CLI-autentiseringsuppgifter. Öppna ~/.aws/credentials och ange värdena för variablerna nedan:

```text
[LLMO]
aws_access_key_id=<VALUE_OF_ACCESS_KEY_ID>
aws_secret_access_key=<VALUE_OF_SECRET_KEY>
aws_session_token=<ONLY_IF_USING_SECURITY_TOKEN_SERVICE> ## Optional
```

### Testa anslutningen

Kör kommandot nedan för att testa anslutningen:

```bash
aws sts get-caller-identity --profile LLMO
```

Exempel på lyckade resultat:

```bash
aws sts get-caller-identity --profile LLMO
{
    "UserId": "AxxxxxxxxxxxP:user",
    "Account": "012345678912",
    "Arn": "arn:aws:sts::012345678912:assumed-role/klam-master-role-BatlY3dnPVinQLC/user"
}
```

### Initiera variabler

Ersätt `REPLACEME123@AdobeOrg` med din organisations-ID för Adobe IMS och kör kommandot nedan. Utdata-ID för det här kommandot kallas `TRANSFORM_IMS_ID`.

```bash
echo "REPLACEME123@AdobeOrg" | sed 's/@AdobeOrg$//' | tr '[:upper:]' '[:lower:]'
```

Ange värdena för `CUSTOMER`, `CDN_ID`, `ACCT1` och `TRANSFORM_IMS_ID` efter stödlinjen nedan och kör sedan kommandon från terminalen.

```bash
export PROFILE1=LLMO
export REGION1=us-east-1
export CUSTOMER=<CUSTOMER_NAME> ## No Space, user letters,numbers and dash
export CDN_ID=<YOUR_CLOUDFRONT_DISTRIBUTION_ID>
export ACCT1=<YOUR_AWS_ACCOUNT_NUMBER>
export DELIVERY_DEST_ARN=arn:aws:logs:us-east-1:640168421876:delivery-destination:cdn-logs-<TRANSFORM_IMS_ID>-ams  ## Replace TRANSFORM_IMS_ID with the output of the command above 
```

<!--Use the **Delivery destination ARN** and org values from the LLM Optimizer CDN configuration page if they differ from the pattern above.-->

### Skapa leveranskällan

Kör kommandot nedan från samma terminal där steg 3 kördes:

```bash
aws logs put-delivery-source --name llmo-cf-${CUSTOMER}-${CDN_ID} \
  --profile $PROFILE1 --region $REGION1 \
  --resource-arn arn:aws:cloudfront::${ACCT1}:distribution/${CDN_ID} \
  --log-type ACCESS_LOGS
```

>[!IMPORTANT]
>
>Om du får följande fel söker du efter den befintliga leveranskällan: *Ett fel uppstod (ConflictException) när åtgärden PutDeliverySource anropades: Detta ResourceId har redan använts i en annan Delivery Source i det här kontot.*
>
>Om du vill söka efter den befintliga leveranskällan kör du det här kommandot:
>
>```bash
>aws logs describe-delivery-sources --region us-east-1 \
>--query "deliverySources[?contains(resourceArns[0], '<CDN DistributionID>')]"
>```
>
>Använd namnet på leveranskällan från resultatet av ovanstående kommando i nästa kommando.

### Skapa leveranskonfigurationen

```bash
aws logs create-delivery \
  --profile "$PROFILE1" --region "$REGION1" \
  --delivery-source-name "llmo-cf-${CUSTOMER}-${CDN_ID}" \
  --delivery-destination-arn $DELIVERY_DEST_ARN \
  --s3-delivery-configuration '{"suffixPath":"/{yyyy}/{MM}/{dd}/{HH}"}' \
  --record-fields 'date' 'time' 'x-edge-location' 'cs-method' 'cs(Host)' 'cs-uri-stem' 'sc-status' 'cs(Referer)' 'cs(User-Agent)' 'time-to-first-byte' 'sc-content-type' 'x-host-header'
```

&lt;!—Justera `--record-fields` och `--s3-delivery-configuration` med fältlistan och sökvägssuffixet som visas på konfigurationssidan för LLM Optimizer CDN om dokument eller produktvärden ändras.—>
