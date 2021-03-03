---
title: Nachrichten und Verbindungen in Azure SignalR Service
description: Eine Übersicht über die Hauptkonzepte im Zusammenhang mit Nachrichten und Verbindungen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: zhshang
ms.openlocfilehash: 9d0e94cf2318db777bb44c15037f73531cd969fa
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100593324"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Nachrichten und Verbindungen in Azure SignalR Service

Das Abrechnungsmodell für Azure SignalR Service basiert auf der Anzahl von Verbindungen und Nachrichten. In diesem Artikel erfahren Sie, wie Nachrichten und Verbindungen definiert und für Abrechnungszwecke gezählt werden.


## <a name="message-formats"></a>Nachrichtenformate 

Azure SignalR Service unterstützt die gleichen Formate wie ASP.NET Core SignalR: [JSON](https://www.json.org/) und [MessagePack](/aspnet/core/signalr/messagepackhubprotocol).

## <a name="message-size"></a>Nachrichtengröße

Die Nachrichtengröße ist bei Azure SignalR Service nicht beschränkt.

Große Nachrichten werden in kleinere Nachrichten mit einer Größe von jeweils maximal 2 KB aufgeteilt und separat übertragen. Das Aufteilen und Zusammensetzen der Meldungen wird von SDKs erledigt. Hierfür ist kein Entwickleraufwand erforderlich.

Große Nachrichten wirken sich negativ auf die Leistung der Nachrichtenübermittlung aus. Verwenden Sie nach Möglichkeit kleinere Nachrichten, und bestimmen Sie anhand von Tests die optimale Nachrichtengröße für das jeweilige Szenario.

## <a name="how-messages-are-counted-for-billing"></a>Zählung von Nachrichten für die Abrechnung

Für die Abrechnung werden nur ausgehende Nachrichten von Azure SignalR Service gezählt. Pingnachrichten zwischen Clients und Servern werden ignoriert.

Nachrichten mit einer Größe von mehr als 2 KB werden als mehrere Nachrichten mit jeweils 2 KB gezählt. Das Diagramm mit der Nachrichtenanzahl im Azure-Portal wird hubspezifisch alle 100 Nachrichten aktualisiert.

Beispiel: Angenommen, Sie verfügen über einen Anwendungsserver und drei Clients:

Der App-Server sendet eine Nachricht mit 1 KB an alle verbundenen Clients. Dabei wird die Nachricht vom App-Server an den Dienst als kostenlose eingehende Nachricht betrachtet. Nur die drei Nachrichten, die vom Dienst an jeden Client gesendet werden, werden als ausgehende Nachrichten abgerechnet.

Client A sendet eine Nachricht mit 1 KB an Client B. Die Nachricht wird dabei nicht über den App-Server geleitet. Die Nachricht von Client A an den Dienst ist eine kostenlose eingehende Nachricht. Die Nachricht vom Dienst an Client B wird als ausgehende Nachricht in Rechnung gestellt.

Sie besitzen drei Clients und einen Anwendungsserver. Ein Client sendet eine 4 KB große Nachricht, damit der Server einen Broadcast an alle Clients durchführt. In diesem Fall werden acht kostenpflichtige Nachrichten gezählt: eine Nachricht vom Dienst an den Anwendungsserver und drei Nachrichten vom Dienst an die Clients. Jede Nachricht wird als zwei Nachrichten mit jeweils 2 KB gezählt.

## <a name="how-connections-are-counted"></a>Zählung von Verbindungen

Es gibt Server- und Clientverbindungen mit Azure SignalR Service. Standardmäßig beginnt jeder Anwendungsserver mit fünf Anfangsverbindungen pro Hub, und jeder Client hat eine Clientverbindung.

Die im Azure-Portal angezeigte Verbindungsanzahl umfasst sowohl Server- als auch Clientverbindungen.

Ein Beispiel: Angenommen, Sie verfügen über zwei Anwendungsserver und definieren fünf Hubs im Code. Dadurch ergeben sich 50 Serververbindungen: 2 App-Server × 5 Hubs × 5 Verbindungen pro Hub

Bei ASP.NET SignalR werden Serververbindungen anders berechnet. In diesem Fall ist zusätzlich zu den definierten Hubs ein einzelner Standardhub enthalten. Jeder Anwendungsserver benötigt standardmäßig fünf weitere anfängliche Serververbindungen. Die Anzahl anfänglicher Verbindungen für den Standardhub bleibt mit anderen Hubs konsistent.

Der Dienst und der Anwendungsserver synchronisieren weiterhin den Verbindungsstatus und passen die Serververbindungen an, um eine höhere Leistung und Dienststabilität zu erzielen.  So kann es vorkommen, dass sich die Anzahl der Serververbindungen von Zeit zu Zeit ändert.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Berücksichtigung von ein-/ausgehendem Datenverkehr

Die an den Dienst gesendete Nachricht ist eine eingehende Nachricht. Die vom Dienst gesendete Nachricht ist eine ausgehende Nachricht. Datenverkehr wird in Bytes berechnet.

## <a name="related-resources"></a>Zugehörige Ressourcen

- [Unterstützte Metriken von Azure Monitor](../azure-monitor/essentials/metrics-supported.md#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR-Konfiguration](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)