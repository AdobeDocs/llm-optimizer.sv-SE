---
title: URL-kontroll
description: Lär dig hur du använder URL-kontrollen för att analysera hur specifika sidor i din domän fungerar vid AI-sökningar.
feature: URL Inspector
source-git-commit: e50c87e8e5a669526f3c10855c1629ce82b67aef
workflow-type: tm+mt
source-wordcount: '681'
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
* **Totalt antal autentiska träffar** - det totala antalet träffar från AI-agenter på dina URL:er.
* **Referensträffar från LLM:er** - Det totala antalet träffar som dirigeras från AI-genererade svar till dina URL:er.

Trendindikatorer för varje översiktsmått visar hur dessa värden ändras över tiden jämfört med föregående period.

## Dina beställda URL:er

De citerade URL:erna visar alla URL:er från ert varumärke som har citerats i AI-genererade svar, med stödjande mått. Båda tabellerna har ett sökfält för snabb åtkomst till ämnen och du kan anpassa vilka mätvärden som visas genom att klicka på knappen **Konfigurera kolumner** . Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.

![Citerade URL:er](/help/dashboards/assets/cited-urls.png)

Följande mätvärden finns:

* **URL** - den analyserade URL:en.
* **Tider som anges** - Antalet gånger som URL:en har citerats i AI-genererade svar.
* **Uppmaningar som citeras i** - Antalet unika AI-uppmaningar som refererade till URL:en.
* **Kategorier** - De produktkategorier eller ämnen som är associerade med URL:en.
* **Regioner** - Den geografiska region där URL:en citerades.
* **Agentträffar** - Det totala antalet träffar från AI-agenter på URL:erna.
* **Referensträffar** - Antalet träffar som dirigeras från AI-genererade svar till URL:erna.

## Trending URLs Competition for CCitations

De trendwebbadresser som tävlar om citationsvyn visar externa webbadresser som för närvarande anges i svar som är relevanta för ditt varumärke, och som mäter vilka som vinner citat i ditt space. Datatabellen har ett sökfält som ger snabb åtkomst till specifika URL:er. Du kan också använda alternativet **Exportera** för att hämta tabellen .csv och dela insikterna med ditt team eller inkludera tabellen i den verkställande rapporten.

![Trendar-URL:er som tävlar om källhänvisningar](/help/dashboards/assets/trend-url.png)

Följande mätvärden finns:

* **URL** - den analyserade URL:en
* **Innehållstyp** - Innehållstypen (ägt, socialt, förtjänat, övrigt).
* **Tider som anges** - Antalet gånger som URL:en har citerats i AI-genererade svar.
* **Uppmaningar som citeras i** - Antalet unika AI-uppmaningar som refererade till URL:en.
* **Kategorier** - De produktkategorier eller ämnen som är associerade med URL:en.
* **Regioner** - Den geografiska region där URL:en citerades.

### Informationsfönstret

För både den citerade vyn och trendvyn har URL-adresserna en **informationsknapp** i slutet av varje rad. Om du klickar på knappen visas ett separat fönster med ytterligare information. I informationsfönstret visas hur ofta URL:en citeras, <!--the sentiment of AI responses where it is mentioned,--> ämnen och uppmaningar visas i den och trender för aginal och hänvisningstrafik över tid (för ägda URL:er).

![Informationsfönstret](/help/dashboards/assets/details-url.png)
