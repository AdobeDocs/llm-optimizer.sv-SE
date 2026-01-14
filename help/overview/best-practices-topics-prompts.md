---
title: Bästa praxis för kategorier, ämnen, frågor och annat
description: Optimera insikterna om det livslånga lärandet genom att konfigurera kategorier, ämnen, uppmaningar och andra varumärken för att spåra även konkurrenter för anpassad varumärkesövervakning och strategisk innehållsanalys.
feature: Best Practices, Customer Configuration
source-git-commit: a4dd9b1aece2936fb95a2e831ec8b41946bc5f46
workflow-type: tm+mt
source-wordcount: '1399'
ht-degree: 0%

---


# Bästa tillvägagångssätt för att konfigurera kategorier, ämnen, uppmaningar och annat att spåra

I det här avsnittet beskrivs de effektivaste strategierna för att bestämma hur du vill konfigurera kategorier, ämnen, uppmaningar och andra för att spåra. Dessutom innehåller det information om branschens promptbibliotek, som Adobe har utvecklat med omfattande forskning med branschexperter.

Detta är ett viktigt första steg. Det ni bestämmer er för avgör nu hur informationen är anpassad efter ert företagskontext. Eventuella ändringar av kategorier i den framtida återställningen av historiska data.

På kontrollpanelen [[!UICONTROL Customer Configuration]](/help/dashboards/customer-configuration.md) kan du definiera hur ditt varumärke ska övervakas och analyseras på plattformen för optimering av användarupplevelser. Mer information om hur du använder instrumentpanelen finns i [[!UICONTROL Customer Configuration]](/help/dashboards/customer-configuration.md).

![Kundkonfigurationsfönstret](/help/assets/best-practices/customer-configuration-best-practices.png)

På [!UICONTROL Customer Configuration]-kontrollpanelen kan du anpassa kategorier (till exempel affärsenheter eller produktlinjer), spåra andra varumärken och lägga till varumärkeskommentarer för att fånga upp alla varianter av ditt varumärke i olika uppmaningar. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

## Branschpromptbibliotek

För att komma igång med frågor och svar har Adobe skapat ett branschfrågebibliotek som utvecklats genom omfattande forskning med branschexperter och analys av AI-sökbeteenden för över 6 000 kunder. Det här biblioteket identifierar de mest relevanta ämnena och uppmaningarna baserat på branschspecifika trender, validerade affärsmål och verkliga kundsökningsmönster.

Så här använder du branschfrågebiblioteket:

1. Hämta filen för frågebiblioteket från LLM Optimizer genom att gå till kontrollpanelen **Kundkonfiguration**.
2. Granska föreslagna **ämnen** och **Fråga** om ert varumärkes bransch på respektive flik och välj de alternativ som är mest relevanta.
3. Granska kolumnen **Kundresa** om du vill visa uppmaningsalternativ under kundens livscykel (till exempel identifiering för konvertering till kvarhållande). Tidiga steg/toppen av funnel-ärenden har hög prioritet, men du bör också överväga alternativ för senare steg för att öka kundlojaliteten, aktivera kundsupport osv.
4. Ändra ämnen eller uppmaningar efter behov för att på bästa sätt stödja dina mål och mål innan du överför till Adobe LLM Optimizer (t.ex. lägg till ditt varumärke/produktnamn, lägg till varumärkesterminologi). Uppmaningar kan läggas till i LLMO manuellt eller gruppvis med hjälp av den tillhandahållna mallen *.csv*.

>[!TIP]
>
> Använd en kombination av domänspecifika frågor som rekommenderas av LLM Optimizer vid den första konfigurationen och branschens promptbibliotek för att strukturera er strategi.

### Fråga Library Research Foundation

The Industry Prompt Library genomgick ett omfattande forskningsprojekt som kombinerade:

* **Kundanalys:** Analys av AI-sökbeteenden och -inställningar för över 6 000 kunder
* **Branschexpertis:** Perspektiv från experter inom kategorierna Auto, Financial Services, Healthcare, Telecom och Travel.
* **Datadrivna insikter:** Identifiering av viktiga ämnen och frågemönster som driver kundengagemang och konvertering.

De vanligaste ämnen som kunder inom olika branscher genomsöker:

* **Auto:** Felsökning av automatiska problem, jämförelse av fordon och finansiering/leasing
* **Finanssektorn:** Utforska finansiella produkter
* **Hälsovård:** Letar efter symtom eller hälsoproblem, jämför behandlingsalternativ, Lär dig labbresultat eller medicinska termer
* **Telekom:** Jämföra planer, avtalsvillkor och kampanjer, Kontrollera tjänsten i det lokala området
* **Resa:** Förbereder en resa, Utforska och boka resor

Kundtrender när det gäller AI-sökning och -uppmaningar i verktygen för livslångt lärande:

* Kunderna föredrar att ställa frågor istället för att använda nyckelord när de använder sökverktygen i LLM.
* De använder främst sökverktyg för att söka i tidiga stadier av forskning och upptäckt.
* Kunderna tenderar att nämna ett visst varumärke eller produktnamn i sina uppmaningar.

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
<!--Can you mix Product/Service with these?-->

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

