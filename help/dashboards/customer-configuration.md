---
title: Kundkonfiguration
description: Använd kundkonfigurationen för att definiera hur ert varumärke ska övervakas och analyseras inom plattformen för optimering av livslångt lärande.
feature: Customer Configuration
source-git-commit: 3fab5f21311a741e51e7a31cd3a26de79fcbff95
workflow-type: tm+mt
source-wordcount: '2100'
ht-degree: 0%

---


# Kundkonfiguration {#customer-configuration}

Instrumentpanelen för kundkonfiguration är ett kraftfullt verktyg som ger insikter om hur ert varumärke är synligt för LLM-system. Genom att ställa in kategorier, ämnen, uppmaningar på rätt sätt kan ni se till att ert varumärke är väl positionerat att visas i svar som genererats av LLM. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

![Kontrollpanel för kundkonfiguration](/help/dashboards/assets/customer-config.png)

För att konfigurera hur LLM Optimizer övervakar och analyserar er varumärkesnärvaro på olika marknader och i olika konkurrenslandskap har du tillgång till följande flikar:

* [Fråga](#prompts-brand)
* [Kategorier](#categories)
* [Övriga varumärken](#other-brands)
* [Varumärkesalias](#brand-aliases)
* [CDN-konfiguration](#agentic-cdn)
* [Google Search Console](#google-console)

>[!IMPORTANT]
>
> Mer information om hur du konfigurerar dina kategorier, ämnen, uppmaningar finns på sidan [Bästa tillvägagångssätt för att konfigurera kategorier, ämnen, uppmaningar](/help/overview/best-practices-topics-prompts.md).

## Fråga {#prompts-brand}

På den här fliken kan du granska, hantera och anpassa uppmaningar. Du kan överföra en [varumärkesnärvaroanalys](/help/dashboards/brand-presence.md) .csv och listan fylls i med uppmaningar och ämnen från den analysen eller [Hämta ett frågebibliotek](/help/overview/best-practices-topics-prompts.md) som skapats av Adobe. Du kan även ta bort, ändra och lägga till ämnen och tillhörande frågor efter behov.

Om du vill importera en CSV-fil med datainsikter måste du först exportera en fil från kontrollpanelen för varumärkesnärvaro. Mer information om hur du gör det finns i avsnittet [datainsikter](/help/dashboards/brand-presence.md#data-insights). När du har fått filen:

1. Klicka på **Överför CSV** på kontrollpanelen.
2. Dra och släpp eller välj filen manuellt i fönstret Importera datainsikter.
3. Klicka på **Överför data**.

Du kan också skapa en ny CSV-fil genom att hämta mallen från fönstret **Importera datainsikter**. När du har skapat mallen kan du öppna den och ange dina ämnen tillsammans med tillhörande instruktioner, kategorier och regioner på en ny rad.

Mer information om hur du hämtar och använder branschfrågebiblioteket som skapats av Adobe finns i avsnittet Branschpromptbibliotek på [den här sidan](/help/overview/best-practices-topics-prompts.md)

Dessutom kan du lägga till ämnen/uppmaningar i listan oberoende av en CSV-fil eller ett frågebibliotek. För att uppnå detta måste du göra följande på kontrollpanelen:

1. Klicka på knappen **Lägg till ämne** .
2. Välj **Kategori** i det nya konfigurationsfönstret. Tidigare skapade kategorier visas här.
3. Ange ämnesnamnet.
4. Lägg till uppmaningstexten.
5. Markera området.
6. Klicka på **Lägg till fråga** så visas ämnet med frågan i listan.

>[!NOTE]
>Nyligen tillagda uppmaningar visas inte i Varumärkesnärvaro förrän bearbetningen är klar.

I listan kan du klicka på varje ämne och tillhörande uppmaningar visas. Om du vill ta bort ämnet och tillhörande uppmaningar klickar du på ikonen Ta bort i listan.

## Kategorier {#categories}

På fliken Kategorier kan du definiera affärskategorier eller produktrader som du vill spåra och associera dem med specifika regioner. Fliken Kategorier avser nästan alla andra anpassningar på den här sidan, eftersom kategorier visas i kategorifältet för de andra anpassningarna (andra spårningar, alias och så vidare). Så här lägger du till en ny kategori:

1. Klicka på knappen **Lägg till**.
2. Lägg till **kategorinamn** i det nya konfigurationsfönstret.
3. Anpassa den **associerade regionen** där kategorin ska övervakas.
4. Klicka på **Spara** så visas den nya kategorin i kategorilistan.

Om du lägger till nya kategorier genereras inte ämnen och uppmaningar automatiskt. Dessa måste läggas till manuellt från fliken [Data Insights](#data-insights) .

Om du vill ta bort en kategori klickar du på ikonen Ta bort i kategorilistan. Var försiktig, eftersom **om du tar bort en kategori tas även tillhörande objekt** bort, som varumärkesalias som är länkade till den aktuella kategorin.

## Övriga varumärken {#others-tracking}

På den här fliken kan du spåra hur andra nämns i relation till ert varumärke i olika kategorier och regioner. Övervaka deras närvaro och resultat i era marknadssegment. Så här anpassar du spårning:

1. Klicka på knappen **Lägg till**.
2. Välj **Kategori** i det nya konfigurationsfönstret. Tidigare skapade kategorier visas här.
3. Lägg till den andres namn.
4. Anpassa övriga alias och domäner om det behövs.
5. Klicka på **Spara**.

Klicka på ikonen Ta bort om du vill ta bort en post från listan.

## Varumärkesalias {#brand-aliases}

Genom att använda varumärkesalias kan ni konfigurera alternativa namn och varianter av ert varumärke som ska spåras i olika kategorier och regioner. Detta garanterar en omfattande övervakning av alla varumärkesomnämnanden. Så här lägger du till ett varumärkesalias:

1. Klicka på knappen **Lägg till**.
2. Välj **Kategori** i det nya konfigurationsfönstret. Tidigare skapade kategorier visas här.
3. Välj den **region** där aliaset ska övervakas.
4. Lägg till varumärkesalias.
5. Klicka på **Spara** så visas varumärkesaliaset i listan.

Om du vill ta bort ett varumärkesalias klickar du på ikonen **Ta bort** i aliaslistan.

## CDN-konfiguration {#cdn-configuration}

På den här fliken kan du konfigurera CDN-strömmarna så att Adobe LLM Optimizer kan analysera CDN-data. Dessa data kommer att användas för att driva instrumentpaneler (som AGT-trafik) och ge insikter i trafikmönster, prestandamätningar och optimeringsmöjligheter. Klicka på **Inbyggt CDN** om du vill ta med din CDN-leverantör.

![CDN för kundkonfiguration](/help/overview/assets/cc-cdn.png)

I fönstret **Onboard CDN Provider**:

1. Välj CDN-leverantör.
2. Klicka på **Anonboard** om du vill aktivera vidarebefordran av loggar.

Om du väljer **Annan** måste du kontakta llmo-now@adobe.com för att få hjälp.

## Google Search Console {#google-console}

Med Adobe LLM Optimizer kan du integrera ditt Google Search Console-konto för att få med riktiga sökfrågor direkt i gränssnittet. Genom att gå igenom riktiga Google Search Console-frågor kan du skapa promptuppsättningar som är rundade av sökbeteenden och identifieringsmönster med hög återgivning. Detta hjälper er att prioritera frågor baserat på beprövad efterfrågan och anpassar optimeringsarbetet för livslångt lärande efter hur användarna söker just nu. Dessutom har du full kontroll eftersom frågor aldrig läggs till automatiskt och måste markeras explicit innan du blir aktiv.

### Så här fungerar det {#how-it-works}

Det viktigaste att komma ihåg när det gäller integreringen mellan LLM Optimizer och Google Search Console är följande: I stället för att manuellt gissa vad kunderna kan fråga om en AI-assistent tittar vi på vad de **redan söker efter** och omvandlar dessa riktiga frågor till naturliga, konversationsfrågor. Den här processen att gå från sökfrågor till AI-uppmaningar visas i diagrammet nedan.

![Processflöde](/help/dashboards/assets/diagram-flow.png)

I allmänhet har processen fem steg:

#### Steg 1 - Samla in dina riktiga sökdata {#gsc-one}

Processen börjar med de nyckelord som målgruppen faktiskt använder när den hittar din webbplats via Google. Denna rådatamängd (ofta tusentals unika frågor) är grunden för allt som följer.

#### Steg 2 - Analysera innebörd och filter för att se om de är säkra {#gsc-two}

Varje fråga analyseras efter semantisk innebörd (vad användaren egentligen frågar om) och raderas med ett säkerhetsfilter som tar bort olämpligt innehåll eller innehåll som inte hör till varumärket. Detta garanterar att endast rena, relevanta nyckelord flyttas framåt.

#### Steg 3 - Gruppera i kategorier och ämnen {#gsc-three}

Relaterade frågor grupperas automatiskt i **kategorier** (breda affärsteman) och **ämnen** (fokuserade underämnen inom varje kategori). Systemet prioriterar kategorier som redan har konfigurerats i din LLM Optimizer-konfiguration. Dessutom kan den visa nya kategorier som sökdata visar, men som ännu inte övervakas. Följande diagram är ett exempel på kategorier och ämnen för ett möbelvarumärke:

![Mötesmärke](/help/dashboards/assets/diagram-example.png)

#### Steg 4 - Generera uppmaningar med riktiga nyckelord {#gsc-four}

För varje ämne genererar systemet uppmaningar som liknar hur verkliga människor pratar med AI-assistenter. Varje fråga påverkas direkt av söknyckelord från Google Search Console, vilket omvandlar nyckelordsmetoden till realistiska konversationsfrågor.

Detta tillvägagångssätt (baserat på nyckelord) innebär:

* Frågorna avspeglar den verkliga efterfrågan, inte hypotetiska frågor.
* Språket speglar hur era kunder faktiskt säger saker.
* Täckningen sträcker sig över hela bredden på det som folk söker efter på webbplatsen.

Snabbgenereringen tar också hänsyn till er varumärkesprofil - produkter, konkurrenter, positionering av branschen och målgrupper - för att säkerställa att era budskap är korrekta i sitt sammanhang.

#### Steg 5 - Kvalitetssäkring och leverans {#gsc-five}

Före leverans går varje fråga igenom flera automatiska kvalitetskontroller:

* Borttagning av dubbletter - nästan identiska uppmaningar tas bort.
* Proportionsbalansering - ger en realistisk blandning (~75 % utan varumärke, ~25 % med varumärke).
* Språkkvalitet - strippar robotfraser så att får ljud naturligt.
* Konsekvenskontroller - validerar, tar bort fyllnadsfraser och säkerställer en kortfattad längd.

Dessutom taggas varje uppmaning med kategori, ämne, avsiktstyp och klassificering, med eller utan varumärke, så att LLM Optimizer kan börja övervaka.

#### Uppmana anatomi {#prompt-anatomy}

När processen ovan är klar har varje fråga som skickas till LLM Optimizer följande attribut:

| Fält | Beskrivning |
|---------|----------|
| Text | Uppmaningen, som hur en användare skriver in den i en AI-assistent |
| Kategori | Det breda affärstema som tilldelats den här uppmaningen. |
| Ämne | Det specifika underämnet i kategorin. |
| Län | Målmarknaden (till exempel USA, Storbritannien och så vidare). |
| Återgivning | Användarens inställning: informativ, jämförande, transaktion, instruktion, planering eller delegering. |
| Typ | Typen kan vara antingen varumärkesprofilerad (omnämns varumärket/produkterna) eller oprofilerad (generiskt branschfråga). |

### Så här använder du {#how-to-use}

Följ stegen nedan för att integrera och använda Google Search Console-frågor med LLM Optimizer.

#### Ansluta Google Search Console {#connect-console}

Innan du använder den här funktionen måste du integrera ditt Google Search Console-konto med LLM-optimering.

1. Öppna instrumentpanelen för kundkonfiguration.
1. Gå till fliken Google Search Console och klicka på **Anslut konto**.
   ![Google Search Console](/help/dashboards/assets/google-console.png)
1. Logga in med ett Google-konto som har tillgång till den önskade sökkonsolegenskapen.
   ![Google-konto](/help/dashboards/assets/google-account.png)
1. Välj den egenskap som du vill ansluta.
   ![Konsolegenskap](/help/dashboards/assets/console-property.png)
1. När anslutningen är klar börjar LLM Optimizer hämta relevanta sökfrågor.
   ![Hämtar data](/help/dashboards/assets/console-complete.png)

#### Granska och söka i frågor {#search-query}

När du har integrerat Google Search Console-kontot med LLM-optimering kan du visa listan med ämnen och uppmaningar som kommer från sökkonsolen och lägga till uppmaningar från listan.

1. På fliken Google Search Console granskar du listan över ämnen och uppmaningar som kommer från sökkonsolen.
   ![Frågelista](/help/dashboards/assets/prompts-list.png)
1. Klicka på önskat ämne/promptkategori för att utöka listan.
1. Använd knappen **Lägg till** för att lägga till uppmaningar från listan. Du kan också lägga till uppmaningar och kategorier gruppvis genom att använda **Lägg till alla**.
   ![Lägg till frågor](/help/dashboards/assets/add-prompts.png)
1. När du är nöjd med markeringen klickar du på **Spara** i meddelandet.

#### Visa tillagda frågor i frågelistan {#prompts-list}

När en fråga har lagts till visas den på fliken [Fråga](#prompts-brand) på kontrollpanelen för kundkonfiguration. Fråga som kommer från Google Search Console är märkta med en Google Search Console-ikon i kolumnen **Ursprung** . Ikonen hjälper dig att skilja mellan uppmaningar som är avrundade i faktiskt användarsökbeteende och de som läggs till manuellt eller från andra källor.

### Vanliga frågor {#gsc-faq}

F: Hur ofta uppdateras uppmaningar på kontrollpanelen i Google Search Console?

Fråga som kommer från Google Search Console uppdateras vanligtvis en gång i månaden. Varje uppdatering hämtar de senaste sökfrågedata från Google Search Console, kör om genereringsflödet och uppdaterar uppsättningen frågor. Detta gör att dina uppmaningar hålls i linje med aktuella söktrender och säsongsförändringar i användarbeteendet.

F: Hur många frågor hämtas vanligtvis från Google Search Console?

Antalet beror på storleken på distributionen och antalet kategorier som spåras. Till exempel:

| Kategorier | Totalt antal ämnen | Levererade frågor |
|---------|----------|----------|
| 1-2 | 3-8 | ~65-180 |
| 4-5 | 12-20 | ~270-450 |
| 10 | 30-40 | ~675-900 |

Vi vill leverera snabbmaterial som uppfyller de kvalitetsmål som meddelats under testperioden och introduktionen: minst 20 prompter per ämne, med 3-4 ämnen per kategori och en hälsosam profilerad/oprofilerad balans.

F: Hur snart kommer jag att se uppmaningar som kommer från Google Search Console när jag har anslutit till Google Search Console?

Frågorna är vanligtvis tillgängliga **inom några timmar** efter att din Google Search Console-anslutning har upprättats. Pipelinen hämtar automatiskt sökdata, bearbetar dem genom genererings- och kvalitetssäkringsstegen och skickar sedan den sista prompten till LLM Optimizer.

F: Vem kan ansluta till Google Search Console?

Alla med **Ägare** eller **Fullständig behörighet** i Google Search Console-egenskapen kan auktorisera anslutningen. Detta är behörighetsnivåerna som ger läsåtkomst till sökfrågedata. Om du är osäker på din behörighetsnivå kan du kontrollera den under **Inställningar>Användare** och behörigheter i din Google Search-konsol.

F: Kan jag markera uppmaningar som ignorerade eller överhoppade så att jag inte ser dem i listan med frågor i Google Search Console?

Ja, du kan ta bort alla uppmaningar som du inte vill övervaka. Borttagna uppmaningar tas bort från den aktiva frågelistan och visas inte i framtida rapporter. Om en borttagen fråga genereras om i en efterföljande månadsuppdatering kan du ta bort den igen.

F: När jag väl har lagt till uppmaningar från Google Search Console i min frågelista, hur snart kommer jag att se informationen om varumärkesnärvaro för dessa uppmaningar?

Varumärkesnärvarodata för nytillagda uppmaningar visas under nästa schemalagda datauppdatering, som vanligtvis körs i början av varje vecka. Beroende på när du lägger till uppmaningarna kan du se resultatet inom några dagar.
