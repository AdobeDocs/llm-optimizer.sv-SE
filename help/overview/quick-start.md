---
title: Snabbstart
description: Kom igång med Adobe LLM Optimizer - ta in ditt varumärke i datorn, lås upp insikter om AI-synlighet och utforska instrumentpaneler för att förbättra sökresultatet.
feature: Quickstart, Onboarding
source-git-commit: 82830e66d43ddd9741617cdf6daab63cd259554b
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 0%

---


# Snabbstart

För att komma igång med LLM Optimizer måste du slutföra introduktionsprocessen enligt anvisningarna nedan. När du har slutfört processen får du fullständig åtkomst till [LLM Optimizer-kontrollpaneler](/help/dashboards/dashboards-overview.md) och andra funktioner.

## Översikt över introduktion

Startprocessen börjar med att du registrerar din domän. Processen skiljer sig åt beroende på om du är en AEM Cloud-kund eller inte. När du är klar med processen måste du ange information för CDN-loggvidarebefordran och slutligen anpassa kategorier, ämnen och uppmaningar. Varje del av processen beskrivs nedan tillsammans med praktiska tips om hur du kommer igång med LLM Optimizer så snart som möjligt.

### Tillåta Adobe LLM Optimizer åtkomst till offentliga sidor

För att kunna leverera korrekt innehåll och tekniska rekommendationer måste Adobe LLM Optimizer ha tillgång till era offentliga sidor. Detta uppnås genom en säker intern crawler (Spacecat/1.0-användaragent).

Konfigurationskrav:

* Lägg till användaragenten Spacecat/1.0 till Tillåtelselista i webbplatsens robots.txt-fil eller Robottrafikhanteringsregler
* Kontrollera att sidorna inte blockeras på domän- eller CDN-nivå. Blockerade sidor kan inte indexeras, vilket innebär att optimeringsuppgifter och insikter inte kan genereras för dem.

Om synligheten för innehållet verkar vara låg på kontrollpanelen kontrollerar du att crawlern har åtkomst till dina domäner. Begränsad åtkomst är en vanlig orsak till ofullständig indexering.

## Steg 1: Anlita din domän

### Testa innan du köper

AEM Cloud-kunder (Cloud Service, Managed Services, Edge Delivery Service) kan välja att använda erbjudandet **Testa innan du köper**. Det är en kostnadsfri testversion av LLM Optimizer med upp till 200 kostnadsfria uppmaningar. Användning av fler än 200 uppmaningar kräver ett separat licensavtal. Tillgång ges i befintligt skick och i befintligt skick och kan ändras, begränsas eller tas bort av Adobe när som helst.

Det finns en del funktioner som inte finns i den kostnadsfria versionen:

* Testversionen är begränsad till en domän. Du kan inte ändra domänen som du angav när du har slutfört installationen.
* Möjligheten att driftsätta optimeringar finns i Tidig åtkomst. Läs mer på [Optimera på Edge Frågor och svar](https://experienceleague.adobe.com/sv/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions).

Se avsnittet nedan för mer ingående information om hur du aktiverar den kostnadsfria testversionen och registrerar din domän.

### AEM Cloud-kunder

Om du är en AEM Cloud-kund kan du testa LLM Optimizer med produktanmälningskortet i [Experience Hub](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/experience-hub/experience-hub).

>[!NOTE]
>Nyligen tillagda uppmaningar visas inte på kontrollpanelen [Varumärkesnärvaro](/help/dashboards/brand-presence.md) förrän bearbetningen är klar. AEM Cloud-kunder kan använda den kostnadsfria testversionen av LLM Optimizer. Användning av fler än 200 uppmaningar kräver ett separat licensavtal. Tillgång ges i befintligt skick och i befintligt skick och kan ändras, begränsas eller tas bort av Adobe när som helst. Kontakta din kontorepresentant om du vill ha mer information.

![Utvärderingsversion av LLM Optimizer](/help/overview/assets/llm-trial.png)

När du klickar på knappen **Testa LLM Optimizer** omdirigeras du till [https://llmo.now](https://llmo.now). Du måste sedan logga in via IMS. När du har loggat in kommer du att starta introduktionsprocessen genom att ange en domän och ett varumärkesnamn.

![LLM Optimizer-domän](/help/overview/assets/domain.png)

>[!NOTE]
>Domänen du angav används av alla i din organisation och kan inte ändras.

En liten uppsättning kategorier, ämnen och uppmaningar genereras under introduktionsfasen. Analys av närvaro av varumärken i dessa meddelanden kommer att vara tillgänglig kort efter att webbplatsen har lanserats.

<!--![Brand Presence Analysis](/help/overview/assets/bp-analysis.png)-->

Dessutom måste du konfigurera [CDN-loggvidarebefordran](#step-4) för trafikanalys. LLM Optimizer kräver att varumärkets närvarodata och insikter från agens- och hänvisningstrafik ska identifiera möjligheter och tillhandahålla prediktiva rekommendationer för att öka AI-synligheten.

### Icke-AEM Cloud-kunder

När affärsavtalet är klart kommer du att vara registrerad på den domän du vill anlita på LLM Optimizer. När introduktionen är klar kan du logga in på LLM Optimizer via [https://llmo.now](https://llmo.now).

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

Om du vill låsa upp information om AGT- och REFERENSTRERINGSTRANSPLANER måste du ange information för vidarebefordran av CDN-loggar. Den kan läggas till från [kundkonfigurationspanelen](/help/dashboards/customer-configuration.md#cdn-configuration) genom att gå till fliken **CDN-konfiguration** och klicka på **Inbyggt CDN**.

![CDN för kundkonfiguration](/help/overview/assets/cc-cdn.png)

Om ingen CDN-leverantör har lagts till i förväg (enligt beskrivningen ovan) uppmanas du att lägga till vidarebefordran av CDN-loggar när du för första gången använder kontrollpanelerna för Agentic and Referral Traffic. Mer information finns i:

* [Myndighetstrafik](/help/dashboards/agentic-traffic.md#cdn-setup)
* [Hänvisningstrafik](/help/dashboards/referral-traffic.md#setup#setup)

## Steg 5: Utforska instrumentpaneler och vidta åtgärder

När du har angett information för CDN-loggvidarebefordran kan du:

* Visa kontrollpanelen [Varumärkesnärvaro](/help/dashboards/brand-presence.md) och visa din synlighetspoäng och spåra din prestanda i förhållande till andra varumärken.
* Utforska kontrollpanelerna [Agentic](/help/dashboards/agentic-traffic.md) och [Referral Traffic](/help/dashboards/referral-traffic.md) om CDN-loggöverföringen har konfigurerats.
* Använd [säljprojekt](/help/dashboards/opportunities.md) för att identifiera innehåll och tekniska förbättringar.
* Exportera data och samarbeta med teamet eller bjud in medarbetaren att använda produkten.

Slutligen, för att få en fullständig förståelse för LLM Optimizer funktioner bör du utforska alla tillgängliga [dashboards](/help/dashboards/dashboards-overview.md).
