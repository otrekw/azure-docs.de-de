---
title: URL-Umleitung in Azure Front Door | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Azure Front Door URL-Umleitung für Routingregeln unterstützt.
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: duau
ms.openlocfilehash: 61077c7900530fd4c5be64054bedd9c5d087fe77
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91442056"
---
# <a name="url-redirect"></a>URL Redirect
Azure Front Door kann Datenverkehr auf jeder der folgenden Ebenen umleiten: Protokoll, Hostname, Pfad, Abfragezeichenfolge. Diese Funktionen können für einzelne Microservices konfiguriert werden, da die Umleitung pfadbasiert ist. Dies kann die Anwendungskonfiguration vereinfachen, indem die Ressourcennutzung optimiert wird und neue Umleitungsszenarien wie etwa globale und pfadbasierte Umleitung unterstützt werden.
</br>

:::image type="content" source="./media/front-door-url-redirect/front-door-url-redirect.png" alt-text="URL-Umleitung in Azure Front Door":::

## <a name="redirection-types"></a>Umleitungstypen
Ein Umleitungstyp legt den Antwortstatuscode für die Clients fest und informiert so über den Zweck der Umleitung. Die folgenden Umleitungstypen werden unterstützt:

- **301 Moved permanently (301 Permanent verschoben)** : Gibt an, dass der Zielressource ein neuer dauerhafter URI zugewiesen wurde. Zukünftige Verweise auf diese Ressource verwenden einen der eingeschlossenen URIs. Verwenden Sie den Statuscode 301 für die HTTP-HTTPS-Umleitung. 
- **302 Found (302 Gefunden)** : Gibt an, dass sich die Zielressource vorübergehend unter einem anderen URI befindet. Da sich die Umleitung gelegentlich ändern kann, sollte der Client weiterhin den effektiven Anforderungs-URI für künftige Anforderungen verwenden.
- **307 Temporary redirect (307 Temporäre Umleitung)** : Gibt an, dass sich die Zielressource vorübergehend unter einem anderen URI befindet. Der Benutzer-Agent darf die Anforderungsmethode NICHT ändern, wenn er eine automatische Umleitung zu diesem URI durchführt. Da die Umleitung im Laufe der Zeit geändert werden kann, sollte der Client weiterhin den ursprünglichen, effektiven Anforderungs-URI für künftige Anforderungen verwenden.
- **308 Permanent redirect (308 Dauerhafte Umleitung)** : Gibt an, dass der Zielressource ein neuer dauerhafter URI zugewiesen wurde. Zukünftige Verweise auf diese Ressource sollten einen der eingeschlossenen URIs verwenden.

## <a name="redirection-protocol"></a>Umleitungsprotokoll
Sie können das Protokoll festlegen, das für die Umleitung verwendet werden soll. Der häufigste Anwendungsfall der Umleitungsfunktion besteht darin, HTTP auf HTTPS-Umleitung festzulegen.

- **Nur HTTPS**: Legen Sie das Protokoll auf „Nur HTTPS“ fest, wenn der Datenverkehr von HTTP zu HTTPS umgeleitet werden soll. Es wird empfohlen, die Umleitung in Azure Front Door immer auf „Nur HTTPS“ zu setzen.
- **Nur HTTP**: Leitet die eingehende Anforderung zu HTTP um. Verwenden Sie diese Einstellung nur, wenn der Datenverkehr im HTTP-Format, d.h. nicht verschlüsselt, bleiben soll.
- **Übereinstimmungsanforderung** : Über diese Option wird das von der eingehenden Anforderung verwendete Protokoll beibehalten. Das bedeutet, dass eine HTTP-Anforderung nach der Umleitung im HTTP-Format und eine HTTPS-Anforderung im HTTPS-Format bleibt.

## <a name="destination-host"></a>Zielhost
Bei der Konfiguration einer Umleitung können Sie auch den Hostnamen und die Domäne für die Umleitungsanforderung ändern. Über dieses Feld können Sie festlegen, dass der Hostname in der URL für die Umleitung geändert wird, oder aber den Hostnamen aus der eingehenden Anforderung beibehalten. Mit anderen Worten: Über dieses Feld lassen sich alle an `https://www.contoso.com/*` gesendeten Anforderungen zu `https://www.fabrikam.com/*` umleiten.

## <a name="destination-path"></a>Zielpfad
Für den Fall, dass Sie das Pfadsegment einer URL im Zuge der Umleitung ersetzen möchten, können Sie in diesem Feld den neuen Pfadwert festlegen. Alternativ können Sie den Pfadwert als Teil der Umleitung auch beibehalten. Mit anderen Worten: Über dieses Feld lassen sich alle an `https://www.contoso.com/\*` gesendeten Anforderungen zu `https://www.contoso.com/redirected-site` umleiten.

## <a name="query-string-parameters"></a>Abfragezeichenfolge-Parameter
Sie haben die Möglichkeit, die Abfragezeichenfolge-Parameter in der umgeleiteten URL zu ersetzen. Um die vorhandene Abfragezeichenfolge in der eingehenden Anforderungs-URL zu ersetzen, legen Sie dieses Feld auf „Ersetzen“ und dann den entsprechenden Wert fest. Alternativ können Sie die ursprünglichen Abfragezeichenfolgen auch übernehmen, indem Sie das Feld auf „Beibehalten“ festlegen. Beispiel: Über dieses Feld lässt sich der gesamte an `https://www.contoso.com/foo/bar` gesendete Datenverkehr zu `https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F` umleiten. 

## <a name="destination-fragment"></a>Zielfragment
Als Zielfragment wird der Teil der URL nach „#“ bezeichnet, der von Browsern verwendet wird, um einen bestimmten Abschnitt einer Webseite anzuzeigen. Über dieses Feld können Sie festlegen, dass ein Fragment zur Umleitungs-URL hinzugefügt wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
