---
title: Exemplarische Vorgehensweise für direktes Peering
titleSuffix: Azure
description: Exemplarische Vorgehensweise für direktes Peering
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 12/15/2020
ms.author: prmitiki
ms.openlocfilehash: 6a1fa7f582511a2089bab2694f9f5ec47634ff54
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97592380"
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
1. Bei Erfolg erhalten Sie eine Benachrichtigung, dass das Peering nun den Verbindungsstatus „Active“ (Aktiv) hat.
1. Daraufhin wird Datenverkehr über das neue Peering zugelassen.

Beachten Sie, dass der Verbindungsstatus nicht mit dem Standardstatus von [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)-Sitzungen verwechselt werden darf.

## <a name="convert-a-legacy-direct-peering-to-azure-resource"></a>Konvertieren einer älteren Instanz für direktes Peering in eine Azure-Ressource
Die folgenden Schritte müssen ausgeführt werden, um ein Legacy-Direct Peering in eine Azure-Ressource zu konvertieren:
1. Befolgen Sie die Anweisungen in [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource](howto-legacy-direct-powershell.md)
1. Microsoft überprüft die übermittelte Konvertierungsanforderung und setzt sich bei Bedarf mit Ihnen in Verbindung.
1. Nach der Genehmigung wird Ihr Direct Peering mit dem Verbindungsstatus aktiv angezeigt.

## <a name="deprovision-direct-peering"></a>Aufheben der Bereitstellung von Direct Peering
Setzen Sie sich mit dem [Microsoft Peering](mailto:peering@microsoft.com)-Team in Verbindung, um die Bereitstellung von Direct Peering aufzuheben.

Wenn für die Aufhebung der Bereitstellung ein Direct Peering festgelegt ist, wird der Verbindungsstatus als **PendingRemove** angezeigt.

> [!NOTE]
> Wenn Sie ein PowerShell-Cmdlet ausführen, um das Direct Peering zu löschen, wenn der ConnectionState ProvisioningStarted oder ProvisioningCompleted ist, schlägt der Vorgang fehl.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit den [Voraussetzungen für das Einrichten von Peering mit Microsoft](prerequisites.md) vertraut.
