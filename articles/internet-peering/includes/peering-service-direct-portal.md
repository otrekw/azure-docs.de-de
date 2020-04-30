---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: e5804aa1b005e670d8b430b1c0a3bd62efd0bb06
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687043"
---
1. Wählen Sie eine Peeringverbindung aus, die Sie für Azure Peering Service aktivieren möchten. Wählen Sie dann **...**  > **Verbindung bearbeiten** aus.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung: „Verbindung bearbeiten“](../media/setup-direct-modify-editconnection.png)
1. Wählen Sie unter **Use for Peering Service** (Für Peering Service verwenden) die Option **Aktiviert** und dann **Speichern** aus.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung: Aktivieren von Peering Service](../media/setup-direct-modify-editconnectionsettings-peering-service.png)
1. Auf dem Bildschirm **Übersicht** werden die Details der Bereitstellung angezeigt. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressource wechseln** aus.
    > [!div class="mx-imgBorder"]
    > ![Ihre Bereitstellung wurde abgeschlossen.](../media/setup-direct-modify-overview-deployment-complete.png)

1. Wählen Sie im Bereich **Registered Prefixes** (Registrierte Präfixe) die Option **Add registered prefix** (Registriertes Präfix hinzufügen) aus.
    > [!div class="mx-imgBorder"]
    > ![Hinzufügen eines registrierten Präfixes](../media/setup-direct-modify-add-registered-prefix.png)
1. Registrieren Sie ein Präfix, indem Sie einen **Namen** und ein **Präfix** auswählen und dann **Speichern** auswählen.
    > [!div class="mx-imgBorder"]
    >  ![Registrieren eines Präfixes](../media/setup-direct-modify-register-a-prefix.png) 

1. Nachdem ein Präfix erstellt wurde, wird es in der Liste der **registrierten Präfixe** angezeigt. Wählen Sie den **Namen** des Präfixes aus, um weitere Details anzuzeigen.
    > [!div class="mx-imgBorder"]
    > ![Registrierte Präfixe und Verbindungen](../media/setup-direct-modify-registered-prefixes.png)
1. Auf der Seite der registrierten Präfixe werden alle Details angezeigt. Dazu gehört auch der **Präfixschlüssel** für die einzelnen Präfixe. Dieser Schlüssel muss dem Kunden, dem dieses Präfix zugeordnet wurde, von seinem Internetdienstanbieter zur Verfügung gestellt werden. Der Kunde kann dann sein Präfix innerhalb seines Abonnements mit diesem Schlüssel registrieren.
    > [!div class="mx-imgBorder"]
    > ![Präfix mit Präfixschlüssel](../media/setup-direct-modify-registered-prefix-detail.png)