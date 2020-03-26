---
title: 'Tutorial: Wiederherstellen einer SAP HANA-Datenbank unter Azure per CLI'
description: In diesem Tutorial wird beschrieben, wie Sie SAP HANA-Datenbanken, die auf einem virtuellen Azure-Computer ausgeführt werden, über die Azure CLI aus einem Recovery Services-Tresor von Azure Backup wiederherstellen.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 6dbe0c4382b648506d853feb281c70a8e8401595
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "75470405"
---
# <a name="tutorial-restore-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Tutorial: Wiederherstellen von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer über die Azure CLI

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Dokumentation wird beschrieben, wie Sie eine gesicherte SAP HANA-Datenbank auf einem virtuellen Azure-Computer über die Azure CLI wiederherstellen. Sie können [diese Schritte auch über das Azure-Portal ausführen](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).

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

Verwenden Sie das Cmdlet[az backup recoverypoint list](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) wie folgt, um die Liste mit allen Wiederherstellungspunkten anzuzeigen:

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
>Sie können auch die Start- und Endpunkte für alle ununterbrochenen Protokollsicherungsketten anzeigen, indem Sie das Cmdlet [az backup recoverypoint show-log-chain](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) verwenden.

## <a name="prerequisites-to-restore-a-database"></a>Voraussetzungen für die Wiederherstellung einer Datenbank

Stellen Sie vor dem Wiederherstellen einer Datenbank sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Sie können die Datenbank nur in einer SAP HANA-Instanz wiederherstellen, die sich in derselben Region befindet.
* Die Zielinstanz muss beim gleichen Tresor wie die Quelle registriert sein.
* Mit Azure Backup ist es nicht möglich, zwei verschiedene SAP HANA-Instanzen auf derselben VM zu identifizieren. Daher ist das Wiederherstellen von Daten einer Instanz auf einer anderen auf demselben virtuellen Computer nicht möglich.

## <a name="restore-a-database"></a>Wiederherstellen einer Datenbank

Azure Backup kann auf virtuellen Azure-Computern ausgeführte SAP HANA-Datenbanken wie folgt wiederherstellen:

* Wiederherstellung eines bestimmten Datums oder einer bestimmten Uhrzeit (sekundengenau) mithilfe von Protokollsicherungen. Azure Backup ermittelt automatisch die geeigneten vollständige differenziellen Sicherungen und die Kette von Protokollsicherungen, die für die Wiederherstellung Ihrer Daten basierend auf dem ausgewählten Zeitpunkt benötigt werden.
* Wiederherstellung einer bestimmten vollständigen oder differenziellen Sicherung, um die Daten eines bestimmten Wiederherstellungspunkts wiederherzustellen.

Verwenden Sie zum Wiederherstellen einer Datenbank das Cmdlet [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl). Hierfür ist ein Objekt für die Wiederherstellungskonfiguration als eines der Eingabeelemente erforderlich. Sie können dieses Objekt generieren, indem Sie das Cmdlet [az backup recoveryconfig show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show) verwenden. Das Objekt für die Wiederherstellungskonfiguration enthält alle Details zur Durchführung einer Wiederherstellung. Hierbei ist ein Punkt der Wiederherstellungsmodus: **OriginalWorkloadRestore** oder **AlternateWorkloadRestore**.

>[!NOTE]
> **OriginalWorkloadRestore**: Die Daten werden auf derselben SAP HANA-Instanz wiederhergestellt, auf der sich auch die ursprüngliche Quelle befunden hat. Bei dieser Option wird die ursprüngliche Datenbank überschrieben. <br>
> **AlternateWorkloadRestore**: Die Datenbank wird an einem alternativen Speicherort wiederhergestellt, und die ursprüngliche Quelldatenbank bleibt erhalten.

## <a name="restore-to-alternate-location"></a>Wiederherstellen an einem alternativen Speicherort

Verwenden Sie **AlternateWorkloadRestore** als Wiederherstellungsmodus, wenn Sie eine Datenbank an einem anderen Speicherort wiederherstellen möchten. Anschließend müssen Sie den Wiederherstellungspunkt auswählen. Dies kann entweder ein früherer Zeitpunkt oder einer der zurückliegenden Wiederherstellungspunkte sein.

In diesem Tutorial stellen Sie die Daten eines zurückliegenden Wiederherstellungspunkts wieder her. Zeigen Sie die [Liste mit den Wiederherstellungspunkten](#view-restore-points-for-a-backed-up-database) für die Datenbank an, und wählen Sie den Punkt aus, für den Sie die Wiederherstellung durchführen möchten. In diesem Tutorial wird der Wiederherstellungspunkt mit dem Namen *7660777527047692711* verwendet.

Mit dem obigen Wiederherstellungspunkt-Namen und unter Nutzung des Wiederherstellungsmodus erstellen wir nun das Objekt für die Wiederherstellungskonfiguration. Hierfür verwenden wir das Cmdlet [az backup recoveryconfig show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show). Die übrigen Parameter dieses Cmdlets haben die folgende Bedeutung:

* **--target-item-name**: Dies ist der Name, der von der wiederhergestellten Datenbank verwendet wird. In diesem Fall haben wir den Namen *restored_database* genutzt.
* **--target-server-name**: Dies ist der Name eines SAP HANA-Servers, der erfolgreich für einen Recovery Services-Tresor registriert wurde und in derselben Region wie die wiederherzustellende Datenbank angeordnet ist. In diesem Tutorial stellen wir die Datenbank auf demselben SAP HANA-Server mit dem Namen *hxehost* wieder her, den wir geschützt haben.
* **--target-server-type**: Für die Wiederherstellung von SAP HANA-Datenbanken muss **SapHanaDatabase** verwendet werden.

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
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Führen Sie nun zum Wiederherstellen der Datenbank das Cmdlet [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) aus. Zum Verwenden dieses Befehls geben wir die obige JSON-Ausgabe ein, die in einer Datei mit dem Namen *recoveryconfig.json* gespeichert wird.

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

In der Antwort ist der Auftragsname enthalten. Dieser Auftragsname kann zum Nachverfolgen des Auftragsstatus mit dem Cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) verwendet werden.

## <a name="restore-and-overwrite"></a>Wiederherstellen und Überschreiben

Wir verwenden **OrignialWorkloadRestore** als Wiederherstellungsmodus, um die Wiederherstellung am ursprünglichen Ort durchzuführen. Anschließend müssen Sie den Wiederherstellungspunkt auswählen. Dies kann entweder ein früherer Zeitpunkt oder einer der zurückliegenden Wiederherstellungspunkte sein.

Für dieses Tutorial wählen wir den zurückliegenden Zeitpunkt „28-11-2019-09:53:00“ für die Wiederherstellung aus. Sie können diesen Wiederherstellungspunkt in den folgenden Formaten angeben: „tt-mm-jjjj“ und „tt-mm-jjjj-hh:mm:ss“. Verwenden Sie das Cmdlet [az backup recoverypoint show-log-chain](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain), um einen gültigen Zeitpunkt für die Wiederherstellung auszuwählen. Hiermit werden die Intervalle mit den ununterbrochenen Protokollkettensicherungen aufgelistet.

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
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Führen Sie nun zum Wiederherstellen der Datenbank das Cmdlet [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) aus. Zum Verwenden dieses Befehls geben wir die obige JSON-Ausgabe ein, die in einer Datei mit dem Namen *recoveryconfig.json* gespeichert wird.

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

In der Antwort ist der Auftragsname enthalten. Dieser Auftragsname kann zum Nachverfolgen des Auftragsstatus mit dem Cmdlet [az backup job show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* Fahren Sie mit dem Tutorial [Sichern von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer über die Azure CLI](tutorial-sap-hana-backup-cli.md) fort, um sich darüber zu informieren, wie Sie per Azure CLI gesicherte SAP HANA-Datenbanken verwalten.

* Informationen zum Wiederherstellen einer auf einer Azure-VM ausgeführten SAP HANA-Datenbank über das Azure-Portal finden Sie unter [Wiederherstellen von SAP HANA-Datenbanken auf virtuellen Azure-Computern](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).
