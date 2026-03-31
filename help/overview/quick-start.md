---
title: Snabbstart
description: Lär dig hur du lägger in ditt varumärke och din domän, aktiverar din testversion från Experience Hub eller Experience Cloud och slutför konfigurationen för Adobe LLM Optimizer.
feature: Quickstart, Onboarding
source-git-commit: dcbeb1c61dd9dcefd83908f65f8303d36c0fb78e
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 0%

---


# Snabbstart

För att komma igång med LLM Optimizer måste du slutföra introduktionsprocessen. Efter introduktionen kan du anpassa kategorier, ämnen, uppmaningar och konfigurera vidarebefordran av loggar för mer korrekta insikter och fullständig åtkomst till [LLM Optimizer kontrollpaneler](/help/dashboards/dashboards-overview.md) och andra funktioner.

## Översikt över introduktion

Startprocessen börjar med att introducera din domän och ditt varumärke. Varje del av introduktionsresan beskrivs nedan tillsammans med praktiska tips om hur du kommer igång med LLM Optimizer så snart som möjligt.

### Tillåta Adobe LLM Optimizer åtkomst till offentliga sidor

För att kunna leverera korrekt innehåll och tekniska rekommendationer måste Adobe LLM Optimizer ha tillgång till era offentliga sidor. Detta uppnås genom en säker intern crawler (Spacecat/1.0-användaragent).

Konfigurationskrav:

* Lägg till användaragenten Spacecat/1.0 till Tillåtelselista i webbplatsens robots.txt-fil eller trafikhanteringsregler för robots.txt.
* Kontrollera att sidorna inte blockeras på domän- eller CDN-nivå. Blockerade sidor kan inte indexeras, vilket innebär att optimeringsuppgifter och insikter inte kan genereras för dem.

Om synligheten för innehållet verkar vara låg på kontrollpanelen kontrollerar du att crawlern har åtkomst till dina domäner. Begränsad åtkomst är en vanlig orsak till ofullständig indexering.

## Steg 1: Ta in ditt varumärke och din domän {#step-1-onboard-your-domain}

För att komma igång med LLM Optimizer måste du först aktivera testversionen (om du är berättigad) och lägga in ditt varumärke och din domän.

### Aktivera testversionen

Aktiveringsflödet varierar beroende på vilken Adobe-produkt du använder.

#### AEM Cloud-kunder

Om du vill aktivera testversionen som AEM Cloud-kund kan du antingen:

* Navigera till [Experience Hub](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/experience-hub/experience-hub) och använd produktmeddelandekortet för att aktivera LLM Optimizer. När du har valt **Prova LLM Optimizer** omdirigeras du till [https://llmo.now](https://llmo.now). Logga in via IMS och ange sedan en domän och ett varumärkesnamn för att starta introduktionsprocessen.
* Eller gå direkt till [https://llmo.now](https://llmo.now) och logga in.

![Utvärderingsversion av LLM Optimizer](/help/overview/assets/llm-trial.png)

#### Adobe Analytics-kunder

Om du är Adobe Analytics-kund ser du en banderoll på Experience Cloud hemsida.

![Experience Cloud startsida med Start your Adobe LLM Optimizer Trial banner](/help/overview/assets/experience-cloud-llmo-trial-banner.png)

Du kan aktivera testversionen på något av följande sätt:

* Välj **Starta testversionen av Adobe LLM Optimizer** i banderollen.
* Gå direkt till [https://llmo.now](https://llmo.now) och logga in.

När testversionen är aktiv fortsätter du med att introducera ditt varumärke och din domän.

>[!NOTE]
>
> * **Kostnadsfri provversion:** AEM Cloud- och Adobe Analytics-kunder kan använda den kostnadsfria provversionen av LLM Optimizer.
> * **Kunder som aktiverar testversionen den 1 april 2026 eller senare** kan använda upp till 100 uppmaningar, en domän, och kan distribuera optimeringar på upp till 10 URL:er för en enda typ av affärsmöjlighet.
> * **Kunder som aktiverade testversionen före 1 april 2026** har fortfarande tillgång till upp till 200 uppmaningar enligt sina befintliga villkor.
>
>Användning utöver de angivna begränsningarna kräver ett separat licensavtal. Tillgång ges i befintligt skick och i befintligt skick och kan ändras, begränsas eller tas bort när som helst. Kontakta din kontorepresentant om du vill ha mer information.

#### Anpassa ert varumärke och er domän

Anlita ert varumärke och er domän för att börja använda LLM Optimizer.

1. Ange ditt varumärke och den tillhörande domänen.

   * Det bör vara huvuddomänen där du vill analysera och optimera innehåll.

1. Fullständig introduktion.

   * När LLM Optimizer väl har skickat in den börjar vi analysera din domän och generera insikter.

![LLM Optimizer-domän](/help/overview/assets/domain.png)

>[!NOTE]
>Nyligen tillagda uppmaningar visas inte på kontrollpanelen [Varumärkesnärvaro](/help/dashboards/brand-presence.md) förrän bearbetningen är klar.

>[!NOTE]
>Domänen du angav används av alla i din organisation och kan inte ändras.

En liten uppsättning kategorier, ämnen och uppmaningar genereras under introduktionsfasen. Analys av närvaro av varumärken i dessa meddelanden kommer att vara tillgänglig kort efter att webbplatsen har lanserats.

Det finns även möjlighet att driftsätta optimeringar i realtid. Läs mer i [Optimera på Edge - Frågor och svar](https://experienceleague.adobe.com/sv/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions).

Konfigurera dessutom [CDN-loggvidarebefordran](#step-4) för trafikanalys. LLM Optimizer kräver varumärkets närvarodata och insikter från agens- och hänvisningstrafik för att identifiera möjligheter och tillhandahålla prediktiva rekommendationer som ökar AI:s synlighet.

### Kunder utanför AEM Cloud

När organisationen har slutfört affärsavtalet anländer du till LLM Optimizer med den domän som din organisation har valt. Logga in på [https://llmo.now](https://llmo.now) när introduktionen är klar.

## Steg 2: Anpassa kategorier, ämnen och frågor

När sajten väl har anslutits kan du visa analysen för varumärkesnärvaro baserat på den lilla uppsättning uppmaningar som automatiskt genererades under introduktionsfasen. I framtiden kan ni anpassa kategorier, ämnen och uppmaningar för ert varumärke. Den här konfigurationen skapas på [kundkonfigurationspanelen](/help/dashboards/customer-configuration.md).

![Kontrollpanel för kundkonfiguration](/help/overview/assets/prompt-creation.png)

Från den här instrumentpanelen kan du:

* Lägg till **nya kategorier** som följer dina affärsprioriteringar. Kategorier kan vara breda innehållsområden som är relevanta för din domän.
* Ange **anpassade ämnen** eller underämnen som du vill spåra. Ämnen kan vara specifika teman kopplade till nyckelord med stora volymer som inte är varumärkesprofilerade och som är kopplade till din domän.
* Skapa **dina uppmaningar** om du vill övervaka synligheten i specifika frågor. Frågar är frågor (varumärkesprofilerade och icke-märkta) som ger en synlighet för baslinjen. Endast ett begränsat antal uppmaningar genereras automatiskt baserat på de kategorier och ämnen du har angett.
* Definiera omnämnande av **alias** för att säkerställa att alla omnämnanden av ett varumärke hämtas och redovisas.
* Definiera **andra alias** om du vill spåra andra varumärken korrekt.

>[!NOTE]
>De exakta uppmaningar ni frågar om livslångt lärande är inte tillgängliga för allmänheten eftersom de inte offentliggörs av de s.k. LLM.

>[!NOTE]
>
> Mer information om hur du konfigurerar dina kategorier, ämnen, uppmaningar finns på sidan [Bästa tillvägagångssätt för att konfigurera kategorier, ämnen, uppmaningar](/help/overview/best-practices-topics-prompts.md).

## Steg 3: Insikter om varumärkesnärvaro

När din domän har introducerats ser du inledande insikter i vyn Varumärkesnärvaro baserat på de uppmaningar som automatiskt genererades under introduktionen. När du har anpassat dina kategorier, ämnen och uppmaningar kommer LLM Optimizer automatiskt att aktivera analysen av närvaro i varumärke på de uppmaningar du har angett och resultaten blir tillgängliga inom 24 timmar.

## Steg 4: Ange information för vidarebefordran av CDN-loggar {#step-4}

Om du vill låsa upp information om agenttrafik och hänvisningstrafik lägger du till information om vidarebefordran av CDN-logg från kontrollpanelen för [kundkonfiguration](/help/dashboards/customer-configuration.md#cdn-configuration). Öppna fliken **CDN-konfiguration** och välj **Inbyggt CDN**.

![CDN för kundkonfiguration](/help/overview/assets/cc-cdn.png)

Om ingen CDN-leverantör har lagts till i förväg (enligt beskrivningen ovan) uppmanas du att lägga till vidarebefordran av CDN-loggar när du för första gången använder kontrollpanelerna för Agentic and Referral Traffic. Mer information finns i:

* [Myndighetstrafik](/help/dashboards/agentic-traffic.md#cdn-setup)
* [Hänvisningstrafik](/help/dashboards/referral-traffic.md#setup)

>[!NOTE]
>Mer information om vidarebefordran av loggar när du använder ett kundhanterat CDN (BYOCDN) finns i [Översikt över vidarebefordran av BYOCDN-loggar](/help/overview/log-forwarding/log-forwarding-overview.md)

## Steg 5: Utforska instrumentpaneler och vidta åtgärder

När du har angett information för CDN-loggvidarebefordran kan du:

* Visa kontrollpanelen [Varumärkesnärvaro](/help/dashboards/brand-presence.md) och visa din synlighetspoäng och spåra din prestanda i förhållande till andra varumärken.
* Utforska kontrollpanelerna [Agentic](/help/dashboards/agentic-traffic.md) och [Referral Traffic](/help/dashboards/referral-traffic.md) om CDN-loggvidarebefordran har konfigurerats.
* Använd [säljprojekt](/help/dashboards/opportunities.md) för att identifiera innehåll och tekniska förbättringar.
* Exportera data och samarbeta med teamet eller bjud in medarbetaren att använda produkten.

Slutligen, för att få en fullständig förståelse för LLM Optimizer funktioner bör du utforska alla tillgängliga [dashboards](/help/dashboards/dashboards-overview.md).
