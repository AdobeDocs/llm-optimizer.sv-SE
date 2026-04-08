---
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---
# Fragment

## Hämtningssteg för API-nyckel {#retrieve-byocdn-api-key}

**Steg för att hämta Edge Optimize API-nyckeln för din produktion:**

1. Öppna **Kundkonfiguration** i LLM Optimizer och välj fliken **CDN-konfiguration** .

   ![Navigera till Kundkonfiguration](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. Leta reda på avsnittet **Distribuera optimeringar till AI-agenter**. Markera kryssrutan **Aktivera optimeringsmotor**.

   ![Distribuera optimeringar till AI-agenter - väntar](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. Välj **Aktivera** i bekräftelsedialogrutan.

   ![Aktivera bekräftelsedialogrutan för optimeringsmotorn](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. Välj **Visa information**. I dialogrutan **Distribuera optimeringsinformation** kopierar du **Production API-nyckeln** (använd **Copy** bredvid fältet).

   ![Produktions-API-nyckel i Distribuera optimeringsinformation](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >I dialogrutan kan det visas att installationen inte är klar. Detta förväntas tills routningen har verifierats - du kan fortfarande kopiera API-nyckeln så att IT- eller CDN-teamet kan slutföra konfigurationen.

Om du behöver hjälp med ovanstående steg kontaktar du ditt Adobe-kontoteam eller `llmo-at-edge@adobe.com`.

## Mellanlagringsdomän-API-nyckel (valfritt) {#retrieve-staging-edge-optimize-api-key}

Använd ett mellanlagringsvärdnamn när du vill testa Optimera på Edge i en lägre miljö innan produktionstrafiken använder routningsreglerna.

**Förutsättningar**

* Mellanlagringsvärdnamnet måste tillhöra **samma registrerade domän** som din produktionsplats (till exempel `https://staging.example.com` när produktionen är `https://www.example.com`).
* Endast **en** mellanlagringsdomän kan konfigureras för platsen. När den har sparats kan den inte ändras utan hjälp.

**Steg**

1. Öppna **Kundkonfiguration** i LLM Optimizer och välj fliken **CDN-konfiguration** .

2. I avsnittet **Distribuera optimeringar till AI-agenter** väljer du **Lägg till scendomän** (eller **scendomän** om en mellanlagringsdomän redan har konfigurerats).

3. Ange den fullständiga mellanlagrings-URL:en inklusive `https://` i dialogrutan **Scendomän** och välj **Ange domän**.

   ![Dialogrutan Indata för scendomän](/help/assets/optimize-at-edge/byocdn-staging-domain-input.png)

4. Bekräfta domänen i nästa uppmaning. När arbetsflödet är klart visas den konfigurerade domänen och dess **API-nyckel** i dialogrutan **Stage Domains**. Välj **Kopiera** om du vill kopiera API-nyckeln för mellanlagring.

   ![API-nyckel för mellanlagringsdomän](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

Kontakta `llmo-at-edge@adobe.com` om du behöver hjälp.

## Kontrollera routningsstatus i LLM Optimizer {#verify-routing-status-in-ui}

Status för trafikroutningen kan också kontrolleras i LLM Optimizer-gränssnittet. Navigera till **Kundkonfiguration** och välj fliken **CDN-konfiguration** .

![Distribuera optimeringar till AI-agenter - slutfört](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## Återgå till översikt {#return-to-overview}

Om du vill veta mer om Optimera på Edge, inklusive tillgängliga möjligheter, automatiska optimeringsarbetsflöden och vanliga frågor och svar, går du tillbaka till [Optimera på Edge-översikt](/help/dashboards/optimize-at-edge/overview.md).
