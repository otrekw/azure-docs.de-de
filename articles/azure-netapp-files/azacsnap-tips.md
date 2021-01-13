---
title: Tipps und Tricks zur Verwendung des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Bietet Tipps und Tricks zur Verwendung des Tools für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
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
ms.topic: how-to
ms.date: 12/14/2020
ms.author: phjensen
ms.openlocfilehash: 86d7b1a6d361ebbc0d8419d184f9a11d390a37f1
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803186"
---
# <a name="tips-and-tricks-for-using-azure-application-consistent-snapshot-tool-preview"></a>Tipps und Tricks zur Verwendung des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files (Vorschau)

Dieser Artikel enthält Tipps und Tricks, die bei der Verwendung von AzAcSnap hilfreich sein können.

## <a name="limit-service-principal-permissions"></a>Begrenzen von Dienstprinzipalberechtigungen

Möglicherweise ist es erforderlich, den Berechtigungsbereich des AzAcSnap-Dienstprinzipals einzuschränken.  Weitere Informationen zur fein abgestuften Zugriffsverwaltung von Azure-Ressourcen finden Sie in der [Azure RBAC-Dokumentation](https://docs.microsoft.com/azure/role-based-access-control/).  

Im Folgenden finden Sie eine Beispielrollendefinition mit den mindestens erforderlichen Aktionen, die für die Funktion von AzAcSnap erforderlich sind.

```bash
az role definition create --role-definition '{ \
  "Name": "Azure Application Consistent Snapshot tool", \
  "IsCustom": "true", \
  "Description": "Perform snapshots on ANF volumes.", \
  "Actions": [ \
    "Microsoft.NetApp/*/read", \
    "Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write", \
    "Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete" \
  ], \
  "NotActions": [], \
  "DataActions": [], \
  "NotDataActions": [], \
  "AssignableScopes": ["/subscriptions/<insert your subscription id>"] \
}'
```

## <a name="take-snapshots-manually"></a>Manuelles Erstellen von Momentaufnahmen

Überprüfen Sie vor dem Ausführen jeglicher Sicherungsbefehle (`azacsnap -c backup`) die Konfiguration, indem Sie die Testbefehle ausführen und überprüfen, ob Sie erfolgreich ausgeführt werden.  Die korrekte Ausführung dieser Tests hat gezeigt, dass `azacsnap` mit der installierten SAP HANA-Datenbank und dem zugrunde liegenden Speichersystem von SAP HANA in **Azure (große Instanzen)** oder dem **Azure NetApp Files**-System kommunizieren kann.

- `azacsnap -c test --test hana`
- `azacsnap -c test --test storage`

Führen Sie dann zur Erstellung einer manuellen Datenbankmomentaufnahme den folgenden Befehl aus:

```bash
azacsnap -c backup --volume data --prefix hana_TEST --retention=1
```

## <a name="setup-automatic-snapshot-backup"></a>Einrichten der automatischen Momentaufnahmensicherung

Es ist üblich, dass auf Unix-/Linux-Systemen `cron` verwendet wird, um die Ausführung von Befehlen auf einem System zu automatisieren. Das Standardverfahren für die Momentaufnahmetools besteht darin, die `crontab` des Benutzers einzurichten.

Ein Beispiel für eine `crontab` für den Benutzer `azacsnap` zum Automatisieren von Momentaufnahmen finden Sie im Folgenden.

```output
MAILTO=""
# =============== TEST snapshot schedule ===============
# Data Volume Snapshots - taken every hour.
@hourly (. /home/azacsnap/.profile ; cd /home/azacsnap/bin ; azacsnap -c backup --volume data --prefix hana_TEST --retention=9)
# Other Volume Snapshots - taken every 5 minutes, excluding the top of the hour when hana snapshots taken
5,10,15,20,25,30,35,40,45,50,55 * * * * (. /home/azacsnap/.profile ; cd /home/azacsnap/bin ; azacsnap -c backup --volume other --prefix logs_TEST --retention=9)
# Other Volume Snapshots - using an alternate config file to snapshot the boot volume daily.
@daily (. /home/azacsnap/.profile ; cd /home/azacsnap/bin ; azacsnap -c backup --volume other --prefix DailyBootVol --retention=7 --configfile boot-vol.json)
```

Erläuterung der obigen crontab.

- `MAILTO=""`: Durch die Verwendung eines leeren Werts wird verhindert, dass cron automatisch versucht, den Benutzer beim Ausführen des crontab-Eintrags per E-Mail zu kontaktieren, da die E-Mail wahrscheinlich in der E-Mail-Datei des lokalen Benutzers eingehen würde.
- Abgekürzte Versionen zur zeitlichen Steuerung von crontab-Einträgen sind selbsterklärend:
  - `@monthly` = Einmal im Monat ausführen,d. h. „0 0 1 * *“.
  - `@weekly` = Einmal in der Woche ausführen,d. h. „0 0 * * 0“.
  - `@daily` = Einmal am Tag ausführen,d. h. „0 0 * * *“.
  - `@hourly` = Einmal pro Stunde ausführen, d. h. „0 * * * *“.
- Die ersten fünf Spalten werden verwendet, um Zeiten festzulegen. Weitere Informationen hierzu finden Sie in den folgenden Spaltenbeispielen:
  - `0,15,30,45`: Alle 15 Minuten
  - `0-23`: Stündlich
  - `*`: Täglich
  - `*`: Monatlich
  - `*`: Jeden Tag der Woche
- Die auszuführende Befehlszeile ist in runde Klammern „()“ eingeschlossen.
  - `. /home/azacsnap/.profile` = Das Profil des Benutzers als Eingabe pullen, um seine Umgebung einzurichten, einschließlich „$PATH“ usw.
  - `cd /home/azacsnap/bin` = Ausführungsverzeichnis in den Speicherort „/home/azacsnap/bin“ ändern, in dem sich die Konfigurationsdateien befinden.
  - `azacsnap -c .....` = Der vollständige azacsnap-Befehl, der ausgeführt werden soll, einschließlich aller Optionen.

Weitere Erläuterungen zu cron und dem Format der Datei „crontab“ finden Sie hier: <https://en.wikipedia.org/wiki/Cron>

> [!NOTE]
> Benutzer sind verantwortlich für die Überwachung der cron-Aufträge, um sicherzustellen, dass Momentaufnahmen erfolgreich generiert werden.

## <a name="monitor-the-snapshots"></a>Überwachen der Momentaufnahmen

Die folgenden Bedingungen sollten überwacht werden, um ein fehlerfreies System zu gewährleisten:

1. Verfügbarer Datenträgerspeicherplatz. Momentaufnahmen verbrauchen langsam Speicherplatz auf dem Datenträger, da die Beibehaltung älterer Festplattenblöcke in der Momentaufnahme erhalten bleibt.
    1. Verwenden Sie zum Automatisieren der Verwaltung des Speicherplatzes auf dem Datenträger die Optionen `--retention` und `--trim`, um die alten Momentaufnahmen und Datenbankprotokolldateien automatisch zu bereinigen.
1. Erfolgreiche Ausführung der Momentaufnahmetools
    1. Überprüfen Sie die Datei `*.result` auf den Erfolg oder das Fehlschlagen der letzten Ausführung von `azacsnap`.
    1. Überprüfen Sie `/var/log/messages` auf Ausgaben des Befehls `azacsnap`.
1. Konsistenz der Momentaufnahmen, indem diese regelmäßig auf einem anderen System wiederhergestellt werden.

> [!NOTE]
> Um Momentaufnahmedetails aufzulisten, führen Sie den Befehl `azacsnap -c details` aus.

## <a name="delete-a-snapshot"></a>Löschen einer Momentaufnahme

Um eine Momentaufnahme zu löschen, führen Sie den Befehl `azacsnap -c delete` aus. Es ist nicht möglich, Momentaufnahmen auf Betriebssystemebene zu löschen. Sie müssen den richtigen Befehl (`azacsnap -c delete`) verwenden, um die Speichermomentaufnahmen zu löschen.

> [!IMPORTANT]
> Seien Sie besonders vorsichtig, wenn Sie eine Momentaufnahme löschen. Nach dem Löschen ist es **UNMÖGLICH**, die gelöschten Momentaufnahmen wiederherzustellen.

## <a name="restore-a-snapshot"></a>Wiederherstellen einer Momentaufnahme

Eine Speichervolume-Momentaufnahme kann auf einem neuen Volume wiederhergestellt (`-c restore --restore snaptovol`) werden.  Bei Azure (große Instanzen) kann das Volume wieder in eine Momentaufnahme (`-c restore --restore revertvolume`) umgewandelt werden.

> [!NOTE]
> Es wird **KEIN** Datenbankwiederherstellungsbefehl bereitgestellt.

Eine Momentaufnahme kann zurück in den SAP HANA-Datenbereich kopiert werden, aber SAP HANA darf nicht ausgeführt werden, wenn eine Kopie erstellt wird (`cp /hana/data/H80/mnt00001/.snapshot/hana_hourly.2020-06-17T113043.1586971Z/*`).

Für Azure (große Instanzen) können Sie sich an das Microsoft-Betriebsteam wenden, indem Sie eine Serviceanfrage zum Wiederherstellen einer gewünschten Momentaufnahme aus den vorhandenen verfügbaren Momentaufnahmen öffnen. Sie können eine Serviceanfrage im Azure-Portal öffnen: <https://portal.azure.com>

Wenn Sie ein Notfallwiederherstellungs-Failover durchführen möchten, stellt der Befehl `azacsnap -c restore --restore revertvolume` am Standort der Notfallwiederherstellung automatisch die neuesten (`/hana/data` und `/hana/logbackups`) Volumemomentaufnahmen zur Verfügung, um eine SAP HANA-Wiederherstellung zu ermöglichen. Verwenden Sie diesen Befehl mit Vorsicht, da er die Replikation zwischen Produktions- und Notfallwiederherstellungsstandorten unterbricht.

## <a name="set-up-snapshots-for-boot-volumes-only"></a>Einrichten von Momentaufnahmen für nur Startvolumes

> [!IMPORTANT]
> Dieser Vorgang gilt nur für Azure (große Instanzen).

In einigen Fällen verfügen Kunden bereits über Tools zum Schutz von SAP HANA und möchten nur Startvolume-Momentaufnahmen konfigurieren.  In diesem Fall vereinfacht sich die Aufgabe, und die folgenden Schritte sind auszuführen.

1. Führen Sie die Schritte 1–4 der erforderlichen Komponenten für die Installation aus.
1. Aktivieren der Kommunikation mit dem Speicher.
1. Herunterladen des Installationsprogramms, um die Momentaufnahmetools zu installieren.
1. Abschließen der Einrichtung der Momentaufnahmetools.
1. Erstellen einer neuen Konfigurationsdatei wie folgt. Die Details zum Startvolume müssen sich im Abschnitt „OtherVolume“ befinden (Benutzereinträge in <span style="color:red">Rot</span>):
    ```output
    > <span style="color:red">azacsnap -c configure --configuration new --configfile BootVolume.json</span>
    Building new config file
    Add comment to config file (blank entry to exit adding comments):<span style="color:red">Boot only config file.</span>
    Add comment to config file (blank entry to exit adding comments):
    Add database to config? (y/n) [n]: <span style="color:red">y</span>
    HANA SID (for example, H80): <span style="color:red">X</span>
    HANA Instance Number (for example, 00): <span style="color:red">X</span>
    HANA HDB User Store Key (for example, `hdbuserstore List`): <span style="color:red">X</span>
    HANA Server's Address (hostname or IP address): <span style="color:red">X</span>
    Add ANF Storage to database section? (y/n) [n]:
    Add HLI Storage to database section? (y/n) [n]: <span style="color:red">y</span>
    Add DATA Volume to HLI Storage section of Database section? (y/n) [n]:
    Add OTHER Volume to HLI Storage section of Database section? (y/n) [n]: <span style="color:red">y</span>
    Storage User Name (for example, clbackup25): <span style="color:red">shoasnap</span>
    Storage IP Address (for example, 192.168.1.30): <span style="color:red">10.1.1.10</span>
    Storage Volume Name (for example, hana_data_soldub41_t250_vol): <span style="color:red">t210_sles_boot_azsollabbl20a31_vol</span>
    Add OTHER Volume to HLI Storage section of Database section? (y/n) [n]:
    Add HLI Storage to database section? (y/n) [n]:
    Add database to config? (y/n) [n]:

    Editing configuration complete, writing output to 'BootVolume.json'.
    ```
1. Überprüfen Sie die Konfigurationsdatei, wobei Sie sich hierbei auf das folgende Beispiel beziehen:

    Verwenden Sie den Befehl `cat`, um den Inhalt der Konfigurationsdatei anzuzeigen:

    ```bash
    cat BootVolume.json
    ```

    ```output
    {
      "version": "5.0 Preview",
      "logPath": "./logs",
      "securityPath": "./security",
      "comments": [
        "Boot only config file."
      ],
      "database": [
        {
          "hana": {
            "serverAddress": "X",
            "sid": "X",
            "instanceNumber": "X",
            "hdbUserStoreName": "X",
            "savePointAbortWaitSeconds": 600,
            "hliStorage": [
              {
                "dataVolume": [],
                "otherVolume": [
                  {
                    "backupName": "shoasnap",
                    "ipAddress": "10.1.1.10",
                    "volume": "t210_sles_boot_azsollabbl20a31_vol"
                  }
                ]
              }
            ],
            "anfStorage": []
          }
        }
      ]
    }
    ```

1. Testen einer Startvolumesicherung

    ```bash
    azacsnap -c backup --volume other --prefix TestBootVolume --retention 1 --configfile BootVolume.json
    ```

1. Überprüfen Sie, ob sie aufgeführt ist, und fügen Sie die Option `--snapshotfilter` hinzu, um die Liste der zurückgegebenen Momentaufnahmen einzugrenzen.

    ```bash
    azacsnap -c details --snapshotfilter TestBootVolume --configfile BootVolume.json
    ```

    Befehlsausgabe:
    ```output
    List snapshot details called with snapshotFilter 'TestBootVolume'
    #, Volume, Snapshot, Create Time, HANA Backup ID, Snapshot Size
    #1, t210_sles_boot_azsollabbl20a31_vol, TestBootVolume.2020-07-03T034651.7059085Z, "Fri Jul 03 03:48:24 2020", "otherVolume Backup|azacsnap version: 5.0 Preview (20200617.75879)", 200KB
    , t210_sles_boot_azsollabbl20a31_vol, , , Size used by Snapshots, 1.31GB
    ```

1. Richten Sie die automatische Momentaufnahmensicherung ein.

> [!NOTE]
> Das Einrichten der Kommunikation mit SAP HANA ist nicht erforderlich.

## <a name="restore-a-boot-snapshot"></a>Wiederherstellen einer Startmomentaufnahme

> [!IMPORTANT]
> Dieser Vorgang gilt nur für Azure (große Instanzen).
> Der Server wird auf den Zeitpunkt wiederhergestellt, an dem die Momentaufnahme erstellt wurde.

Eine Startmomentaufnahme kann wie folgt wiederhergestellt werden:

1. Der Kunde muss hierzu den Server herunterfahren.
1. Nachdem der Server heruntergefahren wurde, muss der Kunde eine Serviceanfrage öffnen, die die Computer-ID und die wiederherzustellende Momentaufnahme enthält.
    > Kunden können eine Serviceanfrage im Azure-Portal öffnen: <https://portal.azure.com>
1. Microsoft stellt die Betriebssystem-LUN mithilfe der angegebenen Computer-ID und der Momentaufnahme wieder her und startet dann den Server.
1. Der Kunde muss dann überprüfen, ob der Server gestartet wurde und fehlerfrei ist.

Nach der Wiederherstellung müssen keine weiteren Schritte ausgeführt werden.

## <a name="key-facts-to-know-about-snapshots"></a>Wichtiges Faktenwissen über Momentaufnahmen

Hauptattribute von Speichervolume-Momentaufnahmen:

- **Speicherort von Momentaufnahmen**: Momentaufnahmen befinden sich in einem virtuellen Verzeichnis (`.snapshot`) innerhalb des Volumes.  Sehen Sie sich das folgende Beispiel für **Azure (große Instanzen)** an:
  - Datenbank: `/hana/data/<SID>/mnt00001/.snapshot`
  - Freigegeben: `/hana/shared/<SID>/.snapshot`
  - Protokolle: `/hana/logbackups/<SID>/.snapshot`
  - Start: Startmomentaufnahmen für HLI werden auf Betriebssystemebene **nicht angezeigt**, können jedoch mit `azacsnap -c details` aufgelistet werden.

  > [!NOTE]
  >  `.snapshot` ist ein schreibgeschützter, ausgeblendeter *virtueller* Ordner, der schreibgeschützten Zugriff auf die Momentaufnahmen gewährt.

- **Max. Momentaufnahme:** Die Hardware kann bis zu 250 Momentaufnahmen pro Volume unterstützen. Der Momentaufnahmebefehl behält basierend auf der in der Befehlszeile festgelegten Aufbewahrungsdauer eine maximale Anzahl von Momentaufnahmen für das Präfix bei und löscht die älteste Momentaufnahme, wenn die maximal aufzubewahrende Anzahl überschritten wird.
- **Name der Momentaufnahme:** Der Name der Momentaufnahme enthält die vom Kunden bereitgestellte Präfixbezeichnung.
- **Größe der Momentaufnahme:** Hängt von der Größe/den Änderungen auf Datenbankebene ab.
- **Speicherort der Protokolldatei:** Die von den Befehlen generierten Protokolldateien werden in Ordner ausgegeben, die in der JSON-Konfigurationsdatei definiert sind. Standardmäßig handelt es sich hierbei um einen Unterordner des Ordners, in dem der Befehl ausgeführt wird (z. B. `./logs`).

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung](azacsnap-troubleshoot.md)
