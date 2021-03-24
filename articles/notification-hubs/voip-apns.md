---
title: Senden von APNS-VOIP-Benachrichtigungen mit Azure Notification Hubs
description: Erfahren Sie mehr über das Senden von APNS-VOIP-Benachrichtigungen über Azure Notification Hubs (nicht offiziell unterstützt).
author: sethmanheim
ms.author: sethm
ms.date: 3/23/2020
ms.topic: how-to
ms.service: notification-hubs
ms.openlocfilehash: c99af881b8f93b75633741c2352dc5df17dd2963
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "80137650"
---
# <a name="use-apns-voip-through-notification-hubs-not-officially-supported"></a>Verwenden von APNS-VOIP über Notification Hubs (nicht offiziell unterstützt)

Es ist möglich, APNS-VOIP-Benachrichtigungen über Azure Notification Hubs zu verwenden, jedoch gibt es keine offizielle Unterstützung für dieses Szenario.

## <a name="considerations"></a>Überlegungen

Falls Sie dennoch APNS-VOIP-Benachrichtigungen über Notification Hubs senden möchten, beachten Sie die folgenden Einschränkungen:

- Zum Senden einer VOIP-Benachrichtigung muss der Header `apns-topic` auf die Anwendungspaket-ID mit dem Suffix `.voip` festgelegt werden. Für eine Beispiel-App mit der Paket-ID `com.microsoft.nhubsample` sollte der Header `apns-topic` beispielsweise auf `com.microsoft.nhubsample.voip.` festgelegt werden.

   Diese Methode funktioniert mit Azure Notification Hubs nicht gut, da die Paket-ID der App als Teil der APNS-Anmeldeinformationen des Hubs konfiguriert werden muss und der Wert nicht geändert werden kann. Außerdem lässt Notification Hubs nicht zu, dass der Wert des Headers `apns-topic` zur Laufzeit überschrieben wird.

   Zum Senden von VOIP-Benachrichtigungen müssen Sie einen separaten Benachrichtigungshub mit der App-Paket-ID `.voip` konfigurieren.

- Zum Senden einer VOIP-Benachrichtigung muss der Header `apns-push-type` auf den Wert `voip` festgelegt werden.

   Zur Unterstützung von Kunden beim Übergang zu iOS 13 versucht Notification Hubs, den korrekten Wert für den Header `apns-push-type` abzuleiten. Die Rückschlusslogik ist absichtlich einfach, um zu vermeiden, dass Standardbenachrichtigungen unterbrochen werden. Leider verursacht diese Methode Probleme mit VOIP-Benachrichtigungen, da Apple diese als Sonderfall betrachtet, für den nicht die gleichen Regeln wie für Standardbenachrichtigungen gelten.

   Zum Senden von VOIP-Benachrichtigungen müssen Sie einen expliziten Wert für den Header `apns-push-type` angeben.

- Notification Hubs beschränkt die APNS-Nutzlast laut Apple-Dokumentation auf 4 KB. Apple lässt für VOIP-Benachrichtigungen Nutzlasten von bis zu 5 KB zu. Notification Hubs unterscheidet nicht zwischen Standard- und VOIP-Benachrichtigungen. Daher sind alle Benachrichtigungen auf 4 KB beschränkt.

   Zum Senden von VOIP-Benachrichtigungen dürfen Sie die Beschränkung der Nutzlastgröße von 4 KB nicht überschreiten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Links:

- [Dokumentation zu den `apns-topic`- und `apns-push-type`-Headern und -Werten, einschließlich der Sonderfälle für VOIP-Benachrichtigungen](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/sending_notification_requests_to_apns)

- [Dokumentation zur Beschränkung der Nutzlastgröße](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/generating_a_remote_notification)

- [Notification Hubs-Updates für iOS 13](push-notification-updates-ios-13.md#apns-push-type)
