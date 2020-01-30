---
title: Sichern von Azure-Dateifreigaben mit Azure CLI
description: Hier erfahren Sie, wie Sie Azure-Dateifreigaben mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) im Recovery Services-Tresor sichern.
ms.topic: conceptual
ms.date: 01/14/2020
ms.openlocfilehash: ff1d8c6245521d2d0262b0440177d65713058742
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844040"
---
# <a name="back-up-azure-file-shares-with-cli"></a>Sichern von Azure-Dateifreigaben mit der Befehlszeilenschnittstelle

Die Azure-Befehlszeilenschnittstelle (CLI) stellt eine Befehlszeilenumgebung zum Verwalten von Azure-Ressourcen bereit. Azure CLI ist ein großartiges Tool zum Erstellen einer benutzerdefinierten Automatisierung zur Verwendung von Azure-Ressourcen. In diesem Artikel wird erläutert, wie Sie Azure-Dateifreigaben mit Azure CLI sichern. Sie können diese Schritte auch mit [Azure PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-afs-automation) oder im [Azure-Portal](backup-afs.md) ausführen.

Am Ende dieses Tutorials erfahren Sie, wie Sie die folgenden Vorgänge mit Azure CLI ausführen:

* Erstellen eines Recovery Services-Tresors
* Aktivieren der Sicherung für Azure-Dateifreigaben
* Auslösen einer bedarfsgesteuerten Sicherung für Dateifreigaben

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, benötigen Sie mindestens die Version 2.0.18 der Azure-Befehlszeilenschnittstelle. Führen Sie zum Ermitteln der CLI-Version `run az --version` aus. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Ein Recovery Services-Tresor ist eine Entität, die eine konsolidierte Ansicht und Verwaltungsfunktion für alle Sicherungselemente bereitstellt. Wenn der Sicherungsauftrag für eine geschützte Ressource ausgeführt wird, wird im Recovery Services-Tresor ein Wiederherstellungspunkt erstellt. Sie können einen dieser Wiederherstellungspunkte dann verwenden, um Daten für einen bestimmten Zeitpunkt wiederherzustellen.

Führen Sie die nachstehenden Schritte aus, um einen Recovery Services-Tresors zu erstellen:

1. Ein Tresor wird in einer Ressourcengruppe platziert. Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) eine neue Ressourcengruppe, wenn noch keine vorhanden ist. In diesem Tutorial wird die neue Ressourcengruppe *azurefiles* in der Region "USA, Osten" erstellt.

    ```azurecli-interactive
    az group create --name AzureFiles --location eastus --output table
    ```

    ```output
    Location    Name
    ----------  ----------
    eastus      AzureFiles
    ```

2. Verwenden Sie das Cmdlet [az backup vault create](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-create), um den Tresor zu erstellen. Geben Sie denselben Speicherort für den Tresor an, der für die Ressourcengruppe verwendet wurde.

    Im folgenden Beispiel wird ein Recovery Services-Tresor mit dem Namen *azurefilesvault* in der Region "USA, Osten" erstellt.

    ```azurecli-interactive
    az backup vault create --resource-group azurefiles --name azurefilesvault --location eastus --output table
    ```

    ```output
    Location    Name                ResourceGroup
    ----------  ----------------    ---------------
    eastus      azurefilesvault     azurefiles
    ```

3. Geben Sie den für den Tresorspeicher zu verwendenden Redundanztyp an. Sie können [lokal redundanten Speicher](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs) oder [georedundanten Speicher](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs) verwenden.

    Das folgende Beispiel legt die Speicherredundanzoption für *azurefilesvault* unter Verwendung des Cmdlets [az backup vault backup-properties set](https://docs.microsoft.com/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set) auf **Georedundant** fest.

    ```azurecli-interactive
    az backup vault backup-properties set --name azurefilesvault --resource-group azurefiles --backup-storage-redundancy Georedundant
    ```

    Um zu überprüfen, ob der Tresor erfolgreich erstellt wurde, rufen Sie mit dem Cmdlet [az backup vault show](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-show) Details zum Tresor ab. Im folgenden Beispiel werden die Details zu dem mit den obigen Schritten erstellten Element *azurefilesvault* angezeigt.

    ```azurecli-interactive
    az backup vault show --name azurefilesvault --resource-group azurefiles --output table
    ```

    Die Ausgabe entspricht etwa folgender Antwort:

    ```output
    Location     Name               ResourceGroup
    ----------   ---------------    ---------------
    eastus       azurefilesvault    azurefiles
    ```

## <a name="enable-backup-for-azure-file-shares"></a>Aktivieren der Sicherung für Azure-Dateifreigaben

In diesem Abschnitt wird davon ausgegangen, dass Sie bereits über eine Azure-Dateifreigabe verfügen, für die Sie die Sicherung konfigurieren möchten. Wenn Sie über keine Azure-Dateifreigabe verfügen, erstellen Sie eine mit dem Befehl [az storage share create](https://docs.microsoft.com/cli/azure/storage/share?view=azure-cli-latest#az-storage-share-create).

Zum Aktivieren der Sicherung für Dateifreigaben müssen Sie eine Schutzrichtlinie erstellen, die festlegt, wann ein Sicherungsauftrag ausgeführt wird und wie lange Wiederherstellungspunkte gespeichert werden. Mit dem Cmdlet [az backup policy create](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) können Sie eine Sicherungsrichtlinie erstellen.

Im folgenden Beispiel wird das Cmdlet [az backup protection enable-for-azurefileshare](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-enable-for-azurefileshare) verwendet, um die Sicherung für die Dateifreigabe *azurefiles* im Speicherkonto *afsaccount* unter Verwendung der Sicherungsrichtlinie *Zeitplan 1* zu aktivieren:

```azurecli-interactive
az backup protection enable-for-azurefileshare --vault-name azurefilesvault --resource-group  azurefiles --policy-name schedule1 --storage-account afsaccount --azure-file-share azurefiles  --output table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
0caa93f4-460b-4328-ac1d-8293521dd928  azurefiles
```

Das Attribut **Name** in der Ausgabe entspricht dem Namen des Auftrags, der vom Sicherungsdienst für Ihre **Sicherungsaktivierung** erstellt wird. Verwenden Sie zum Nachverfolgen des Auftragsstatus das Cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show).

## <a name="trigger-an-on-demand-backup-for-file-share"></a>Auslösen einer bedarfsgesteuerten Sicherung für Dateifreigaben

Wenn Sie eine bedarfsgesteuerte Sicherung für die Dateifreigabe auslösen möchten, anstatt zu warten, bis die Sicherungsrichtlinie den Auftrag zum geplanten Zeitpunkt ausführt, verwenden Sie das Cmdlet [az backup protection backup-now](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-backup-now).

Sie müssen die folgenden Parameter angeben, um eine bedarfsgesteuerte Sicherung zu starten:

* **--container-name** ist der Name des Speicherkontos, das die Dateifreigabe hostet. Um den **Namen** oder den **Anzeigenamen** Ihres Containers abzurufen, verwenden Sie den Befehl [az backup container list](/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list).
* **--item-name** ist der Name der Dateifreigabe, für die Sie eine bedarfsgesteuerte Sicherung auslösen möchten. Um den **Namen** oder den **Anzeigenamen** Ihres gesicherten Elements abzurufen, verwenden Sie den Befehl [az backup item list](https://docs.microsoft.com/cli/azure/backup/item?view=azure-cli-latest#az-backup-item-list).
* **--retain-until** gibt das Datum an, bis wann Sie der Wiederherstellungspunkt aufbewahrt werden soll. Der Wert muss im UTC-Zeitformat (TT-MM-JJJJ) festgelegt werden.

Im folgenden Beispiel wird eine bedarfsgesteuerte Sicherung für die Dateifreigabe *azuresfiles* im Speicherkonto *afsaccount* mit Aufbewahrung bis *20-01-2020* ausgelöst.

```azurecli-interactive
az backup protection backup-now --vault-name azurefilesvault --resource-group azurefiles --container-name "StorageContainer;Storage;AzureFiles;afsaccount" --item-name "AzureFileShare;azurefiles" --retain-until 20-01-2020 --output table
```

```output
Name                                  ResourceGroup
------------------------------------  ---------------
9f026b4f-295b-4fb8-aae0-4f058124cb12  azurefiles
```

Das Attribut **Name** in der Ausgabe entspricht dem Namen des Auftrags, der vom Sicherungsdienst für Ihre bedarfsgesteuerte Sicherung erstellt wird. Verwenden Sie zum Nachverfolgen des Auftragsstatus das Cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show).

## <a name="next-steps"></a>Nächste Schritte

* [Wiederherstellen von Azure-Dateifreigaben mit der Befehlszeilenschnittstelle](restore-afs-cli.md)
* [Verwalten von Azure-Dateifreigabensicherungen mit der Befehlszeilenschnittstelle](manage-afs-backup-cli.md)
