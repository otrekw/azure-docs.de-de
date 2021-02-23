---
title: Dedizierte Azure Monitor-Protokollcluster
description: Kunden, die mehr als 1 TB an Überwachungsdaten pro Tag erfassen, können ggf. dedizierte Cluster anstelle freigegebener Cluster verwenden.
ms.subservice: logs
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 09/16/2020
ms.openlocfilehash: bcd89274b1f2ba7b31a10a481e3d73901eac2c60
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100375186"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Dedizierte Azure Monitor-Protokollcluster

Dedizierte Azure Monitor-Protokollcluster sind eine Bereitstellungsoption, die erweiterte Funktionen für Kunden von Azure Monitor-Protokollen bietet. Kunden mit dedizierten Clustern können die Arbeitsbereiche auswählen, die auf diesen Clustern gehostet werden sollen.

Folgende Funktionen erfordern dedizierte Cluster:

- **[Kundenseitig verwaltete Schlüssel](../platform/customer-managed-keys.md)** : Die Clusterdaten werden mithilfe von Schlüsseln verschlüsselt, die vom Kunden bereitgestellt und gesteuert werden.
- **[Lockbox](../platform/customer-managed-keys.md#customer-lockbox-preview)** : Kunden können Zugriffsanforderungen von Mitarbeitern des Microsoft-Supports für Daten steuern.
- **[Doppelte Verschlüsselung](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption)** schützt vor dem Szenario, dass einer der Verschlüsselungsalgorithmen oder Schlüssel kompromittiert wurde. In diesem Fall werden die Daten weiterhin durch die zusätzliche Verschlüsselungsebene geschützt.
- **[Mehrere Arbeitsbereiche](../log-query/cross-workspace-query.md)** : Wenn ein Kunde mehr als einen Arbeitsbereich für die Produktion verwendet, ist es möglicherweise sinnvoll, einen dedizierten Cluster zu verwenden. Arbeitsbereichsübergreifende Abfragen werden schneller ausgeführt, wenn sich alle Arbeitsbereiche im selben Cluster befinden. Möglicherweise ist die Verwendung eines dedizierten Clusters auch kostengünstiger, da die zugewiesenen Tarife für die Kapazitätsreservierung die gesamte Clustererfassung berücksichtigen und auf alle Arbeitsbereiche angewendet werden, auch wenn einige davon klein sind und nicht für den Kapazitätsreservierungsrabatt berechtigt sind.

Dedizierte Cluster erfordern es, dass sich Kunden zur Verwendung einer Datenerfassung von mindestens 1 TB pro Tag verpflichten. Die Migration zu einem dedizierten Cluster ist einfach. Es gibt keinen Datenverlust und keine Dienstunterbrechung. 

## <a name="management"></a>Verwaltung 

Dedizierte Cluster werden über eine Azure-Ressource verwaltet, die Azure Monitor-Protokollcluster darstellt. Alle Vorgänge werden für diese Ressource mithilfe von PowerShell oder der REST-API ausgeführt.

Nachdem der Cluster erstellt wurde, ist es möglich, ihn zu konfigurieren und Arbeitsbereiche mit ihm zu verknüpfen. Wenn ein Arbeitsbereich mit einem Cluster verknüpft ist, befinden sich neue Daten, die an den Arbeitsbereich gesendet werden, im Cluster. Mit einem Cluster können nur Arbeitsbereiche verknüpft werden, die sich in der gleichen Region wie der Cluster befinden. Das Aufheben der Verknüpfung von Arbeitsbereichen mit einem Cluster ist mit Einschränkungen möglich. Weitere Einzelheiten zu diesen Einschränkungen finden Sie in diesem Artikel. 

In dedizierten Clustern erfasste Daten werden zweimal verschlüsselt: einmal auf der Dienstebene mit von Microsoft verwalteten Schlüsseln oder [kundenseitig verwaltetem Schlüssel](../platform/customer-managed-keys.md) und einmal auf der Infrastrukturebene mit zwei verschiedenen Verschlüsselungsalgorithmen und zwei verschiedenen Schlüsseln. Die [doppelte Verschlüsselung](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) schützt vor dem Szenario, dass einer der Verschlüsselungsalgorithmen oder Schlüssel kompromittiert wurde. In diesem Fall werden die Daten weiterhin durch die zusätzliche Verschlüsselungsebene geschützt. Ein dedizierter Cluster ermöglicht Ihnen außerdem das Schützen Ihrer Daten mit [Lockbox](../platform/customer-managed-keys.md#customer-lockbox-preview).

Für alle Vorgänge auf Clusterebene ist die Aktionsberechtigung `Microsoft.OperationalInsights/clusters/write` für den Cluster erforderlich. Diese Berechtigung kann über den Besitzer oder einen Mitwirkenden erteilt werden, der die Aktion `*/write` enthält, oder über die Rolle „Analytics-Mitwirkender“, die die Aktion `Microsoft.OperationalInsights/*` enthält. Weitere Informationen zu den Log Analytics-Berechtigungen finden Sie unter [Verwalten des Zugriffs auf Protokolldaten und Arbeitsbereiche in Azure Monitor](../platform/manage-access.md). 


## <a name="cluster-pricing-model"></a>Preismodell für Cluster

Dedizierte Log Analytics-Cluster verwenden ein Preismodell für die Kapazitätsreservierung mit mindestens 1000 GB/Tag. Jeder über die Reservierung hinausgehende Verbrauch wird entsprechend der nutzungsbasierten Bezahlung berechnet.  Preisinformationen zur Kapazitätsreservierung finden Sie auf der Seite [Azure Monitor – Preise]( https://azure.microsoft.com/pricing/details/monitor/).  

Die Reservierungsebene für die Clusterkapazität wird programmgesteuert mit Azure Resource Manager mithilfe des `Capacity`-Parameters unter `Sku` konfiguriert. `Capacity` wird in Einheiten von GB und in Schritten von 100 GB pro Tag in Werten von 1000 GB/Tag oder mehr angegeben.

Für die Abrechnung des Verbrauchs in einem Cluster stehen zwei Modi zur Verfügung. Diese können beim Konfigurieren Ihres Clusters mithilfe des Parameters `billingType` festgelegt werden. 

1. **Cluster** (Standardeinstellung): In diesem Fall werden erfasste Daten auf der Clusterebene abgerechnet. Die erfassten Datenmengen aus den einzelnen Arbeitsbereichen, die einem Cluster zugeordnet sind, werden aggregiert, um die tägliche Abrechnung für den Cluster zu berechnen. 

2. **Arbeitsbereiche**: Die Kapazitätsreservierungskosten für Ihren Cluster werden proportional den Arbeitsbereichen im Cluster zugeordnet (nach Berücksichtigung der knotenspezifischen Zuordnungen von [Azure Security Center](../../security-center/index.yml) für den jeweiligen Arbeitsbereich).

Wenn Ihr Arbeitsbereich den Legacytarif pro Knoten verwendet, wird er, wenn er mit einem Cluster verknüpft ist, basierend auf den in Bezug auf die Kapazitätsreservierung des Clusters erfassten Daten und nicht mehr pro Knoten abgerechnet. Datenzuweisungen pro Knoten von Azure Security Center werden weiterhin angewendet.

Weitere Informationen zur Abrechnung für dedizierte Log Analytics-Cluster finden Sie [hier]( https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters).

## <a name="asynchronous-operations-and-status-check"></a>Asynchrone Vorgänge und Statusüberprüfung

Einige Konfigurationsschritte werden asynchron ausgeführt, da sie nicht schnell abgeschlossen werden können. Der Status in der Antwort enthält eine der folgenden Angaben: „Wird ausgeführt“, „Wird aktualisiert“, „Wird gelöscht“, „Erfolgreich“ oder „Fehler“ einschließlich des Fehlercodes. Wenn REST verwendet wird, gibt die Antwort anfänglich den HTTP-Statuscode 202 (Akzeptiert) und einen Header mit der Eigenschaft „Azure-AsyncOperation“ zurück:

```JSON
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

Sie können den Status des asynchronen Vorgangs prüfen, indem Sie eine GET-Anforderung für den Headerwert Azure-AsyncOperation senden:

```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

## <a name="creating-a-cluster"></a>Erstellen eines Clusters

Erstellen Sie zunächst Clusterressourcen, um mit dem Erstellen eines dedizierten Clusters zu beginnen.

Die folgenden Eigenschaften müssen angegeben werden:

- **ClusterName**: Wird zu Verwaltungszwecken verwendet. Benutzer werden für diesen Namen nicht offengelegt.
- **ResourceGroupName**: Wie bei jeder Azure-Ressource gehören Cluster zu einer Ressourcengruppe. Es wird empfohlen, dass Sie eine zentrale IT-Ressourcengruppe verwenden, da Cluster in der Regel von vielen Teams in der Organisation gemeinsam genutzt werden. Weitere Entwurfsaspekte finden Sie unter [Entwerfen Ihrer Azure Monitor-Protokollbereitstellung](../platform/design-logs-deployment.md).
- **Standort**: Ein Cluster befindet sich in einer bestimmten Azure-Region. Nur Arbeitsbereiche, die sich in dieser Region befinden, können mit diesem Cluster verknüpft werden.
- **SkuCapacity**: Beim Erstellen einer *Clusterressource* müssen Sie die *Kapazitätsreservierungsebene* (sku) angeben. Die *Kapazitätsreservierungsebene* kann im Bereich von 1.000 bis 3.000 GB pro Tag liegen. Sie können sie bei Bedarf später in 100er Schritten aktualisieren. Wenn Sie eine Kapazitätsreservierungsebene von mehr als 3.000 GB pro Tag benötigen, kontaktieren Sie uns unter LAIngestionRate@microsoft.com. Weitere Informationen zu den Clusterkosten finden Sie unter [Verwalten von Kosten für Log Analytics-Cluster](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters).

Nachdem Sie die *Clusterressource* erstellt haben, können Sie sie mit *sku*, *keyVaultProperties oder *billingType* aktualisieren. Weitere Einzelheiten finden Sie unten.

Pro Abonnement und Region können bis zu zwei aktive Cluster vorhanden sein. Wenn ein Cluster gelöscht wird, ist er weiterhin für 14 Tage reserviert. Pro Abonnement und Region können bis zu vier reservierte Cluster (aktiv oder kürzlich gelöscht) vorhanden sein.

> [!WARNING]
> Die Clustererstellung löst die Ressourcenzuordnung und -bereitstellung aus. Dieser Vorgang kann einige Stunden dauern. Es wird empfohlen, ihn asynchron auszuführen.

Das Benutzerkonto, mit dem die Cluster erstellt werden, muss über die Standardberechtigung für die Erstellung von Azure-Ressourcen verfügen: `Microsoft.Resources/deployments/*` und die Clusterschreibberechtigung `Microsoft.OperationalInsights/clusters/write`. Dazu muss in seinen Rollenzuweisungen diese spezifische Aktion, `Microsoft.OperationalInsights/*` oder `*/write` angegeben sein.

### <a name="create"></a>Erstellen 

**PowerShell**

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -Location {region-name} -SkuCapacity {daily-ingestion-gigabyte} -AsJob

# Check when the job is done
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

**REST**

*Call* 
```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 1000
    },
  "properties": {
    "billingType": "cluster",
    },
  "location": "<region-name>",
}
```

*Antwort*

202 (Akzeptiert) und ein Header

### <a name="check-cluster-provisioning-status"></a>Überprüfen des Bereitstellungsstatus für den Cluster

Es dauert eine Weile, bis die Bereitstellung des Log Analytics-Clusters abgeschlossen ist. Sie können den Bereitstellungsstatus auf verschiedene Weise überprüfen:

- Führen Sie den PowerShell-Befehl „Get-AzOperationalInsightsCluster“ mit dem Ressourcengruppennamen aus, und überprüfen Sie die Eigenschaft „ProvisioningState“. Während der Bereitstellung lautet dieser Wert *ProvisioningAccount*, und nach Abschluss des Vorgangs lautet er *Succeeded*.
  ```powershell
  New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

- Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die Überprüfung des Status asynchroner Vorgänge.

- Senden Sie eine GET-Anforderung für die *Clusterressource*, und überprüfen Sie den Wert für *provisioningState*. Während der Bereitstellung lautet dieser Wert *ProvisioningAccount*, und nach Abschluss des Vorgangs lautet er *Succeeded*.

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
   Authorization: Bearer <token>
   ```

   **Antwort**

   ```json
   {
     "identity": {
       "type": "SystemAssigned",
       "tenantId": "tenant-id",
       "principalId": "principal-id"
       },
     "sku": {
       "name": "capacityReservation",
       "capacity": 1000,
       "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
       },
     "properties": {
       "provisioningState": "ProvisioningAccount",
       "billingType": "cluster",
       "clusterId": "cluster-id"
       },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
   }
   ```

Die GUID *principalId* wird vom verwalteten Identitätsdienst für die *Clusterressource* generiert.

## <a name="link-a-workspace-to-cluster"></a>Verknüpfen eines Arbeitsbereichs mit einem Cluster

Wenn ein Arbeitsbereich mit einem dedizierten Cluster verknüpft ist, werden neue Daten, die in den Arbeitsbereich erfasst werden, an den neuen Cluster weitergeleitet, während vorhandene Daten im vorhandenen Cluster verbleiben. Wenn der dedizierte Cluster mithilfe von kundenseitig verwalteten Schlüsseln (Customer-managed keys, CMK) verschlüsselt wird, werden nur neue Daten mit dem Schlüssel verschlüsselt. Das System abstrahiert diesen Unterschied von den Benutzern, und die Benutzer fragen den Arbeitsbereich wie gewohnt ab, während das System clusterübergreifende Abfragen für das Back-End ausführt.

Ein Cluster kann mit bis zu 100 Arbeitsbereichen verknüpft werden. Verknüpfte Arbeitsbereiche befinden sich in derselben Region wie der Cluster. Um das System-Back-End zu schützen und die Fragmentierung von Daten zu vermeiden, kann ein Arbeitsbereich nicht mehr als zweimal pro Monat mit einem Cluster verknüpft werden.

Um den Verknüpfungsvorgang auszuführen, benötigen Sie Schreibberechtigungen für den Arbeitsbereich und die *Clusterressource*:

- Im Arbeitsbereich: *Microsoft.OperationalInsights/workspaces/write*
- In der *Clusterressource*: *Microsoft.OperationalInsights/clusters/write*

Zusätzlich zu den Abrechnungsaspekten behält der verknüpfte Arbeitsbereich seine eigenen Einstellungen wie etwa die Länge der Datenaufbewahrung bei.
Der Arbeitsbereich und der Cluster können sich in verschiedenen Abonnements befinden. Es ist möglich, dass sich der Arbeitsbereich und der Cluster in unterschiedlichen Mandanten befinden, wenn Azure Lighthouse verwendet wird, um beide einem einzelnen Mandanten zuzuordnen.

Wie jeder Clustervorgang kann das Verknüpfen eines Arbeitsbereichs nur nach Abschluss der Log Analytics-Clusterbereitstellung ausgeführt werden.

> [!WARNING]
> Das Verknüpfen eines Arbeitsbereichs mit einem Cluster erfordert es, mehrere Back-End-Komponenten zu synchronisieren und die Cachehydration zu gewährleisten. Dieser Vorgang kann bis zu zwei Stunden in Anspruch nehmen. Es wird empfohlen, ihn asynchron auszuführen.


**PowerShell**

Verwenden Sie den folgenden PowerShell-Befehl zum Verknüpfen eines Clusters:

```powershell
# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name}).id

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```


**REST**

Verwenden Sie den folgenden REST-Aufruf zum Verknüpfen eines Clusters:

*Senden*

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Antwort*

202 (Akzeptiert) und Header

### <a name="check-workspace-link-status"></a>Überprüfen des Verknüpfungsstatus des Arbeitsbereichs
  
Wenn Sie kundenseitig verwaltete Schlüssel verwenden, werden erfasste Daten nach dem Zuordnungsvorgang mit Ihrem verwalteten Schlüssel verschlüsselt gespeichert, was bis zu 90 Minuten dauern kann. 

Sie können den Zuordnungsstatus des Arbeitsbereichs auf zwei Arten überprüfen:

- Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die Überprüfung des Status asynchroner Vorgänge.

- Führen Sie den Get-Vorgang für den Arbeitsbereich aus, und beobachten Sie, ob die *clusterResourceId*-Eigenschaft unter *features* in der Antwort vorhanden ist.

**BEFEHLSZEILENSCHNITTSTELLE (CLI)**

```azurecli
az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
```

**PowerShell**

```powershell
Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

**REST**

*Call*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>?api-version=2020-08-01
Authorization: Bearer <token>
```

*Antwort*

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region-name"
}
```

## <a name="change-cluster-properties"></a>Ändern der Clustereigenschaften

Nachdem Sie Ihre *Clusterressource* erstellt und vollständig bereitgestellt haben, können Sie zusätzliche Eigenschaften auf Clusterebene mithilfe von PowerShell oder der REST-API bearbeiten. Abgesehen von den Eigenschaften, die während der Clustererstellung verfügbar sind, können zusätzliche Eigenschaften erst festgelegt werden, nachdem der Cluster bereitgestellt wurde:

- **keyVaultProperties**: Hiermit wird der Schlüssel in Azure Key Vault aktualisiert. Weitere Informationen finden Sie unter [Aktualisieren des Clusters mit Schlüsselbezeichnerdetails](../platform/customer-managed-keys.md#update-cluster-with-key-identifier-details). Sie enthält die folgenden Parameter: *KeyVaultUri*, *KeyName*, *KeyVersion*. 
- **billingType**: Die Eigenschaft *billingType* bestimmt die Abrechnungszuordnung für die *Clusterressource* und deren Daten:
  - **Cluster** (Standard): Die Kapazitätsreservierungskosten für Ihren Cluster werden der *Clusterressource* zugeordnet.
  - **Arbeitsbereiche**: Die Kapazitätsreservierungskosten für Ihren Cluster werden proportional den Arbeitsbereichen im Cluster zugeordnet. Wenn die Gesamtmenge der erfassten Daten unter der Kapazitätsreservierung liegt, wird ein Teil des Verbrauchs über die *Clusterressource* abgerechnet. Weitere Informationen zum Clusterpreismodell finden Sie unter [Dedizierte Log Analytics-Cluster](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters). 

> [!NOTE]
> Die Eigenschaft *billingType* wird in PowerShell nicht unterstützt.

### <a name="get-all-clusters-in-resource-group"></a>Abrufen aller Cluster in einer Ressourcengruppe
  
**BEFEHLSZEILENSCHNITTSTELLE (CLI)**

```azurecli
az monitor log-analytics cluster list --resource-group "resource-group-name"
```

**PowerShell**

```powershell
Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
```

**REST**

*Call*

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

*Antwort*
  
  ```json
  {
    "value": [
      {
        "identity": {
          "type": "SystemAssigned",
          "tenantId": "tenant-id",
          "principalId": "principal-Id"
        },
        "sku": {
          "name": "capacityReservation",
          "capacity": 1000,
          "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
          },
        "properties": {
           "keyVaultProperties": {
              "keyVaultUri": "https://key-vault-name.vault.azure.net",
              "keyName": "key-name",
              "keyVersion": "current-version"
              },
          "provisioningState": "Succeeded",
          "billingType": "cluster",
          "clusterId": "cluster-id"
        },
        "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
        "name": "cluster-name",
        "type": "Microsoft.OperationalInsights/clusters",
        "location": "region-name"
      }
    ]
  }
  ```

### <a name="get-all-clusters-in-subscription"></a>Abrufen aller Cluster in einem Abonnement

**BEFEHLSZEILENSCHNITTSTELLE (CLI)**

```azurecli
az monitor log-analytics cluster list
```

**PowerShell**

```powershell
Get-AzOperationalInsightsCluster
```

**REST**

*Call*

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
Authorization: Bearer <token>
```
    
*Antwort*
    
Dasselbe wie bei „Cluster in einer Ressourcengruppe“, jedoch im Abonnementbereich.



### <a name="update-capacity-reservation-in-cluster"></a>Aktualisieren der Kapazitätsreservierung in einem Cluster

Wenn sich das Datenvolumen Ihrer verknüpften Arbeitsbereiche im Laufe der Zeit ändert und Sie die Kapazitätsreservierungsebene entsprechend aktualisieren möchten. Die Kapazität wird in der Einheit GB angegeben und kann Werte von 1000 GB/Tag oder mehr in Schritten von 100 GB pro Tag aufweisen. Beachten Sie, dass Sie nicht den vollständigen REST-Anforderungstext angeben müssen, aber die SKU einschließen sollten.

**BEFEHLSZEILENSCHNITTSTELLE (CLI)**

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity 1000
```

**PowerShell**

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity 1000
```

**REST**

*Call*

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 2000
    }
  }
  ```

### <a name="update-billingtype-in-cluster"></a>Aktualisieren von billingType im Cluster

Die Eigenschaft *billingType* bestimmt die Abrechnungszuordnung für den Cluster und dessen Daten:
- *Cluster* (Standard): Die Abrechnung wird dem Abonnement zugeordnet, das Ihre Clusterressource hostet.
- *Arbeitsbereiche*: Die Abrechnung wird den Abonnements zugeordnet, die Ihre Arbeitsbereiche proportional hosten.

**REST**

*Call*

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ```

### <a name="unlink-a-workspace-from-cluster"></a>Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Cluster

Sie können die Verknüpfung eines Arbeitsbereichs mit einem Cluster aufheben. Nach dem Aufheben der Verknüpfung eines Arbeitsbereichs mit dem Cluster werden neue Daten, die diesem Arbeitsbereich zugeordnet sind, nicht an den dedizierten Cluster gesendet. Außerdem erfolgt die Abrechnung des Arbeitsbereichs nicht mehr über den Cluster. Alte Daten des nicht verknüpften Arbeitsbereichs können im Cluster verbleiben. Wenn diese Daten mithilfe von kundenseitig verwalteten Schlüsseln (Customer-managed keys, CMK) verschlüsselt werden, werden die Key Vault-Geheimnisse beibehalten. Das System abstrahiert diese Änderung von Log Analytics-Benutzern. Benutzer können den Arbeitsbereich einfach wie gewohnt abfragen. Das System führt bei Bedarf clusterübergreifende Abfragen für das Back-End ohne einen Hinweis auf die Benutzer durch.  

> [!WARNING] 
> Es gibt eine Begrenzung von zwei Verknüpfungsvorgängen für einen bestimmten Arbeitsbereich innerhalb eines Monats. Berücksichtigen und planen Sie das Aufheben der Verknüpfung von Aktionen gründlich.

**BEFEHLSZEILENSCHNITTSTELLE (CLI)**

```azurecli
az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --workspace-name "MyWorkspace" --name cluster
```

**PowerShell**

Verwenden Sie den folgenden PowerShell-Befehl zum Aufheben der Verknüpfung eines Arbeitsbereichs mit einem Cluster:

```powershell
# Unlink a workspace from cluster
Remove-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster
```

### <a name="delete-cluster"></a>Löschen von Clustern

Eine dedizierte Clusterressource kann gelöscht werden. Sie müssen die Verknüpfung aller Arbeitsbereiche mit dem Cluster aufheben, bevor Sie ihn löschen. Sie brauchen Schreibberechtigungen für die *Clusterressource*, um diesen Vorgang auszuführen. 

Nachdem die Clusterressource gelöscht wurde, wechselt der physische Cluster in einen Bereinigungs- und Löschvorgang. Beim Löschen eines Clusters werden alle Daten gelöscht, die auf dem Cluster gespeichert waren. Die Daten stammen unter Umständen aus Arbeitsbereichen, die in der Vergangenheit mit dem Cluster verknüpft wurden.

Eine *Clusterressource*, die in den letzten 14 Tagen gelöscht wurde, befindet sich im vorläufig gelöschten Zustand und kann zusammen mit den Daten wiederhergestellt werden. Da bei der Löschung der *Clusterressource* die Zuordnung aller Arbeitsbereiche zur *Clusterressource* aufgehoben wurde, müssen Sie die Arbeitsbereiche nach der Wiederherstellung neu zuordnen. Der Wiederherstellungsvorgang kann vom Benutzer nicht durchgeführt werden. Wenden Sie sich daher bei Wiederherstellungsanfragen an Ihren Microsoft-Kanal oder den Support.

Innerhalb von 14 Tagen nach dem Löschvorgang ist der Name der Clusterressource reserviert und kann nicht von anderen Ressourcen verwendet werden.

> [!WARNING] 
> Es gibt eine Begrenzung von drei Clustern pro Abonnement. Dazu zählen sowohl aktive als auch vorläufig gelöschte Cluster. Kunden sollten keine wiederkehrenden Prozeduren erstellen, mit denen Cluster erstellt und gelöscht werden. Dies hat erhebliche Auswirkungen auf Log Analytics-Back-End-Systeme.

**PowerShell**

Verwenden Sie den folgenden PowerShell-Befehl zum Löschen eines Clusters:

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

**REST**

Verwenden Sie den folgenden REST-Aufruf zum Löschen eines Clusters:

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Antwort**

  200 – OK

## <a name="limits-and-constraints"></a>Grenzen und Einschränkungen

- Die maximale Anzahl aktiver Cluster pro Region und Abonnement beträgt 2.

- Die maximale Anzahl reservierter Cluster (aktiv oder kürzlich gelöscht) pro Region und Abonnement beträgt 4. 

- Das Maximum der verknüpften Arbeitsbereiche für den Cluster beträgt 1000.

- Sie können einen Arbeitsbereich mit Ihrem Cluster verknüpfen und dann die Verknüpfung aufheben. Die Anzahl der Verknüpfungen in einem bestimmten Arbeitsbereich ist innerhalb eines Zeitraums von 30 Tagen auf 2 begrenzt.

- Das Verschieben eines Clusters in eine andere Ressourcengruppe oder ein anderes Abonnement wird derzeit nicht unterstützt.

- Lockbox ist in China derzeit nicht verfügbar. 

- [Doppelte Verschlüsselung](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) wird automatisch für Cluster konfiguriert, die ab Oktober 2020 in unterstützten Regionen erstellt werden. Sie können überprüfen, ob Ihr Cluster für doppelte Verschlüsselung konfiguriert ist. Dazu senden Sie eine GET-Anforderung für den Cluster und beobachten, ob der `isDoubleEncryptionEnabled`-Wert für Cluster mit aktivierter doppelter Verschlüsselung `true` ist. 
  - Wenn Sie einen Cluster erstellen und die Fehlermeldung „Die doppelte Verschlüsselung für Cluster wird von <Regionsname> nicht unterstützt.“ erhalten, können Sie den Cluster immer noch ohne doppelte Verschlüsselung erstellen, indem Sie `"properties": {"isDoubleEncryptionEnabled": false}` im REST-Anforderungstext hinzufügen.
  - Die Einstellung für doppelte Verschlüsselung kann nach dem Erstellen des Clusters nicht mehr geändert werden.

## <a name="troubleshooting"></a>Problembehandlung

- Wenn beim Erstellen eines Clusters ein Konfliktfehler auftritt, haben Sie Ihren Cluster möglicherweise in den letzten 14 Tagen gelöscht und dieser befindet sich nun im Zustand des vorläufigen Löschens. Der Clustername bleibt während des Zeitraums des vorläufigen Löschens reserviert, und Sie können keinen neuen Cluster mit diesem Namen erstellen. Der Name wird nach Ablauf des Zeitraums des vorläufigen Löschens freigegeben, wenn der Cluster dauerhaft gelöscht wird.

- Wenn Sie den Cluster aktualisieren, während der Cluster bereitgestellt oder bereits aktualisiert wird, tritt bei der Aktualisierung ein Fehler auf.

- Einige Vorgänge dauern lange und können einige Zeit in Anspruch nehmen. Dies sind Erstellen des Clusters, Aktualisieren des Clusterschlüssels und Löschen des Clusters. Sie können den Vorgangsstatus auf zwei Arten überprüfen:
  - Kopieren Sie bei Verwendung von REST den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
  - Senden Sie eine GET-Anforderung an den Cluster oder Arbeitsbereich, und beobachten Sie die Antwort. Beispielsweise weist der nicht verknüpfte Arbeitsbereich unter *features* nicht die *clusterResourceId* auf.

- Die Arbeitsbereichverknüpfung mit dem Cluster schlägt fehl, wenn er mit einem anderen Cluster verknüpft ist.

- Fehlermeldungen
  
  Clustererstellung:
  -  400 – Der Clustername ist ungültig. Der Clustername darf die Zeichen „a–z“, „A–Z“, „0–9“ und die Länge „3–63“ enthalten.
  -  400 – Der Text der Anforderung ist NULL oder hat ein ungültiges Format.
  -  400 – Der SKU-Name ist ungültig. Legen Sie den SKU-Namen auf „capacityReservation“ fest.
  -  400 – Kapazität wurde bereitgestellt, aber SKU ist nicht „capacityReservation“. Legen Sie den SKU-Namen auf „capacityReservation“ fest.
  -  400 – Fehlende Kapazität in SKU. Legen Sie den Kapazitätswert in Schritten von 100 (GB) auf „1.000“ oder höher fest.
  -  400 – Die Kapazität in SKU liegt nicht im Bereich. Sie sollte mindestens 1.000 und bis zur maximal zulässigen Kapazität betragen, die in Ihrem Arbeitsbereich unter „Nutzung und geschätzte Kosten“ zur Verfügung steht.
  -  400 – Die Kapazität ist für 30 Tage gesperrt. Eine Verringerung der Kapazität ist 30 Tage nach dem Update zulässig.
  -  400 – Es wurde keine SKU festgelegt. Legen Sie den SKU-Namen auf „capacityReservation“ und den Kapazitätswert in Schritten von 100 (GB) auf „1.000“ oder höher fest.
  -  400 – „Identität“ ist NULL oder leer. Legen Sie die Identität mit dem Typ „systemAssigned“ fest.
  -  400 – „KeyVaultProperties“ werden bei der Erstellung festgelegt. Aktualisieren Sie „KeyVaultProperties“ nach der Clustererstellung.
  -  400 – Der Vorgang kann jetzt nicht ausgeführt werden. Der asynchrone Vorgang befindet sich in einem anderen Zustand als „erfolgreich“. Der Cluster muss seinen Vorgang beenden, bevor ein Aktualisierungsvorgang ausgeführt wird.

  Clusterupdate
  -  400 –-Der Cluster befindet sich im Zustand „wird gelöscht“. Asynchroner Vorgang wird ausgeführt. Der Cluster muss seinen Vorgang beenden, bevor ein Aktualisierungsvorgang ausgeführt wird.
  -  400 – „KeyVaultProperties“ ist nicht leer, hat aber ein ungültiges Format. Lesen Sie [Key Identifier Update](../platform/customer-managed-keys.md#update-cluster-with-key-identifier-details) (Aktualisierung des Schlüsselbezeichners).
  -  400 – Fehler beim Überprüfen des Schlüssels in Key Vault. Mögliche Ursachen: Fehlende Berechtigungen, oder der Schlüssel ist nicht vorhanden. Vergewissern Sie sich, dass Sie die [Schlüssel- und Zugriffsrichtlinie in Key Vault](../platform/customer-managed-keys.md#grant-key-vault-permissions) festgelegt haben.
  -  400 – Der Schlüssel kann nicht wiederhergestellt werden. Key Vault muss auf „Vorläufiges Löschen und Löschschutz“ festgelegt werden. Lesen Sie die [Dokumentation zu Key Vault](../../key-vault/general/soft-delete-overview.md).
  -  400 – Der Vorgang kann jetzt nicht ausgeführt werden. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.
  -  400 –-Der Cluster befindet sich im Zustand „wird gelöscht“. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.

  Clusterabruf:
    -  404 – Der Cluster wurde nicht gefunden; möglicherweise wurde er gelöscht. Wenn Sie versuchen, einen Cluster mit diesem Namen zu erstellen, und es zu einem Konflikt kommt, ist der Cluster 14 Tage lang im Zustand „Vorläufig gelöscht“. Sie können den Support kontaktieren, um ihn wiederherzustellen, oder einen anderen Namen zum Erstellen eines neuen Clusters verwenden. 

  Clusterlöschung
    -  409 – Ein Cluster kann im Bereitstellungsstatus nicht gelöscht werden. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.

  Arbeitsbereichverknüpfung:
  -  404 – Arbeitsbereich nicht gefunden. Der von Ihnen angegebene Arbeitsbereich ist nicht vorhanden oder wurde gelöscht.
  -  409 – Arbeitsbereichverknüpfung oder Aufheben der Verknüpfung in Bearbeitung.
  -  400 – Der Cluster wurde nicht gefunden, der angegebene Cluster ist nicht vorhanden, oder er wurde gelöscht. Wenn Sie versuchen, einen Cluster mit diesem Namen zu erstellen, und es zu einem Konflikt kommt, ist der Cluster 14 Tage lang im Zustand „Vorläufig gelöscht“. Sie können den Support kontaktieren, um ihn wiederherzustellen.

  Aufheben der Arbeitsbereichverknüpfung:
  -  404 – Arbeitsbereich nicht gefunden. Der von Ihnen angegebene Arbeitsbereich ist nicht vorhanden oder wurde gelöscht.
  -  409 – Arbeitsbereichverknüpfung oder Aufheben der Verknüpfung in Bearbeitung.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Abrechnung dedizierter Log Analytics-Cluster](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters).
- Erfahren Sie mehr über das [Entwerfen von Log Analytics-Arbeitsbereichen](../platform/design-logs-deployment.md).
