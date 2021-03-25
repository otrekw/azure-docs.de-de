---
title: Konfigurieren von Regelsatzaktionen in Azure Front Door Standard/Premium
description: Dieser Artikel enthält eine Liste der verschiedenen Aktionen, die Sie mit einem Azure Front Door-Regelsatz durchführen können.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: c9995df0f292c5e528156a3280df5484db017fca
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098025"
---
# <a name="azure-front-door-standardpremium-rule-set-actions"></a>Regelsatzaktionen in Azure Front Door Standard/Premium

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Ein Azure Front Door-[Regelsatz](concept-rule-set.md) besteht aus Regeln mit einer Kombination aus Vergleichsbedingungen und Aktionen. Dieser Artikel enthält detaillierte Beschreibungen der Aktionen, die Sie in einem Regelsatz verwenden können. Die Aktion definiert das Verhalten, das auf einen Anforderungstyp angewendet wird, der von einer oder mehreren Vergleichsbedingungen identifiziert wird. Eine Regel in einem Azure Front Door-Regelsatz kann bis zu fünf Aktionen enthalten. Die Servervariable wird für alle Aktionen unterstützt.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Die folgenden Aktionen können auch im Azure Front Door-Regelsatz verwendet werden.  

## <a name="cache-expiration"></a>Cacheablauf

Verwenden Sie diese Aktion, um die Gültigkeitsdauer (TTL) des Endpunkts für Anforderungen zu überschreiben, die die Bedingungen für die Regelübereinstimmung angeben.

### <a name="required-fields"></a>Pflichtfelder

Die folgenden Beschreibungen gelten für die Auswahl des unten aufgeführten Zwischenspeicherverhaltens bei übereinstimmender Regel:

Cacheverhalten |  BESCHREIBUNG              
---------------|----------------
Cache umgehen | Der Inhalt wird nicht zwischengespeichert.
Überschreiben | Der vom Ursprung zurückgegebene TTL-Wert wird mit dem in der Aktion angegebenen Wert überschrieben. Dieses Verhalten wird nur angewendet, wenn die Antwort zwischengespeichert werden kann. Für den Antwortheader „cache-control“ mit den Werten „no-cache“, „private“ und „no-store“ ist die Aktion nicht anwendbar.
Bei Fehlen festlegen | Wenn vom Ursprung kein TTL-Wert zurückgegeben wird, legt die Regel den TTL-Wert auf den in der Aktion angegebenen Wert fest. Dieses Verhalten wird nur angewendet, wenn die Antwort zwischengespeichert werden kann. Für den Antwortheader „cache-control“ mit den Werten „no-cache“, „private“ und „no-store“ ist die Aktion nicht anwendbar.

### <a name="additional-fields"></a>Zusätzliche Felder

Tage | Stunden | Minuten | Sekunden
-----|-------|---------|--------
Int | Int | Int | Int 

## <a name="cache-key-query-string"></a>Abfragezeichenfolge für Cacheschlüssel

Verwenden Sie diese Aktion, um den Cacheschlüssel basierend auf Abfragezeichenfolgen zu ändern.

### <a name="required-fields"></a>Pflichtfelder

Die folgenden Beschreibungen gelten für die Auswahl des unten aufgeführten Verhaltens bei übereinstimmender Regel:

Verhalten | BESCHREIBUNG
---------|------------
Einschließen | Die in den Parametern angegebenen Abfragezeichenfolgen werden eingeschlossen, wenn der Cacheschlüssel generiert wird. 
Zwischenspeichern jeder eindeutigen URL | Jede eindeutige URL verfügt über einen eigenen Cacheschlüssel. 
Exclude | Die in den Parametern angegebenen Abfragezeichenfolgen werden ausgeschlossen, wenn der Cacheschlüssel generiert wird.
Ignorieren von Abfragezeichenfolgen | Beim Generieren des Cacheschlüssels werden Abfragezeichenfolgen nicht berücksichtigt. 

## <a name="modify-request-header"></a>Anforderungsheader ändern

Verwenden Sie diese Aktion, um Header zu ändern, die in den an ihren Ursprung gesendeten Anforderungen vorhanden sind.

### <a name="required-fields"></a>Pflichtfelder

Die folgenden Beschreibungen gelten für die Auswahl der unten aufgeführten Aktionen bei übereinstimmender Regel:

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Der in **Headername** angegebene Header wird mit dem angegebenen Wert zur Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, wird der Wert an den vorhandenen Wert angefügt. | String
Overwrite | Der in **Headername** angegebene Header wird mit dem angegebenen Wert zur Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, wird der vorhandene Wert von dem angegebenen Wert überschrieben. | String
Löschen | Wenn der in der Regel angegebene Header vorhanden ist, wird der Header aus der Anforderung gelöscht. | String

## <a name="modify-response-header"></a>Antwortheader ändern

Verwenden Sie diese Aktion, um Header zu ändern, die in den an Ihre Clients zurückgegebenen Antworten vorhanden sind.

### <a name="required-fields"></a>Pflichtfelder

Die folgenden Beschreibungen gelten für die Auswahl der unten aufgeführten Aktionen bei übereinstimmender Regel:

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Der in **Headername** angegebene Header wird mit dem angegebenen **Wert** zur Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, wird der **Wert** an den vorhandenen Wert angefügt. | String
Overwrite | Der in **Headername** angegebene Header wird mit dem angegebenen **Wert** zur Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, überschreibt der **Wert** den vorhandenen Wert. | String
Löschen | Wenn der in der Regel angegebene Header vorhanden ist, wird der Header aus der Antwort gelöscht. | String

## <a name="url-redirect"></a>Umleitungs-URL

Leiten Sie mit dieser Aktion Clients an eine neue URL um. 

### <a name="required-fields"></a>Pflichtfelder

Feld | BESCHREIBUNG 
------|------------
Umleitungstyp | Wählen Sie den Antworttyp aus, der an den Anforderer zurückgegeben werden soll: „Gefunden“ (302), „Verschoben“ (301), „Temporäre Umleitung“ (307) und „Permanente Umleitung“ (308).
Umleitungsprotokoll | Übereinstimmungsanforderung, HTTP, HTTPS.
Zielhost | Wählen Sie den Hostnamen aus, an den die Anforderung umgeleitet werden soll. Lassen Sie das Feld leer, um den eingehenden Host beizubehalten.
Zielpfad | Definieren Sie den bei der Umleitung zu verwendenden Pfad. Lassen Sie das Feld leer, um den eingehenden Pfad beizubehalten.  
Abfragezeichenfolge | Definieren Sie die bei der Umleitung zu verwendende Abfragezeichenfolge. Lassen Sie das Feld leer, um die eingehende Abfragezeichenfolge beizubehalten. 
Zielfragment | Definieren Sie das bei der Umleitung zu verwendende Fragment. Lassen Sie das Feld leer, um das eingehende Fragment beizubehalten. 

## <a name="url-rewrite"></a>URL Rewrite

Verwenden Sie diese Aktion, um den Pfad einer Anforderung auf dem Weg zu Ihrem Ursprung erneut zu generieren.

### <a name="required-fields"></a>Pflichtfelder

Feld | BESCHREIBUNG 
------|------------
Quellmuster | Definieren Sie das Quellmuster im URL-Pfad, das ersetzt werden soll. Derzeit verwendet das Quellmuster eine auf Präfixen basierende Übereinstimmung. Verwenden Sie einen Schrägstrich ( **/** ) als Quellmusterwert, um alle URL-Pfade abzugleichen.
Destination | Definieren Sie den Zielpfad, der für das erneute Generieren verwendet werden soll. Der Zielpfad überschreibt das Quellmuster.
Nicht übereinstimmenden Pfad beibehalten | Wenn **Ja** ausgewählt ist, wird der verbleibende Pfad nach dem Quellmuster an den neuen Zielpfad angefügt. 

## <a name="server-variable"></a>Servervariable

### <a name="supported-variables"></a>Unterstützte Variablen

| Variablenname | BESCHREIBUNG                                                  |
| -------------------------- | :----------------------------------------------------------- |
| socket_ip                  | Die IP-Adresse der direkten Verbindung mit dem Azure Front Door-Edge. Wenn der Client einen HTTP-Proxy oder einen Lastenausgleich zum Senden der Anforderung verwendet hat, ist der Wert von „SocketIp“ die IP-Adresse des Proxys oder Lastenausgleichs. |
| client_ip                  | Die IP-Adresse des Clients, der die ursprüngliche Anforderung gestellt hat. Wenn in der Anforderung ein X-Forwarded-For-Header vorhanden ist, wird die Client-IP daraus ausgewählt. |
| client_port                | Der IP-Port des Clients, der die Anforderung gestellt hat. |
| hostname                      | Der Hostname in der Anforderung vom Client. |
| geo_country                     | Gibt das Herkunftsland/die Herkunftsregion der anfordernden Person über den jeweiligen Länder-/Regionscode an. |
| http_method                | Die Methode, die für die URL-Anforderung verwendet wird. Beispielsweise GET oder POST. |
| http_version               | Das Anforderungsprotokoll. In der Regel HTTP/1.0, HTTP/1.1 oder HTTP/2.0. |
| query_string               | Die Liste der Variablen/Wert-Paare nach dem „?“ in der angeforderten URL. Beispiel: In der Anforderung *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* lautet der query_string-Wert *id=123&title=fabrikam*. |
| request_scheme             | Das Anforderungsschema: „http“ oder „https“. |
| request_uri                | Der vollständige ursprüngliche Anforderungs-URI (mit Argumenten). Beispiel: In der Anforderung *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* lautet der request_uri-Wert */article.aspx?id=123&title=fabrikam*. |
| server_port                | Der Port des Servers, der eine Anforderung akzeptiert hat. |
| ssl_protocol    | Das Protokoll einer hergestellten TLS-Verbindung. |
| url_path                   | Identifiziert die bestimmte Ressource auf dem Host, auf die der Webclient zugreifen möchte. Dies ist der Teil des Anforderungs-URI ohne die Argumente. Beispiel: In der Anforderung *http://contoso.com:8080/article.aspx?id=123&title=fabrikam* lautet der uri_path-Wert */article.aspx*. |

### <a name="server-variable-format"></a>Format der Servervariablen    

**Format:** {variable:offset}, {variable:offset:length}, {variable}

### <a name="supported-server-variable-actions"></a>Unterstützte Aktionen für Servervariablen

* Anforderungsheader
* Antwortheader
* Abfragezeichenfolge für Cacheschlüssel
* URL Rewrite
* URL Redirect

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Regelsätze in Azure Front Door Standard/Premium](concept-rule-set.md).
* Informieren Sie sich über [Vergleichsbedingungen für Regelsätze](concept-rule-set-match-conditions.md).
