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
ms.openlocfilehash: e3e87e59f5b3c95051d9ee53e4b8d87afe9d9ba8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773745"
---
Dieser Abschnitt beschreibt, wie Sie die folgenden Änderungsvorgänge für Direct Peering ausführen:

### <a name="add-exchange-peering-connections"></a>Hinzufügen von Exchange Peering-Verbindungen

1. Klicken Sie oben auf die Schaltfläche **+ Verbindungen hinzufügen**, und konfigurieren Sie eine neue Peeringverbindung.
    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-exchange-modify-addconnection.png)
1. Füllen Sie das Formular **Exchange Peering-Verbindung** aus, und klicken Sie auf **Speichern**. Hilfe zum Konfigurieren einer Peeringverbindung finden Sie oben in den Schritten unter „Erstellen und Bereitstellen eines Direct Peerings“.
    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-exchange-modify-savenewconnection.png)

### <a name="remove-exchange-peering-connections"></a>Entfernen von Exchange Peering-Verbindungen

1. Klicken Sie auf eine Peeringverbindung, die Sie löschen möchten, und klicken Sie dann auf die Schaltfläche **...**  > **Verbindung** löschen.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung löschen](../media/setup-exchange-modify-deleteconnection.png)
1. Geben Sie die Ressourcen-ID in das Feld **Löschen bestätigen** ein, wie in den hervorgehobenen Feldern dargestellt, und klicken Sie auf **Löschen**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung DeleteConfirm](../media/setup-exchange-modify-deleteconnectionconfirm.png)

### <a name="add-ipv4ipv6-session-on-active-connections"></a>Hinzufügen einer IPv4/IPv6-Sitzung zu den aktiven Verbindungen

1. Klicken Sie auf eine Peeringverbindung, die Sie ändern möchten, und klicken Sie dann auf die Schaltfläche **...**  > **Verbindung bearbeiten**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung bearbeiten](../media/setup-exchange-modify-editconnection.png)
1. Fügen Sie **IPv4-Adress**- oder **IPv6-Adress**-Informationen hinzu, und klicken Sie auf **Speichern**.
    > [!div class="mx-imgBorder"]
    > ![Peeringverbindung ändern](../media/setup-exchange-modify-editconnectionsettings.png)

### <a name="remove-ipv4ipv6-session-on-active-connections"></a>Entfernen einer IPv4/IPv6-Sitzung aus aktiven Verbindungen

Das Entfernen einer IPv4/IPv6-Sitzung aus einer vorhandenen Verbindung wird derzeit im Portal nicht unterstützt. Wenden Sie sich an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).