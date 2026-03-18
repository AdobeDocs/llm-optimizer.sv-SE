---
title: Logga vidare - snabbt
description: Lär dig hur du vidarebefordrar CDN-loggar från Fast till Adobe S3-bucket för insamling av autentiska trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# Loggvidarebefordran: Snabbt {#log-forwarding-fastly}

På den här sidan beskrivs hur du vidarebefordrar CDN-loggar från Fastly till Adobe S3-bucket för insamling av agentiska trafikdata. Du kommer att använda konfigurationssidan för LLM Optimizer CDN för att komma in på LLM Optimizer. När introduktionsprocessen är klar följer du stegen på den här sidan för att konfigurera vidarebefordran av loggar i webbkonsolen Snabbt.

## Steg 1: Anställ i LLM Optimizer {#step-1}

På LLM Optimizer-sidan [https://llmo.now/](https://llmo.now/):

1. Gå till **Konfiguration**.

   ![Konfigurationsknappen](/help/overview/assets/log-forwarding/common/config-button.png)

1. Klicka på fliken **CDN-konfiguration**.

   ![fliken Konfiguration i CDN](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. Klicka på **Kom igång**.
1. Klicka på **Konfigurera** bredvid **Aktivera AI-trafikinformation**.

   ![Konfigurera](/help/overview/assets/log-forwarding/common/configure.png)
1. Välj **Snabbt (BYOCDN)**.

   ![Välj snabbt](/help/overview/assets/log-forwarding/fastly/fastly-select.png)
1. Klicka på **Anonboard**.

## Steg 2: Skapa en S3-slutpunkt snabbt {#step-2}

Om du vill skapa en S3-slutpunkt går du till **snabbkontrollpanelen**:

1. Gå till **CDN-tjänster** (inte Compute-tjänster) på snabbpanelen.
1. Klicka på **Skapa slutpunkt** i området **Amazon Web Services S3**.
1. Fyll i fälten **Skapa en Amazon S3-slutpunkt**:

| Fält | Beskrivning |
| --- | --- |
| **Namn** | Namn som kan läsas av människor för slutpunkten. |
| **Placement** | Standard |
| **Loggformat** | Använd loggformatssträngen som visas i avsnittet **Loggformatsträng** nedan. |
| **Tidsstämpelformat** | `%Y-%m-%dT%H:%M:%S.000` |
| **Bucket-namn** | Kopiera **Bucket-namnet** från LLM Optimizer konfigurationssida. ![Bucket-namn &#x200B;](/help/overview/assets/log-forwarding/fastly/fastly-bucket-name.png) |
| **Domän** | Kopiera **domännamnet** från LLM Optimizer konfigurationssida. ![Domännamn &#x200B;](/help/overview/assets/log-forwarding/fastly/fastly-domain-name.png) |
| **Åtkomstmetod** | **Användarautentiseringsuppgifter** |
| **Användarautentiseringsuppgifter** | Kopiera **Access Key** och **Secret Key** från konfigurationssidan för LLM Optimizer. ![Åtkomstnycklar &#x200B;](/help/overview/assets/log-forwarding/common/access-keys.png) |
| **Period** | `300` |

**Loggformatsträng:**

```json
{ "timestamp": "%{strftime(\{"%Y-%m-%dT%H:%M:%S%z"\}, time.start)}V", "host": "%{if(req.http.Fastly-Orig-Host, req.http.Fastly-Orig-Host, req.http.Host)}V", "url": "%{json.escape(req.url)}V", "request_method": "%{json.escape(req.method)}V", "request_referer": "%{json.escape(req.http.referer)}V", "request_user_agent": "%{json.escape(req.http.User-Agent)}V", "response_status": %{resp.status}V, "response_content_type": "%{json.escape(resp.http.Content-Type)}V", "client_country_code": "%{client.geo.country_name}V", "time_to_first_byte": "%{time.to_first_byte}V" }
```

>[!WARNING]
>
>Lösenordshanteraren kan automatiskt fylla i fältet **Hemlig nyckel** med ditt snabba lösenord. Om AWS-integreringen misslyckas anger du hemlig nyckel manuellt.

När du har slutfört stegen ovan klickar du på **Avancerade alternativ** och anger:

| Fält | Beskrivning |
| --- | --- |
| **Sökväg** | Kopiera **Sökväg** från LLM Optimizer konfigurationssida. ![Sökväg &#x200B;](/help/overview/assets/log-forwarding/fastly/fastly-path.png) |
| **Välj ett loggradformat** | Tom |
| **Komprimering** | Gzip |
| **Redundansnivå** | Standard |
| **ACL** | Ingen |
| **Kryptering på serversidan** | Ingen |
| **Maximalt antal byte** | 0 |

Efter inställning av de avancerade alternativen:

1. Klicka på **Skapa** för att skapa slutpunkten.
1. På menyn **Aktivera** väljer du **Aktivera vid produktion** att distribuera.

>[!NOTE]
>
>Direktuppspelar snabbt loggar kontinuerligt till S3, på S3:s webbplats och i API:t blir filerna bara tillgängliga när överföringen är klar.

### Exempel på loggpost {#example}

Nedan visas ett exempel på en formatsträng för att skicka data till Amazon S3:

```json
{
  "timestamp": "2026-02-10T05:05:36+0000",
  "host": "example.com",
  "url": "/my/path",
  "request_method": "GET",
  "request_referer": "https://example.com/my/other/path",
  "request_user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1",
  "response_status": 200,
  "response_content_type": "text/css; charset=utf-8",
  "client_country_code": "argentina",
  "time_to_first_byte": "0.138"
}
```
