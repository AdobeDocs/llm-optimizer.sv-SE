---
title: Kundkonfiguration
description: Använd kundkonfigurationen för att definiera hur ert varumärke ska övervakas och analyseras inom plattformen för optimering av livslångt lärande.
feature: Customer Configuration
source-git-commit: e50c87e8e5a669526f3c10855c1629ce82b67aef
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 0%

---


# Kundkonfiguration {#customer-configuration}

Instrumentpanelen för kundkonfiguration är ett kraftfullt verktyg som ger insikter om hur ert varumärke är synligt för LLM-system. Genom att ställa in kategorier, ämnen, uppmaningar på rätt sätt kan ni se till att ert varumärke är väl positionerat att visas i svar som genererats av LLM. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

![Kontrollpanel för kundkonfiguration](/help/dashboards/assets/customer-config.png)

För att konfigurera hur LLM Optimizer övervakar och analyserar er varumärkesnärvaro på olika marknader och i olika konkurrenslandskap har du tillgång till följande flikar:

* [Kategorier](#categories)
* [Övrig spårning](#others-tracking)
* [Varumärkesalias](#brand-aliases)
* [Datainsikter](#data-insights)
* [CDN-konfiguration](#agentic-cdn)

>[!IMPORTANT]
>
> Mer information om hur du konfigurerar dina kategorier, ämnen, uppmaningar finns på sidan [Bästa tillvägagångssätt för att konfigurera kategorier, ämnen, uppmaningar](/help/overview/best-practices-topics-prompts.md).

## Kategorier {#categories}

På fliken Kategorier kan du definiera affärskategorier eller produktrader som du vill spåra och associera dem med specifika regioner. Fliken Kategorier avser nästan alla andra anpassningar på den här sidan, eftersom kategorier visas i kategorifältet för de andra anpassningarna (andra spårningar, alias och så vidare). Så här lägger du till en ny kategori:

1. Klicka på knappen **Lägg till**.
2. Lägg till **kategorinamn** i det nya konfigurationsfönstret.
3. Anpassa den **associerade regionen** där kategorin ska övervakas.
4. Klicka på **Spara** så visas den nya kategorin i kategorilistan.

Om du lägger till nya kategorier genereras inte ämnen och uppmaningar automatiskt. Dessa måste läggas till manuellt från fliken [Data Insights](#data-insights) .

Om du vill ta bort en kategori klickar du på ikonen Ta bort i kategorilistan. Var försiktig, eftersom **om du tar bort en kategori tas även tillhörande objekt** bort, som varumärkesalias som är länkade till den aktuella kategorin.

## Övrig spårning {#others-tracking}

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

Om du vill ta bort ett varumärkesalias klickar du på ikonen Ta bort i aliaslistan.

## Datainsikter {#data-insights}

På den här fliken kan du granska, hantera och anpassa uppmaningar. Du kan överföra en [varumärkesidentifierare](/help/dashboards/brand-presence.md#data-insights) .csv och listan fylls med uppmaningar och ämnen från den analysen. Du kan även ta bort, ändra och lägga till ämnen och tillhörande frågor efter behov.

Om du vill importera en CSV-fil med datainsikter måste du först exportera en fil från kontrollpanelen för varumärkesnärvaro. Mer information om hur du gör det finns i avsnittet [datainsikter](/help/dashboards/brand-presence.md#data-insights). När du har fått filen:

1. Klicka på **Överför CSV** på kontrollpanelen.
2. Dra och släpp eller välj filen manuellt i fönstret Importera datainsikter.
3. Klicka på **Överför data**.

Du kan också skapa en ny CSV-fil genom att hämta mallen från fönstret **Importera datainsikter**. När du har skapat mallen kan du öppna den och ange dina ämnen tillsammans med tillhörande instruktioner, kategorier och regioner på en ny rad.

Dessutom kan du lägga till ämnen/uppmaningar i listan oberoende av en CSV-fil. För att uppnå detta måste du göra följande på kontrollpanelen:

1. Klicka på knappen **Lägg till ämne** .
2. Välj **Kategori** i det nya konfigurationsfönstret. Tidigare skapade kategorier visas här.
3. Ange ämnesnamnet.
4. Lägg till uppmaningstexten.
5. Markera området.
6. Klicka på **Lägg till fråga** så visas ämnet med frågan i listan.

>[!NOTE]
>Nyligen tillagda uppmaningar visas inte i Varumärkesnärvaro förrän bearbetningen är klar.

I listan kan du klicka på varje ämne och tillhörande uppmaningar visas. Om du vill ta bort ämnet och tillhörande uppmaningar klickar du på ikonen Ta bort i listan.

## CDN-konfiguration {#cdn-configuration}

På den här fliken kan du konfigurera CDN-strömmarna så att Adobe LLM Optimizer kan analysera CDN-data. Dessa data kommer att användas för att driva instrumentpaneler (som AGT-trafik) och ge insikter i trafikmönster, prestandamätningar och optimeringsmöjligheter. Klicka på **Inbyggt CDN** om du vill ta med din CDN-leverantör.

![CDN för kundkonfiguration](/help/overview/assets/cc-cdn.png)

I fönstret **Onboard CDN Provider**:

1. Välj CDN-leverantör.
2. Klicka på **Anonboard** om du vill aktivera vidarebefordran av loggar.

Om du väljer **Annan** måste du kontakta llmo-now@adobe.com för att få hjälp.
