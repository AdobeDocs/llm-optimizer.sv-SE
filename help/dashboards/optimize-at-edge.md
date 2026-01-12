---
title: Optimera på Edge
description: Lär dig leverera optimeringar i LLM Optimizer i CDN-kanten utan att behöva göra några redigeringsändringar.
feature: Opportunities
source-git-commit: 2311bd2990c6ff7ecee22ca82b25987df10e7e1c
workflow-type: tm+mt
source-wordcount: '2188'
ht-degree: 0%

---


# Optimera på Edge

På den här sidan finns en detaljerad översikt över hur du kan leverera optimeringar vid CDN-kanten utan att behöva göra några redigeringsändringar. Det handlar om introduktionsprocessen, tillgängliga optimeringsmöjligheter och hur ni kan optimera automatiskt vid behov.

>[!NOTE]
>Den här funktionen är för närvarande i tidig åtkomst. Du kan läsa mer om Tidig åtkomst-program [här](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs).

## Vad är Optimize på Edge?

Optimize at Edge är en edge-baserad driftsättningsfunktion i LLM Optimizer som ger AI-vänliga ändringar av användaragenter för livslångt lärande. I det aktuella sammanhanget innebär&quot;Edge&quot; att optimeringen används i CDN-lagret. Eftersom det levererar optimeringar i CDN-lagret behövs inga redigeringsändringar i Content Management System (CMS), vilket innebär att CMS inte ändras. Tack vare den här separationen kan du förbättra LLM-synligheten utan att ändra befintliga publiceringsarbetsflöden. Den riktar sig endast till agell trafik och påverkar varken mänskliga användare eller SEO-robotar. När LLM Optimizer upptäcker möjligheter att optimera en sida kan man driftsätta korrigeringar direkt vid CDN-kanten.

Optimera på Edge är ett snabbare och smidigare alternativ till traditionella korrigeringar som kräver komplexa konstruktionssatsningar. När du har slutfört en engångskonfiguration behöver du som sagt inte göra några plattformsändringar eller längre utvecklingscykler för att ändringarna ska börja gälla. Ni kan publicera förbättringar på några minuter utan att behöva något engagemang från utvecklarna. Det är ett kodfritt sätt att optimera webbplatsen för AI-agenter.

Optimera på Edge är utformat för företagsanvändare i team som arbetar med marknadsföring, sökmotoroptimering, innehåll och digital strategi. Det kan göra det möjligt för företagsanvändare att slutföra hela kundresan i LLM Optimizer: identifiera möjligheter, förstå förslag och enkelt implementera korrigeringarna. Med Optimize på Edge kan man förhandsgranska ändringarna, snabbt driftsätta dem vid CDN-kanten och validera att optimeringarna är aktiva. Prestanda kan spåras i LLM Optimizer ekosystem.

### Viktiga fördelar

* **Leverans endast för AI:** Serverar optimerade HTML endast för AI-agenter utan påverkan på vare sig besökare eller SEO-robotar.
* **Snabbare cykler:** Publicera ändringar på några minuter, inte veckor. Inga plattformsändringar eller långa konstruktionscykler krävs.
* **Reversibel:** Stöds med en enklicksåterställning som kan återställa sidan på några minuter.
* **Ingen prestandapåverkan:** Edge-baserade optimeringar och cachelagring förhindrar att webbplatsens latens påverkas.
* **CDN och CMS-agnostiker:** Fungerar med alla CDN-konfigurationer och frontendkonfigurationer oavsett Content Management System.

### Vilka möjligheter stöds av Optimize på Edge?

Möjligheter som kan förbättra den autentiska webbupplevelsen stöds av Optimize på Edge. Läs mer om varje affärsmöjlighet både på sidan [Affärsmöjligheter](/help/dashboards/opportunities.md) och i avsnittet om affärsmöjligheter på den aktuella sidan.

## Onboarding

Du bör kontakta antingen ditt Adobe-kontoteam eller FDE-teamet för att starta introduktionsprocessen. IT- eller CDN-teamet måste också slutföra installationen. Du kan även kontakta `llmo-at-edge@adobe.com` om du vill ha mer hjälp med introduktionen.

Krav för att kunna utnyttja Optimera på Edge:

* Slutför introduktionsprocessen till LLM Optimizer.
* Slutför vidarebefordringsprocessen för CDN-loggarna.

Krav för IT-avdelningen:

* Generera en API-nyckel.
* Lägg till Optimize på Edge routningsregler i CDN.
* Tillåtslista användardefinierade sökvägar för hela domänen.
* Lägg till en användardefinierad lista med användaragenter för LLM i målgruppen.
* Kontrollera att `robots.txt` inte blockerar några användaragenter som är avsedda att vara målanvändare.
* Bekräfta Optimera vid Edge-routning i LLM Optimizer gränssnitt.

Som vägledning för konfigurationsprocessen, som presenteras nedan, är exempelkonfigurationer för ett antal CDN-inställningar. Tänk på att dessa exempel bör anpassas till den aktuella livekonfigurationen. Vi rekommenderar att du gör ändringar i de lägre miljöerna först.

>[!NOTE]
>I kodexemplen nedan kan du se referenser till &quot;tokowaka&quot;, som är arbetsprojektets namn för Optimera på Edge. Dessa identifierare finns kvar i koden av kompatibilitetsskäl och hänvisar till samma funktioner som beskrivs i den här dokumentationen.

>[!BEGINTABS]

>[!TAB Hanterad CDN för Adobe]

**Hanterad CDN för Adobe**

Syftet med den här konfigurationen är att konfigurera begäranden med agentiska användaragenter som dirigeras till optimeringstjänsten (`edge.tokowaka.now` backend). Testa konfigurationen genom att söka efter rubriken `x-tokowaka-request-id` i svaret när konfigurationen är klar.

```
curl -svo page.html https://frescopa.coffee/about-us --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-tokowaka-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

Cirkulationskonfigurationen görs med en [originSelector CDN-regel](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors). Förutsättningarna är följande:

* bestämma vilken domän som ska dirigeras
* bestämma vilka banor som ska dirigeras
* bestämma vilka användaragenter som ska dirigeras (rekommenderad regex)

För att kunna distribuera regeln måste du:

* skapa en [konfigurationspipeline](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/config-pipeline)
* implementera konfigurationsfilen `cdn.yaml` i din databas
* köra konfigurationsflödet


```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Tokowaka backend
  originSelectors:
    rules:
      - name: route-to-tokowaka-backend
        when:
          allOf:
            - reqHeader: x-tokowaka-request
              exists: false # avoid loops when requests comes from Tokowaka
            - reqHeader: user-agent
              matches: "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: tokowaka-backend
    origins:
      - name: tokowaka-backend
        domain: "edge.tokowaka.now"
```

Testa installationen genom att köra en vändning och förvänta dig följande:

```
curl -svo page.html https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-tokowaka-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

<!-- >>[!TAB Akamai (BYOCDN)]

**Tokowaka BYOCDN - Akamai**

```
{
    "name": "Project Tokowaka CDN Rule",
    "children": [
        {
            "name": "Connection settings",
            "children": [],
            "behaviors": [
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.health-detect.status>off</forward:availability.health-detect.status>\n<forward:availability>\n<max-reforwards>1</max-reforwards>\n<max-reconnects>1</max-reconnects>\n</forward:availability>\n<match:forward.server-type value=\"CUSTOMER_ORIGIN\">\n<network:http.read>%(PMUSER_HTTP_READ)</network:http.read>\n<network:http.first-byte-timeout>%(PMUSER_FIRST_BYTE_TIMEOUT)</network:http.first-byte-timeout>\n<network:http.connect-timeout>%(PMUSER_HTTP_CONNECT_TIMEOUT)</network:http.connect-timeout> \n</match:forward.server-type>"
                    },
                    "uuid": "4a8c027b-1b23-44a7-8e12-f8d07e453679",
                    "templateUuid": "41c77091-419f-43f2-9a84-0b406b050cc8"
                }
            ],
            "uuid": "4759571b-8036-4c16-9b60-d3aeb84958f1",
            "criteria": [],
            "criteriaMustSatisfy": "all"
        },
        {
            "name": "Site Failover Behavior",
            "children": [],
            "behaviors": [
                {
                    "name": "failAction",
                    "options": {
                        "actionType": "RECREATED_CO",
                        "contentCustomPath": false,
                        "contentHostname": "www.adobe.com",
                        "enabled": true
                    }
                },
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.fail-action2>\n<add-header>\n<status>on</status>\n<name>x-tokowaka-request</name>\n<value>fo</value>\n</add-header>\n</forward:availability.fail-action2>"
                    }
                }
            ],
            "uuid": "b3000c12-1ab8-49b1-a5d0-75e58bb18c9c",
            "criteria": [
                {
                    "name": "matchResponseCode",
                    "options": {
                        "lowerBound": 400,
                        "matchOperator": "IS_BETWEEN",
                        "upperBound": 599
                    }
                },
                {
                    "name": "originTimeout",
                    "options": {
                        "matchOperator": "ORIGIN_TIMED_OUT"
                    }
                }
            ],
            "criteriaMustSatisfy": "any",
            "comments": "If Tokowaka origin returns a 4xx or 5xx error (or times out), failover condition is to send the request back to Akamai and set the x-tokowaka-request header so we don't re-send the request to Tokowaka"
        }
    ],
    "behaviors": [
        {
            "name": "origin",
            "options": {
                "cacheKeyHostname": "ORIGIN_HOSTNAME",
                "compress": true,
                "customValidCnValues": [
                    "{{Origin Hostname}}",
                    "{{Forward Host Header}}",
                    "*.tokowaka.now"
                ],
                "enableTrueClientIp": true,
                "forwardHostHeader": "ORIGIN_HOSTNAME",
                "hostname": "edge.tokowaka.now",
                "httpPort": 80,
                "httpsPort": 443,
                "ipVersion": "IPV4",
                "minTlsVersion": "DYNAMIC",
                "originCertificate": "",
                "originCertsToHonor": "STANDARD_CERTIFICATE_AUTHORITIES",
                "originSni": true,
                "originType": "CUSTOMER",
                "ports": "",
                "standardCertificateAuthorities": [
                    "akamai-permissive",
                    "THIRD_PARTY_AMAZON"
                ],
                "tlsVersionTitle": "",
                "trueClientIpClientSetting": true,
                "trueClientIpHeader": "True-Client-IP",
                "verificationMode": "CUSTOM"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_LLMCLIENT",
                "variableValue": "TRUE"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "caseSensitive": false,
                "extractLocation": "CLIENT_REQUEST_HEADER",
                "globalSubstitution": false,
                "headerName": "Accept-Language ",
                "regex": "^([a-zA-Z]{2}).*",
                "replacement": "$1",
                "transform": "SUBSTITUTE",
                "valueSource": "EXTRACT",
                "variableName": "PMUSER_LANG"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_X_FORWARDED_HOST",
                "variableValue": "{{builtin.AK_HOST}}"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY",
                "variableValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}};X_FORWARDED_HOST={{user.PMUSER_X_FORWARDED_HOST}}"
            }
        },
        {
            "name": "caching",
            "options": {
                "behavior": "CACHE_CONTROL_AND_EXPIRES",
                "cacheControlDirectives": "",
                "defaultTtl": "1d",
                "enhancedRfcSupport": false,
                "honorMustRevalidate": false,
                "honorPrivate": false,
                "mustRevalidate": false
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-tokowaka-api-key",
                "newHeaderValue": "<your api-key here>",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-config",
                "newHeaderValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-url",
                "newHeaderValue": "{{builtin.AK_URL}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "cacheId",
            "options": {
                "rule": "INCLUDE_VARIABLE",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY"
            }
        },
        {
            "name": "modifyIncomingResponseHeader",
            "options": {
                "action": "DELETE",
                "customHeaderName": "Age",
                "standardDeleteHeaderName": "OTHER"
            }
        },
        {
            "name": "prefreshCache",
            "options": {
                "enabled": true,
                "prefreshval": 90
            }
        },
        {
            "name": "modifyOutgoingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-Forwarded-Host",
                "newHeaderValue": "{{builtin.AK_HOST}}",
                "standardModifyHeaderName": "OTHER"
            }
        }
    ],
    "criteria": [
        {
            "name": "userAgent",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "IS_ONE_OF",
                "matchWildcard": true,
                "values": [
                    "*Tokowaka-AI*",
                    "*ChatGPT-User*",
                    "*GPTBot*",
                    "*OAI-SearchBot*"
                ]
            }
        },
        {
            "name": "path",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "MATCHES_ONE_OF",
                "normalize": false,
                "values": [
                ]
            }
        },
        {
            "name": "requestHeader",
            "options": {
                "headerName": "x-tokowaka-request",
                "matchOperator": "DOES_NOT_EXIST",
                "matchWildcardName": false
            }
        },
        {
            "name": "matchVariable",
            "options": {
                "matchCaseSensitive": true,
                "matchOperator": "IS",
                "matchWildcard": false,
                "variableExpression": "FALSE",
                "variableName": "PMUSER_TOKOWAKA_DISABLE"
            }
        }
    ],
    "criteriaMustSatisfy": "all"
}
```

Important considerations:

* Tokowaka Rule will be ON based on User-Agent + Path + x-tokowaka-request (if not present) + TOKOWAKA_DISABLE variable (to allow switch off using a single variable toggle)
* Set up rules to **Modify Incoming Request Headers** rule to set custom headers
* Set cache-key in Akamai using user defined variable through Cache-ID modification mechanism. Only a single user defined variable is allowed, so create a separate variable for cache_key and set it accordingly.
* Lang: extracted from Accept-Language header using "regex": "^([a-zA-Z]{2}).*"
* With Cache ID Modification within a match on User Agent, the content can't be purged by URL (just FYI)
* Site failover mechanism: With the match on User-Agent rule, Akamai does not allows to failover based on health check, but only only basis of origin response/connectivity per request. Set **x-tokowaka-fo:true**  resp header in case of failover response.
* SWR is not supported by Akamai. So, only TTL based caching is there. So, configure a rule in Akamai to strip Age header from origin response else TTL based caching would not work.
* Ensure that the Tokowaka rule is the bottom most rule in the rule hierarchy (so that it overrides all other rules).-->

>[!TAB Snabbt (BYOCDN)]

**Tokowaka BYOCDN - snabbt - VCL**

![Snabbt VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![Lägg till VCL-fragment](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv-kodfragment**

```
unset req.http.x-tokowaka-url;
unset req.http.x-tokowaka-config;
unset req.http.x-tokowaka-api-key;

if (!req.http.x-tokowaka-request
    && req.http.user-agent ~ "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-fowarded-host = req.http.host; # required for identifying the original host
  set req.http.x-tokowaka-url = req.url; # required for identifying the original url
  set req.http.x-tokowaka-config = "LLMCLIENT=true"; # required for cache key
  set req.http.x-tokowaka-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_Tokowaka;
}
```

**vcl_hash-kodfragment**

```
if (req.http.x-tokowaka-config) {
  set req.hash += "tokowaka";
  set req.hash += req.http.x-tokowaka-config;
}
```

**vcl_deliver snippet**

```
if (req.http.x-tokowaka-config && resp.status >= 400) {
  set req.http.x-tokowaka-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-tokowaka-config && req.http.x-tokowaka-request == "failover") {
  set resp.http.x-tokowaka-fo = "1";
}
```

>[!ENDTABS]

>[!NOTE]
>Kontakta `llmo-at-edge@adobe.com` om du vill hjälpa dina IT-/CDN-team med att komma igång för andra CDN-leverantörer. När konfigurationerna är klara kan du driftsätta förslag för Optimera på Edge i LLM Optimizer.

## Möjligheter

I följande tabell visas möjligheter som kan förbättra den agentiska webbupplevelsen och stöds av Optimize på Edge.

| Möjligheter | Typ | Identifiera automatiskt | Föreslå automatiskt | Automatisk optimering |
|---------|----------|----------|----------|----------|
| Återskapa innehållets synlighet | Teknisk GEO | Identifierar sidor där kritiskt innehåll är dolt för AI-agenter. Visar URL:er som påverkas och förväntat innehåll som kan återställas. | Markerar innehåll som kan göras tillgängligt för AI-agenter och rekommenderar att förrendering aktiveras för dessa sidor. | Skapar en helåtergiven, AI-vänlig HTML-ögonblicksbild av den autentiska trafik som återställer det tidigare dolda innehållet. |
| Optimera rubriker för budskap om livslångt lärande | Optimering av innehåll | Söker igenom rubriker för att upptäcka tomma, duplicerade, saknade eller tvetydiga rubriker som kan minska maskinläsbarheten. | Föreslår en renare rubrikhierarki och förbättrade etiketter och visar en förhandsvisning av den uppdaterade strukturen för varje sida. | Infogar den förbättrade rubrikstrukturen för AI-agenter och bevarar den visuella designen samtidigt som sidan blir mer läsbar för LLM-program. |
| Lägg till LLM-vänliga sammanfattningar | Optimering av innehåll | Identifierar långa eller komplexa sidor som saknar koncisa sammanfattningar på sid- eller avsnittsnivå, vilket gör dem svårare för AI att snabbt skanna och förstå. | Rekommenderar korta, AI-genererade sammanfattningar på sid- och avsnittsnivå som hämtar nyckelinnehåll. | Infogar sammanfattningarna i de relevanta HTML-avsnitten, vilket förbättrar hur modellerna tolkar och beskriver sidinnehållet. |
| Lägg till relevanta frågor | Optimering av innehåll | Identifierar mellanrum i det befintliga sidinnehållet som skulle kunna dra nytta av vanliga frågor. | Föreslår AI-genererat FAQ-innehåll som är anpassat till användaravsikten och befintliga ämnen. | Inför innehåll med vanliga frågor i HTML, vilket gör sidorna mer identifierbara och relevanta i AI-drivna svar. |
| Förenkla komplext innehåll | Optimering av innehåll | Flaggar sidor med komplex text som kan förhindra AI-förståelsen. | Innehåller AI-genererade förenklade versioner av komplex text samtidigt som den ursprungliga innebörden bevaras. | Skriver om komplexa avsnitt på sidan, vilket förbättrar AI-läsbarheten. |

### Ytterligare verktyg

[Adobe LLM Optimizer: Kan din webbsida redigeras?Tillägget &#x200B;](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome visar hur mycket av ditt webbsidesinnehåll som LLM kan komma åt och vad som döljs. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration.

Med ett enda klick kan du utvärdera vilken dator som kan läsas på en webbplats. Du kan visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad människor ser och uppskatta hur mycket innehåll som kan återställas med hjälp av LLM Optimizer. Se [Kan AI läsa din webbplats?](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension) sida för mer information.

## Detaljerade affärsmöjligheter

I följande avsnitt kan du visa ytterligare information för varje affärsmöjlighet som stöds av Optimize på Edge.

### Återskapa innehållets synlighet

Den här affärsmöjligheten flaggar sidor där nyckelinnehåll döljs för AI-agenter på grund av klientåtergivning. För varje identifierad sida visar den exakt vilket innehåll som saknas i AI-agentvyn, markerar luckor i synligheten och gör det möjligt att direkt tillämpa ändringar för att återskapa det dolda innehållet. När du distribuerar denna möjlighet med Optimize på Edge får LLM-användaragenterna en förrenderad, AI-optimerad version av sidan så att de kan komma åt den fullständiga kontexten utan att köra Javascript.
Detta garanterar att sidan först är helt synlig för AI-agenter. Ytterligare förbättringar tillämpas ovanpå den förrenderade HTML.

>[!IMPORTANT]
>Denna förrenderingsfunktion gäller automatiskt alla möjligheter som presenteras nedan när den används med Optimera på Edge för att säkerställa att sidan är helt synlig för AI-agenter.

### Optimera rubriker för budskap om livslångt lärande

Denna möjlighet identifierar sidor där rubrikstrukturen gör det svårt för AI-agenter att förstå sidan på grund av tomma, duplicerade, saknade eller tvetydiga rubriker. För varje berörd sida kommer affärsmöjligheten att visa de underoptimala rubrikerna och rekommenderar en tydligare hierarki. När de används med Optimera i Edge används de förbättrade rubrikerna i HTML för agell trafik. Detta gör att datorn blir läsbar samtidigt som den mänskliga motstående layouten förblir densamma.

### Lägg till LLM-vänliga sammanfattningar

Denna möjlighet identifierar sidor som kan dra nytta av koncisa sammanfattningar för att hjälpa webbdesigners att snabbt förstå vad sidinnehållet handlar om. För varje sida identifierar affärsmöjligheten var en sammanfattning behövs och skapar AI-genererade sammanfattningar på sidnivå eller avsnittsnivå. När du distribuerar med Optimera på Edge infogas dessa sammanfattningar i HTML som AI-agenter hämtar, vilket förbättrar chanserna att ditt innehåll beskrivs mer korrekt.

### Lägg till relevanta frågor

Den här affärsmöjligheten flaggar sidor där ytterligare frågor och svar-innehåll bättre kan matcha användaravsikten och frågar vid AI-driven identifiering. För varje sida föreslås AI-genererade FAQ-block kopplade till användaravsikt och innehåll på sidan. Med Optimize på Edge injiceras dessa frågor och svar i HTML, vilket gör sidan mer AI-vänlig och ökar sannolikheten för att AI-svaren direkt speglar din vägledning.

### Förenkla komplext innehåll

Här hittar du sidor med långa, komplexa stycken som kan minska förståelsen av AI. För varje sida som överskrider läsbarhetströskeln skapas AI-genererat innehåll som är enklare och enklare att läsa in samtidigt som den ursprungliga innebörden bevaras. När innehållet distribueras i framkant hjälper det förenklade innehållet i den autentiska trafiken att tolka och sammanfatta innehållet på ett trognare sätt.

## Optimera automatiskt på Edge

För varje affärsmöjlighet kan du förhandsgranska, redigera, driftsätta, visa direkt och återställa optimeringarna.

>[!VIDEO](https://video.tv.adobe.com/v/3477983/?learn=on&enablevpops)

### Förhandsgranska

Med **Förhandsgranska** kan du se effekten av ett förslag innan det publiceras. Den visar på en skillnad sida vid sida mellan den aktuella sidan och den AI-optimerade versionen som förväntas när förslaget har tillämpats. I den här vyn används samma Optimera-funktion på Edge som i direkttrafiken, men i ett isolerat förhandsvisningsläge. Detta påverkar inte Live-trafik eftersom det är en skrivskyddad simulering för granskning.

![Förhandsgranska](/help/assets/optimize-at-edge/preview.png)

### Redigera

Med **Redigera** kan du förfina eller skriva om det automatiskt genererade förslaget helt innan du distribuerar det. Istället för att godkänna förslaget behåller du full kontroll genom redigeringsarbetsflödet. Vyn visar föreslagna ändringar i en strukturerad redigerare, där du kan ändra texten så att den bättre matchar den ursprungliga metoden. Den redigerade versionen skickas sedan till AI-agenter när de har distribuerats.

![Redigera](/help/assets/optimize-at-edge/edit.png)

### Distribuera

**Distribuera** publicerar de valda förslagen så att de optimerade upplevelserna kan hanteras från kanten till AI-agenter. Om CDN är helt dirigerad visas vanligtvis alla sidor i domänen med de nya ändringarna inom några minuter. Om routning har konfigurerats för enbart valda sökvägar, är det bara de tillåtslista sidorna som får publiceras med optimeringarna.

![Distribuera](/help/assets/optimize-at-edge/deploy.png)

### Visa Live

Med **Visa Live** kan du verifiera att optimeringen är aktiv och fungerar som förväntat för autentisk trafik, en vy som annars skulle vara svår att komma åt. Du kan visa den aktiva sidan under Fasta förslag, som återger sidan så som den visas för AI-agenter.

![Visa Live](/help/assets/optimize-at-edge/view-live.png)

### Återställning

Återställning återställer säkert en tidigare distribuerad optimering. Sidans AI-version återgår vanligtvis till det tidigare läget inom några minuter, vilket gör det enkelt att experimentera med optimeringar när det behövs.

![Återställning](/help/assets/optimize-at-edge/rollback.png)

## Vanliga frågor

F. Vilken typ av LLM riktar du dig till med Optimize på Edge?

Listan med användaragenter som ska användas som mål definieras av dig under introduktionsprocessen.

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

F. Vad händer om jag inte har börjat optimera på Edge än?

Om du klickar på **Distribuera optimeringar** innan du slutför den nödvändiga konfigurationen kommer ingenting att tillämpas på din plats. I stället visas en popup-dialogruta där du uppmanas att kontakta vårt team på `llmo-at-edge@adobe.com` för att få hjälp med introduktionen. Tills introduktionen är klar kan du fortfarande utforska de identifierade möjligheterna och förslagen, men arbetsflödet för distribution med ett klick förblir inaktivt.

F: Vad händer när innehållet uppdateras vid källan?

Vi levererar den optimerade versionen av sidan från cache så länge som den underliggande källsidan inte har ändrats. Men när källan ändras uppdateras vårt system automatiskt så att AI-agenter alltid får det senaste innehållet. Det beror på att vi använder låg cachetid för att göra inställningar (i minutordning) så att alla innehållsuppdateringar på webbplatsen utlöser en ny optimering i det fönstret. <!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

F. Optimerar Edge endast för webbplatser som använder Adobe Edge Delivery Service (EDS)?

Nej. Optimera på Edge är CDN-agnostiker och fungerar med alla frontendarkitekturer, inte bara de som används i Adobe EDS Stack.

F. Hur skiljer sig Optimera vid Edge-förrendering från traditionell SSR-rendering (server-side rendering)?

Båda problemen kan lösas och fungera tillsammans. Traditionell SSR återger innehåll på serversidan, men inkluderar inte innehåll som läses in senare i webbläsaren. Optimera vid Edge-förrendering fångar upp sidan när data från JavaScript och klientsidan har lästs in, vilket skapar den fullständigt monterade versionen vid CDN-kanten. SSR fokuserar på att förbättra människors upplevelse och på Optimera på Edge förbättrar webbupplevelsen för LLM.
