---
title: Erstellen eines Wörterbuchs für Clusterfehler
description: Erfahren Sie, wie Sie ein Wörterbuch für Clusterfehler erstellen.
ms.reviewer: hrasheed
author: karkrish
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: troubleshooting
ms.date: 11/19/2019
ms.author: v-todmc
ms.openlocfilehash: e537014f741196024c24dd6ee98af0d8af2e1b31
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475932"
---
# <a name="hdinsight-cluster-creation-errors"></a>HDInsight: Fehler bei der Clustererstellung

In diesem Artikel werden Lösungen für Fehler beschrieben, die beim Erstellen von Clustern auftreten. 

> [!NOTE]
> Die ersten drei Fehler in der folgenden Liste treten aufgrund von Validierungsfehlern auf, wenn die CsmDocument_2_0-Klasse in einem HDInsight-Produkt verwendet wird.



## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error-script-action-location-cannot-be-accessed-uriscript-action-url"></a>**Fehler:** Script Action location cannot be accessed URI:\<SCRIPT ACTION URL\> (Zugriff auf Speicherort der Skriptaktion nicht möglich. URI: <URL DER SKRIPTAKTION>)

**Fehlermeldung: Der Remoteserver hat einen Fehler zurückgegeben: (404) Nicht gefunden.**

### <a name="cause"></a>Ursache
Der Zugriff auf die URL der Skriptaktion, die als Teil der Anforderung zum Erstellen eines Clusters angegeben wird, ist im HDInsight-Dienst nicht möglich. Die Fehlermeldung wird beim Versuch angezeigt, auf die Skriptaktion zuzugreifen.

### <a name="resolution"></a>Lösung 
  - Bei einer HTTP/HTTPS-URL können Sie dies überprüfen, indem Sie die URL im Inkognitomodus des Browsers öffnen. 
  - Stellen Sie bei einer WASB-URL sicher, dass das Skript in dem in der Anforderung angegebenen Speicherkonto vorhanden ist. Vergewissern Sie sich, dass der Speicherschlüssel für das Speicherkonto korrekt ist. 
  - Stellen Sie bei einer ADLS-URL sicher, dass das Skript im Speicherkonto vorhanden ist.

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error-script-action-location-cannot-be-accessed-uri-script-action-url"></a>**Fehler:** Script Action location cannot be accessed URI: \<SCRIPT ACTION URL\> (Zugriff auf Speicherort der Skriptaktion nicht möglich. URI: <URL DER SKRIPTAKTION>)

**Fehlermeldung: The given script URI \<SCRIPT URI\> is in ADLS, but this cluster has no data lake storage principal** (Der angegebene Skript-URI <SKRIPT-URI> befindet sich in ADLS, dieser Cluster hat jedoch keinen Data Lake Storage-Prinzipal)

### <a name="cause"></a>Ursache
Der Zugriff auf die URL der Skriptaktion, die als Teil der Anforderung zum Erstellen eines Clusters angegeben wird, ist im HDInsight-Dienst nicht möglich. Die Fehlermeldung wird beim Versuch angezeigt, auf die Skriptaktion zuzugreifen. 

### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass das entsprechende Azure Data Lake Store Gen1-Konto im Cluster hinzugefügt ist. Der Dienstprinzipal, der für den Zugriff auf das Azure Data Lake Store Gen1-Konto verwendet wird, wird dem Cluster ebenfalls hinzugefügt. 

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error-vm-size-customer-specified-vm-size-provided-in-the-request-is-invalid-or-not-supported-for-role-role-valid-values-are-valid-vm-size-for-role"></a>**Fehler:** Die in der Anforderung angegebene VM-Größe \<VOM KUNDEN ANGEGEBENE VM-GRÖSSE\> ist ungültig oder wird für die Rolle \<ROLLE\> nicht unterstützt. Gültige Werte sind: \<GÜLTIGE VM-GRÖSSE FÜR ROLLE\>.

### <a name="cause"></a>Ursache
Die vom Kunden angegebenen VM-Größen sind für die Rolle nicht zulässig. Dies kann der Fall sein, da der VM-Größenwert nicht wie erwartet funktioniert oder nicht für die Computerrolle geeignet ist. 

### <a name="resolution"></a>Lösung
In der Fehlermeldung sind die gültigen Werte für die VM-Größe aufgeführt. Wählen Sie einen der gültigen Werte aus, und wiederholen Sie die Anforderung zum Erstellen eines Clusters. 

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Fehlercode: InvalidVirtualNetworkId  

### <a name="error-the-virtualnetworkid-is-not-valid-virtualnetworkid-user_virtualnetworkid"></a>**Fehler:** Die VirtualNetworkId ist ungültig. VirtualNetworkId: „\<BENUTZER_VIRTUALNETWORKID\>“*

### <a name="cause"></a>Ursache
Der bei der Clustererstellung angegebene Wert für **VirtualNetworkId** weist nicht das richtige Format auf. 

### <a name="resolution"></a>Lösung
Stellen Sie sicher, dass **VirtualNetworkId** und das Subnetz im richtigen Format angegeben sind. Wählen Sie zum Abrufen des Werts für **VirtualnetworkId** im Azure-Portal Ihr virtuelles Netzwerk und dann im Menü die Option **Eigenschaften** aus. Die **ResourceID**-Eigenschaft ist der Wert für **VirtualNetworkId**. 

Beispiel für die ID eines virtuellen Netzwerks: 

„/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet“ 

---

## <a name="error-code-customizationfailederrorcode"></a>Fehlercode: CustomizationFailedErrorCode

### <a name="error-cluster-deployment-failed-due-to-an-error-in-the-custom-script-action-failed-actions-script_name-please-go-to-ambari-ui-to-further-debug-the-failure"></a>**Fehler:** Fehler bei Clusterbereitstellung aufgrund eines Fehlers in der benutzerdefinierten Skriptaktion. Failed Actions: \<SCRIPT_NAME\>, Please go to Ambari UI to further debug the failure. (<SKRIPTNAME>. Debuggen Sie den Fehler auf der Ambari-Benutzeroberfläche.)

### <a name="cause"></a>Ursache
Das benutzerdefinierte Skript des Benutzers, das bei der Anforderung zum Erstellen eines Clusters angegeben wurde, wird nach der erfolgreichen Bereitstellung des Clusters ausgeführt. Dieser Fehlercode gibt an, dass bei der Ausführung des benutzerdefinierten Skripts \<SKRIPTNAME\> ein Fehler aufgetreten ist.   

### <a name="resolution"></a>Lösung
Da es sich um das benutzerdefinierte Skript des Benutzers handelt, muss der Benutzer das Problem beheben und das Skript bei Bedarf erneut ausführen. Überprüfen Sie die Protokolle im Ordner „/var/lib/ambari-agent/*“, um den Skriptfehler zu beheben. Alternativ können Sie auch die Seite „Operations“ (Vorgänge) auf der Ambari-Benutzeroberfläche öffnen und dann den Vorgang **run_customscriptionaction** auswählen, um die Fehlerdetails anzuzeigen. 

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Fehlercode: InvalidDocumentErrorCode

### <a name="error-the-meta_store_type-metastore-schema-version-metastore_major_version-in-database-database_name-is-incompatible-with-cluster-version-cluster_version"></a>**Fehler:** The \<META_STORE_TYPE\> Metastore schema version \<METASTORE_MAJOR_VERSION\> in database \<DATABASE_NAME\> is incompatible with cluster version \<CLUSTER_VERSION\> (Die Schemaversion <METASTORE_HAUPTVERSION> des Metastores <METASTORETYP> in der Datenbank <DATABANKNAME> ist nicht kompatibel mit der Clusterversion <CLUSTERVERSION>)

### <a name="cause"></a>Ursache
Der benutzerdefinierte Metastore ist nicht mit der ausgewählten HDInsight-Clusterversion kompatibel. Derzeit unterstützen HDInsight 4.0-Cluster nur die Metastore-Version 3.*x*, und HDInsight 3.6 unterstützt die Metastore-Version 3.*x* oder höher nicht. 

### <a name="resolution"></a>Lösung
Stellen Sie sicher, dass nur Metastore-Versionen verwendet werden, die in jeder HDInsight-Clusterversion unterstützt werden. Beachten Sie, dass bei fehlender Angabe eines benutzerdefinierten Metastores in HDInsight intern ein Metastore erstellt wird. Dieser Metastore wird jedoch beim Löschen des Clusters automatisch gelöscht. 

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Fehlercode: FailedToConnectWithClusterErrorCode 

### <a name="error-unable-to-connect-to-cluster-management-endpoint-to-perform-scaling-operation-verify-that-network-security-rules-are-not-blocking-external-access-to-the-cluster-and-that-the-cluster-manager-ambari-ui-can-be-successfully-accessed"></a>**Fehler:** Unable to connect to cluster management endpoint to perform scaling operation. Verify that network security rules are not blocking external access to the cluster, and that the cluster manager (Ambari) UI can be successfully accessed. (Zum Ausführen des Skalierungsvorgangs kann keine Verbindung mit dem Clusterverwaltungsendpunkt hergestellt werden. Vergewissern Sie sich, dass der externe Zugriff auf den Cluster nicht durch Netzwerksicherheitsregeln gesperrt ist und dass der Zugriff auf die Cluster Manager-Benutzeroberfläche (Ambari) möglich ist.)

### <a name="cause"></a>Ursache
Eine Firewallregel in der Netzwerksicherheitsgruppe (NSG) blockiert die Kommunikation des Clusters mit wichtigen Integritäts- und Verwaltungsdiensten in Azure. 

### <a name="resolution"></a>Lösung
Wenn Sie **Netzwerksicherheitsgruppen**  zum Steuern des Netzwerkdatenverkehrs verwenden möchten, sollten Sie vor dem Installieren von HDInsight die folgenden Vorgänge durchführen: 
  - Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten. 
  - Identifizieren Sie die IP-Adressen, die für HDInsight erforderlich sind. Weitere Informationen finden Sie unter [HDInsight management IP addresses (IP-Adressen für die HDInsight-Verwaltung)](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
    - Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten. 
    - **Netzwerksicherheitsgruppen:** Lassen Sie  **eingehenden** Datenverkehr ** über Port**443  von den IP-Adressen zu. Dadurch wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können. 

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Fehlercode: StoragePermissionsBlockedForMsi  

### <a name="error-the-managed-identity-does-not-have-permissions-on-the-storage-account-please-verify-that-storage-blob-data-owner-role-is-assigned-to-the-managed-identity-for-the-storage-account-storage-subscriptions-subscription-id-resourcegroups-resource-group-name-providersmicrosoftstoragestorageaccounts-storage-account-name-managed-identity-subscriptions-subscription-id-resourcegroups--resource-group-name-providersmicrosoftmanagedidentityuserassignedidentities-user-managed-identity-name"></a>**Fehler:** The Managed Identity does not have permissions on the storage account. Please verify that 'Storage Blob Data Owner' role is assigned to the Managed Identity for the storage account. Storage: /subscriptions/ \<Subscription ID\> /resourceGroups/\< Resource Group Name\> /providers/Microsoft.Storage/storageAccounts/ \<Storage Account Name\>, Managed Identity: /subscriptions/ \<Subscription ID\> /resourceGroups/ /\< Resource Group Name\> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ \<User Managed Identity Name\> (Die verwaltete Identität verfügt nicht über Berechtigungen für das Speicherkonto. Überprüfen Sie, ob der verwalteten Identität die Rolle „Besitzer von Speicherblobdaten“ für das Speicherkonto zugewiesen ist. Speicher: /subscriptions/ <Abonnement-ID> /resourceGroups/ <Ressourcengruppenname> /providers/Microsoft.Storage/storageAccounts/ <Speicherkontoname>, Verwaltete Identität: /subscriptions/ <Abonnement-ID> /resourceGroups/ /<Ressourcengruppenname> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ <Name des Benutzers der benutzerseitig zugewiesenen verwalteten Identität>)

### <a name="cause"></a>Ursache
Die erforderlichen Berechtigungen wurden für die **verwaltete Identität** nicht angegeben. Der **benutzerseitig zugewiesenen verwalteten Identität** wurde die Rolle „Mitwirkender an Storage-Blobdaten“ für das ADLS Gen2-Speicherkonto nicht zugewiesen. 

### <a name="resolution"></a>Lösung

Öffnen Sie das Azure-Portal, navigieren Sie zu Ihrem Speicherkonto, und vergewissern Sie sich unter **Zugriffssteuerung (IAM)** , dass eine der Rollen „Mitwirkender an Storage-Blobdaten“ oder „Besitzer von Speicherblobdaten“ Zugriff auf die  **benutzerseitig zugewiesene verwaltete Identität** für das Abonnement hat. Weitere Informationen finden Sie unter [Festlegen von Berechtigungen für verwaltete Identitäten im Data Lake Storage Gen2-Konto](hdinsight-hadoop-use-data-lake-storage-gen2.md). 

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Fehlercode: InvalidNetworkSecurityGroupSecurityRules  

### <a name="error-the-security-rules-in-the-network-security-group-subscriptionssubscriptionidresourcegroupsresource-group-name-defaultprovidersmicrosoftnetworknetworksecuritygroupsnetwork-security-group-name-configured-with-subnet-subscriptionssubscriptionidresourcegroupsresource-group-name-rg-westeurope-vnet-tomtom-defaultprovidersmicrosoftnetworkvirtualnetworksvirtual-network-namesubnetssubnet-name"></a>**Fehler:** The security rules in the Network Security Group /subscriptions/\<SubscriptionID>\/resourceGroups/<Resource Group name> default/providers/Microsoft.Network/networkSecurityGroups/\<Network Security Group Name\> configured with subnet /subscriptions/\<SubscriptionID>\/resourceGroups/\<Resource Group name> RG-westeurope-vnet-tomtom-default\/providers/Microsoft.Network/virtualNetworks/\<Virtual Network Name>\/subnets/\<Subnet Name\> 
does not allow required inbound and/or outbound connectivity. (Die Sicherheitsregeln in der Netzwerksicherheitsgruppe /subscriptions/<Abonnement-ID>/resourceGroups/default/providers/Microsoft.Network/networkSecurityGroups/<Name der Netzwerksicherheitsgruppe>, die mit dem Subnetz /subscriptions/<Abonnement-ID>/resourceGroups/<Ressourcengruppenname> RG-westeurope-vnet-tomtom-default/providers/Microsoft.Network/virtualNetworks/<Name des virtuellen Netzwerks>/subnets/<Subnetzname> konfiguriert ist, lassen die erforderliche eingehende und/oder ausgehende Verbindung nicht zu.) Weitere Informationen finden Sie unter [Planen eines virtuellen Netzwerks für Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment), oder wenden Sie sich an den Support.**

### <a name="cause"></a>Ursache
Wenn Sie Netzwerksicherheitsgruppen (NSG) oder benutzerdefinierte Routen (UDRs) zum Steuern von eingehendem Datenverkehr im HDInsight-Cluster verwenden, müssen Sie sicherstellen, dass der Cluster mit wichtigen Integritäts- und -Verwaltungsdiensten in Azure kommunizieren kann.

### <a name="resolution"></a>Lösung
Wenn Sie **Netzwerksicherheitsgruppen**  zum Steuern des Netzwerkdatenverkehrs verwenden möchten, sollten Sie vor dem Installieren von HDInsight die folgenden Vorgänge durchführen: 
  - Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten, und erstellen Sie eine Liste mit sicheren IP-Adressen für die Region: [Integritäts- und Verwaltungsdienste: Bestimmte Regionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions).
  - Identifizieren Sie die IP-Adressen, die für HDInsight erforderlich sind. Weitere Informationen finden Sie unter  [HDInsight-Verwaltungs-IP-Adressen](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses). 
  - Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten. **Netzwerksicherheitsgruppen:** Lassen Sie eingehenden Datenverkehr über Port **443** für die IP-Adressen zu. Dadurch wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können.
  
---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Fehlercode: Cluster setup failed to install components on one or more hosts (Bei der Clustereinrichtung konnten Komponenten auf mindestens einem Host nicht erstellt werden.)

###  <a name="errorcluster-setup-failed-to-install-components-on-one-or-more-hosts-please-retry-your-request"></a>**Fehler:**  Cluster setup failed to install components on one or more hosts. (Bei der Clustereinrichtung konnten Komponenten auf mindestens einem Host nicht erstellt werden.) Bitte wiederholen Sie die Anforderung. 

### <a name="cause"></a>Ursache 
Dieser Fehler wird in der Regel generiert, wenn ein vorübergehendes Problem vorliegt oder ein Azure-Ausfall auftritt. 

### <a name="resolution"></a>Lösung

Überprüfen Sie die Seite [Azure-Status](https://status.azure.com/status) auf mögliche Azure-Ausfälle, die sich auf die Clusterbereitstellung auswirken können. Wenn keine Ausfälle angegeben sind, führen Sie die Clusterbereitstellung erneut durch. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung bei der Clustererstellung finden Sie unter [Behandeln von Fehlern bei der Clustererstellung mit Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails).
