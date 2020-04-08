---
title: Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln für Azure HDInsight
description: In diesem Artikel erfahren Sie, wie Sie mit Ihrem eigenen Verschlüsselungsschlüssel aus Azure Key Vault Daten verschlüsseln, die auf verwalteten Datenträgern in Azure HDInsight gespeichert sind.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: c0521f384a333c3054397fb0ec7c2ab907e54f67
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411746"
---
# <a name="customer-managed-key-disk-encryption"></a>Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln

Azure HDInsight unterstützt die Verschlüsselung mit kundenseitig verwalteten Schlüsseln für Daten auf verwalteten Datenträgern und Ressourcendatenträgern, die an virtuelle HDInsight-Clustercomputer angefügt sind. Dank dieses Features können Sie die Verschlüsselungsschlüssel, mit denen ruhende Daten in Ihren HDInsight-Clustern geschützt werden, mithilfe von Azure Key Vault verwalten.

Alle verwalteten Datenträger in HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Wenn Sie kundenseitig verwaltete Schlüssel für HDInsight aktivieren, stellen Sie die Verschlüsselungsschlüssel für HDInsight bereit, um diese Schlüssel mithilfe von Azure Key Vault zu verwenden und zu verwalten.

In diesem Dokument wird nicht auf Daten eingegangen, die in Ihrem Azure Storage-Konto gespeichert sind. Weitere Informationen zur Azure Storage-Verschlüsselung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md). Ihre Cluster verfügen möglicherweise über mindestens ein angefügtes Azure Storage-Konto, bei dem die Verschlüsselungsschlüssel auch von Microsoft oder kundenseitig verwaltet werden können, der Verschlüsselungsdienst ist jedoch anders.

## <a name="introduction"></a>Einführung

Die Verschlüsselung mit kundenseitig verwalteten Schlüsseln ist ein Prozess mit nur einem Schritt, der bei der Clustererstellung ohne zusätzliche Kosten ausgeführt wird. Sie müssen bei der Erstellung Ihres Clusters lediglich HDInsight als verwaltete Identität bei Azure Key Vault registrieren und den Verschlüsselungsschlüssel hinzufügen.

Sowohl Ressourcendatenträger als auch verwaltete Datenträger auf den einzelnen Knoten des Clusters werden mit einem symmetrischen Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) verschlüsselt. Der DEK wird mit dem Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) aus Ihrem Schlüsseltresor geschützt. Die Ver- und Entschlüsselung wird vollständig von Azure HDInsight übernommen.

Wenn die Schlüsseltresor-Firewall für den Schlüsseltresor aktiviert ist, in dem der Schlüssel für die Datenträgerverschlüsselung gespeichert ist, müssen die IP-Adressen der regionalen HDInsight-Ressourcenanbieter für die Region, in der der Cluster bereitgestellt wird, der Konfiguration für die Schlüsseltresor-Firewall hinzugefügt werden. Dies ist erforderlich, weil HDInsight kein vertrauenswürdiger Azure Key Vault-Dienst ist.

Sie können für das sichere Rotieren der Schlüssel im Schlüsseltresor das Azure-Portal oder die Azure CLI verwenden. Beim Rotieren eines Schlüssels beginnt der HDInsight-Cluster innerhalb weniger Minuten mit der Verwendung des neuen Schlüssels. Aktivieren Sie die Schlüsselschutzfunktion [Vorläufiges Löschen](../key-vault/key-vault-ovw-soft-delete.md) zum Schutz vor Ransomware und versehentlichem Löschen. Schlüsseltresore ohne diese Schutzfunktion werden nicht unterstützt.

|Clustertyp |Betriebssystemdatenträger (verwalteter Datenträger) |Datenträger für Daten (verwalteter Datenträger) |Temporärer Datenträger (lokale SSD) |
|---|---|---|---|
|Kafka, HBase mit beschleunigten Schreibvorgängen|[SSE-Verschlüsselung](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#encryption)|SSE-Verschlüsselung + optionale CMK-Verschlüsselung|Optionale CMK-Verschlüsselung|
|Alle anderen Cluster (Spark, Interactive, Hadoop, HBase ohne beschleunigte Schreibvorgänge)|SSE-Verschlüsselung|–|Optionale CMK-Verschlüsselung|

## <a name="get-started-with-customer-managed-keys"></a>Erste Schritte mit von Kunden verwalteten Schlüsseln

Die Erstellung eines HDInsight-Clusters mit aktiviertem kundenseitig verwaltetem Schlüssel umfasst folgende Schritte:

1. Erstellen verwalteter Identitäten für Azure-Ressourcen
1. Azure Key Vault erstellen
1. Erstellen eines Schlüssels
1. Erstellen einer Zugriffsrichtlinie
1. Erstellen eines HDInsight-Clusters mit aktiviertem kundenseitig verwaltetem Schlüssel
1. Rotieren des Verschlüsselungsschlüssels

## <a name="create-managed-identities-for-azure-resources"></a>Erstellen verwalteter Identitäten für Azure-Ressourcen

Erstellen Sie eine benutzerseitig zugewiesene verwaltete Identität, um sich bei Key Vault zu authentifizieren.

Die einzelnen Schritte finden Sie unter [Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). Weitere Informationen zur Funktionsweise verwalteter Identitäten in Azure HDInsight finden Sie unter [Verwaltete Identitäten in Azure HDInsight](hdinsight-managed-identities.md). Achten Sie darauf, die Ressourcen-ID der verwalteten Identität zu speichern, da Sie sie später zur Key Vault-Zugriffsrichtlinie hinzufügen.

## <a name="create-azure-key-vault"></a>Azure Key Vault erstellen

Erstellen eines Schlüsseltresors Die einzelnen Schritte finden Sie unter [Erstellen eines Tresors](../key-vault/quick-create-portal.md).

HDInsight unterstützt nur Azure Key Vault. Falls Sie einen eigenen Schlüsseltresor besitzen, können Sie Ihre Schlüssel in Azure Key Vault importieren. Denken Sie daran, dass für den Schlüsseltresor das **vorläufige Löschen** aktiviert sein muss. Weitere Informationen zum Importieren vorhandener Schlüsseln finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../key-vault/about-keys-secrets-and-certificates.md).

## <a name="create-key"></a>Erstellen eines Schlüssels

1. Navigieren Sie in Ihrem neuen Schlüsseltresor zu **Einstellungen** > **Schlüssel** >  **+ Generieren/Importieren**.

    ![Generieren eines neuen Schlüssels in Azure Key Vault](./media/disk-encryption/create-new-key.png "Generieren eines neuen Schlüssels in Azure Key Vault")

1. Geben Sie einen Namen an, und wählen Sie dann **Erstellen** aus. Behalten Sie den standardmäßigen **Schlüsseltyp** (Option **RSA**) bei.

    ![Generieren des Schlüsselnamens](./media/disk-encryption/create-key.png "Generieren des Schlüsselnamens")

1. Wenn Sie zur Seite **Schlüssel** zurückkehren, wählen Sie den erstellten Schlüssel aus.

    ![Key Vault-Schlüsselliste](./media/disk-encryption/key-vault-key-list.png)

1. Wählen Sie die Version aus, um die Seite **Schlüsselversion** zu öffnen. Wenn Sie Ihren eigenen Schlüssel für die Verschlüsselung des HDInsight-Clusters verwenden, müssen Sie die Schlüssel-URI angeben. Kopieren Sie den **Schlüsselbezeichner**, und speichern sie ihn, bis Sie zum Erstellen des Clusters bereit sind.

    ![Abrufen des Schlüsselbezeichners](./media/disk-encryption/get-key-identifier.png)

## <a name="create-access-policy"></a>Erstellen einer Zugriffsrichtlinie

1. Navigieren Sie in Ihrem neuen Schlüsseltresor zu **Einstellungen** > **Zugriffsrichtlinien** >  **+ Zugriffsrichtlinie hinzufügen**.

    ![Erstellen einer neuen Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/key-vault-access-policy.png)

1. Geben Sie auf der Seite **Zugriffsrichtlinie hinzufügen** die folgenden Informationen an:

    |Eigenschaft |BESCHREIBUNG|
    |---|---|
    |Schlüsselberechtigungen|Wählen Sie **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen** aus.|
    |Berechtigungen für Geheimnis|Wählen Sie **Abrufen**, **Festlegen** und **Löschen** aus.|
    |Prinzipal auswählen|Wählen Sie die benutzerseitig zugewiesene verwaltete Identität aus, die Sie zuvor erstellt haben.|

    ![Festlegen der Option „Prinzipal auswählen“ für die Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/azure-portal-add-access-policy.png)

1. Wählen Sie **Hinzufügen**.

1. Wählen Sie **Speichern** aus.

    ![Speichern einer Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Erstellen eines Clusters mit Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln

Sie können nun einen neuen HDInsight-Cluster erstellen. Der kundenseitig verwaltete Schlüssel kann ausschließlich im Rahmen der Clustererstellung auf neue Cluster angewendet werden. Die Verschlüsselung kann nicht aus Clustern mit kundenseitig verwalteten Schlüsseln entfernt werden, und der kundenseitig verwaltete Schlüssel kann keinen vorhandenen Clustern hinzugefügt werden.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Geben Sie während der Clustererstellung den vollständigen **Schlüsselbezeichner** (einschließlich Schlüsselversion) an. Beispiel: `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Darüber hinaus müssen Sie die verwaltete Identität dem Cluster zuweisen und den Schlüssel-URI angeben.

![Neuen Cluster erstellen](./media/disk-encryption/create-cluster-portal.png)

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Im folgenden Beispiel wird mithilfe der Azure-Befehlszeilenschnittstelle ein neuer Apache Spark-Cluster mit aktivierter Datenträgerverschlüsselung erstellt. Weitere Informationen finden Sie in der Dokumentation der Azure-Befehlszeilenschnittstelle unter [az hdinsight create](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-create).

```azurecli
az hdinsight create -t spark -g MyResourceGroup -n MyCluster \
-p "HttpPassword1234!" --workernode-data-disks-per-node 2 \
--storage-account MyStorageAccount \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--assign-identity MyMSI
```

## <a name="rotating-the-encryption-key"></a>Rotieren des Verschlüsselungsschlüssels

In manchen Szenarien müssen unter Umständen die vom HDInsight-Cluster verwendeten Verschlüsselungsschlüssel geändert werden, nachdem der Cluster bereits erstellt wurde. Hierzu können Sie das Portal verwenden. Für diesen Vorgang muss der Cluster sowohl auf den aktuellen Schlüssel als auch auf den vorgesehenen neuen Schlüssel zugreifen können, andernfalls schlägt der Vorgang zum Rotieren des Schlüssels fehl.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Zum Rotieren des Schlüssels benötigen Sie die Basis-URI des Schlüsseltresors. Wechseln Sie anschließend im Portal zum Abschnitt mit den HDInsight-Clustereigenschaften, und klicken Sie unter **Schlüssel-URL für Datenträgerverschlüsselung** auf **Schlüssel ändern**. Geben Sie die neue Schlüssel-URL ein, und senden Sie sie, um den Schlüssel zu rotieren.

![Rotieren des Schlüssels für die Datenträgerverschlüsselung](./media/disk-encryption/change-key.png)

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Im folgenden Beispiel wird der Datenträgerverschlüsselungsschlüssel für einen vorhandenen HDInsight-Cluster rotiert. Weitere Informationen finden Sie in der Dokumentation der Azure-Befehlszeilenschnittstelle unter [az hdinsight rotate-disk-encryption-key](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-rotate-disk-encryption-key).

```azurecli
az hdinsight rotate-disk-encryption-key \
--encryption-key-name SparkClusterKey \
--encryption-key-version 00000000000000000000000000000000 \
--encryption-vault-uri https://MyKeyVault.vault.azure.net \
--name MyCluster \
--resource-group MyResourceGroup
```

## <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Aktualisieren Sie die Resource Manager-Vorlage mit den folgenden Änderungen, um vom Kunden verwaltete Schlüssel mithilfe der Vorlage zu verwenden:

1. Fügen Sie in der Datei **azuredeploy.json** die folgende Eigenschaft zum Objekt „Ressource“ hinzu:

    ```json
       "diskEncryptionProperties":
         {
                 "vaultUri": "[parameters('diskEncryptionVaultUri')]",
                  "keyName": "[parameters('diskEncryptionKeyName')]",
                  "keyVersion": "[parameters('diskEncryptionKeyVersion')]",
                   "msiResourceId": "[parameters('diskEncryptionMsiResourceId')]"
         }

1. In the **azuredeploy.parameters.json** file, add the following parameters. You can get the values of these parameters from the Key Vault URI and the managed Identity. For example, if you have the following URI and identity values,
    * Sample key vault URI: https://<KeyVault_Name>.vault.azure.net/keys/clusterkey/<Cluster_Key_Value>
    * Sample user-assigned managed identity: "/subscriptions/<subscriptionID>/resourcegroups/<ResourceGroup_Name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI_Name>

    The parameters in the **azuredeploy.parameters.json** file are:

    ```json
   "diskEncryptionVaultUri": {
            "value": "https://<KeyVault_Name>.vault.azure.net"
        },
        "diskEncryptionKeyName": {
            "value": "clusterkey"
        },
        "diskEncryptionKeyVersion": {
            "value": "<Cluster_Key_Value>"
        },
        "diskEncryptionMsiResourceId": {
            "value": "/subscriptions/<subscriptionID>/resourcegroups/<ResourceGroup_Name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI_Name>"
        }
    ```

## <a name="faq-for-customer-managed-key-encryption"></a>Häufig gestellte Fragen zur Verschlüsselung mit kundenseitig verwalteten Schlüsseln

**Wie greift der HDInsight-Cluster auf meinen Schlüsseltresor zu?**

HDInsight verwendet die verwaltete Identität, die Sie dem HDInsight-Cluster zuordnen, um auf Ihre Azure Key Vault-Instanz zuzugreifen. Diese verwaltete Identität kann vor oder während der Clustererstellung erstellt werden. Außerdem müssen Sie der verwalteten Identität Zugriff auf den Schlüsseltresor mit dem gespeicherten Schlüssel gewähren.

**Ist dieses Feature für alle Cluster in HDInsight verfügbar?**

Die Verschlüsselung mit kundenseitig verwalteten Schlüsseln steht für alle Clustertypen zur Verfügung (mit Ausnahme von Spark 2.1 und 2.2).

**Kann ich mehrere Schlüssel verwenden, um verschiedene Datenträger oder Ordner zu verschlüsseln?**

Nein. Alle verwalteten Datenträger und Ressourcendatenträger werden mit dem gleichen Schlüssel verschlüsselt.

**Was geschieht, wenn der Cluster den Zugriff auf den Schlüsseltresor oder den Schlüssel verliert?**

Wenn der Cluster den Zugriff auf den Schlüssel verliert, werden im Apache Ambari-Portal Warnungen angezeigt. In diesem Zustand schlägt der Vorgang **Schlüssel ändern** fehl. Sobald der Schlüsselzugriff wieder hergestellt wurde, verschwinden die Ambari-Warnungen, und Vorgänge wie z. B. die Schlüsselrotation können erfolgreich ausgeführt werden.

![Ambari-Warnung zum Schlüsselzugriff](./media/disk-encryption/ambari-alert.png)

**Wie kann ich den Cluster wiederherstellen, wenn die Schlüssel gelöscht werden?**

Da nur Schlüssel unterstützt werden, für die „vorläufiges Löschen“ aktiviert ist, sollte der Cluster wieder Zugriff auf die Schlüssel erlangen, wenn die Schlüssel im Schlüsseltresor wiederhergestellt werden. Informationen zum Wiederherstellen eines Azure Key Vault-Schlüssels finden Sie unter [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) oder [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Welche Arten von Datenträgern werden verschlüsselt? Werden Betriebssystemdatenträger/Ressourcendatenträger ebenfalls verschlüsselt?**

Ressourcendatenträger und reguläre Datenträger/verwaltete Datenträger werden verschlüsselt. Betriebssystemdatenträger werden nicht verschlüsselt.

**Werden kundenseitig verwaltete Schlüssel nahtlos von neuen Knoten unterstützt, wenn ein Cluster zentral hochskaliert wird?**

Ja. Der Cluster benötigt während der Hochskalierung Zugriff auf den Schlüssel im Schlüsseltresor. Sowohl verwaltete Datenträger als auch Ressourcendatenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Stehen kundenseitig verwaltete Schlüssel an meinem Standort zur Verfügung?**

Kundenseitig verwaltete Schlüssel von HDInsight stehen in allen öffentlichen und nationalen Clouds zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure Key Vault finden Sie unter [Was ist Azure Key Vault](../key-vault/key-vault-overview.md).
* [Übersicht über die Unternehmenssicherheit in Azure HDInsight](./domain-joined/hdinsight-security-overview.md).
