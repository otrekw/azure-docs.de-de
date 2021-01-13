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
ms.openlocfilehash: e06b5261ca6923e158c818d236a30cf6ebff189b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81680932"
---
In diesem Abschnitt wird beschrieben, wie Sie die folgenden Änderungsvorgänge für Direct-Peering ausführen.

### <a name="add-exchange-peering-connections"></a>Hinzufügen von Exchange Peering-Verbindungen

1. Wählen Sie die Schaltfläche **+ Verbindungen hinzufügen** aus, und konfigurieren Sie eine neue Peeringverbindung.
    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-exchange-modify-addconnection.png)
1. Füllen Sie das Formular **Exchange peering Connection** (Exchange-Peeringverbindung) aus, und wählen Sie **Speichern** aus. Hilfe zum Konfigurieren einer Peeringverbindung finden Sie in den Schritten zum Erstellen und Bereitstellen einer Direct-Peeringverbindung.
    > [!div class="mx-imgBorder"]
    > ![Formular für Exchange-Peeringverbindungen](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Entfernen von Exchange Peering-Verbindungen

1. Wählen Sie eine Peeringverbindung aus, die Sie löschen möchten, und wählen Sie dann **...**  > **Verbindung löschen** aus.
    > [!div class="mx-imgBorder"]
    > ![Schaltfläche „Verbindung löschen“](../media/setup-exchange-modify-deleteconnection.png)
1. Geben Sie die Ressourcen-ID im Feld **Löschvorgang bestätigen** ein, und wählen Sie **Löschen** aus.
    > [!div class="mx-imgBorder"]
    > ![Bestätigung des Löschvorgangs](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-an-ipv4-or-ipv6-session-on-active-connections"></a>Hinzufügen von IPv4-/IPv6-Sitzungen in aktiven Verbindungen

1. Wählen Sie eine Peeringverbindung aus, die Sie ändern möchten, und wählen Sie dann **...**  > **Verbindung bearbeiten** aus.
    > [!div class="mx-imgBorder"]
    > ![Schaltfläche „Verbindung bearbeiten“](../media/setup-exchange-modify-editconnection.png)
1. Fügen Sie Informationen zur **IPv4-Adresse** oder **IPv6-Adresse** hinzu, und wählen Sie **Speichern** aus.
    > [!div class="mx-imgBorder"]
    > ![Ändern von Peeringverbindungen](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-an-ipv4-or-ipv6-session-on-active-connections"></a>Entfernen von IPv4-/IPv6-Sitzungen aus aktiven Verbindungen

Das Entfernen einer IPv4-/IPv6-Sitzung aus einer vorhandenen Verbindung über das Portal wird derzeit nicht unterstützt. Wenden Sie sich für weitere Informationen an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).