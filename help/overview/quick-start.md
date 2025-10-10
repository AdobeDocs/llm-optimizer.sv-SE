---
title: Snabbstart
description: Lär dig hur du kommer igång med LLM-optimering och går igenom introduktionsprocessen.
source-git-commit: e8ea9ae0d6592ea3d1e9945ec117f852112ba9d7
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---


# Snabbstart

För att komma igång med LLM-optimering måste ni gå igenom introduktionsprocessen. Därefter har du full tillgång till LLM Optimizer kontrollpaneler och alla funktioner.

## Översikt över introduktion

Startprocessen börjar med att du registrerar din domän. Processen skiljer sig åt beroende på om du är en AEM Cloud-kund eller inte. När du är klar med processen måste du ange information för CDN-loggvidarebefordran och slutligen anpassa kategorier, ämnen och uppmaningar.

### Steg 1: Anlita din domän

### AEM Cloud-kunder

AEM Cloud-kunder (Cloud Service/Managed Services/Edge Delivery) ser alternativet att testa LLM Optimizer via produkttilläggskortet i [Experience Hub](https://experienceleague.adobe.com/sv/docs/experience-manager-cloud-service/content/experience-hub/experience-hub).

>[!NOTE]
>Nyligen tillagda uppmaningar visas inte i Varumärkesnärvaro förrän bearbetningen är klar. AEM Cloud-kunder (Cloud Service, Managed Services/Edge Delivery) kan använda den kostnadsfria testversionen av LLM Optimizer. Användning av fler än 200 uppmaningar kräver ett separat licensavtal. Tillgång ges i befintligt skick och i befintligt skick och kan ändras, begränsas eller tas bort av Adobe när som helst. Kontakta din [kontorepresentant] om du vill ha mer information.

![Utvärderingsversion av LLM Optimizer](/help/overview/assets/llm-trial.png)

När du klickar på knappen **Prova LLM Optimizer** omdirigeras du till [https://llmo.now](https://llmo.now) . Du måste sedan logga in via IMS. När du har loggat in kommer du att starta introduktionsprocessen genom att ange en domän och ett varumärkesnamn.

![LLM Optimizer-domän](/help/overview/assets/domain.png)

>[!NOTE]
>Domänen du angav används av alla i organisationen och kan inte ändras.

För att aktivera analysen av närvaro av varumärke måste du ange kategorier, ämnen och uppmaningar.

![Analys av varumärkesnärvaro](/help/overview/assets/bp-analysis.png)

Dessutom måste du konfigurera CDN-loggvidarebefordran för trafikanalys. LLM Optimizer kräver att varumärkets närvarodata och insikter från agell- och hänvisningstrafik identifierar möjligheter och tillhandahåller prediktiva rekommendationer som hjälper kunderna att öka sin AI-synlighet.

### Icke-AEM Cloud-kunder

När du har signerat kontraktet anlitas du via kommandot slackbot med den domän du vill anlita i LLM Optimizer. När introduktionen är klar kan du logga in på LLM Optimizer via [https://llmo.now](https://llmo.now) .

### Steg 2: Automatisk förifyllning av insikter

När din domän har anslutit sig fyller LLM Optimizer automatiskt i följande:

* **Kategorier** - Omfattande innehållsområden som är relevanta för din domän.
* **Ämnen** - Specifika teman som är kopplade till nyckelord för stora volymer som inte är varumärkesprofilerade och som är kopplade till din domän.
* **Uppmanar** - Frågor (profilerade och icke-märkta) om att ange synlighet för baslinjen.

Detta garanterar att ni redan innan ni lägger till era anpassade konfigurationer och indata får de inledande insikterna om varumärkessynligheten.

### Steg 3: Anpassa kategorier, ämnen och frågor

Klicka på kontrollpanelen för [kundkonfiguration](/help/dashboards/customer-configuration.md) för att börja anpassa dina kategorier, ämnen och frågor.

![Kontrollpanel för kundkonfiguration](/help/dashboards/assets/customer-config.png)

Från den här instrumentpanelen kan du:

* Lägg till nya kategorier som passar era affärsprioriteringar.
* Ange anpassade ämnen eller underämnen som behöver spåras.
* Skapa dina uppmaningar för att övervaka synligheten i specifika frågor.
* Definiera omnämnandealias så att alla omnämnanden hämtas.
* Definiera konkurrentalias för att spåra konkurrenterna korrekt.

### Steg 4: Ange information för vidarebefordran av CDN-loggar

Om du vill låsa upp information om AGT- och REFERENSTRERINGSTRANSPLANER måste du ange information för vidarebefordran av CDN-loggar. Mer information om hur du konfigurerar vidarebefordran av loggar finns på varje sida:

* [Myndighetstrafik](/help/dashboards/agentic-traffic.md)
* [Hänvisningstrafik](/help/dashboards/referral-traffic.md#setup#cdn-setup)

### Steg 5: Utforska instrumentpaneler och vidta åtgärder

När du har angett information för CDN-loggvidarebefordran kan du:

* Visa instrumentpanelen [Varumärkesnärvaro](/help/dashboards/brand-presence.md), visa din synlighetspoäng och spåra din prestanda i förhållande till dina konkurrenter.
* Utforska kontrollpanelerna [Agentic](/help/dashboards/agentic-traffic.md) och [Referral Traffic](/help/dashboards/referral-traffic.md).
* Använd [säljprojekt](/help/dashboards/opportunities.md) för att identifiera innehåll och tekniska förbättringar.
* Exportera data och samarbeta med teamet eller bjud in medarbetaren att använda produkten.

Utforska alla tillgängliga [kontrollpaneler](/help/dashboards/dashboards-overview.md) för att få en fullständig förståelse för funktionerna i LLM-optimering.
