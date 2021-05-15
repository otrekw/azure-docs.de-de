---
title: Betriebssystemsicherung und -wiederherstellung für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Durchführen von Betriebssystemsicherungen und -wiederherstellungen für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: Ajayan1008
manager: juergent
editor: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c624f2cdee61d138722632869901e2c59110b1a9
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109735116"
---
# <a name="os-backup-and-restore"></a>Betriebssystemsicherung und -wiederherstellung

In diesem Dokument werden die Schritte zum Durchführen einer Betriebssystemsicherung und -wiederherstellung auf Dateiebene beschrieben. Das Verfahren unterscheidet sich in Abhängigkeit von bestimmten Parametern wie Typ I oder Typ II, Revision 3 oder höher, Speicherort usw. Wenden Sie sich an Microsoft Operations, um genaue Werte für diese Parameter für Ihre Ressourcen zu erhalten.

## <a name="os-backup-and-restore-for-type-ii-skus-of-revision-3-stamps"></a>Sichern und Wiederherstellen des Betriebssystems für Typ-II-SKUs von Revision 3-Stamps

Die unten stehenden Informationen beschreiben die Schritte zum Durchführen einer Betriebssystemsicherung und -wiederherstellung auf Dateiebene für die **Typ-II-SKUs** von SAP HANA (große Instanzen) von Revision 3.

>[!Important]
> **Dieser Artikel gilt nicht für Typ-II-SKU-Bereitstellungen in Stamps von SAP HANA (große Instanzen) von Revision 4.** Start-LUNS von Einheiten von SAP HANA (große Instanzen) des Typs II, die in Stamps von Revision 4 von SAP HANA (große Instanzen) eingesetzt werden, können mit Speichermomentaufnahmen gesichert werden, wie dies bei Typ-I-SKUs bereits in Revision 3-Stamps der Fall ist.


>[!NOTE]
>In den Skripts zum Sichern des Betriebssystems wird die auf dem Server vorinstallierte ReaR-Software verwendet.  

Nach Abschluss der Bereitstellung durch das Microsoft `Service Management`-Team ist der Server standardmäßig mit zwei geplanten Sicherungen zum Sichern des Betriebssystems auf Dateisystemebene konfiguriert. Die Zeitpläne der Sicherungen können mithilfe des folgenden Befehls geprüft werden:
```
#crontab –l
```
Sie können den Sicherungszeitplan jederzeit mithilfe des folgenden Befehls ändern:
```
#crontab -e
```
### <a name="how-to-take-a-manual-backup"></a>Erstellen einer manuellen Sicherung

Die Sicherung des Betriebssystem-Dateisystems wird bereits mithilfe eines **cron-Auftrags** geplant. Sie können die Sicherung des Betriebssystems auf Dateisystemebene jedoch auch manuell ausführen. Führen Sie für eine manuelle Sicherung den folgenden Befehl aus:

```
#rear -v mkbackup
```
Auf dem folgenden Bildschirm ist eine beispielhafte manuelle Sicherung dargestellt:

![Vorgehensweise](media/HowToHLI/OSBackupTypeIISKUs/HowtoTakeManualBackup.PNG)


### <a name="how-to-restore-a-backup"></a>Wiederherstellen einer Sicherung

Sie können eine vollständige Sicherung oder eine einzelne Datei aus der Sicherung wiederherstellen. Führen Sie zum Wiederherstellen den folgenden Befehl aus:

```
#tar  -xvf  <backup file>  [Optional <file to restore>]
```
Nach der Wiederherstellung wird die Datei im aktuellen Arbeitsverzeichnis wiederhergestellt.

Der folgende Befehl zeigt die Wiederherstellung der Datei */etc/fstab* aus der Sicherungsdatei *backup.tar.gz*.
```
#tar  -xvf  /osbackups/hostname/backup.tar.gz  etc/fstab 
```
>[!NOTE] 
>Sie müssen die Datei an den gewünschten Speicherort kopieren, nachdem sie aus der Sicherung wiederhergestellt wurde.

Der folgende Screenshot zeigt die Wiederherstellung einer vollständigen Sicherung:

![Screenshot mit einem Eingabeaufforderungsfenster, das die Wiederherstellung zeigt](media/HowToHLI/OSBackupTypeIISKUs/HowtoRestoreaBackup.PNG)

### <a name="how-to-install-the-rear-tool-and-change-the-configuration"></a>Installieren des ReaR-Tools und Ändern der Konfiguration 

Die Relax-and-Recover-Pakete (ReaR) sind in den **Typ-II-SKUs** großer HANA-Instanzen **vorinstalliert**, und es ist keine Aktion erforderlich. Sie können direkt mit der Verwendung von ReaR zum Sichern des Betriebssystems beginnen.
In Situationen, in denen Sie die Pakete selbst installieren müssen, können Sie das ReaR-Tool allerdings auch über die aufgeführten Schritte installieren und konfigurieren.

Verwenden Sie die folgenden Befehle, um die **ReaR**-Sicherungspakete zu installieren:

Verwenden Sie für **SLES**-Betriebssysteme den folgenden Befehl:
```
#zypper install <rear rpm package>
```
Verwenden Sie für **RHEL**-Betriebssysteme den folgenden Befehl: 
```
#yum install rear -y
```
Um das ReaR-Tool zu konfigurieren, müssen Sie die Parameter **OUTPUT_URL** und **BACKUP_URL** in der Datei */etc/rear/local.conf* aktualisieren.
```
OUTPUT=ISO
ISO_MKISOFS_BIN=/usr/bin/ebiso
BACKUP=NETFS
OUTPUT_URL="nfs://nfsip/nfspath/"
BACKUP_URL="nfs://nfsip/nfspath/"
BACKUP_OPTIONS="nfsvers=4,nolock"
NETFS_KEEP_OLD_BACKUP_COPY=
EXCLUDE_VG=( vgHANA-data-HC2 vgHANA-data-HC3 vgHANA-log-HC2 vgHANA-log-HC3 vgHANA-shared-HC2 vgHANA-shared-HC3 )
BACKUP_PROG_EXCLUDE=("${BACKUP_PROG_EXCLUDE[@]}" '/media' '/var/tmp/*' '/var/crash' '/hana' '/usr/sap'  ‘/proc’)
```

Der folgende Screenshot zeigt die Wiederherstellung einer vollständigen Sicherung: ![Screenshot mit dem Eingabeaufforderungsfenster, das die Wiederherstellung mithilfe des ReaR-Tools zeigt](media/HowToHLI/OSBackupTypeIISKUs/RearToolConfiguration.PNG)


## <a name="os-backup-and-restore-for-all-other-skus"></a>Betriebssystemsicherung und -wiederherstellung für alle anderen SKUs

Die unten stehenden Informationen beschreiben die Schritte zum Durchführen einer Betriebssystemsicherung und -wiederherstellung auf Dateiebene für alle SKUs aller Revisionen mit Ausnahme von **Typ-II-SKUs** von SAP HANA (große Instanzen) von Revision 3.

### <a name="how-to-take-a-manual-backup"></a>Erstellen einer manuellen Sicherung

Beziehen Sie die neuesten Microsoft Snapshot Tools für SAP HANA von [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md), und konfigurieren Sie sie für die regelmäßige Ausführung über `crontab` mit dem Flag `--type=boot`. Dadurch werden regelmäßige Betriebssystemsicherungen sichergestellt. Das folgende Beispiel zeigt einen Cron-Zeitplan in `/etc/crontab` für eine Betriebssystemsicherung für Typ-I-SKUs:

```
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```

Das folgende Beispiel zeigt einen Cron-Zeitplan in `/etc/crontab` für eine Betriebssystemsicherung für Typ-II-SKUs:

```
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

Zusätzliche Referenzen:
- [Einrichten von Speichermomentaufnahmen](hana-backup-restore.md#set-up-storage-snapshots)
- Handbuch „Microsoft Snapshot Tools for SAP HANA on Azure“ auf [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md)

### <a name="how-to-restore-a-backup"></a>Wiederherstellen einer Sicherung

Der Wiederherstellungsvorgang kann nicht vom Betriebssystem selbst ausgeführt werden. Richten Sie hierfür ein Supportticket bei Microsoft Operations ein. Für den Wiederherstellungsvorgang muss sich die HLI-Instanz im ausgeschalteten Zustand befinden. Planen Sie dies bitte entsprechend.

### <a name="managed-os-snapshots"></a>Verwaltete Momentaufnahmen des Betriebssystems

Azure kann automatisch Betriebssystemsicherungen für Ihre HLI-Ressourcen erstellen. Diese Sicherungen werden einmal täglich erstellt, und Azure behält jeweils die letzten drei dieser Sicherungen bei. Dies ist standardmäßig für alle Kunden in den folgenden Regionen aktiviert:
- USA (Westen)
- Australien (Osten)
- Australien, Südosten
- USA Süd Mitte
- USA (Ost) 2

Diese Einrichtung ist in den folgenden Regionen teilweise verfügbar:
- East US
- Nordeuropa
- Europa, Westen

Die Häufigkeit oder Aufbewahrungsdauer der von dieser Einrichtung durchgeführten Sicherungen kann nicht geändert werden. Falls eine andere Strategie für Betriebssystemsicherungen für Ihre HLI-Ressourcen erforderlich ist, können Sie diese Einrichtung deaktivieren, indem Sie ein Supportticket bei Microsoft Operations erstellen und dann Microsoft Snapshot Tools für SAP HANA konfigurieren, um Betriebssystemsicherungen gemäß den Anweisungen im vorherigen Abschnitt dieses Dokuments vorzunehmen.