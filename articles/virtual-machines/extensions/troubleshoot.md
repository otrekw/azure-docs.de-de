---
title: Problembehandlung bei Fehlern im Zusammenhang mit Windows-VM-Erweiterungen
description: Erfahren Sie mehr über die Problembehandlung für Fehler bei Azure Windows-VM-Erweiterungen.
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: gwallace
editor: ''
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.subservice: extensions
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: b8b7a03d5176f5dbd8500b5ff9044c2f22ecbfc0
ms.sourcegitcommit: 02b1179dff399c1aa3210b5b73bf805791d45ca2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98127140"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problembehandlung bei Fehlern im Zusammenhang mit Azure Windows-VM-Erweiterungen
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Anzeigen des Erweiterungsstatus
Azure Resource Manager-Vorlagen können aus Azure PowerShell ausgeführt werden. Sobald die Vorlage ausgeführt wird, kann der Erweiterungsstatus im Azure-Ressourcen-Explorer oder in den Befehlszeilentools angezeigt werden.

Beispiel:

Azure PowerShell:

```azurepowershell
Get-AzVM -ResourceGroupName $RGName -Name $vmName -Status
```

Hier ist die Beispielausgabe:

```output
Extensions:  {
  "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
  "Name": "myCustomScriptExtension",
  "SubStatuses": [
    {
      "Code": "ComponentStatus/StdOut/succeeded",
      "DisplayStatus": "Provisioning succeeded",
      "Level": "Info",
      "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
          \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
          test.txt                          \\n\\n",
                  "Time": null
      },
    {
      "Code": "ComponentStatus/StdErr/succeeded",
      "DisplayStatus": "Provisioning succeeded",
      "Level": "Info",
      "Message": "",
      "Time": null
    }
  ]
}
```

## <a name="troubleshooting-extension-failures"></a>Problembehandlung bei Erweiterungsfehlern

### <a name="rerun-the-extension-on-the-vm"></a>Erneutes Ausführen der Erweiterung auf dem virtuellen Computer
Wenn Sie mithilfe der benutzerdefinierten Skripterweiterung Skripts auf dem virtuellen Computer ausführen, könnten ab und an Fehler auftreten, bei denen der virtuelle Computer erfolgreich erstellt wurde, das Skript jedoch fehlgeschlagen ist. Unter diesen Bedingungen empfiehlt es sich, die Erweiterung zu entfernen und die Vorlage erneut auszuführen, um den Fehler zu beheben.
Hinweis: In Zukunft wird diese Funktionalität verbessert, damit es nicht mehr notwendig ist, die Erweiterung zu deinstallieren.

#### <a name="remove-the-extension-from-azure-powershell"></a>Entfernen der Erweiterung aus Azure PowerShell
```azurepowershell
Remove-AzVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"
```

Nachdem die Erweiterung entfernt wurde, kann die Vorlage erneut ausgeführt werden, um die Skripts auf dem virtuellen Computer auszuführen.

### <a name="trigger-a-new-goalstate-to-the-vm"></a>Auslösen eines neuen GoalState für die VM
Möglicherweise stellen Sie fest, dass eine Erweiterung nicht ausgeführt wurde oder aufgrund eines fehlenden „Microsoft Azure CRP Certificate Generator“ nicht ausgeführt werden kann. (Dieses Zertifikat wird verwendet, um den Transport der geschützten Einstellungen der Erweiterung abzusichern.)
Dieses Zertifikat wird automatisch erneut generiert, indem der Windows-Gast-Agent im virtuellen Computer neu gestartet wird:
- Öffnen Sie den Task-Manager.
- Navigieren Sie zur Registerkarte „Details“.
- Suchen Sie den Prozess „WindowsAzureGuestAgent.exe“.
- Klicken Sie mit der rechten Maustaste darauf, und wählen Sie „Task beenden“ aus. Der Prozess wird automatisch neu gestartet.


Ferner können Sie für die VM einen neuen GoalState auslösen, indem Sie ein „VM Reapply“ ausführen. VM [Reapply](https://docs.microsoft.com/rest/api/compute/virtualmachines/reapply) ist eine API, die 2020 eingeführt wurde und dazu dient, den Zustand einer VM erneut anzuwenden. Wir empfehlen Ihnen, dies zu einem Zeitpunkt durchzuführen, an dem eine kurze Ausfallzeit der VM akzeptabel ist. Zwar löst Reapply seinerseits keinen Neustart der VM aus, und in der überwiegenden Mehrzahl der Fälle führt das Aufrufen von Reapply nicht zu einem Neustart der VM, es besteht jedoch das sehr kleine Risiko, dass ein anderes ausstehendes Update des VM-Modells angewendet wird, wenn Reapply einen neuen Zielzustand auslöst, und diese andere Änderung macht möglicherweise einen Neustart erforderlich. 

Azure-Portal:

Wählen Sie im Portal die VM und im linken Bereich unter **Support und Problembehandlung** die Option **Redeploy + reapply** (Erneut bereitstellen und erneut anwenden) aus. Wählen Sie dann **Reapply** (Erneut anwenden) aus.


Azure PowerShell *(ersetzen Sie den RG-Namen und den VM-Namen durch Ihre Werte)* :

```azurepowershell
Set-AzVM -ResourceGroupName <RG Name> -Name <VM Name> -Reapply
```

Azure CLI *(ersetzen Sie den RG-Namen und den VM-Namen durch Ihre Werte)* :

```azurecli
az vm reapply -g <RG Name> -n <VM Name>
```

Wenn ein „VM Reapply“ nicht funktioniert, können Sie der VM einen neuen leeren Datenträger für Daten aus dem Azure-Verwaltungsportal hinzufügen und diesen später entfernen, nachdem das Zertifikat wieder hinzugefügt wurde.
