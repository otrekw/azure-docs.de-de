---
title: Wiederherstellen der Nutzung des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Enthält eine Anleitung zum Ausführen des Wiederherstellungsbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
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
ms.date: 04/21/2021
ms.author: phjensen
ms.openlocfilehash: df292fc59d46d59c94f43dae0b1827c068b3e057
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107929865"
---
# <a name="restore-using-azure-application-consistent-snapshot-tool"></a>Durchführen der Wiederherstellung mit dem Tool für konsistente Momentaufnahmen in Azure-Anwendungen

Dieser Artikel enthält eine Anleitung zum Ausführen des Wiederherstellungsbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.

## <a name="introduction"></a>Einführung

Für die Wiederherstellung eines Volumes aus einer Momentaufnahme wird der Befehl `azacsnap -c restore` verwendet.

> [!IMPORTANT]
> Hierbei wird keine Datenbankwiederherstellung durchgeführt, sondern nur von einem oder mehreren Volumes. Dies ist unten unter den einzelnen Optionen beschrieben.

## <a name="command-options"></a>Befehlsoptionen

Der Befehl `-c restore` verfügt über die folgenden Optionen:

- `--restore snaptovol`: Erstellt ein neues Volume basierend auf der letzten Momentaufnahme auf dem Zielvolume. Mit diesem Befehl wird ein neues „geklontes“ Volume basierend auf dem konfigurierten Zielvolume erstellt, indem die aktuelle Volumemomentaufnahme als Grundlage für die Erstellung des neuen Volumes genutzt wird.  Mit diesem Befehl wird die Speicherreplikation vom primären auf den sekundären Standort nicht unterbrochen. Stattdessen werden Klone der letzten verfügbaren Momentaufnahme am Standort für die Notfallwiederherstellung erstellt und die empfohlenen Dateisystem-Bereitstellungspunkte der geklonten Volumes angezeigt. Dieser Befehl sollte auf dem System vom Typ „Azure (große Instanz)“ **in der Region der Notfallwiederherstellung** (also dem Zielsystem des Failovers) ausgeführt werden.

- `--restore revertvolume`: Setzt das Zielvolume auf Grundlage der letzten Momentaufnahme auf einen früheren Zustand zurück.  Verwenden Sie diesen Befehl beim Ausführen des Failovers für die Notfallwiederherstellung in die gekoppelte Region. Mit diesem Befehl wird die Speicherreplikation vom primären zum sekundären Standort **angehalten**, und die Zielvolumes für die Notfallwiederherstellung werden auf die neuesten verfügbaren Momentaufnahmen auf den Notfallwiederherstellungsvolumes zurückgesetzt. Dies erfolgt zusammen mit den empfohlenen Dateisystem-Bereitstellungspunkten für die zurückgesetzten Notfallwiederherstellungsvolumes. Dieser Befehl sollte auf dem System vom Typ „Azure (große Instanz)“ **in der Region der Notfallwiederherstellung** (also dem Zielsystem des Failovers) ausgeführt werden.
    > [!NOTE]
    > Der Unterbefehl (`--restore revertvolume`) ist nur für Azure (große Instanz) verfügbar, und nicht für Azure NetApp Files.
- `--dbsid <SAP HANA SID>`: Dies ist die SAP HANA-SID, die aus der Konfigurationsdatei für die Anwendung der Befehle für die Volumewiederherstellung ausgewählt wird.

- `[--configfile <config filename>]`: Dies ist ein optionaler Parameter, der benutzerdefinierte Konfigurationsdateinamen zulässt.

## <a name="perform-a-test-dr-failover-azacsnap--c-restore---restore-snaptovol"></a>Ausführen eines Testfailovers für die Notfallwiederherstellung: `azacsnap -c restore --restore snaptovol`

Dieser Befehl ähnelt dem „vollständigen“ Failoverbefehl für die Notfallwiederherstellung (`--restore restorevolume`). Hierbei wird aber nicht die Replikation zwischen dem primären Standort und dem Standort für die Notfallwiederherstellung unterbrochen, sondern aus den Notfallwiederherstellungsvolumes ein Klonvolume erstellt, um die Wiederherstellung der aktuellsten Momentaufnahme am Standort für die Notfallwiederherstellung zu ermöglichen. Diese geklonten Volumes können dann vom Kunden zum Testen der Notfallwiederherstellung genutzt werden, ohne ein vollständiges Failover seiner HANA-Umgebung auszuführen, bei dem die Replikationsvereinbarung zwischen dem primären Standort und dem Standort für die Notfallwiederherstellung gebrochen wird.

- Auf diese Weise können mehrere unterschiedliche Wiederherstellungspunkte getestet werden, die jeweils über einen eigenen Wiederherstellungspunkt verfügen.
- Der Klon erhält den Zeitstempel des Zeitpunkts der Befehlsausführung und umfasst die aktuellen Daten und anderen Momentaufnahmen, die bei der Ausführung verfügbar waren.

> [!IMPORTANT]
> Dieser Vorgang gilt nur für Azure (große Instanzen).
>
> - Bei der Ausführung dieses Befehls wird die E-Mail-Adresse des Kontakts benötigt, um vor dem Löschen der Klone nach einem Zeitraum von vier Wochen die Kommunikation mit Microsoft Operations zu ermöglichen.
> - Bei jeder Ausführung des Befehls wird ein neuer Klon erstellt, der nach Abschluss des Tests von Microsoft Operations gelöscht werden muss.
> - Alle erstellten Klonvolumes werden nach vier Wochen automatisch gelöscht.

Die Konfigurationsdatei (z. B. `DR.json`) sollte nur die Notfallwiederherstellungsvolumes und nicht die Produktionsvolumes enthalten, da andernfalls ggf. Klone der Produktionsvolumes erstellt werden.

### <a name="output-of-the-azacsnap--c-restore---restore-snaptovol-command-for-single-node-scenario"></a>Ausgabe des Befehls `azacsnap -c restore --restore snaptovol` (für das Szenario mit nur einem Knoten)

```output
> azacsnap --configfile DR.json -c restore --restore snaptovol --dbsid H80
* This program is designed for those customers who have previously installed the
  Production HANA instance in the Disaster Recovery Location either as a
  stand-alone instance or as part of a multi-purpose environment.
* This program should be executed from the Disaster Recovery location otherwise
  unintended consequences may result.
* This program is intended to allow the customer to simulate a Disaster Recovery
  failover without actually requiring a failover and subsequent failback.
* Any other restore points must be handled by Microsoft Operations.
* As part of the process, a clone is created of the each of the 'data' and 'other'
  volumes per the configuration file.

Do you wish to continue? (y/n) [n]: y

About to create clones of volumes based on the latest snapshot, these will be
kept for 4 weeks before being automatically deleted by Microsoft Operations.
Enter an email address to contact when deleting clones: <b>person@nowhere.com</b>
Checking state of HLI volumes for SID 'PEW'
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Configured volumes (Data and Other) are not ready to clone, will retry in 00:00:10 seconds
Displaying Mount Points by Volume as follows:
10.50.251.34:/hana_data_h80_sapprdhdb80_mnt00001_t020_xdp_rwclone_20200916_0256  /hana/data/H80/mnt00001 nfs  rw,bg,hard,timeo=600,vers=4,rsize=1048576,wsize=1048576,intr,noatime,lock 0 0
10.50.251.36:/hana_log_backups_h80_sapprdhdb80_t020_xdp_rwclone_20200916_0256  /hana/log_backups/H80/01 nfs  rw,bg,hard,timeo=600,vers=4,rsize=1048576,wsize=1048576,intr,noatime,lock 0 0
*******************  HANA Test DR Restore Steps  ******************************
* Complete the following steps to recover your HANA database:           *
* 1. Ensure ALL the target mount points exist to mount the snapshot clones.    *
*    e.g. mkdir /hana/logbackups/H99_SOURCE                                    *
* 2. Add Mount Point Details from 'Displaying Mount Points by Volume' as       *
*    output above into /etc/fstab of DR Server.                                *
* 3. Mount newly added filesystems.                                            *
* 4. Perform HANA Snapshot Recovery using HANA Studio.                         *
********************************************************************************
*  These snapshot copies (clones) are kept for 4 weeks before                  *
*  being automatically removed.                                                *
*  Please contact Microsoft Operations to delete them earlier.                 *
********************************************************************************
```

> [!IMPORTANT]
> Die Ausgabe vom Typ „Anzeige von Bereitstellungspunkten nach Volume“ unterscheidet sich für die einzelnen Szenarien.

## <a name="perform-full-dr-failover-azacsnap--c-restore---restore-revertvolume"></a>Ausführen eines vollständigen Failovers für die Notfallwiederherstellung: `azacsnap -c restore --restore revertvolume`

Mit diesem Befehl wird Folgendes durchgeführt: Speicherreplikation vom primären an den sekundären Standort, Wiederherstellung der letzten Momentaufnahme auf den Notfallwiederherstellungsvolumes und Angabe der Bereitstellungspunkte für die Notfallwiederherstellungsvolumes.

Dieser Befehl MUSS auf dem Server für die Notfallwiederherstellung ausgeführt werden, indem eine Konfigurationsdatei (z. B. `DR.json`) NUR mit Notfallwiederherstellungsvolumes genutzt wird!

Führen Sie mit dem Befehl `azacsnap -c restore --restore revertvolume` ein Failover auf den Notfallwiederherstellungsstandort aus.  Bei diesem Befehl muss eine SID als Parameter hinzugefügt werden. Dies ist die SID der HANA-Instanz, die am Standort für die Notfallwiederherstellung wiederhergestellt werden muss.

> [!IMPORTANT]
> Führen Sie diesen Befehl nur aus, wenn Sie planen, die Notfallwiederherstellungsübung oder einen Test durchzuführen. Mit diesem Befehl wird die Replikation unterbrochen. Sie müssen sich an Microsoft Operations wenden, um die Replikation wieder aktivieren zu lassen.

Hier sind die allgemeinen Schritte zum Ausführen eines Notfallwiederherstellungsfailovers angegeben:

- Sie müssen die HANA-Instanz am **primären** Standort herunterfahren. Diese Aktion ist nur erforderlich, wenn Sie das Failover zum Standort für die Notfallwiederherstellung wirklich ausführen, um Dateninkonsistenzen zu vermeiden.
- Fahren Sie die HANA-Instanz auf dem Notfallwiederherstellungsknoten für die Produktions-SID herunter.
- Führen Sie den Befehl `azacsnap -c restore --restore revertvolume` auf dem Notfallwiederherstellungsknoten mit der wiederherzustellenden SID aus.
  - Mit dem Befehl wird der Link für die Speicherreplikation vom primären Standort zum Standort für die Notfallwiederherstellung unterbrochen.
  - Mit dem Befehl werden die Volumes vom Typ „Daten“ und „Sonstiges“ so wiederhergestellt, wie sie konfiguriert wurden.  Normalerweise gilt dieser Vorgang für die Volumes für die Dateisysteme `/hana/data` und `/hana/logbackups`.  Das Dateisystem `/hana/shared` wird NICHT wiederhergestellt, sondern das vorhandene Dateisystem `/hana/shared` wird als SID für den Standort für die Notfallwiederherstellung verwendet.
  - Stellen Sie die Volumes `/hana/data` und `/hana/logbackups` bereit. Achten Sie hierbei darauf, dass sie der Datei `/etc/fstab` hinzugefügt werden.
- Stellen Sie die HANA SYSTEMDB-Momentaufnahme wieder her. In HANA Studio werden nur die aktuellen HANA-Momentaufnahmen angezeigt, die unter der Speichermomentaufnahme verfügbar sind und beim Ausführen des Momentaufnahmebefehls `azacsnap -c restore --restore revertvolume` wiederhergestellt werden.
- Stellen Sie die Mandantendatenbank wieder her.
- Starten Sie die HANA-Instanz am Notfallwiederherstellungs-Standort für die Produktions-SID (Beispiel für diesen Fall: H80).
- Führen Sie die erforderlichen Datenbanktests durch.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Sicherung](azacsnap-cmd-ref-backup.md)
- [Abrufen von Momentaufnahmendetails](azacsnap-cmd-ref-details.md)
