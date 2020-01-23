---
title: Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln für Azure HDInsight
description: In diesem Artikel erfahren Sie, wie Sie mit Ihrem eigenen Verschlüsselungsschlüssel aus Azure Key Vault Daten verschlüsseln, die auf verwalteten Datenträgern in Azure HDInsight gespeichert sind.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/06/2019
ms.openlocfilehash: 2c015db828bcbfa8b26f519b3a4707b5ec69b8f3
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75982508"
---
# <a name="customer-managed-key-disk-encryption"></a>Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln

Azure HDInsight unterstützt die Verschlüsselung mit kundenseitig verwalteten Schlüsseln für Daten auf verwalteten Datenträgern und Ressourcendatenträgern, die an virtuelle HDInsight-Clustercomputer angefügt sind. Diese Art der Verschlüsselung wird auch als BYOK-Verschlüsselung (Bring Your Own Key) bezeichnet. Dank dieses Features können Sie die Verschlüsselungsschlüssel, mit denen ruhende Daten in Ihren HDInsight-Clustern geschützt werden, mithilfe von Azure Key Vault verwalten. Ihre Cluster verfügen möglicherweise über mindestens ein angefügtes Azure Storage-Konto, bei dem die Verschlüsselungsschlüssel auch von Microsoft oder kundenseitig verwaltet werden können, der Verschlüsselungsdienst ist jedoch anders.

In diesem Dokument wird nicht auf Daten eingegangen, die in Ihrem Azure Storage-Konto gespeichert sind. Weitere Informationen zu ASE finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../storage/common/storage-service-encryption.md).

Alle verwalteten Datenträger in HDInsight werden mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) von Azure geschützt. Die Daten auf diesen Datenträgern werden standardmäßig mit von Microsoft verwalteten Schlüsseln verschlüsselt. Wenn Sie kundenseitig verwaltete Schlüssel für HDInsight aktivieren, stellen Sie die Verschlüsselungsschlüssel für HDInsight bereit, um diese Schlüssel mithilfe von Azure Key Vault zu verwenden und zu verwalten.

Die Verschlüsselung mit kundenseitig verwalteten Schlüsseln ist ein Prozess mit nur einem Schritt, der bei der Clustererstellung ohne zusätzliche Kosten ausgeführt wird. Sie müssen bei der Erstellung Ihres Clusters lediglich HDInsight als verwaltete Identität bei Azure Key Vault registrieren und den Verschlüsselungsschlüssel hinzufügen.

Sowohl Ressourcendatenträger als auch verwaltete Datenträger auf den einzelnen Knoten des Clusters werden mit einem symmetrischen Datenverschlüsselungsschlüssel (Data Encryption Key, DEK) verschlüsselt. Der DEK wird mit dem Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) aus Ihrem Schlüsseltresor geschützt. Die Ver- und Entschlüsselung wird vollständig von Azure HDInsight übernommen.

Sie können für das sichere Rotieren der Schlüssel im Schlüsseltresor das Azure-Portal oder die Azure CLI verwenden. Beim Rotieren eines Schlüssels beginnt der HDInsight-Cluster innerhalb weniger Minuten mit der Verwendung des neuen Schlüssels. Aktivieren Sie die Schlüsselschutzfunktion „Soft Delete“ (Vorläufig löschen) zum Schutz vor Ransomware und versehentlichem Löschen. Schlüsseltresore ohne diese Schutzfunktion werden nicht unterstützt.

## <a name="get-started-with-customer-managed-keys"></a>Erste Schritte mit von Kunden verwalteten Schlüsseln

Die Erstellung eines HDInsight-Clusters mit aktiviertem kundenseitig verwaltetem Schlüssel umfasst folgende Schritte:

1. Erstellen verwalteter Identitäten für Azure-Ressourcen
2. Einrichten von Azure Key Vault mit den Schlüsseln
3. Erstellen eines HDInsight-Clusters mit aktiviertem kundenseitig verwaltetem Schlüssel
4. Rotieren des Verschlüsselungsschlüssels

## <a name="create-managed-identities-for-azure-resources"></a>Erstellen verwalteter Identitäten für Azure-Ressourcen

Erstellen Sie für die Authentifizierung bei Key Vault mit dem [Azure-Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md), [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md) oder der [Azure-Befehlszeilenschnittstelle](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) eine benutzerseitig zugewiesene verwaltete Identität. Weitere Informationen zur Funktionsweise verwalteter Identitäten in Azure HDInsight finden Sie unter [Verwaltete Identitäten in Azure HDInsight](hdinsight-managed-identities.md). Achten Sie darauf, die Ressourcen-ID der verwalteten Identität zu speichern, da Sie sie später zur Key Vault-Zugriffsrichtlinie hinzufügen.

## <a name="set-up-the-key-vault-and-keys"></a>Einrichten von Key Vault und Schlüsseln

HDInsight unterstützt nur Azure Key Vault. Falls Sie einen eigenen Schlüsseltresor besitzen, können Sie Ihre Schlüssel in Azure Key Vault importieren. Denken Sie daran, dass für die Schlüssel „Vorläufig löschen“ aktiviert sein muss. Die Funktion für vorläufiges Löschen ist über REST, .NET/C#, PowerShell und die Azure-Befehlszeilenschnittstelle verfügbar.

1. Befolgen Sie zum Erstellen eines neuen Schlüsseltresors die Schnellstartanleitung für [Azure Key Vault](../key-vault/key-vault-overview.md). Weitere Informationen zum Importieren vorhandener Schlüsseln finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../key-vault/about-keys-secrets-and-certificates.md).

1. Aktivieren Sie „soft-delete“ (Vorläufiges Löschen) für den Schlüsseltresor mithilfe des CLI-Befehls [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update).

    ```azurecli
    az keyvault update --name <Key Vault Name> --enable-soft-delete
    ```

1. Erstellen Sie Schlüssel.

    a. Wählen Sie zum Erstellen eines neuen Schlüssels **Generieren/Importieren** im Menü **Schlüssel** unter **Einstellungen** aus.

    ![Generieren eines neuen Schlüssels in Azure Key Vault](./media/disk-encryption/create-new-key.png "Generieren eines neuen Schlüssels in Azure Key Vault")

    b. Legen Sie **Optionen** auf **Generieren** fest, und benennen Sie den Schlüssel.

    ![Generieren des Schlüsselnamens](./media/disk-encryption/create-key.png "Generieren des Schlüsselnamens")

    c. Wählen Sie den Schlüssel, den Sie erstellt haben, aus der Liste der Schlüssel aus.

    ![Key Vault-Schlüsselliste](./media/disk-encryption/key-vault-key-list.png)

    d. Wenn Sie Ihren eigenen Schlüssel für die Verschlüsselung des HDInsight-Clusters verwenden, müssen Sie die Schlüssel-URI angeben. Kopieren Sie den **Schlüsselbezeichner**, und speichern sie ihn, bis Sie zum Erstellen des Clusters bereit sind.

    ![Abrufen des Schlüsselbezeichners](./media/disk-encryption/get-key-identifier.png)

1. Fügen Sie eine verwaltete Identität zur Key Vault-Zugriffsrichtlinie hinzu.

    a. Erstellen Sie eine neue Azure Key Vault-Zugriffsrichtlinie.

    ![Erstellen einer neuen Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/add-key-vault-access-policy.png)

    b. Wählen Sie unter **Prinzipal auswählen** die von Ihnen erstellte benutzerseitig zugewiesene verwaltete Identität aus.

    ![Festlegen der Option „Prinzipal auswählen“ für die Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/add-key-vault-access-policy-select-principal.png)

    c. Aktivieren Sie unter **Schlüsselberechtigungen** die Optionen **Abrufen**, **Schlüssel entpacken** und **Schlüssel packen**.

    ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 1](./media/disk-encryption/add-key-vault-access-policy-keys.png "Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 1")

    d. Aktivieren Sie unter **Berechtigungen für Geheimnis** die Optionen **Abrufen**, **Festlegen** und **Löschen**.

    ![Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 2](./media/disk-encryption/add-key-vault-access-policy-secrets.png "Festlegen der Schlüsselberechtigungen für die Azure Key Vault-Zugriffsrichtlinie: 2")

    e. Wählen Sie **Speichern** aus.

    ![Speichern einer Azure Key Vault-Zugriffsrichtlinie](./media/disk-encryption/add-key-vault-access-policy-save.png)

## <a name="create-cluster-with-customer-managed-key-disk-encryption"></a>Erstellen eines Clusters mit Datenträgerverschlüsselung mit kundenseitig verwalteten Schlüsseln

Sie können nun einen neuen HDInsight-Cluster erstellen. Der kundenseitig verwaltete Schlüssel kann ausschließlich im Rahmen der Clustererstellung auf neue Cluster angewendet werden. Die Verschlüsselung kann nicht aus Clustern mit kundenseitig verwalteten Schlüsseln entfernt werden, und der kundenseitig verwaltete Schlüssel kann keinen vorhandenen Clustern hinzugefügt werden.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Geben Sie während der Clustererstellung die vollständige Schlüssel-URL (einschließlich Schlüsselversion) an. Beispiel: `https://contoso-kv.vault.azure.net/keys/myClusterKey/46ab702136bc4b229f8b10e8c2997fa4`. Darüber hinaus müssen Sie die verwaltete Identität dem Cluster zuweisen und den Schlüssel-URI angeben.

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

Um den Schlüssel zu rotieren, müssen Sie über die vollständige URL des neuen Schlüssels verfügen (siehe Schritt 3 unter [Einrichten des Schlüsseltresors und der Schlüssel](#set-up-the-key-vault-and-keys)). Wechseln Sie anschließend im Portal zum Abschnitt mit den HDInsight-Clustereigenschaften, und klicken Sie unter **Schlüssel-URL für Datenträgerverschlüsselung** auf **Schlüssel ändern**. Geben Sie die neue Schlüssel-URL ein, und senden Sie sie, um den Schlüssel zu rotieren.

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

Da nur Schlüssel unterstützt werden, für die „Soft Delete“ (Vorläufig löschen) aktiviert ist, sollte der Cluster wieder Zugriff auf die Schlüssel erlangen, wenn die Schlüssel im Schlüsseltresor wiederhergestellt werden. Informationen zum Wiederherstellen eines Azure Key Vault-Schlüssels finden Sie unter [Undo-AzKeyVaultKeyRemoval](/powershell/module/az.keyvault/Undo-AzKeyVaultKeyRemoval) oder [az-keyvault-key-recover](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-recover).

**Welche Arten von Datenträgern werden verschlüsselt? Werden Betriebssystemdatenträger/Ressourcendatenträger ebenfalls verschlüsselt?**

Ressourcendatenträger und reguläre Datenträger/verwaltete Datenträger werden verschlüsselt. Betriebssystemdatenträger werden nicht verschlüsselt.

**Werden kundenseitig verwaltete Schlüssel nahtlos von neuen Knoten unterstützt, wenn ein Cluster zentral hochskaliert wird?**

Ja. Der Cluster benötigt während der zentralen Hochskalierung Zugriff auf den Schlüssel im Schlüsseltresor. Sowohl verwaltete Datenträger als auch Ressourcendatenträger im Cluster werden mit dem gleichen Schlüssel verschlüsselt.

**Stehen kundenseitig verwaltete Schlüssel an meinem Standort zur Verfügung?**

Kundenseitig verwaltete Schlüssel von HDInsight stehen in allen öffentlichen und nationalen Clouds zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Unternehmenssicherheit in Azure HDInsight](./domain-joined/hdinsight-security-overview.md)
