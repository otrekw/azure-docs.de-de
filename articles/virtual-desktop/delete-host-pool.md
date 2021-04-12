---
title: Löschen des Windows Virtual Desktop-Hostpools – Azure
description: Erfahren Sie, wie Sie Hostpools in Windows Virtual Desktop löschen.
author: Heidilohr
ms.topic: how-to
ms.date: 07/11/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 3693a82a0d9c9544ed6a14d4596d544725125bd3
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106447045"
---
# <a name="delete-a-host-pool"></a>Löschen eines Hostpools

Alle in Windows Virtual Desktop erstellten Hostpools sind Sitzungshosts und App-Gruppen angefügt. Zum Löschen eines Hostpools müssen Sie die zugehörigen App-Gruppen und Sitzungshosts löschen. Das Löschen einer App-Gruppe ist recht einfach, aber das Löschen eines Sitzungshosts ist komplizierter. Wenn Sie einen Sitzungshost löschen, müssen Sie sicherstellen, dass auf ihm keine aktiven Benutzersitzungen ausgeführt werden. Alle Benutzersitzungen auf dem Sitzungshost sollten abgemeldet werden, um zu verhindern, dass Benutzer Daten verlieren.

## <a name="delete-a-host-pool-with-powershell"></a>Löschen eines Hostpools mit PowerShell

Zum Löschen eines Hostpools mithilfe von PowerShell müssen Sie zunächst alle App-Gruppen im Hostpool löschen. Führen Sie das folgende PowerShell-Cmdlet aus, um alle App-Gruppen zu löschen:

```powershell
Remove-AzWvdApplicationGroup -Name <appgroupname> -ResourceGroupName <resourcegroupname>
```

Führen Sie anschließend dieses Cmdlet aus, um den Hostpool zu löschen:

```powershell
Remove-AzWvdHostPool -Name <hostpoolname> -ResourceGroupName <resourcegroupname> -Force:$true
```

Mit diesem Cmdlet werden alle vorhandenen Benutzersitzungen auf dem Sitzungshost des Hostpools entfernt. Außerdem wird die Registrierung des Sitzungshosts beim Hostpool aufgehoben. Alle zugehörigen virtuellen Computer (Virtual Machines, VMs) sind weiterhin in Ihrem Abonnement vorhanden.

## <a name="delete-a-host-pool-with-the-azure-portal"></a>Löschen eines Hostpools mit dem Azure-Portal

So löschen Sie einen Hostpool mit dem Azure-Portal:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Suchen Sie nach **Windows Virtual Desktop**, und wählen Sie diese Option aus.

3. Wählen Sie im Menü auf der linken Seite des Bildschirms **Hostpools** aus, und wählen Sie dann den Namen des Hostpools aus, den Sie löschen möchten.

4. Wählen Sie im Menü links auf der Seite die Option **Anwendungsgruppen** aus.

5. Wählen Sie alle Anwendungsgruppen in dem Hostpool aus, den Sie löschen möchten, und wählen Sie dann **Entfernen** aus.

6. Nachdem Sie die App-Gruppen entfernt haben, wechseln Sie zum Menü auf der linken Seite, und wählen Sie **Übersicht** aus.

7. Wählen Sie **Entfernen**.

8. Wenn der zu löschende Hostpool Sitzungshosts enthält, wird eine Meldung angezeigt, in der Ihre Erlaubnis zum Fortfahren abgefragt wird. Wählen Sie **Ja** aus.

9. Der Azure-Portal entfernt jetzt alle Sitzungshosts und löscht den Hostpool. Die virtuellen Computer, die mit dem Sitzungshost in Zusammenhang stehen, werden nicht gelöscht und bleiben in Ihrem Abonnement erhalten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen eines Hostpools finden Sie in den folgenden Artikeln:

- [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md)

Weitere Informationen zum Konfigurieren von Hostpooleinstellungen finden Sie in den folgenden Artikeln:

- [Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool](customize-rdp-properties.md)
- [Konfigurieren der Lastenausgleichsmethode für Windows Virtual Desktop](configure-host-pool-load-balancing.md)
- [Konfigurieren des Zuweisungstyps für den Hostpool mit persönlichen Desktops](configure-host-pool-personal-desktop-assignment-type.md)