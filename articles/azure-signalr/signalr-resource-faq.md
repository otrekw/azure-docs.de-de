---
title: Häufig gestellte Fragen zum Azure SignalR Service
description: Erhalten Sie Antworten auf häufig gestellte Fragen zu Azure SignalR Service, einschließlich Informationen zur Problembehandlung und typischen Verwendungsszenarien.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.custom: devx-track-dotnet
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: d5dd765dd9b174ffbfec35b63ad5e55ce84193ad
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489560"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR Service FAQ

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Ist Azure SignalR Service bereit für den Produktionseinsatz?

Ja.
Die Ankündigung der allgemeinen Verfügbarkeit finden Sie unter [Azure SignalR Service jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) wird vollständig unterstützt.

Unterstützung für ASP.NET SignalR befindet sich noch in der *öffentlichen Vorschau*. [Hier sehen Sie ein Codebeispiel](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>Die Clientverbindung wird mit der Fehlermeldung „Kein Server verfügbar“ geschlossen. Was bedeutet das?

Dieser Fehler tritt nur auf, wenn Clients Meldungen an den Azure SignalR Service senden.

Wenn Sie keinen Anwendungsserver besitzen und nur die Azure SignalR Service-REST-API verwenden, ist dieses Verhalten *beabsichtigt*.
In einer serverlosen Architektur befinden sich Clientverbindungen im *Lauschmodus* und senden keine Nachrichten an die Azure SignalR Service-Instanz.
Weitere Informationen zur [REST-API](./signalr-quickstart-rest-api.md).

Wenn Sie über Anwendungsserver verfügen, bedeutet diese Fehlermeldung, dass kein Anwendungsserver mit Ihrer Azure SignalR Service-Instanz verbunden ist.

Mögliche Ursachen sind:
- Kein Anwendungsserver ist mit Azure SignalR Service verbunden. Überprüfen Sie die Anwendungsserverprotokolle auf mögliche Verbindungsfehler. Dieser Fall kommt selten in Hochverfügbarkeitsszenarien mit mehr als einem Anwendungsserver vor.
- Es liegen Konnektivitätsprobleme mit Azure SignalR Service-Instanzen vor. Dieses Problem ist vorübergehend und wird automatisch für die Instanzen behoben.
Wenn es länger als eine Stunde auftritt, [öffnen Sie ein Problem auf GitHub](https://github.com/Azure/azure-signalr/issues/new), oder [erstellen Sie eine Supportanfrage in Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Wenn mehrere Anwendungsserver vorhanden sind, werden Clientmeldungen an alle Server oder nur an einen von ihnen gesendet?

Zwischen einem Client und einem Anwendungsserver besteht eine 1:1-Zuordnung. Meldungen von einem Client werden immer an denselben Anwendungsserver gesendet.

Die Zuordnung wird aufrecht erhalten, bis die Verbindung vom Client oder Anwendungsserver getrennt wird.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Wenn einer meiner Anwendungsserver nicht betriebsbereit ist, wie kann ich ihn auffinden und darüber informiert werden?

Azure SignalR Service überwacht den Heartbeat der Anwendungsserver.
Wenn über einen angegebenen Zeitraum hinweg keine Heartbeats empfangen werden, gilt der Anwendungsserver als offline. Alle Clientverbindungen, die diesem Anwendungsserver zugeordnet sind, werden getrennt.

## <a name="why-does-my-custom-iuseridprovider-throw-an-exception-when-im-switching-from-aspnet-core-signalr-sdk-to-azure-signalr-service-sdk"></a>Warum löst mein benutzerdefinierter `IUserIdProvider` eine Ausnahme aus, wenn ich vom ASP.NET Core SignalR SDK zum Azure SignalR Service SDK wechsle?

Der Parameter `HubConnectionContext context` unterscheidet sich zwischen dem ASP.NET Core SignalR SDK und dem Azure SignalR Service SDK, wenn `IUserIdProvider` aufgerufen wird.

In ASP.NET Core SignalR ist `HubConnectionContext context` der Kontext aus der physischen Clientverbindung mit gültigen Werten für alle Eigenschaften.

Im Azure SignalR Service SDK ist `HubConnectionContext context` der Kontext aus der logischen Clientverbindung. Die physische Client ist mit der Azure SignalR Service-Instanz verbunden, weshalb also nur eine begrenzte Anzahl von Eigenschaften bereitgestellt wird.

Vorerst sind nur `HubConnectionContext.GetHttpContext()` und `HubConnectionContext.User` für den Zugriff verfügbar.
Sie können den [Quellcode überprüfen](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs).

## <a name="can-i-configure-the-transports-available-in-azure-signalr-service-on-the-server-side-with-aspnet-core-signalr-for-example-can-i-disable-websocket-transport"></a>Kann ich die in Azure SignalR Service verfügbaren Transporte auf der Serverseite mit ASP.NET Core SignalR konfigurieren? Kann ich beispielsweise den WebSocket-Transport deaktivieren?

Nein.

Azure SignalR Service bietet alle drei Transporte, die von ASP.NET Core SignalR standardmäßig unterstützt werden. Dies ist nicht konfigurierbar. Azure SignalR Service verarbeitet Verbindungen und Transporte für alle Clientverbindungen.

Sie können clientseitige Transporte konfigurieren, wie in [ASP.NET Core SignalR-Konfiguration](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2) dokumentiert.

## <a name="what-is-the-meaning-of-metrics-like-message-count-or-connection-count-shown-in-the-azure-portal-which-kind-of-aggregation-type-should-i-choose"></a>Welche Bedeutung haben im Azure-Portal angezeigte Metriken wie die Nachrichtenanzahl oder die Anzahl der Verbindungen? Welchen Aggregationstyp sollte ich auswählen?

Ausführliche Informationen zur Berechnung dieser Metriken finden Sie unter [Nachrichten und Verbindungen in Azure SignalR Service](signalr-concept-messages-and-connections.md).

Im Bereich „Übersicht“ von Azure SignalR Service-Ressourcen ist bereits der entsprechende Aggregationstyp für Sie ausgewählt. Wenn Sie in den Bereich „Metriken“ wechseln, können Sie den Aggregationstyp [Nachrichten und Verbindungen in Azure SignalR Service](../azure-monitor/platform/metrics-supported.md#microsoftsignalrservicesignalr) als Referenz verwenden.

## <a name="what-is-the-meaning-of-the-default-serverless-and-classic-service-modes-how-can-i-choose"></a>Welche Bedeutung haben die Dienstmodi `Default`, `Serverless` und `Classic`? Wie kann ich eine Auswahl treffen?

Die folgenden Informationen treffen auf die Modi zu:
* Der `Default`-Modus *erfordert* einen Hubserver. In diesem Modus leitet Azure SignalR Service den Clientdatenverkehr an die Verbindungen der verbundenen Hubserver weiter. Azure SignalR Service überprüft, ob ein verbundener Hubserver vorhanden ist. Wenn der Dienst keinen verbundenen Hubserver finden kann, lehnt er die eingehenden Clientverbindungen ab. Sie können in diesem Modus auch die *Verwaltungs-API* verwenden, um die verbundenen Clients direkt über Azure SignalR Service zu verwalten.
* Der `Serverless`-Modus lässt *keine* Serververbindung zu. Das heißt, alle Serververbindungen werden abgelehnt. Alle Clients müssen sich im serverlosen Modus befinden. Clients stellen eine Verbindung mit Azure SignalR Service her, und Benutzer verwenden normalerweise serverlose Technologien wie *Azure Functions* für die Verarbeitung der Hublogik. Sehen Sie sich ein [einfaches Beispiel](https://docs.microsoft.com/azure/azure-signalr/signalr-quickstart-azure-functions-javascript?WT.mc_id=signalrquickstart-github-antchu) an, in dem der serverlose Modus in Azure SignalR Service verwendet wird.
* Der `Classic`-Modus ist ein gemischter Status. Wenn ein Hub über eine Serververbindung verfügt, wird der neue Client an einen Hubserver weitergeleitet. Wenn dies nicht der Fall ist, wechselt der Client in den serverlosen Modus. 

  Dies kann zu einem Problem führen. Wenn beispielsweise alle Serververbindungen für einen Moment verloren gehen, wechseln einige Clients in den serverlosen Modus wechseln, anstatt eine Weiterleitung an den Hubserver vorzunehmen.

Im Folgenden finden Sie einige Richtlinien für die Auswahl eines Modus:
- Wenn kein Hubserver vorhanden ist, wählen Sie `Serverless` aus.
- Wenn alle Hubs über Hubserver verfügen, wählen Sie `Default` aus.
- Wenn einige Hubs über Hubserver verfügen, andere aber nicht, können Sie `Classic` auswählen, dies kann aber zu einem Problem führen. Die bessere Möglichkeit besteht darin, zwei Instanzen zu erstellen: eine ist `Serverless` und die andere `Default`.

## <a name="are-there-any-feature-differences-in-using-azure-signalr-service-with-aspnet-signalr"></a>Gibt es bei der Verwendung von Azure SignalR Service für ASP.NET SignalR Funktionsunterschiede?
Bei Verwendung von Azure SignalR Service werden einige APIs und Features von ASP.NET SignalR nicht unterstützt:
- Die Möglichkeit, beliebige Zustände zwischen Clients und dem Hub zu übergeben (häufig als `HubState` bezeichnet), wird nicht unterstützt.
- Die `PersistentConnection`-Klasse wird nicht unterstützt.
- *Forever Frame-Transport* wird nicht unterstützt.
- Azure SignalR Service gibt keine an den Client gesendeten Nachrichten mehr wieder, wenn der Client offline ist.
- Wenn Sie Azure SignalR Service verwenden, wird der Datenverkehr für eine Clientverbindung für die Dauer der Verbindung immer an eine App-Serverinstanz weitergeleitet (auch als *sticky* bezeichnet).

Bei der Unterstützung für ASP.NET SignalR liegt der Fokus auf der Kompatibilität. Daher werden nicht alle neuen Features von ASP.NET Core SignalR unterstützt. Beispielsweise sind *MessagePack* und *Streaming* nur für ASP.NET Core SignalR-Anwendungen verfügbar.

Sie können Azure SignalR Service für verschiedene Dienstmodi konfigurieren: `Classic`, `Default` und `Serverless`. Der `Serverless`-Modus wird für ASP.NET nicht unterstützt. Die REST-API auf Datenebene wird ebenfalls nicht unterstützt.

## <a name="where-does-my-data-reside"></a>Wo befinden sich meine Daten?

Azure SignalR Service ist ein Datenverarbeitungsdienst. Inhalte von Kunden werden nicht gespeichert, und Data Residency wird durch den Entwurf garantiert. Wenn Sie Azure SignalR Service in Verbindung mit anderen Azure-Diensten (z. B. Azure Storage) zu Diagnosezwecken verwenden, finden Sie in [diesem Whitepaper](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) Informationen zum Aufrechterhalten von Data Residency in Azure-Regionen.
