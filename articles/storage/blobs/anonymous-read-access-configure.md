---
title: Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie den anonymen Zugriff auf Blobdaten für das Speicherkonto aktivieren bzw. deaktivieren. Legen Sie die Einstellung für den öffentlichen Containerzugriff fest, damit anonym auf Container und Blobs zugegriffen werden kann.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/29/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: af589874021baaf04a423b7bbaa0e36528eda93c
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209048"
---
# <a name="configure-anonymous-public-read-access-for-containers-and-blobs"></a>Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs

Azure Storage unterstützt den anonymen, öffentlichen Lesezugriff auf Container und Blobs. Standardmäßig müssen alle Anforderungen an einen Container und zugehörige Blobs entweder mithilfe von Azure Active Directory (Azure AD) oder der Autorisierung mit dem gemeinsam verwendetem Schlüssel autorisiert werden. Wenn Sie die Einstellung für die öffentliche Zugriffsebene eines Containers so konfigurieren, dass der anonyme Zugriff erlaubt ist, können Clients Daten in diesem Container lesen, ohne die Anforderung zu autorisieren.

> [!WARNING]
> Wenn ein Container für den öffentlichen Zugriff konfiguriert ist, kann jeder Client Daten in diesem Container lesen. Der öffentliche Zugriff stellt ein potenzielles Sicherheitsrisiko dar. Sollte es in Ihrem Szenario nicht erforderlich sein, empfiehlt Microsoft die Deaktivierung des Zugriffs für das Speicherkonto. Weitere Informationen finden Sie unter [Verhindern des anonymem, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-prevent.md).

Um den öffentlichen Zugriff auf einen Container zu konfigurieren, müssen Sie zwei Schritte ausführen:

1. Aktivieren des öffentlichen Zugriffs für das Speicherkonto
1. Konfigurieren der Einstellung für den öffentlichen Zugriff des Containers

In diesem Artikel wird beschrieben, wie Sie anonymen, öffentlichen Lesezugriff auf einen Container und zugehörige Blobs konfigurieren. Informationen dazu, wie Sie anonym von einer Clientanwendung aus auf Blobdaten zugreifen können, finden Sie unter [Anonymer Zugriff auf öffentliche Container und Blobs mit .NET](anonymous-read-access-client.md).

## <a name="enable-or-disable-public-read-access-for-a-storage-account"></a>Aktivieren oder Deaktivieren des öffentlichen Lesezugriffs für ein Speicherkonto

Der öffentliche Zugriff ist für ein Speicherkonto standardmäßig aktiviert. Die Deaktivierung des öffentlichen Zugriffs verhindert den anonymen Zugriff auf Container und Blobs in diesem Konto. Zur Verbesserung der Sicherheit empfiehlt Microsoft, den öffentlichen Zugriff auf Ihre Speicherkonten zu deaktivieren, wenn das Szenario nicht erfordert, dass Benutzer anonym auf Blobressourcen zugreifen.

> [!WARNING]
> Durch die Deaktivierung des öffentlichen Zugriffs für ein Speicherkonto werden die Einstellungen für den öffentlichen Zugriff für alle Container in diesem Speicherkonto überschrieben. Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert ist, können keine zukünftigen anonymen Anforderungen an dieses Konto mehr gestellt werden.

Verwenden Sie das Azure-Portal oder die Azure CLI zum Konfigurieren der Eigenschaft **blobPublicAccess** des Kontos, um den öffentlichen Zugriff für ein Speicherkonto zu aktivieren oder zu deaktivieren. Diese Eigenschaft ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie unter [Speicherkontoübersicht](../common/storage-account-overview.md).

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um den öffentlichen Zugriff für ein Speicherkonto im Azure-Portal zu aktivieren oder zu deaktivieren:

1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Suchen Sie unter **Einstellungen** die Einstellung **Konfiguration**.
1. Klicken Sie unter **Öffentlicher Blobzugriff** auf die Option **Deaktiviert** oder **Aktiviert**.

    :::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Screenshot, der zeigt, wie Sie den öffentlichen Blobzugriff für das Konto aktivieren oder deaktivieren":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wenn Sie den öffentlichen Zugriff für ein Speicherkonto mit der Azure CLI aktivieren oder deaktivieren möchten, rufen Sie zunächst die Ressourcen-ID für Ihr Speicherkonto über den Befehl [az resource show](/cli/azure/resource#az-resource-show) ab. Rufen Sie danach den Befehl [az resource update](/cli/azure/resource#az-resource-update) auf, um die Eigenschaft **allowBlobPublicAccess** für das Speicherkonto festzulegen. Legen Sie zum Aktivieren des öffentlichen Zugriffs die Eigenschaft **allowBlobPublicAccess** auf TRUE fest. Zum Deaktivieren müssen Sie die Eigenschaft auf **FALSE** festlegen.

Im folgenden Beispiel wird der Zugriff auf öffentliche Blobs für das Speicherkonto deaktiviert. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
storage_account_id=$(az resource show \
    --name anonpublicaccess \
    --resource-group storagesamples-rg \
    --resource-type Microsoft.Storage/storageAccounts \
    --query id \
    --output tsv)

az resource update \
    --ids $storage_account_id \
    --set properties.allowBlobPublicAccess=false
    ```
```

Um zu überprüfen, ob der öffentliche Zugriff mit der Azure CLI aktiviert ist, müssen Sie den Befehl [az resource show](/cli/azure/resource#az-resource-show) aufrufen und eine Abfrage für die Eigenschaft **allowBlobPublicAccess** ausführen:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

---

> [!NOTE]
> Die Deaktivierung des öffentlichen Zugriffs auf ein Speicherkonto wirkt sich nicht auf statische Websites aus, die in diesem Speicherkonto gehostet werden. Auf den Container **$web** kann immer öffentlich zugegriffen werden.

## <a name="check-the-public-access-setting-for-a-storage-account"></a>Überprüfen der Einstellung für den öffentlichen Zugriff für ein Speicherkonto

Wenn Sie die Einstellung für den öffentlichen Zugriff für ein Speicherkonto überprüfen möchten, können Sie den Wert der Eigenschaft **allowBlobPublicAccess** abrufen. Über den Azure Resource Graph-Explorer können Sie diese Eigenschaft für eine große Anzahl von Speicherkonten gleichzeitig überprüfen.

Die Eigenschaft **allowBlobPublicAccess** wird nicht standardmäßig festgelegt und gibt erst dann einen Wert zurück, wenn Sie sie explizit festgelegt haben. Das Speicherkonto erlaubt standardmäßig den öffentlichen Zugriff, wenn der Eigenschaftswert NULL ist.

### <a name="check-the-public-access-setting-for-a-single-storage-account"></a>Überprüfen der Einstellung für den öffentlichen Zugriff für ein einzelnes Speicherkonto

Sie müssen zur Überprüfung der Einstellung für den öffentlichen Zugriff für ein einzelnes Speicherkonto mit der Azure CLI den Befehl [az resource show](/cli/azure/resource#az-resource-show) aufrufen und eine Abfrage für die Eigenschaft **allowBlobPublicAccess** ausführen:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

### <a name="check-the-public-access-setting-for-a-set-of-storage-accounts"></a>Überprüfen der Einstellung für den öffentlichen Zugriff für eine Gruppe von Speicherkonten

Wenn Sie die Einstellung für den öffentlichen Zugriff für eine Gruppe von Speicherkonten mit optimaler Leistung überprüfen möchten, können Sie den Azure Resource Graph-Explorer im Azure-Portal verwenden. Weitere Informationen zur Verwendung des Resource Graph-Explorers finden Sie unter [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](/azure/governance/resource-graph/first-query-portal).

Wenn Sie die folgende Abfrage im Resource Graph-Explorer ausführen, wird eine Liste der Speicherkonten zurückgegeben und für jedes Konto der Wert der Eigenschaft **allowBlobPublicAccess** angezeigt:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend allowBlobPublicAccess = parse_json(properties).allowBlobPublicAccess
| project subscriptionId, resourceGroup, name, allowBlobPublicAccess
| order by subscriptionId, resourceGroup, name asc
```

## <a name="set-the-public-access-level-for-a-container"></a>Festlegen der öffentlichen Zugriffsebene auf einen Container

Wenn anonyme Benutzer Lesezugriff auf den Container und auf die zugehörigen Blobs erhalten sollen, müssen Sie zunächst den öffentlichen Zugriff für das Speicherkonto aktivieren und können anschließend die öffentliche Zugriffsebene für den Container festlegen. Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert ist, können Sie den öffentlichen Zugriff auf einen Container nicht konfigurieren.

Wenn der öffentliche Zugriff für ein Speicherkonto aktiviert ist, können Sie einen Container mit den folgenden Berechtigungen konfigurieren:

- **No public read access** (Kein öffentlicher Lesezugriff): Auf den Container und zugehörige Blobs kann nur mit einer autorisierten Anforderung zugegriffen werden. Diese Option ist die Standardeinstellung für alle neuen Container.
- **Public read access for blobs only** (Öffentlicher Lesezugriff nur für Blobs): Blobs innerhalb des Containers können über anonyme Anforderungen gelesen werden, Containerdaten sind aber anonym nicht verfügbar. Anonyme Clients können die Blobs im Container nicht aufzählen.
- **Public read access for container and its blobs** (Öffentlicher Lesezugriff für Container und zugehörige Blobs): Container- und Blobdaten können über anonyme Anforderungen gelesen werden, mit Ausnahme der Einstellungen für Containerberechtigungen und -metadaten. Clients können Blobs im Container über anonyme Anforderungen aufzählen, können aber keine Container im Speicherkonto aufzählen.

Sie können die öffentliche Zugriffsebene nicht für einzelne Blobs ändern, da diese ausschließlich auf Containerebene festgelegt wird.

Verwenden Sie das Azure-Portal oder die Azure CLI, um die öffentliche Zugriffsebene eines Containers festzulegen. Sie können die öffentliche Zugriffsebene des Containers festlegen, wenn Sie den Container erstellen, oder diese Einstellung für einen vorhandenen Container aktualisieren.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um die öffentliche Zugriffsebene für einen oder mehrere vorhandene Container im Azure-Portal zu aktualisieren:

1. Navigieren Sie zu Ihrer Speicherkontoübersicht im Azure-Portal.
1. Wählen Sie auf dem Menüblatt unter **Blob-Dienst** die Option **Container** aus.
1. Wählen Sie die Container aus, für die Sie die öffentliche Zugriffsebene festlegen möchten.
1. Klicken Sie auf **Zugriffsebene ändern**, um die Einstellungen für den öffentlichen Zugriff aufzurufen.
1. Wählen Sie aus der Dropdownliste **Öffentliche Zugriffsebene** den gewünschten Eintrag aus, und klicken Sie auf „OK“, damit die Änderungen auf die ausgewählten Container angewendet werden.

    ![Screenshot: Festlegen der öffentlichen Zugriffsebene im Azure-Portal](./media/anonymous-read-access-configure/configure-public-access-container.png)

Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert ist, kann die öffentliche Zugriffsebene eines Containers nicht festgelegt werden. Wenn Sie die öffentliche Zugriffsebene eines Containers festlegen möchten, sehen Sie, dass die Einstellung deaktiviert ist, da der öffentliche Zugriff für dieses Konto unzulässig ist.

:::image type="content" source="media/anonymous-read-access-configure/container-public-access-blocked.png" alt-text="Screenshot, der zeigt, dass das Festlegen der öffentlichen Zugriffsebene des Containers blockiert wird, wenn der öffentliche Zugriff deaktiviert ist":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sie müssen Sie den Befehl [az storage container set permission](/cli/azure/storage/container#az-storage-container-set-permission) aufrufen, um die öffentliche Zugriffsebene für einen oder mehrere Container mit der Azure CLI zu aktualisieren. Autorisieren Sie diesen Vorgang, indem Sie Ihren Kontoschlüssel, eine Verbindungszeichenfolge oder eine Shared Access Signature (SAS) übergeben. Der Vorgang [Container-ACL festlegen](/rest/api/storageservices/set-container-acl), der die öffentliche Zugriffsebene des Containers festlegt, bietet keine Unterstützung für die Autorisierung mit Azure AD. Weitere Informationen finden Sie unter [Berechtigungen zum Aufrufen von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

Im folgenden Beispiel wird die Einstellung für den öffentlichen Zugriff für einen Container festgelegt, um den anonymen Zugriff auf den Container und zugehörige Blobs zu ermöglichen. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage container set-permission \
    --name <container-name> \
    --account-name <account-name> \
    --public-access container \
    --account-key <account-key> \
    --auth-mode key
```

Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert ist, kann die öffentliche Zugriffsebene eines Containers nicht festgelegt werden. Wenn Sie die öffentliche Zugriffsebene des Containers festlegen möchten, tritt ein Fehler auf, der besagt, dass der öffentliche Zugriff für das Speicherkonto unzulässig ist.

---

## <a name="check-the-container-public-access-setting"></a>Überprüfen der Einstellung für den öffentlichen Zugriff des Containers

Um die Einstellung für den öffentlichen Zugriff für einen oder mehrere Container zu überprüfen, können Sie das Azure-Portal, PowerShell, die Azure CLI, eine der Azure Storage-Clientbibliotheken oder den Azure Storage-Ressourcenanbieter verwenden. In den folgenden Abschnitten finden Sie einige Beispiele.  

### <a name="check-the-public-access-setting-for-a-single-container"></a>Überprüfen der Einstellung für den öffentlichen Zugriff auf einen einzelnen Container

Um die öffentliche Zugriffsebene für einen oder mehrere Container mit der Azure CLI abzurufen, müssen Sie den Befehl [az storage container show permission](/cli/azure/storage/container#az-storage-container-show-permission) aufrufen. Autorisieren Sie diesen Vorgang, indem Sie Ihren Kontoschlüssel, eine Verbindungszeichenfolge oder eine Shared Access Signature (SAS) übergeben. Der Vorgang [Container-ACL abrufen](/rest/api/storageservices/get-container-acl), der die öffentliche Zugriffsebene des Containers zurückgibt, bietet keine Unterstützung für die Autorisierung mit Azure AD. Weitere Informationen finden Sie unter [Berechtigungen zum Aufrufen von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

Im folgenden Beispiel wird die Einstellung für den öffentlichen Zugriff auf einen Container dargestellt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage container show-permission \
    --name <container-name> \
    --account-name <account-name> \
    --account-key <account-key>
    --auth-mode key
```

### <a name="check-the-public-access-setting-for-a-set-of-containers"></a>Überprüfen der Einstellung für den öffentlichen Zugriff für eine Gruppe von Containern

Durch Auflisten der Container und Überprüfen der Einstellung für den öffentlichen Zugriff können Sie überprüfen, welche Container in einem oder mehreren Speicherkonten für den öffentlichen Zugriff konfiguriert sind. Diese Vorgehensweise ist praktisch, wenn ein Speicherkonto keine große Anzahl von Containern enthält oder wenn Sie die Einstellung für eine kleine Anzahl von Speicherkonten überprüfen. Die Leistung kann jedoch beeinträchtigt werden, wenn Sie versuchen, eine große Anzahl von Containern aufzuzählen.

Im folgenden Beispiel wird PowerShell verwendet, um die Einstellung für den öffentlichen Zugriff auf alle Container in einem Speicherkonto zu erhalten. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Get-AzStorageContainer -Context $ctx | Select Name, PublicAccess
```

## <a name="next-steps"></a>Nächste Schritte

- [Verhindern des anonymem, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-prevent.md)
- [Anonymer Zugriff auf öffentliche Container und Blobs mit .NET](anonymous-read-access-client.md)
- [Autorisierung des Zugriffs auf Azure Storage](../common/storage-auth.md)