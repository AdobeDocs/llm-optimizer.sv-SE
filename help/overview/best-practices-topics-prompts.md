---
title: Bästa praxis för kategorier, ämnen, frågor och annat
description: Optimera insikterna om det livslånga lärandet genom att konfigurera kategorier, ämnen, uppmaningar och andra varumärken för att spåra även konkurrenter för anpassad varumärkesövervakning och strategisk innehållsanalys.
source-git-commit: 06c09c4690488d9b29d0626521fb53bf7d70d3a8
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---


# Bästa tillvägagångssätt för att konfigurera kategorier, ämnen, uppmaningar och annat att spåra

I det här avsnittet beskrivs de effektivaste strategierna för att bestämma hur du vill konfigurera kategorier, ämnen, uppmaningar och andra för att spåra.

Detta är ett viktigt första steg. Det ni bestämmer er för avgör nu hur informationen är anpassad efter ert företagskontext. Eventuella ändringar av kategorier i den framtida återställningen av historiska data.

På kontrollpanelen [[!UICONTROL Customer Configuration]](/help/dashboards/customer-configuration.md) kan du definiera hur ditt varumärke ska övervakas och analyseras på plattformen för optimering av användarupplevelser. Mer information om hur du använder instrumentpanelen finns i [[!UICONTROL Customer Configuration]](/help/dashboards/customer-configuration.md).

![Kundkonfigurationsfönstret](/help/assets/best-practices/customer-configuration-best-practices.png)

På [!UICONTROL Customer Configuration]-kontrollpanelen kan du anpassa kategorier (till exempel affärsenheter eller produktlinjer), spåra andra varumärken och lägga till varumärkeskommentarer för att fånga upp alla varianter av ditt varumärke i olika uppmaningar. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

## Bästa tillvägagångssätt för kategorier

Med kategorier kan du ordna innehåll i strategiska affärsenheter eller logiska grupperingar. Det är&quot;var det hör&quot; och den översta organisationsstrukturen för ert innehåll.

När du bestämmer dig för hur du ställer in kategorier måste du ta hänsyn till dina mål och vem som behöver agera utifrån vad du rapporterar.

>[!IMPORTANT]
>
> Se till att kategorierna är korrekt konfigurerade från början, eftersom ändringar av kategorier återställer historiska data. Det innebär att om du ändrar dessa, förlorar du historiska data från före ändringen.

Här är en översikt över vilka typer av strategier du kan välja och när du vill välja ett visst tillvägagångssätt:

| Metod | Beskrivning | Fördelar |
|---------|----------|---------|
| Strategic Business Unit (SBU) | Använd den här metoden om din organisation är uppdelad efter lista (t.ex. konsument, företag, tjänster). | Ni får rena rapporter efter affärsområde och enklare spårbarhet. |
| Katalog på högsta nivån för webbplatser (URL_DIR) | Använd det här alternativet om webbplatsens informationsarkitektur speglar ägarskap (/products/, /support/, /docs/, /news/). | Ni anpassar er efter hur team publicerar och underhåller innehåll. |
| Produktkategori (eller tjänstekategori) | Använd den om du säljer en katalog (SKU/tjänster). | Du får sortimentsvyer, mellanrumsanalyser och svar om vilken kategori som behöver innehåll. |

Hur du avgör hur du ställer in kategorier baseras på en fråga: **Vem behöver agera på rapporten?**

* Om du är *företagsledare* väljer du **SBU**-metoden.
* Om du är *webb-/innehållsägare* väljer du **URL_DIR**-metoden.
* Om du är *sälj-/erbjudandeansvarig* väljer du **produkt-/tjänstkategori**.

![Lägga till kategorier i LLM Optimizer](/help/assets/best-practices/add-category.png)

>[!IMPORTANT]
>
> * Välj ett tillvägagångssätt och håll dig till det.
> * Du kan bara ha **en** kategorimodell per konto/varumärke. Blanda inte **SBU** och **URL_DIR** samtidigt.
>   <!--Can you mix Product/Service with these?-->

Exempel:

| Typ av plats | Kategori | Exempel på ämnestaxonomi |
|---------|----------|---------|
| Företag med flera företag | SBU | Liten metod (handledning, felsökning, jämförelse, priser, policy) |
| Stort antal webbplatser för dokumentation/support | URL_DIR | Instruktioner, felsökning, referens, Versionsinformation |
| eCommerce/Services-katalog | Produkt/tjänst | Jämförelse, Recensioner, Priser/Tillgänglighet, Instruktioner, Felsökning |

## Bästa tillvägagångssätt för ämnen

Ämnen hjälper dig att förstå användaravsikten - de visar vad användaren vill ha. De gör att du kan gruppera frågor med liknande användaravsikt. Se det som att samla relevanta tips tillsammans.

>[!IMPORTANT]
>
>Ämnen är universella i **alla** kategorier, vilket innebär att de förblir konsekventa oavsett vilken kategori de tilldelas. De representerar grupper av frågor eller frågor som delar en gemensam avsikt.

När du bestämmer dig för ämnen vill du skapa en kort, platt lista (max 6-12). Till exempel:

* Produkter/tjänster
* Instruktioner (konfiguration/användning)
* Felsökning (fel/problem)
* Jämförelse (X vs Y; &quot;best ... for ...&quot;)
* Recensioner och omdömen
* Priser och tillgänglighet
* Policy och garanti
* Supportkontakt
* Corporate/News (om du verkligen behöver detta)

![Lägga till ämnen i LLM Optimizer](/help/assets/best-practices/add-topic.png)

Tänk på följande när du skapar listan:

* Kan en redigerare förstå ämnet på 5 sekunder från prompttexten? Om inte, byt namn/förenkla.
* Äger ett annat team lösningen för olika ämnen? Om ja, valde du användbara ämnen.
  <!-- Last bullet point does not make sense. Clarification needed. Also not sure what is meant by "editor"?-->

Ytterligare några praktiska tips:

* Använd kunskap om ert företag eller er webbplats för att definiera ämnen som passar ert varumärkes strategiska mål
* Tänk på hur ert varumärke står sig jämfört med andra varumärken inom specifika ämnen.

>[!IMPORTANT]
>
> * Behåll ämnen avsiktsbaserade, inte organisatoriska.
> * Lägg inte till kategorier/filter för märke/icke-varumärke/geografisk eftersom du kan filtrera specifikt för detta på fliken **[!UICONTROL Brands]**.
> * Ämnen sprids ut över flera kategorier. Du **kan inte** definiera unika ämnen för varje kategori.
> * En enda fråga **kan** finnas i flera ämnen eller kategorier.

## Bästa tillvägagångssätt för uppmaningar

Frågar identifierar specifika frågor eller frågor som kunderna ställer, vilket kan påverka ert företag. Det är de faktiska frågor eller frågor som användarna anger i ett LLM-formulär.

Se till att granska och uppdatera uppmaningarna regelbundet för att säkerställa att de är anpassade till kundernas behov och verksamhetsmål.

Bästa tillvägagångssätt för uppmaningar:

* Gruppera liknande uppmaningar baserat på vad andra frågar.
* Fokusera på de frågor som är viktigast för era kunder.
* Kontrollera om ert varumärke har goda möjligheter att omnämnas för vissa frågor.

>[!TIP]
>
>* Du kan använda verktyg som Adobe LLM Optimizer och Google Search Console med regex-filter för att identifiera vanliga frågestrukturer (till exempel&quot;hur&quot;,&quot;vad&quot;,&quot;när&quot;,&quot;var&quot;) och ta reda på vilka uppmaningar folk använder för att besöka din webbplats.
>* För att ta reda på vilka frågor som är relevanta för er webbplats/varumärke kan ni använda sökdata på webbplatsen, Frågor och svar på sökmotorernas resultatsidor, eller till och med ställa frågor direkt om hur kunderna kan ställa frågor om ert varumärke.

## Bästa tillvägagångssätt för att spåra andra varumärken

Med hjälp av spårning av andra kan du övervaka synlighet och omnämnanden i LLM-svar för frågor och ämnen som är viktiga för ditt företag.

På fliken [!UICONTROL **Övrig spårning**] kan du lägga till andra, inklusive konkurrenter, för att spåra deras synlighet för specifika frågor och ämnen.

Med andra spårningsfunktioner kan ni se hur ofta andra varumärken nämns tillsammans med ert varumärke i olika regioner och kategorier och jämföra deras synlighet med er egen.

>[!TIP]
>
>regelbundet granska konkurrenterna eller andra omnämns och citeras för att identifiera områden där ert varumärke kan förbättras.

## Läs mer

* [Kontrollpanelen för kundkonfiguration](/help/dashboards/customer-configuration.md) är den plats där du konfigurerar kategorier, ämnen, uppmaningar och annan spårning.
* [LLM Optimizer bästa praxis](/help/tutorials/best-practices.md) beskriver de effektivaste strategierna för optimering av LLM

