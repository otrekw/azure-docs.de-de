---
title: Aktualisieren oder Löschen einer vorhandenen Azure Load Balancer-Instanz, die von einer VM-Skalierungsgruppe verwendet wird
titleSuffix: Update or delete existing Azure Load Balancer used by Virtual Machine Scale Set
description: In diesem Anleitungsartikel beginnen Sie mit Azure Load Balancer Standard und VM-Skalierungsgruppen.
services: load-balancer
documentationcenter: na
author: irenehua
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/30/2020
ms.author: irenehua
ms.openlocfilehash: f8f664375e53a1cef28b0c7b95207770434f67fa
ms.sourcegitcommit: 6d6030de2d776f3d5fb89f68aaead148c05837e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97893238"
---
# <a name="how-to-updatedelete-azure-load-balancer-used-by-virtual-machine-scale-sets"></a>Aktualisieren oder Löschen von Azure Load Balancer, der von einer VM-Skalierungsgruppe verwendet wird

## <a name="how-to-set-up-azure-load-balancer-for-scaling-out-virtual-machine-scale-sets"></a>Einrichten von Azure Load Balancer zum Aufskalieren von VM-Skalierungsgruppen
  * Stellen Sie sicher, dass für den Load Balancer ein [NAT-Pool für eingehenden Datenverkehr](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest) eingerichtet wurde und dass die VM-Skalierungsgruppe im Back-End-Pool des Load Balancer platziert ist. Azure Load Balancer erstellt automatisch neue NAT-Regeln für eingehenden Datenverkehr im NAT-Pool für eingehenden Datenverkehr, wenn der VM-Skalierungsgruppe neue VM-Instanzen hinzugefügt werden. 
  * So überprüfen Sie, ob ein eingehender NAT-Pool ordnungsgemäß eingerichtet wurde 
  1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
  
  1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
  
  1. Wählen Sie unter **Einstellungen** die Option **NAT-Regel für eingehenden Datenverkehr** aus.
Wenn im rechten Bereich eine Liste der Regeln angezeigt wird, die für jede einzelne Instanz in der VM-Skalierungsgruppe erstellt wurden, können Sie jetzt jederzeit eine Aufskalierung vornehmen.

## <a name="how-to-add-inbound-nat-rules"></a>Hinzufügen von NAT-Regeln für eingehenden Datenverkehr 
  * Es kann keine einzelne NAT-Regel für eingehenden Datenverkehr hinzugefügt werden. Sie können jedoch einen Satz von NAT-Regeln für eingehenden Datenverkehr mit definiertem Front-End-Portbereich und Back-End-Port für alle Instanzen in der VM-Skalierungsgruppe hinzufügen.
  * Zum Hinzufügen eines ganzen Satzes von NAT-Regeln für eingehenden Datenverkehr für die VM-Skalierungsgruppen müssen Sie zunächst einen NAT-Pool für eingehenden Datenverkehr im Load Balancer erstellen und dann aus dem Netzwerkprofil der VM-Skalierungsgruppe auf den NAT-Pool für eingehenden Datenverkehr verweisen. Ein vollständiges Beispiel unter Verwendung der CLI ist nachfolgend dargestellt.
  * Der neue NAT-Pool für eingehenden Datenverkehr darf keinen überlappenden Front-End-Portbereich mit vorhandenen NAT-Pools für eingehenden Datenverkehr aufweisen. Wenn Sie vorhandene eingerichtete NAT-Pools für eingehenden Datenverkehr anzeigen möchten, können Sie diesen [CLI-Befehl](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-pool?view=azure-cli-latest#az_network_lb_inbound_nat_pool_list) verwenden.
```azurecli-interactive
az network lb inbound-nat-pool create 
        -g MyResourceGroup 
        --lb-name MyLb
        -n MyNatPool 
        --protocol Tcp 
        --frontend-port-range-start 80 
        --frontend-port-range-end 89 
        --backend-port 80 
        --frontend-ip-name MyFrontendIp
az vmss update 
        -g MyResourceGroup 
        -n myVMSS 
        --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools "{'id':'/subscriptions/mySubscriptionId/resourceGroups/MyResourceGroup/providers/Microsoft.Network/loadBalancers/MyLb/inboundNatPools/MyNatPool'}"
        
az vmss update-instances
        -–instance-ids *
        --resource-group MyResourceGroup
        --name MyVMSS
```
## <a name="how-to-update-inbound-nat-rules"></a>Aktualisieren von NAT-Regeln für eingehenden Datenverkehr 
  * Eine einzelne NAT-Regel für eingehenden Datenverkehr kann nicht aktualisiert werden. Sie können jedoch einen Satz von NAT-Regeln für eingehenden Datenverkehr mit definiertem Front-End-Portbereich und Back-End-Port für alle Instanzen in der VM-Skalierungsgruppe aktualisieren.
  * Um einen vollständigen Satz von NAT-Regeln für eingehenden Datenverkehr für die VM-Skalierungsgruppen zu aktualisieren, müssen Sie den NAT-Pool für eingehenden Datenverkehr im Load Balancer aktualisieren. 
```azurecli-interactive
az network lb inbound-nat-pool update 
        -g MyResourceGroup 
        --lb-name MyLb 
        -n MyNatPool
        --protocol Tcp 
        --backend-port 8080
```

## <a name="how-to-delete-inbound-nat-rules"></a>Löschen von NAT-Regeln für eingehenden Datenverkehr 
* Eine einzelne NAT-Regel für eingehenden Datenverkehr kann nicht gelöscht werden. Allerdings können Sie den gesamten Satz von NAT-Regeln für eingehenden Datenverkehr löschen.
* Um einen vollständigen Satz von NAT-Regeln für eingehenden Datenverkehr, die von der VM-Skalierungsgruppe verwendet werden, zu löschen, müssen Sie zunächst den NAT-Pool aus der Skalierungsgruppe entfernen. Ein vollständiges Beispiel unter Verwendung der CLI ist nachfolgend dargestellt:
```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
   az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
  az network lb inbound-nat-pool delete
     --resource-group MyResourceGroup
     -–lb-name MyLoadBalancer
     --name MyNatPool
```

## <a name="how-to-add-multiple-ip-configurations"></a>Hinzufügen mehrerer IP-Konfigurationen:
1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Klicken Sie unter **Einstellungen** auf **Front-End-IP-Konfigurationen**, und wählen Sie dann **Hinzufügen** aus.
   
1. Geben Sie auf der Seite **Front-End-IP-Adresse hinzufügen** die Werte ein, und wählen Sie **OK** aus.

1. Führen Sie [Schritt 5](https://docs.microsoft.com/azure/load-balancer/load-balancer-multiple-ip#step-5-configure-the-health-probe) und [Schritt 6](https://docs.microsoft.com/azure/load-balancer/load-balancer-multiple-ip#step-5-configure-the-health-probe) in diesem Tutorial aus, wenn neue Load Balancer-Regeln erforderlich sind.

1. Erstellen Sie bei Bedarf einen neuen Satz von NAT-Regeln für eingehenden Datenverkehr unter Verwendung der neu erstellten Front-End-IP-Konfigurationen. Ein Beispiel finden Sie hier im [vorherigen Abschnitt].

## <a name="how-to-delete-frontend-ip-configuration-used-by-virtual-machine-scale-set"></a>Löschen der von der VM-Skalierungsgruppe verwendeten Front-End-IP-Konfiguration: 
 1. Zum Löschen der Front-End-IP-Konfiguration, die von der Skalierungsgruppe verwendet wird, müssen Sie zuerst den NAT-Pool für eingehenden Datenverkehr (Satz von NAT-Regeln für eingehenden Datenverkehr) löschen. Anweisungen zum Löschen der Regeln für eingehenden Datenverkehr finden Sie im vorherigen Abschnitt.
 1. Löschen Sie die Load Balancer-Regel, die auf die Front-End-IP-Konfiguration verweist. 
 1. Löschen Sie die Front-End-IP-Konfiguration.
 

## <a name="how-to-delete-azure-load-balancer-used-by-virtual-machine-scale-set"></a>Löschen von Azure Load Balancer, der von einer VM-Skalierungsgruppe verwendet wird: 
 1. Zum Löschen der Front-End-IP-Konfiguration, die von der Skalierungsgruppe verwendet wird, müssen Sie zuerst den NAT-Pool für eingehenden Datenverkehr (Satz von NAT-Regeln für eingehenden Datenverkehr) löschen. Anweisungen zum Löschen der Regeln für eingehenden Datenverkehr finden Sie im vorherigen Abschnitt.
 
 1. Löschen Sie die Load Balancer-Regel, die auf den Back-End-Pool mit der VM-Skalierungsgruppe verweist.
 
 1. Entfernen Sie den Verweis „loadBalancerBackendAddressPool“ aus dem Netzwerkprofil der VM-Skalierungsgruppe. Ein vollständiges Beispiel unter Verwendung der CLI ist nachfolgend dargestellt:
 ```azurecli-interactive
  az vmss update
     --resource-group MyResourceGroup
     --name MyVMSS
     --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools
  az vmss update-instances 
     --instance-ids "*" 
     --resource-group MyResourceGroup
     --name MyVMSS
```
Löschen Sie zum Schluss die Load Balancer-Ressource.
 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Load Balancer und VM-Skalierungsgruppen finden Sie in den Artikeln zu den Konzepten.

> [Azure Load Balancer mit Azure-VM-Skalierungsgruppen](load-balancer-standard-virtual-machine-scale-sets.md)
