---
title: Kundenseitig verwaltete Schlüssel in Azure Monitor
description: Informationen und Schritte zum Konfigurieren kundenseitig verwalteter Schlüssel (Customer-Managed Key, CMK) für das Verschlüsseln von Daten in Ihren Log Analytics-Arbeitsbereichen mithilfe eines Azure Key Vault-Schlüssels.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 09/09/2020
ms.openlocfilehash: 5d44758ebf94c7487935ef47a17ad810dc5cf9f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89657310"
---
# <a name="azure-monitor-customer-managed-key"></a>Kundenseitig verwaltete Schlüssel in Azure Monitor 

Dieser Artikel enthält Hintergrundinformationen und Schritte zum Konfigurieren kundenseitig verwalteter Schlüssel (Customer-Managed Key, CMK) für Ihre Log Analytics-Arbeitsbereiche. Nach der Konfiguration werden alle Daten, die an Ihre Arbeitsbereiche gesendet werden, mit Ihrem Azure Key Vault-Schlüssel verschlüsselt.

Es wird empfohlen, vor der Konfiguration die [Einschränkungen](#limitationsandconstraints) weiter unten zu überprüfen.

## <a name="customer-managed-key-cmk-overview"></a>Übersicht über kundenseitig verwaltete Schlüssel (Customer-Managed Key, CMK)

Die [Verschlüsselung ruhender Daten](../../security/fundamentals/encryption-atrest.md) ist eine übliche Datenschutz- und Sicherheitsanforderung in Organisationen. Sie können die Verschlüsselung ruhender Daten vollständig von Azure verwalten lassen, wobei Ihnen verschiedene Optionen zum genauen Verwalten der Verschlüsselung oder Verschlüsselungsschlüssel bereitstehen.

Mit Azure Monitor wird sichergestellt, dass alle Daten und gespeicherten Abfragen im Ruhezustand mit von Microsoft verwalteten Schlüsseln (MMK) verschlüsselt werden. Azure Monitor bietet auch eine Option für die Verschlüsselung mithilfe eines eigenen Schlüssels, der in Ihrer [Azure Key Vault](../../key-vault/general/overview.md)-Instanz gespeichert ist und auf den über die Authentifizierung der systemseitig zugewiesenen [verwalteten Identität](../../active-directory/managed-identities-azure-resources/overview.md) zugegriffen wird. Dieser Schlüssel (CMK) kann entweder durch [Software oder Hardware (HSM) geschützt](../../key-vault/general/overview.md) werden. Die Verwendung der Verschlüsselung durch Azure Monitor entspricht der Funktionsweise der [Azure Storage-Verschlüsselung](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption).

Die CMK-Funktion wird auf dedizierten Log Analytics-Clustern bereitgestellt und bietet Ihnen die Möglichkeit, den Zugriff auf Ihre Daten jederzeit zu widerrufen und die Daten mit [Lockbox](#customer-lockbox-preview) zu schützen. Um sicherzustellen, dass die erforderliche Kapazität für einen dedizierten Cluster in Ihrer Region vorhanden ist, muss Ihr Abonnement im Voraus zugelassen sein. Wenden Sie sich an Ihren Microsoft-Kontakt, um Ihr Abonnement zuzulassen, bevor Sie mit der Konfiguration von CMK beginnen.

Beim [Preismodell für Log Analytics-Cluster](./manage-cost-storage.md#log-analytics-dedicated-clusters) werden Kapazitätsreservierungen ab 1000 GB/Tag verwendet.

Daten, die in den letzten 14 Tagen erfasst wurden, werden für einen effizienten Betrieb der Abfrage-Engine auch im Hot-Cache (SSD-gestützt) aufbewahrt. Diese Daten bleiben unabhängig von der CMK-Konfiguration mit Microsoft-Schlüsseln verschlüsselt, aber Ihre Kontrolle über SSD-Daten entspricht der [Schlüsselsperrung](#cmk-kek-revocation). Wir arbeiten daran, dass SSD-Daten in der zweiten Hälfte des Jahres 2020 mit CMK verschlüsselt werden.

## <a name="how-cmk-works-in-azure-monitor"></a>Funktionsweise von CMK in Azure Monitor

Azure Monitor nutzt eine systemseitig zugewiesene verwaltete Identität, um Zugriff auf Azure Key Vault zu gewähren. Eine systemseitig zugewiesene verwaltete Identität kann nur einer einzigen Azure-Ressource zugeordnet werden, während die Identität des Log Analytics Clusters auf Clusterebene unterstützt wird. Dadurch ist vorgegeben, dass die CMK-Funktion auf einem dedizierten Log Analytics-Cluster bereitgestellt wird. Zur Unterstützung von CMK in mehreren Arbeitsbereichen wird eine neue Log Analytics-*Clusterressource* als temporäre Identitätsverbindung zwischen Key Vault und Log Analytics-Arbeitsbereichen eingesetzt. Der Log Analytics-Clusterspeicher verwendet die der *Clusterressource* zugeordnete verwaltete Identität für die Authentifizierung bei Azure Key Vault über Azure Active Directory. 

Nach der CMK-Konfiguration werden alle Daten, die in Arbeitsbereichen erfasst wurden, die Ihrer *Clusterressource* zugeordnet sind, mit Ihrem Schlüssel in Key Vault verschlüsselt. Sie können die Zuordnung von Arbeitsbereichen zur *Clusterressource* jederzeit aufheben. Neue Daten werden im Log Analytics-Speicher erfasst und mit dem Microsoft-Schlüssel verschlüsselt, während Sie die neuen und alten Daten nahtlos abfragen können.


![CMK-Übersicht](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. Log Analytics-*Clusterressource* mit verwalteter Identität und Berechtigungen für Key Vault. Die Identität wird an den zugrunde liegenden dedizierten Log Analytics-Clusterspeicher weitergegeben.
3. Dedizierter Log Analytics-Cluster
4. Arbeitsbereiche, die der *Clusterressource* für die CMK-Verschlüsselung zugeordnet sind

## <a name="encryption-keys-operation"></a>Vorgang für Verschlüsselungsschlüssel

An der Speicherdatenverschlüsselung sind drei Arten von Schlüsseln beteiligt:

- **KEK** (Key Encryption Key): Schlüsselverschlüsselungsschlüssel (CMK)
- **AEK** (Account Encryption Key): Kontoverschlüsselungsschlüssel
- **DEK** (Data Encryption Key): Datenverschlüsselungsschlüssel

Es gelten die folgenden Regeln:

- Die Speicherkonten im Log Analytics-Cluster generieren einen eindeutigen Verschlüsselungsschlüssel für jedes Speicherkonto, der als AEK bezeichnet wird.

- Der AEK dient zum Ableiten von DEKs, bei denen es sich um die Schlüssel handelt, die zum Verschlüsseln der einzelnen, auf den Datenträger geschriebenen Datenblöcke verwendet werden.

- Wenn Sie den Schlüssel in Key Vault konfigurieren und in der *Clusterressource* darauf verweisen, sendet Azure Storage Anforderungen an Azure Key Vault, um den AEK zu packen und zu entpacken und so Verschlüsselungs- und Entschlüsselungsvorgänge für Daten auszuführen.

- Der KEK verlässt niemals den Key Vault, und im Fall eines HSM-Schlüssels verlässt dieser niemals die Hardware.

- Azure Storage verwendet die der *Clusterressource* zugeordnete verwaltete Identität für die Authentifizierung und den Zugriff auf Azure Key Vault über Azure Active Directory.

## <a name="cmk-provisioning-procedure"></a>CMK-Bereitstellungsverfahren

1. Zulassen des Abonnements: Die CMK-Funktion wird auf dedizierten Log Analytics-Clustern bereitgestellt. Um sicherzustellen, dass die erforderliche Kapazität in Ihrer Region vorhanden ist, muss Ihr Abonnement im Voraus zugelassen sein. Wenden Sie sich an Ihren Microsoft-Kontakt, um Ihr Abonnement zuzulassen.
2. Erstellen von Azure Key Vault und Speichern des Schlüssels
3. Erstellen einer *Clusterressource*
4. Gewähren von Berechtigungen für Key Vault
5. Zuordnen von Log Analytics-Arbeitsbereichen

Das Verfahren wird im Azure-Portal nicht unterstützt, und die Bereitstellung erfolgt über PowerShell oder REST-Anforderungen.

> [!IMPORTANT]
> Jede REST-Anforderung muss ein Bearer-Autorisierungstoken im Anforderungsheader enthalten.

Beispiel:

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2020-08-01
Authorization: Bearer eyJ0eXAiO....
```

Hierbei stellt *eyJ0eXAiO....* das vollständige Autorisierungstoken dar. 

Sie können das Token mit einer der folgenden Methoden abrufen:

1. Verwenden Sie die Methode der [App-Registrierungen](/graph/auth/auth-concepts#access-tokens).
2. Im Azure-Portal
    1. Navigieren Sie zum Azure-Portal, während Sie sich im „Entwicklertool“ (F12) befinden.
    1. Suchen Sie in einer der „batch?api-version“-Instanzen unter „Anforderungsheader“ nach der Autorisierungszeichenfolge. Diese ähnelt Folgendem: „authorization: Bearer eyJ0eXAiO....“. 
    1. Kopieren Sie die Zeichenfolge, und fügen Sie sie Ihrem API-Aufruf gemäß den folgenden Beispielen hinzu.
3. Navigieren Sie zur Azure-REST-Dokumentationswebsite. Wählen Sie für eine beliebige API die Option „Ausprobieren“ aus, und kopieren Sie das Bearertoken.

### <a name="asynchronous-operations-and-status-check"></a>Asynchrone Vorgänge und Statusüberprüfung

Einige der Vorgänge in dieser Konfigurationsprozedur werden asynchron ausgeführt, da Sie nicht schnell abgeschlossen werden können. Wenn REST-Anforderungen in der Konfiguration verwendet werden, gibt die Antwort anfänglich einen HTTP-Statuscode 200 (OK) und einen Header mit der Eigenschaft *Azure-AsyncOperation* zurück, wenn sie akzeptiert werden:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

Sie können dann den Status des asynchronen Vorgangs prüfen, indem Sie eine GET-Anforderung für den Headerwert *Azure-AsyncOperation* senden:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

Die Antwort enthält Informationen zu dem Vorgang und seinen *Status*. Folgende Werte sind möglich:

Vorgang wird ausgeführt
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "InProgress", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
}
```

Schlüsselbezeichner-Aktualisierungsvorgang wird ausgeführt
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Updating", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Löschung einer *Clusterressource* wird ausgeführt. Wenn Sie eine *Clusterressource* löschen, der Arbeitsbereiche zugeordnet sind, wird für jeden Arbeitsbereich in asynchronen Vorgängen, die einige Zeit in Anspruch nehmen können, eine Aufhebung der Zuordnung durchgeführt.
Dies trifft nicht zu, wenn Sie einen *Cluster* ohne zugeordneten Arbeitsbereich löschen. In diesem Fall wird die *Clusterressource* sofort gelöscht.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Deleting", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Vorgang ist abgeschlossen
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Succeeded", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Fehler beim Vorgang
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Failed", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "error" : { 
        "code": "error-code",  
        "message": "error-message" 
    }
}
```

### <a name="allowing-subscription-for-cmk-deployment"></a>Zulassen eines Abonnements für die CMK-Bereitstellung

Die CMK-Funktion wird auf dedizierten Log Analytics-Clustern bereitgestellt. Um sicherzustellen, dass die erforderliche Kapazität in Ihrer Region vorhanden ist, muss Ihr Abonnement im Voraus zugelassen sein. Verwenden Sie Ihre Kontakte zu Microsoft zum Angeben Ihrer Abonnement-IDs.

> [!IMPORTANT]
> Die CMK-Funktion ist regional. Azure Key Vault, *Clusterressource* und zugehörige Log Analytics-Arbeitsbereiche müssen sich in der gleichen Region befinden, können jedoch in unterschiedlichen Abonnements enthalten sein.

### <a name="storing-encryption-key-kek"></a>Speichern des Verschlüsselungsschlüssels (KEK)

Erstellen Sie eine Azure Key Vault-Instanz oder verwenden Sie eine vorhandene Instanz, um einen Schlüssel zu generieren oder importieren, der für die Datenverschlüsselung verwendet werden soll. Azure Key Vault muss als wiederherstellbar konfiguriert werden, um Ihren Schlüssel und den Zugriff auf Ihre Daten in Azure Monitor zu schützen. Sie können diese Konfiguration unter den Eigenschaften in Ihrer Key Vault-Instanz überprüfen. Sowohl *Vorläufiges Löschen* als auch *Bereinigungsschutz* sollten aktiviert sein.

![Einstellungen für vorläufiges Löschen und Löschschutz](media/customer-managed-keys/soft-purge-protection.png)

Diese Einstellungen können über die CLI und PowerShell aktualisiert werden:

- [Vorläufiges Löschen](../../key-vault/general/soft-delete-overview.md)
- [Bereinigungsschutz](../../key-vault/general/soft-delete-overview.md#purge-protection) schützt vor erzwungenem Löschen des Geheimnisses/Tresors auch nach vorläufigem Löschen

### <a name="create-cluster-resource"></a>Erstellen einer *Clusterressource*

Diese Ressource wird als eine temporäre Identitätsverbindung zwischen Key Vault und Ihren Log Analytics-Arbeitsbereichen verwendet. Nachdem Sie eine Bestätigung erhalten haben, dass Ihre Abonnements zugelassen wurden, erstellen Sie eine Log Analytics-*Clusterressource* in der Region, in der sich Ihre Arbeitsbereiche befinden.

Beim Erstellen einer *Clusterressource* müssen Sie die *Kapazitätsreservierungsebene* (sku) angeben. Die *Kapazitätsreservierungsebene* kann im Bereich von 1000 bis 3000 GB pro Tag liegen, und Sie können sie in 100er Schritten aktualisieren. Wenn Sie eine Kapazitätsreservierungsebene von mehr als 3000 GB pro Tag benötigen, kontaktieren Sie uns unter LAIngestionRate@microsoft.com. [Weitere Informationen](./manage-cost-storage.md#log-analytics-dedicated-clusters)

Die Eigenschaft *billingType* bestimmt die Abrechnungszuordnung für die *Clusterressource* und deren Daten:
- *Cluster* (Standard): Die Kapazitätsreservierungskosten für Ihren Cluster werden der *Clusterressource* zugeordnet.
- *Arbeitsbereiche*: Die Kapazitätsreservierungskosten für Ihren Cluster werden proportional den Arbeitsbereichen im Cluster zugeordnet. Wenn die Gesamtmenge der erfassten Daten unter der Kapazitätsreservierung liegt, wird ein Teil des Verbrauchs über die *Clusterressource* abgerechnet. Weitere Informationen zum Clusterpreismodell finden Sie unter [Dedizierte Log Analytics-Cluster](manage-cost-storage.md#log-analytics-dedicated-clusters). 

> [!NOTE]
> * Nachdem Sie die *Clusterressource* erstellt haben, können Sie sie mithilfe der PATCH REST-Anforderung mit *sku*, *keyVaultProperties* oder *billingType* aktualisieren.
> * Sie können *billingType* mithilfe einer REST-Anforderung aktualisieren, was derzeit nicht in PowerShell unterstützt wird.

Dieser Vorgang ist asynchron und kann einige Zeit in Anspruch nehmen.

> [!IMPORTANT]
> Kopieren und speichern Sie die Antwort, da Sie die Detail in den nächsten Schritten benötigen werden.
> 

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -Location "region-name" -SkuCapacity daily-ingestion-gigabyte 
```

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

Die Identität wird der *Clusterressource* zum Zeitpunkt der Erstellung zugewiesen.

**Antwort**

200 OK und Header.

Während die Bereitstellung des Log Analytics-Clusters eine Weile dauert, können Sie den Bereitstellungsstatus auf zwei Arten überprüfen:

1. Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
2. Senden Sie eine GET-Anforderung für die *Clusterressource*, und überprüfen Sie den Wert für *provisioningState*. Während der Bereitstellung lautet dieser *ProvisioningAccount*, und nach Abschluss des Vorgangs lautet er *Succeeded*.

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

Die GUID „principalId“ wird vom verwalteten Identitätsdienst für die *Clusterressource* generiert.

### <a name="grant-key-vault-permissions"></a>Erteilen von Key Vault-Berechtigungen

Aktualisieren Sie Ihre Key Vault-Instanz mit einer neuen Zugriffsrichtlinie, um Berechtigungen für Ihre *Clusterressource* zu erteilen. Diese Berechtigungen werden von der zugrunde liegenden Azure Monitor Storage-Instanz für die Datenverschlüsselung verwendet. Öffnen Sie Ihre Key Vault-Instanz im Azure-Portal, und klicken Sie auf „Zugriffsrichtlinien“ und dann auf „+ Zugriffsrichtlinie hinzufügen“, um eine Richtlinie mit den folgenden Einstellungen zu erstellen:

- Schlüsselberechtigungen: Wählen Sie die Berechtigungen „Abrufen“, „Schlüssel packen“ und „Schlüssel entpacken“ aus.
- Prinzipal auswählen: Geben Sie den Namen der *Clusterressource* oder den Wert von „principal-id“ ein, der in der Antwort des vorherigen Schritts zurückgegeben wurde.

![Erteilen von Key Vault-Berechtigungen](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

Die Berechtigung *Abrufen* ist erforderlich, damit sichergestellt werden kann, dass Key Vault als wiederherstellbar konfiguriert ist, um Ihren Schlüssel und den Zugriff auf Ihre Azure Monitor-Daten zu schützen.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Aktualisieren der Clusterressource mit Schlüsselbezeichnerdetails

Dieser Schritt wird während der anfänglichen und bei zukünftigen Aktualisierungen der Schlüsselversion in Ihrer Key Vault-Instanz ausgeführt. Hiermit wird Azure Monitor Storage über die neue Schlüsselversion informiert, die für die Datenverschlüsselung verwendet werden soll. Nach der Aktualisierung wird der neue Schlüssel zum Packen und Entpacken im Speicherschlüssel (AEK) verwendet.

Um die *Clusterressource* mit Details zum *Schlüsselbezeichner* in Key Vault zu aktualisieren, wählen Sie in Azure Key Vault die aktuelle Version Ihres Schlüssels aus, um die Schlüsselbezeichnerdetails abzurufen.

![Erteilen von Key Vault-Berechtigungen](media/customer-managed-keys/key-identifier-8bit.png)

Aktualisieren Sie „KeyVaultProperties“ der *Clusterressource* mit Schlüsselbezeichnerdetails.

Dieser Vorgang ist beim Aktualisieren der Schlüsselbezeichnerdetails asynchron und kann einige Zeit in Anspruch nehmen. Er ist synchron, wenn der Kapazitätswert aktualisiert wird.

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

> [!NOTE]
> Sie können *Clusterressource*, *sku*, *keyVaultProperties* oder *billingType* mithilfe von PATCH aktualisieren.

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
   "identity": { 
     "type": "systemAssigned" 
     },
   "sku": {
     "name": "capacityReservation",
     "capacity": 1000
     },
   "properties": {
    "billingType": "cluster",
     "KeyVaultProperties": {
       "KeyVaultUri": "https://<key-vault-name>.vault.azure.net",
       "KeyName": "<key-name>",
       "KeyVersion": "<current-version>"
       }
   },
   "location":"<region-name>"
}
```

„KeyVaultProperties“ enthält die Details des Key Vault-Schlüsselbezeichners.

**Antwort**

200 OK und Header.
Die Weitergabe des Schlüsselbezeichners dauert einige Minuten. Sie können den Aktualisierungsstatus auf zwei Arten überprüfen:
1. Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
2. Senden Sie eine GET-Anforderung für die *Clusterressource*, und überprüfen Sie die Eigenschaften *KeyVaultProperties*. Die zuletzt aktualisierten Schlüsselbezeichnerdetails sollten in der Antwort zurückgegeben werden.

Nachdem die Aktualisierung des Schlüsselbezeichners abgeschlossen ist, sollte die Antwort auf die GET-Anforderung für die *Clusterressource* wie folgt aussehen:

```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
      },
    "provisioningState": "Succeeded",
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="workspace-association-to-cluster-resource"></a>Arbeitsbereichszuordnung zur *Clusterressource*

Sie müssen über Schreibberechtigungen (write) sowohl für Ihren Arbeitsbereich als auch die *Clusterressource* verfügen, um diesen Vorgang auszuführen. Dies umfasst die folgenden Aktionen:

- Für den Arbeitsbereich: Microsoft.OperationalInsights/workspaces/write
- Für die *Clusterressource*: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Dieser Schritt sollte nur nach Abschluss der Log Analytics-Clusterbereitstellung ausgeführt werden. Wenn Sie vor der Bereitstellung Arbeitsbereiche zuordnen und Daten erfassen, werden die erfassten Daten gelöscht und können nicht wiederhergestellt werden.

Dieser Vorgang ist asynchron und kann einige Zeit in Anspruch nehmen.

```powershell
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name").id
Set-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId
```

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

**Antwort**

200 OK und Header.

Erfasste Daten werden nach dem Zuordnungsvorgang mit Ihrem verwalteten Schlüssel verschlüsselt gespeichert, was bis zu 90 Minuten dauern kann. Sie können den Zuordnungsstatus des Arbeitsbereichs auf zwei Arten überprüfen:

1. Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
2. Senden Sie eine Anforderung [Arbeitsbereiche – Get](/rest/api/loganalytics/workspaces/get) und sehen Sie sich die Antwort an. Zugeordnete Arbeitsbereiche haben eine clusterResourceId unter „Features“.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-08-01
Authorization: Bearer <token>
```

**Antwort**

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

## <a name="cmk-kek-revocation"></a>CMK-Widerruf (KEK)

Sie können den Zugriff auf Daten widerrufen, indem Sie den Schlüssel deaktivieren oder die Zugriffsrichtlinie der *Clusterressource* in Ihrer Key Vault-Instanz löschen. Im Log Analytics-Clusterspeicher werden Änderungen der Schlüsselberechtigungen immer innerhalb einer Stunde oder früher berücksichtigt, und der Speicher steht dann nicht mehr zur Verfügung. Alle neuen Daten, die in Arbeitsbereichen erfasst wurden, die ihrer  *Clusterressource* zugeordnet sind, werden gelöscht und können nicht wiederhergestellt werden, auf die Daten kann nicht zugegriffen werden, und Abfragen an diese Arbeitsbereiche schlagen fehl. Zuvor erfasste Daten verbleiben im Speicher, solange die *Clusterressource* und Ihre Arbeitsbereiche nicht gelöscht werden. Daten, auf die nicht zugegriffen werden kann, unterliegen der Datenaufbewahrungsrichtlinie und werden bereinigt, sobald der Aufbewahrungszeitraum abgelaufen ist. 

Die in den letzten 14 Tagen erfassten Daten werden für einen effizienten Betrieb der Abfrage-Engine auch im Hot-Cache (SSD-gestützt) aufbewahrt. Diese werden beim Schlüsselsperrungsvorgang gelöscht, und es kann dann ebenfalls nicht mehr darauf zugegriffen werden.

Storage fragt Key Vault in regelmäßigen Abständen ab und versucht, den Verschlüsselungsschlüssel zu entpacken. Sobald der Zugriff erfolgt, werden Datenerfassung und Abfrage innerhalb von 30 Minuten fortgesetzt.

## <a name="cmk-kek-rotation"></a>CMK-Rotation (KEK)

Die Rotation des CMK erfordert eine explizite Aktualisierung der *Clusterressource* mit der neuen Schlüsselversion in Azure Key Vault. Befolgen Sie die Anweisungen im Schritt „Aktualisieren der *Clusterressource* mit Schlüsselbezeichnerdetails“. Wenn Sie die neuen Schlüsselbezeichnerdetails in der *Clusterressource* nicht aktualisieren, verwendet der Log Analytics-Clusterspeicher weiterhin den vorherigen Schlüssel zur Verschlüsselung. Wenn Sie den alten Schlüssel deaktivieren oder löschen, bevor Sie den neuen Schlüssel in der *Clusterressource* aktualisieren, wird der Status der [Schlüsselsperrung](#cmk-kek-revocation) aktiv.

Nach der Schlüsselrotation kann auf alle Ihre Daten weiter zugegriffen werden, da Daten immer mit dem Kontoverschlüsselungsschlüssel (Account Encryption Key, AEK) verschlüsselt werden, während AEK nun mit der neuen Version des Schlüsselverschlüsselungsschlüssels (Key Encryption Key, KEK) in Key Vault verschlüsselt wird.

## <a name="cmk-for-queries"></a>CMK für Abfragen

Die in Log Analytics verwendete Abfragesprache ist ausdrucksstark und kann vertrauliche Informationen in Kommentaren enthalten, die Sie Abfragen oder in der Abfragesyntax hinzufügen. Einige Organisationen verlangen, dass diese Informationen als Teil der CMK-Richtlinie geschützt werden, und Sie müssen Ihre Abfragen mit Ihrem Schlüssel verschlüsselt speichern. Azure Monitor ermöglicht Ihnen das Speichern von Abfragen für *gespeicherter Suchvorgänge* und *Protokollwarnungen* mit Verschlüsselung mit Ihrem Schlüssel in Ihrem eigenen Speicherkonto, sofern Sie mit Ihrem Arbeitsbereich verbunden sind. 

> [!NOTE]
> Log Analytics-Abfragen können je nach verwendetem Szenario in verschiedenen Speichern abgelegt werden. Abfragen bleiben unabhängig von der CMK-Konfiguration in den folgenden Szenarios mit von Microsoft verwalteten Schlüsseln (MMK) verschlüsselt: Arbeitsmappen in Azure Monitor, Azure-Dashboards, Azure-Logik-App, Azure Notebooks und Automation Runbooks.

Wenn Sie Bring Your Own Storage (BYOS) verwenden und es Ihrem Arbeitsbereich zuordnen, lädt der Dienst Abfragen für *gespeicherte Suchen* und *Protokollwarnungen* in Ihr Speicherkonto hoch. Dies bedeutet, dass Sie das Speicherkonto und die [Richtlinie für die Verschlüsselung ruhender Daten](../../storage/common/encryption-customer-managed-keys.md) entweder mit dem gleichen Schlüssel steuern, den Sie zum Verschlüsseln von Daten im Log Analytics-Cluster verwenden, oder mit einem anderen Schlüssel. Sie sind jedoch auch für die mit diesem Speicherkonto verbundenen Kosten verantwortlich. 

**Überlegungen vor dem Festlegen von CMK für Abfragen**
* Sie müssen für Ihren Arbeitsbereich und das Speicherkonto über die Berechtigung „Schreiben“ verfügen.
* Stellen Sie sicher, dass Sie Ihr Speicherkonto in derselben Region erstellen, in der sich Ihr Log Analytics-Arbeitsbereich befindet.
* Die *gespeicherten Suchvorgänge* im Speicher werden als Dienstartefakte angesehen und ihr Format kann sich ändern.
* Vorhandene *gespeicherte Suchvorgänge* werden aus dem Arbeitsbereich entfernt. Kopieren Sie alle *gespeicherten Suchvorgänge*, die Sie vor der Konfiguration benötigen. Sie können Ihre *gespeicherten Suchvorgänge* mithilfe von [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch) anzeigen.
* Der Abfrageverlauf wird nicht unterstützt, und Sie können keine Abfragen sehen, die Sie ausgeführt haben.
* Sie können dem Arbeitsbereich ein einzelnes Speicherkonto zum Speichern von Abfragen zuordnen, es kann aber sowohl für Abfragen für *gespeicherte Suchvorgänge* als auch *Protokollwarnungen* verwendet werden.
* Anheften an das Dashboard wird nicht unterstützt.

**Konfigurieren von BYOS für Abfragen für gespeicherte Suchvorgänge**

Ordnen Sie das Speicherkonto für *Abfragen* Ihrem Arbeitsbereich zu. Abfragen für *gespeicherte Suchvorgänge* werden in Ihrem Speicherkonto gespeichert. 

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Query -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Query?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Query", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Nach der Konfiguration werden alle neuen Abfragen *gespeicherter Suchvorgänge* in Ihrem Speicher gespeichert.

**Konfigurieren von BYOS für Abfragen für Protokollwarnungen**

Ordnen Sie das Speicherkonto für *Warnungen* Ihrem Arbeitsbereich zu. Abfragen für *Protokollwarnungen* werden in Ihrem Speicherkonto gespeichert. 

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Alerts -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Alerts?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Alerts", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Nach der Konfiguration werden alle neuen Warnungsabfragen in Ihrem Speicher gespeichert.

## <a name="customer-lockbox-preview"></a>Kunden-Lockbox (Vorschau)
Lockbox bietet Ihnen die Möglichkeit, die Anforderung eines Microsoft-Technikers für den Zugriff auf Ihre Daten während einer Supportanfrage zu genehmigen oder abzulehnen.

In Azure Monitor haben Sie diese Kontrolle über Daten in Arbeitsbereichen, die Ihrem dedizierten Log Analytics-Cluster zugeordnet sind. Die Lockbox-Kontrolle gilt für Daten, die in einem dedizierten Log Analytics-Cluster gespeichert sind, wo sie in Speicherkonten des Clusters unter Ihrem Lockbox-geschützten Abonnement isoliert aufbewahrt werden.  

Weitere Informationen finden Sie unter [Kunden-Lockbox für Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview).

## <a name="cmk-management"></a>CMK-Verwaltung

- **Abrufen aller *Clusterressourcen* für eine Ressourcengruppe**
  
  ```powershell
  Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Antwort**
  
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

- **Abrufen aller *Clusterressourcen* für ein Abonnement**
  
  ```powershell
  Get-AzOperationalInsightsCluster
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
    
  **Antwort**
    
  Dieselbe Antwort wie bei *Clusterressourcen* für eine Ressourcengruppe, jedoch im Abonnementbereich.

- **Aktualisieren der *Kapazitätsreservierung* für eine *Clusterressource***

  Wenn sich das Datenvolumen Ihrer zugeordneten Arbeitsbereiche im Laufe der Zeit ändert und Sie die Kapazitätsreservierungsebene entsprechend aktualisieren möchten. Führen Sie die Schritte zum [Aktualisieren der *Clusterressource*](#update-cluster-resource-with-key-identifier-details) aus, und geben Sie den neuen Kapazitätswert an. Dieser kann im Bereich von 1000 bis 3000 GB pro Tag liegen und in 100er Schritten aktualisiert werden. Wenn Sie mehr als 3000 GB pro Tag benötigen, wenden Sie sich an Ihren Microsoft-Kontakt. Beachten Sie, dass Sie nicht den vollständigen REST-Anforderungstext angeben müssen, aber die SKU einschließen sollten:

  ```powershell
  Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity "daily-ingestion-gigabyte"
  ```

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 1000
    }
  }
  ```

- **Aktualisieren von *billingType* für eine *Clusterressource***

  Die Eigenschaft *billingType* bestimmt die Abrechnungszuordnung für die *Clusterressource* und deren Daten:
  - *Cluster* (Standard): Die Abrechnung wird dem Abonnement zugeordnet, das Ihre Clusterressource hostet.
  - *Arbeitsbereiche*: Die Abrechnung wird den Abonnements zugeordnet, die Ihre Arbeitsbereiche proportional hosten.
  
  Führen Sie die Schritte zum [Aktualisieren der *Clusterressource*](#update-cluster-resource-with-key-identifier-details) aus, und geben Sie den neuen Wert für „billingType“ an. Beachten Sie, dass Sie nicht den vollständigen REST-Anforderungstext angeben müssen und *billingType* einschließen sollten:

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

- **Aufheben der Zuordnung eines Arbeitsbereichs**

  Sie brauchen Schreibberechtigungen für den Arbeitsbereich und die *Clusterressource*, um diesen Vorgang auszuführen. Sie können die Zuordnung eines Arbeitsbereichs zur *Clusterressource* jederzeit aufheben. Neue erfasste Daten werden nach dem Aufheben der Zuordnung im Log Analytics-Speicher gespeichert und mit dem Microsoft-Schlüssel verschlüsselt. Sie können Daten, die vor und nach dem Aufheben der Zuordnung im Arbeitsbereich erfasst wurden, nahtlos abfragen, solange die *Clusterressource* mit gültigem Key Vault-Schlüssel bereitgestellt und konfiguriert wurde.

  Dieser Vorgang ist asynchron und kann einige Zeit in Anspruch nehmen.

  ```powershell
  Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -Name "workspace-name" -LinkedServiceName cluster
  ```

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Antwort**

  200 OK und Header.

  Erfasste Daten werden nach dem Aufheben der Zuordnung im Log Analytics-Speicher gespeichert. Dieser Vorgang kann bis zu 90 Minuten dauern. Sie können den Zuordnungsstatus des Arbeitsbereichs auf zwei Arten überprüfen:

  1. Kopieren Sie den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
  2. Senden Sie eine [GET-Anforderung zum Abrufen von Arbeitsbereichen](/rest/api/loganalytics/workspaces/get), und sehen Sie sich die Antwort an. Für Arbeitsbereiche, deren Zuordnung aufgehoben wurde, wird die *clusterResourceId* nicht unter *features* aufgeführt.

- **Überprüfung des Arbeitsbereichszuordnungs-Status**
  
  Führen Sie den Get-Vorgang für den Arbeitsbereich aus, und beobachten Sie, ob die *clusterResourceId*-Eigenschaft unter *features* in der Antwort vorhanden ist. Der zugeordnete Arbeitsbereich verfügt über die *clusterResourceId*-Eigenschaft.

  ```powershell
  Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
  ```

- **Löschen der *Clusterresource***

  Sie brauchen Schreibberechtigungen für die *Clusterressource*, um diesen Vorgang auszuführen. Es wird ein vorläufiger Löschvorgang durchgeführt, um die Wiederherstellung Ihrer Ressource *Cluster*, einschließlich der Daten, innerhalb von 14 Tagen zu ermöglichen, unabhängig davon, ob der Löschvorgang versehentlich oder gezielt durchgeführt wurde. Der Name der *Clusterressource* bleibt während des Zeitraums des vorläufigen Löschens reserviert, und Sie können keinen neuen Cluster mit diesem Namen erstellen. Nach Ablauf des Zeitraums des vorläufigen Löschens wird der Name der *Clusterressource* freigegeben, und die *Clusterressource* sowie die Daten werden dauerhaft gelöscht und können nicht wiederhergestellt werden. Alle Zuordnungen von Arbeitsbereichen zur *Clusterressource* werden beim Löschvorgang aufgehoben. Neue erfasste Daten werden im Log Analytics-Speicher gespeichert und mit dem Microsoft-Schlüssel verschlüsselt. 
  
  Das Aufheben der Zuordnung von Arbeitsbereichen ist ein asynchroner Vorgang und kann bis zu 90 Minuten dauern.

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Antwort**

  200 – OK

- **Wiederherstellen der *Clusterressource* und Daten** 
  
  Eine *Clusterressource*, die in den letzten 14 Tagen gelöscht wurde, befindet sich im vorläufig gelöschten Zustand und kann zusammen mit den Daten wiederhergestellt werden. Da bei der Löschung der *Clusterressource* die Zuordnung aller Arbeitsbereiche zur *Clusterressource* aufgehoben wurde, müssen Sie die Arbeitsbereiche nach der Wiederherstellung für die CMK-Verschlüsselung neu zuordnen. Dieser Wiederherstellungsvorgang wird derzeit manuell von der Produktgruppe durchgeführt. Verwenden Sie Ihren Microsoft-Kanal für Wiederherstellungsanfragen.

## <a name="limitationsandconstraints"></a>Einschränkungen

- Der CMK wird auf dedizierten Log Analytics-Clustern unterstützt und eignet sich für Kunden, die 1 TB oder mehr pro Tag senden.

- Die maximale Anzahl von *Clusterressourcen* pro Region und Abonnement beträgt 2.

- Sie können Ihrer *Clusterressource* einen Arbeitsbereich zuordnen und dann die Zuordnung aufheben, wenn der CMK für den Arbeitsbereich nicht erforderlich ist. Die Anzahl der Zuordnungen in einem bestimmten Arbeitsbereich ist innerhalb eines Zeitraums von 30 Tagen auf 2 begrenzt.

- Die Arbeitsbereichszuordnung zu einer *Clusterressource* sollte NUR ausgeführt werden, nachdem Sie sichergestellt haben, dass die Log Analytics-Clusterbereitstellung abgeschlossen wurde. Daten, die vor dem Abschluss an Ihren Arbeitsbereich gesendet wurden, werden gelöscht und können nicht wiederhergestellt werden.

- Die CMK-Verschlüsselung gilt für nach der CMK-Konfiguration neu erfasste Daten. Daten, die vor der CMK-Konfiguration erfasst wurden, bleiben mit dem Microsoft-Schlüssel verschlüsselt. Sie können vor und nach der CMK-Konfiguration erfasste Daten nahtlos abfragen.

- Azure Key Vault muss als wiederherstellbar konfiguriert werden. Die folgenden Eigenschaften sind standardmäßig nicht aktiviert und sollten mithilfe der CLI oder PowerShell konfiguriert werden:<br>
  - [Vorläufiges Löschen](../../key-vault/general/soft-delete-overview.md)
  - Der [Bereinigungsschutz](../../key-vault/general/soft-delete-overview.md#purge-protection) sollte aktiviert werden, wenn Sie sich auch nach dem vorläufigen Löschen vor dem erzwungenen Löschen des Geheimnis/Schlüsseltresors schützen möchten.

- Das Verschieben einer *Clusterressource* in eine andere Ressourcengruppe oder ein anderes Abonnement wird derzeit nicht unterstützt.

- Azure Key Vault, *Clusterressource* und zugehörige Arbeitsbereiche müssen sich in derselben Region und in demselben Azure Active Directory (Azure AD)-Mandanten befinden, können jedoch in unterschiedlichen Abonnements enthalten sein.

- Die Arbeitsbereichszuordnung zur *Clusterressource* schlägt fehl, wenn sie einer anderen *Clusterressource* zugeordnet ist.

## <a name="troubleshooting"></a>Problembehandlung

- Verhalten bei Key Vault-Verfügbarkeit
  - Im Normalbetrieb: Der AEK wird für kurze Zeiträume von Storage zwischengespeichert und in regelmäßigen Abständen zum Entpacken in Key Vault zurückgeführt.
    
  - Vorübergehende Verbindungsfehler: Storage handhabt vorübergehende Fehler (Timeouts, Verbindungsfehler, DNS-Probleme), indem Schlüssel für eine kurze Zeit im Cache verbleiben können und dadurch kurze Unterbrechungen der Verfügbarkeit überbrückt werden. Die Abfrage- und Erfassungsfunktionen werden ohne Unterbrechung fortgesetzt.
    
  - Livewebsite: Eine Nichtverfügbarkeit von ungefähr 30 Minuten führt dazu, dass das Speicherkonto nicht mehr zur Verfügung steht. Die Abfragefunktion ist nicht verfügbar, und erfasste Daten werden über mehrere Stunden mithilfe des Microsoft-Key zwischengespeichert, um Datenverluste zu vermeiden. Wenn der Zugriff auf Key Vault wiederhergestellt ist, steht die Abfragefunktion wieder zur Verfügung, und die temporär zwischengespeicherten Daten werden in den Datenspeicher aufgenommen und mit CMK verschlüsselt.

  - Key Vault-Zugriffsrate: Die Häufigkeit, mit der Azure Monitor Storage für Pack- und Entpackvorgänge auf Key Vault zugreift, liegt zwischen 6 und 60 Sekunden.

- Wenn Sie eine *Clusterressource* erstellen und „KeyVaultProperties“ sofort angeben, tritt bei dem Vorgang möglicherweise ein Fehler auf, da die Zugriffsrichtlinie erst definiert werden kann, nachdem die Systemidentität der *Clusterressource* zugewiesen wurde.

- Wenn Sie eine vorhandene *Clusterressource* mit „KeyVaultProperties“ aktualisieren und die Zugriffsrichtlinie für den Schlüsselabruf in Key Vault nicht vorhanden ist, schlägt der Vorgang fehl.

- Wenn beim Erstellen einer *Clusterressource* ein Konfliktfehler auftritt, haben Sie möglicherweise Ihre *Clusterressource* in den letzten 14 Tagen gelöscht und diese befindet sich nun im Zeitraum des vorläufigen Löschens. Der Name der *Clusterressource* bleibt während des Zeitraums des vorläufigen Löschens reserviert, und Sie können keinen neuen Cluster mit diesem Namen erstellen. Der Name wird nach Ablauf des Zeitraums des vorläufigen Löschens freigegeben, wenn die *Clusterressource* dauerhaft gelöscht wird.

- Wenn Sie die Ressource *Cluster* aktualisieren, während ein Vorgang läuft, schlägt der Vorgang fehl.

- Wenn Sie die *Clusterressource* nicht bereitstellen, vergewissern Sie sich, dass sich Azure Key Vault,  *Clusterressource* und zugeordnete Log Analytics-Arbeitsbereiche in der gleichen Region befinden. Sie können in unterschiedlichen Abonnements enthalten sein.

- Wenn Sie die Schlüsselversion in Key Vault aktualisieren und die neuen Schlüsselbezeichnerdetails in der *Clusterressource* nicht aktualisieren, verwendet der Log Analytics-Cluster weiterhin den vorherigen Schlüssel, und auf die Daten kann nicht mehr zugegriffen werden. Aktualisieren Sie neue Schlüsselbezeichnerdetails in der *Clusterressource*, um mit der Datenerfassung fortzufahren und Daten abfragen zu können.

- Einige Vorgänge dauern lange und können einige Zeit in Anspruch nehmen. Dies sind Erstellen des *Clusters*, Aktualisieren des *Clusterschlüssels* und Löschen des *Clusters*. Sie können den Vorgangsstatus auf zwei Arten überprüfen:
  1. Kopieren Sie bei Verwendung von REST den URL-Wert von „Azure-AsyncOperation“ aus der Antwort, und befolgen Sie die [Überprüfung des Status asynchroner Vorgänge](#asynchronous-operations-and-status-check).
  2. Senden Sie eine GET-Anforderung an *Cluster* oder Arbeitsbereich, und beobachten Sie die Antwort. Beispielsweise weist der nicht zugeordnete Arbeitsbereich unter *features* nicht die *clusterResourceId* auf.

- Wenn Sie Unterstützung und Hilfe im Zusammenhang mit kundenseitig verwalteten Schlüsseln benötigen, nutzen Sie Ihre Kontakte zu Microsoft.

- Fehlermeldungen
  
  *Clusterressource* – Erstellen:
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

  *Clusterressource* – Aktualisieren
  -  400 –-Der Cluster befindet sich im Zustand „wird gelöscht“. Asynchroner Vorgang wird ausgeführt. Der Cluster muss seinen Vorgang beenden, bevor ein Aktualisierungsvorgang ausgeführt wird.
  -  400 – „KeyVaultProperties“ ist nicht leer, hat aber ein ungültiges Format. Lesen Sie [Key Identifier Update](#update-cluster-resource-with-key-identifier-details) (Aktualisierung des Schlüsselbezeichners).
  -  400 – Fehler beim Überprüfen des Schlüssels in Key Vault. Mögliche Ursachen: Fehlende Berechtigungen, oder der Schlüssel ist nicht vorhanden. Vergewissern Sie sich, dass Sie die [Schlüssel- und Zugriffsrichtlinie in Key Vault](#grant-key-vault-permissions) festgelegt haben.
  -  400 – Der Schlüssel kann nicht wiederhergestellt werden. Key Vault muss auf „Vorläufiges Löschen und Löschschutz“ festgelegt werden. Lesen Sie die [Dokumentation zu Key Vault](../../key-vault/general/soft-delete-overview.md).
  -  400 – Der Vorgang kann jetzt nicht ausgeführt werden. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.
  -  400 –-Der Cluster befindet sich im Zustand „wird gelöscht“. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.

    *Clusterressource* – Abrufen:
    -  404 – Der Cluster wurde nicht gefunden; möglicherweise wurde er gelöscht. Wenn Sie versuchen, einen Cluster mit diesem Namen zu erstellen, und es zu einem Konflikt kommt, ist der Cluster 14 Tage lang im Zustand „Vorläufig gelöscht“. Sie können den Support kontaktieren, um ihn wiederherzustellen, oder einen anderen Namen zum Erstellen eines neuen Clusters verwenden. 

  *Clusterressource* – Löschen:
    -  409 – Ein Cluster kann im Bereitstellungsstatus nicht gelöscht werden. Warten Sie, bis der asynchrone Vorgang beendet wurde, und versuchen Sie es erneut.

  Arbeitsbereichszuordnung:
  -  404 – Arbeitsbereich nicht gefunden. Der von Ihnen angegebene Arbeitsbereich ist nicht vorhanden oder wurde gelöscht.
  -  409 – Vorgang der Arbeitsbereichszuordnung oder deren Aufhebung läuft.
  -  400 – Der Cluster wurde nicht gefunden, der angegebene Cluster ist nicht vorhanden, oder er wurde gelöscht. Wenn Sie versuchen, einen Cluster mit diesem Namen zu erstellen, und es zu einem Konflikt kommt, ist der Cluster 14 Tage lang im Zustand „Vorläufig gelöscht“. Sie können den Support kontaktieren, um ihn wiederherzustellen.

  Aufhebung der Arbeitsbereichszuordnung:
  -  404 – Arbeitsbereich nicht gefunden. Der von Ihnen angegebene Arbeitsbereich ist nicht vorhanden oder wurde gelöscht.
  -  409 – Vorgang der Arbeitsbereichszuordnung oder deren Aufhebung läuft.
