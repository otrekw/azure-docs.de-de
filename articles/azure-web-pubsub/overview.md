---
title: Worum handelt es sich beim Azure Web PubSub-Dienst?
description: Informieren Sie sich über typische Anwendungsszenarien für die Verwendung von Azure Web PubSub, und lernen Sie die wichtigsten Vorteile von Azure Web PubSub kennen.
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 01/27/2021
ms.openlocfilehash: 920c55746ab78f9479c37027b479af7cf6604865
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108166892"
---
# <a name="what-is-azure-web-pubsub-service"></a>Worum handelt es sich beim Azure Web PubSub-Dienst? 

Der Azure Web PubSub-Dienst unterstützt Sie beim einfachen Erstellen von Echtzeit-Messagingwebanwendungen mithilfe von WebSockets und des Veröffentlichen-Abonnieren-Musters. Diese Echtzeitfunktion ermöglicht das Veröffentlichen von Inhaltsupdates zwischen Server und verbundenen Clients (z. B. Single-Page-Webanwendung oder mobile Anwendung). Die Clients müssen nicht die neuesten Updates abrufen oder neue HTTP-Anforderungen für Updates übermitteln.

Dieser Artikel enthält eine Übersicht über den Azure Web PubSub-Dienst.

## <a name="what-is-azure-web-pubsub-service-used-for"></a>Wofür wird der Azure Web PubSub-Dienst verwendet?

Der Azure Web PubSub-Dienst kann in allen Szenarien verwendet werden, für die Veröffentlichen-Abonnieren-Messaging in Echtzeit zwischen Server und Clients oder zwischen Clients erforderlich ist. Herkömmliche Echtzeitfeatures, die häufig das Abrufen vom Server oder das Übermitteln von HTTP-Anforderungen erfordern, können ebenfalls den Azure Web PubSub-Dienst verwenden.

Azure Web PubSub wird in einer Vielzahl von Branchen für unterschiedlichste Anwendungstypen verwendet, die Inhaltsupdates in Echtzeit erfordern. Die folgende Liste enthält einige Beispiele, für die sich Azure Web PubSub besonders eignet:

* **Sehr häufige Datenaktualisierungen:** Spiele, Abstimmungen, Befragungen, Auktionen
* **Livedashboards und -überwachung:** Unternehmensdashboard, Finanzmarktdaten, sofortige Vertriebsupdates, Ranglisten bei Mehrspieler-Games und IoT-Überwachung
* **Plattformübergreifender Livechat:** Livechatraum, Chatbot, Onlinekundensupport, Einkaufshilfen in Echtzeit, Messenger, In-Game-Chat usw.
* **Echtzeitstandort auf Karten:** Nachverfolgung in der Logistik, Nachverfolgung des Lieferstatus, Transportstatusupdates, GPS-Apps
* **Zielgerichtete Werbung in Echtzeit:** personalisierte Push-Werbung und Angebote in Echtzeit, interaktive Anzeigen
* **Zusammenarbeits-Apps:** gemeinsames Verfassen, Whiteboard-Apps und Software für Teambesprechungen
* **Sofortige Pushbenachrichtigungen:** soziale Netzwerke, E-Mail, Spiele, Reisewarnung
* **Echtzeitübertragungen:** Live-Übertragung von Audio-/Videosendungen, Live-Untertitelung, Übersetzung, Übertragungen von Veranstaltungen/Nachrichten
* **IoT und verbundene Geräte:** IoT-Echtzeitmetriken, Remotesteuerung, Echtzeitstatus und Standortverfolgung
* **Automatisierung:** Echtzeitauslöser aus Upstreamereignissen

## <a name="what-are-the-benefits-using-azure-web-pubsub-service"></a>Welche Vorteile bietet Azure Web PubSub?

**Integrierte Unterstützung für umfangreiche Clientverbindungen und hochverfügbare Architekturen:**

Azure Web PubSub ist für sehr umfangreiche Echtzeitanwendungen konzipiert. Damit erlaubt der Dienst die Zusammenarbeit mehrerer Instanzen und damit eine Skalierung auf Millionen von Clientverbindungen. Der Dienst unterstützt außerdem mehrere globale Regionen für Sharding, Hochverfügbarkeit oder Notfallwiederherstellung.

**Unterstützung für eine Vielzahl von Client-SDKs und Programmiersprachen:**

Azure Web PubSub funktioniert mit einer breiten Palette von Clients, z. B. webbasierte und mobile Browser, Desktop-Apps, mobile Apps, Serverprozesse, IoT-Geräte und Spielekonsolen. Da dieser Dienst das unformatierte WebSocket mit dem Veröffentlichen-Abonnieren-Muster unterstützt, kann einfach jedes WebSocket-Standardclient-SDK in verschiedenen Sprachen mit diesem Dienst verwendet werden. 

**Funktionsreiche APIs für unterschiedliche Nachrichtenmuster:**

Der Azure Web PubSub-Dienst ist ein bidirektionaler Messagingdienst, der verschiedene Messagingmuster zwischen Servern und Clients zulässt, z. B.:

* Der Server sendet Nachrichten an eine bestimmte Verbindung, an alle Verbindungen oder an eine Teilmenge von Verbindungen, die zu einem bestimmten Benutzer gehören oder in einer beliebigen Gruppe platziert wurden. 
* Der Client sendet Nachrichten an eine bestimmte Verbindung, an alle Verbindungen oder an eine Teilmenge von Verbindungen, die zu einem beliebigen Gruppe gehören.
* Die Clients senden Nachrichten an den Server.


## <a name="how-to-use-the-azure-web-pubsub-service"></a>Wie wird der Azure Web PubSub-Dienst verwendet?

Es gibt viele verschiedene Möglichkeiten für die Programmierung mit Azure Web PubSub, etwa:

- **Erstellen von serverlosen Echtzeitanwendungen:** Verwenden Sie die Integration von Azure Functions in den Azure Web PubSub-Dienst, um serverlose Echtzeitanwendungen in Sprachen wie JavaScript, C#, Java und Python zu erstellen. 
- **Senden von Nachrichten vom Server an Clients über die REST-API:** Der Web PubSub-Dienst bietet eine REST-API, um es Anwendungen zu ermöglichen, Nachrichten an verbundene Clients in allen REST-fähigen Programmiersprachen zu senden.