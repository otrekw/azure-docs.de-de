---
title: Erstellen einer Azure-Dateifreigabe
titleSuffix: Azure Files
description: Hier erfahren Sie, wie Sie über das Azure-Portal, mithilfe von PowerShell oder mit der Azure CLI eine Azure-Dateifreigabe erstellen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 2/22/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ed6abbac7c5953eaec4fa4584248d0d98b49ba63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596890"
---
# <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe
Um eine Azure-Dateifreigabe zu erstellen, müssen Sie drei Fragen zur Verwendung beantworten:

- **Welche Leistungsanforderungen gelten für Ihre Azure-Dateifreigabe?**  
    Azure Files verfügt über Standard-Dateifreigaben, die auf festplattenbasierter Hardware (HDD-basiert) gehostet werden, und über Premium-Dateifreigaben, die auf SSD-Datenträgerhardware (SSD-basiert) gehostet werden.

- **Welche Größe benötigen Sie für Dateifreigaben?**  
    Standard-Dateifreigaben können bis zu 100 TiB groß sein, aber dieses Feature ist standardmäßig nicht aktiviert. Falls Sie eine Dateifreigabe benötigen, die größer als 5 TiB ist, müssen Sie das Feature für große Dateifreigaben für Ihr Speicherkonto aktivieren. Premium-Dateifreigaben können ohne spezielle Einstellung bis zu 100 TiB groß sein, aber sie werden gesondert bereitgestellt. Es wird keine nutzungsbasierte Bezahlung wie bei Standard-Dateifreigaben verwendet. Dies bedeutet, dass sich die Gesamtkosten für den Speicher erhöhen, wenn eine Dateifreigabe bereitgestellt wird, die Ihren Bedarf deutlich überschreitet.

- **Welche Redundanzanforderungen gelten für Ihre Azure-Dateifreigabe?**  
    Standard-Dateifreigaben ermöglichen die lokal redundante (LRS), zonenredundante (ZRS), georedundante (GRS) und geozonenredundante (GZRS) Speicherung. Das Feature für große Dateifreigaben wird aber nur für lokal redundante und zonenredundante Dateifreigaben unterstützt. Für Premium-Dateifreigaben wird keine Form von Georedundanz unterstützt.

    Premium-Dateifreigaben sind mit lokaler Redundanz in den meisten Regionen verfügbar, in denen Speicherkonten angeboten werden. Mit Zonenredundanz sind sie in einer kleineren Teilmenge der Regionen verfügbar. Um herauszufinden, ob Premium-Dateifreigaben derzeit in Ihrer Region verfügbar sind, lesen Sie die Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=storage) für Azure. Informationen zu Regionen, die ZRS unterstützen, finden Sie unter [Azure Storage-Redundanz](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

Weitere Informationen zu diesen drei Optionen finden Sie unter [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md).

## <a name="prerequisites"></a>Voraussetzungen
- In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Azure-Abonnement erstellt haben. Wenn Sie noch kein Abonnement haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Falls Sie Azure PowerShell verwenden möchten, [installieren Sie die neueste Version](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Falls Sie die Azure CLI verwenden möchten, [installieren Sie die neueste Version](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Azure-Dateifreigaben werden in *Speicherkonten* bereitgestellt, bei denen es sich um Objekte der obersten Ebene handelt, die einen freigegebenen Speicherpool darstellen. Dieser Speicherpool kann verwendet werden, um mehrere Dateifreigaben bereitzustellen. 

Azure unterstützt mehrere Arten von Speicherkonten für unterschiedliche Kundenszenarien, aber es gibt zwei Hauptarten von Speicherkonten für Azure Files. Welche Art von Speicherkonto Sie erstellen müssen, hängt davon ab, ob Sie eine Standard- oder Premium-Dateifreigabe erstellen möchten: 

- **Speicherkonten vom Typ „Universell Version 2 (GPv2)“** : GPv2-Speicherkonten ermöglichen Ihnen die Bereitstellung von Azure-Dateifreigaben auf Standard- bzw. festplattenbasierter Hardware (HDD-basiert). Neben Azure-Dateifreigaben können in GPv2-Speicherkonten auch andere Speicherressourcen wie Blobcontainer, Warteschlangen oder Tabellen gespeichert werden. 

- **FileStorage-Speicherkonten**: FileStorage-Speicherkonten ermöglichen Ihnen die Bereitstellung von Azure-Dateifreigaben auf SSD-basierter Hardware (Premium/Solid State Drive). FileStorage-Konten können nur zum Speichern von Azure-Dateifreigaben verwendet werden. In einem FileStorage-Konto können keine anderen Speicherressourcen (Blobcontainer, Warteschlangen, Tabellen usw.) bereitgestellt werden.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Wählen Sie zum Erstellen eines Speicherkontos über das Azure-Portal im Dashboard die Option **+ Ressource erstellen** aus. Suchen Sie im angezeigten Azure Marketplace-Suchfenster nach **Speicherkonto**, und wählen Sie das entsprechende Suchergebnis aus. Sie gelangen auf eine Übersichtsseite für Speicherkonten. Wählen Sie **Erstellen** aus, um den Assistenten für die Erstellung von Speicherkonten zu starten.

![Screenshot: Option zur schnellen Erstellung eines Speicherkontos in einem Browser](media/storage-how-to-create-file-share/create-storage-account-0.png)

#### <a name="the-basics-section"></a>Abschnitt „Grundlagen“
Der erste Abschnitt bei der Erstellung eines Speicherkontos hat die Bezeichnung **Grundlagen**. Er enthält alle erforderlichen Felder für die Erstellung eines Speicherkontos. Stellen Sie bei der Erstellung eines GPv2-Speicherkontos sicher, dass das Optionsfeld **Leistung** auf *Standard* festgelegt und in der Dropdownliste **Kontoart** die Option *StorageV2 (universell v2)* ausgewählt ist.

![Screenshot: Optionsfeld „Leistung“ mit Auswahl von „Standard“ und Auswahl von „StorageV2 (universell v2)“ für „Kontoart“](media/storage-how-to-create-file-share/create-storage-account-1.png)

Stellen Sie zum Erstellen eines FileStorage-Speicherkontos sicher, dass das Optionsfeld **Leistung** auf *Premium* festgelegt und in der Dropdownliste **Kontoart** die Option *FileStorage* ausgewählt ist.

![Screenshot: Optionsfeld „Leistung“ mit Auswahl von „Premium“ und Auswahl von „FileStorage“ für „Kontoart“](media/storage-how-to-create-file-share/create-storage-account-2.png)

Die anderen Felder für die Grundlagen sind unabhängig von der Auswahl des Speicherkontos:
- **Abonnement**: Das Abonnement, unter dem das Speicherkonto bereitgestellt werden soll. 
- **Ressourcengruppe**: Die Ressourcengruppe, in der das Speicherkonto bereitgestellt werden soll. Sie können entweder eine neue Ressourcengruppe erstellen oder eine vorhandene verwenden. Eine Ressourcengruppe ist ein logischer Container zur Gruppierung Ihrer Azure-Dienste. Beim Erstellen eines Speicherkontos haben Sie die Wahlmöglichkeit, entweder eine neue Ressourcengruppe zu erstellen oder eine vorhandene Ressourcengruppe zu verwenden.
- **Speicherkontoname**: Der Name der Speicherkontoressource, die erstellt werden soll. Dieser Name muss global eindeutig sein, aber ansonsten gelten keine weiteren Einschränkungen. Der Name des Speicherkontos wird als Servername verwendet, wenn Sie eine Azure-Dateifreigabe per SMB einbinden.
- **Standort**: Die Region, in der das Speicherkonto bereitgestellt werden soll. Dies kann die Region sein, die der Ressourcengruppe zugeordnet ist, oder eine andere verfügbare Region.
- **Replikation**: Die Bezeichnung dieser Option lautet zwar „Replikation“, aber es geht eigentlich um **Redundanz**. Hier wird die gewünschte Redundanzebene angegeben: lokale Redundanz (LRS), Zonenredundanz (ZRS), Georedundanz (GRS) und Geozonenredundanz. Diese Dropdownliste enthält auch Optionen für Georedundanz mit Lesezugriff (RA-GRS) und Geozonenredundanz mit Lesezugriff (RA-GZRS), die nicht für Azure-Dateifreigaben gelten. Alle Dateifreigaben, die mit diesen Optionen in einem Speicherkonto erstellt werden, sind entweder georedundant oder geozonenredundant. Je nach Region oder ausgewähltem Speicherkontotyp sind einige Redundanzoptionen ggf. nicht zulässig.
- **Zugriffsebene**: Dieses Feld gilt nicht für Azure Files. Sie können also eines der beiden Optionsfelder auswählen.

#### <a name="the-networking-blade"></a>Blatt „Netzwerk“
Im Abschnitt „Netzwerk“ können Sie die Netzwerkoptionen konfigurieren. Diese Einstellungen sind für die Erstellung des Speicherkontos optional und können bei Bedarf zu einem späteren Zeitpunkt konfiguriert werden. Weitere Informationen zu diesen Optionen finden Sie unter [Azure Files – Überlegungen zum Netzwerkbetrieb](storage-files-networking-overview.md).

#### <a name="the-advanced-blade"></a>Blatt „Erweitert“
Der Abschnitt „Erweitert“ enthält mehrere wichtige Einstellungen für Azure-Dateifreigaben:

- **Sichere Übertragung erforderlich**: Dieses Feld gibt an, ob für die Kommunikation mit dem Speicherkonto in eingehender Richtung eine Verschlüsselung während der Übertragung erforderlich ist. Wir empfehlen Ihnen, diese Option aktiviert zu lassen. Falls Sie SMB 2.1-Unterstützung benötigen, müssen Sie sie aber deaktivieren. Bei einer Deaktivierung der Verschlüsselung empfehlen wir Ihnen, den Speicherkontozugriff auf ein virtuelles Netzwerk mit Dienstendpunkten bzw. privaten Endpunkten einzuschränken.
- **Große Dateifreigaben**: Mit diesem Feld wird für das Speicherkonto die Nutzung von Dateifreigaben mit einer Größe von bis zu 100 TiB ermöglicht. Wenn Sie dieses Feature aktivieren, wird Ihr Speicherkonto auf die Nutzung von lokal redundantem und zonenredundantem Speicher beschränkt. Nachdem für ein GPv2-Speicherkonto große Dateifreigaben aktiviert wurden, können Sie diese Funktion nicht mehr deaktivieren. Für Speicherkonten vom Typ „FileStorage“ (Speicherkonten für Premium-Dateifreigaben) ist diese Option nicht vorhanden, da alle Premium-Dateifreigaben auf bis zu 100 TiB hochskaliert werden können. 

![Screenshot: Wichtige erweiterte Einstellungen für Azure Files](media/storage-how-to-create-file-share/create-storage-account-3.png)

Die anderen Einstellungen, die auf der Registerkarte „Erweitert“ verfügbar sind (vorläufiges Löschen von Blobs, hierarchischer Namespace für Azure Data Lake Storage Gen2 und NFSv3 für Blobspeicher) gelten nicht für Azure Files.

#### <a name="tags"></a>`Tags`
Tags sind Name/Wert-Paare, die Ihnen das Kategorisieren von Ressourcen und die Anzeige einer konsolidierten Abrechnung ermöglichen, indem Sie dasselbe Tag auf mehrere Ressourcen und Ressourcengruppen anwenden. Sie sind optional und können angewendet werden, nachdem das Speicherkonto erstellt wurde.

#### <a name="review--create"></a>Bewerten + erstellen
Der letzte Schritt zur Erstellung des Speicherkontos ist die Auswahl der Schaltfläche **Erstellen** auf der Registerkarte **Bewerten + erstellen**. Diese Schaltfläche ist nicht verfügbar, wenn nicht alle erforderlichen Felder für ein Speicherkonto ausgefüllt sind.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Wir verwenden das Cmdlet `New-AzStorageAccount`, um mit PowerShell ein Speicherkonto zu erstellen. Dieses Cmdlet hat viele Optionen, von denen nur die erforderlichen Optionen angezeigt werden. Weitere Informationen zu den erweiterten Optionen finden Sie in der [Dokumentation zum Cmdlet `New-AzStorageAccount`](/powershell/module/az.storage/new-azstorageaccount).

Zur Vereinfachung der Erstellung des Speicherkontos und der nachfolgenden Dateifreigabe speichern wir Parameter in Variablen. Sie können den Inhalt der Variablen durch beliebige Werte ersetzen. Beachten Sie hierbei aber, dass der Name des Speicherkontos global eindeutig sein muss.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$storageAccountName = "mystorageacct$(Get-Random)"
$region = "westus2"
```

Wir verwenden den folgenden Befehl, um ein Speicherkonto zu erstellen, mit dem Azure-Dateifreigaben vom Typ „Standard“ gespeichert werden können. Der Parameter `-SkuName` bezieht sich auf den gewünschten Redundanztyp. Wenn Sie ein georedundantes oder geozonenredundantes Speicherkonto verwenden möchten, müssen Sie auch den Parameter `-EnableLargeFileShare` entfernen.

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Standard_LRS `
    -Location $region `
    -Kind StorageV2 `
    -EnableLargeFileShare
```

Wir verwenden den folgenden Befehl, um ein Speicherkonto zu erstellen, mit dem Azure-Dateifreigaben vom Typ „Premium“ gespeichert werden können. Beachten Sie, dass sich der Parameter `-SkuName` geändert hat und nun sowohl `Premium` als auch die gewünschte Redundanzebene „Lokale Redundanz“ (`LRS`) enthält. Für den Parameter `-Kind` wird `FileStorage` anstelle von `StorageV2` verwendet, da Premium-Dateifreigaben nicht unter einem GPv2-Speicherkonto, sondern unter einem FileStorage-Speicherkonto erstellt werden müssen.

```azurepowershell-interactive
$storAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $region `
    -Kind FileStorage 
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Um ein Speicherkonto über die Azure-Befehlszeilenschnittstelle zu erstellen, verwenden wir den Befehl „az storage account create“. Dieser Befehl hat viele Optionen, von denen nur die erforderlichen Optionen angezeigt werden. Weitere Informationen zu den erweiterten Optionen finden Sie in der [Dokumentation zum Befehl `az storage account create`](/cli/azure/storage/account).

Zur Vereinfachung der Erstellung des Speicherkontos und der nachfolgenden Dateifreigabe speichern wir Parameter in Variablen. Sie können den Inhalt der Variablen durch beliebige Werte ersetzen. Beachten Sie hierbei aber, dass der Name des Speicherkontos global eindeutig sein muss.

```azurecli-interactive
resourceGroupName="myResourceGroup"
storageAccountName="mystorageacct$RANDOM"
region="westus2"
```

Wir verwenden den folgenden Befehl, um ein Speicherkonto zu erstellen, mit dem Azure-Dateifreigaben vom Typ „Standard“ gespeichert werden können. Der Parameter `--sku` bezieht sich auf den gewünschten Redundanztyp. Wenn Sie ein georedundantes oder geozonenredundantes Speicherkonto verwenden möchten, müssen Sie auch den Parameter `--enable-large-file-share` entfernen.

```azurecli-interactive
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind StorageV2 \
    --sku Standard_ZRS \
    --enable-large-file-share \
    --output none
```

Wir verwenden den folgenden Befehl, um ein Speicherkonto zu erstellen, mit dem Azure-Dateifreigaben vom Typ „Premium“ gespeichert werden können. Beachten Sie, dass sich der Parameter `--sku` geändert hat und nun sowohl `Premium` als auch die gewünschte Redundanzebene „Lokale Redundanz“ (`LRS`) enthält. Für den Parameter `--kind` wird `FileStorage` anstelle von `StorageV2` verwendet, da Premium-Dateifreigaben nicht unter einem GPv2-Speicherkonto, sondern unter einem FileStorage-Speicherkonto erstellt werden müssen.

```azurecli-interactive
az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --kind FileStorage \
    --sku Premium_LRS \
    --output none
```

---

## <a name="create-file-share"></a>Erstellen einer Dateifreigabe
Nachdem Sie Ihr Speicherkonto erstellt haben, fehlt nur noch die Erstellung der Dateifreigabe. Dieser Prozess ist unabhängig davon, ob eine Premium- oder Standard-Dateifreigabe verwendet wird, größtenteils identisch. Der Hauptunterschied ist das **Kontingent** und die sich daraus ergebenden Konsequenzen.

Bei Standard-Dateifreigaben ist dies eine Obergrenze für die Azure-Dateifreigabe, die von Endbenutzern nicht überschritten werden darf. Der Hauptgrund bei der Verwendung eines Kontingents für eine Standard-Dateifreigabe ist das Budget: „Ich möchte nicht, dass die Größe dieser Dateifreigabe diese Grenze überschreitet.“ Wenn kein Kontingent angegeben wird, kann eine Standard-Dateifreigabe eine Größe von bis zu 100 TiB haben (bzw. 5 TiB, wenn die Eigenschaft für große Dateifreigaben für ein Speicherkonto nicht festgelegt wurde).

Für Premium-Dateifreigaben ist mit dem Kontingent die **bereitgestellte Größe** gemeint. Die bereitgestellte Größe ist die Menge, die Ihnen unabhängig von der tatsächlichen Nutzung berechnet wird. Es ist ratsam, beim Bereitstellen einer Premium-Dateifreigabe zwei Faktoren zu berücksichtigen: 1) zukünftiges Wachstum der Freigabe aus Sicht der Speicherplatzauslastung und 2) erforderlicher IOPS-Wert für Ihre Workload. Für jedes bereitgestellte GiB haben Sie Anspruch auf zusätzliche reservierte bzw. Burst-IOPS-Kapazität. Weitere Informationen zur Planung einer Premium-Dateifreigabe finden Sie im Abschnitt zum [Bereitstellen von Premium-Dateifreigaben](storage-files-planning.md#understanding-provisioning-for-premium-file-shares).

# <a name="portal"></a>[Portal](#tab/azure-portal)
Wenn Sie Ihr Speicherkonto gerade erstellt haben, können Sie vom Bereitstellungsbildschirm dorthin navigieren, indem Sie **Zu Ressource wechseln** auswählen. Falls Sie das Speicherkonto schon vorher erstellt haben, können Sie darauf über die Ressourcengruppe zugreifen, in der es enthalten ist. Wählen Sie im Speicherkonto die Kachel **Dateifreigaben** aus. (Sie können auch über das Inhaltsverzeichnis des Speicherkontos zu **Dateifreigaben** navigieren.)

![Screenshot: Kachel „Dateifreigaben“](media/storage-how-to-create-file-share/create-file-share-1.png)

In der Liste mit den Dateifreigaben sollten alle Dateifreigaben angezeigt werden, die Sie unter diesem Speicherkonto zuvor erstellt haben. Die Tabelle ist leer, falls noch keine Dateifreigaben erstellt wurden. Wählen Sie **+ Dateifreigabe** aus, um eine neue Dateifreigabe zu erstellen.

Das Blatt „Neue Dateifreigabe“ wird angezeigt. Füllen Sie die Felder des Blatts „Neue Dateifreigabe“ aus, um eine Dateifreigabe zu erstellen:

- **Name**: Der Name der Dateifreigabe, die erstellt werden soll.
- **Kontingent**: Das Kontingent der Dateifreigabe für Standard-Dateifreigaben. Dies ist die bereitgestellte Dateifreigabegröße für Premium-Dateifreigaben.

Wählen Sie **Erstellen** aus, um die Erstellung der neuen Freigabe abzuschließen. Beachten Sie Folgendes: Wenn sich Ihr Speicherkonto in einem virtuellen Netzwerk befindet, ist die Erstellung einer Azure-Dateifreigabe nur erfolgreich, wenn Ihr Client in demselben virtuellen Netzwerk angeordnet ist. Sie können diese Point-in-Time-Einschränkung umgehen, indem Sie das Azure PowerShell-Cmdlet `New-AzRmStorageShare` verwenden.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Sie können die Azure-Dateifreigabe mit dem Cmdlet [`New-AzRmStorageShare`](/powershell/module/az.storage/New-AzRmStorageShare) erstellen. Bei den folgenden PowerShell-Befehlen wird davon ausgegangen, dass Sie die Variablen `$resourceGroupName` und `$storageAccountName` gemäß der obigen Definition im Abschnitt zum „Erstellen eines Speicherkontos mit Azure PowerShell“ festgelegt haben. 

> [!Important]  
> Bei Premium-Dateifreigaben verweist der Parameter `-QuotaGiB` auf die bereitgestellte Größe der Dateifreigabe. Die bereitgestellte Größe der Dateifreigabe ist die Menge, die Ihnen unabhängig von der Nutzung berechnet wird. Standard-Dateifreigaben werden nicht basierend auf der Nutzung, sondern auf der bereitgestellten Größe berechnet.

```azurepowershell-interactive
$shareName = "myshare"

New-AzRmStorageShare `
    -ResourceGroupName $resourceGroupName `
    -StorageAccountName $storageAccountName `
    -Name $shareName `
    -QuotaGiB 1024 | Out-Null
```

> [!Note]  
> Der Name der Dateifreigabe darf nur Kleinbuchstaben enthalten. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter  [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://msdn.microsoft.com/library/azure/dn167011.aspx).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Bevor Sie mit der Azure CLI eine Azure-Dateifreigabe erstellen können, müssen Sie sich einen Speicherkontoschlüssel beschaffen, mit dem Sie die Autorisierung für den Vorgang zum Erstellen der Dateifreigabe durchführen können. Verwenden Sie hierfür den Befehl [`az storage account keys list`](/cli/azure/storage/account/keys):

```azurecli-interactive
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')
```

Wenn Sie im Besitz des Speicherkontoschlüssels sind, können Sie die Azure-Dateifreigabe mit dem Befehl [`az storage share create`](/cli/azure/storage/share) erstellen. 

> [!Important]  
> Bei Premium-Dateifreigaben verweist der Parameter `--quota` auf die bereitgestellte Größe der Dateifreigabe. Die bereitgestellte Größe der Dateifreigabe ist die Menge, die Ihnen unabhängig von der Nutzung berechnet wird. Standard-Dateifreigaben werden nicht basierend auf der Nutzung, sondern auf der bereitgestellten Größe berechnet.

```azurecli-interactive
shareName="myshare"

az storage share create \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --name $shareName \
    --quota 1024 \
    --output none
```

Für diesen Befehl tritt ein Fehler auf, wenn sich das Speicherkonto in einem virtuellen Netzwerk befindet und der Computer, auf dem Sie diesen Befehl aufrufen, nicht Teil des virtuellen Netzwerks ist. Sie können diese Point-in-Time-Einschränkung umgehen, indem Sie wie oben beschrieben das Azure PowerShell-Cmdlet `New-AzRmStorageShare` verwenden oder die Azure CLI auf einem Computer ausführen, der Teil des virtuellen Netzwerks ist (auch per VPN-Verbindung).

---

> [!Note]  
> Der Name der Dateifreigabe darf nur Kleinbuchstaben enthalten. Ausführliche Informationen zur Benennung von Dateifreigaben und Dateien finden Sie unter  [Benennen und Referenzieren von Freigaben, Verzeichnissen, Dateien und Metadaten](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="next-steps"></a>Nächste Schritte
- [Planen der Bereitstellung von Azure Files](storage-files-planning.md) oder [Planen der Bereitstellung der Azure-Dateisynchronisierung](storage-sync-files-planning.md). 
- [Netzwerkübersicht](storage-files-networking-overview.md).
- Führen Sie die Verbindungsherstellung und Einbindung einer Dateifreigabe unter [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) und [Linux](storage-how-to-use-files-linux.md) durch.