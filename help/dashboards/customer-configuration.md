---
title: Kundkonfiguration
description: Det här är artikelöversikten.
source-git-commit: e35ddb9b055d2f974fd94b3a21e13e92d05c8799
workflow-type: tm+mt
source-wordcount: '723'
ht-degree: 0%

---


# Kundkonfiguration

I kundkonfigurationen definieras hur ert varumärke ska övervakas och analyseras inom plattformen. Ni kan anpassa kategorier (t.ex. affärsenheter eller produktlinjer), spåra konkurrenter och lägga till alias för varumärket för att fånga upp alla variationer av ert varumärke i olika uppmaningar. Med den här konfigurationen kan plattformen skräddarsy insikter efter verksamhetens sammanhang, vilket möjliggör korrekt synlighet, trafik och analys av affärsmöjligheter.

![Trendar-URL:er som tävlar om källhänvisningar](/help/dashboards/assets/customer-config.png)

För att konfigurera hur LLM Optimizer övervakar och analyserar er varumärkesnärvaro på olika marknader och i olika konkurrenslandskap har du tillgång till följande flikar:

* [Kategorier](#categories)
* [Konkurrentspårning](#competitor-tracking)
* [Varumärkesalias](#brand-aliases)
* [Datainsikter](#data-insights)
* [Agence CDN](#agentic-cdn)

## Kategorier {#categories}

På fliken Kategorier kan du definiera affärskategorier eller produktrader som du vill spåra och associera dem med specifika regioner. På det hela taget gäller kategorifliken för alla andra anpassningar på den här sidan, eftersom kategorier visas i kategorifältet för andra anpassningar (konkurrentspårning, alias och så vidare).Så här lägger du till en ny kategori:

1. Klicka på knappen **Lägg till**.
2. Lägg till **kategorinamn** i det nya konfigurationsfönstret.
3. Anpassa den **associerade regionen** där kategorin ska övervakas.
4. Klicka på **Spara** så visas den nya kategorin i kategorilistan.

Om du lägger till nya kategorier genereras inte ämnen och uppmaningar automatiskt. Dessa måste läggas till manuellt från fliken [Data Insights](#data-insights) .

Om du vill ta bort en kategori klickar du på ikonen Ta bort i kategorilistan. Var försiktig, eftersom **om du tar bort en kategori tas även tillhörande objekt** bort, precis som konkurrenter som du har konfigurerat eller varumärkesalias som är kopplade till den aktuella kategorin.

## Konkurrentspårning {#competitor-tracking}

Genom att använda konkurrentspårning kan ni spåra hur konkurrenterna nämns i förhållande till ert varumärke i olika kategorier och regioner. Övervaka deras närvaro och resultat i era marknadssegment. Så här anpassar du konkurrentspårning:

1. Klicka på knappen **Lägg till** om du vill lägga till en ny konkurrent.
2. Välj **Kategori** i det nya konfigurationsfönstret. Tidigare skapade kategorier visas här.
3. Lägg till konkurrentens namn.
4. Anpassa konkurrentens alias och domäner om det behövs.
5. Klicka på **Spara** så visas den nya konkurrenten i konkurrentlistan.

Om du vill ta bort en konkurrent klickar du på ikonen Ta bort i konkurrentlistan.

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

## Agence CDN {#agentic-cdn}

Inte tillgängligt (kommer det att vara tillgängligt för release?).

