---
title: Översikt över vidarebefordran av BYOCDN-logg
description: Lär dig hur du vidarebefordrar CDN-loggar från din leverantör till Adobe S3-bucket för insamling av autentiska trafikdata i LLM Optimizer.
feature: Agentic Traffic
source-git-commit: b6e74e8706c4074a47cc355cb5f3a69a817f8a49
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---


# Översikt över vidarebefordran av BYOCDN-logg {#cdn-log-forwarding}

Loggvidarebefordran för ett kundhanterat CDN (BYOCDN) innebär att du skickar dina CDN-åtkomstloggar till Adobe Amazon S3-bucket så att LLM Optimizer kan samla in och analysera autentiska trafikdata. Utan CDN-loggvidarebefordran kan instrumentpanelen [Agentisk trafik](/help/dashboards/agentic-traffic.md) inte visa mått.

Guiderna nedan följer samma tvåstegsarbetsflöde:

1. **Anmäl dig till LLM Optimizer** - registrera ditt CDN på [CDN-konfigurationssidan](/help/dashboards/customer-configuration.md) för att generera nödvändiga S3-autentiseringsuppgifter och sökvägsinformation.
2. **Konfigurera ditt CDN** - använd dessa uppgifter för att skapa ett jobb för vidarebefordran av loggar (eller för att överföra loggar manuellt) i CDN-providerns konsol. För CloudFront kan du använda konsolen eller slutföra leveransinstallationen med endast **AWS CLI**; se [CloudFront (AWS CLI)](/help/overview/log-forwarding/cloudfront-cli.md).

## CDN-leverantörer {#cdn-providers}

Följ motsvarande guide för din CDN-leverantör.

| CDN-provider | Guide |
|---|---|
| Akamai | [Visa stödlinje](/help/overview/log-forwarding/akamai.md) |
| Cloudflare | [Visa stödlinje](/help/overview/log-forwarding/cloudflare.md) |
| CloudFront (konsol) | [Visa stödlinje](/help/overview/log-forwarding/cloudfront.md) |
| CloudFront (AWS CLI) | [Visa stödlinje](/help/overview/log-forwarding/cloudfront-cli.md) |
| Snabbt | [Visa stödlinje](/help/overview/log-forwarding/fastly.md) |
| Imperva | [Visa stödlinje](/help/overview/log-forwarding/imperva.md) |
| Annat (manuellt/CDN som inte stöds) | [Visa stödlinje](/help/overview/log-forwarding/other.md) |

>[!NOTE]
>
>Om din CDN-leverantör inte finns med i listan ovan använder du handboken **Annan (manuell/ej stödd CDN)** som omfattar manuella överföringar, ad hoc-skript och CDN som inte stöds internt.
