---
title: Exemplarische Vorgehensweise für direktes Peering
titleSuffix: Azure
description: Exemplarische Vorgehensweise für direktes Peering
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d88fcfc4d3e073bf544f2ca0f4d01dbe305b45da
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774037"
---
# <a name="direct-peering-walkthrough"></a>Exemplarische Vorgehensweise für direktes Peering

In diesem Abschnitt werden die auszuführenden Schritte zum Einrichten und Verwalten von Direct Peering erläutert.

## <a name="create-a-direct-peering"></a>Erstellen eines Direct Peering
> [!div class="mx-imgBorder"]
> ![Direct Peering-Workflow und Verbindungsstatus](./media/direct-peering.png)

Die folgenden Schritte müssen ausgeführt werden, um ein Direct Peering bereitzustellen:
1. Arbeiten Sie die Microsoft [Peeringrichtlinie](https://peering.azurewebsites.net/peering) durch, um die Anforderungen für Direct Peering zu verstehen.
1. Befolgen Sie die Anweisungen in [Erstellen oder Ändern einer Instanz für Direct Peering](howto-direct-powershell.md), um eine Peeringanforderung einzureichen.
1. Nachdem Sie eine Peeringanforderung eingereicht haben, setzt sich Microsoft mit Ihnen über Ihre registrierte E-Mail-Adresse in Verbindung, um den LOA (Letter Of Authorization) bereitzustellen oder weitere Informationen einzuholen.
1. Nachdem die Peeringanforderung genehmigt wurde, ändert sich der Verbindungsstatus in ProvisioningStarted.
1. Sie müssen folgende Schritte durchführen:
    1. die Verkabelung gemäß dem LOA fertigstellen
    1. (optional) einen Verbindungstest mit 169.254.0.0/16 ausführen
    1. die BGP-Sitzung konfigurieren und uns dann benachrichtigen.
1. Microsoft stellt eine BGP-Sitzung mit DENY ALL-Richtlinie und End-to-End-Validierung bereit.
1. Bei Erfolg erhalten Sie eine Benachrichtigung, dass der Verbindungsstatus des Peerings aktiv ist.
1. Daraufhin wird Datenverkehr durch das neue Peering zugelassen.

Beachten Sie, dass der Verbindungsstatus nicht mit dem Standardstatus von [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)-Sitzungen verwechselt werden darf.

## <a name="convert-a-legacy-direct-peering-to-azure-resource"></a>Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource
Die folgenden Schritte müssen ausgeführt werden, um ein Legacy-Direct Peering in eine Azure-Ressource zu konvertieren:
1. Befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource](howto-legacy-direct-powershell.md)
1. Nachdem Sie die Konvertierungsanforderung eingereicht haben, überprüft Microsoft die Anforderung und kontaktiert Sie bei Bedarf.
1. Nach der Genehmigung wird Ihr Direct Peering mit dem Verbindungsstatus aktiv angezeigt.

## <a name="deprovision-direct-peering"></a>Aufheben der Bereitstellung von Direct Peering
Setzen Sie sich mit dem [Microsoft Peering](mailto:peering@microsoft.com)-Team in Verbindung, um die Bereitstellung von Direct Peering aufzuheben.

Wenn für die Aufhebung der Bereitstellung ein Direct Peering festgelegt ist, wird der Verbindungsstatus als **PendingRemove** angezeigt.

> [!NOTE]
> Wenn Sie ein PowerShell-Cmdlet ausführen, um das Direct Peering zu löschen, wenn der ConnectionState ProvisioningStarted oder ProvisioningCompleted ist, schlägt der Vorgang fehl.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den [Voraussetzungen für das Einrichten von Peering mit Microsoft](prerequisites.md)
