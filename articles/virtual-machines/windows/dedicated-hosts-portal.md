---
title: Bereitstellen von dedizierten Azure-Hosts über das Portal
description: Bereitstellen von VMs auf dedizierten Hosts über das Portal.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure
ms.date: 03/10/2020
ms.author: cynthn
ms.openlocfilehash: b6f5e155b76535c4d9e0080983d5f54cec3adb01
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79086948"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-portal"></a>Bereitstellen von VMs auf dedizierten Hosts über das Portal

Dieser Artikel führt Sie durch die Erstellung eines [dedizierten Azure-Hosts](dedicated-hosts.md) zum Hosten Ihrer virtuellen Computer (VMs). 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>Erstellen einer VM

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
1. Wählen Sie auf der Seite **Neu** unter **Beliebt** die Option **Windows Server 2016 Datacenter** aus.
1. Stellen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** sicher, dass das richtige Abonnement ausgewählt ist, und wählen Sie dann *myDedicatedHostsRG* als **Ressourcengruppe** aus. 
1. Geben Sie unter **Instanzdetails***myVM* als **Namen des virtuellen Computers** ein, und wählen Sie *USA, Osten* als Ihre **Ort** aus.
1. Wählen Sie unter **Verfügbarkeitsoptionen** die Option **Verfügbarkeitszone** aus, und wählen Sie in der Dropdownliste *1* aus.
1. Wählen Sie für die Größe **Größe ändern** aus. Wählen Sie in der Liste der verfügbaren Größen eine der Esv3-Serie aus, z. B. **Standard E2s v3**. Möglicherweise müssen Sie den Filter löschen, um alle verfügbaren Größen anzuzeigen.
1. Geben Sie unter **Administratorkonto** einen Benutzernamen wie *azureuser* an sowie ein Kennwort. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](faq.md#what-are-the-password-requirements-when-creating-a-vm) erfüllen.
1. Wählen Sie unter **Regeln für eingehende Ports** die Option **Ausgewählte Ports zulassen** aus, und wählen Sie dann **RDP (3389)** aus der Dropdownliste aus.
1. Wählen Sie oben auf der Seite die Registerkarte **Erweitert** und im Abschnitt **Host** den Eintrag *myHostGroup* für **Hostgruppe**  und *myHost* für den **Host** aus. 
    ![Auswählen von Hostgruppe und Host](./media/dedicated-hosts-portal/advanced.png)
1. Belassen Sie die übrigen Standardeinstellungen, und wählen Sie dann die Schaltfläche **Überprüfen + erstellen** am unteren Rand der Seite aus.
1. Wenn eine Meldung über die erfolgreiche Validierung angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="add-an-existing-vm"></a>Hinzufügen einer vorhandenen VM 

Sie können einem dedizierten Host eine vorhandene VM hinzufügen, aber die VM muss zunächst beendet bzw. ihre Zuordnung aufgehoben werden. Stellen Sie vor dem Verschieben einer VM auf einen dedizierten Host sicher, dass die VM-Konfiguration unterstützt wird:

- Die VM-Größe muss sich in derselben Größenfamilie wie der dedizierte Host befinden. Wenn der dedizierte Host z. B. DSv3 ist, kann die VM-Größe Standard_D4s_v3, aber nicht Standard_A4_v2 sein. 
- Die VM muss sich in derselben Region wie der dedizierte Host befinden.
- Die VM darf nicht Teil einer Näherungsplatzierungsgruppe sein. Entfernen Sie die VM aus der Näherungsplatzierungsgruppe, bevor Sie sie auf einen dedizierten Host verschieben. Weitere Informationen finden Sie unter [Verschieben einer VM aus einer Näherungsplatzierungsgruppe](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#move-an-existing-vm-out-of-a-proximity-placement-group).
- Die VM darf sich nicht in einer Verfügbarkeitsgruppe befinden.
- Wenn sich die VM in einer Verfügbarkeitszone befindet, muss es sich um dieselbe Verfügbarkeitszone wie die Hostgruppe handeln. Die Verfügbarkeitszoneneinstellungen für die VM und die Hostgruppe müssen identisch sein.

Verschieben Sie die VM mithilfe des [Portals](https://portal.azure.com) auf einen dedizierten Host.

1. Öffnen Sie die Seite für die VM.
1. Wählen Sie **Beenden** aus, um die VM zu beenden bzw. ihre Zuordnung aufzuheben.
1. Wählen Sie im Menü auf der linken Seite die Option **Konfiguration** aus.
1. Wählen Sie eine Hostgruppe und einen Host aus den Dropdownmenüs aus.
1. Wählen Sie im oberen Bereich der Seite **Speichern** aus, wenn Sie fertig sind.
1. Nachdem die VM dem Host hinzugefügt wurde, klicken Sie im linken Menü auf **Übersicht**.
1. Wählen Sie oben auf der Seite **Starten** aus, um die VM neu zu starten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der Übersicht zu [dedizierten Hosts](dedicated-hosts.md). 

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.

- Sie können dedizierte Hosts auch über [Azure PowerShell](dedicated-hosts-powershell.md) bereitstellen.
