---
title: Exemplarische Vorgehensweise für Austauschpeering
titleSuffix: Azure
description: Exemplarische Vorgehensweise für Austauschpeering
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: 97430d86cba1107ef67264a181925d2a4d7c12c3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97586821"
---
# <a name="exchange-peering-walkthrough"></a>Exemplarische Vorgehensweise für Austauschpeering

In diesem Abschnitt werden die Schritte zum Einrichten und Verwalten eines Austauschpeerings erläutert.

## <a name="create-an-exchange-peering"></a>Erstellen eines Austauschpeerings
> [!div class="mx-imgBorder"]
> ![Workflow für Austauschpeering und Verbindungsstatus](./media/exchange-peering.png)

Die Bereitstellung eines Austauschpeerings umfasst folgende Schritte:
1. Machen Sie sich anhand der [Peeringrichtlinie](https://peering.azurewebsites.net/peering) von Microsoft mit den Anforderungen für das Austauschpeering vertraut.
1. Ermitteln Sie unter [PeeringDB](https://www.peeringdb.com/net/694) den Microsoft-Peeringstandort und die ID der Peeringeinrichtung.
1. Fordern Sie ein Austauschpeering für einen Peeringstandort an. Ausführlichere Informationen finden Sie im Artikel [Erstellen oder Ändern eines Austauschpeerings mithilfe von PowerShell](howto-exchange-powershell.md).
1. Microsoft überprüft die übermittelte Peeringanforderung und setzt sich bei Bedarf mit Ihnen in Verbindung.
1. Nach der Genehmigung lautet der Verbindungsstatus „Approved“ (Genehmigt).
1. Konfigurieren Sie die BGP-Sitzung, und benachrichtigen Sie Microsoft.
1. Wir stellen eine BGP-Sitzung mit DENY ALL-Richtlinie und End-to-End-Validierung bereit.
1. Bei Erfolg erhalten Sie eine Benachrichtigung, dass der Verbindungsstatus des Peerings aktiv ist.
1. Daraufhin wird Datenverkehr über das neue Peering zugelassen.

Beachten Sie, dass der Verbindungsstatus nicht mit dem Standardstatus von [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)-Sitzungen verwechselt werden darf.

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Konvertieren einer älteren Instanz für Austauschpeering in eine Azure-Ressource
Führen Sie die folgenden Schritte aus, um ein älteres Austauschpeering in eine Azure-Ressource zu konvertieren:
1. Gehen Sie wie unter [Konvertieren eines älteren Austauschpeerings in eine Azure-Ressource mithilfe von PowerShell](howto-legacy-exchange-powershell.md) beschrieben vor.
1. Nachdem Sie die Konvertierungsanforderung eingereicht haben, überprüft Microsoft die Anforderung und kontaktiert Sie bei Bedarf.
1. Nach der Genehmigung hat Ihr Austauschpeering den Verbindungsstatus „Active“ (Aktiv).

## <a name="deprovision-exchange-peering"></a>Aufheben der Bereitstellung eines Austauschpeerings
Setzen Sie sich mit [Microsoft-Peering](mailto:peering@microsoft.com) in Verbindung, um die Bereitstellung des Austauschpeerings aufzuheben.

Wenn für ein Austauschpeering die Aufhebung der Bereitstellung angefordert wurde, lautet der Verbindungsstatus **PendingRemove** (Entfernen steht aus).

> [!NOTE]
> Wenn Sie ein PowerShell-Cmdlet zum Löschen des Austauschpeerings ausführen, während der Verbindungsstatus „ProvisioningStarted“ oder „ProvisioningCompleted“ lautet, tritt ein Fehler auf.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit den [Voraussetzungen für das Einrichten von Peering mit Microsoft](prerequisites.md) vertraut.
