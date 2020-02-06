---
title: Erstellen eines Wörterbuchs für Clusterfehler
description: Erfahren Sie, wie Sie ein Wörterbuch für Clusterfehler erstellen.
author: karkrish
ms.author: v-todmc
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/19/2019
ms.openlocfilehash: 6e5ed996a0f44bae6c37027bc01f30be85d164f9
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905481"
---
# <a name="azure-hdinsight-cluster-creation-errors"></a>Azure HDInsight: Fehler bei der Clustererstellung

In diesem Artikel werden Lösungen für Fehler beschrieben, die beim Erstellen von Clustern auftreten können.

> [!NOTE]
> Bei den ersten drei Fehlern, die in diesem Artikel beschrieben werden, handelt es sich um Überprüfungsfehler. Sie können auftreten, wenn ein Azure HDInsight-Produkt die **CsmDocument_2_0**-Klasse verwendet.

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error"></a>Fehler

„Script Action location cannot be accessed URI:\<SCRIPT ACTION URL\> (Zugriff auf Speicherort der Skriptaktion nicht möglich. URI: <URL DER SKRIPTAKTION>)“

#### <a name="error-message"></a>Fehlermeldung

Der Remoteserver hat einen Fehler zurückgegeben: (404) Nicht gefunden.“

### <a name="cause"></a>Ursache

Der HDInsight-Dienst kann nicht auf die URL der Skriptaktion zugreifen, die Sie in der Anforderung zum Erstellen eines Clusters angegeben haben. Der Dienst empfängt die oben angegebene Fehlermeldung beim Versuch, auf die Skriptaktion zuzugreifen.

### <a name="resolution"></a>Lösung

- Bei einer HTTP- oder HTTPS-URL überprüfen Sie die URL, indem Sie sie im Inkognitomodus eines Browsers öffnen.
- Stellen Sie bei einer WASB-URL sicher, dass das Skript in dem in der Anforderung angegebenen Speicherkonto vorhanden ist. Vergewissern Sie sich ebenfalls, dass der Speicherschlüssel für das Speicherkonto korrekt ist.
- Stellen Sie bei einer ADLS-URL sicher, dass das Skript im Speicherkonto vorhanden ist.

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error"></a>Fehler

„Script Action location cannot be accessed URI: (Zugriff auf Speicherort der Skriptaktion nicht möglich. URI:) \<SCRIPT_ACTION_URL\> (<URL_DER_SKRIPTAKTION>)“

#### <a name="error-message"></a>Fehlermeldung

„The given script URI \<SCRIPT_URI\> is in ADLS, but this cluster has no data lake storage principal (Der angegebene Skript-URI <SKRIPT_URI> befindet sich in ADLS, dieser Cluster hat jedoch keinen Data Lake Storage-Prinzipal.)“

### <a name="cause"></a>Ursache

Der HDInsight-Dienst kann nicht auf die URL der Skriptaktion zugreifen, die Sie in der Anforderung zum Erstellen eines Clusters angegeben haben. Der Dienst empfängt die oben angegebene Fehlermeldung beim Versuch, auf die Skriptaktion zuzugreifen.

### <a name="resolution"></a>Lösung

Fügen Sie dem Cluster das entsprechende Azure Data Lake Store Gen1-Konto hinzu. Fügen Sie dem Cluster auch den Dienstprinzipal hinzu, der auf das Data Lake Storage Gen1-Konto zugreift.

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>Fehlercode: Fehler beim Überprüfen von DeploymentDocument „CsmDocument_2_0“

### <a name="error"></a>Fehler

„VM size \<CUSTOMER_SPECIFIED_VM_SIZE\> provided in the request is invalid or not supported for role \<ROLE\>. (Die in der Anforderung angegebene VM-Größe <VOM_KUNDEN_ANGEGEBENE_VM-GRÖSSE> ist ungültig oder wird für die Rolle <ROLLE> nicht unterstützt.) Gültige Werte sind: \<VALID_VM_SIZE_FOR_ROLE\>. (<GÜLTIGE_VM-GRÖSSE_FÜR_ROLLE>.)“

### <a name="cause"></a>Ursache

Die von Ihnen angegebene VM-Größe ist für die Rolle nicht zulässig. Dieser Fehler kann auftreten, wenn der Wert für die VM-Größe nicht erwartungsgemäß funktioniert oder für die Computerrolle nicht geeignet ist.

### <a name="resolution"></a>Lösung

In der Fehlermeldung sind die gültigen Werte für die VM-Größe aufgeführt. Wählen Sie einen dieser Werte aus, und wiederholen Sie die Anforderung zum Erstellen eines Clusters.

---

## <a name="error-codeinvalidvirtualnetworkid"></a>Fehlercode: InvalidVirtualNetworkId  

### <a name="error"></a>Fehler

„Die VirtualNetworkId ist ungültig. VirtualNetworkId: \<BENUTZER_VIRTUALNETWORKID\>*“

### <a name="cause"></a>Ursache

Der bei der Clustererstellung angegebene Wert für **VirtualNetworkId** weist nicht das richtige Format auf.

### <a name="resolution"></a>Lösung

Stellen Sie sicher, dass **VirtualNetworkId** und die Subnetzwerte im richtigen Format angegeben sind. So erhalten Sie den Wert für **VirtualNetworkId**:

1. Öffnen Sie das Azure-Portal.
1. Wählen Sie Ihr virtuelles Netzwerk aus.
1. Wählen Sie das Menüelement **Eigenschaften** aus. Der **ResourceID**-Eigenschaftswert entspricht dem Wert für **VirtualNetworkId**.

Hier ein Beispiel für die ID eines virtuellen Netzwerks:

„/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet“

---

## <a name="error-code-customizationfailederrorcode"></a>Fehlercode: CustomizationFailedErrorCode

### <a name="error"></a>Fehler

„Cluster deployment failed due to an error in the custom script action. (Fehler bei Clusterbereitstellung aufgrund eines Fehlers in der benutzerdefinierten Skriptaktion.) Failed Actions: \<SCRIPT_NAME\>, Please go to Ambari UI to further debug the failure. (<SKRIPTNAME>. Debuggen Sie den Fehler auf der Ambari-Benutzeroberfläche.)“

### <a name="cause"></a>Ursache

Das benutzerdefinierte Skript, das Sie bei der Anforderung zum Erstellen eines Clusters angegeben haben, wird nach der erfolgreichen Bereitstellung des Clusters ausgeführt. Dieser Fehlercode gibt an, dass während der Ausführung des benutzerdefinierten Skripts mit dem Namen \<SKRIPTNAME\> ein Fehler aufgetreten ist.

### <a name="resolution"></a>Lösung

Da es sich bei dem Skript um Ihr benutzerdefiniertes Skript handelt, empfiehlt es sich, das Problem zu beheben und das Skript bei Bedarf erneut auszuführen. Überprüfen Sie die Protokolle im Ordner „/var/lib/ambari-agent/*“, um den Skriptfehler zu beheben. Alternativ können Sie auch die Seite **Operations** (Vorgänge) auf der Ambari-Benutzeroberfläche öffnen und dann den Vorgang **run_customscriptaction** auswählen, um die Fehlerdetails anzuzeigen.

---

## <a name="error-codeinvaliddocumenterrorcode"></a>Fehlercode: InvalidDocumentErrorCode

### <a name="error"></a>Fehler

„The \<META_STORE_TYPE\> Metastore schema version \<METASTORE_MAJOR_VERSION\> in database \<DATABASE_NAME\> is incompatible with cluster version \<CLUSTER_VERSION\> (Die Schemaversion <METASTORE_HAUPTVERSION> des Metastores <METASTORETYP> in der Datenbank <DATENBANKNAME> ist mit der Clusterversion <CLUSTERVERSION> nicht kompatibel.)“

### <a name="cause"></a>Ursache

Der benutzerdefinierte Metastore ist nicht mit der ausgewählten HDInsight-Clusterversion kompatibel. HDInsight 4.0-Cluster unterstützen derzeit nur Metastore, Version 3.0 und höher, während HDInsight 3.6-Cluster keine Unterstützung für Metastore, Version 3.0 und höher, bieten.

### <a name="resolution"></a>Lösung

Verwenden Sie nur Metastore-Versionen, die Ihre HDInsight-Clusterversion unterstützt. Wenn Sie keinen benutzerdefinierten Metastore angeben, erstellt HDInsight intern einen Metastore und löscht diesen beim Löschen des Clusters.

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>Fehlercode: FailedToConnectWithClusterErrorCode 

### <a name="error"></a>Fehler

„Unable to connect to cluster management endpoint to perform scaling operation. Verify that network security rules are not blocking external access to the cluster, and that the cluster manager (Ambari) UI can be successfully accessed. (Zum Ausführen des Skalierungsvorgangs kann keine Verbindung mit dem Clusterverwaltungsendpunkt hergestellt werden. Vergewissern Sie sich, dass der externe Zugriff auf den Cluster nicht durch Netzwerksicherheitsregeln blockiert ist und dass der Zugriff auf die Cluster Manager-Benutzeroberfläche [Ambari] möglich ist.)“

### <a name="cause"></a>Ursache

Eine Firewallregel in der Netzwerksicherheitsgruppe (NSG) blockiert die Kommunikation des Clusters mit wichtigen Integritäts- und Verwaltungsdiensten in Azure.

### <a name="resolution"></a>Lösung

Wenn Sie Netzwerksicherheitsgruppen zum Steuern des Netzwerkdatenverkehrs verwenden möchten, führen Sie vor der Installation von HDInsight folgende Aktionen aus:

- Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten.
- Identifizieren Sie die IP-Adressen, die für HDInsight erforderlich sind. Weitere Informationen finden Sie unter [HDInsight management IP addresses (IP-Adressen für die HDInsight-Verwaltung)](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).
  - Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten.
  - Lassen Sie für Netzwerksicherheitsgruppen eingehenden Datenverkehr über Port 443 für die IP-Adressen zu. Durch diese Konfiguration wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können.

---

## <a name="error-code-storagepermissionsblockedformsi"></a>Fehlercode: StoragePermissionsBlockedForMsi  

### <a name="error"></a>Fehler

„The Managed Identity does not have permissions on the storage account. (Die verwaltete Identität weist keine Berechtigungen für das Speicherkonto auf.) Please verify that 'Storage Blob Data Owner' role is assigned to the Managed Identity for the storage account. Storage: /subscriptions/ \<Subscription ID\> /resourceGroups/\< Resource Group Name\> /providers/Microsoft.Storage/storageAccounts/ \<Storage Account Name\>, Managed Identity: /subscriptions/ \<Subscription ID\> /resourceGroups/ /\< Resource Group Name\> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ \<User Managed Identity Name\> (Die verwaltete Identität weist keine Berechtigungen für das Speicherkonto auf. Überprüfen Sie, ob der verwalteten Identität die Rolle „Besitzer von Speicherblobdaten“ für das Speicherkonto zugewiesen wurde. Speicher: /subscriptions/ <Abonnement-ID> /resourceGroups/ <Ressourcengruppenname> /providers/Microsoft.Storage/storageAccounts/ <Speicherkontoname>, Verwaltete Identität: /subscriptions/ <Abonnement-ID> /resourceGroups/ / <Ressourcengruppenname> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/ <Name der benutzerseitig zugewiesenen verwalteten Identität>)“

### <a name="cause"></a>Ursache

Sie haben nicht die erforderlichen Berechtigungen zum Verwalten der Identität bereitgestellt. Die benutzerseitig zugewiesene verwaltete Identität weist nicht die Rolle „Mitwirkender an Storage-Blobdaten“ für das Azure Data Lake Storage Gen2-Speicherkonto auf.

### <a name="resolution"></a>Lösung

1. Öffnen Sie das Azure-Portal.
1. Wechseln Sie zum Speicherkonto.
1. Prüfen Sie die Einstellung unter **Zugriffssteuerung (IAM)** .
1. Stellen Sie sicher, dass der Rolle „Mitwirkender an Storage-Blobdaten“ oder der Rolle „Besitzer von Speicherblobdaten“ der Zugriff auf die benutzerseitig zugewiesene verwaltete Identität für das Abonnement zugewiesen wurde.

Weitere Informationen finden Sie unter [Festlegen von Berechtigungen für verwaltete Identitäten im Data Lake Storage Gen2-Konto](hdinsight-hadoop-use-data-lake-storage-gen2.md).

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>Fehlercode: InvalidNetworkSecurityGroupSecurityRules  

### <a name="error"></a>Fehler

„The security rules in the Network Security Group /subscriptions/\<SubscriptionID\>/resourceGroups/<Resource Group name\> default/providers/Microsoft.Network/networkSecurityGroups/\<Network Security Group Name\> configured with subnet /subscriptions/\<SubscriptionID\>/resourceGroups/\<Resource Group name\> RG-westeurope-vnet-tomtom-default/providers/Microsoft.Network/virtualNetworks/\<Virtual Network Name\>/subnets/\<Subnet Name\> does not allow required inbound and/or outbound connectivity. (Die Sicherheitsregeln in der Netzwerksicherheitsgruppe /subscriptions/<SubscriptionID>/resourceGroups/<Name der Ressourcengruppe> default/providers/Microsoft.Network/networkSecurityGroups/<Name der Netzwerksicherheitsgruppe>, die mit Subnetz /subscriptions/<SubscriptionID>/resourceGroups/<Name der Ressourcengruppe> RG-westeurope-vnet-tomtom-default/providers/Microsoft.Network/virtualNetworks/<Name des virtuellen Netzwerks>/subnets/<Subnetzname> konfiguriert wurde, lassen keine erforderlichen eingehenden und/oder ausgehenden Verbindungen zu.) For more information, please visit [Plan a virtual network for Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment), or contact support. (Weitere Informationen finden Sie unter „Planen eines virtuellen Netzwerks für Azure HDInsight“, oder wenden Sie sich an den Support.)“

### <a name="cause"></a>Ursache

Wenn der beim HDInsight-Cluster eingehende Datenverkehr über Netzwerksicherheitsgruppen oder benutzerdefinierte Routen (UDRs) gesteuert wird, müssen Sie sicherstellen, dass Ihr Cluster mit wichtigen Integritäts- und Verwaltungsdiensten in Azure kommunizieren kann.

### <a name="resolution"></a>Lösung

Wenn Sie Netzwerksicherheitsgruppen zum Steuern des Netzwerkdatenverkehrs verwenden möchten, führen Sie vor der Installation von HDInsight folgende Aktionen aus:

- Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten, und erstellen Sie eine Liste mit sicheren IP-Adressen für die Region. Weitere Informationen finden Sie unter [Integritäts- und Verwaltungsdienste: Bestimmte Regionen](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions).
- Identifizieren Sie die IP-Adressen, die für HDInsight erforderlich sind. Weitere Informationen finden Sie unter  [HDInsight-Verwaltungs-IP-Adressen](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses).
- Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten. Lassen Sie für Netzwerksicherheitsgruppen eingehenden Datenverkehr über Port 443 für die IP-Adressen zu. Durch diese Konfiguration wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können.
  
---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>Fehlercode: Cluster setup failed to install components on one or more hosts (Bei der Clustereinrichtung konnten Komponenten auf mindestens einem Host nicht erstellt werden.)

###  <a name="error"></a>Fehler

„Cluster setup failed to install components on one or more hosts. (Bei der Clustereinrichtung konnten auf mindestens einem Host keine Komponenten installiert werden.) Please retry your request. (Wiederholen Sie die Anforderung.)“

### <a name="cause"></a>Ursache 

Dieser Fehler wird in der Regel generiert, wenn ein vorübergehendes Problem vorliegt oder Azure ausfällt.

### <a name="resolution"></a>Lösung

Überprüfen Sie die Seite [Azure-Status](https://status.azure.com) auf Azure-Ausfälle, die sich auf die Clusterbereitstellung auswirken könnten. Wenn keine Ausfälle angegeben sind, führen Sie die Clusterbereitstellung erneut durch.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung bei der Clustererstellung finden Sie unter [Behandeln von Fehlern bei der Clustererstellung mit Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails).
