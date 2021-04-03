---
title: Weiterleitung und Tagausdrücke in Azure Notification Hubs
description: Erfahren Sie etwas über Weiterleitung und Tagausdrücke für Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 12/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 4ff8225522e79e2be40682fb5e4823777dde2aa0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88998301"
---
# <a name="routing-and-tag-expressions"></a>Weiterleitung und Tagausdrücke

## <a name="overview"></a>Übersicht

Tagausdrücke ermöglichen Ihnen, bestimmte Sammlungen von Geräten oder speziell Registrierungen beim Senden einer Pushbenachrichtigung über Notification Hubs als Ziele anzugeben.

## <a name="targeting-specific-registrations"></a>Verwenden bestimmter Registrierungen als Ziel

Die einzige Möglichkeit zum Verwenden bestimmter Benachrichtigungsregistrierungen als Ziel ist das Zuordnen von Tags und das anschließende Verwenden dieser Tags als Ziele. Wie in [Registrierungsverwaltung](notification-hubs-push-notification-registration-management.md) erläutert, muss eine App ein Gerätehandle auf einem Benachrichtigungshub registrieren, um Pushbenachrichtigungen empfangen zu können. Nachdem die App eine Registrierung auf einem Benachrichtigungshub erstellt hat, kann das Anwendungs-Back-End Pushbenachrichtigungen an sie senden. Das Anwendungs-Back-End kann folgendermaßen auswählen, an welche Registrierungen eine bestimmte Benachrichtigung gesendet werden soll:

1. **Broadcast:** Alle Registrierungen im Notification Hub empfangen die Benachrichtigung.
2. **Tag:** Alle Registrierungen, die das angegebene Tag enthalten, empfangen die Benachrichtigung.
3. **Tagausdruck:** Alle Registrierungen, deren Satz von Tags mit dem angegebenen Ausdruck übereinstimmt, empfangen die Benachrichtigung.

## <a name="tags"></a>`Tags`

Ein Tag kann eine beliebige bis zu 120 Zeichen umfassende Zeichenfolge mit alphanumerischen und den folgenden nicht-alphanumerischen Zeichen sein: „`_`“, „`@`“, „`#`“, „`.`“, „`:`“, „`-`“. Das folgende Beispiel zeigt eine Anwendung, von der Sie Popupbenachrichtigungen zu bestimmten Musikgruppen empfangen können. In diesem Szenario besteht eine einfache Möglichkeit zum Weiterleiten von Benachrichtigungen im Kennzeichnen von Registrierungen mit Tags, die die verschiedenen Gruppen angeben. Dies ist in der folgenden Abbildung dargestellt:

![Übersicht über Tags](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags.png)

In der Abbildung erreicht die als **Beatles** markierte Nachricht nur das Tablet, das mit dem Tag **Beatles** registriert ist.

Weitere Informationen zum Erstellen von Registrierungen für Tags finden Sie unter [Registrierungsverwaltung](notification-hubs-push-notification-registration-management.md).

Sie können Benachrichtigungen mithilfe der Methoden zum Senden von Benachrichtigungen der `Microsoft.Azure.NotificationHubs.NotificationHubClient` -Klasse im [Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) -SDK an Tags senden. Sie können auch Node.js oder die Pushbenachrichtigungs-REST-APIs verwenden.  Hier ist ein Beispiel mit dem SDK.

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You requested a Beatles notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You requested a Wailers notification</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");
```

Tags müssen nicht vorab bereitgestellt werden und können sich auf mehrere App-spezifische Konzepte beziehen. Beispielsweise können Benutzer dieser Beispielanwendung Musikgruppen kommentieren und Popups erhalten. Dies gilt nicht nur für die Kommentare zu ihren Lieblingsgruppen, sondern für alle Kommentare von ihren Freunden, unabhängig von den Gruppen, die diese bewerteten. Die folgende Abbildung zeigt ein Beispiel für dieses Szenario:

![Freunde markieren](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags2.png)

In diesem Beispiel ist Alice an Nachrichten über die Beatles interessiert, und Bob ist an Nachrichten über die Wailers interessiert. Bob ist außerdem an Charlies Kommentaren interessiert, und Charlie ist an den Wailers interessiert. Wenn eine Benachrichtigung für Charlies Kommentar zu den Beatles gesendet wird, sendet Notification Hubs sie an Alice und Bob.

Es können zwar mehrere Konzepte in Tags codiert werden (z. B. `band_Beatles` oder `follows_Charlie`), Tags sind jedoch einfache Zeichenfolgen und keine Eigenschaften mit Werten. Eine Registrierung wird nur auf das Vorhandensein oder Fehlen eines bestimmten Tags überprüft.

Ein vollständiges schrittweises Tutorial zur Verwendung von Tags zum Senden an Interessengruppen finden Sie unter [Aktuelle Nachrichten](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

> [!NOTE]
> Azure Notification Hubs unterstützt maximal 60 Tags pro Registrierung.

## <a name="using-tags-to-target-users"></a>Verwenden von Tags mit Benutzern als Ziel

Eine weitere Möglichkeit zur Verwendung von Tags ist das Identifizieren aller Geräte, die einem bestimmten Benutzer zugeordnet sind. Sie können eine Registrierung mit einem Tag kennzeichnen, das eine Benutzer-ID enthält. Dies ist in der folgenden Abbildung dargestellt:

![Benutzer markieren](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags3.png)

In der Abbildung erreicht die Nachricht mit dem Tag `user_Alice` alle mit `user_Alice` markierten Geräte.

## <a name="tag-expressions"></a>Tagausdrücke

In einigen Fällen hat eine Benachrichtigung eine Gruppe von Registrierungen zum Ziel, die nicht durch ein einzelnes Tag, sondern durch einen booleschen Ausdruck für Tags identifiziert ist.

Beispiel: Eine Sportanwendung sendet eine Erinnerung an alle Benutzer in Boston zu einem Spiel zwischen den Red Sox und den Cardinals. Wenn die Client-App Tags zu einem Interesse an Mannschaften und Orten registriert, sollte die Benachrichtigung alle Benutzer in Boston als Ziel verwenden, die an den Red Sox oder den Cardinals interessiert ist. Diese Bedingung kann mit dem folgenden booleschen Ausdruck ausgedrückt werden:

```csharp
(follows_RedSox || follows_Cardinals) && location_Boston
```

![Tagausdrücke](./media/notification-hubs-tags-segment-push-message/notification-hubs-tags4.png)

Tagausdrücke unterstützen gängige boolesche Operatoren wie `AND` (`&&`), `OR` (`||`) und `NOT` (`!`), und sie können auch Klammern enthalten. Tagausdrücke, die nur `OR`-Operatoren verwenden, können auf 20 Tags verweisen. Ausdrücke mit `AND`-Operatoren, aber ohne `OR`-Operatoren, können auf 10 Tags verweisen. Ansonsten sind Tagausdrücke auf 6 Tags beschränkt.

Es folgt ein Beispiel für das Senden von Benachrichtigungen mit Tagausdrücken mithilfe des SDK:

```csharp
Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

String userTag = "(location_Boston && !follows_Cardinals)";

// Windows 8.1 / Windows Phone 8.1
var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

// Windows 10
toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
"You want info on the Red Sox</text></binding></visual></toast>";
outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
```
