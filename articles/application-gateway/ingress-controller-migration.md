---
title: Migrieren von Azure Application Gateway-Eingangsdatencontroller Helm zum AGIC-Add-On
description: Dieser Artikel enthält Anweisungen zum Migrieren von über Helm bereitgestelltem AGIC zu als AKS-Add-On bereitgestelltem AGIC.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/02/2021
ms.author: caya
ms.openlocfilehash: 4b5cb9abe53cdb29cbc1b4c9711fb8efcf0348d6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743348"
---
# <a name="migrate-from-agic-helm-to-agic-add-on"></a>Migrieren von AGIC Helm zum AGIC-Add-On 

Wenn Sie AGIC bereits über Helm bereitgestellt haben, AGIC aber zu als AKS-Add-On bereitgestelltem AGIC migrieren möchten, helfen Ihnen die folgenden Schritte beim Durchführen des Migrationsprozesses. 

## <a name="prerequisites"></a>Voraussetzungen 
Bevor Sie mit dem Migrationsprozess beginnen, müssen Sie einige Dinge überprüfen. 
  - Verwenden Sie Funktionen mit AGIC Helm, die [derzeit nicht mit dem AGIC-Add-On unterstützt](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on) werden?
  - Verwenden Sie mehr als eine AGIC Helm-Bereitstellung pro AKS-Cluster? 
  - Verwenden Sie mehrere AGIC Helm-Bereitstellungen mit einem Application Gateway als Ziel? 

Wenn Sie eine der oben genannten Fragen mit „Ja“ beantwortet haben, unterstützt das AGIC-Add-On Ihren Anwendungsfall noch nicht, weshalb es am besten ist, in der Zwischenzeit weiterhin AGIC Helm zu verwenden. Fahren Sie andernfalls mit dem unten stehenden Migrationsprozess außerhalb der Geschäftszeiten fort. 

## <a name="find-the-application-gateway-resource-id-that-agic-helm-is-currently-targeting"></a>Suchen der Application Gateway-Ressourcen-ID, die aktuell das Ziel für AGIC Helm ist 
Navigieren Sie zu dem Application Gateway, auf das Ihre AGIC Helm-Bereitstellung abzielt. Kopieren und speichern Sie die Ressourcen-ID dieses Application Gateways. Sie benötigen die Ressourcen-ID in einem späteren Schritt. Die Ressourcen-ID finden Sie im Portal auf der Registerkarte „Eigenschaften“ Ihres Application Gateways oder über die Azure CLI. Im folgenden Beispiel wird die Ressourcen-ID des Application Gateways in *appgwId* für ein Gateway namens *myApplicationGateway* in der Ressourcengruppe *myResourceGroup* gespeichert.

```azurecli-interactive
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id") 
```

## <a name="delete-agic-helm-from-your-aks-cluster"></a>Löschen von AGIC Helm aus Ihrem AKS-Cluster
Löschen Sie über die Azure CLI Ihre AGIC Helm-Bereitstellung aus Ihrem Cluster. Sie müssen zuerst die AGIC Helm-Bereitstellung löschen, bevor Sie das AGIC AKS-Add-On aktivieren können. Beachten Sie, dass alle Änderungen, die in Ihrem AKS-Cluster zwischen dem Zeitpunkt der Löschung Ihrer AGIC Helm-Bereitstellung und dem Zeitpunkt der Aktivierung des AGIC-Add-Ons erfolgen, nicht in Ihrem Application Gateway reflektiert werden. Daher sollte dieser Migrationsprozess außerhalb der Geschäftszeiten durchgeführt werden, um seine Auswirkungen zu minimieren. Application Gateway fährt mit der letzten von AGIC angewendeten Konfiguration fort, sodass vorhandene Routingregeln davon nicht betroffen sind. 

## <a name="enable-agic-add-on-using-your-existing-application-gateway"></a>Aktivieren des AGIC-Add-Ons mithilfe Ihres vorhandenen Application Gateways 
Sie können das AGIC-Add-On jetzt in Ihrem AKS-Cluster aktivieren mit Ihrem vorhandenen Application Gateway als Ziel über Azure CLI oder das Portal aktivieren. Führen Sie den folgenden Azure CLI-Befehl aus, um das AGIC-Add-On in Ihrem AKS-Cluster zu aktivieren. Im Beispiel wird das Add-On in einem Cluster namens *myCluster* in einer Ressourcengruppe namens *myResourceGroup* mithilfe der Application Gateway-Ressourcen-ID *appgwId* aktiviert, die wir oben in einem vorherigen Schritt gespeichert haben. 


```azurecli-interactive
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

Alternativ können Sie über diesen [Link](https://portal.azure.com/?feature.aksagic=true) im Portal zu Ihrem AKS-Cluster navigieren und das AGIC-Add-On auf der Registerkarte „Netzwerk“ Ihres Clusters aktivieren. Wählen Sie im Dropdownmenü Ihr vorhandenes Application Gateway aus, wenn Sie das Application Gateway-Ziel für das Add-On auswählen. 

![Application Gateway-Eingangsdatencontroller-Portal](./media/tutorial-ingress-controller-add-on-existing/portal_ingress_controller_addon.png)

## <a name="next-steps"></a>Nächste Schritte
- [**Problembehandlung des Application Gateway-Eingangsdatencontrollers**](ingress-controller-troubleshoot.md): Handbuch zur Problembehandlung für AGIC 
- [**Anmerkungen zum Application Gateway-Eingangsdatencontroller**](ingress-controller-annotations.md): Liste der Anmerkungen zu AGIC 