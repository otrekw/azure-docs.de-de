---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d9ff01cf0180dae7f75d9753ba889d0caddad937
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773985"
---
Dieser Abschnitt beschreibt, wie Sie die folgenden Änderungsvorgänge für Direct Peering ausführen:

### <a name="add-direct-peering-connections"></a>Hinzufügen von Direct Peering-Verbindungen
1. Klicken Sie oben auf die Schaltfläche **+ Verbindungen hinzufügen**, und konfigurieren Sie eine neue Peeringverbindung.
    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-modify-addconnection.png)
1. Füllen Sie das Formular **Direct Peering-Verbindung** aus, und klicken Sie auf **Speichern**. Hilfe zum Konfigurieren einer Peeringverbindung finden Sie oben in den Schritten unter „Erstellen und Bereitstellen eines Direct Peerings“.
    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-modify-savenewconnection.png)

### <a name="remove-direct-peering-connections"></a>Entfernen von Direct Peering-Verbindungen

Das Entfernen einer Verbindung wird derzeit im Portal nicht unterstützt. Wenden Sie sich an das [Microsoft Peering-Team](mailto:peeringexperience@microsoft.com).

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Upgrade oder Downgrade der Bandbreite in aktiven Verbindungen
1. Klicken Sie auf eine Peeringverbindung, die Sie ändern möchten, und klicken Sie dann auf die Schaltfläche **...**  > **Verbindung bearbeiten**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung: Bearbeiten](../media/setup-direct-modify-editconnection.png)
1. Ändern Sie die Bandbreite wie unten dargestellt, und klicken Sie dann auf **Speichern**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung – Bandbreite ändern](../media/setup-direct-modify-editconnectionsettings.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Hinzufügen einer IPv4/IPv6-Sitzung in aktiven Verbindungen
1. Klicken Sie auf eine Peeringverbindung, die Sie ändern möchten, und klicken Sie dann auf die Schaltfläche **...**  > **Verbindung bearbeiten**, wie oben dargestellt.
1. Fügen Sie Informationen zum **IPv4-Sitzungspräfix** oder **IPv6-Sitzungspräfix** hinzu, und klicken Sie auf **Speichern**.

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen
    Removing a **Session IPv4 prefix** or **Session IPv6 prefix** info is not currently supported on portal. Contact [Microsoft peering](mailto:peeringexperience@microsoft.com).
