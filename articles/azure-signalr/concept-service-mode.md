---
title: Dienstmodi in Azure SignalR Service
description: In diesem Artikel finden Sie eine Übersicht über verschiedene Dienstmodi in Azure SignalR Service mit Erläuterungen zu den Unterschieden und anwendbaren Benutzerszenarien.
author: chenkennt
ms.service: signalr
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: kenchen
ms.openlocfilehash: 2cdce64b0ff03521a5848e2b4fd6e01431b5cc16
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89514733"
---
# <a name="service-mode-in-azure-signalr-service"></a>Dienstmodi in Azure SignalR Service

Der Dienstmodus ist ein wichtiges Konzept in Azure SignalR Service. Wenn Sie eine neue SignalR-Ressource erstellen, werden Sie aufgefordert, einen Dienstmodus anzugeben:

:::image type="content" source="media/concept-service-mode/create.png" alt-text="Auswählen des Dienstmodus bei der Erstellung":::

Sie können ihn auch später im Menü „Einstellungen“ ändern:

:::image type="content" source="media/concept-service-mode/update.png" alt-text="Auswählen des Dienstmodus bei der Erstellung":::

Azure SignalR Service unterstützt derzeit drei Dienstmodi: **Standard**, **serverlos** und **klassisch**. Die SignalR-Ressource verhält sich in den verschiedenen Modi unterschiedlich. In diesem Artikel erfahren Sie, welche Unterschiede dies sind und wie Sie den richtigen Dienstmodus für Ihr Szenario auswählen.

## <a name="default-mode"></a>Standardmodus

Der Standardmodus ist die Standardeinstellung für den Dienstmodus, wenn Sie eine neue SignalR-Ressource erstellen. In diesem Modus funktioniert Ihre Anwendung als typische SignalR-Anwendung mit ASP.NET Core (oder ASP.NET) mit einem Webserver, der einen Hub hostet (im Folgenden als „Hubserver“ bezeichnet). Die Clients können im Duplexmodus in Echtzeit mit dem Hubserver kommunizieren. Der einzige Unterschied besteht darin, dass Client und Server nicht direkt miteinander verbunden werden, sondern sowohl Client als auch Server eine Verbindung mit SignalR Service herstellen und den Dienst als Proxy verwenden. Im folgenden Diagramm wird die typische Anwendungsstruktur im Standardmodus veranschaulicht:

:::image type="content" source="media/concept-service-mode/default.png" alt-text="Auswählen des Dienstmodus bei der Erstellung":::

Wenn Sie über eine SignalR-Anwendung verfügen und diese mit SignalR Service integrieren möchten, sollte der Standardmodus daher in den meisten Fällen die richtige Wahl sein.

### <a name="connection-routing-in-default-mode"></a>Verbindungsrouting im Standardmodus

Im Standardmodus gibt es WebSocket-Verbindungen zwischen dem Hubserver und SignalR Service (sogenannte Serververbindungen). Diese Verbindungen werden zum Übertragen von Nachrichten zwischen Server und Client verwendet. Wenn ein neuer Client verbunden wird, leitet SignalR Service den Client über vorhandene Serververbindungen an einen Hubserver weiter (sofern Sie über mehrere Server verfügen). Die Clientverbindung ist während ihrer gesamten Lebensdauer an denselben Hubserver gebunden. Wenn der Client Nachrichten sendet, erfolgt dies an denselben Hubserver. Durch dieses Verhalten können Sie bestimmte Zustände für einzelne Verbindungen auf Ihrem Hubserver sicher verwalten. Wenn Sie z. B. etwas zwischen Server und Client streamen möchten, müssen Sie nicht den Fall berücksichtigen, dass Datenpakete an verschiedene Server gesendet werden.

> [!IMPORTANT]
> Dies bedeutet aber auch, dass ein Client im Standardmodus keine Verbindung mit einem Server herstellen kann, ohne dass diese zuvor eingerichtet wird. Wenn alle Verbindungen mit Ihren Hubservern aufgrund einer Netzwerkunterbrechung oder eines Serverneustarts getrennt sind, erhält der Client beim Versuch, eine Verbindung herzustellen, einen Fehler mit dem Hinweis, dass kein Server verbunden ist. Daher müssen Sie sicherstellen, dass zu jedem Zeitpunkt mindestens ein Hubserver mit SignalR Service verbunden ist (indem Sie z. B. mehrere Hubserver einrichten und sicherstellen, dass sie etwa bei Wartungsarbeiten nicht gleichzeitig offline geschaltet werden).

Dieses Routingmodell bedeutet auch, dass Verbindungen mit einem Hubserver getrennt werden, wenn dieser offline geschaltet wird. Sie sollten daher Verbindungstrennungen erwarten, wenn Ihr Hubserver für Wartungsmaßnahmen offline ist, und die Wiederherstellung der Verbindung ordnungsgemäß planen, damit keine negativen Auswirkungen auf Ihre Anwendung entstehen.

## <a name="serverless-mode"></a>Serverloser Modus

Der serverlose Modus ist, wie der Name schon sagt, ein Modus, in dem Sie keinen Hubserver haben. Im Gegensatz zum Standardmodus erfordert der Client in diesem Modus keinen Hubserver, um eine Verbindung herstellen zu können. Alle Verbindungen mit dem Dienst erfolgen serverlos. Der Dienst ist für die Verwaltung der Clientverbindungen wie das Behandeln von Clientpings verantwortlich (im Standardmodus wird dies von den Hubservern erledigt).

Außerdem gibt es in diesem Modus keine Serververbindung (wenn Sie versuchen, das Dienst-SDK zum Herstellen einer Serververbindung zu verwenden, erhalten Sie einen Fehler). Daher gibt es auch kein Verbindungsrouting und keine Server-Client-Bindung (wie im Abschnitt zum Standardmodus beschrieben). Sie können jedoch weiterhin über eine serverseitige Anwendung Nachrichten an Clients pushen. Dies kann auf zwei Arten erfolgen: Sie können [REST-APIs](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) für das einmalige Senden verwenden oder eine WebSocket-Verbindung einrichten, sodass Sie mehrere Nachrichten effizienter senden können. (Beachten Sie, dass sich diese WebSocket-Verbindung von einer Serververbindung unterscheidet.)

> [!NOTE]
> Sowohl das Verfahren mit REST-APIs als auch das mit WebSockets wird über das [Verwaltungs-SDK](https://github.com/Azure/azure-signalr/blob/dev/docs/management-sdk-guide.md) von SignalR Service unterstützt. Wenn Sie eine andere Sprache als .NET verwenden, können Sie die REST-APIs auch manuell aufrufen. Nutzen Sie dazu diese [Spezifikation](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md).
>
> Wenn Sie Azure Functions verwenden, können Sie [SignalR Service-Bindungen für Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-bindings-signalr-service) (im Folgenden als Funktionsbindung bezeichnet) verwenden, um Nachrichten als Ausgabebindung zu senden.

Auch Ihre Serveranwendung kann Nachrichten und Verbindungsereignisse von Clients empfangen. Der Dienst übermittelt Nachrichten und Verbindungsereignisse an vorkonfigurierte Endpunkte (als Upstream bezeichnet) mithilfe von Webhooks. Im Vergleich zum Standardmodus gibt es keine Garantie für die dauerhafte Bindung, und HTTP-Anforderungen sind eventuell weniger effizient als WebSocket-Verbindungen.

Weitere Informationen zum Konfigurieren von Upstreamübertragungen finden Sie in diesem [Dokument](https://docs.microsoft.com/azure/azure-signalr/concept-upstream).

Im folgenden Diagramm wird die Funktionsweise des serverlosen Modus veranschaulicht:

:::image type="content" source="media/concept-service-mode/serverless.png" alt-text="Auswählen des Dienstmodus bei der Erstellung":::

> [!NOTE]
> Beachten Sie, dass Sie im Standardmodus auch REST-APIs, das Verwaltungs-SDK und Funktionsbindungen verwenden können, um Nachrichten direkt an den Client zu senden, wenn Sie den Hubserver nicht verwenden möchten. Im Standardmodus werden Clientverbindungen jedoch weiterhin von den Hubservern verarbeitet, und der Upstream funktioniert in diesem Modus nicht.

## <a name="classic-mode"></a>Klassischer Modus

Der klassische Modus ist ein gemischter Modus aus Standardmodus und serverlosem Modus. In diesem Modus wird der Verbindungsmodus danach festgelegt, ob beim Herstellen einer Clientverbindung ein Hubserver verbunden ist. Bei vorhandenem Hubserver wird die Clientverbindung an einen Hubserver weitergeleitet. Andernfalls wird der serverlose Modus angewandt, in dem Client-zu-Server-Nachrichten nicht an den Hubserver übermittelt werden können. Dies führt zu einigen Konflikten. Wenn z. B. alle Hubserver für kurze Zeit nicht verfügbar sind, werden alle während dieser Zeit erstellten Clientverbindungen im serverlosen Modus ausgeführt und können daher nicht für Nachrichten an den Hubserver verwendet werden.

> [!NOTE]
> Der klassische Modus dient hauptsächlich für die Abwärtskompatibilität von Anwendungen, die vor der Einführung des Standardmodus und des serverlosen Modus erstellt wurden. Es wird dringend empfohlen, diesen Modus nicht mehr zu verwenden. Wählen Sie für neue Anwendungen basierend auf Ihrem Szenario den Standardmodus oder den serverlosen Modus aus. Für vorhandene Anwendungen empfiehlt es sich ebenfalls, Ihre Anwendungsfälle zu überprüfen und den geeignetsten Dienstmodus auszuwählen.

Der klassische Modus unterstützt auch einige neue Features im serverlosen Modus wie Upstream nicht.

## <a name="choose-the-right-service-mode"></a>Auswählen des richtigen Dienstmodus

Sie kennen nun die Unterschiede zwischen den Dienstmodi und wissen, wie Sie sich für einen Dienstmodus entscheiden. Wie Sie bereits im vorherigen Abschnitt gelernt haben, wird der klassische Modus nicht empfohlen. Sie sollten daher nur zwischen Standardmodus und serverlosem Modus auswählen. Im Folgenden finden Sie einige Tipps, die Ihnen bei der richtigen Auswahl für neue Anwendungen und für den Umstieg vom klassischen Modus für vorhandene Anwendungen helfen.

* Wenn Sie bereits mit der Funktionsweise der SignalR-Bibliothek vertraut sind und von selbstgehostetem SignalR zu Azure SignalR Service wechseln möchten, wählen Sie den Standardmodus aus. Der Standardmodus funktioniert genauso wie selbstgehostetes SignalR (und Sie können das gleiche Programmiermodell in der SignalR-Bibliothek verwenden). SignalR Service dient lediglich als Proxy zwischen den Clients und den Hubservern.

* Wenn Sie eine neue Anwendung erstellen und keine Hubserver und Serververbindungen verwalten möchten, verwenden Sie den serverlosen Modus. Dieser Modus funktioniert in der Regel zusammen mit Azure Functions, sodass Sie keine Server verwalten müssen. Sie können weiterhin die Duplexkommunikation (über REST-API/Verwaltungs-SDK/Funktionsbindung und Upstream) nutzen, aber das Programmiermodell unterscheidet sich von der SignalR-Bibliothek.

* Wenn Sie sowohl über Hubserver für Clientverbindungen als auch eine Back-End-Anwendung zum direkten Pushen von Nachrichten an Clients verfügen (z. B. über eine REST-API), sollten Sie trotzdem den Standardmodus verwenden. Denken Sie daran, dass der Hauptunterschied zwischen dem Standardmodus und dem serverlosen Modus darin besteht, ob Sie über Hubserver verfügen und wie Clientverbindungen weitergeleitet werden. REST-API, Verwaltungs-SDK und Funktionsbindungen können in beiden Modi verwendet werden.

* Wenn Sie z. B. in einem gemischten Szenario über zwei unterschiedliche Hubs in derselben SignalR-Ressource verfügen (einer als herkömmlicher SignalR-Hub und einer mit Azure Functions), aber kein Hubserver vorhanden ist, sollten Sie in Erwägung ziehen, diese in zwei SignalR-Ressourcen aufzuteilen und eine im Standardmodus und die andere im serverlosen Modus auszuführen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Standardmodus und serverlosem Modus finden Sie in den folgenden Artikeln:

* [Ausführliche Informationen zu Azure SignalR Service](signalr-concept-internals.md)

* [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](signalr-concept-serverless-development-config.md)
