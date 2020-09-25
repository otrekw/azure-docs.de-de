---
title: SMS-Konzepte in Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Hier finden Sie Informationen zu SMS-Konzepten in Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e5cfc1e27bae10a1c67e4506afe9db825664785f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944239"
---
# <a name="sms-concepts"></a>SMS-Konzepte

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Azure Communication Services ermöglicht das Senden und Empfangen von SMS-Textnachrichten unter Verwendung der SMS-Client Bibliotheken von Communication Services. Diese Clientbibliotheken können zur Unterstützung von Kundendienstszenarien, Terminerinnerungen, zweistufiger Authentifizierung und anderer Echtzeitkommunikationsanforderungen verwendet werden. Das SMS-Feature von Communication Services ermöglicht das zuverlässige Senden von Nachrichten sowie die Nutzung von Informationen zur Zustellbarkeit und zur Antwortrate rund um Ihre Kampagnen.

Zu den wichtigsten Features der SMS-Clientbibliotheken von Azure Communication Services gehören folgende:

-  **Unkomplizierte Einrichtungsumgebung** zum Hinzufügen von SMS-Funktionen zu Ihren Anwendungen
- **Unterstützung von Hochgeschwindigkeitsnachrichten** über gebührenfreie Nummern für A2P-Anwendungsfälle (Application to Person) in den USA
- **Bidirektionale Konversationen** zur Unterstützung von Szenarien wie Kundensupport, Warnungen und Terminerinnerungen
- **Zuverlässige Zustellung** mit Zustellberichten in Echtzeit für Nachrichten, die von Ihrer Anwendung gesendet wurden
- **Analysen** zur Nachverfolgung von Verwendungsmustern und Kundeninteraktion
- **Unterstützung von Abmeldungen**, um Abmeldungen für gebührenfreie Nummern automatisch zu erkennen und zu berücksichtigen. Communication Services erkennt STOP- und START-Nachrichten und sendet folgende Standardantworten an Endbenutzer: 
  - STOP: *You have successfully been unsubscribed to messages from this number. Reply START to resubscribe.* (Sie erhalten in Zukunft keine Nachrichten von dieser Nummer mehr. Antworten Sie mit „START“, um das Abonnement wieder zu aktivieren.)
  - START: *You have successfully been re-subscribed to messages from this number. Reply STOP to unsubscribe.* (Sie erhalten ab sofort wieder Nachrichten von dieser Nummer. Antworten Sie mit „STOP“, um das Abonnement zu kündigen.)
  - Die STOP- und START-Nachrichten werden an Sie weitergeleitet. Von Azure Communication Services wird die Überwachung und Implementierung dieser Abmeldungen empfohlen, um sicherzustellen, dass keine weiteren Nachrichten an Empfänger gesendet werden, die sich von Ihrer Kommunikation abgemeldet haben.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Senden einer SMS-Nachricht](../../quickstarts/telephony-sms/send.md)

Die Artikel zu den folgenden Themen könnten Sie auch interessieren:

- Informationen zur [Clientbibliothek für SMS](../telephony-sms/sdk-features.md)
- Beziehen einer für SMS geeigneten [Telefonnummer](../../quickstarts/telephony-sms/get-phone-number.md)
- [Planen Ihrer SMS-Lösung](../telephony-sms/plan-solution.md)
