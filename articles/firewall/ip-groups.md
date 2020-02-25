---
title: IP-Adressgruppen in Azure Firewall
description: IP-Adressgruppen ermöglichen Ihnen das Gruppieren und Verwalten von IP-Adressen für Azure Firewall-Regeln.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 74e5a427d62d5249ffe6b0426b62a3577e43462f
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444839"
---
# <a name="ip-groups-preview-in-azure-firewall"></a>IP-Adressgruppen (Vorschau) in Azure Firewall

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP-Adressgruppen ermöglichen Ihnen das Gruppieren und Verwalten von IP-Adressen für Azure Firewall-Regeln auf folgende Weise:

- Als Quelladresse in DNAT-Regeln
- Als Quell- oder Zieladresse in Netzwerkregeln
- Als Quelladresse in Anwendungsregeln


Eine IP-Adressgruppe kann eine einzelne IP-Adresse, mehrere IP-Adressen oder aber einen oder mehrere IP-Adressbereiche enthalten.

IP-Adressgruppen können in Azure Firewall-DNAT-, -Netzwerk- und -Anwendungsregeln für mehrere Firewalls regions- und abonnementübergreifend in Azure wiederverwendet werden. Gruppennamen müssen eindeutig sein. Sie können eine IP-Adressgruppe im Azure-Portal, in der Azure CLI oder der Rest-API konfigurieren. Eine Beispielvorlage wird bereitgestellt, um Ihnen den Einstieg zu erleichtern.

## <a name="sample-format"></a>Beispielformat

Die folgenden Beispiele für das IPv4-Adressformat sind für die Verwendung in IP-Adressgruppen gültig:

- Einzelne Adresse: 10.0.0.0
- CIDR-Notation: 10.1.0.0/32
- Adressbereich: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>Erstellen einer IP-Adressgruppe

Eine IP-Adressgruppe kann über das Azure-Portal, die Azure CLI oder die Rest-API erstellt werden. Weitere Informationen finden Sie unter [Erstellen einer IP-Adressgruppe (Vorschau)](create-ip-group.md).

## <a name="browse-ip-groups"></a>Durchsuchen von IP-Adressgruppen
1. Geben Sie in der Suchleiste des Azure-Portals **IP-Adressgruppen** ein, und wählen Sie sie aus. Sie können die Liste der IP-Adressgruppen anzeigen oder **Hinzufügen** auswählen, um eine neue IP-Adressgruppe zu erstellen.
2. Wählen Sie die gewünschte IP-Adressgruppe aus, um die Übersichtsseite zu öffnen. Sie können IP-Adressen oder IP-Adressgruppen bearbeiten, hinzufügen oder löschen.

   ![Übersicht über IP-Adressgruppen](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>Verwalten einer IP-Adressgruppe

Sie können in der IP-Adressgruppe alle IP-Adressen sowie die ihr zugeordneten Regeln oder Ressourcen anzeigen. Wenn Sie eine IP-Adressgruppe löschen möchten, müssen Sie sie zuerst von der Ressource trennen, von der sie gerade verwendet wird.

1. Zum Anzeigen oder Bearbeiten der IP-Adressen wählen Sie im linken Bereich unter **Einstellungen** die Option **IP-Adressen** aus.
2. Zum Hinzufügen von einer oder mehreren IP-Adresse(n) wählen Sie **IP-Adressen hinzufügen** aus. Daraufhin wird die Seite **Drag or Browse** (Ziehen oder durchsuchen) für einen Upload angezeigt, oder Sie können die Adresse manuell eingeben.
3.  Wählen Sie die Auslassungspunkte ( **...** ) rechts aus, um IP-Adressen zu bearbeiten oder zu löschen. Wenn Sie mehrere IP-Adressen bearbeiten oder löschen möchten, wählen Sie die Kontrollkästchen und dann oben **Bearbeiten** oder **Löschen** aus.
4. Schließlich können Sie die Datei im CSV-Dateiformat exportieren.

> [!NOTE]
> Wenn Sie in einer IP-Adressgruppe alle IP-Adressen löschen, während die Gruppe in einer Regel noch verwendet wird, wird diese Regel übersprungen.


## <a name="use-an-ip-group"></a>Verwenden einer IP-Adressgruppe

Sie können jetzt beim Erstellen von Azure Firewall-DNAT-, -Anwendungs- oder -Netzwerkregeln **IP-Adressgruppe** als **Quelltyp** oder **Zieltyp** für die IP-Adresse(n) auswählen.

> [!NOTE]
> IP-Adressgruppen werden in der Firewallrichtlinie nicht unterstützt. Sie werden derzeit nur bei herkömmlichen Firewallregeln unterstützt.

![IP-Adressgruppen in Firewall](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Regionale Verfügbarkeit

IP-Adressgruppen sind derzeit in den folgenden Regionen verfügbar:

- USA (Westen)
- USA, Westen 2
- East US
- USA (Ost) 2
- USA (Mitte)
- USA Nord Mitte
- USA, Westen-Mitte
- USA Süd Mitte
- Kanada, Mitte
- Nordeuropa
- Europa, Westen
- Frankreich, Mitte
- UK, Süden
- Australien (Osten)
- Australien, Mitte
- Australien, Südosten

## <a name="related-azure-powershell-cmdlets"></a>Zugehörige Azure PowerShell-Cmdlets

Die folgenden Azure PowerShell-Cmdlets können zum Erstellen und Verwalten von IP-Adressgruppen verwendet werden:

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Remove-AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [New-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [New-AzFirewallApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [New-AzFirewallNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [Azure Firewall bereitstellen und konfigurieren](tutorial-firewall-deploy-portal.md).