---
title: Optimera på Edge
description: Lär dig leverera optimeringar i LLM Optimizer i CDN-kanten utan att behöva göra några redigeringsändringar.
feature: Opportunities
source-git-commit: 3c6f287b3c3787cee95f99b7031412f26692a88b
workflow-type: tm+mt
source-wordcount: '2291'
ht-degree: 0%

---


# Optimera på Edge

Det här avsnittet ...

## Vad är Optimize på Edge?

Optimize at Edge är en edge-baserad driftsättningsfunktion i LLM Optimizer som kan hantera AI-vänliga ändringar av användaragenter för livslångt lärande. Eftersom det ger optimeringar i CDN-kanten behövs inga redigeringsändringar i Content Management System (CMS). Den riktar sig också endast mot den agentiska trafiken och påverkar inte mänskliga användare eller SEO-robotar.

När LLM Optimizer upptäcker möjligheter att optimera en sida kan man driftsätta korrigeringar direkt utan plattformsändringar.

Den här funktionen är för närvarande i tidig åtkomst.

## Varför ska en kund vara intresserad?

Optimera på Edge är ett snabbare och smidigare alternativ till traditionella korrigeringar som kräver komplexa konstruktionssatsningar. När kunderna har slutfört en engångskonfiguration krävs inga plattformsändringar eller långa utvecklingscykler för att ändringarna ska kunna tillämpas på webbsidorna. Användaren kan publicera förbättringar på några minuter, inte veckor, utan att det krävs något engagemang från utvecklaren. Detta är ett lågrisksätt att optimera webbplatsen för AI-agenter utan kod.

### Viktiga fördelar och värdeförslag

* **Leverans endast för AI:** Fungerar optimerade HTML till AI-agenter utan att påverka mänskliga besökare eller SEO-robotar.
* **Snabbare cykler:** Publicera ändringar på några minuter, inte veckor. Inga plattformsändringar eller långa konstruktionscykler krävs.
* **Lågrisk och reversibel:** Stöds med en enklicksåterställning som kan återställa sidan på några minuter.
* **Ingen prestandapåverkan:** Edge-baserade optimeringar och cachning påverkar inte webbplatsens latens.
* **CDN och CMS-agnostiker:** Fungerar med alla CDN-konfigurationer och frontendkonfigurationer oavsett CMS.

## Vem ska använda den?

Optimera på Edge är utformat för företagsanvändare i team som arbetar med marknadsföring, sökmotoroptimering, innehåll och digital strategi. Det kan göra det möjligt för företagsanvändare att slutföra hela kundresan i LLM Optimizer: identifiera möjligheter, förstå förslag och enkelt implementera korrigeringarna. Med Optimera på Edge kan man förhandsgranska ändringarna, snabbt driftsätta dem och validera att optimeringarna är aktiva. Prestanda kan spåras i LLM Optimizer ekosystem.

## Vilka möjligheter har Optimize på Edge?

Möjligheter som kan förbättra den autentiska webbupplevelsen stöds av Optimize på Edge. Läs mer om varje affärsmöjlighet i avsnittet [Affärsmöjligheter](/help/dashboards/opportunities.md).

## Onboarding

Du kan aktivera Optimera på Edge när du har registrerat dig på LLM Optimizer och har vidarebefordrat dina CDN-loggar.

En CDN-tekniker krävs för att slutföra den första konfigurationen och aktivera Optimera på Edge.

Krav för installationen:

* Generera en API-nyckel.
* Lägg till Optimize på Edge routningsregler i CDN.
* Tillåtslista användardefinierade sökvägar för hela domänen.
* Lägg till en användardefinierad lista med användaragenter för LLM i målgruppen.
* Kontrollera att robots.txt inte blockerar några användaragenter som är avsedda att rikta sig till.
* Bekräfta att Optimera vid Edge-routning finns i LLM Optimizer användargränssnitt.

Adobe tillhandahåller exempelkonfigurationskisser för de flesta vanliga CDN:er som kan vägleda installationsprocessen. De kodfragment som ingår i våra riktlinjer måste anpassas till den faktiska konfigurationen. Adobe rekommenderar att du implementerar ändringar i de lägre miljöerna först.

>[!BEGINTABS]

>[!TAB Hanterad CDN för tjänsten AEM Cloud (snabbt)]

**Tokowaka BYOCDN - Adobe hanterat CDN**

Använder bara originSelectors för att välja Tokowakorigo.

I följande exempel dirigeras LLM-agentbegäranden på en viss domän som matchar mönstret &quot;/es/*&quot; eller exakta sökvägar (endast HTML-sidor dirigeras). Exemplet ska ge en startpunkt och om du har flera originSelectors i konfigurationen bör du placera det här först.

Viktigt:

* x-tokowaka-request måste kontrolleras innan routning till Tokowaka backend. Endast begäranden som inte har den här rubriken ska dirigeras till Tokowaka-serverdelen.
* originSelector-regeln som dirigerar till Tokowaka-serverdelen ska vara först i listan om det finns flera regler.
* TOKOWAKA_API_KEY-hemligheten måste distribueras innan cdn.yaml distribueras

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
          headers:
            x-tokowaka-api-key: "${{TOKOWAKA_API_KEY}}"
    origins:
      - name: tokowaka-backend
        domain: "edge.tokowaka.now"
```

>[!TAB Akamai (BYOCDN)]

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

Viktigt att tänka på:

* Tokowaka-regeln aktiveras baserat på variabeln User-Agent + Path + x-tokowaka-request (om sådan inte finns) + TOKOWAKA_DISABLE (för att tillåta avstängning med en variabelväxel)
* Ställ in regler på regeln **Ändra inkommande begäranrubriker** för att ange anpassade rubriker
* Ange cache-key i Akamai med hjälp av användardefinierad variabel via Cache-ID-ändringsmekanismen. Endast en användardefinierad variabel tillåts, så skapa en separat variabel för cache_key och ställ in den därefter.
* Språk: har extraherats från sidhuvudet Accept-Language med regex: &quot;^([a-zA-Z]{2}).*&quot;
* Med cache-ID-ändring i en matchning på en användaragent kan innehållet inte rensas av URL (bara FYI)
* Platsfel: Med matchningen i regeln för användaragent tillåter inte Akamai redundans baserat på hälsokontroll, utan bara baserar på ursprungligt svar/anslutning per begäran. Ange **x-tokowaka-fo:true**-resp-huvudet om ett redundanssvar skulle inträffa.
* SWR stöds inte av Akamai. Det är alltså bara TTL-baserad cachelagring som finns. Konfigurera en regel i Akamai för att ta bort sidhuvudet från ursprungssvaret, annars fungerar inte TTL-baserad cachelagring.
* Se till att Tokowaka-regeln är den understa regeln i regelhierarkin (så att den åsidosätter alla andra regler).

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


För andra CDN-leverantörer kan du kontakta llmo-at-edge@adobe.com för att hjälpa dina IT-/CDN-team med introduktionen.

<!--This should probably be included Opportunities dashboard content. Content also needs serious editing - lots of "customer needs"and business user" etc.-->

När konfigurationerna är klara kan företagsanvändare distribuera förslag på Optimera för Edge i LLM Optimizer.

## Möjligheter

| Möjligheter | Typ | Identifiera automatiskt | Föreslå automatiskt | Automatisk optimering |
|---------|----------|----------|----------|----------|
| Återskapa innehållets synlighet | Teknisk GEO | Identifierar sidor där kritiskt innehåll är dolt för AI-agenter. Visar URL:er som påverkas och förväntat innehåll som kan återställas. | Markerar innehåll som kan göras tillgängligt för AI-agenter och rekommenderar att förrendering aktiveras för dessa sidor. | Skapar en helåtergiven, AI-vänlig HTML-ögonblicksbild av den autentiska trafik som återställer det tidigare dolda innehållet. |
| Optimera rubriker för AI | Optimering av innehåll | Söker igenom rubriker för att identifiera tomma, duplicerade, saknade eller tvetydiga rubriker som kan minska maskinläsbarheten. | Föreslår en renare rubrikhierarki och förbättrade etiketter och visar en förhandsvisning av den uppdaterade strukturen för varje sida. | Infogar den förbättrade rubrikstrukturen för AI-agenter och bevarar den visuella designen samtidigt som sidan blir mer lättförståelig för LLM-program. |
| Lägg till AI-vänliga sammanfattningar | Optimering av innehåll | Identifierar långa eller komplexa sidor som saknar koncisa sammanfattningar på sid- eller avsnittsnivå, vilket gör dem svårare för AI att snabbt skanna och förstå. | Rekommenderar korta, AI-genererade sammanfattningar på sid- och avsnittsnivå som hämtar nyckelinnehåll. | Infogar sammanfattningarna i de relevanta HTML-avsnitten, vilket förbättrar hur modellerna tolkar och beskriver sidinnehållet. |
| Lägg till relevanta frågor | Optimering av innehåll | Identifierar mellanrum i det befintliga sidinnehållet som skulle kunna dra nytta av vanliga frågor. | Föreslår AI-genererat FAQ-innehåll anpassat till användaravsikt och befintliga ämnen. | Inför innehåll med vanliga frågor i HTML, vilket gör sidorna mer identifierbara och relevanta i AI-drivna svar. |
| Förenkla komplext innehåll | Optimering av innehåll | Flaggar sidor med komplex text som kan förhindra AI-förståelsen. | Innehåller AI-genererade förenklade versioner av komplexa tester samtidigt som den ursprungliga innebörden bevaras. | Skriver om komplexa avsnitt på sidan, vilket förbättrar AI-läsbarheten. |

### Återskapa innehållets synlighet

Den här affärsmöjligheten flaggar sidor där nyckelinnehåll döljs för AI-agenter på grund av klientåtergivning. För varje identifierad sida visar den exakt vilket innehåll som saknas i AI-agentvyn, markerar luckor i synligheten och gör det möjligt att direkt tillämpa ändringar för att återskapa det dolda innehållet. När du distribuerar denna möjlighet med Optimize på Edge får LLM-användaragenterna en förrenderad, AI-optimerad version av sidan så att de kan komma åt den fullständiga kontexten utan att köra Javascript.

**Den här förrenderingsfunktionen gäller automatiskt alla möjligheter som följer när den distribueras med Optimera på Edge.** Detta garanterar att sidan är helt synlig för AI-agenter först. Ytterligare förbättringar tillämpas ovanpå den förrenderade HTML.

#### Ytterligare verktyg

Kan din webbsida redigeras? [Adobe LLM Optimizer: Kan din webbsida redigeras?Med Chrome-tillägget &#x200B;](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) kan du se exakt hur mycket av webbsidans innehåll som LLM kan komma åt och vad som inte kan döljas. Det är utformat som ett kostnadsfritt, fristående diagnosverktyg och kräver ingen produktlicens eller konfiguration.

Med ett enda klick kan du utvärdera en webbplats maskinläsbarhet, visa en jämförelse sida vid sida av vad AI-agenter ser jämfört med vad människor ser och uppskatta hur mycket innehåll som kan återställas med LLM Optimizer. Se [Kan AI läsa din webbplats?](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension) om du vill ha mer information.

### Optimera rubriker för budskap om livslångt lärande

Denna möjlighet identifierar sidor där rubrikstrukturen gör det svårt för AI-agenter att förstå sidan på grund av tomma, duplicerade, saknade eller tvetydiga rubriker. För varje berörd sida kommer affärsmöjligheten att visa de underoptimala rubrikerna och rekommenderar en tydligare hierarki. När de används med Optimize på Edge används de förbättrade rubrikerna i HTML för agell trafik, vilket kan förbättra maskinläsbarheten samtidigt som du låter din mänskliga layout vara densamma.

### Lägg till LLM-vänliga sammanfattningar

Denna möjlighet identifierar sidor som kan dra nytta av koncisa sammanfattningar för att snabbt förstå vad sidan handlar om. För varje sida identifierar affärsmöjligheten var en sammanfattning behövs och skapar AI-genererade sammanfattningar på sidnivå och/eller avsnittsnivå. När du distribuerar med Optimera på Edge infogas dessa sammanfattningar i HTML som AI-agenter hämtar, vilket förbättrar chanserna att ditt innehåll beskrivs mer korrekt.

### Lägg till relevanta frågor

Den här affärsmöjligheten flaggar sidor där ytterligare frågor och svar-innehåll bättre kan matcha användaravsikten och uppmaningar i AI-driven identifiering. För varje sida föreslås AI-genererade FAQ-block kopplade till användaravsikt och innehåll på sidan. Med Optimize på Edge injiceras dessa frågor och svar i HTML, vilket gör sidan mer AI-vänlig och ökar sannolikheten för att AI-svaren direkt speglar din vägledning.

### Förenkla komplext innehåll

Här hittar du sidor med långa, komplexa stycken som kan minska förståelsen av AI. För varje sida som överskrider läsbarhetströskeln skapas AI-genererat innehåll som är enklare och mer skannerbart samtidigt som den ursprungliga innebörden bevaras. När innehållet distribueras i framkant hjälper det förenklade innehållet i den autentiska trafiken att tolka och sammanfatta innehållet på ett trognare sätt.

## Förslag

För varje affärsmöjlighet kan du förhandsgranska, redigera, driftsätta, förhandsgranska och återställa optimeringarna vid en kant.

### Förhandsgranska

Med Förhandsgranska kan användarna se effekten av ett förslag på sidan innan något publiceras. Den visar på en skillnad sida vid sida mellan den aktuella sidan och den AI-optimerade versionen som förväntas när förslaget har tillämpats. I den här vyn används samma Optimera-funktion på Edge som används för direkttrafik, men i ett säkert, isolerat förhandsvisningsläge. Detta påverkar inte Live-trafik eftersom det är en skrivskyddad simulering för granskning.

![Förhandsgranska](/help/assets/optimize-at-edge/preview.png)

### Redigera

Med Redigera kan användare förfina eller skriva om det automatiskt genererade förslaget helt innan det distribueras. Istället för att passivt acceptera förslaget behåller man full kontroll genom det här arbetsflödet. Vyn visar föreslagna ändringar i ett strukturerat redigeringsprogram, där användarna kan ändra texten så att den bättre matchar deras avsikter. Den redigerade versionen skickas sedan till AI-agenter när de har distribuerats.

![Redigera](/help/assets/optimize-at-edge/edit.png)

### Distribuera

Distribuera publicerar de valda förslagen så att de optimerade upplevelserna kan hanteras från kant till AI-agenter. Om CDN är helt dirigerad kommer alla sidor i domänen att publiceras med de nya ändringarna, vanligtvis inom några minuter. Om routning har konfigurerats för enbart valda sökvägar, är det bara de tillåtslista sidorna som får publiceras med optimeringarna.

![Distribuera](/help/assets/optimize-at-edge/deploy.png)

### Visa Live

Med Visa Live kan användarna verifiera att optimeringen är aktiv och fungerar som förväntat för autentisk trafik, vilket annars skulle vara svårt att komma åt. Användare kan visa den aktiva sidan under Fasta förslag, som återger sidan så som den visas för AI-agenter.

![Visa Live](/help/assets/optimize-at-edge/view-live.png)

### Återställning

Återställning återställer säkert en tidigare distribuerad optimering. Sidans AI-version återgår vanligtvis till det tidigare läget inom några minuter, vilket gör det säkert för användarna att experimentera med optimeringar om det behövs.

![Återställning](/help/assets/optimize-at-edge/rollback.png)

## Vanliga frågor

F. Vilken typ av LLM riktar du dig till med Optimize på Edge?

Listan över användaragenter som ska användas är fullständigt definierad av kunden vid introduktionen.

F. Vad betyder&quot;Edge&quot; i Optimera på Edge?

I vårt sammanhang betyder&quot;Edge&quot; att optimeringen tillämpas i CDN-lagret och inte i din CMS.

F. Varför kräver denna optimering ett CDN?

CDN är den plats där den optimerade versionen av sidan monteras och levereras till AI-agenter. Vi utnyttjar CDN för att säkerställa att din ursprungliga CMS förblir oförändrad. Tack vare den här separationen kan du förbättra LLM-synligheten utan att ändra befintliga publiceringsarbetsflöden.

F. Vad händer om jag inte har börjat optimera på Edge än?

Om du klickar på **Distribuera optimeringar** innan du slutför den nödvändiga konfigurationen kommer ingenting att tillämpas på din plats. I stället visas en popup-dialogruta där du uppmanas att kontakta vårt team på llmo-at-edge@adobe.com för att få hjälp med introduktionen. Tills introduktionen är klar kan du fortfarande utforska de identifierade möjligheterna och förslagen, men arbetsflödet för distribution med ett klick förblir inaktivt.

F: Vad händer när innehållet uppdateras vid källan?

Vi levererar den optimerade versionen av sidan från cache så länge som den underliggande källsidan inte har ändrats. Men när källan ändras uppdateras vårt system automatiskt så att AI-agenter alltid får det senaste innehållet. Detta beror på att vi använder låga TTL-värden för cache i minuten så att alla innehållsuppdateringar på din webbplats utlöser en ny optimering i det fönstret. Eftersom det inte finns någon universell TTL som passar alla webbplatser kan vi konfigurera denna TTL baserat på dina cacheminnesogiltighetsregler för att säkerställa att båda systemen hålls synkroniserade.

F. Optimerar Edge endast för webbplatser som använder Adobe Edge Delivery Service (EDS)?

Nej. Optimera på Edge är CDN-agnostiker och fungerar med alla frontendarkitekturer, inte bara sådana som används i Adobe EDS Stack.

F. Hur skiljer sig Optimera vid Edge-förrendering från traditionell SSR-rendering (server-side rendering)?

De två löser olika problem och kan fungera tillsammans. Traditionell SSR återger innehåll på serversidan, men inkluderar inte innehåll som läses in senare i webbläsaren. Optimera vid Edge-förrendering fångar upp sidan när data från JavaScript och klientsidan har lästs in, vilket skapar den fullständiga versionen vid CDN-kanten. SSR fokuserar på att förbättra människors upplevelse och på Optimera på Edge förbättrar webbupplevelsen för LLM.


















