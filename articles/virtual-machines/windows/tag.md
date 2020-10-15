---
title: Kennzeichnen einer VM mithilfe von PowerShell
description: Erfahren Sie etwas über das Kennzeichnen eines virtuellen Computers mithilfe von PowerShell.
author: mmccrory
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 3aa40ed396e87b342207fc51576cd28170c7d4e9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89322380"
---
# <a name="how-to-tag-a-virtual-machine-in-azure-using-powershell"></a>Kennzeichnen eines virtuellen Computers in Azure mithilfe von PowerShell
In diesem Artikel werden verschiedene Methoden zum Markieren eines virtuellen Windows-Computers in Azure mithilfe des Resource Manager-Bereitstellungsmodells beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 50 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Tags nur für Ressourcen unterstützt werden, die über das Resource Manager-Bereitstellungsmodell erstellt wurden. Wenn Sie einen virtuellen Computer mit der Azure-Befehlszeilenschnittstelle kennzeichnen möchten, finden Sie weitere Informationen unter [Kennzeichnen eines virtuellen Computers in Azure über die Azure-Befehlszeilenschnittstelle](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Markieren mit PowerShell
Zum Erstellen, Hinzufügen und Löschen von Tags über PowerShell müssen Sie zunächst Ihre [PowerShell-Umgebung mit dem Azure-Ressourcen-Manager][PowerShell environment with Azure Resource Manager]einrichten. Sobald Sie die Einrichtung abgeschlossen haben, können Sie die Tags auf Compute-, Netzwerk- und Speicherressourcen platzieren, entweder während der Erstellung oder nachdem die Ressource mithilfe von PowerShell erstellt wurde. Dieser Artikel konzentriert sich auf das Anzeigen und Bearbeiten von Tags, die auf virtuellen Computern platziert sind.

 

Navigieren Sie zuerst über das `Get-AzVM` -Cmdlet zu einem virtuellen Computer.

```azurepowershell
PS C:\> Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"
```

Wenn der virtuelle Computer bereits Tags enthält, werden auf der Ressource anschließend alle Tags angezeigt:

```json
Tags : {
        "Application": "MyApp1",
        "Created By": "MyName",
        "Department": "MyDepartment",
        "Environment": "Production"
        }
```

Wenn Sie Tags über PowerShell hinzufügen möchten, können Sie den Befehl `Set-AzResource` verwenden. Beachten Sie, dass beim Aktualisieren von Tags über PowerShell die Tags als Ganzes aktualisiert werden. Wenn Sie ein Tag auf eine Ressource hinzufügen, die bereits über Tags verfügt, müssen Sie alle Tags einschließen, die auf der Ressource platziert werden sollen. Es folgt ein Beispiel für das Hinzufügen zusätzlicher Tags zu einer Ressource über PowerShell-Cmdlets.

Das erste Cmdlet legt alle Tags, die auf *MyTestVM* platziert sind, mithilfe der Eigenschaften `Get-AzResource` und `Tags` auf die *$tags*-Variable fest.

```azurepowershell
PS C:\> $tags = (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags
```

Der zweite Befehl zeigt die Tags für die angegebene Variable an.

```azurepowershell
PS C:\> $tags

Key           Value
----          -----
Department    MyDepartment
Application   MyApp1
Created By    MyName
Environment   Production
```

Der dritte Befehl fügt ein zusätzliches Tag für die *$tags* -Variable hinzu. Beachten Sie, wie **+=** verwendet wird, um das neue Schlüssel-Wert-Paar an die *$tags* -Liste anzufügen.

```azurepowershell
PS C:\> $tags += @{Location="MyLocation"}
```

Der vierte Befehl legt alle Tags, die gemäß der *$tags* -Variablen definiert sind, auf die angegebene Ressource fest. In diesem Fall ist dies „MyTestVM“.

```azurepowershell
PS C:\> Set-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags
```

Der fünfte Befehl zeigt alle Tags auf der Ressource an. Wie Sie sehen können, wird der *Speicherort* jetzt als Tag mit *MeinStandort* als Wert definiert.

```azurepowershell
PS C:\> (Get-AzResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Key           Value
----          -----
Department    MyDepartment
Application   MyApp1
Created By    MyName
Environment   Production
Location      MyLocation
```

Weitere Informationen zum Festlegen von Tags über PowerShell finden Sie in den [Azure Resource-Cmdlets][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Markieren Ihrer Azure-Ressourcen finden Sie unter [Übersicht über den Azure Resource Manager][Azure Resource Manager Overview] und [Verwenden von Tags zum Organisieren von Azure-Ressourcen][Using Tags to organize your Azure Resources].
* Informationen dazu, wie Tags Ihnen helfen können, die Verwendung Ihrer Azure-Ressourcen zu verwalten, finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure][Understanding your Azure Bill] und [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure][Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/management/manage-resources-powershell.md
[Azure Resource Cmdlets]: /powershell/module/az.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/management/overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/management/tag-resources.md
[Understanding your Azure Bill]:../../cost-management-billing/understand/review-individual-bill.md
[Gain insights into your Microsoft Azure resource consumption]:../../cost-management-billing/manage/usage-rate-card-overview.md
