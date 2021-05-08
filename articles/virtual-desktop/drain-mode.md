---
title: Festlegen des Windows Virtual Desktop-Ausgleichsmodus – Azure
description: Konfigurieren und Verwenden des Ausgleichsmodus in Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/14/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 19ef7d520800ac703ed77dc0520e5b860306c4bd
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107508951"
---
# <a name="set-drain-mode"></a>Festlegen des Ausgleichsmodus

Der Ausgleichsmodus isoliert einen Sitzungshost, wenn Sie Patches anwenden und Wartungsarbeiten ohne Unterbrechung der Benutzersitzungen ausführen möchten. Bei Isolation akzeptiert der Sitzungshost keine neuen Benutzersitzungen. Alle neuen Verbindungen werden an den nächsten verfügbaren Sitzungshost umgeleitet. Vorhandene Verbindungen im Sitzungshost funktionieren weiterhin, bis sich der Benutzer abmeldet oder der Administrator die Sitzung beendet. Wenn sich der Sitzungshost im Ausgleichsmodus befindet, können Administratoren auch eine Remoteverbindung mit dem Server herstellen, ohne Windows Virtual Desktop zu verwenden. Sie können diese Einstellung sowohl auf gepoolte als auch auf persönliche Desktops anwenden.

## <a name="set-drain-mode-using-the-azure-portal"></a>Festlegen des Ausgleichsmodus mithilfe des Azure-Portals

So aktivieren Sie den Ausgleichsmodus im Azure-Portal

1. Öffnen Sie das Azure-Portal, und wechseln Sie zu dem Hostpool, den Sie isolieren möchten.

2. Wählen Sie im Navigationsmenü **Sitzungshosts** aus.

3. Wählen Sie als Nächstes die Hosts aus, für die Sie den Ausgleichsmodus aktivieren möchten, und klicken Sie auf **Ausgleichsmodus aktivieren**.

4. Um den Ausgleichsmodus zu deaktivieren, wählen Sie die Hostpools aus, für die der Ausgleichsmodus aktiviert ist, und klicken Sie dann auf **Ausgleichsmodus deaktivieren**.

## <a name="set-drain-mode-using-powershell"></a>Festlegen des Ausgleichsmodus mithilfe von PowerShell

Sie können den Ausgleichsmodus in PowerShell über den Parameter *AllowNewSessions* festlegen, der zum Befehl [Update-AzWvdSessionhost](/powershell/module/az.desktopvirtualization/update-azwvdsessionhost?view=azps-5.8.0&preserve-view=true) gehört.

Führen Sie dieses Cmdlet aus, um den Ausgleichsmodus zu aktivieren:

```powershell
Update-AzWvdSessionHost -ResourceGroupName <resourceGroupName> -HostPoolName <hostpoolname> -Name <hostname> -AllowNewSession:$False
```

Führen Sie dieses Cmdlet aus, um den Ausgleichsmodus zu deaktivieren:

```powershell
Update-AzWvdSessionHost -ResourceGroupName <resourceGroupName> -HostPoolName <hostpoolname> -Name <hostname> -AllowNewSession:$True
```

>[!IMPORTANT]
>Sie müssen diesen Befehl für jeden Sitzungshost ausführen, auf den Sie die Einstellung anwenden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mehr über das Azure-Portal für Windows Virtual Desktop erfahren möchten, sehen Sie sich [unsere Tutorials](create-host-pools-azure-marketplace.md) an. Wenn Sie bereits mit den Grundlagen vertraut sind, sehen Sie sich einige der anderen Features an, die Sie mit dem Azure-Portal verwenden können, z. B. das [MSIX-Feature zum Anfügen von Apps](app-attach-azure-portal.md) und [Azure Advisor](azure-advisor.md).

Wenn Sie die PowerShell-Methode verwenden und sehen möchten, welche weiteren Aktionen das Modul durchführen kann, lesen Sie [Einrichten des PowerShell-Moduls für Windows Virtual Desktop](powershell-module.md) und unsere [PowerShell-Referenz](/powershell/module/az.desktopvirtualization/).