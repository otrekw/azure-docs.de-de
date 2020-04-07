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
ms.openlocfilehash: 60752cf1b3c05ab7817083e70310ba7b40227dec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129926"
---
1. Klicken Sie auf eine Peeringverbindung, die Sie für Peering Service aktivieren möchten, und klicken Sie anschließend auf **...**  > **Verbindung bearbeiten**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung: Bearbeiten](../media/setup-direct-modify-editconnection.png)
1. Klicken Sie im Abschnitt ***Use for Peering Service*** (Für Peering Service verwenden) auf **Aktiviert** und anschließend auf **Speichern**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung: Aktivieren für Peering Service](../media/setup-direct-modify-editconnectionsettings-peering-service.png)
1. Auf dem Übersichtsbildschirm werden die Details der Bereitstellung angezeigt. Klicken Sie nach der Bereitstellung auf **Zu Ressource wechseln**.
    > [!div class="mx-imgBorder"]
    > ![Ihre Bereitstellung wurde abgeschlossen.](../media/setup-direct-modify-overview-deployment-complete.png)

1. Unter Einstellungen wird dann **Registrierte Präfixe** angezeigt. Klicken Sie auf **Registriertes Präfix hinzufügen**.
    > [!div class="mx-imgBorder"]
    > ![Registrierte Präfixe und Verbindungen](../media/setup-direct-modify-add-registered-prefix.png)
1. Registrieren Sie ein Präfix, indem Sie einen **Namen** und ein **Präfix** auswählen und auf **Speichern** klicken.
    > [!div class="mx-imgBorder"]
    >  ![Registrieren eines Präfixes](../media/setup-direct-modify-register-a-prefix.png) 

1. Nachdem ein Präfix erstellt wurde, wird es in der Liste der registrierten Präfixe angezeigt. Klicken Sie auf den **Namen** des Präfixes, um weitere Details anzuzeigen.
    > [!div class="mx-imgBorder"]
    > ![Registrierte Präfixe und Verbindungen](../media/setup-direct-modify-registered-prefixes.png)
1. Auf der Seite der registrierten Präfixe werden Ihnen alle Details angezeigt, um den **Präfixschlüssel** für die einzelnen Präfixe einzubeziehen. Dieser Schlüssel muss dem Kunden, dem dieses Präfix zugeordnet wurde, von seinem Internetdienstanbieter zur Verfügung gestellt werden. Der Kunde kann dann sein Präfix innerhalb seines Abonnements mit diesem Schlüssel registrieren.
    > [!div class="mx-imgBorder"]
    > ![Präfix mit Präfixschlüssel](../media/setup-direct-modify-registered-prefix-detail.png)