---
title: Azure Event Grid – Handbuch zur Problembehandlung
description: Dieser Artikel bietet eine Liste mit Fehlercodes, Fehlermeldungen, Beschreibungen und empfohlenen Aktionen.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: 3b09b431e827bed4e416913c88d23ee1eddaf17c
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629013"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Problembehandlung von Azure Event Grid-Fehlern
In diesem Handbuch zur Problembehandlung erhalten Sie eine Liste mit Azure Event Grid-Fehlercodes und -Fehlermeldungen sowie deren Beschreibungen und empfohlenen Aktionen, die Sie ausführen sollten, wenn diese Fehler bei Ihnen auftreten. 

## <a name="error-code-400"></a>Fehlercode: 400
| Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.BadRequest<br/>400 | Der Themenname muss zwischen 3 und 50 Zeichen lang sein. | Der benutzerdefinierte Themenname sollte zwischen 3 und 50 Zeichen lang sein. Themennamen dürfen nur alphanumerische Buchstaben, Ziffern und das Zeichen „-“ (Bindestrich) enthalten. Außerdem sollte der Name nicht mit den folgenden reservierten Wörtern beginnen: <ul><li>Microsoft</li><li>EventGrid</li><li>System</li></ul> | Wählen Sie einen anderen Themennamen aus, der die Anforderungen an Themennamen erfüllt. |
| HttpStatusCode.BadRequest<br/>400 | Der Domänenname muss zwischen 3 und 50 Zeichen lang sein. | Die Länge des Domänennamens sollte zwischen 3 und 50 Zeichen betragen. Themennamen dürfen nur alphanumerische Buchstaben, Ziffern und das Zeichen „-“ (Bindestrich) enthalten. Außerdem sollte der Name nicht mit den folgenden reservierten Wörtern beginnen:<ul><li>Microsoft</li><li>EventGrid</li><li>System</li> | Wählen Sie einen anderen Domänennamen aus, der die Anforderungen an Domänennamen erfüllt. |
| HttpStatusCode.BadRequest<br/>400 | Ungültige Ablaufzeit. | Die Ablaufzeit für das Ereignisabonnement bestimmt, wann das Ereignisabonnement abläuft. Dieser Wert sollte in der Zukunft liegender, gültiger DateTime-Wert sein.| Stellen Sie sicher, dass die Ablaufzeit des Ereignisabonnements ein gültiges DateTime-Format hat und in der Zukunft liegt. |

## <a name="error-code-409"></a>Fehlercode: 409
| Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfohlene Maßnahme |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode.Conflict <br/>409 | Ein Thema mit dem angegebenen Namen ist bereits vorhanden. Wählen Sie einen anderen Themennamen aus.   | Der benutzerdefinierte Themenname sollte in einer einzelnen Azure-Region eindeutig sein, um einen korrekten Veröffentlichungsvorgang sicherzustellen. Derselbe Name kann in verschiedenen Azure-Regionen verwendet werden. | Wählen Sie einen anderen Namen für das Thema aus. |
| HttpStatusCode.Conflict <br/> 409 | Eine Domäne mit dem angegebenen Namen ist bereits vorhanden. Wählen Sie einen anderen Domänennamen aus. | Der Domänenname sollte in einer einzelnen Azure-Region eindeutig sein, um einen korrekten Veröffentlichungsvorgang sicherzustellen. Derselbe Name kann in verschiedenen Azure-Regionen verwendet werden. | Wählen Sie einen anderen Namen für die Domäne aus. |
| HttpStatusCode.Conflict<br/>409 | Kontingentlimit erreicht. Weitere Informationen zu diesen Limits finden Sie unter [Azure Event Grid-Grenzwerte](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Für jedes Azure-Abonnement gilt ein Limit hinsichtlich der Anzahl von Azure Event Grid-Ressourcen, die es verwenden kann. Einige oder alle dieses Kontingente wurden überschritten, und es konnten keine Ressourcen mehr erstellt werden. |    Überprüfen Sie Ihre aktuelle Ressourcennutzung, und löschen Sie alle nicht benötigten Ressourcen. Wenn Sie Ihr Kontingent immer noch erhöhen müssen, senden Sie eine E-Mail mit der genauen Anzahl der benötigten Ressourcen an [aeg@microsoft.com](mailto:aeg@microsoft.com). |

## <a name="troubleshoot-event-subscription-validation"></a>Problembehandlung bei der Überprüfung von Ereignisabonnements

Wenn bei der Erstellung eines Ereignisabonnements eine Fehlermeldung wie `The attempt to validate the provided endpoint https://your-endpoint-here failed. For more details, visit https://aka.ms/esvalidation` angezeigt wird, weist dies darauf hin, dass im Überprüfungshandshake ein Fehler aufgetreten ist. Überprüfen Sie Folgendes, um diesen Fehler zu beheben:

- Führen Sie einen „HTTP POST“-Aufruf an Ihre Webhook-URL mit einem [SubscriptionValidationEvent](webhook-event-delivery.md#validation-details)-Beispielanforderungstext unter Verwendung von Postman oder curl oder einem ähnlichen Tool aus.
- Wenn Ihr Webhook einen Handshake-Mechanismus mit synchroner Überprüfung implementiert, überprüfen Sie, ob der Überprüfungscode als Teil der Antwort zurückgegeben wird.
- Wenn Ihr Webhook einen Handshake-Mechanismus mit asynchroner Überprüfung implementiert, überprüfen Sie, ob Ihr „HTTP POST“-Aufruf „200 OK“ zurückgibt.
- Wenn Ihr Webhook „403 (Forbidden)“ in der Antwort zurückgibt, überprüfen Sie, ob sich Ihr Webhook hinter einem Azure Application Gateway oder einer Web Application Firewall befindet. Wenn dies der Fall ist, müssen Sie diese Firewallregeln deaktivieren und erneut einen „HTTP POST“-Aufruf ausführen:

  920300 (Fehlender Accept-Header in Anforderung; das können wir beheben)

  942430 (Eingeschränkte Anomalieerkennung für SQL-Zeichen (Argumente): Anzahl von Sonderzeichen überschritten (12))

  920230 (Mehrere URL-Codierungen erkannt)

  942130 (Angriff mit Einschleusung von SQL-Befehlen: SQL-Tautologie erkannt.)

  931130 (Möglicher RFI-Angriff (Remote File Inclusion) = Domänenexterner Verweis/Link)


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihr Problem im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-eventgrid), oder öffnen Sie ein [Supportticket](https://azure.microsoft.com/support/options/). 
