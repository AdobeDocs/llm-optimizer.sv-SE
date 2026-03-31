---
title: Adobe Analytics Integration
description: Lär dig hur du kopplar Adobe Analytics med LLM Optimizer för att mäta AI-driven identifiering, webbplatsengagemang och affärsresultat i kontrollpanelen för hänvisningstrafik.
feature: Referral Traffic
source-git-commit: e7c9bc1d40267dc92608baa005f85f4be21cfda1
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---


# Adobe Analytics Integration

Adobe Analytics-integreringen kopplar samman LLM Optimizer med era Adobe Analytics-data så att ni kan mäta hur AI-styrd identifiering kan förvandlas till verkligt webbplatsengagemang och affärsresultat. När integreringsprocessen är klar är data tillgängliga på kontrollpanelen **Referenstrafik** på fliken **Affärseffekt** .

Genom att länka analysdata till AI-insikter kan LLM Optimizer hjälpa er att spåra:

* Användarengagemang på AI-refererade sidor.
* Konverteringssignaler kopplade till AI-identifieringsresor.
* Prestandaeffekter av GEO-optimeringar.

Integrationen överbryggar klyftan mellan AI-mätning av synlighet och analys av företagsprestanda. Teamen kan nu inte bara se var varumärket visas i AI-svar, utan även vad som händer när användarna kommer.

## Tillgänglighet {#availability}

>[!IMPORTANT]
>
>Adobe Analytics-integreringen ingår i det betalda LLM Optimizer-erbjudandet. Kunder som använder den kostnadsfria provversionen kommer inte att kunna ansluta Adobe Analytics förrän de uppgraderar till ett betalt erbjudande.

## Anslut Adobe Analytics till kontrollpanelen för hänvisningstrafik {#connect}

Anslutningsflödet startar från kontrollpanelen [Referenstrafik](/help/dashboards/referral-traffic.md) enligt följande:

1. Öppna instrumentpanelen [Referenstrafik](/help/dashboards/referral-traffic.md). Standardvyn är **Trafikinsikter**.

   ![Kontrollpanel för referenstrafik, fliken Traffic Insights](/help/dashboards/assets/aa-integration-01-referral-traffic-traffic-insights.png)

1. Välj fliken **Affärspåverkan**. Om ingen analysleverantör är ansluten visas en banderoll: **Anslut till Business Impact** med **Anslut till Analytics**.

   ![Fliken Business Impact med Connect to Analytics](/help/dashboards/assets/aa-integration-02-business-impact-connect.png)

1. Välj **Anslut till analys**. Kontrollpanelen [Kundkonfiguration](/help/dashboards/customer-configuration.md) öppnas på fliken **Analytics**.

   ![Kundkonfiguration, fliken Analytics](/help/dashboards/assets/aa-integration-03-analytics-tab.png)

1. Under **Autentiseringsuppgifter** anger du **klient-ID** och **klienthemlighet** och väljer sedan **Verifiera och fortsätt**. Obs!

   * **Verifiera och fortsätt** är bara tillgängligt när båda fälten är ifyllda.
   * Efter en lyckad verifiering läses rapportsviterna in.
   * Använd **Klient-ID** och **Klienthemlighet** för ett [tekniskt konto](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) som har tillgång till den rapportserie du behöver.

   ![Analysautentiseringsuppgifter och verifiera och fortsätt](/help/dashboards/assets/aa-integration-04-credentials.png)

1. Under **Konfiguration** väljer du en **rapportsvit**.

   När en rapportsvit har valts läser LLM Optimizer in de **sid-URL:er för Dimension** som är tillgängliga för den sviten.

   ![Rapportsviten har valts och dimensioner har lästs in](/help/dashboards/assets/aa-integration-05-report-suite.png)

1. Välj en **sid-URL för Dimension** (en programspecifik dimensionslista) och välj sedan **Spara och aktivera**.

   * **Sidadressen Dimension** är inaktiverad tills en rapportsvit har valts och dimensionerna har lästs in.
   * **Spara och aktivera** är bara tillgängligt när du har valt en sidas URL-dimension.

   ![Sidans URL-dimension och Spara och aktivera](/help/dashboards/assets/aa-integration-06-page-url-dimension.png)

1. När konfigurationen har sparats bör den visa statusen **Ansluten**. Du kan gå tillbaka till kontrollpanelen för hänvisningstrafik med **Gå till kontrollpanelen för hänvisningstrafik**. I **Referenstrafik** på fliken **Affärspåverkan** ska statusen vara **Ansluten till Adobe Analytics**.

   ![Ansluten till Adobe Analytics i konfiguration och affärspåverkan](/help/dashboards/assets/aa-integration-07-connected.png)

### När du har anslutit {#after-connect}

* LLM Optimizer fyller i de **senaste fyra fullständiga kalenderveckorna** och den **aktuella kalenderveckan hittills**.
* Efter bakåtfyllnad uppdateras data **dagligen** med en dra av **hela föregående dag**.

>[!NOTE]
>
>Bakfyllningen kan ta några timmar.

## Så här fungerar det {#how-it-works}

### Konfiguration

Under konfigurationen definierar du vilken rapportsserie och siddimension som LLM Optimizer använder för Adobe Analytics-inmatningen. Siddimensionen kan vara `variables/page`-standardmappningen eller en anpassad `eVar`, beroende på rapportsviten.

### Hur LLM-trafik identifieras

Trafik som har sitt ursprung i LLM identifieras med Adobe Analytics [Refererartyp - konversationsbaserade AI-verktyg](https://experienceleague.adobe.com/sv/docs/analytics/components/dimensions/referrer-type#conversational-ai-tools).

### Insamlade data {#data-ingested}

Följande data hämtas från LLM Optimizer:

**Dimensioner**

* Referensdomän
* Referenstyp
* Land
* Enhetstyp
* Vald siddimension

**Mätvärden**

* Sidvyer
* Besök
* Besökare
* Poster
* Avslutar
* studsar
* Besök på en sida
* Tidsåtgång
* Korgar
* Kundtillägg
* Ta bort kundvagn
* Vyer
* Utcheckningar
* Beställningar
* Intäkter
* Enheter

### Hur LLM Optimizer använder dessa data

Den här datauppsättningen ger LLM Optimizer insikter för:

* Belastning för LLM-trafik på sidnivå.
* Referensprestanda för alla LLM-källor.
* Regionala trender och trender på enhetsnivå.
* Resultat för nedströmsförsäljning.

## Frågor och svar {#faq}

F: Är Adobe Analytics-integreringen tillgänglig under testperioden?

Nej. Integreringen är endast tillgänglig för betalande LLM Optimizer-kunder.

F: Vilka data samlas in eller lagras?

Se kapitlet [Inkapslade data](#data-ingested) ovan. LLM Optimizer arbetar med aggregerade mätvärden från Adobe Analytics API:er som godkänts av er organisation, inte med rådata på träffnivå.

F: Hur importeras data?

Din organisation tillåter LLM Optimizer att fråga Adobe Analytics API:er. Referenstrafik som är anpassad till LLM-källor förbrukas via dessa API:er.

F: Hur ofta uppdateras data?

Data uppdateras **dagligen** (fullständig tidigare dag efter att bakåtfyllnaden har slutförts).

F: Lagras rådata på träffnivå i LLM Optimizer?

Nej. Endast **aggregerade** mätvärden används för att förstå trafikmönster och trender.

F: Är fullständiga URL:er, frågesträngar eller sidinnehåll lagrade?

Fullständiga URL:er som används för den valda siddimensionen kan kapslas. Frågesträngar och sidinnehåll är inte kapslade för den här integreringen.

F: Lagras användaridentifierare (ECID, IP-adress, cookie-ID)?

Nej.

F: Hur länge sparas data?

Tänk på att bevarandeprinciper kan utvecklas. För närvarande lagras data i oändlighet.

F: Krypteras data under överföring och i vila?

För närvarande krypteras data under överföring och inte i vila. Detta kan komma att ändras i framtida uppdateringar.

F: Är historiska data efterfyllda?

Ja. När konfigurationen är klar fylls de fyra senaste fullständiga kalenderveckorna och den aktuella kalenderveckan i bakåt. Se även [När du har anslutit](#after-connect).
