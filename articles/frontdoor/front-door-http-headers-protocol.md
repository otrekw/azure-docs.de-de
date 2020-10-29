---
title: Protokollunterstützung für HTTP-Header in Azure Front Door | Microsoft-Dokumentation
description: Dieser Artikel beschreibt HTTP-Headerprotokolle, die Front Door unterstützt.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2020
ms.author: duau
ms.openlocfilehash: 6c820754f7566261cd49f0b2ee24ec82dc1c8a9c
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92318359"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door"></a>Protokollunterstützung für HTTP-Header in Azure Front Door
Dieser Artikel beschreibt das Protokoll, das Front Door mit Teilen des Aufrufpfads unterstützt (siehe Abbildung). Die folgenden Abschnitte enthalten weitere Informationen zu HTTP-Headern, die von Front Door unterstützt werden.

:::image type="content" source="./media/front-door-http-headers-protocol/front-door-protocol-summary.png" alt-text="HTTP-Headerprotokoll von Azure Front Door":::

>[!IMPORTANT]
>Front Door zertifiziert keine HTTP-Header, die hier nicht dokumentiert sind.

## <a name="client-to-front-door"></a>Client zu Front Door
Front Door akzeptiert die meisten Header für die eingehende Anforderung, ohne sie zu ändern. Einige reservierte Header werden aus der eingehenden Anforderung entfernt, falls gesendet, einschließlich der Header mit dem X-FD-*-Präfix.

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| X-Azure-InternalError:  | Dieser Header enthält den Fehlercode, auf den Front Door bei der Verarbeitung der Anforderung stößt. Dieser Fehler gibt an, dass das Problem intern für den Front Door-Dienst bzw. die Front Door-Infrastruktur auftritt. Melden Sie das Problem dem Support.  |

## <a name="front-door-to-backend"></a>Front Door zum Back-End

Front Door enthält Header für eine eingehende Anforderung, sofern sie nicht aufgrund von Einschränkungen entfernt werden. Front Door fügt außerdem die folgenden Header hinzu:

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| Via |  *Via: 1.1 Azure* </br> Front Door fügt die HTTP-Version des Clients, gefolgt von *Azure* , als Wert für den Via-Header hinzu. Dieser Header gibt die HTTP-Version des Clients an, und dass Front Door für die Anforderung zwischen dem Client und dem Back-End als Empfänger zwischengeschaltet war.  |
| X-Azure-ClientIP | *X-Azure-ClientIP: 127.0.0.1* </br> Stellt die Client-IP-Adresse dar, die mit der zu verarbeitenden Anforderung verknüpft ist. Beispiel: Eine Anforderung von einem Proxy wird möglicherweise dem X-Forwarded-For-Header hinzugefügt, um die IP-Adresse des ursprünglichen Aufrufers anzugeben. |
| X-Azure-SocketIP |  *X-Azure-SocketIP: 127.0.0.1* </br> Stellt die Socket-IP-Adresse dar, die mit der TCP-Verbindung verknüpft ist, von der die aktuelle Anforderung stammt. Die Client-IP-Adresse einer Anforderung entspricht möglicherweise nicht der Socket-IP-Adresse, da sie durch einen Benutzer willkürlich überschrieben werden kann.|
| X-Azure-Ref | *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dies ist eine eindeutige Referenzzeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert. Sie wird zum Durchsuchen von Zugriffsprotokollen verwendet und ist zur Problembehandlung wichtig.|
| X-Azure-RequestChain |  *X-Azure-RequestChain: hops=1* </br> Diesen Header verwendet Front Door, um Anforderungsschleifen zu erkennen, und Benutzer sollten dafür keine Abhängigkeit erstellen. |
| X-Azure-FDID | *X-Azure-FDID: 55ce4ed1-4b06-4bf1-b40e-4638452104da* <br/> Eine Verweiszeichenfolge, die angibt, dass die Anforderung von einer bestimmten Front Door-Ressource stammt. Der Wert kann im Azure-Portal angezeigt oder über die Verwaltungs-API abgerufen werden. Sie können diesen Header in Kombination mit IP-ACLs verwenden, um den Endpunkt so zu sperren, dass nur Anforderungen von einer bestimmten Front Door-Ressource angenommen werden. [Weitere Informationen](front-door-faq.md#how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door) finden Sie in den häufig gestellten Fragen (FAQ). |
| X-Forwarded-For | *X-Forwarded-For: 127.0.0.1* </br> Das HTTP-Headerfeld „X-Forwarded-For (XFF)“ identifiziert oft die Ursprungs-IP-Adresse eines Clients, der über einen HTTP-Proxy oder Lastverteiler eine Verbindung zu einem Webserver herstellt. Falls ein XFF-Header vorhanden ist, fügt Front Door die Socket-IP-Adresse des Clients daran an, oder fügt den XFF-Header mit der Socket-IP-Adresse des Clients hinzu. |
| X-Forwarded-Host | *X-Forwarded-Host: contoso.azurefd.net* </br> Das X-Forwarded-Host-HTTP-Headerfeld ist eine gängige Methode zum Identifizieren des ursprünglich vom Client im Host-HTTP-Anforderungsheader angeforderten Hosts. Dies liegt daran, dass der Hostname aus Front Door für den Back-End-Server, der die Anforderung verarbeitet, abweichen kann. |
| X-Forwarded-Proto | *X-Forwarded-Proto: http* </br> Das X-Forwarded-Proto-HTTP-Headerfeld wird häufig zum Identifizieren des Ursprungsprotokolls einer HTTP-Anforderung verwendet. Front Door könnte basierend auf der Konfiguration über HTTPS mit dem Back-End kommunizieren. Dies gilt auch, wenn eine HTTP-Anforderung an den Reverseproxy gesendet wird. |
| X-FD-HealthProbe | Das HTTP-Headerfeld X-FD-HealthProbe wird verwendet, um den Integritätstest von Azure Front Door zu identifizieren. Wenn der Header auf 1 festgelegt ist, wird ein Integritätstest angefordert. Sie können ihn verwenden, um den Zugriff von Front Door mit dem Headerfeld „X-Forwarded-Host“ einzuschränken. |
|X-Azure-FDID | *X-Azure-FDID-Header: 437c82cd-360a-4a54-94c3-5ff707647783* </br> Dieses Feld enthält eine frontdoorID, mit der ermittelt werden kann, von welcher Front Door die eingehende Anforderung stammt. Dieses Feld wird vom Front Door-Dienst ausgefüllt. | 
|X-Azure-ExternalError | *X-Azure-ExternalError: 0x830c1011, Unbekannte Zertifizierungsstelle.* </br> Dieser Header zeigt den Fehlercode an, den Front Door-Server beim Herstellen einer Verbindung mit dem Back-End-Server zum Verarbeiten einer Anforderung erhalten. Er hilft bei der Identifizierung von Problemen bei der Verbindung zwischen Front Door und der Back-End-Anwendung. Dieser Header enthält eine ausführliche Fehlermeldung, mit deren Hilfe Sie Konnektivitätsprobleme bei Ihrem Back-End identifizieren können (z. B. bei der DNS-Auflösung, bei ungültigen Zertifikaten usw.). | 

## <a name="backend-to-front-door"></a>Back-End zu Front Door

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| X-Azure-OriginStatusCode |  *X-Azure-OriginStatusCode: 503* </br> Dieser Header enthält den vom Back-End zurückgegebenen HTTP-Statuscode. Mithilfe dieses Headers können Sie ohne Zuhilfenahme der Back-End-Protokolle den HTTP-Statuscode ermitteln, der von der Anwendung zurückgegeben wird, die in Ihrem Back-End ausgeführt wird. Dieser Statuscode unterscheidet sich möglicherweise vom HTTP-Statuscode in der Antwort, die von Front Door an den Client gesendet wird. Mit diesem Header können Sie ermitteln, ob das Back-End fehlerhaft ist oder ob das Problem beim Front Door-Dienst vorliegt. |

## <a name="front-door-to-client"></a>Front Door zum Client

Vom Back-End an Front Door gesendete Header werden auch an den Client weitergeleitet. Die folgenden Header werden von Front Door an Clients gesendet.

| Header  | Beispiel und Beschreibung |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Dabei handelt es sich um eine eindeutige Verweiszeichenfolge, die eine von Front Door verarbeitete Anforderung identifiziert, die für die Problembehandlung wichtig ist, da sie zum Suchen nach Zugriffsprotokollen verwendet wird.|
| X-Cache | *X-Cache: TCP_HIT* </br> Dieser Header beschreibt den Cachestatus der Anforderung. Dadurch können Sie ermitteln, ob der Antwortinhalt aus dem Cache von Front Door bereitgestellt wird. |

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer „Front Door“](quickstart-create-front-door.md)
- [Übersicht über die Routingarchitektur](front-door-routing-architecture.md)
