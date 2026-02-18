---
title: Optimeringsmöjligheter
description: Lär dig hur du använder kontrollpanelen för affärsmöjligheter för att automatiskt upptäcka hur webbplatsen kan förbättras för att öka varumärkets synlighet.
feature: Opportunities
source-git-commit: 1f665bd14349c15d92f8274742606abcf9b02000
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 0%

---


# Optimeringsmöjligheter

Optimeringsmöjligheter upptäcks automatiskt insikter som visar var er webbplats och externa närvaro kan förbättras för att öka varumärkets synlighet i AI-sökningar.

Optimeringarna omfattar korrigeringar på sidan (tillägg av strukturerat innehåll, kanonisaler eller sammanfattningar), tekniska justeringar (avblockerande AI-crawler eller lösta fel) och påverkan på innehåll på auktoritativa webbplatser från tredje part. Genom att ta itu med dessa optimeringsmöjligheter kan ert varumärke representeras på ett korrekt sätt och det är troligare att det citeras i generativa svar.

![Optimeringsmöjligheter](/help/dashboards/assets/oport.png)

## Kontrollpanel för affärsmöjligheter

Optimeringsmöjligheterna som presenteras på kontrollpanelen prioriteras baserat på spelarluckor, trendavsnitt och prestandadata och presenteras som en lista. Du kan söka efter en specifik affärsmöjlighet genom att använda sökfältet. Dessutom grupperas affärsmöjligheterna efter taggar, och du kan klicka direkt på en tagg för att visa alla affärsmöjligheter som grupperats under den taggen.

Om du klickar på **Detaljer** öppnas ett separat fönster där mer information och ytterligare vägledning finns.

## Möjligheter som stöds

Nedan finns en tabell över de affärsmöjligheter som stöds:

| Möjligheter | Typ | Identifierade problem | Korrigera förslag |
|---------|----------|----------|----------|
| Sammanfatta långa stycken | Innehåll (på plats) | Identifierar stycken som överskrider de rekommenderade tröskelvärdena för längd. Visar URL:er som påverkas och stora textstycken. | Skapa abstrakter eller dela upp lång text i kortare, skannningsbara avsnitt. |
| Rekommendera strukturerat innehåll (frågor och svar) | Innehåll (på plats) | Identifierar meddelanden med stor popularitet utan matchande FAQ-poster. Visar relaterade uppmaningar, kategorier och URL:er som påverkas. | Lägg till schemablock med vanliga frågor och svar som matchar vanliga frågor. |
| Identifiera saknade reflexer | Innehåll (på plats) | Flaggar sidor som saknar speglingsattribut. Tillhandahåller URL:er som påverkas och förväntad täckning per språk/region. | Implementera hreflang-taggar för att ange rätt lokaliserade versioner. |
| Identifiera saknade kanonicaler | Innehåll (på plats) | Söker efter sidor utan kanoniska taggar eller med taggar i konflikt. Listar URL:er och dubbletter som påverkas. | Lägg till kanoniska taggar som pekar på den önskade versionen av varje sida. Säkerställ konsekvent användning över alla varianter. |
| Identifiera blockerad kontorstrafik | Teknisk GEO | Analyserar CDN-loggar för blockerade begäranden från kända AI-agenter (t.ex. GPTBot, PerplexityBot). Rapporterar URL:er och agenter som påverkas. | Uppdatera robots.txt eller serverkonfigurationer för att tillåta åtkomst för AI-crawler där det är lämpligt. |
| Identifiera 404s/403s/5xx-problem | Teknisk GEO | Övervakar CDN-loggar för felsvar. Rapporteringsfrekvens, påverkade URL:er och beräknade antal förlorade träffar. | Åtgärda brutna länkar, uppdatera behörigheter och åtgärda problem på serversidan så att nyckelinnehållet returnerar 200 svar. |
| Återskapa innehållets synlighet (tidig åtkomst) | Teknisk GEO | Flaggar sidor där kritiskt innehåll döljs för AI-agenter. Visar URL:er som påverkas och förväntat innehåll som kan återställas. | Återge sidorna i förväg så att mer innehåll är tillgängligt för AI-agenter utan att JavaScript exekveras. |

## Automatisk optimering {#auto-optimization}

Automatisk optimering gör det möjligt att med ett enda klick driftsätta rekommenderade optimeringar, vilket minskar den manuella ansträngningen och den tid det tar att utvärdera. Optimeringar kan användas antingen i innehållskällan eller vid CDN-kanten. Edge-baserad automatisk optimering är för närvarande tillgänglig i tidig åtkomst för utvalda möjligheter. Mer information finns på sidan [Optimera på Edge](/help/dashboards/optimize-at-edge.md).

<!--### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.-->

### Ytterligare verktyg

[LLM-synlighetskontrollen](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc) är ett Chrome-tillägg som gör att du kan se exakt hur mycket av webbsidans innehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration. Med ett enda klick kan man utvärdera vilken dator som är läsbar på en webbplats, visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad som är fallet för den mänskliga användaren. Beräknar också hur mycket innehåll som kan återskapas med LLM Optimizer.
