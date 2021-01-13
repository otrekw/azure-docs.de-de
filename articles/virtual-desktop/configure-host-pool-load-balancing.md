---
title: Konfigurieren des Lastenausgleichs für Windows Virtual Desktop – Azure
description: Es wird beschrieben, wie Sie die Lastenausgleichsmethode für eine Windows Virtual Desktop-Umgebung konfigurieren.
author: Heidilohr
ms.topic: how-to
ms.date: 10/12/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2c57ac10fbd318dd4bbb2dc86457e186dd824834
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91951654"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Konfigurieren der Lastenausgleichsmethode für Windows Virtual Desktop

Beim Konfigurieren der Lastenausgleichsmethode für einen Hostpool können Sie die Windows Virtual Desktop-Umgebung anpassen, damit Ihre Anforderungen besser erfüllt werden.

>[!NOTE]
> Dies gilt nicht für einen dauerhaften Desktophostpool, da Benutzer im Hostpool immer über eine 1:1-Zuordnung zu einem Sitzungshost verfügen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie die Anweisungen unter [Einrichten des Windows Virtual Desktop PowerShell-Moduls](powershell-module.md) zum Herunterladen und Installieren des PowerShell-Moduls und zum Anmelden bei Ihrem Azure-Konto befolgt haben.

## <a name="configure-breadth-first-load-balancing"></a>Konfigurieren der Lastenausgleichsmethode „Breiter Ansatz“

Die Lastenausgleichsmethode „Breiter Ansatz“ ist die Standardkonfiguration für neue nicht dauerhafte Hostpools. Bei der Lastenausgleichsmethode „Breiter Ansatz“ werden neue Benutzersitzungen auf alle verfügbaren Sitzungshosts im Hostpool verteilt. Wenn Sie die Lastenausgleichsmethode „Breiter Ansatz“ konfigurieren, können Sie ein maximales Sitzungslimit pro Sitzungshost im Hostpool festlegen.

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool zum Durchführen der Lastenausgleichsmethode „Breiter Ansatz“ zu konfigurieren, ohne das maximale Sitzungslimit anzupassen:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -LoadBalancerType 'BreadthFirst'
```

Führen Sie anschließend das folgende Cmdlet aus, um sicherzustellen, dass Sie die breitenorientierte Lastenausgleichsmethode festgelegt haben:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, LoadBalancerType

Name             : hostpoolname
LoadBalancerType : BreadthFirst
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool zum Durchführen der Lastenausgleichsmethode „Breiter Ansatz“ und zum Verwenden eines neuen maximalen Sitzungslimits zu konfigurieren:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -LoadBalancerType 'BreadthFirst' -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Konfigurieren der Lastenausgleichsmethode „Tiefer Ansatz“

Bei der Lastenausgleichsmethode „Tiefer Ansatz“ werden neue Benutzersitzungen auf einen verfügbaren Sitzungshost mit der höchsten Anzahl von Verbindungen verteilt, für den der Schwellenwert des maximalen Sitzungslimits aber noch nicht erreicht wurde.

>[!IMPORTANT]
>Wenn Sie die tiefenorientierte Lastenausgleichsmethode konfigurieren, müssen Sie ein maximales Sitzungslimit pro Sitzungshost im Hostpool festlegen.

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool für die Durchführung der Lastenausgleichsmethode „Tiefer Ansatz“ zu konfigurieren:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -LoadBalancerType 'DepthFirst' -MaxSessionLimit ###
```

>[!NOTE]
> Der Algorithmus für den tiefenorientierten Lastenausgleich verteilt Sitzungen auf der Grundlage des maximalen Grenzwerts für Hostsitzungen (`-MaxSessionLimit`). Der Standardwert dieses Parameters ist `999999`, was zugleich der höchste Wert ist, auf den diese Variable festgelegt werden kann. Dieser Parameter ist erforderlich, wenn der tiefenorientierte Lastenausgleich verwendet wird. Zum Erzielen einer optimalen Benutzererfahrung sollten Sie darauf achten, den Parameter für den maximalen Grenzwert der Hostsitzungen auf eine Zahl festzulegen, die möglichst gut zu Ihrer Umgebung passt.

Um sicherzustellen, dass die Einstellung aktualisiert wurde, führen Sie dieses Cmdlet aus:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, LoadBalancerType, MaxSessionLimit

Name             : hostpoolname
LoadBalancerType : DepthFirst
MaxSessionLimit  : 6
```

## <a name="configure-load-balancing-with-the-azure-portal"></a>Konfigurieren des Lastenausgleichs mit dem Azure-Portal

Sie können Lastenausgleich auch mit dem Azure-Portal konfigurieren.

So konfigurieren Sie Lastenausgleich:

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
2. Suchen Sie nach **Windows Virtual Desktop** , und wählen Sie diese Option unter „Dienste“ aus.
3. Wählen Sie auf der Seite „Windows Virtual Desktop“ die Option **Hostpools** aus.
4. Wählen Sie den Namen des Hostpools aus, den Sie bearbeiten möchten.
5. Wählen Sie **Eigenschaften** aus.
6. Geben Sie in das Feld das **maximale Sitzungslimit** ein, und wählen Sie im Dropdownmenü den **Lastenausgleichsalgorithmus** aus, den Sie für diesen Hostpool wünschen.
7. Wählen Sie **Speichern** aus. Die neuen Einstellungen für den Lastenausgleich werden angewendet.