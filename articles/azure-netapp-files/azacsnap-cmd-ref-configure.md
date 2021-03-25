---
title: Konfigurieren des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Bietet eine Anleitung zum Ausführen des Konfigurationsbefehls des Tools für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
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
ms.openlocfilehash: 0875aae8bb9049fc96377c1c98efa7391211d08f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97632193"
---
# <a name="configure-azure-application-consistent-snapshot-tool-preview"></a>Konfigurieren des Tools für konsistente Momentaufnahmen in Azure-Anwendungen (Vorschau)

Dieser Artikel enthält eine Anleitung zum Ausführen des Konfigurationsbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.

## <a name="introduction"></a>Einführung

Die Konfigurationsdatei kann mit dem Befehl `azacsnap -c configure` erstellt oder bearbeitet werden.

## <a name="command-options"></a>Befehlsoptionen

Der Befehl `-c configure` hat folgende Optionen:

- `--configuration new` zum Erstellen einer neuen Konfigurationsdatei.

- `--configuration edit` zum Bearbeiten einer vorhandenen Konfigurationsdatei.

- `[--configfile <config filename>]` ist ein optionaler Parameter, der benutzerdefinierte Konfigurationsdateinamen zulässt.

## <a name="configuration-file-for-snapshot-tools"></a>Konfigurationsdatei für Momentaufnahmetools

Eine Konfigurationsdatei kann durch Ausführen von `azacsnap -c configure --configuration new` erstellt werden.  Standardmäßig lautet der Name der Konfigurationsdatei `azacsnap.json`.  Ein benutzerdefinierter Dateiname kann mit dem Parameter `--configfile=` verwenden werden (z. B. `--configfile=<customname>.json`). Das folgende Beispiel gilt für die Konfiguration von Azure (große Instanzen):

```bash
azacsnap -c configure --configuration new
```

```output
Building new config file
Add comment to config file (blank entry to exit adding comments):This is a new config file for `azacsnap`
Add comment to config file (blank entry to exit adding comments):
Add database to config? (y/n) [n]: y
HANA SID (for example, H80): H80
HANA Instance Number (for example, 00): 00
HANA HDB User Store Key (for example, `hdbuserstore List`): AZACSNAP
HANA Server's Address (hostname or IP address): testing01
Add ANF Storage to database section? (y/n) [n]:
Add HLI Storage to database section? (y/n) [n]: y
Add DATA Volume to HLI Storage section of Database section? (y/n) [n]: y
Storage User Name (for example, clbackup25): clt1h80backup
Storage IP Address (for example, 192.168.1.30): 172.18.18.11
Storage Volume Name (for example, hana_data_h80_testing01_mnt00001_t250_vol): hana_data_h80_testing01_mnt00001_t020_vol
Add DATA Volume to HLI Storage section of Database section? (y/n) [n]:
Add OTHER Volume to HLI Storage section of Database section? (y/n) [n]:
Add HLI Storage to database section? (y/n) [n]:
Add database to config? (y/n) [n]:
Editing configuration complete, writing output to 'azacsnap.json'
```

## <a name="details-of-required-values"></a>Details zu erforderlichen Werten

In den folgenden Abschnitten finden Sie ausführliche Anleitungen zu den verschiedenen Werten, die für die Konfigurationsdatei erforderlich sind.

### <a name="sap-hana-values"></a>SAP HANA-Werte

Wenn Sie der Konfiguration eine *Datenbank* hinzufügen, sind die folgenden Werte erforderlich:

- **HANA-Serveradresse** = Der Hostname oder die IP-Adresse des SAP HANA-Servers.
- **HANA-SID** = Die SAP HANA-System-ID.
- **HANA-Instanznummer** = Die SAP HANA-Instanznummer.
- **HANA HDB-Benutzerspeicherschlüssel** = Der SAP HANA-Benutzer, der mit Berechtigungen zum Ausführen von Datenbanksicherungen konfiguriert ist.

- Einzelner Knoten: IP-Adresse und Hostname des Knotens.
- HSR mit STONITH: IP-Adresse und Hostname des Knotens.
- Horizontales Skalieren (N+N, N+M): Aktuelle IP-Adresse und Hostname des Masterknotens.
- HSR ohne STONITH: IP-Adresse und Hostname des Knotens.
- Multi-SID auf einzelnem Knoten: Hostname und IP-Adresse des Knotens, der diese SIDs hostet.

### <a name="azure-large-instance-hli-storage-values"></a>Speicherwerte für Azure (große Instanzen) (HLI)

Wenn Sie einem Datenbankabschnitt *HLI-Speicher* hinzufügen, sind die folgenden Werte erforderlich:

- **Speicherbenutzername** = Dieser Wert ist der Benutzername, der zum Herstellen der SSH-Verbindung mit dem Speicher verwendet wird.
- **Speicher-IP-Adresse** = Die IP-Adresse Speichersystems.
- **Speichervolumename** = Der Volumename für die Momentaufnahme.  Dieser Wert kann auf verschiedene Weisen festgelegt werden. Möglicherweise ist es am einfachsten, den folgenden Shellbefehl auszuprobieren:

    ```bash
    grep nfs /etc/fstab | cut -f2 -d"/" | sort | uniq
    ```

    ```output
    hana_data_p40_soldub41_mnt00001_t020_vol
    hana_log_backups_p40_soldub41_t020_vol
    hana_log_p40_soldub41_mnt00001_t020_vol
    hana_shared_p40_soldub41_t020_vol
    ```

### <a name="azure-netapp-files-anf-storage-values"></a>Speicherwerte für Azure NetApp Files (ANF)

Wenn Sie einem Datenbankabschnitt *ANF-Speicher* hinzufügen, sind die folgenden Werte erforderlich:

- **Dateiname der Dienstprinzipalauthentifizierung** = Dies ist die beim Konfigurieren der Kommunikation mit dem Azure NetApp Files-Speicher in der Cloud Shell generierte Datei `authfile.json`.
- **Vollständige Ressourcen-ID des ANF-Speichervolumes** = Die vollständige Ressourcen-ID des Volumes, von dem eine Momentaufnahme erstellt wird.  Diese kann wie folgt abgerufen werden: Azure-Portal –> ANF –> Volume –> Einstellungen/Eigenschaften –> Ressourcen-ID

## <a name="configuration-file-overview-azacsnapjson"></a>Konfigurationsdateiübersicht (`azacsnap.json`)

Im folgenden Beispiel wird `azacsnap.json` mit der einen SID konfiguriert.

Die Parameterwerte müssen auf die spezifische SAP HANA-Umgebung des Kunden festgelegt werden.
Für **Azure (große Instanzen)** werden diese Informationen von der Microsoft-Dienstverwaltung während des Onboarding-/Übergabeanrufs bereitgestellt und in einer Excel-Datei zur Verfügung gestellt, die während der Übergabe bereitgestellt wird. Öffnen Sie eine Serviceanfrage, wenn Sie diese Informationen erneut benötigen.

Im Folgenden finden Sie nur ein Beispiel. Aktualisieren Sie alle Werte entsprechend.

```bash
cat azacsnap.json
```

```output
{
  "version": "5.0 Preview",
  "logPath": "./logs",
  "securityPath": "./security",
  "comments": [],
  "database": [
    {
      "hana": {
        "serverAddress": "sapprdhdb80",
        "sid": "H80",
        "instanceNumber": "00",
        "hdbUserStoreName": "SCADMIN",
        "savePointAbortWaitSeconds": 600,
        "hliStorage": [
          {
            "dataVolume": [
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_data_h80_azsollabbl20a31_mnt00001_t210_vol"
              },
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_shared_h80_azsollabbl20a31_t210_vol"
              }
            ],
            "otherVolume": [
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_log_backups_h80_azsollabbl20a31_t210_vol"
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

> [!NOTE]
> Für ein Notfallwiederherstellungsszenario, bei dem Sicherungen am Standort der Notfallwiederherstellung ausgeführt werden müssen, sollte der in der Notfallwiederherstellungsdatei (z. B. `DR.json`) am Standort der Notfallwiederherstellung konfigurierte HANA-Servername mit dem Namen des Produktionsservers identisch sein.

> [!NOTE]
> Bei Azure (große Instanzen) muss sich Ihre Speicher-IP-Adresse im selben Subnetz wie Ihr Serverpool befinden. In diesem Fall ist unser Serverpool-Subnetz beispielsweise 172. 18. 18.0/24, und unsere zugewiesene Speicher-IP-Adresse ist 172.18.18.11.

## <a name="next-steps"></a>Nächste Schritte

- [Testen des Tools für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-cmd-ref-test.md)
