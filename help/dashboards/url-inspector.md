---
title: URL-kontroll
description: Lär dig hur du använder URL-kontrollen för att analysera hur specifika sidor i din domän fungerar vid AI-sökningar.
source-git-commit: 6d845628264885fd40fc6e14dff89b8aa80f9ca5
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---


# URL-kontroll

Med URL-kontrollen kan du analysera hur specifika sidor i din domän fungerar vid AI-sökningar. Den kombinerar synlighet, agens-trafik och hänvisningsdata på URL-nivå för att ge dig en detaljerad bild av vilka URL:er som anges och hur ofta de visas i svaren.

![URL-kontrollen](/help/dashboards/assets/url-insp.png)

## Filter

Överst på sidan kan du använda filter för att förfina visningen. De filter du väljer påverkar **alla** -avsnitt som finns på kontrollpanelen. Du kan anpassa följande:

* **Datumintervall** - Välj tidsram för visade data. De senaste fyra veckorna, till exempel. Du kan också anpassa tidsperioden genom att välja alternativet **Anpassade veckor**.
* **Kategori** - Filtrera de resultat som visas efter kategorier.
* **Plattform** - Välj vilken AI-motor som ska analyseras.
* **Sidinnehållstyp** - Filtrera efter innehållstyp.
* **Region** - Filtrera resultaten efter geografi. Alla regioner är inte tillgängliga vid lanseringen.

När du har valt önskat filter klickar du på **Använd filter** för att använda markeringen på instrumentpanelen.

## Översikt, mått

URL-kontrollen innehåller flera översiktsmått, så att du snabbt kan bedöma hur sidorna fungerar vid AI-sökningar. Följande mätvärden finns:

* **Unika uppmaningar med egna citat** - Det totala antalet unika AI-prompter med egna citat.
* **Totalt antal unika uppmaningar** - Totalt antal unika AI-uppmaningar.
* **Unika citerade URL:er** - Antalet unika ägda URL:er som har angetts.
* **Antal angivna gånger** - Totalt antal gånger en ägd URL har angetts i AI-genererade svar.
<!-- * **Total agentic hits** - The total number of hits from AI agents on your URLs.
* **Referral hits from LLMs** - The total number of hits directed from AI-generated answers to your URLs.-->

Trendindikatorer för varje översiktsmått visar hur dessa värden ändras över tiden jämfört med föregående period.

## Dina beställda URL:er

Den URL-vy som citeras listar alla URL-adresser från ert varumärke som har citerats i AI-genererade svar, med stödjande mått. Datatabellen har också ett sökfält för snabb åtkomst till specifika URL:er. Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten. Följande mätvärden finns:

* **URL** - den analyserade URL:en
* **Tider som anges** - Antalet gånger som URL:en har citerats i AI-genererade svar.
* **Uppmaningar som citeras i** - Antalet unika AI-uppmaningar som refererade till URL:en.
* **Kategorier** - De produktkategorier eller ämnen som är associerade med URL:en.
* **Regioner** - Den geografiska region där URL:en citerades.
* **Agentträffar** - Det totala antalet träffar från AI-agenter på URL:erna.
* **Referensträffar** - Antalet träffar som dirigeras från AI-genererade svar till URL:erna.

## Trending URLs Competition for CCitations

De trender-URL:er som tävlar om citationsvyn visar externa URL:er som för närvarande anges i svar som är relevanta för ditt varumärke och som mäter vilka som vinner citat i ditt space. Datatabellen har ett sökfält som ger snabb åtkomst till specifika URL:er. Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.

![Trendar-URL:er som tävlar om källhänvisningar](/help/dashboards/assets/trend-url.png)

Följande mätvärden finns:

* **URL** - den analyserade URL:en
* **Innehållstyp** - Innehållstypen (ägd, social, upparbetad, konkurrent).
* **Tider som anges** - Antalet gånger som URL:en har citerats i AI-genererade svar.
* **Uppmaningar som citeras i** - Antalet unika AI-uppmaningar som refererade till URL:en.
* **Kategorier** - De produktkategorier eller ämnen som är associerade med URL:en.
* **Regioner** - Den geografiska region där URL:en citerades.

### Informationsfönstret

För både den citerade vyn och trendvyn har URL-adresserna en **Detaljer** -knapp när du håller muspekaren över en viss URL-adress. Om du klickar på knappen visas ett separat fönster med mer information. I informationsfönstret visas hur ofta URL:en citeras, känslan av AI-svar där den nämns, ämnen och uppmaningar som den visas i samt trender för agens- och hänvisningstrafik över tid (för ägda URL:er).

![Informationsfönstret](/help/dashboards/assets/details-url.png)
