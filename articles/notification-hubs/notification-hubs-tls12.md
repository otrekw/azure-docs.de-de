---
title: 'Notification Hubs: TLS-Updates'
description: Erfahren Sie mehr über die Unterstützung für TLS in Azure Notification Hubs.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/07/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/28/2020
ms.openlocfilehash: 4da96df50e961f4291029a37e883fdcf88c6c87f
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885751"
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

Um eine höhere Sicherheitsstufe zu gewährleisten, deaktiviert Notification Hubs Unterstützung für die TLS-Versionen 1.0 und 1.1 am 31. Mai 2020 (verlängert vom 30. April 2020). Diese älteren Protokolle bieten eine schwache Kryptografie und sind anfällig für BEAST- und POODLE-Angriffe. Anwendungen, die auf Geräten mit Android Version 5 oder höher oder iOS Version 5 oder höher bereitgestellt werden, sind von dieser Änderung nicht betroffen, da diese Betriebssysteme TLS 1.2 unterstützen, und der Client und der Server verhandeln die höchste gegenseitig unterstützte Version des Protokolls beim Herstellen einer Verbindung.

Es wird empfohlen, alle Anwendungen, die Azure Notification Hubs verwenden, zu überprüfen, um sicherzustellen, dass Sie die am besten anwendbaren Bibliotheken und TLS-Stapel verwenden, die TLS 1.2 unterstützen.

## <a name="update-apps"></a>Aktualisieren von Apps

Sie können mithilfe der Netzwerksicherheitsfunktion von Apple (App Transport Security, ATS) sicherstellen, dass Ihre iOS-Apps TLS 1.2 verwenden. ATS kann nicht für SDKs verwendet werden, die älter als iOS 9.0 oder macOS 10.11 sind. Weitere Informationen dazu finden Sie in der [Dokumentation von Apple](https://developer.apple.com/documentation/security/preventing_insecure_network_connections).

Legen Sie für Android-Anwendungen, die SSLSocket-Instanzen verwenden, für jede SSLSocket-Instanz aktivierte Protokolle fest, wie in [setEnabledProtocols](https://developer.android.com/reference/javax/net/ssl/SSLSocket#setEnabledProtocols(java.lang.String%5B%5D)) angegeben.

Die Tabelle auf der Unterstützungsseite für die [TLS-Protokollkompatibilität](https://support.globalsign.com/customer/portal/articles/2934392-tls-protocol-compatibility) unterstützt Sie bei der Zuordnung von Betriebssystemen zu kompatiblen TLS-Versionen.

Weitere Informationen finden Sie in der Übersicht zur [Unterstützung für TLS-Protokolle unter Windows](https://docs.microsoft.com/archive/blogs/kaushal/support-for-ssltls-protocols-on-windows).

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Notification Hubs](notification-hubs-push-notification-overview.md)