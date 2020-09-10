---
title: Konfigurieren eines privaten Endpunkts (Vorschau)
titleSuffix: Azure Machine Learning
description: Verwenden Sie Azure Private Link, um sicher von einem virtuellen Netzwerk aus auf Ihren Azure Machine Learning-Arbeitsbereich zuzugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/28/2020
ms.openlocfilehash: 9ce139131e2c6cbfd73f9160b986d9886ae4844b
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181951"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace-preview"></a>Konfigurieren von Azure Private Link für einen Azure Machine Learning-Arbeitsbereich (Vorschauversion)

In diesem Dokument erfahren Sie, wie Sie Azure Private Link mit Ihrem Azure Machine Learning-Arbeitsbereich verwenden. 

> [!IMPORTANT]
> Die Verwendung von Azure Private Link mit dem Azure Machine Learning-Arbeitsbereich befindet sich derzeit in der öffentlichen Vorschau. Diese Funktionalität ist nur in den Regionen **USA, Osten**, **USA, Süden-Mitte** und **USA, Westen 2** verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Private Link ermöglicht Ihnen das Herstellen einer Verbindung mit Ihrem Arbeitsbereich über einen privaten Endpunkt. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in Ihrem virtuellen Netzwerk. Sie können dann den Zugriff auf Ihren Arbeitsbereich so einschränken, dass er nur über die privaten IP-Adressen erfolgt. Private Link hilft dabei, das Risiko einer Datenexfiltration zu verringern. Weitere Informationen zu privaten Endpunkten finden Sie im Artikel zu [Azure Private Link](/azure/private-link/private-link-overview).

> [!IMPORTANT]
> Azure Private Link hat keine Auswirkungen auf die Azure-Steuerungsebene (Verwaltungsvorgänge) aus, etwa das Löschen des Arbeitsbereichs oder das Verwalten von Computeressourcen. Beispiele: Erstellen, Aktualisieren oder Löschen eines Computeziels. Diese Vorgänge werden ganz normal über das öffentliche Internet ausgeführt.
>
> Wenn Sie Mozilla Firefox verwenden, treten möglicherweise Probleme beim Zugriff auf den privaten Endpunkt für Ihren Arbeitsbereich auf. Dieses Problem kann im Zusammenhang mit DNS über HTTPS in Mozilla auftreten. Die Verwendung von Microsoft Edge von Google Chrome wird als Problemumgehung empfohlen.

> [!TIP]
> Die Azure Machine Learning-Computeinstanz kann mit einem Arbeitsbereich und einem privaten Endpunkt verwendet werden. Diese Funktion befindet sich derzeit in den Regionen **USA, Osten**, **USA, Süden-Mitte** und **USA, Westen 2** in der öffentlichen Vorschau.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Erstellen eines Arbeitsbereichs, der einen privaten Endpunkt verwendet

> [!IMPORTANT]
> Derzeit wird das Aktivieren eines privaten Endpunkts nur beim Erstellen eines neuen Azure Machine Learning-Arbeitsbereichs unterstützt.

Die Vorlage unter [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) kann zur Erstellung eines Arbeitsbereichs mit einem privaten Endpunkt verwendet werden.

Informationen zur Verwendung dieser Vorlage, einschließlich privater Endpunkte, finden Sie unter [Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Arbeitsbereichs für Azure Machine Learning](how-to-create-workspace-template.md).

## <a name="using-a-workspace-over-a-private-endpoint"></a>Verwenden eines Arbeitsbereichs über einen privaten Endpunkt

Da die Kommunikation mit dem Arbeitsbereich nur aus dem virtuellen Netzwerk zulässig ist, müssen alle Entwicklungsumgebungen, in denen der Arbeitsbereich verwendet wird, Mitglieder des virtuellen Netzwerks sein. Beispiel: ein virtueller Computer im virtuellen Netzwerk.

> [!IMPORTANT]
> Um eine vorübergehende Unterbrechung der Konnektivität zu vermeiden, empfiehlt Microsoft, den DNS-Cache auf Computern, die eine Verbindung mit dem Arbeitsbereich herstellen, nach dem Aktivieren von Private Link zu leeren. 

Weitere Informationen zu Azure Virtual Machines finden Sie in der [Dokumentation zu Virtual Machines](/azure/virtual-machines/).


## <a name="using-azure-storage"></a>Verwenden von Azure Storage

Um das von Ihrem Arbeitsbereich verwendete Azure Storage-Konto abzusichern, fügen Sie es in das virtuelle Netzwerk ein.

Informationen zum Platzieren des Speicherkontos im virtuellen Netzwerk finden Sie unter [Verwenden eines Speicherkontos für Ihren Arbeitsbereich](how-to-enable-virtual-network.md#use-a-storage-account-for-your-workspace).

> [!WARNING]
> Azure Machine Learning unterstützt die Verwendung des Azure Storage-Kontos nicht, wenn dafür Private Link aktiviert ist.

## <a name="using-azure-key-vault"></a>Verwenden von Azure Key Vault

Um die vom Arbeitsbereich verwendeten Azure Key Vault-Instanz zu schützen, können Sie diese entweder im virtuellen Netzwerk platzieren oder Private Link für sie aktivieren.

Informationen zum Platzieren der Key Vault-Instanz im virtuellen Netzwerk finden Sie unter [Verwenden einer Key Vault-Instanz mit Ihrem Arbeitsbereich](how-to-enable-virtual-network.md#key-vault-instance).

Informationen zum Aktivieren von Private Link für Key Vault finden Sie unter [Integrieren von Key Vault in Azure Private Link](/azure/key-vault/private-link-service).

## <a name="using-azure-kubernetes-services"></a>Verwenden von Azure Kubernetes Service

Um die von Ihrem Arbeitsbereich verwendete Azure Kubernetes Service-Instanz zu schützen, fügen Sie sie in das virtuelle Netzwerk ein. Weitere Informationen finden Sie unter [Verwenden von Azure Kubernetes Service mit Ihrem Arbeitsbereich](how-to-enable-virtual-network.md#aksvnet).

Azure Machine Learning unterstützt jetzt die Verwendung von Azure Kubernetes Service mit aktiviertem Private Link-Dienst.
Informationen zum Erstellen eines privaten AKS-Clusters finden Sie [hier](https://docs.microsoft.com/azure/aks/private-clusters)

## <a name="azure-container-registry"></a>Azure Container Registry

Informationen zum Schützen von Azure Container Registry im virtuellen Netzwerk finden Sie unter [Verwenden von Azure Container Registry](how-to-enable-virtual-network.md#azure-container-registry).

> [!IMPORTANT]
> Wenn Sie Private Link für Ihren Azure Machine Learning-Arbeitsbereich verwenden und die Azure Container Registry-Instanz für Ihren Arbeitsbereich in einem virtuellen Netzwerk platzieren, müssen Sie außerdem die folgende Azure Resource Manager-Vorlage anwenden. Durch diese Vorlage kann Ihr Arbeitsbereich über Private Link mit ACR kommunizieren.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "keyVaultArmId": {
      "type": "string"
      },
      "workspaceName": {
      "type": "string"
      },
      "containerRegistryArmId": {
      "type": "string"
      },
      "applicationInsightsArmId": {
      "type": "string"
      },
      "storageAccountArmId": {
      "type": "string"
      },
      "location": {
      "type": "string"
      }
  },
  "resources": [
      {
      "type": "Microsoft.MachineLearningServices/workspaces",
      "apiVersion": "2019-11-01",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "identity": {
          "type": "SystemAssigned"
      },
      "sku": {
          "tier": "enterprise",
          "name": "enterprise"
      },
      "properties": {
          "sharedPrivateLinkResources":
  [{"Name":"Acr","Properties":{"PrivateLinkResourceId":"[concat(parameters('containerRegistryArmId'), '/privateLinkResources/registry')]","GroupId":"registry","RequestMessage":"Approve","Status":"Pending"}}],
          "keyVault": "[parameters('keyVaultArmId')]",
          "containerRegistry": "[parameters('containerRegistryArmId')]",
          "applicationInsights": "[parameters('applicationInsightsArmId')]",
          "storageAccount": "[parameters('storageAccountArmId')]"
      }
      }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Schützen Ihres Azure Machine Learning-Arbeitsbereichs finden Sie im Artikel [Sicherheit im Unternehmen](concept-enterprise-security.md).