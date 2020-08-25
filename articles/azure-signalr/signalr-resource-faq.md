---
title: Häufig gestellte Fragen zum Azure SignalR Service
description: Erhalten Sie schnellen Zugriff auf häufig gestellte Fragen zu Azure SignalR Service, z. B. zum Troubleshooting und zu typischen Verwendungsszenarien.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: c944ae3a5d647cc457edd20a5d3dd0489e19e286
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192286"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR Service FAQ

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Ist Azure SignalR Service bereit für den Produktionseinsatz?

Ja.
Unsere Ankündigung der allgemeinen Verfügbarkeit, finden Sie unter [Azure SignalR Service jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) wird vollständig unterstützt.

Unterstützung für ASP.NET SignalR befindet sich noch in der *öffentlichen Vorschauphase*. Hier finden Sie ein [Codebeispiel](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>Die Clientverbindung wird mit der Fehlermeldung „Kein Server verfügbar“ geschlossen. Was bedeutet das?

Dieser Fehler tritt nur auf, wenn Clients Meldungen an den SignalR Service senden.

Wenn Sie keinen Anwendungsserver besitzen und nur die SignalR Service REST-API verwenden, ist dieses Verhalten **beabsichtigt**.
In einer serverlosen Architektur befinden sich Clientverbindungen im **LAUSCHEN**-Modus und senden keine Meldungen an SignalR Service.
Weitere Informationen zur [REST-API](./signalr-quickstart-rest-api.md).

Wenn Sie über Anwendungsserver verfügen, bedeutet diese Fehlermeldung, dass kein Anwendungsserver mit Ihrer SignalR Service-Instanz verbunden ist.

Mögliche Ursachen sind:
- Kein Anwendungsserver ist mit SignalR Service verbunden. Überprüfen Sie die Anwendungsserverprotokolle auf mögliche Verbindungsfehler. Dieser Fall kommt selten in Hochverfügbarkeitsszenarien mit mehr als einem Anwendungsserver vor.
- Es liegen Konnektivitätsprobleme mit SignalR Service-Instanzen vor. Dieses Problem ist vorübergehend und wird automatisch behoben.
Wenn es länger als eine Stunde auftritt, [öffnen Sie ein Problem auf GitHub](https://github.com/Azure/azure-signalr/issues/new), oder [erstellen Sie eine Supportanfrage in Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Wenn mehrere Anwendungsserver vorhanden sind, werden Clientmeldungen an alle Server oder nur an einen von ihnen gesendet?

Zwischen Client und Anwendungsserver besteht eine 1: 1-Zuordnung. Meldungen von einem Client werden immer an denselben Anwendungsserver gesendet.

Die Zuordnung zwischen Client und Anwendungsserver bleibt bestehen, bis der Client oder der Anwendungsserver die Verbindung trennt.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Wenn einer meiner Anwendungsserver nicht betriebsbereit ist, wie kann ich ihn auffinden und darüber informiert werden?

SignalR Service überwacht den Heartbeat der Anwendungsserver.
Wenn über einen angegebenen Zeitraum hinweg keine Heartbeats empfangen werden, gilt der Anwendungsserver als offline. Alle Clientverbindungen, die diesem Anwendungsserver zugeordnet sind, werden getrennt.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>Warum löst mein benutzerdefinierter `IUserIdProvider` eine Ausnahme aus, wenn vom ASP.NET Core SignalR SDK zum Azure SignalR Service SDK gewechselt wird?

Der Parameter `HubConnectionContext context` unterscheidet sich zwischen ASP.NET Core SignalR SDK und Azure SignalR Service-SDK, wenn `IUserIdProvider` aufgerufen wird.

In ASP.NET Core SignalR ist `HubConnectionContext context` der Kontext aus der physischen Clientverbindung mit gültigen Werten für alle Eigenschaften.

In der Azure SignalR Service SDK ist `HubConnectionContext context` der Kontext aus der logischen Clientverbindung. Die physische Clientverbindung besteht mit der SignalR Service-Instanz, weshalb also nur eine begrenzte Anzahl von Eigenschaften bereitgestellt wird.

Vorerst sind nur `HubConnectionContext.GetHttpContext()` und `HubConnectionContext.User` für den Zugriff verfügbar.
Den Quellcode können Sie sich [hier](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs) ansehen.

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>Kann ich die in SignalR Service verfügbaren Transporte so konfigurieren, wie sie sich serverseitig bei ASP.NET Core SignalR konfigurieren lassen? Kann ich beispielsweise den WebSocket-Transport deaktivieren?

Nein.

Azure SignalR Service bietet alle drei Transporte, die von ASP.NET Core SignalR standardmäßig unterstützt werden. Dies ist nicht konfigurierbar. SignalR Service verarbeitet Verbindungen und Transporte für alle Clientverbindungen.

Sie können die clientseitigen Transporte wie [hier](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2) dokumentiert konfigurieren.

## <a name="what-is-the-meaning-of-metrics-like-message-count-or-connection-count-showed-in-azure-portal-which-kind-of-aggregation-type-should-i-choose"></a>Welche Bedeutung haben im Azure-Portal angezeigte Metriken wie die Nachrichtenanzahl oder die Anzahl der Verbindungen? Welchen Aggregationstyp sollte ich auswählen?

Ausführliche Informationen dazu, wie wir diese Metriken berechnen finden Sie [hier](signalr-concept-messages-and-connections.md).

Auf dem Blatt „Übersicht“ von Azure SignalR Service-Ressourcen ist bereits der entsprechende Aggregationstyp für Sie ausgewählt. Auf dem Blatt „Metriken“ können Sie den [hier](../azure-monitor/platform/metrics-supported.md#microsoftsignalrservicesignalr) aufgeführten Aggregationstyp als Referenz verwenden.

## <a name="what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose"></a>Welche Bedeutung hat der Dienstmodus `Default`/`Serverless`/`Classic`? Wie kann ich eine Auswahl treffen?

Modi:
* Der `Default`-Modus **erfordert** einen Hubserver. Wenn für den Hub keine Serververbindung verfügbar ist, schlagen Verbindungsversuche des Clients mit diesem Hub fehl.
* Der `Serverless`-Modus lässt Serververbindungen **NICHT** zu, d. h. alle Serververbindungen werden abgelehnt. Alle Clients müssen sich im serverlosen Modus befinden.
* Der `Classic`-Modus ist ein gemischter Status. Wenn ein Hub über eine Serververbindung verfügt, wird der neue Client an den Hubserver weitergeleitet. Andernfalls wechselt der Client in den serverlosen Modus.

  Dies kann zu Problemen führen, z. B. dass alle Serververbindungen für einen Moment verloren gehen und einige Clients in den serverlosen Modus wechseln, anstatt an den Hubserver weitergeleitet zu werden.

Auswählen einer Option:
1. Wenn kein Hubserver verfügbar ist, wählen Sie `Serverless` aus.
1. Wenn alle Hubs über Hubserver verfügen, wählen Sie `Default` aus.
1. Wenn nur einige Hubs über Hubserver verfügen, wählen Sie `Classic` aus. Da dies jedoch zu Problemen führen kann, empfiehlt es sich, stattdessen zwei Instanzen zu erstellen – eine mit `Serverless` und eine weitere mit `Default`.

## <a name="any-feature-differences-when-using-azure-signalr-for-aspnet-signalr"></a>Gibt es bei der Verwendung von Azure SignalR Service für ASP.NET SignalR Funktionsunterschiede?
Bei der Verwendung von Azure SignalR Service werden einige APIs und Features von ASP.NET SignalR nicht mehr unterstützt:
- Die Möglichkeit, beliebige Zustände zwischen Clients und dem Hub zu übergeben (häufig als `HubState` bezeichnet), wird bei der Verwendung von Azure SignalR Service nicht unterstützt.
- Die `PersistentConnection`-Klasse wird bei der Verwendung von Azure SignalR Service noch nicht unterstützt.
- Der **Forever Frame-Transport** wird bei der Verwendung von Azure SignalR Service nicht unterstützt.
- Azure SignalR Service gibt keine an den Client gesendeten Nachrichten mehr wieder, wenn der Client offline ist.
- Bei der Verwendung von Azure SignalR Service wird der Datenverkehr für eine Clientverbindung für die Dauer Verbindung immer an eine App-Serverinstanz weitergeleitet (dies wird auch als **festes Routing** bezeichnet).

Bei der Unterstützung für ASP.NET SignalR liegt der Fokus auf der Kompatibilität. Daher werden nicht alle neuen Features von ASP.NET Core SignalR unterstützt. Beispielsweise sind **MessagePack**, **Streaming** usw. nur für ASP.NET Core SignalR-Anwendungen verfügbar.

Azure SignalR Service kann für verschiedene Dienstmodi konfiguriert werden: `Classic`/`Default`/`Serverles`s. Bei dieser ASP.NET-Unterstützung wird der `Serverless`-Modus nicht unterstützt. Die REST-API auf Datenebene wird ebenfalls nicht unterstützt.

## <a name="where-do-my-data-reside"></a>Wo befinden sich meine Daten?

Azure SignalR Service ist ein Datenverarbeitungsdienst. Inhalte von Kunden werden nicht gespeichert, und die Data Residency ist durch den Entwurf garantiert. Wenn Sie Azure SignalR Service in Verbindung mit anderen Azure-Diensten (z. B. Azure Storage) zu Diagnosezwecken verwenden, finden Sie [hier](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) Informationen zum Kontrollieren der Data Residency in Azure-Regionen.
