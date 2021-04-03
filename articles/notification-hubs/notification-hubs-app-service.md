---
title: Notification Hubs-Integration in Mobile App Service-Apps
description: Erfahren Sie, wie Azure Notification Hubs mit Azure App Service Mobile-Apps zusammenarbeitet.
author: sethmanheim
manager: femila
services: notification-hubs
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 08/06/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: e7042be7e407e8e0827e142ba6878dfff812e1f6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88004060"
---
# <a name="integration-with-app-service-mobile-apps"></a>Integration in Mobile App Service-Apps

Um eine nahtlose und einheitliche Verwendung in Azure-Diensten zu ermöglichen, ist in [Mobile App Service-Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop) die Unterstützung von Benachrichtigungen, die über Azure Notification Hubs abgewickelt werden, bereits integriert. [Mobile App Service-Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop) bietet eine hochgradig skalierbare, global verfügbare Entwicklungsplattform für mobile Anwendungen für Unternehmensentwickler und Systemintegratoren, die umfassende Funktionen für mobile Entwickler bereitstellt.

Mobile Apps-Entwickler können Notification Hubs mit dem folgenden Workflow verwenden:

1. Abrufen des PNS-Gerätehandle
2. Registrieren des Geräts bei Notification Hubs über die Registrierungs-APIs der Mobile Apps Client SDKs

    > [!NOTE]
    > Beachten Sie, dass Mobile Apps aus Sicherheitsgründen alle Tags für Registrierungen entfernt. Arbeiten Sie direkt von Ihrem Back-End aus mit Notification Hubs, um Tags Geräten zuzuordnen.

3. Senden von Benachrichtigungen von Ihrem App-Back-End mit Notification Hubs

Diese Integration bietet u. a. folgende Vorteile:

- **Mobile Apps-Client-SDKs**: Diese plattformübergreifenden SDKs bieten APIs für die Registrierung und die Kommunikation mit dem Notification Hub, der mit der mobilen App verknüpft ist. Sie benötigen keine Notification Hubs-Anmeldeinformationen und müssen keinen zusätzlichen Dienst verwenden.
  - *Push an Benutzer:* Die SDKs markieren automatisch das angegebene Gerät mit einer bei Mobile Apps authentifizierten Benutzer-ID, um das Push-an-Benutzer-Szenario zu ermöglichen.
  - *Push an Gerät:* Die SDKs verwenden die Installations-ID von Mobile Apps automatisch als GUID für die Registrierung bei Notification Hubs, sodass nicht mehrere Dienst-GUIDs verwaltet werden müssen.
- **Installationsmodell:** Mobile Apps verwendet das neueste Notification Hubs-Pushmodell zur Darstellung aller einem Gerät zugeordneten Pusheigenschaften in einer JSON-Installation, das auf Push Notification Services ausgerichtet und einfach zu verwenden ist.
- **Flexibilität**: Entwickler können auch bei eingerichteter Integration immer mit Notification Hubs direkt arbeiten.
- **Integration in das [Azure-Portal](https://portal.azure.com)** : Push als Funktion ist in Mobile Apps visuell dargestellt, sodass Entwickler problemlos mit dem zugeordneten Notification Hub über Mobile Apps arbeiten können.
