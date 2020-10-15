---
title: 'Tutorial: Wiederherstellen einer SAP HANA-Datenbank unter Azure per CLI'
description: In diesem Tutorial wird beschrieben, wie Sie SAP HANA-Datenbanken, die auf einem virtuellen Azure-Computer ausgeführt werden, über die Azure CLI aus einem Recovery Services-Tresor von Azure Backup wiederherstellen.
ms.topic: tutorial
ms.date: 12/4/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0e524bfe090f0d67b76c13e876f44e83986aeb9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91334802"
---
# <a name="tutorial-restore-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Tutorial: Wiederherstellen von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer über die Azure CLI

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Dokumentation wird beschrieben, wie Sie eine gesicherte SAP HANA-Datenbank auf einem virtuellen Azure-Computer über die Azure CLI wiederherstellen. Sie können [diese Schritte auch über das Azure-Portal ausführen](./sap-hana-db-restore.md).

Verwenden Sie [Azure Cloud Shell](tutorial-sap-hana-backup-cli.md), um die CLI-Befehle auszuführen.

In diesem Tutorial wird Folgendes vermittelt:

> [!div class="checklist"]
>
> * Anzeigen von Wiederherstellungspunkten für eine gesicherte Datenbank
> * Wiederherstellen einer Datenbank

In diesem Tutorial wird davon ausgegangen, dass Sie über eine per Azure Backup gesicherte SAP HANA-Datenbank verfügen, die auf einem virtuellen Azure-Computer ausgeführt wird. Falls Sie die Anleitung unter [Sichern einer SAP HANA-Datenbank in Azure mit der CLI](tutorial-sap-hana-backup-cli.md) verwendet haben, um Ihre SAP HANA-Datenbank zu sichern, nutzen Sie die folgenden Ressourcen:

* eine Ressourcengruppe mit dem Namen *saphanaResourceGroup*
* einen Tresor mit dem Namen *saphanaVault*
* einen geschützten Container mit dem Namen *VMAppContainer;Compute;saphanaResourceGroup;saphanaVM*
* eine gesicherte Datenbank bzw. ein anderes Element mit dem Namen *saphanadatabase;hxe;hxe*
* Ressourcen in der Region *westus2*

## <a name="view-restore-points-for-a-backed-up-database"></a>Anzeigen von Wiederherstellungspunkten für eine gesicherte Datenbank

Verwenden Sie das Cmdlet[az backup recoverypoint list](/cli/azure/backup/recoverypoint#az-backup-recoverypoint-show-log-chain) wie folgt, um die Liste mit allen Wiederherstellungspunkten anzuzeigen:

```azurecli-interactive
az backup recoverypoint list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
   --output table
```

Die Liste mit den Wiederherstellungspunkten sieht wie folgt aus:

```output
Name                      Time                               BackupManagementType   Item Name               RecoveryPointType
-------------------       ---------------------------------  ---------------------  ----------------------  ------------------
7660777527047692711       2019-12-10T04:00:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Full
7896624824685666836       2019-12-15T10:33:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Differential
DefaultRangeRecoveryPoint                                    AzureWorkload          SAPHanaDtabase;hxe;hxe  Log
```

Wie Sie sehen, enthält die obige Liste drei Wiederherstellungspunkte: jeweils einen für die vollständige, differenzielle und Protokollsicherung.

>[!NOTE]
>Sie können auch die Start- und Endpunkte für alle ununterbrochenen Protokollsicherungsketten anzeigen, indem Sie das Cmdlet [az backup recoverypoint show-log-chain](/cli/azure/backup/recoverypoint#az-backup-recoverypoint-show-log-chain) verwenden.

## <a name="prerequisites-to-restore-a-database"></a>Voraussetzungen für die Wiederherstellung einer Datenbank

Stellen Sie vor dem Wiederherstellen einer Datenbank sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Sie können die Datenbank nur in einer SAP HANA-Instanz wiederherstellen, die sich in derselben Region befindet.
* Die Zielinstanz muss beim gleichen Tresor wie die Quelle registriert sein.
* Mit Azure Backup ist es nicht möglich, zwei verschiedene SAP HANA-Instanzen auf derselben VM zu identifizieren. Daher ist das Wiederherstellen von Daten einer Instanz auf einer anderen auf demselben virtuellen Computer nicht möglich.

## <a name="restore-a-database"></a>Wiederherstellen einer Datenbank

Azure Backup kann auf virtuellen Azure-Computern ausgeführte SAP HANA-Datenbanken wie folgt wiederherstellen:

* Wiederherstellung eines bestimmten Datums oder einer bestimmten Uhrzeit (sekundengenau) mithilfe von Protokollsicherungen. Azure Backup ermittelt automatisch die geeigneten vollständige differenziellen Sicherungen und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten basierend auf dem ausgewählten Zeitpunkt benötigt werden.
* Wiederherstellung einer bestimmten vollständigen oder differenziellen Sicherung, um die Daten eines bestimmten Wiederherstellungspunkts wiederherzustellen.

Verwenden Sie zum Wiederherstellen einer Datenbank das Cmdlet [az restore restore-azurewl](/cli/azure/backup/restore#az-backup-restore-restore-azurewl). Hierfür ist ein Objekt für die Wiederherstellungskonfiguration als eines der Eingabeelemente erforderlich. Sie können dieses Objekt generieren, indem Sie das Cmdlet [az backup recoveryconfig show](/cli/azure/backup/recoveryconfig#az-backup-recoveryconfig-show) verwenden. Das Objekt für die Wiederherstellungskonfiguration enthält alle Details zur Durchführung einer Wiederherstellung. Hierbei ist ein Punkt der Wiederherstellungsmodus: **OriginalWorkloadRestore** oder **AlternateWorkloadRestore**.

>[!NOTE]
> **OriginalWorkloadRestore**: Die Daten werden auf derselben SAP HANA-Instanz wiederhergestellt, auf der sich auch die ursprüngliche Quelle befunden hat. Bei dieser Option wird die ursprüngliche Datenbank überschrieben. <br>
> **AlternateWorkloadRestore**: Die Datenbank wird an einem alternativen Speicherort wiederhergestellt, und die ursprüngliche Quelldatenbank bleibt erhalten.

## <a name="restore-to-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

Verwenden Sie **AlternateWorkloadRestore** als Wiederherstellungsmodus, wenn Sie eine Datenbank an einem anderen Speicherort wiederherstellen möchten. Anschließend müssen Sie den Wiederherstellungspunkt auswählen. Dies kann entweder ein früherer Zeitpunkt oder einer der zurückliegenden Wiederherstellungspunkte sein.

In diesem Tutorial stellen Sie die Daten eines zurückliegenden Wiederherstellungspunkts wieder her. Zeigen Sie die [Liste mit den Wiederherstellungspunkten](#view-restore-points-for-a-backed-up-database) für die Datenbank an, und wählen Sie den Punkt aus, für den Sie die Wiederherstellung durchführen möchten. In diesem Tutorial wird der Wiederherstellungspunkt mit dem Namen *7660777527047692711* verwendet.

Mit dem obigen Wiederherstellungspunkt-Namen und unter Nutzung des Wiederherstellungsmodus erstellen wir nun das Objekt für die Wiederherstellungskonfiguration. Hierfür verwenden wir das Cmdlet [az backup recoveryconfig show](/cli/azure/backup/recoveryconfig#az-backup-recoveryconfig-show). Die übrigen Parameter dieses Cmdlets haben die folgende Bedeutung:

* **--target-item-name**: Dies ist der Name, der von der wiederhergestellten Datenbank verwendet wird. In diesem Fall haben wir den Namen *restored_database* genutzt.
* **--target-server-name**: Dies ist der Name eines SAP HANA-Servers, der erfolgreich für einen Recovery Services-Tresor registriert wurde und sich in derselben Region wie die wiederherzustellende Datenbank befindet. In diesem Tutorial stellen wir die Datenbank auf demselben SAP HANA-Server mit dem Namen *hxehost* wieder her, den wir geschützt haben.
* **--target-server-type**: Für die Wiederherstellung von SAP HANA-Datenbanken muss **HANAInstance** verwendet werden.

```azurecli-interactive

az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode AlternateWorkloadRestore \
    --rp-name 7660777527047692711 \
    --target-item-name restored_database \
    --target-server-name hxehost \
    --target-server-type HANAInstance \
    --workload-type SAPHANA \
    --output json
```

Die Antwort auf die obige Abfrage ist ein Objekt für die Wiederherstellungskonfiguration, das in etwa wie folgt aussieht:

```output
{"restore_mode": "AlternateLocation", "container_uri": " VMAppContainer;Compute;saphanaResourceGroup;saphanaVM ", "item_uri": "SAPHanaDatabase;hxe;hxe", "recovery_point_id": "7660777527047692711", "item_type": "SAPHana", "source_resource_id": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm", "database_name": null, "container_id": null, "alternate_directory_paths": null}
```

Führen Sie nun zum Wiederherstellen der Datenbank das Cmdlet [az restore restore-azurewl](/cli/azure/backup/restore#az-backup-restore-restore-azurewl) aus. Zum Verwenden dieses Befehls geben wir die obige JSON-Ausgabe ein, die in einer Datei mit dem Namen *recoveryconfig.json* gespeichert wird.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Die Ausgabe sieht dann wie folgt aus:

```output
Name                                  Resource
------------------------------------  -------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

In der Antwort ist der Auftragsname enthalten. Dieser Auftragsname kann zum Nachverfolgen des Auftragsstatus mit dem Cmdlet [az backup job show](/cli/azure/backup/job#az-backup-job-show) verwendet werden.

## <a name="restore-and-overwrite"></a>Wiederherstellen und Überschreiben

Wir verwenden **OrignialWorkloadRestore** als Wiederherstellungsmodus, um die Wiederherstellung am ursprünglichen Ort durchzuführen. Anschließend müssen Sie den Wiederherstellungspunkt auswählen. Dies kann entweder ein früherer Zeitpunkt oder einer der zurückliegenden Wiederherstellungspunkte sein.

Für dieses Tutorial wählen wir den zurückliegenden Zeitpunkt „28-11-2019-09:53:00“ für die Wiederherstellung aus. Sie können diesen Wiederherstellungspunkt in den folgenden Formaten angeben: „tt-mm-jjjj“ und „tt-mm-jjjj-hh:mm:ss“. Verwenden Sie das Cmdlet [az backup recoverypoint show-log-chain](/cli/azure/backup/recoverypoint#az-backup-recoverypoint-show-log-chain), um einen gültigen Zeitpunkt für die Wiederherstellung auszuwählen. Hiermit werden die Intervalle mit den ununterbrochenen Protokollkettensicherungen aufgelistet.

```azurecli-interactive
az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode OriginalWorkloadRestore \
    --log-point-in-time 28-11-2019-09:53:00 \
    --output json
```

Die Antwort auf die obige Abfrage ist ein Objekt für die Wiederherstellungskonfiguration, das wie folgt aussieht:

```output
{"restore_mode": "OriginalLocation", "container_uri": " VMAppContainer;Compute;saphanaResourceGroup;saphanaVM ", "item_uri": "SAPHanaDatabase;hxe;hxe", "recovery_point_id": "DefaultRangeRecoveryPoint", "log_point_in_time": "28-11-2019-09:53:00", "item_type": "SAPHana", "source_resource_id": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm", "database_name": null, "container_id": null, "alternate_directory_paths": null}"
```

Führen Sie nun zum Wiederherstellen der Datenbank das Cmdlet [az restore restore-azurewl](/cli/azure/backup/restore#az-backup-restore-restore-azurewl) aus. Zum Verwenden dieses Befehls geben wir die obige JSON-Ausgabe ein, die in einer Datei mit dem Namen *recoveryconfig.json* gespeichert wird.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Die Ausgabe sieht dann wie folgt aus:

```output
Name                                  Resource
------------------------------------  --------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

In der Antwort ist der Auftragsname enthalten. Dieser Auftragsname kann zum Nachverfolgen des Auftragsstatus mit dem Cmdlet [az backup job show](/cli/azure/backup/job#az-backup-job-show) verwendet werden.

## <a name="restore-as-files"></a>Wiederherstellen als Dateien

Um die Sicherungsdaten als Dateien anstatt einer Datenbank wiederherzustellen, verwenden wir **RestoreAsFiles** als Wiederherstellungsmodus. Wählen Sie dann den Wiederherstellungspunkt aus. Dies kann entweder ein früherer Zeitpunkt oder einer der zurückliegenden Wiederherstellungspunkte sein. Nachdem die Dateien in einem angegebenen Pfad gesichert wurden, können Sie diese Dateien auf jeden SAP HANA-Computer verschieben, auf dem sie als Datenbank wiederhergestellt werden sollen. Da Sie diese Dateien auf einen beliebigen Computer verschieben können, können Sie nun die Daten über Abonnements und Regionen hinweg wiederherstellen.

In diesem Tutorial wählen wir den vorherigen Zeitpunkt `28-11-2019-09:53:00` für die Wiederherstellung und den Speicherort für Sicherungsdateikopien als `/home/saphana/restoreasfiles` auf demselben SAP HANA-Server aus. Sie können diesen Wiederherstellungspunkt in einem der folgenden Formate angeben: **tt-mm-jjjj** oder **tt-mm-jjjj-hh:mm:ss**. Verwenden Sie das Cmdlet [az backup recoverypoint show-log-chain](/cli/azure/backup/recoverypoint#az-backup-recoverypoint-show-log-chain), um einen gültigen Zeitpunkt für die Wiederherstellung auszuwählen. Hiermit werden die Intervalle mit den ununterbrochenen Protokollkettensicherungen aufgelistet.

Mit dem oben aufgeführten Wiederherstellungspunktnamen und unter Nutzung des Wiederherstellungsmodus erstellen wir nun das Objekt für die Wiederherstellungskonfiguration. Hierfür verwenden wir das Cmdlet [az backup recoveryconfig show](/cli/azure/backup/recoveryconfig#az-backup-recoveryconfig-show). Die übrigen Parameter dieses Cmdlets haben die folgende Bedeutung:

* **--target-container-name**: Dies ist der Name eines SAP HANA-Servers, der erfolgreich für einen Recovery Services-Tresor registriert wurde und sich in derselben Region wie die wiederherzustellende Datenbank befindet. In diesem Tutorial stellen wir die Datenbank als Datenen auf demselben SAP HANA-Server mit dem Namen *hxehost* wieder her, den wir geschützt haben.
* **--rp-name**: Für eine Point-in-Time-Wiederherstellung lautet der Wiederherstellungspunktname **DefaultRangeRecoveryPoint**.

```azurecli-interactive
az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode RestoreAsFiles \
    --log-point-in-time 28-11-2019-09:53:00 \
    --rp-name DefaultRangeRecoveryPoint \
    --target-container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --filepath /home/saphana/restoreasfiles \
    --output json
```

Die Antwort auf die Abfrage oben ist ein Objekt für die Wiederherstellungskonfiguration, das wie folgt aussieht:

```output
{
  "alternate_directory_paths": null,
  "container_id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.RecoveryServices/vaults/SAPHANAVault/backupFabrics/Azure/protectionContainers/VMAppContainer;Compute;SAPHANA;hanamachine",
  "container_uri": "VMAppContainer;compute;saphana;hanamachine",
  "database_name": null,
  "filepath": "/home/",
  "item_type": "SAPHana",
  "item_uri": "SAPHanaDatabase;hxe;hxe",
  "log_point_in_time": "04-07-2020-09:53:00",
  "recovery_mode": "FileRecovery",
  "recovery_point_id": "DefaultRangeRecoveryPoint",
  "restore_mode": "AlternateLocation",
  "source_resource_id": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/hanamachine"
}
```

Führen Sie nun zum Wiederherstellen der Datenbank als Dateien das Cmdlet [az restore restore-azurewl](/cli/azure/backup/restore#az-backup-restore-restore-azurewl) aus. Zum Verwenden dieses Befehls geben wir die oben aufgeführte JSON-Ausgabe ein, die in einer Datei mit dem Namen *recoveryconfig.json* gespeichert wird.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output json
```

Die Ausgabe sieht dann wie folgt aus:

```output
{
  "eTag": null,
  "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/SAPHANARESOURCEGROUP/providers/Microsoft.RecoveryServices/vaults/SAPHANAVault/backupJobs/608e737e-c001-47ca-8c37-57d909c8a704",
  "location": null,
  "name": "608e737e-c001-47ca-8c37-57d909c8a704",
  "properties": {
    "actionsInfo": [
      "Cancellable"
    ],
    "activityId": "7ddd3c3a-c0eb-11ea-a5f8-54ee75ec272a",
    "backupManagementType": "AzureWorkload",
    "duration": "0:00:01.781847",
    "endTime": null,
    "entityFriendlyName": "HXE [hxehost]",
    "errorDetails": null,
    "extendedInfo": {
      "dynamicErrorMessage": null,
      "propertyBag": {
        "Job Type": "Restore as files"
      },
      "tasksList": [
        {
          "status": "InProgress",
          "taskId": "Transfer data from vault"
        }
      ]
    },
    "jobType": "AzureWorkloadJob",
    "operation": "Restore",
    "startTime": "2020-07-08T07:20:29.336434+00:00",
    "status": "InProgress",
    "workloadType": "SAPHanaDatabase"
  },
  "resourceGroup": "saphanaResourceGroup",
  "tags": null,
  "type": "Microsoft.RecoveryServices/vaults/backupJobs"
}
```

In der Antwort ist der Auftragsname enthalten. Dieser Auftragsname kann zum Nachverfolgen des Auftragsstatus mit dem Cmdlet [az backup job show](/cli/azure/backup/job#az-backup-job-show) verwendet werden.

Die folgenden Dateien werden im Zielcontainer gesichert:

* Datenbanksicherungsdateien
* Katalogdateien
* JSON-Metadatendateien (für jede betroffene Sicherungsdatei)

Normalerweise kann über einen Netzwerkfreigabepfad oder den Pfad einer eingebundenen Azure-Dateifreigabe, wenn dieser als Zielpfad angegeben ist, über andere Computer im selben Netzwerk oder mit derselben eingebundenen Azure-Dateifreigabe einfacher auf diese Dateien zugegriffen werden.

>[!NOTE]
>Um die Datenbank-Sicherungsdateien auf einer Azure-Dateifreigabe wiederherzustellen, die auf der registrierten Ziel-VM bereitgestellt ist, stellen Sie sicher, dass das root-Konto über Lese-/Schreibberechtigungen für die Azure-Dateifreigabe verfügt.

Basierend auf dem ausgewählten Typ von Wiederherstellungspunkt (**Point-in-Time** oder **Vollständig und differenziell**) sehen Sie einen oder mehrere im Zielpfad erstellte Ordner. Einer der Ordner mit dem Namen `Data_<date and time of restore>` enthält die vollständigen und differenziellen Sicherungen, während der andere Ordner mit dem Namen `Log` die Protokollsicherungen enthält.

Verschieben Sie diese wiederhergestellten Dateien auf den SAP HANA-Server, auf dem Sie sie als-Datenbank wiederherstellen möchten. Führen Sie dann die folgenden Schritte zum Wiederherstellen der Datenbank aus:

1. Legen Sie Berechtigungen für den Ordner/das Verzeichnis fest, indem die Sicherungsdateien gespeichert sind. Führen Sie dazu folgenden Befehl aus:

    ```bash
    chown -R <SID>adm:sapsys <directory>
    ```

1. Führen Sie den nächsten Satz als `<SID>adm` aus.

    ```bash
    su - <sid>adm
    ```

1. Generieren Sie die Katalogdatei für die Wiederherstellung. Extrahieren Sie die **BackupId** aus der JSON-Metadatendatei für die vollständige Sicherung, die später im Wiederherstellungsvorgang verwendet wird. Stellen Sie sicher, dass sich die vollständigen Sicherungen und die Protokollsicherungen in unterschiedlichen Ordnern befinden, und löschen Sie die Katalogdateien und die JSON-Metadatendateien in diesen Ordnern.

    ```bash
    hdbbackupdiag --generate --dataDir <DataFileDir> --logDirs <LogFilesDir> -d <PathToPlaceCatalogFile>
    ```

    Im obigen Befehl:

    * `<DataFileDir>` ist der Ordner mit den vollständigen Sicherungen.
    * `<LogFilesDir>` ist der Ordner mit den Protokollsicherungen.
    * `<PathToPlaceCatalogFile>` ist der Ordner, in dem die generierte Katalogdatei platziert werden muss.

1. Führen Sie die Wiederherstellung mit der neu generierten Katalogdatei über HANA Studio aus, oder führen Sie die HDBSQL-Wiederherstellungsabfrage mit diesem neu generierten Katalog aus. HDBSQL-Abfragen sind unten aufgelistet:

    * Wiederherstellen eines bestimmten Zeitpunkts:

        Wenn Sie eine neue wiederhergestellte Datenbank erstellen, führen Sie den HDBSQL-Befehl aus, um eine neue Datenbank `<DatabaseName>` zu erstellen, und beenden Sie die Datenbank anschließend für die Wiederherstellung. Wenn Sie jedoch nur eine vorhandene Datenbank wiederherstellen, führen Sie den HDBSQL-Befehl aus, um die Datenbank zu beenden.

        Führen Sie dann den folgenden Befehl aus, um die Datenbank wiederherzustellen:

        ```hdbsql
        RECOVER DATABASE FOR <DatabaseName> UNTIL TIMESTAMP '<TimeStamp>' CLEAR LOG USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING LOG PATH (' <LogFileDir>') USING DATA PATH ('<DataFileDir>') USING BACKUP_ID <BackupIdFromJsonFile> CHECK ACCESS USING FILE
        ```

        * `<DatabaseName>` ist der Name der neuen oder vorhandenen Datenbank, die Sie wiederherstellen möchten.
        * `<Timestamp>` ist der genaue Zeitstempel der Point-in-Time-Wiederherstellung.
        * `<DatabaseName@HostName>` ist der Name der Datenbank, deren Sicherung für die Wiederherstellung verwendet wird, und der Name des **Hosts**/SAP HANA-Servers, auf dem sich diese Datenbank befindet. Die Option `USING SOURCE <DatabaseName@HostName>` gibt an, dass sich die Datensicherung (die für die Wiederherstellung verwendet wird) auf eine Datenbank mit einer anderen SID oder einem anderen Namen als der Ziel-SAP HANA-Computer bezieht. Daher muss sie nicht für Wiederherstellungen angegeben werden, die auf dem HANA-Server ausgeführt werden, auf dem die Sicherung aufgezeichnet wurde.
        * `<PathToGeneratedCatalogInStep3>` ist der Pfad zur Katalogdatei, die in **Schritt 3** generiert wurde.
        * `<DataFileDir>` ist der Ordner mit den vollständigen Sicherungen.
        * `<LogFilesDir>` ist der Ordner mit den Protokollsicherungen.
        * `<BackupIdFromJsonFile>` ist die **BackupId**, die in **Schritt 3** extrahiert wurde.

    * Wiederherstellen einer bestimmten vollständigen oder differenziellen Sicherung:

        Wenn Sie eine neue wiederhergestellte Datenbank erstellen, führen Sie den HDBSQL-Befehl aus, um eine neue Datenbank `<DatabaseName>` zu erstellen, und beenden Sie die Datenbank anschließend für die Wiederherstellung. Wenn Sie jedoch nur eine vorhandene Datenbank wiederherstellen, führen Sie den HDBSQL-Befehl aus, um die Datenbank zu beenden:

        ```hdbsql
        RECOVER DATA FOR <DatabaseName> USING BACKUP_ID <BackupIdFromJsonFile> USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING DATA PATH ('<DataFileDir>')  CLEAR LOG
        ```

        * `<DatabaseName>` ist der Name der neuen oder vorhandenen Datenbank, die Sie wiederherstellen möchten.
        * `<Timestamp>` ist der genaue Zeitstempel der Point-in-Time-Wiederherstellung.
        * `<DatabaseName@HostName>` ist der Name der Datenbank, deren Sicherung für die Wiederherstellung verwendet wird, und der Name des **Hosts**/SAP HANA-Servers, auf dem sich diese Datenbank befindet. Die Option `USING SOURCE <DatabaseName@HostName>` gibt an, dass sich die Datensicherung (die für die Wiederherstellung verwendet wird) auf eine Datenbank mit einer anderen SID oder einem anderen Namen als der Ziel-SAP HANA-Computer bezieht. Daher muss sie nicht für Wiederherstellungen angegeben werden, die auf dem HANA-Server ausgeführt werden, auf dem die Sicherung aufgezeichnet wurde.
        * `<PathToGeneratedCatalogInStep3>` ist der Pfad zur Katalogdatei, die in **Schritt 3** generiert wurde.
        * `<DataFileDir>` ist der Ordner mit den vollständigen Sicherungen.
        * `<LogFilesDir>` ist der Ordner mit den Protokollsicherungen.
        * `<BackupIdFromJsonFile>` ist die **BackupId**, die in **Schritt 3** extrahiert wurde.

## <a name="next-steps"></a>Nächste Schritte

* Fahren Sie mit dem Tutorial [Sichern von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer über die Azure CLI](tutorial-sap-hana-backup-cli.md) fort, um sich darüber zu informieren, wie Sie per Azure CLI gesicherte SAP HANA-Datenbanken verwalten.

* Informationen zum Wiederherstellen einer auf einer Azure-VM ausgeführten SAP HANA-Datenbank über das Azure-Portal finden Sie unter [Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern](./sap-hana-db-restore.md).
