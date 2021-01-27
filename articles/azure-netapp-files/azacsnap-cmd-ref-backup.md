---
title: Sichern mithilfe des Tools für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Bietet eine Anleitung zum Ausführen des Sicherungsbefehls des Tools für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
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
ms.openlocfilehash: 17c29fdf88495f6ecc40963eda08858887173fd1
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98730937"
---
# <a name="back-up-using-azure-application-consistent-snapshot-tool-preview"></a>Sichern mithilfe des Tools für konsistente Momentaufnahmen in Azure-Anwendungen (Vorschau)

Dieser Artikel enthält eine Anleitung zum Ausführen des Sicherungsbefehls für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.

## <a name="introduction"></a>Einführung

Eine auf einer Speichermomentaufnahme basierende Sicherung wird mit dem Befehl `azacsnap -c backup` ausgeführt.  Dieser Befehl führt die Orchestrierung einer datenbankkonsistenten Speichermomentaufnahme auf den DATEN-Volumes durch sowie eine Speichermomentaufnahme (ohne Datenbankkonsistenz-Einrichtung) auf den ANDEREN Volumes.  

Für DATEN-Volumes bereitet `azacsnap` die Datenbank auf eine Speichermomentaufnahme vor, dann wird die Speichermomentaufnahme für alle konfigurierten Volumes erstellt, und schließlich wird die Datenbank darüber informiert, dass die Momentaufnahme abgeschlossen ist.  Ferner werden alle Datenbankeinträge verwaltet, die Momentaufnahmesicherungsaktivitäten aufzeichnen (z. B. SAP HANA-Sicherungskatalog).

## <a name="command-options"></a>Befehlsoptionen

Der Befehl `-c backup` akzeptiert folgende Argumente:

- `--volume=` Typ des Volumes für die Momentaufnahme, dieser Parameter kann `data` oder `other` enthalten.
  - `data` Erstellt eine Momentaufnahmen des Volumes im Abschnitt „dataVolume“ der Konfigurationsdatei.
  - `other` Erstellt eine Momentaufnahmen des Volumes im Abschnitt „otherVolume“ der Konfigurationsdatei.
  
  > [!NOTE]
  > Wenn Sie eine separate Konfigurationsdatei mit dem Startvolume als „otherVolume“ erstellen, können `boot`-Momentaufnahmen nach einem ganz anderen Zeitplan erstellt werden (z. B. täglich).

- `--prefix=` Das Momentaufnahmepräfix des Kunden für den Namen der Momentaufnahme. Dieser Parameter hat zwei Funktionen. Zunächst soll ein eindeutiger Name für die Gruppierung von Momentaufnahmen bereitgestellt werden. Ferner soll die `--retention`-Anzahl von Speichermomentaufnahmen ermittelt werden, die für das angegebene `--prefix` aufbewahrt werden.

    > [!IMPORTANT]
    > Nur alphanumerische Zeichen („A–Z, a–z, 0–9“), Unterstriche („_“) und Bindestriche („-“) sind zulässig.

- `--retention` Die Anzahl der Momentaufnahmen mit dem definierten `--prefix`, die aufbewahrt werden sollen. Alle zusätzlichen Momentaufnahmen werden entfernt, nachdem eine neue Momentaufnahme für dieses `--prefix` erstellt wurde.

- `--trim` Verfügbar für SAP HANA v2 und höher. Diese Option verwaltet den Sicherungskatalog, den Katalog auf dem Datenträger und Protokollsicherungen. Die Anzahl der im Sicherungskatalog aufzubewahrenden Einträge wird durch die oben genannte Option `--retention` bestimmt, und ältere Einträge für das definierte Präfix (`--prefix`) werden aus dem Sicherungskatalog sowie die Sicherung der zugehörigen physischen Protokolle wird gelöscht. Ferner werden alle Protokollsicherungseinträge gelöscht, die älter sind als der älteste Nicht-Protokollsicherungseintrag. Mit diesem Vorgang wird verhindert, dass die Protokollsicherungen den gesamten verfügbaren Speicherplatz verbrauchen.

  > [!NOTE]
  > Mit dem folgenden Beispielbefehl werden 9 Speichermomentaufnahmen aufbewahrt und sichergestellt, dass der Sicherungskatalog fortlaufend gekürzt wird, damit er die aufbewahrten 9 Speichermomentaufnahmen erfüllt.

    ```bash
    azacsnap -c backup --volume data --prefix hana_TEST --retention 9 --trim
    ```

- `[--ssl=]` Ein optionaler Parameter, der die Verschlüsselungsmethode definiert, die für die Kommunikation mit SAP HANA verwendet wird – `openssl` oder `commoncrypto`. Wenn dieses Argument festgelegt wird, erwartet der Befehl `azacsnap -c backup` zwei Dateien im selben Verzeichnis. Diese Dateien müssen nach der entsprechenden SID benannt sein. Weitere Informationen finden Sie unter [Verwenden von SSL für die Kommunikation mit SAP HANA](azacsnap-installation.md#using-ssl-for-communication-with-sap-hana). Im folgenden Beispiel wird eine Momentaufnahme vom Typ `hana` mit dem Präfix `hana_TEST` erstellt, wovon `9` aufbewahrt werden, wobei mittels SSL (`openssl`) mit SAP HANA kommuniziert wird.

    ```bash
    azacsnap -c backup --volume data --prefix hana_TEST --retention 9 --trim --ssl=openssl
    ```

- `[--configfile <config filename>]` ist ein optionaler Parameter, der benutzerdefinierte Konfigurationsdateinamen zulässt.

## <a name="snapshot-backups-are-fast"></a>Momentaufnahmesicherungen sind schnell

Die Dauer einer Momentaufnahmesicherung ist unabhängig von der Volumegröße, wobei die Momentaufnahme eines 10-TB-Volumes ungefähr innerhalb derselben Zeit wie die eines 10-GB-Volumes erstellt wird.  

Die Hauptfaktoren, die sich auf die Gesamtausführungszeit auswirken, sind die Anzahl der Volumes, für die Momentaufnahmen erstellt werden sollen, sowie alle Änderungen am Parameter `--retention` (wobei eine Verringerung die Ausführungszeit verlängern kann, wenn überschüssige Momentaufnahmen entfernt werden).

In der obigen Beispielkonfiguration (für **Azure (große Instanzen)** ) benötigten Momentaufnahmen für die beiden Volumes weniger als 5 Sekunden. Für **Azure NetApp Files** würden Momentaufnahmen für die beiden Volumes ungefähr 60 Sekunden dauern.

> [!NOTE]
> Wenn `--retention` erheblich gegenüber dem vorherigen Mall verringert wird, als `azacsnap` ausgeführt wurde (z. B. von `--retention 50` auf `--retention 5`), erhöht sich die benötigte Zeit, da `azacsnap` die zusätzlichen Momentaufnahmen entfernen muss.

## <a name="example-with-data-parameter"></a>Beispiel mit dem Parameter `data`

```bash
azacsnap -c backup --volume data --prefix hana_TEST --retention 9 --trim
```

Der Befehl gibt keine Ausgabe an die Konsole aus, schreibt aber in eine Protokolldatei, eine Ergebnisdatei und in `/var/log/messages`.

Die *Protokolldatei* besteht aus dem Befehlsnamen und der Option „-c“ mit dem Konfigurationsdateinamen. Standardmäßig wird ein Protokolldateiname für eine `-c backup` mit einem standardmäßigen Konfigurationsdateinamen `azacsnap-backup-azacsnap.log` ausgeführt.

Die *Ergebnis* datei hat denselben Basisnamen wie die Protokolldatei mit `.result` als Suffix, z. B. `azacsnap-backup-azacsnap.result`, die die folgende Ausgabe enthält:

```bash
cat logs/azacsnap-backup-azacsnap.result
```

```output
Database # 1 (H80) : completed ok
```

Die Datei in `/var/log/messages` enthält dieselbe Ausgabe wie die Datei `.result`. Sehen Sie sich folgendes Beispiel an (ausgeführt als root):

```bash
grep "azacsnap.*Database" /var/log/messages | tail -n10
```

```output
Jul  2 05:22:07 server01 azacsnap[183868]: Database # 1 (H80) : completed ok
Jul  2 05:27:06 server01 azacsnap[4069]: Database # 1 (H80) : completed ok
Jul  2 05:32:07 server01 azacsnap[19769]: Database # 1 (H80) : completed ok
Jul  2 05:37:06 server01 azacsnap[35312]: Database # 1 (H80) : completed ok
Jul  2 05:42:06 server01 azacsnap[50877]: Database # 1 (H80) : completed ok
Jul  2 05:47:06 server01 azacsnap[66429]: Database # 1 (H80) : completed ok
Jul  2 05:52:06 server01 azacsnap[82964]: Database # 1 (H80) : completed ok
Jul  2 05:57:06 server01 azacsnap[98522]: Database # 1 (H80) : completed ok
Jul  2 05:59:13 server01 azacsnap[105519]: Database # 1 (H80) : completed ok
Jul  2 06:02:06 server01 azacsnap[114280]: Database # 1 (H80) : completed ok
```

## <a name="example-with-other-parameter"></a>Beispiel mit dem Parameter `other`

```bash
azacsnap -c backup --volume other --prefix logs_TEST --retention 9
```

Der Befehl gibt keine Ausgabe an die Konsole aus, schreibt aber nur in eine Protokolldatei.  Er schreibt _nicht_ in eine Ergebnisdatei oder in `/var/log/messages`.

Die *Protokolldatei* besteht aus dem Befehlsnamen und der Option „-c“ mit dem Konfigurationsdateinamen. Standardmäßig wird ein Protokolldateiname für eine `-c backup` mit einem standardmäßigen Konfigurationsdateinamen `azacsnap-backup-azacsnap.log` ausgeführt.

## <a name="example-with-other-parameter-to-backup-host-os"></a>Beispiel mit dem Parameter `other` (zum Sichern des Host-BS)

> [!NOTE]
> Die Verwendung einer anderen Konfigurationsdatei (`--configfile bootVol.json`), die nur die Startvolumes enthält.

```bash
azacsnap -c backup --volume other --prefix boot_TEST --retention 9 --configfile bootVol.json
```

Der Befehl gibt keine Ausgabe an die Konsole aus, schreibt aber nur in eine Protokolldatei.  Er schreibt _nicht_ in eine Ergebnisdatei oder in `/var/log/messages`.

Der Name der *Protokolldatei* in diesem Beispiel lautet `azacsnap-backup-bootVol.log`.

> [!NOTE]
> Der Name der Protokolldatei besteht aus dem „(Befehlsnamen-(der Option `-c`)-(dem Konfigurationsdateinamen)“.  Wenn Sie z. B. die Option `-c backup` mit dem Protokolldateinamen `h80.json` verwenden, wird die Protokolldatei `azacsnap-backup-h80.log` aufgerufen.  Oder wenn Sie die Option `-c test` mit derselben Konfigurationsdatei verwenden, wird die Protokolldatei `azacsnap-test-h80.log` aufgerufen.

- Typ der großen HANA-Instanz: Es gibt zwei gültige Werte mit `TYPEI` bzw. `TYPEII`, je nach der großen HANA-Instanzeinheit.
- Die verfügbaren SKUs können Sie unter [Verfügbare SKUs für große HANA-Instanzen](../virtual-machines/workloads/sap/hana-available-skus.md) bestätigen.

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen von Momentaufnahmedetails](azacsnap-cmd-ref-details.md)
- [Löschen von Momentaufnahmen](azacsnap-cmd-ref-delete.md)