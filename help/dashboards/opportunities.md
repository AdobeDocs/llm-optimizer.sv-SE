---
title: Optimeringsmöjligheter
description: Det här är artikelöversikten.
source-git-commit: 8c38027e46b53d85776fffe17597883c742235d6
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---


# Optimeringsmöjligheter

Optimeringsmöjligheter upptäcks automatiskt insikter som visar var er webbplats och externa närvaro kan förbättras för att öka varumärkets synlighet i AI-sökningar. Optimeringarna omfattar korrigeringar på sidan (tillägg av strukturerat innehåll, kanonisaler eller sammanfattningar), tekniska justeringar (avblockerande AI-crawler eller lösta fel) och påverkan på innehåll på auktoritativa webbplatser från tredje part. Genom att ta itu med dessa optimeringsmöjligheter kan ert varumärke representeras på ett korrekt sätt och det är troligare att det citeras i generativa svar.

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
| Identifiera tomma rubriker | Innehåll (på plats) | Flaggar sidor där rubriktaggar finns men inte innehåller någon text. Visar URL och plats för tomma taggar. | Lägg till beskrivande text i rubriker som återspeglar innehållet under dem. |
| Identifiera duplicerade rubriker | Innehåll (på plats) | Söker igenom HTML rubriktaggar och flaggar upprepade rubriker. Visar URL:er som påverkas och duplicerade textfragment. | Ändra rubrikerna så att de blir unika och bibehåll hierarkin (H1 → H2 → H3). Sammanfoga eller byt namn på duplicerade avsnitt. |
| Identifiera blockerad kontorstrafik | Teknisk GEO | Analyserar CDN-loggar för blockerade begäranden från kända AI-agenter (t.ex. GPTBot, PerplexityBot). Rapporterar URL:er och agenter som påverkas. | Uppdatera robots.txt eller serverkonfigurationer för att tillåta åtkomst för AI-crawler där det är lämpligt. |
| Identifiera 404s/403s/5xx-problem | Teknisk GEO | Övervakar CDN-loggar för felsvar. Rapporteringsfrekvens, påverkade URL:er och beräknade antal förlorade träffar. | Åtgärda brutna länkar, uppdatera behörigheter och åtgärda problem på serversidan så att nyckelinnehållet returnerar 200 svar. |

### Återskapa innehållssynlighet {#recover-contet}

Som anges ovan flaggar innehållssynligheten sidor där nyckelinnehåll förloras för AI-agenter på grund av klientåtergivning. För varje identifierad sida visas exakt vilket innehåll som saknas i AI-agentvyn, vilket hjälper dig att identifiera luckor i synligheten. Det stöds också av en edge-baserad förrenderingsfunktion som kan leverera mer HTML-innehåll till agell trafik utan att CMS (Content Management System) behöver ändras. Observera att den här funktionen för närvarande används i **tidig åtkomst** och kräver även konfiguration från LLMO-teamet för att optimerad innehållsleverans ska kunna aktiveras.

### Ytterligare verktyg

[LLM-synlighetskontrollen](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc) är ett Chrome-tillägg som gör att du kan se exakt hur mycket av webbsidans innehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration. Med ett enda klick kan man utvärdera vilken dator som är läsbar på en webbplats, visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad som är fallet för den mänskliga användaren. Beräknar också hur mycket innehåll som kan återskapas med LLM Optimizer.
