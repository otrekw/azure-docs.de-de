---
title: Konfigurieren, Optimieren und Problembehandlung in AzCopy mit Azure Storage | Microsoft-Dokumentation
description: Konfigurieren, Optimieren und Problembehandlung in AzCopy.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: c3ee0f335741c171c3a7ee1df3eea6dea9c4b728
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2020
ms.locfileid: "82176157"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Konfigurieren, Optimieren und Problembehandlung in AzCopy

AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Blobs oder Dateien in oder aus einem Speicherkonto zu kopieren. Dieser Artikel hilft Ihnen dabei, erweiterte Konfigurationsaufgaben durchzuführen, und hilft Ihnen bei der Behandlung von Problemen, die bei der Verwendung von AzCopy auftreten können.

> [!NOTE]
> Wenn Sie nach Inhalten suchen, die Ihnen bei den ersten Schritten mit AzCopy helfen, sehen Sie in den folgenden Artikeln nach:
> - [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
> - [Übertragen von Daten mit AzCopy und Blob Storage](storage-use-azcopy-blobs.md)
> - [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
> - [Übertragen von Daten mit AzCopy und Amazon S3-Buckets](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

Um die Proxyeinstellungen für AzCopy zu konfigurieren, legen Sie die Umgebungsvariable `https_proxy` fest. Unter Windows ist diese Einstellung nicht erforderlich, da AzCopy Proxyeinstellungen automatisch erkennt. Falls Sie diese Einstellung unter Windows verwenden, wird dadurch die automatische Erkennung außer Kraft gesetzt.

| Betriebssystem | Get-Help  |
|--------|-----------|
| **Windows** | Verwenden Sie in einer Eingabeaufforderung: `set https_proxy=<proxy IP>:<proxy port>`<br> Verwenden Sie in PowerShell: `$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **MacOS** | `export https_proxy=<proxy IP>:<proxy port>` |

AzCopy unterstützt zurzeit keine Proxys, für die eine Authentifizierung mit NTLM oder Kerberos erforderlich ist.

## <a name="optimize-performance"></a>Optimieren der Leistung

Sie können Leistungsvergleichstests durchführen und dann mithilfe von Befehlen und Umgebungsvariablen ein ausgewogenes Verhältnis zwischen Leistung und Ressourcenverbrauch ermitteln.

In diesem Abschnitt wird beschrieben, wie Sie diese Optimierungsaufgaben ausführen:

> [!div class="checklist"]
> * Ausführen von Vergleichstests
> * Optimieren des Durchsatzes
> * Optimieren der Arbeitsspeichernutzung 
> * Optimieren der Dateisynchronisierung

### <a name="run-benchmark-tests"></a>Ausführen von Vergleichstests

Sie können einen Leistungsvergleichstest für bestimmte Blobcontainer ausführen, um allgemeine Leistungsstatistiken zu erhalten und Leistungsengpässe zu ermitteln. 

Verwenden Sie den folgenden Befehl, um einen Leistungsvergleichstest auszuführen.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy bench 'https://<storage-account-name>.blob.core.windows.net/<container-name>'` |
| **Beispiel** | `azcopy bench 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

> [!TIP]
> In diesem Beispiel werden Pfadargumente in einfache Anführungszeichen ('') eingeschlossen. Verwenden Sie in allen Befehlsshells außer der Windows-Befehlszeile (cmd.exe) einfache Anführungszeichen. Wenn Sie eine Windows-Befehlszeile (cmd.exe) verwenden, müssen Sie Pfadargumente in doppelte Anführungszeichen ("") anstelle von einfachen Anführungszeichen ('') einschließen.

Mit diesem Befehl wird ein Leistungsvergleichstest ausgeführt, indem Testdaten an ein angegebenes Ziel hochgeladen werden. Die Testdaten werden im Arbeitsspeicher generiert, an das Ziel hochgeladen und dann nach Abschluss des Tests aus dem Ziel gelöscht. Mithilfe optionaler Befehlsparameter können Sie angeben, wie viele Dateien und in welcher Größe generiert werden sollen.

Ausführliche Referenzdokumente finden Sie unter [azcopy bench](storage-ref-azcopy-bench.md).

Wenn Sie ausführliche Hilfe zu diesem Befehl anzeigen möchten, geben Sie `azcopy bench -h` ein, und drücken Sie dann EINGABE.

### <a name="optimize-throughput"></a>Optimieren des Durchsatzes

Mithilfe des Flags `cap-mbps` in den Befehlen können Sie eine Obergrenze für die Durchsatzdatenrate festlegen. Mit dem folgenden Befehl wird beispielsweise ein Auftrag fortgesetzt und der Durchsatz auf `10` Megabyte (MB) pro Sekunde begrenzt. 

```azcopy
azcopy jobs resume <job-id> --cap-mbps 10
```

Bei der Übertragung kleiner Dateien kann der Durchsatz zurückgehen. Sie können den Durchsatz durch Festlegen der Umgebungsvariablen `AZCOPY_CONCURRENCY_VALUE` erhöhen. Diese Variable gibt die zulässige Anzahl gleichzeitiger Anforderungen an.  

Wenn Ihr Computer über weniger als 5 CPUs verfügt, wird der Wert dieser Variablen auf `32` festgelegt. Andernfalls ist der Standardwert gleich 16, multipliziert mit der Anzahl der CPUs. Der maximale Standardwert dieser Variablen ist `3000`, aber Sie können diesen Wert manuell höher oder niedriger festlegen. 

| Betriebssystem | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **MacOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Verwenden Sie `azcopy env`, um den aktuellen Wert dieser Variablen zu überprüfen. Wenn der Wert leer ist, können Sie den verwendeten Wert ermitteln, indem Sie sich den Anfang einer AzCopy-Protokolldatei ansehen. Dort sind der ausgewählte Wert und der Grund aufgeführt, warum er ausgewählt wurde.

Bevor Sie diese Variable festlegen, wird empfohlen, einen Vergleichstest auszuführen. Im Vergleichstest wird der empfohlene Parallelitätswert angegeben. Wenn die Netzwerkbedingungen und Nutzlasten variieren, legen Sie diese Variable alternativ auf den Begriff `AUTO` anstatt auf eine bestimmte Zahl fest. Dies bewirkt, dass von AzCopy immer derselbe automatische Abstimmungsprozess ausgeführt wird, der in Vergleichstests verwendet wird.

### <a name="optimize-memory-use"></a>Optimieren der Arbeitsspeichernutzung

Legen Sie die Umgebungsvariable `AZCOPY_BUFFER_GB` fest, um die maximale Kapazität an Systemarbeitsspeicher festzulegen, die AzCopy beim Herunterladen und Hochladen von Dateien verwenden soll.
Geben Sie diesen Wert in Gigabytes (GB) an.

| Betriebssystem | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_BUFFER_GB=<value>` |
| **Linux** | `export AZCOPY_BUFFER_GB=<value>` |
| **MacOS** | `export AZCOPY_BUFFER_GB=<value>` |

### <a name="optimize-file-synchronization"></a>Optimieren der Dateisynchronisierung

Der Befehl [sync](storage-ref-azcopy-sync.md) identifiziert alle Dateien am Ziel und vergleicht dann die Dateinamen und die Zeitstempel der letzten Änderung vor dem Starten des Synchronisierungsvorgangs. Wenn Sie über eine große Anzahl von Dateien verfügen, können Sie die Leistung verbessern, indem Sie diese Vorabverarbeitung vermeiden. 

Dazu müssen Sie stattdessen den Befehl [azcopy copy](storage-ref-azcopy-copy.md) verwenden und das Flag `--overwrite` auf `ifSourceNewer` festlegen. AzCopy vergleicht Dateien, während sie kopiert werden, ohne vorab Überprüfungen und Vergleiche auszuführen. In Fällen, in denen sehr viele Dateien verglichen werden müssen, bietet dies einen Leistungsvorteil.

Mit dem Befehl [azcopy copy](storage-ref-azcopy-copy.md) werden keine Dateien aus dem Ziel gelöscht. Wenn Sie also Dateien aus dem Ziel löschen möchten, wenn sie nicht mehr in der Quelle vorhanden sind, verwenden Sie den Befehl [azcopy sync](storage-ref-azcopy-sync.md), und legen Sie dabei das Flag `--delete-destination` auf den Wert `true` oder `prompt` fest. 

## <a name="troubleshoot-issues"></a>Behandeln von Problemen

AzCopy erstellt Protokoll- und Plandateien für jeden Auftrag. Sie können die Protokolle verwenden, um potenzielle Probleme zu untersuchen und zu beheben. 

Die Protokolle enthalten den Status des Fehlers (`UPLOADFAILED`, `COPYFAILED` und `DOWNLOADFAILED`), den vollständigen Pfad und die Ursache des Fehlers.

Standardmäßig befinden sich die Protokoll- und Plandateien im Verzeichnis `%USERPROFILE%\.azcopy` unter Windows bzw. im Verzeichnis `$HOME$\.azcopy` unter Mac und Linux. Allerdings können Sie den Speicherort ändern.

Der relevante Fehler ist nicht zwingend der erste Fehler in der Datei. Bei Netzwerkfehlern, Timeouts, Serverauslastungsfehlern und Ähnlichem wird der Vorgang von AzCopy bis zu 20-mal wiederholt, was in der Regel zum Erfolg führt.  Der erste angezeigte Fehler ist unter Umständen ein harmloser Vorgang, der erfolgreich wiederholt wurde.  Sehen Sie sich daher nicht den ersten Fehler in der Datei an, sondern die Fehler in der Nähe von `UPLOADFAILED`, `COPYFAILED` oder `DOWNLOADFAILED`. 

> [!IMPORTANT]
> Geben Sie beim Senden einer Anforderung an den Microsoft-Support (oder bei der Problembehandlung unter Einbeziehung eines Drittanbieters) die bearbeitete Version des Befehls an, den Sie ausführen möchten. So wird sichergestellt, dass die SAS nicht versehentlich offengelegt wird. Die editierte Version steht am Anfang der Protokolldatei.

### <a name="review-the-logs-for-errors"></a>Überprüfen der Protokolle auf Fehler

Der folgende Befehl ruft alle Fehler mit dem Status `UPLOADFAILED` aus dem Protokoll `04dc9ca9-158f-7945-5933-564021086c79` ab:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Anzeigen und Fortsetzen von Aufträgen

Jeder Übertragungsvorgang erstellt einen AzCopy-Auftrag. Verwenden Sie den folgenden Befehl, um den Verlauf von Aufträgen anzuzeigen:

```
azcopy jobs list
```

Verwenden Sie den folgenden Befehl, um die Auftragsstatistiken anzuzeigen:

```
azcopy jobs show <job-id>
```

Um die Übertragungen nach ihrem Status zu filtern, verwenden Sie den folgenden Befehl:

```
azcopy jobs show <job-id> --with-status=Failed
```

Verwenden Sie den folgenden Befehl, um einen fehlgeschlagenen bzw. abgebrochenen Auftrag fortzusetzen. Dieser Befehl verwendet seinen Bezeichner zusammen mit dem SAS-Token, da er aus Sicherheitsgründen nicht persistent ist:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

> [!TIP]
> Schließen Sie Pfadargumente wie das SAS-Token in einfache Anführungszeichen ('') ein. Verwenden Sie in allen Befehlsshells außer der Windows-Befehlszeile (cmd.exe) einfache Anführungszeichen. Wenn Sie eine Windows-Befehlszeile (cmd.exe) verwenden, müssen Sie Pfadargumente in doppelte Anführungszeichen ("") anstelle von einfachen Anführungszeichen ('') einschließen.

Wenn Sie einen Auftrag fortsetzen, überprüft AzCopy die Plandatei. In der Plandatei werden alle Dateien aufgelistet, die zur Verarbeitung identifiziert wurden, als der Auftrag erstmalig erstellt wurde. Wenn Sie einen Auftrag fortsetzen, versucht AzCopy, alle Dateien zu übertragen, die in der Plandatei aufgeführt sind, die nicht bereits übertragen wurden.

## <a name="change-the-location-of-the-plan-and-log-files"></a>Ändern des Speicherorts der Plan- und Protokolldateien

Standardmäßig befinden sich die Plan- und Protokolldateien im Verzeichnis `%USERPROFILE%\.azcopy` unter Windows bzw. im Verzeichnis `$HOME$\.azcopy` unter Mac und Linux. Sie können diesen Speicherort ändern.

### <a name="change-the-location-of-plan-files"></a>Ändern des Speicherorts der Plandateien

Verwenden Sie einen der folgende Befehle.

| Betriebssystem | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **Linux** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_JOB_PLAN_LOCATION=<value>` |

Verwenden Sie `azcopy env`, um den aktuellen Wert dieser Variablen zu überprüfen. Wenn der Wert leer ist, werden Plandateien an den Standardspeicherort geschrieben.

### <a name="change-the-location-of-log-files"></a>Ändern des Speicherorts der Protokolldateien

Verwenden Sie einen der folgende Befehle.

| Betriebssystem | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **MacOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Verwenden Sie `azcopy env`, um den aktuellen Wert dieser Variablen zu überprüfen. Wenn der Wert leer ist, werden Protokolle an den Standardspeicherort geschrieben.

## <a name="change-the-default-log-level"></a>Ändern der Standardprotokollebene

Standardmäßig ist die AzCopy-Protokollebene auf `INFO` festgelegt. Wenn Sie die Ausführlichkeit des Protokolls verringern möchten, um Speicherplatz auf dem Datenträger zu sparen, überschreiben Sie die Einstellung mithilfe der Option ``--log-level``. 

Verfügbare Protokolliergrade sind: `NONE`, `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC` und `FATAL`.

## <a name="remove-plan-and-log-files"></a>Entfernen von Plan- und Protokolldateien

Wenn Sie alle Plan- und Protokolldateien von Ihrem lokalen Computer entfernen möchten, um Speicherplatz zu sparen, verwenden Sie den Befehl `azcopy jobs clean`.

Verwenden Sie `azcopy jobs rm <job-id>`, um die Plan- und Protokolldateien zu entfernen, die nur einem Auftrag zugeordnet sind. Ersetzen Sie den Platzhalter `<job-id>` in diesem Beispiel durch die Auftrags-ID des Auftrags.


