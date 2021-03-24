---
title: Löschen mithilfe des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Enthält eine Anleitung zum Ausführen des Löschbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 12/14/2020
ms.author: phjensen
ms.openlocfilehash: 1f2c767d45bb08e25a057c7db1f380ceb250f607
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104864906"
---
# <a name="delete-using-azure-application-consistent-snapshot-tool-preview"></a>Löschen mithilfe des Tools für konsistente Momentaufnahmen in Azure-Anwendungen (Vorschau)

Dieser Artikel enthält eine Anleitung zum Ausführen des Löschbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.

## <a name="introduction"></a>Einführung

Der Befehl `azacsnap -c delete` ermöglicht das Löschen von Volumemomentaufnahmen und Datenbankkatalog-Einträgen.

> [!IMPORTANT]
> Momentaufnahmen, die weniger als zehn Minuten vor dem Ausführen dieses Befehls erstellt wurden, sollten nicht gelöscht werden, da sonst unter Umständen die Momentaufnahmereplikation beeinträchtigt wird.

## <a name="command-options"></a>Befehlsoptionen

Der Befehl `-c delete` verfügt über folgende Optionen:

- `--delete hana`: Bei Verwendung mit den Optionen `--dbsid <SID>` und `--hanabackupid <HANA backup id>` werden Einträge aus dem SAP HANA-Sicherungskatalog gelöscht, der den Kriterien entsprechen.

- `--delete storage`: Bei Verwendung mit der Option `--snapshot <snapshot name>` wird die Momentaufnahme aus dem Back-End-Speichersystem löschen.

- `--delete sync`: Bei Verwendung mit den Optionen `--dbsid <SID>` und `--hanabackupid <HANA backup id>` wird der Name der Speichermomentaufnahme aus dem Sicherungskatalog für die HANA-Sicherungs-ID (`<HANA backup id>`) abgerufen. Anschließend werden der Eintrag im Sicherungskatalog _und_ die Momentaufnahme aus allen Volumes mit der genannten Momentaufnahme gelöscht.

- `--delete sync`: Bei Verwendung mit `--snapshot <snapshot name>` wird im Sicherungskatalog nach Einträgen für den Namen der Momentaufnahme (`<snapshot name>`) gesucht und die SAP HANA-Sicherungs-ID abgerufen. Anschließend werden der Eintrag im Sicherungskatalog _und_ die Momentaufnahme aus allen Volumes mit der genannten Momentaufnahme gelöscht.

- `[--force]` (optional). *Verwenden Sie diese Option mit Bedacht.*  Durch diesen Vorgang wird das Löschen erzwungen, ohne eine Bestätigung anzufordern.

- `[--configfile <config filename>]` ist ein optionaler Parameter, der die Verwendung benutzerdefinierter Konfigurationsdateinamen ermöglicht.

### <a name="delete-a-snapshot-using-sync-option"></a>Löschen einer Momentaufnahme mit der Option `sync`

```bash
azacsnap -c delete --delete sync --dbsid H80 --hanabackupid 157979797979
```

> [!NOTE]
> Sucht im Sicherungskatalog nach Einträgen für die SAP HANA-Sicherungs-ID 157979797979 und ruft den Namen der Speichermomentaufnahme ab. Anschließend werden sowohl der Eintrag im Sicherungskatalog als auch die Momentaufnahme aus allen Volumes mit der genannten Momentaufnahme gelöscht.

```bash
azacsnap -c delete --delete sync --snapshot hana_hourly.2020-01-22_2358
```

> [!NOTE]
> Sucht im Sicherungskatalog nach Einträgen für Momentaufnahme „hana_hourly.2020-01-22_2358“ und ruft die SAP HANA-Sicherungs-ID ab. Anschließend werden sowohl der Eintrag im Sicherungskatalog als auch die Momentaufnahme aus allen Volumes mit der genannten Momentaufnahme gelöscht.

### <a name="delete-a-snapshot-using-hana-option"></a>Löschen einer Momentaufnahme mit der Option `hana`

```bash
azacsnap -c delete --delete hana --dbsid H80 --hanabackupid 157979797979
```

> [!NOTE]
> Löscht die SAP HANA-Sicherungs-ID 157979797979 aus dem Sicherungskatalog für die SID H80.

### <a name="delete-a-snapshot-using-storage-option"></a>Löschen einer Momentaufnahme mit der Option `storage`

```bash
azacsnap -c delete --delete storage --snapshot hana_hourly.2020-01-22_2358
```

> [!NOTE]
> Löscht die Momentaufnahme aus allen Volumes, die die Momentaufnahme „hana_hourly.2020-01-22_2358“ enthalten.

**Ausgabe mit der Option `--delete storage`**

Der Benutzer wird aufgefordert, den Löschvorgang zu bestätigen.

```bash
azacsnap -c delete --delete storage --snapshot azacsnap-hsr-ha.2020-07-02T221702.2535255Z
```

```output
Processing delete request for snapshot 'azacsnap-hsr-ha.2020-07-02T221702.2535255Z'.
Are you sure you want to permanently delete the snapshot 'azacsnap-hsr-ha.2020-07-02T221702.2535255Z' from all storage volumes.? (y/n) [n]: y
Snapshot deletion completed
```

Die Benutzerbestätigung kann mithilfe des optionalen Parameters `--force` vermieden werden:

```bash
azacsnap -c delete --delete storage --snapshot azacsnap-hsr-ha.2020-07-02T222201.4988618Z --force
```

```output
Processing delete request for snapshot 'azacsnap-hsr-ha.2020-07-02T222201.4988618Z'.
Snapshot deletion completed
```

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen von Momentaufnahmedetails](azacsnap-cmd-ref-details.md)
- [Erstellen einer Sicherung](azacsnap-cmd-ref-backup.md)
