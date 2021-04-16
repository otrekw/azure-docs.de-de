---
title: Azure-Hybridvorteil für Skalierungssätze für VM-Skalierungsgruppe
description: Erfahren Sie, wie Sie mit dem Azure-Hybridvorteil bei VM-Skalierungsgruppe, die in Azure ausgeführt werden, Geld sparen können.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mathapli
manager: rochakm
ms.service: virtual-machine-scale-sets
ms.collection: linux
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 03/20/2021
ms.author: mathapli
ms.openlocfilehash: a714434c39a0c40c2e908f2d0c424f02851921a6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105933677"
---
# <a name="azure-hybrid-benefit-for-linux-virtual-machine-scale-set-public-preview"></a>Azure-Hybridvorteil für Skalierungssätze für VM-Skalierungsgruppe (Public Preview)

**Azure-Hybridvorteil für Linux VM-Skalierungsgruppe ist im Public Review jetzt**. Der Azure-Hybridvorteil kann Ihnen helfen, die Kosten für die Ausführung ihrer RHEL-und SLES-VM- [Skalierungs Gruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)zu reduzieren.

Mit diesem Vorteil zahlen Sie nur die Infrastrukturkosten für Ihr Skalierungsgruppe. Der Vorteil ist für alle RHEL- und SLES-Images im Marketplace mit nutzungsbasierter Bezahlung verfügbar.


>[!NOTE]
> Dieser Artikel beschreibt den Azure-Hybridvorteil für Linux VMSS. Es gibt einen separaten [Artikel, der [hier für den Azure-Hybridvorteil](https://docs.microsoft.com/azure/virtual-machines/linux/azure-hybrid-benefit-linux)-Computer verfügbar ist, der für Azure-Kunden bereits seit November 2020 verfügbar ist.

## <a name="benefit-description"></a>Beschreibung des Vorteils
Mit Azure Hybrid können Sie die vorhandenen Cloud-Zugangslizenzen von Red Hat oder SUSE nutzen und VM-Skalierungsgruppen flexibel auf Bring-your-own-Subscription (BYOS)-Abrechnung umstellen. 

Bei der Bereitstellung von Images für VM-Skalierungsgruppe auf dem PAYG-Marktplatz werden sowohl Infrastruktur- als auch Software-Gebühren erhoben. Mit AHB können PAYG-Instanzen ohne Umstellung auf ein BYOS-Abrechnungsmodell umgestellt werden.

:::image type="content" source="./media/azure-hybrid-benefit-linux/azure-hybrid-benefit-linux-cost.png" alt-text="Darstellung der Kosten für den Azure-Hybridvorteil auf virtuellen Linux-Computern.":::

## <a name="scope-of-azure-hybrid-benefit-eligibility-for-linux"></a>Umfang der Azure-Hybridvorteilsberechtigung für Linux
Der Azure-Hybridvorteil ist für alle RHEL- und SLES-Images mit nutzungsbasierter Bezahlung vom Azure Marketplace verfügbar. Für RHEL- oder SLES-BYOS-Images oder benutzerdefinierte Images vom Azure Marketplace ist der Vorteil noch nicht verfügbar.

Die Hybridvorteile, Azure Dedicated Host-Instanzen und SQL berechtigen nicht zum Azure-Hybridvorteil, wenn Sie bereits den Vorteil für virtuelle Linux-Computer nutzen.

## <a name="get-started"></a>Erste Schritte

### <a name="red-hat-customers"></a>Red Hat-Kunden

Der Azure-Hybridvorteil für RHEL steht Red Hat-Kunden zur Verfügung, die beide Kriterien erfüllen:

- Sie haben aktive oder nicht verwendete RHEL-Abonnements, die für die Verwendung in Azure geeignet sind.
- Mindestens ein Abonnement ist für die Verwendung in Azure mit dem [Red Hat Cloud Access](https://www.redhat.com/en/technologies/cloud-computing/cloud-access)-Programm aktiviert.

> [!IMPORTANT]
> Stellen Sie sicher, dass das richtige Abonnement für das [Cloud Access](https://www.redhat.com/en/technologies/cloud-computing/cloud-access)-Programm aktiviert wurde.

So beginnen Sie mit der Verwendung des Vorteils für Red Hat:

1. Aktivieren Sie Ihre berechtigten RHEL-Abonnements in Azure über die[Red Hat Cloud Access-Kundenschnittstelle](https://access.redhat.com/management/cloud).

   Die von Ihnen während des Red Hat Cloud Access-Aktivierungsprozesses bereitgestellten Azure-Abonnements sind dann berechtigt, den Azure-Hybridvorteil zu nutzen.
1. Wenden Sie Azure-Hybridvorteil auf alle Ihre bestehenden und neuen RHEL PAYG VM-Skalierungsgruppe an. Sie können den Vorteil über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle aktivieren.
1. Befolgen Sie die empfohlenen [weiteren Schritte](https://access.redhat.com/articles/5419341) für das Konfigurieren von Updatequellen für Ihre RHEL-VMs und für Compliancerichtlinien für RHEL-Abonnements.


### <a name="suse-customers"></a>SUSE-Kunden

So beginnen Sie mit der Verwendung des Vorteils für SUSE:

1. Registrieren Sie sich beim SUSE Public Cloud-Programm.
1. Wenden Sie den Vorteil auf Ihr neu erstelltes oder bestehendes VM-Skalierungsgruppe über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle an.
1. Registrieren Sie die virtuellen Computer, auf die der Vorteil angewendet wird, bei einer separaten Quelle für Updates.


## <a name="enable-and-disable-the-benefit-on-azure-portal"></a>Aktivieren und Deaktivieren der Leistung im Azure-Portal 
Die Portal Darstellung zum Aktivieren und Deaktivieren von AHB in der VM-Skalierungsgruppe ist **zurzeit nicht verfügbar**.

## <a name="enable-and-disable-the-benefit-using-azure-cli"></a>Aktivieren und Deaktivieren der Leistung mit Azure CLI

Verwenden Sie den Befehl `az vmss update`, um die vorhandenen VMs zu aktualisieren. Führen Sie den Befehl bei virtuellen RHEL-Computern mit einem `--license-type`-Parameter mit dem Wert `RHEL_BYOS` aus. Führen Sie den Befehl bei virtuellen SLES-Computern mit einem `--license-type`-Parameter mit dem Wert `SLES_BYOS` aus.

### <a name="cli-example-to-enable-the-benefit"></a>Beispiel für die Aktivierung des Vorteils über die Befehlszeilenschnittstelle
```azurecli
# This will enable the benefit on a RHEL VMSS
az vmss update --resource-group myResourceGroup --name myVmName --license-type RHEL_BYOS

# This will enable the benefit on a SLES VMSS
az vmss update --resource-group myResourceGroup --name myVmName --license-type SLES_BYOS
```
### <a name="cli-example-to-disable-the-benefit"></a>Beispiel für die Deaktivierung des Vorteils über die Befehlszeilenschnittstelle
Verwenden Sie zum Deaktivieren des Vorteils den `--license-type`-Wert `None`:

```azurecli
# This will disable the benefit on a VM
az vmss update -g myResourceGroup -n myVmName --license-type None
```

>[!NOTE]
> Für Skalierungsgruppen gilt eine [„Upgraderichtlinie“](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model), die festlegt, wie virtuelle Computer auf das aktuelle Skalierungsgruppenmodell aktualisiert werden. Wenn Ihre VMSS die Upgrade-Richtlinie „Automatisch“ haben, wird der AHB-Vorteil automatisch angewendet, wenn VM-Instanzen aktualisiert werden. Wenn VMSS eine Richtlinie zum „parallelen Upgrade“ haben, wird AHB basierend auf den geplanten Updates angewendet.
Bei der Upgrade-Richtlinie „Manuel“ müssen Sie für jede vorhandene VM ein „manuelles Upgrade“ durchführen.  

### <a name="cli-example-to-upgrade-virtual-machine-scale-set-instances-in-case-of-manual-upgrade-policy"></a>CLI-Beispiel für das Upgrade der VM-Skalierungsgruppe bei der Richtlinie „Manuelles Upgrade“ 
```azurecli
# This will bring VMSS instances up to date with latest VMSS model 
az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
```

## <a name="apply-the-azure-hybrid-benefit-at-virtual-machine-scale-set-create-time"></a>Azure-Hybridvorteil bei Erstellung der VM-Skalierungsgruppe anwenden 
Sie können den Azure-Hybridvorteil nicht nur auf eine bestehende nutzungsbasierte Bezahlung für die VM-Skalierungsgruppe anwenden, sondern ihn auch zum Zeitpunkt der Erstellung des Scale-Sets für die VM-Skalierungsgruppe aufrufen. Dies bietet einen dreifachen Vorteil:
- Sie können sowohl PAYG- als auch BYOS-VM-Skalierungsgruppe-Instanzen bereitstellen, indem Sie das gleiche Bild und den gleichen Prozess verwenden.
- Es ermöglicht zukünftige Änderungen des Lizenzierungsmodus, was bei einem reinen BYOS-Image nicht möglich ist.
- Die Instanzen der VM-Skalierungsgruppe werden standardmäßig mit der Red Hat Update Infrastructure (RHUI) verbunden, um sicherzustellen, dass sie auf dem neuesten Stand und sicher bleiben. Sie können den aktualisierten Mechanismus nach der Bereitstellung jederzeit ändern.

### <a name="cli-example-to-create-virtual-machine-scale-set-with-ahb-benefit"></a>CLI-Beispiel zum Erstellen einer VM-Skalierungsgruppe mit AHB-Vorteil
```azurecli
# This will enable the benefit while creating RHEL VMSS
az vmss create --name myVmName --resource-group myResourceGroup --vnet-name myVnet --subnet mySubnet  --image myRedHatImageURN --admin-username myAdminUserName --admin-password myPassword --instance-count myInstanceCount --license-type RHEL_BYOS 

# This will enable the benefit while creating RHEL VMSS
az vmss create --name myVmName --resource-group myResourceGroup --vnet-name myVnet --subnet mySubnet  --image myRedHatImageURN --admin-username myAdminUserName --admin-password myPassword --instance-count myInstanceCount --license-type SLES_BYOS
```

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie, wie Sie mit der Azure CLI VMs erstellen und aktualisieren und Lizenztypen (RHEL_BYOS, SLES_BYOS) für den Azure-Hybridvorteil hinzufügen.](/cli/azure/vmss)
