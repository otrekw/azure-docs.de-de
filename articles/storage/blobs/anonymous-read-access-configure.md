---
title: Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie den anonymen Zugriff auf Blobdaten für das Speicherkonto zulassen bzw. verweigern. Legen Sie die Einstellung für den öffentlichen Containerzugriff fest, damit anonym auf Container und Blobs zugegriffen werden kann.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/23/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: daf4eb4492f723b049dc62a16351e04ffc252337
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289245"
---
# <a name="configure-anonymous-public-read-access-for-containers-and-blobs"></a>Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs

Azure Storage unterstützt optional den anonymen, öffentlichen Lesezugriff für Container und Blobs. Standardmäßig wird der anonyme Zugriff auf Ihre Daten nie zugelassen. Wenn Sie den anonymen Zugriff nicht explizit aktivieren, müssen alle Anforderungen an einen Container und dessen Blobs autorisiert werden. Wenn Sie die Einstellung für die öffentliche Zugriffsebene eines Containers so konfigurieren, dass der anonyme Zugriff erlaubt ist, können Clients Daten in diesem Container lesen, ohne die Anforderung zu autorisieren.

> [!WARNING]
> Wenn ein Container für den öffentlichen Zugriff konfiguriert ist, kann jeder Client Daten in diesem Container lesen. Der öffentliche Zugriff stellt ein potenzielles Sicherheitsrisiko dar. Sollte es in Ihrem Szenario nicht erforderlich sein, empfiehlt Microsoft die Verweigerung des Zugriffs für das Speicherkonto. Weitere Informationen finden Sie unter [Verhindern des anonymem, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-prevent.md).

In diesem Artikel wird beschrieben, wie Sie anonymen, öffentlichen Lesezugriff auf einen Container und zugehörige Blobs konfigurieren. Informationen dazu, wie Sie anonym von einer Clientanwendung aus auf Blobdaten zugreifen können, finden Sie unter [Anonymer Zugriff auf öffentliche Container und Blobs mit .NET](anonymous-read-access-client.md).

## <a name="about-anonymous-public-read-access"></a>Anonymer öffentlicher Lesezugriff

Der öffentliche Zugriff auf Ihre Daten ist standardmäßig immer unzulässig. Es gibt zwei Möglichkeiten, die Einstellungen für den öffentlichen Zugriff zu ändern:

1. **Lassen Sie den öffentlichen Zugriff für das Speicherkonto zu:** Standardmäßig kann ein Speicherkontobenutzer mit entsprechenden Berechtigungen den öffentlichen Zugriff auf Container aktivieren. Für Blobdaten ist der öffentliche Zugriff nicht verfügbar, sofern der Benutzer nicht in einem zusätzlichen Schritt explizit die Containereinstellung für den öffentlichen Zugriff konfiguriert.
1. **Konfigurieren Sie die Containereinstellung für den öffentlichen Zugriff:** Standardmäßig ist die Containereinstellung für den öffentlichen Zugriff deaktiviert. Jede Anforderung an den Container oder dessen Daten muss also autorisiert werden. Ein Benutzer mit entsprechenden Berechtigungen kann die Containereinstellung für den öffentlichen Zugriff so ändern, dass anonymer Zugriff nur zugelassen wird, wenn dieser auch für das Speicherkonto aktiviert ist.

In der folgenden Tabelle wird zusammengefasst, wie sich diese beiden Einstellungen auf den öffentlichen Zugriff für einen Container auswirken.

| Einstellung für öffentlichen Zugriff | Öffentlicher Zugriff für einen Container ist deaktiviert (Standardeinstellung) | Öffentlicher Zugriff für einen Container ist auf „Container“ festgelegt | Öffentlicher Zugriff für einen Container ist auf „Blob“ festgelegt |
|--|--|--|--|
| Öffentlicher Zugriff wird für das Speicherkonto verweigert | Kein öffentlicher Zugriff auf die Container im Speicherkonto | Kein öffentlicher Zugriff auf die Container im Speicherkonto – die Einstellung für das Speicherkonto überschreibt die Containereinstellung. | Kein öffentlicher Zugriff auf die Container im Speicherkonto – die Einstellung für das Speicherkonto überschreibt die Containereinstellung. |
| Öffentlicher Zugriff wird für das Speicherkonto zugelassen (Standardeinstellung) | Kein öffentlicher Zugriff auf diesen Container (Standardkonfiguration) | Der öffentliche Zugriff ist für diesen Container und dessen Blobs zulässig. | Der öffentliche Zugriff auf Blobs in diesem Container ist zulässig, jedoch nicht auf den Container selbst. |

## <a name="allow-or-disallow-public-read-access-for-a-storage-account"></a>Zulassen oder Verweigern des öffentlichen Lesezugriffs für ein Speicherkonto

Standardmäßig sind Speicherkonten so konfiguriert, dass ein Benutzer mit entsprechenden Berechtigungen den öffentlichen Zugriff auf Container aktivieren kann. Wenn der öffentliche Zugriff zugelassen wird, kann ein Benutzer mit entsprechenden Berechtigungen die Containereinstellung für den öffentlichen Zugriff so ändern, dass anonymer öffentlicher Zugriff auf die Daten in diesem Container aktiviert wird. Für Blobdaten ist der öffentliche Zugriff nicht verfügbar, sofern der Benutzer nicht in einem zusätzlichen Schritt explizit die Containereinstellung für den öffentlichen Zugriff konfiguriert.

Beachten Sie, dass der öffentliche Zugriff auf einen Container standardmäßig deaktiviert ist und explizit konfiguriert werden muss, damit anonyme Anforderungen zugelassen werden. Unabhängig von der Einstellung für das Speicherkonto sind Ihre Daten niemals für den öffentlichen Zugriff verfügbar, sofern nicht ein Benutzer mit entsprechenden Berichtigungen den öffentlichen Zugriff auf den Container in einem zusätzlichen Schritt aktiviert.

Wenn Sie den öffentlichen Zugriff für das Speicherkonto nicht zulassen, wird der anonyme Zugriff auf alle Container und Blobs in diesem Konto verhindert. Wenn der öffentliche Zugriff für das Konto nicht zulässig ist, kann die Einstellung für den öffentlichen Zugriff für einen Container nicht so konfiguriert werden, dass anonymer Zugriff zugelassen wird. Zur Verbesserung der Sicherheit empfiehlt Microsoft, den öffentlichen Zugriff auf Ihre Speicherkonten zu verweigern, wenn das Szenario nicht erfordert, dass Benutzer anonym auf Blobressourcen zugreifen.

> [!IMPORTANT]
> Durch Verweigerung des öffentlichen Zugriffs für ein Speicherkonto werden die Einstellungen für den öffentlichen Zugriff für alle Container in diesem Konto außer Kraft gesetzt. Wenn der öffentliche Zugriff für das Speicherkonto verweigert wird, tritt bei künftigen anonymen Anforderungen für dieses Konto ein Fehler auf. Bevor Sie diese Einstellung ändern, sollten Sie sich mit den Auswirkungen auf Clientanwendung befassen, die ggf. anonym auf Daten in Ihrem Speicherkonto zugreifen. Weitere Informationen finden Sie unter [Verhindern des anonymem, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-prevent.md).

Verwenden Sie das Azure-Portal oder die Azure CLI zum Konfigurieren der Eigenschaft **blobPublicAccess** des Kontos, um den öffentlichen Zugriff für ein Speicherkonto zu zuzulassen oder zu verweigern. Diese Eigenschaft ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie unter [Speicherkontoübersicht](../common/storage-account-overview.md).

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Befolgen Sie diese Schritte, um den öffentlichen Zugriff für ein Speicherkonto im Azure-Portal zuzulassen oder zu verweigern:

1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Suchen Sie unter **Einstellungen** die Einstellung **Konfiguration**.
1. Legen Sie **Öffentlicher Blobzugriff** auf **Aktiviert** oder **Deaktiviert** fest.

    :::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Screenshot: öffentlichen Blobzugriff für das Konto zulassen oder verweigern":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wenn Sie den öffentlichen Zugriff für ein Speicherkonto mithilfe der Azure CLI zulassen oder verweigern möchten, rufen Sie zunächst die Ressourcen-ID für Ihr Speicherkonto über den Befehl [az resource show](/cli/azure/resource#az-resource-show) ab. Rufen Sie danach den Befehl [az resource update](/cli/azure/resource#az-resource-update) auf, um die Eigenschaft **allowBlobPublicAccess** für das Speicherkonto festzulegen. Legen Sie die Eigenschaft **allowBlobPublicAccess** auf true fest, um den öffentlichen Zugriff zuzulassen, und auf **false**, um diesen zu verweigern.

Im folgenden Beispiel wird der Zugriff auf öffentliche Blobs für das Speicherkonto verweigert. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

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

---

> [!NOTE]
> Die Verweigerung des öffentlichen Zugriffs auf ein Speicherkonto wirkt sich nicht auf statische Websites aus, die in diesem Speicherkonto gehostet werden. Auf den Container **$web** kann immer öffentlich zugegriffen werden.

## <a name="check-whether-public-access-is-allowed-for-a-storage-account"></a>Überprüfen des öffentlichen Zugriffs für ein Speicherkonto

Wenn Sie überprüfen möchten, ob der öffentliche Zugriff für ein Speicherkonto zulässig ist, können Sie den Wert der Eigenschaft **allowBlobPublicAccess** abrufen. Über den Azure Resource Graph-Explorer können Sie diese Eigenschaft für eine große Anzahl von Speicherkonten gleichzeitig überprüfen.

> [!IMPORTANT]
> Die Eigenschaft **allowBlobPublicAccess** wird nicht standardmäßig festgelegt und gibt erst dann einen Wert zurück, wenn Sie sie explizit festgelegt haben. Das Speicherkonto lässt den öffentlichen Zugriff zu, wenn der Eigenschaftswert **NULL** oder **true** ist.

### <a name="check-whether-public-access-is-allowed-for-a-single-storage-account"></a>Überprüfen des öffentlichen Zugriffs für ein einzelnes Speicherkonto

Wenn Sie mithilfe der Azure CLI überprüfen möchten, ob der öffentliche Zugriff für ein einzelnes Speicherkonto zulässig ist, können Sie den Befehl [az resource show](/cli/azure/resource#az-resource-show) ausführen und die Eigenschaft **allowBlobPublicAccess** abfragen:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

### <a name="check-whether-public-access-is-allowed-for-a-set-of-storage-accounts"></a>Überprüfen des öffentlichen Zugriffs für mehrere Speicherkonten

Wenn Sie effizient überprüfen möchten, ob der öffentliche Zugriff für bestimmte Speicherkonten zulässig ist, können Sie den Azure Resource Graph-Explorer im Azure-Portal verwenden. Weitere Informationen zur Verwendung des Resource Graph-Explorers finden Sie unter [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](/azure/governance/resource-graph/first-query-portal).

Wenn Sie die folgende Abfrage im Resource Graph-Explorer ausführen, wird eine Liste der Speicherkonten zurückgegeben und für jedes Konto der Wert der Eigenschaft **allowBlobPublicAccess** angezeigt:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend allowBlobPublicAccess = parse_json(properties).allowBlobPublicAccess
| project subscriptionId, resourceGroup, name, allowBlobPublicAccess
| order by subscriptionId, resourceGroup, name asc
```

## <a name="set-the-public-access-level-for-a-container"></a>Festlegen der öffentlichen Zugriffsebene auf einen Container

Wenn anonyme Benutzer Lesezugriff auf den Container und auf die zugehörigen Blobs erhalten sollen, müssen Sie zunächst den öffentlichen Zugriff für das Speicherkonto zulassen und anschließend die öffentliche Zugriffsebene für den Container festlegen. Wenn der öffentliche Zugriff für das Speicherkonto verweigert wird, können Sie den öffentlichen Zugriff für einen Container nicht konfigurieren.

Wenn der öffentliche Zugriff für ein Speicherkonto zulässig ist, können Sie einen Container mit den folgenden Berechtigungen konfigurieren:

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

Wenn der öffentliche Zugriff für das Speicherkonto verweigert wird, kann die öffentliche Zugriffsebene eines Containers nicht festgelegt werden. Wenn Sie die öffentliche Zugriffsebene eines Containers festlegen möchten, stellen Sie fest, dass die Einstellung deaktiviert ist, da der öffentliche Zugriff für dieses Konto verweigert wird.

:::image type="content" source="media/anonymous-read-access-configure/container-public-access-blocked.png" alt-text="Screenshot: Festlegen der öffentlichen Zugriffsebene des Containers wird blockiert, wenn der öffentliche Zugriff verweigert wird":::

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

Wenn der öffentliche Zugriff für das Speicherkonto verweigert wird, kann die öffentliche Zugriffsebene eines Containers nicht festgelegt werden. Wenn Sie die öffentliche Zugriffsebene des Containers festlegen möchten, tritt ein Fehler auf, der besagt, dass der öffentliche Zugriff für das Speicherkonto unzulässig ist.

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
