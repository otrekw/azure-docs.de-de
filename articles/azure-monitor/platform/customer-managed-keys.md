---
title: Konfiguration kundenseitig verwalteter Schlüssel in Azure Monitor
description: Informationen und Schritte zum Konfigurieren kundenseitig verwalteter Schlüssel (Customer-Managed Key, CMK) für das Verschlüsseln von Daten in Ihren Log Analytics-Arbeitsbereichen mithilfe eines Azure Key Vault-Schlüssels.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 02/24/2020
ms.openlocfilehash: b3e110766b2e131330f3108b7938e9e5e01e48a4
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2020
ms.locfileid: "78208558"
---
# <a name="azure-monitor-customer-managed-key-configuration"></a>Konfiguration kundenseitig verwalteter Schlüssel in Azure Monitor 

Dieser Artikel enthält Hintergrundinformationen und Schritte zum Konfigurieren kundenseitig verwalteter Schlüssel (Customer-Managed Key, CMK) für Ihre Log Analytics-Arbeitsbereiche und Application Insights-Komponenten. Nach der Konfiguration werden alle Daten, die an Ihre Arbeitsbereiche oder Komponenten gesendet werden, mit dem Azure Key Vault-Schlüssel verschlüsselt.

Es wird empfohlen, vor der Konfiguration die [Einschränkungen](#limitations-and-constraints) weiter unten zu überprüfen.

## <a name="disclaimers"></a>Haftungsausschlüsse

- Azure Monitor CMK ist ein Feature mit frühzeitigem Zugriff, das für registrierte Abonnements aktiviert ist.

- Die in diesem Artikel beschriebene CMK-Bereitstellung erfolgt in Produktionsqualität und wird als solche unterstützt, obwohl es sich um ein Feature mit frühzeitigem Zugriff handelt.

- Die CMK-Funktion wird in einem dedizierten Datenspeichercluster bereitgestellt. Hierbei handelt es sich um einen ADX-Cluster (Azure Data Explorer), der für Kunden geeignet ist, die mindestens 1 TB pro Tag senden. 

- Das CMK-Preismodell ist derzeit nicht verfügbar und wird in diesem Artikel nicht behandelt. Ein Preismodell für dedizierte ADX-Cluster wird im zweiten Quartal des Kalenderjahres 2020 erwartet und gilt dann für alle bestehenden CMK-Bereitstellungen.

- In diesem Artikel wird die CMK-Konfiguration für Log Analytics-Arbeitsbereiche beschrieben. CMK für Application Insights-Komponenten wird mit diesem Artikel ebenfalls unterstützt, wobei Unterschiede im Anhang aufgeführt sind.

> [!NOTE]
> Log Analytics und Application Insights verwenden die gleiche Datenspeicherplattform und Abfrage-Engine.
> Diese beiden Speicher werden über die Integration von Application Insights in Log Analytics zusammengeführt, um unter Azure Monitor einen einzigen Speicher mit vereinheitlichten Protokollen zu erstellen. Diese Änderung ist für das zweite Quartal des Kalenderjahres 2020 geplant. Wenn Sie CMK bis dahin nicht für Ihre Application Insights-Daten bereitstellen müssen, empfiehlt es sich, bis zum Abschluss der Konsolidierung zu warten, da solche Bereitstellungen durch die Konsolidierung unterbrochen werden und Sie CMK nach der Migration zum Log Analytics-Arbeitsbereich neu konfigurieren müssen. Der Mindestwert von 1 TB pro Tag gilt auf Clusterebene, und bis die Konsolidierung im zweiten Quartal abgeschlossen ist, erfordern Application Insights und Log Analytics separate Cluster.

## <a name="customer-managed-key-cmk-overview"></a>Übersicht über kundenseitig verwaltete Schlüssel (Customer-Managed Key, CMK)

Die [Verschlüsselung ruhender Daten](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) ist eine übliche Datenschutz- und Sicherheitsanforderung in Organisationen. Sie können die Verschlüsselung ruhender Daten vollständig von Azure verwalten lassen, wobei Ihnen verschiedene Optionen zum genauen Verwalten der Verschlüsselung oder Verschlüsselungsschlüssel bereitstehen.

Der Azure Monitor-Datenspeicher stellt sicher, dass alle ruhenden Daten während der Speicherung in Azure Storage mit von Azure verwalteten Schlüsseln verschlüsselt werden. Azure Monitor bietet auch eine Option für die Datenverschlüsselung mithilfe eines eigenen Schlüssels, der in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) gespeichert ist und auf den über die Authentifizierung der systemseitig zugewiesenen [verwaltete Identität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) zugegriffen wird. Dieser Schlüssel kann entweder durch [Software oder Hardware (HSM) geschützt](https://docs.microsoft.com/azure/key-vault/key-vault-overview) werden.
Die Verwendung der Verschlüsselung durch Azure Monitor ist identisch mit der Funktionsweise der [Azure Storage-Verschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption).

Die Häufigkeit, mit der Azure Monitor Storage für Pack- und Entpackvorgänge auf Key Vault zugreift, liegt zwischen 6 und 60 Sekunden. In Azure Monitor Storage werden Änderungen der Schlüsselberechtigungen immer innerhalb einer Stunde berücksichtigt.

Die in den letzten 14 Tagen erfassten Daten werden für einen effizienten Betrieb der Abfrage-Engine auch im Hot-Cache (SSD-gestützt) aufbewahrt. Diese Daten sind unabhängig von der CMK-Konfiguration weiterhin mit Microsoft-Schlüsseln verschlüsselt, doch arbeiten wir daran, dass der SSD Anfang 2020 mit CMK verschlüsselt wird.

## <a name="how-cmk-works-in-azure-monitor"></a>Funktionsweise von CMK in Azure Monitor

Azure Monitor nutzt eine systemseitig zugewiesene verwaltete Identität, um Zugriff auf Azure Key Vault zu gewähren. Eine systemseitig zugewiesene verwaltete Identität kann nur einer einzigen Azure-Ressource zugeordnet werden. Die Identität des Azure Monitor-Datenspeichers (ADX-Cluster) wird auf Clusterebene unterstützt. Dadurch ist vorgegeben, dass die CMK-Funktion in einem dedizierten ADX-Cluster bereitgestellt wird. Zur Unterstützung von CMK in mehreren Arbeitsbereichen wird eine neue Log Analytics-Ressource (*Cluster*) als temporäre Identitätsverbindung zwischen Key Vault und Log Analytics-Arbeitsbereichen eingesetzt. Dieses Konzept entspricht der Einschränkung der systemseitig zugewiesenen Identität, und die Identität wird zwischen dem ADX-Cluster und der Log Analytics-*Clusterressource* aufrechterhalten, während die Daten aller zugeordneten Arbeitsbereiche mit Ihrem Key Vault-Schlüssel geschützt werden. Der zugrunde liegende ADX-Clusterspeicher verwendet die der *Clusterressource* zugeordnete verwaltete Identität für die Authentifizierung und den Zugriff auf Azure Key Vault über Azure Active Directory.

![CMK-Übersicht](media/customer-managed-keys/cmk-overview.png)
1.  Key Vault des Kunden.
2.  Log Analytics-Clusterressource des Kunden mit verwalteter Identität und Berechtigungen für Key Vault. Die Identität wird auf Ebene des Datenspeichers (ADX-Cluster) unterstützt.
3.  Dedizierter ADX-Cluster in Azure Monitor.
4.  Arbeitsbereiche des Kunden, die der Cluster Ressource für die CMK-Verschlüsselung zugeordnet sind.

## <a name="encryption-keys-management"></a>Verwaltung von Verschlüsselungsschlüsseln

An der Speicherdatenverschlüsselung sind drei Arten von Schlüsseln beteiligt:

- **KEK** (Key Encryption Key): Schlüsselverschlüsselungsschlüssel in Key Vault (CMK)
- **AEK** (Account Encryption Key): Kontoverschlüsselungsschlüssel
- **DEK** (Data Encryption Key): Datenverschlüsselungsschlüssel

Es gelten die folgenden Regeln:

- Das ADX-Speicherkonto generiert einen eindeutigen Verschlüsselungsschlüssel für jedes Speicherkonto, der als AEK bezeichnet wird.

- Der AEK dient zum Ableiten von DEKs, bei denen es sich um die Schlüssel handelt, die zum Verschlüsseln der einzelnen, auf den Datenträger geschriebenen Datenblöcke verwendet werden.

- Wenn Sie den Schlüssel in Key Vault konfigurieren und in der *Clusterressource* darauf verweisen, packt Azure Storage den AEK mit Ihrem KEK in Azure Key Vault.

- Der KEK verlässt niemals den Key Vault, und im Fall eines HSM-Schlüssels verlässt dieser niemals die Hardware.

- Azure Storage verwendet die der *Clusterressource* zugeordnete verwaltete Identität für die Authentifizierung und den Zugriff auf Azure Key Vault über Azure Active Directory.

- Bei Lese-/Schreibvorgängen sendet Azure Storage Anforderungen an Azure Key Vault, um den AEK zu packen und zu entpacken und so Verschlüsselungs- und Entschlüsselungsvorgänge auszuführen.

## <a name="cmk-provisioning-procedure"></a>CMK-Bereitstellungsverfahren

Befolgen Sie für die CMK-Konfiguration für Application Insights die Anweisungen im Anhang für die Schritte 3 und 6.

1. Aufnahme in die Abonnement-Whitelist (für dieses Feature mit frühzeitigem Zugriff erforderlich)
2. Erstellen von Azure Key Vault und Speichern des Schlüssels
3. Erstellen einer *Clusterressource*
4. Bereitstellung des Azure Monitor-Datenspeichers (ADX-Cluster)
5. Gewähren von Berechtigungen für Key Vault
6. Zuordnung von Log Analytics-Arbeitsbereichen

Das Verfahren wird derzeit auf der Benutzeroberfläche nicht unterstützt, und der Bereitstellungsprozess erfolgt über die REST-API.

> [!IMPORTANT]
> Jede API-Anforderung muss ein Bearer-Autorisierungstoken im Anforderungsheader enthalten.

Beispiel:

```rst
GET
https://management.azure.com/subscriptions/<subscriptionId>/resourcegroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>?api-version=2015-11-01-preview
Authorization: Bearer eyJ0eXAiO....
```

Hierbei stellt *eyJ0eXAiO....* das vollständige Autorisierungstoken dar. 

Sie können das Token mit einer der folgenden Methoden abrufen:

1. Verwenden Sie die Methode der [App-Registrierungen](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens).

2. Im Azure-Portal
    1. Navigieren Sie zum Azure-Portal im Entwicklertool (F12).
    1. Suchen Sie in einer der „batch?api-version“-Instanzen unter „Anforderungsheader“ nach der Autorisierungszeichenfolge. Diese ähnelt Folgendem: „authorization: Bearer \<Token\>“. 
    1. Kopieren Sie die Zeichenfolge, und fügen Sie sie Ihrem API-Aufruf gemäß den folgenden Beispielen hinzu.

3. Navigieren Sie zur Azure-REST-Dokumentationswebsite. Wählen Sie für eine beliebige API die Option „Ausprobieren“ aus, und kopieren Sie das Bearertoken.

### <a name="subscription-whitelisting"></a>Aufnahme in die Abonnement-Whitelist

Die CMK-Funktion ist ein Feature mit frühzeitigem Zugriff. Die Abonnements, in denen Sie *Clusterressourcen* erstellen möchten, müssen vorab von der Azure-Produktgruppe in die Whitelist aufgenommen werden. Verwenden Sie Ihre Kontakte zu Microsoft zum Angeben Ihrer Abonnement-IDs.

> [!IMPORTANT]
> Die CMK-Funktion ist regional. Azure Key Vault, Speicherkonto, *Clusterressource* und zugehörige Log Analytics-Arbeitsbereiche müssen sich in der gleichen Region befinden, können jedoch in unterschiedlichen Abonnements enthalten sein.

### <a name="storing-encryption-key-kek"></a>Speichern des Verschlüsselungsschlüssels (KEK)

Erstellen Sie eine Azure Key Vault-Ressource, und generieren oder importieren Sie dann einen Schlüssel, der für die Datenverschlüsselung verwendet werden soll.

Azure Key Vault muss als wiederherstellbar konfiguriert werden, um Ihren Schlüssel und den Zugriff auf Ihre Azure Monitor-Daten zu schützen.

Die folgenden Einstellungen sind über die CLI und PowerShell verfügbar:
- [Vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) muss aktiviert werden.
- Der [Bereinigungsschutz](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) sollte aktiviert werden, wenn Sie sich auch nach dem vorläufigen Löschen vor dem erzwungenen Löschen des Geheimnis/Schlüsseltresors schützen möchten.

### <a name="create-cluster-resource"></a>Erstellen einer *Clusterressource*

Diese Ressource wird als temporäre Identitätsverbindung zwischen Key Vault und Ihren Arbeitsbereichen verwendet. Nachdem Sie eine Bestätigung erhalten haben, dass Ihre Abonnements in die Whitelist aufgenommen wurden, erstellen Sie eine Log Analytics-*Clusterressource* in der Region, in der sich Ihre Arbeitsbereiche befinden. Application Insights und Log Analytics erfordern separate Clusterressourcen. Der Typ der *Clusterressource* wird zum Erstellungszeitpunkt definiert, indem die Eigenschaft „clusterType“ entweder auf „LogAnalytics“ oder „ApplicationInsights“ festgelegt wird. Der Typ der Clusterressource kann nicht geändert werden.

Befolgen Sie für die CMK-Konfiguration für Application Insights die Anweisungen im Anhang für diesen Schritt.

**Erstellen**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "location": "<region-name>",
   "properties": {
      "clusterType": "LogAnalytics"
    },
   "identity": {
      "type": "systemAssigned"
   }
}
```
Die Identität wird der *Clusterressource* zum Zeitpunkt der Erstellung zugewiesen.
Der „clusterType“-Wert ist für Application Insights-CMK „ApplicationInsights“.

**Antwort**

202 (Akzeptiert). Hierbei handelt es sich um eine Resource Manager-Standardantwort für asynchrone Vorgänge.

Wenn Sie die *Clusterressource* aus irgendeinem Grund löschen möchten (um sie beispielsweise mit einem anderen Namen oder Clustertyp zu erstellen), verwenden Sie die folgende REST-API:

```rst
DELETE
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
```

### <a name="azure-monitor-data-store-adx-cluster-provisioning"></a>Bereitstellung des Azure Monitor-Datenspeichers (ADX-Cluster)

Während des Zeitraums des frühzeitigen Zugriffs auf das Feature wird der ADX-Cluster manuell vom Produktteam bereitgestellt, sobald die vorherigen Schritte abgeschlossen sind. Verwenden Sie Ihren Microsoft-Kanal, um die Details der *Clusterressource* anzugeben. Kopieren Sie die JSON-Antwort aus der GET REST-API der *Clusterressource*:

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
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
  "properties": {
    "provisioningState": "Succeeded",
    "clusterType": "LogAnalytics", 
    "clusterId": "cluster-id"
    },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
  }
```

„principal-id“ ist eine GUID, die vom Dienst für verwaltete Identitäten für die *Clusterressource* generiert wird.

> [!IMPORTANT]
> Kopieren Sie den Wert „principal-id“, und bewahren Sie ihn auf, da Sie ihn in den nächsten Schritten benötigen.


### <a name="grant-key-vault-permissions"></a>Erteilen von Key Vault-Berechtigungen

> [!IMPORTANT]
> Dieser Schritt muss ausgeführt werden, nachdem Sie über Ihren Microsoft-Kanal eine Bestätigung von der Produktgruppe erhalten haben, dass die Bereitstellung des Azure Monitor-Datenspeichers (ADX-Cluster) abgeschlossen wurde. Wenn Sie die Key Vault-Zugriffsrichtlinie vor dieser Bereitstellung aktualisieren, tritt möglicherweise ein Fehler auf.

Aktualisieren Sie Ihre Key Vault-Instanz mit einer neuen Zugriffsrichtlinie, um Ihrer *Clusterressource* Berechtigungen zu erteilen. Diese Berechtigungen werden von der zugrunde liegenden Azure Monitor Storage-Instanz für die Datenverschlüsselung verwendet.
Öffnen Sie Ihre Key Vault-Instanz im Azure-Portal, und klicken Sie auf „Zugriffsrichtlinien“ und dann auf „+ Zugriffsrichtlinie hinzufügen“, um eine neue Richtlinie mit den folgenden Einstellungen zu erstellen:

- Schlüsselberechtigungen: Wählen Sie die Berechtigungen „Abrufen“, „Schlüssel packen“ und „Schlüssel entpacken“ aus.
- Prinzipal auswählen: Geben Sie den Wert von „principal-id“ ein, der in der Antwort des vorherigen Schritts zurückgegeben wurde.

![Erteilen von Key Vault-Berechtigungen](media/customer-managed-keys/grant-key-vault-permissions.png)

Die Berechtigung *Abrufen* ist erforderlich, damit sichergestellt werden kann, dass Key Vault als wiederherstellbar konfiguriert ist, um Ihren Schlüssel und den Zugriff auf Ihre Azure Monitor-Daten zu schützen.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Aktualisieren der Clusterressource mit Schlüsselbezeichnerdetails

Dieser Schritt gilt für zukünftige Aktualisierungen der Schlüsselversion in Ihrer Key Vault-Instanz. Aktualisieren Sie die *Clusterressource* mit Details zum *Schlüsselbezeichner* in Key Vault, damit Azure Monitor Storage die neue Schlüsselversion verwenden kann. Wählen Sie in Azure Key Vault die aktuelle Version Ihres Schlüssels aus, um die Schlüsselbezeichnerdetails abzurufen.

![Erteilen von Key Vault-Berechtigungen](media/customer-managed-keys/key-identifier-8bit.png)

Aktualisieren Sie „KeyVaultProperties“ der *Clusterressource* mit Schlüsselbezeichnerdetails.

**Aktualisieren**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
   "properties": {
     "KeyVaultProperties": {
       KeyVaultUri: "https://<key-vault-name>.vault.azure.net",
       KeyName: "<key-name>",
       KeyVersion: "<current-version>"
       },
   },
   "location":"<region-name>",
   "identity": { 
     "type": "systemAssigned" 
     }
}
```
„KeyVaultProperties“ enthält die Details des Key Vault-Schlüsselbezeichners.

**Antwort**

```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
  },
  "properties": {
       "KeyVaultProperties": {
            KeyVaultUri: "https://key-vault-name.vault.azure.net",
            KeyName: "key-name",
            KeyVersion: "current-version"
            },
    "provisioningState": "Succeeded",
    "clusterType": "LogAnalytics", 
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="workspace-association-to-cluster-resource"></a>Arbeitsbereichszuordnung zur *Clusterressource*

> [!NOTE]
> Dieser Schritt sollte **NUR** erfolgen, nachdem Sie über Ihren Microsoft-Kanal eine Bestätigung von der Produktgruppe erhalten haben, dass die **Bereitstellung des Azure Monitor-Datenspeichers (ADX-Cluster)** abgeschlossen wurde. Wenn Sie vor dieser **Bereitstellung** Arbeitsbereiche zuordnen und Daten erfassen, werden die Daten gelöscht und können nicht wiederhergestellt werden.

Befolgen Sie für die CMK-Konfiguration für Application Insights die Anweisungen im Anhang für diesen Schritt.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2019-08-01-preview 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```
*clusterDefinitionId* ist der *clusterId*-Wert, der in der Antwort aus dem vorherigen Schritt angegeben wurde.

**Antwort**

```json
{
  "properties": {
    "WriteAccessResourceId": "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name/linkedservices/cluster",
  "name": "workspace-name/cluster",
  "type": "microsoft.operationalInsights/workspaces/linkedServices",
}
```

Nach der Zuordnung werden Daten, die an Ihre Arbeitsbereiche gesendet werden, mit dem verwalteten Schlüssel verschlüsselt gespeichert.

### <a name="workspace-association-verification"></a>Überprüfung der Arbeitsbereichszuordnung
Sie können überprüfen, ob ein Arbeitsbereich einer *Cluster*-Ressource zugeordnet ist, indem Sie sich die [Arbeitsbereiche – Abrufen](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get)-Antwort ansehen. Der zugeordnete Arbeitsbereich verfügt über eine Eigenschaft „clusterResourceId“ mit der *Cluster*-Ressourcen-ID.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview
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
    "retentionInDays": days,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true/false,
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

In Azure Monitor Storage werden Änderungen der Schlüsselberechtigungen immer innerhalb einer Stunde (normalerweise früher) berücksichtigt, und Storage steht dann nicht mehr zur Verfügung. Alle Daten, die in Arbeitsbereichen erfasst wurden, die Ihrer *Clusterressource* zugeordnet sind, werden gelöscht, und Abfragen schlagen fehl. Auf zuvor erfasste Daten in Azure Monitor Storage kann nicht zugegriffen werden, solange der Schlüssel widerrufen ist und Ihre Arbeitsbereiche nicht gelöscht wurden. Daten, auf die nicht zugegriffen werden kann, unterliegen der Datenaufbewahrungsrichtlinie und werden bereinigt, sobald der Aufbewahrungszeitraum abgelaufen ist.

Storage fragt Key Vault in regelmäßigen Abständen ab und versucht, den Verschlüsselungsschlüssel zu entpacken. Sobald der Zugriff erfolgt, werden Datenerfassung und Abfrage innerhalb von 30 Minuten fortgesetzt.

## <a name="cmk-kek-rotation"></a>CMK-Rotation (KEK)

Die Rotation des CMK erfordert eine explizite Aktualisierung der *Clusterressource* mit der neuen Azure Key Vault-Schlüsselversion. Wenn Sie Azure Monitor mit der neuen Schlüsselversion aktualisieren möchten, befolgen Sie die Anweisungen im Schritt „Aktualisieren der *Clusterressource* mit *Schlüsselbezeichnerdetails*“.

Wenn Sie den Schlüssel in Key Vault aktualisieren und die neuen *Schlüsselbezeichnerdetails* in der *Clusterressource* nicht aktualisieren*, verwendet Azure Monitor Storage weiterhin den vorherigen Schlüssel.

## <a name="limitations-and-constraints"></a>Einschränkungen

- Das CMK-Feature wird auf Ebene des ADX-Clusters unterstützt und erfordert einen dedizierten Azure Monitor ADX-Cluster.

- Die maximale Anzahl von *Clusterressourcen* pro Abonnement ist auf fünf begrenzt.

- Die Zuordnung der *Clusterressource* zum Arbeitsbereich sollte NUR ausgeführt werden, nachdem Sie eine Bestätigung von der Produktgruppe erhalten haben, dass die Bereitstellung des ADX-Clusters abgeschlossen wurde. Daten, die vor dieser Bereitstellung gesendet werden, werden gelöscht und können nicht wiederhergestellt werden.

- Die CMK-Verschlüsselung gilt für nach der CMK-Konfiguration neu erfasste Daten. Daten, die vor der CMK-Konfiguration erfasst wurden, bleiben mit dem Microsoft-Schlüssel verschlüsselt. Sie können Daten vor und nach der Konfiguration nahtlos abfragen.

- Sobald der Arbeitsbereich einer *Clusterressource* zugeordnet ist, kann die Zuordnung zur *Clusterressource* nicht aufgehoben werden, da die Daten mit Ihrem Schlüssel verschlüsselt werden und ohne den KEK in Azure Key Vault nicht darauf zugegriffen werden kann.

- Azure Key Vault muss als wiederherstellbar konfiguriert werden. Die folgenden Eigenschaften sind standardmäßig nicht aktiviert und sollten mithilfe der CLI und PowerShell konfiguriert werden:

  - [Vorläufiges Löschen](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) muss aktiviert werden.
  - Der [Bereinigungsschutz](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) sollte aktiviert werden, wenn Sie sich auch nach dem vorläufigen Löschen vor dem erzwungenen Löschen des Geheimnis/Schlüsseltresors schützen möchten.

- Application Insights und Log Analytics erfordern separate *Clusterressourcen*. Der Typ der *Clusterressource* wird zum Erstellungszeitpunkt definiert, indem die Eigenschaft „clusterType“ entweder auf „LogAnalytics“ oder „ApplicationInsights“ festgelegt wird. Der Typ der *Clusterressource* kann nicht geändert werden.

- Das Verschieben einer *Clusterressource* in eine andere Ressourcengruppe oder ein anderes Abonnement wird derzeit nicht unterstützt.

- Azure Key Vault, *Clusterressource* und zugehörige Arbeitsbereiche müssen sich in derselben Region und in demselben Azure Active Directory (Azure AD)-Mandanten befinden, können jedoch in unterschiedlichen Abonnements enthalten sein.

- Die Arbeitsbereichszuordnung zur *Clusterressource* schlägt fehl, wenn sie einer anderen *Clusterressource* zugeordnet ist.

## <a name="troubleshooting-and-management"></a>Problembehandlung und Verwaltung

- Key Vault-Verfügbarkeit
    - Im Normalbetrieb wird der AEK für kurze Zeiträume von Storage zwischengespeichert und in regelmäßigen Abständen zum Entpacken in Key Vault zurückgeführt.
    
    - Vorübergehende Verbindungsfehler. Storage handhabt vorübergehende Fehler (Timeouts, Verbindungsfehler, DNS-Probleme), indem Schlüssel für eine kurze Zeit im Cache verbleiben können und dadurch kurze Unterbrechungen der Verfügbarkeit überbrückt werden. Die Abfrage- und Erfassungsfunktionen werden ohne Unterbrechung fortgesetzt.
    
    - Bei der Livewebsite führt eine Nichtverfügbarkeit von ungefähr 30 Minuten dazu, dass das Speicherkonto nicht mehr zur Verfügung steht. Die Abfragefunktion ist nicht verfügbar, und erfasste Daten werden über mehrere Stunden mithilfe des Microsoft-Key zwischengespeichert, um Datenverluste zu vermeiden. Wenn der Zugriff auf Key Vault wiederhergestellt ist, steht die Abfragefunktion wieder zur Verfügung, und die temporär zwischengespeicherten Daten werden in den Datenspeicher aufgenommen und mit CMK verschlüsselt.

- Wenn Sie eine *Clusterressource* erstellen und „KeyVaultProperties“ sofort angeben, tritt bei dem Vorgang möglicherweise ein Fehler auf, da die Zugriffsrichtlinie erst definiert werden kann, nachdem die Systemidentität der *Clusterressource* zugewiesen wurde.

- Wenn Sie eine vorhandene *Clusterressource* mit „KeyVaultProperties“ aktualisieren und die Zugriffsrichtlinie für den Schlüsselabruf in Key Vault nicht vorhanden ist, schlägt der Vorgang fehl.

- Wenn Sie versuchen, eine *Clusterressource* zu löschen, die einem Arbeitsbereich zugeordnet ist, tritt beim Löschvorgang ein Fehler auf.

- Verwenden Sie diese API, um alle *Cluster*-Ressourcen für eine Ressourcengruppe abzurufen:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2019-08-01-preview
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
        "properties": {
           "KeyVaultProperties": {
              KeyVaultUri: "https://key-vault-name.vault.azure.net",
              KeyName: "key-name",
              KeyVersion: "current-version"
              },
          "provisioningState": "Succeeded",
          "clusterType": "LogAnalytics", 
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

- Verwenden Sie den folgenden API-Aufruf, um alle *Clusterressourcen* für ein Abonnement abzurufen:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2019-08-01-preview
  Authorization: Bearer <token>
  ```
    
  **Antwort**
    
  Dieselbe Antwort wie bei *Clusterressourcen* für eine Ressourcengruppe, jedoch im Abonnementbereich.
    
- Verwenden Sie den folgenden API-Aufruf, um eine *Clusterressource* zu löschen. Sie müssen alle zugeordneten Arbeitsbereiche löschen, bevor Sie die *Clusterressource* löschen können:

  ```rst
  DELETE
  https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
  Authorization: Bearer <token>
  ```

  **Antwort**

  200 – OK


## <a name="appendix"></a>Anhang

CMK (Customer-Managed Key, kundenseitig verwalteter Schlüssel) für Application Insights wird ebenfalls unterstützt. Sie sollten jedoch beim Planen der Bereitstellung von CMK für Application Insight-Komponenten folgende Änderung beachten.

Log Analytics und Application Insights verwenden die gleiche Datenspeicherplattform und Abfrage-Engine. Diese beiden Speicher werden über die Integration von Application Insights in Log Analytics zusammengeführt, um unter Azure Monitor einen einzigen Speicher mit vereinheitlichten Protokollen im zweiten Quartal bereitzustellen.
2020. Durch diese Änderung werden Ihre Application Insight-Daten in Log Analytics-Arbeitsbereiche eingebunden sowie Abfragen, Einblicke und andere Verbesserungen ermöglicht, wobei die Konfiguration von CMK in Ihrem Arbeitsbereich auch auf Ihre Application Insights-Daten angewendet wird.

> [!NOTE]
> Wenn Sie CMK vor der Integration nicht für Ihre Application Insights-Daten bereitstellen müssen, empfiehlt es sich, mit CMK für Application Insights zu warten, da solche Bereitstellungen durch die Integration unterbrochen werden und Sie CMK nach der Migration zum Log Analytics-Arbeitsbereich neu konfigurieren müssen. Der Mindestwert von 1 TB pro Tag gilt auf Clusterebene, und bis die Konsolidierung im zweiten Quartal abgeschlossen ist, erfordern Application Insights und Log Analytics separate Cluster.

## <a name="application-insights-cmk-configuration"></a>CMK-Konfiguration für Application Insights

Die Konfiguration von CMK für Application Insights ist mit dem in diesem Artikel beschriebenen Prozess identisch, einschließlich Einschränkungen und Problembehandlung, jedoch mit Ausnahme der folgenden Schritten:

- Erstellen einer *Clusterressource*

- Zuordnen einer Komponente zu einer *Clusterressource*

Wenn Sie CMK für Application Insights konfigurieren, verwenden Sie die folgenden Schritte anstelle der oben aufgeführten Schritte.

### <a name="create-a-cluster-resource"></a>Erstellen einer *Clusterressource*

Diese Ressource wird als temporäre Identitätsverbindung zwischen Key Vault und Ihren Komponenten verwendet. NACHDEM Sie eine Bestätigung erhalten haben, dass Ihre Abonnements in die Whitelist aufgenommen wurden, erstellen Sie eine Log Analytics-*Clusterressource* in der Region, in der sich Ihre Komponenten befinden. Der Typ der *Clusterressource* wird zum Erstellungszeitpunkt definiert, indem die Eigenschaft *clusterType* entweder auf *LogAnalytics* oder *ApplicationInsights* festgelegt wird. Dieser sollte für Application Insights-CMK *ApplicationInsights* lauten. Die *clusterType*-Einstellung kann nach der Konfiguration nicht geändert werden.

**Erstellen**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "location": "<region-name>",
  "properties": {
      "clusterType":"ApplicationInsights"
  },
  "identity": {
      "type": "systemAssigned"
  }
}
```

**Antwort**

Die Identität wird der *Clusterressource* zum Zeitpunkt der Erstellung zugewiesen.

```json

{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
  },
  "properties": {
    "provisioningState": "Succeeded",
    "clusterType": "ApplicationInsights",    //The value is ‘ApplicationInsights’ for Application Insights CMK
    "clusterId": "cluster-id" 
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name", //The cluster resource Id
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```
„principle-id“ ist eine GUID, die vom Dienst für verwaltete Identitäten generiert wurde.

> [!IMPORTANT]
> Kopieren Sie den Wert von „principle-id“, und bewahren Sie ihn auf, da Sie ihn in den nächsten Schritten benötigen.

### <a name="associate-a-component-to-a-cluster-resource-using-components---create-or-update-api"></a>Zuordnen einer Komponente zu einer *Clusterressource* mithilfe der API für [Komponenten: Erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/application-insights/components/createorupdate)

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Insights/components/<component-name>?api-version=2015-05-01
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "clusterDefinitionId": "cluster-id"
  },
  "location": "<region-name>",
  "kind": "<component-type>"
}
```
„clusterDefinitionId“ ist der „clusterId“-Wert, der in der Antwort aus dem vorherigen Schritt bereitgestellt wurde.
„kind“-Beispiel ist „web“.

**Antwort**

```json
{
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.insights/components/component-name",
  "name": "component-name",
  "type": "Microsoft.Insights/components",
  "location": "region-name",
  "tags": "",
  "kind": "",
  "properties": {
    "clusterDefinitionId": "cluster-id"
    "ApplicationId": "",
    "AppId": "",
    "Application_Type": "",
    "Flow_Type": "",
    "Request_Source": "",
    "InstrumentationKey": "",
    "CreationDate": "",
    "TenantId": "",
    "HockeyAppId": "",
    "HockeyAppToken": "",
    "provisioningState": "",
    "SamplingPercentage":,
    "RetentionInDays":,
    "ConnectionString": "",
    "DisableIpMasking":,
    "ImmediatePurgeDataOn30Days": 
  }
}
```
„clusterDefinitionId“ ist die *Cluster*-Ressourcen-ID, die dieser Komponente zugeordnet ist.

Nach der Zuordnung werden Daten, die an Ihre Komponenten gesendet werden, mit dem verwalteten Schlüssel verschlüsselt gespeichert.
