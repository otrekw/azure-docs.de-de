---
title: Deaktivieren und erneutes Aktivieren des Application Gateway Ingress Controller-Add-Ons für Azure Kubernetes Service-Cluster
description: Dieser Artikel enthält Informationen zum Deaktivieren und erneuten Aktivieren des AGIC-Add-Ons für Ihren AKS-Cluster.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: fe4da0435731c536a723cb2cb43428166456360b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84807952"
---
# <a name="disable-and-re-enable-agic-add-on-for-your-aks-cluster"></a>Deaktivieren und erneutes Aktivieren des AGIC-Add-Ons für Ihren AKS-Cluster
Der als AKS-Add-On bereitgestellte Application Gateway Ingress Controller (AGIC) ermöglicht Ihnen, das Add-On mit einer Zeile in Azure CLI zu aktivieren und zu deaktivieren. Der Lebenszyklus des Application Gateway ist unterschiedlich, wenn Sie das AGIC-Add-On deaktivieren, je nachdem, ob das Application Gateway vom AGIC-Add-On erstellt wurde, oder ob es separat vom AGIC-Add-On bereitgestellt wurde. Sie können denselben Befehl ausführen, um das AGIC-Add-On erneut zu aktivieren, wenn Sie es jemals deaktivieren sollten, oder um das AGIC-Add-On mithilfe eines vorhandenen AKS-Clusters und Application Gateway zu aktivieren.

## <a name="disabling-agic-add-on-with-associated-application-gateway"></a>Deaktivieren des AGIC-Add-Ons mit zugeordnetem Application Gateway 
Wenn das AGIC-Add-On das Application Gateway bei der ersten Einrichtung automatisch für Sie bereitgestellt hat, wird das Application Gateway durch das Deaktivieren des AGIC-Add-Ons aufgrund von zwei Kriterien standardmäßig gelöscht. Es gibt zwei Kriterien, nach denen das AGIC-Add-On sucht, wenn Sie es deaktivieren, um zu bestimmen, ob das zugeordnete Application Gateway gelöscht werden soll:
- Wird das Application Gateway, das dem AGIC-Add-On zugeordnet ist, in der MC_*-Knotenressourcengruppe bereitgestellt? 
- Hat das Application Gateway, dem das AGIC-Add-On zugeordnet ist, das Tag „created-by: ingress-appgw“? Mit dem Tag bestimmt AGIC, ob das Application Gateway vom Add-On bereitgestellt wurde. 

Wenn beide Kriterien erfüllt sind, löscht das AGIC-Add-On das Application Gateway, das es erstellt hat, beim Deaktivieren des Add-Ons. Die öffentliche IP-Adresse oder das Subnetz, mit der/in dem das Application Gateway bereitgestellt wurde, wird jedoch nicht gelöscht. Wenn das erste Kriterium nicht erfüllt ist, ist es unerheblich, ob das Application Gateway das Tag „created-by: ingress-appgw“ aufweist – die Deaktivierung des Add-Ons löscht das Application Gateway nicht. Wenn das zweite Kriterium nicht erfüllt ist, d. h. das Application Gateway nicht über dieses Tag verfügt, löscht das Deaktivieren des Add-Ons wiederum nicht das Application Gateway in der MC_*-Knotenressourcengruppe. 

> [!TIP] 
> Wenn das Application Gateway beim Deaktivieren des Add-Ons nicht gelöscht werden soll, aber beide Kriterien erfüllt sind, entfernen Sie das Tag „created-by: ingress-appgw“, um zu verhindern, dass das Add-On Ihr Application Gateway löscht. 

Führen Sie den folgenden Befehl aus, um das AGIC-Add-On zu deaktivieren: 
```azurecli-interactive
az aks disable-addons -n <AKS-cluster-name> -g <AKS-resource-group-name> -a ingress-appgw 
```

## <a name="enable-agic-add-on-on-existing-application-gateway-and-aks-cluster"></a>Aktivieren des AGIC-Add-Ons auf einem vorhandenen Application Gateway und AKS-Cluster
Wenn Sie das AGIC-Add-On jemals deaktivieren sollten und es erneut aktivieren möchten oder das Add-On mithilfe eines vorhandenen Application Gateways und AKS-Clusters aktivieren möchten, führen Sie den folgenden Befehl aus:

```azurecli-interactive
appgwId=$(az network application-gateway show -n <application-gateway-name> -g <resource-group-name> -o tsv --query "id") 
az aks enable-addons -n <AKS-cluster-name> -g <AKS-cluster-resource-group> -a ingress-appgw --appgw-id $appgwId
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Aktivieren des AGIC-Add-Ons mithilfe eines vorhandenen Application Gateways und AKS-Clusters finden Sie unter [Tutorial: Aktivieren des Application Gateway-Eingangscontroller-Add-Ons für einen vorhandenen AKS-Cluster mit einem vorhandenen Application Gateway über die Azure CLI (Vorschau)](tutorial-ingress-controller-add-on-existing.md).