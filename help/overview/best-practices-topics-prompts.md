---
title: Bästa praxis för kategorier, ämnen och frågor
description: Beskriv här
source-git-commit: 099d4387b6a5efd25e142db13e309a181fe67941
workflow-type: tm+mt
source-wordcount: '884'
ht-degree: 0%

---


# Intro här

I kundkonfigurationen definieras hur ert varumärke ska övervakas och analyseras inom plattformen för optimering av livslångt lärande. Ni kan anpassa kategorier (t.ex. affärsenheter eller produktlinjer), spåra konkurrenter och lägga till alias för varumärket för att fånga upp alla variationer av ert varumärke i olika uppmaningar. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

## Bästa tillvägagångssätt för att konfigurera kategorier, ämnen och uppmaningar

I det här avsnittet beskrivs de bästa sätten att avgöra hur du vill konfigurera kategorier, ämnen, uppmaningar och konkurrenter.
Detta är ett viktigt första steg. Det ni bestämmer er för avgör nu hur informationen är anpassad efter ert företagskontext. Eventuella ändringar av kategorier i den framtida återställningen av historiska data.

### Bästa tillvägagångssätt för kategorier

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

>[!IMPORTANT]
>
> * Välj ett tillvägagångssätt och håll dig till det.
> * Du kan bara ha **en** kategorimodell per konto/varumärke. Blanda inte **SBU** och **URL_DIR** samtidigt.

Exempel:

| Typ av plats | Kategori | Exempel på ämnestaxonomi |
|---------|----------|---------|
| Företag med flera företag | SBU | liten intent set (How-to, Troubleshooting, Comparison, Pricing, Policy) |
| Stort antal webbplatser för dokumentation/support | URL_DIR | Instruktioner, felsökning, referens, Versionsinformation |
| eCommerce/Services-katalog | Produkt/tjänst | Jämförelse, Recensioner, Priser/Tillgänglighet, Instruktioner, Felsökning |

### Bästa tillvägagångssätt för ämnen

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
* Företag/nyheter (om du verkligen behöver dem)

Tänk på följande när du skapar listan:

* Kan en redigerare förstå ämnet på 5 sekunder från prompttexten? Om inte, byt namn/förenkla.
* Äger ett annat team lösningen för olika ämnen? Om ja, valde du användbara ämnen.

Ytterligare några praktiska tips:

* Använd kunskap om ert företag eller er webbplats för att definiera ämnen som passar ert varumärkes strategiska mål
* Tänk på hur ert varumärke står sig jämfört med konkurrenterna inom specifika områden.

>[!IMPORTANT]
>
> * Behåll ämnen avsiktsbaserade, inte organisatoriska.
> * Lägg inte till kategorier/filter för varumärken/varumärken/andra varumärken/geografiska eftersom du kan filtrera specifikt för detta på fliken **Varumärken** .
> * Ämnen är spridda över flera kategorier. Du kan **inte** ha olika ämnen per kategori.
> * Det kan finnas en enda fråga i flera ämnen eller kategorier.

### Bästa tillvägagångssätt för uppmaningar

Frågar identifierar specifika frågor eller frågor som kunderna ställer, vilket kan påverka ert företag. Det är de faktiska frågor eller frågor som användarna anger i ett LLM-formulär.

Se till att granska och uppdatera uppmaningarna regelbundet för att säkerställa att de är anpassade till kundernas behov och verksamhetsmål.

>[!TIP]
>
>* Du kan använda verktyg som Adobe LLM Optimizer och Google Search Console med regex-filter för att identifiera vanliga frågestrukturer (till exempel&quot;hur&quot;,&quot;vad&quot;,&quot;när&quot;,&quot;var&quot;) och ta reda på vilka uppmaningar folk använder för att besöka din webbplats.
>* För att ta reda på vilka frågor som är relevanta för er webbplats/varumärke kan ni använda sökdata på webbplatsen, Frågor och svar på sökmotorernas resultatsidor, eller till och med ställa frågor direkt om hur kunderna kan ställa frågor om ert varumärke.

### Bästa tillvägagångssätt för konkurrenter

Med hjälp av konkurrenterna kan ni övervaka synlighet och omnämnanden i svaren på frågor som är viktiga för ert företag.

På fliken [!UICONTROL **Konkurrentspårning**] kan du lägga till konkurrenter och spåra deras synlighet för specifika frågor.

Med hjälp av uppföljning av konkurrenter kan ni se hur ofta konkurrenter omnämns tillsammans med ert varumärke i olika regioner och kategorier och jämföra deras synlighet med er egen.

>[!TIP]
>
>Granska regelbundet konkurrenternas omnämnanden och citat för att identifiera områden där ert varumärke kan förbättras.

