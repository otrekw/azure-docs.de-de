---
title: Problembehandlung von ConstrainedAllocationFailed beim Bereitstellen eines Clouddiensts in Azure | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie eine ConstrainedAllocationFailed-Ausnahme bei der Bereitstellung eines Clouddiensts in Azure auflösen.
services: cloud-services
author: mibufo
ms.author: v-mibufo
ms.service: cloud-services
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.openlocfilehash: de344bbcd89158676bacf2a8aa1743d282700b9d
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100520782"
---
# <a name="troubleshoot-constrainedallocationfailed-when-deploying-a-cloud-service-to-azure"></a>Problembehandlung von ConstrainedAllocationFailed beim Bereitstellen eines Clouddiensts in Azure

In diesem Artikel behandeln Sie Zuordnungsfehler, bei denen Azure Cloud Services aufgrund von Einschränkungen nicht bereitstellen kann.

Microsoft Azure führt Zuordnungen durch, wenn Sie folgende Aktionen durchführen:

- Aktualisieren von Cloud Services-Instanzen

- Hinzufügen neuer Web- oder Workerrolleninstanzen

- Bereitstellen von Instanzen in einem Clouddienst

Unter Umständen erhalten Sie bei diesen Vorgängen auch dann gelegentlich Fehlermeldungen, wenn Sie die Grenzwerte des Azure-Abonnements noch nicht erreicht haben.

> [!TIP]
> Diese Informationen können auch hilfreich sein, wenn Sie die Bereitstellung Ihrer Dienste planen.

## <a name="symptom"></a>Symptom

Navigieren Sie im Azure-Portal zu Ihrem Clouddienst, und wählen Sie in der Randleiste *Vorgangsprotokolle (klassisch)* aus, um die Protokolle anzuzeigen.

Wenn Sie die Protokolle Ihres Clouddiensts überprüfen, wird die folgende Ausnahme angezeigt:

|Ausnahmetyp  |Fehlermeldung  |
|---------|---------|
|ConstrainedAllocationFailed     |Fehler bei Azure-Vorgang '`{Operation ID}`‘ mit dem Code Compute.ConstrainedAllocationFailed. Details: Fehler bei der Zuordnung. Einschränkungen in der Anforderung konnten nicht erfüllt werden. Die angeforderte neue Dienstbereitstellung ist an eine Affinitätsgruppe gebunden oder auf ein virtuelles Netzwerk ausgerichtet, oder unter diesem gehosteten Dienst befindet sich eine vorhandene Bereitstellung. Alle diese Bedingungen beschränken die neue Bereitstellung auf bestimmte Azure-Ressourcen. Versuchen Sie es später noch mal, oder verringern Sie die VM-Größe oder die Anzahl von Rolleninstanzen. Sie können, wenn möglich, auch die zuvor erwähnten Einschränkungen entfernen oder den virtuellen Computer in einer anderen Region bereitstellen.|

## <a name="cause"></a>Ursache

Bei der Region oder dem Cluster, wo Sie die Bereitstellung durchführen, liegt ein Kapazitätsproblem vor. Dies tritt auf, wenn die von Ihnen ausgewählte Ressourcen-SKU für den angegebenen Speicherort nicht verfügbar ist.

> [!NOTE]
> Wenn der erste Knoten eines Clouddiensts bereitgestellt wird, wird er einem Ressourcenpool *angeheftet*. Ein Ressourcenpool kann ein einzelner Cluster oder eine Gruppe von Clustern sein.
>
> Im Laufe der Zeit können die Ressourcen in diesem Ressourcenpool vollständig ausgelastet werden. Wenn ein Clouddienst eine Zuordnungsanforderung für zusätzliche Ressourcen sendet, wenn die Ressourcen im angehefteten Ressourcenpool unzureichend sind, führt die Anforderung zu einem [Zuordnungsfehler](cloud-services-allocation-failures.md).

## <a name="solution"></a>Lösung

In diesem Szenario sollten Sie eine andere Region oder SKU auswählen, wo der Clouddienst bereitgestellt werden soll. Vor dem Bereitstellen oder Aktualisieren Ihres Clouddiensts können Sie ermitteln, welche SKUs in einer Region oder Verfügbarkeitszone verfügbar sind. Befolgen Sie die folgenden [Azure CLI](#list-skus-in-region-using-azure-cli)-, [PowerShell](#list-skus-in-region-using-powershell)- oder [REST-API](#list-skus-in-region-using-rest-api)-Prozesse.

### <a name="list-skus-in-region-using-azure-cli"></a>Auflisten von SKUs in der Region mithilfe von Azure CLI

Sie können den Befehl [az vm list-skus](https://docs.microsoft.com/cli/azure/vm.html#az_vm_list_skus) verwenden.

- Filtern Sie die Ausgabe mit dem Parameter `--location` auf den verwendeten Standort.
- Verwenden Sie den Parameter `--size`, um nach dem Teil eines Namens für die Größe zu suchen.
- Weitere Informationen finden Sie unter [Lösung 2: Azure CLI](../azure-resource-manager/templates/error-sku-not-available.md#solution-2---azure-cli).

    **Beispiel:**

    ```azurecli
    az vm list-skus --location southcentralus --size Standard_F --output table
    ```

    **Beispielergebnisse:** ![Azure CLI-Ausgabe der Ausführung des Befehls „az vm list-skus --location southcentralus --size Standard_F --output table“, der die verfügbaren SKUs anzeigt.](./media/cloud-services-troubleshoot-constrained-allocation-failed/cloud-services-troubleshoot-constrained-allocation-failed-1.png)

#### <a name="list-skus-in-region-using-powershell"></a>Auflisten von SKUs in der Region mithilfe von PowerShell

Sie können den Befehl [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku) verwenden.

- Filtern Sie die Ergebnisse nach Standort.
- Für diesen Befehl benötigen Sie die aktuelle Version von PowerShell.
- Weitere Informationen finden Sie unter [Lösung 1: PowerShell](../azure-resource-manager/templates/error-sku-not-available.md#solution-1---powershell).

**Beispiel:**

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

**Einige weitere nützliche Befehle:**

Filtern Sie die Standorte, die die Größe (Standard_DS14_v2) enthalten:

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
```

Filtern Sie alle Standorte, die die Größe (V3) enthalten:

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

#### <a name="list-skus-in-region-using-rest-api"></a>Auflisten von SKUs in der Region mithilfe von REST-API

Verwenden Sie den Vorgang [Auflisten von Ressourcen-SKUs](https://docs.microsoft.com/rest/api/compute/resourceskus/list). Verfügbare SKUs und Regionen werden im folgenden Format zurückgegeben:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    <Rest_of_your_file_is_located_here...>
  ]
}
    
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Zuordnungsfehlerlösungen und Informationen zum besseren Verständnis ihrer Generierung finden Sie hier:

> [!div class="nextstepaction"]
> [Behandeln von Zuordnungsfehlern bei der Bereitstellung von Cloud Services (klassisch) in Azure](cloud-services-allocation-failures.md)

Suchen Sie in den Azure-Foren bei [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder [auf Twitter an @AzureSupport senden](https://twitter.com/AzureSupport). Sie können auch eine Azure-Supportanfrage senden. Wenn Sie eine Supportanfrage senden möchten, wählen Sie auf der [Azure-Support-Seite](https://azure.microsoft.com/support/options/) die Option *Support erhalten* aus.
