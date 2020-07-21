---
title: Verwenden verwalteter Identitäten in Azure Kubernetes Service
description: Erfahren Sie, wie Sie verwaltete Identitäten in Azure Kubernetes Service (AKS) verwenden.
services: container-service
author: mlearned
ms.topic: article
ms.date: 06/30/2020
ms.author: mlearned
ms.openlocfilehash: 30d1290f9eb7b2750f09e5e256d4dd212c7e4607
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610284"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Verwenden verwalteter Identitäten in Azure Kubernetes Service

Derzeit erfordert ein AKS-Cluster (Azure Kubernetes Service) und insbesondere der Kubernetes-Cloudanbieter zum Erstellen zusätzlicher Ressourcen in Azure Container Service, wie Lastenausgleichsmodule und verwaltete Datenträger, eine Identität. Dabei kann es sich um eine *verwaltete Identität* oder einen *Dienstprinzipal* handeln. Wenn Sie einen [Dienstprinzipal](kubernetes-service-principal.md) verwenden, müssen Sie diesen bereitstellen oder in Ihrem Namen durch AKS erstellen lassen. Wenn Sie eine verwaltete Identität verwenden, wird diese automatisch von AKS erstellt. Cluster mit Dienstprinzipalen erreichen irgendwann einen Zustand, in dem der Dienstprinzipal erneuert werden muss, damit der Cluster ordnungsgemäß funktioniert. Das Verwalten von Dienstprinzipalen erhöht die Komplexität, daher ist es einfacher, stattdessen verwaltete Identitäten zu verwenden. Dieselben Berechtigungsanforderungen gelten sowohl für Dienstprinzipale als auch für verwaltete Identitäten.

*Verwaltete Identitäten* stellen im Wesentlichen einen Wrapper um Dienstprinzipale dar und vereinfachen deren Verwaltung. Die Rotation der Anmeldeinformationen für verwaltete Identitäten erfolgt in Azure Active Directory standardmäßig automatisch alle 46 Tage. AKS unterstützt sowohl systemseitig zugewiesene als auch benutzerseitig zugewiesene Typen verwalteter Identitäten. Diese Identitäten sind derzeit unveränderlich. Weitere Informationen finden Sie im Artikel über [verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="before-you-begin"></a>Voraussetzungen

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI ab Version 2.2.0

## <a name="limitations"></a>Einschränkungen

* Die Verwendung eigener verwalteter Identitäten wird derzeit nicht unterstützt.
* AKS-Cluster mit verwalteten Identitäten können nur während der Erstellung des Clusters aktiviert werden.
* Es ist nicht möglich, AKS-Cluster zu aktualisieren oder ein Upgrade für sie durchzuführen, um verwaltete Identitäten zu aktivieren.
* Während eines **Upgradevorgangs** des Clusters ist die verwaltete Identität vorübergehend nicht verfügbar.

## <a name="summary-of-managed-identities"></a>Zusammenfassung der verwalteten Identitäten

AKS verwendet mehrere verwaltete Identitäten für integrierte Dienste und Add-Ons.

| Identity                       | Name    | Anwendungsfall | Standardberechtigungen | Verwendung eigener Identitäten
|----------------------------|-----------|----------|
| Steuerungsebene | Nicht sichtbar | Verwaltung von Netzwerkressourcen, etwa zum Erstellen eines Load Balancers für eingehenden Datenverkehr, öffentliche IP-Adressen usw.| Rolle „Mitwirkender“ für Knotenressourcengruppe | Derzeit nicht unterstützt
| Kubelet | Name des AKS-Clusters: agentpool | Authentifizierung bei Azure Container Registry (ACR) | Rolle „Leser“ für Knotenressourcengruppe | Derzeit nicht unterstützt
| Add-On | AzureNPM | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | Azure CNI-Netzwerküberwachung | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | azurepolicy (Gatekeeper) | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | azurepolicy | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | Calico | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | Dashboard | Keine Identität erforderlich | Nicht verfügbar | Nein
| Add-On | HTTPApplicationRouting | Verwaltung erforderlicher Netzwerkressourcen | Rolle „Leser“ für Knotenressourcengruppe, Rolle „Mitwirkender“ für DNS-Zone | Nein
| Add-On | Anwendungsgateway für eingehenden Datenverkehr | Verwaltung erforderlicher Netzwerkressourcen| Rolle „Mitwirkender“ für Knotenressourcengruppe | Nein
| Add-On | omsagent | Senden von AKS-Metriken an Azure Monitor | Rolle „Herausgeber von Überwachungsmetriken“ | Nein
| Add-On | Virtual-Node (ACI-Connector) | Verwaltung erforderlicher Netzwerkressourcen für Azure Container Instances (ACI) | Rolle „Mitwirkender“ für Knotenressourcengruppe | Nein


## <a name="create-an-aks-cluster-with-managed-identities"></a>Erstellen eines AKS-Clusters mit verwalteten Identitäten

Sie können jetzt einen AKS-Cluster mit verwalteten Identitäten erstellen, indem Sie die folgenden CLI-Befehle verwenden.

Erstellen Sie als Erstes eine Azure-Ressourcengruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Erstellen Sie anschließend einen AKS-Cluster:

```azurecli-interactive
az aks create -g myResourceGroup -n myManagedCluster --enable-managed-identity
```

Bei einer erfolgreichen Clustererstellung mit verwalteten Identitäten sind folgende Dienstprinzipal-Profilinformationen enthalten:

```json
"servicePrincipalProfile": {
    "clientId": "msi"
  }
```

Rufen Sie mit dem folgenden Befehl die Objekt-ID der verwalteten Identität Ihrer Steuerungsebene ab:

```azurecli-interactive
az aks show -g myResourceGroup -n MyManagedCluster --query "identity"
```

Das Ergebnis sollte wie folgt aussehen:

```json
{
  "principalId": "<object_id>",   
  "tenantId": "<tenant_id>",      
  "type": "SystemAssigned"                                 
}
```

> [!NOTE]
> Verwenden Sie zum Erstellen und Verwenden Ihres eigenen VNETs, Ihrer statischen IP-Adresse oder Ihres angeschlossenen Azure-Datenträgers, auf dem sich die Ressourcen außerhalb der Ressourcengruppe „Workerknoten“ befinden, die Prinzipal-ID der systemseitig zugewiesenen verwalteten Identität des Clusters, um eine Rolle zuzuweisen. Weitere Informationen zur Rollenzuweisung finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Es kann bis zu 60 Minuten dauern, bis die vom Azure-Cloudanbieter verwendeten Berechtigungszuweisungen für die Erstellung von Clustern für verwaltete Identitäten übernommen werden.

Rufen Sie schließlich Anmeldeinformationen für den Zugriff auf den Cluster ab:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

Der Cluster wird in wenigen Minuten erstellt. Sie können Ihre Anwendungsworkloads im neuen Cluster bereitstellen und damit interagieren, wie Sie dies mit dienstprinzipalbasierten AKS-Clustern getan haben.

## <a name="next-steps"></a>Nächste Schritte
* Erstellen Sie mithilfe von [Azure Resource Manager-Vorlagen (ARM)][aks-arm-template] Cluster mit aktivierter verwalteter Identität.

<!-- LINKS - external -->
[aks-arm-template]: https://docs.microsoft.com/azure/templates/microsoft.containerservice/managedclusters
