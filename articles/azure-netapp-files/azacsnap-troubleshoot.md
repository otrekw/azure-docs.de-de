---
title: Problembehandlung für das Tool für anwendungskonsistente Momentaufnahmen in Azure für Azure NetApp Files | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Problembehandlung für das Tool für anwendungskonsistente Momentaufnahmen in Azure, das Sie mit Azure NetApp Files verwenden können.
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
ms.topic: troubleshooting
ms.date: 12/14/2020
ms.author: phjensen
ms.openlocfilehash: 7ba5a05515284612e17d5aba4cc673c7e78f7ba1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104869921"
---
# <a name="troubleshoot-azure-application-consistent-snapshot-tool-preview"></a>Problembehandlung für das Tool für anwendungskonsistente Momentaufnahmen in Azure (Vorschau)

Dieser Artikel bietet Informationen zur Problembehandlung für das Tool für anwendungskonsistente Momentaufnahmen in Azure (Azure Application Consistent Snapshot, AzAcSnap), das Sie mit Azure NetApp Files verwenden können.

Hier finden Sie einige häufige Probleme, die bei der Ausführung der Befehle auftreten können. Befolgen Sie die angegebenen Anweisungen, um das jeweilige Problem zu beheben. Falls weiterhin Probleme auftreten, eröffnen Sie eine Serviceanfrage im Azure-Portal, und ordnen Sie diese zur Beantwortung durch den Microsoft-Support in die Warteschlange für große SAP HANA-Instanzen ein.

## <a name="log-files"></a>Protokolldateien

Eine der besten Informationsquellen beim Debuggen von Fehlern mit AzAcSnap sind die Protokolldateien.  

### <a name="log-file-location"></a>Speicherort der Protokolldatei

Die Protokolldateien sind einem Verzeichnis gespeichert, das in der Konfigurationsdatei für AzAcSnap mit dem Parameter `logPath` konfiguriert ist.  Der Standardname der Konfigurationsdatei lautet `azacsnap.json`, und der Standardwert für `logPath` ist `"./logs"`. Das bedeutet, dass die Protokolldateien in das `./logs`-Verzeichnis im relativen Pfad zum Ausführungsverzeichnis des Befehls `azacsnap` geschrieben werden.  Indem `logPath` als absoluter Speicherort (z. B. `/home/azacsnap/logs`) festgelegt wird, ist sichergestellt, dass `azacsnap` die Protokolle in `/home/azacsnap/logs` ausgibt, unabhängig davon, wo der Befehl `azacsnap` ausgeführt wurde.

### <a name="log-file-naming"></a>Benennen der Protokolldatei

Der Name der Protokolldatei basiert auf dem Anwendungsnamen (z. B. `azacsnap`), der Befehlsoption (`-c`), die verwendet wurde (z. B. `backup`, `test`, `details` usw.), und dem Namen der Konfigurationsdatei (Standardwert: `azacsnap.json`).  Wenn Sie also den Befehl `-c backup` verwenden, lautet der Name der Protokolldatei standardmäßig `azacsnap-backup-azacsnap.log`, und die Datei wird in das durch `logPath` konfigurierte Verzeichnis geschrieben.  

Diese Namenskonvention wurde eingerichtet, um mehrere Konfigurationsdateien zu ermöglichen (eine pro Datenbank) und das Auffinden der zugehörigen Protokolldateien zu erleichtern.  Daher gilt: Wenn der Name der Konfigurationsdatei `SID.json` lautet und die `azacsnap -c backup --configfile SID.json`-Optionen verwendet werden, ist der Name der Ergebnisdatei `azacsnap-backup-SID.log`.

### <a name="result-file-and-syslog"></a>Ergebnisdatei und Syslog

Bei der Befehlsoption `-c backup` schreibt AzAcSnap in eine `*.result`-Datei und das Systemprotokoll (`/var/log/messages`). Dabei wird der Befehl `logger` verwendet.  Der Dateiname `*.result` weist denselben Basisnamen auf wie die [Protokolldatei](#log-file-naming) und wird im selben [Speicherort gespeichert wie die Protokolldatei](#log-file-location).  Es handelt sich um eine einfache einzeilige Ausgabedatei, wie in den folgenden Beispielen gezeigt.

Beispielausgabe aus der `*.result`-Datei.
```output
Database # 1 (PR1) : completed ok
```

Beispielausgabe aus der `/var/log/messages`-Datei.
```output
Dec 17 09:01:13 azacsnap-rhel azacsnap: Database # 1 (PR1) : completed ok
```

## <a name="failed-communication-with-azure-netapp-files"></a>Fehler bei der Kommunikation mit Azure NetApp Files

Bei der Validierung der Kommunikation mit Azure NetApp Files kann die Kommunikation fehlschlagen oder eine Zeitüberschreitung auftreten.  Stellen Sie sicher, dass die Firewall-Regeln den ausgehenden Datenverkehr vom System, auf dem AzAcSnap läuft, zu den folgenden Adressen und TCP/IP-Ports nicht blockieren:-

- (https://)management.azure.com:443
- (https://)login.microsoftonline.com:443 

## <a name="failed-communication-with-sap-hana"></a>Fehler in der Kommunikation mit SAP HANA

Beim Überprüfen der Kommunikation mit SAP HANA mithilfe eines Tests mit `azacsnap -c test --test hana` tritt der folgende Fehler auf:

```output
> azacsnap -c test --test hana
BEGIN : Test process started for 'hana'
BEGIN : SAP HANA tests
CRITICAL: Command 'test' failed with error:
Cannot get SAP HANA version, exiting with error: 127
```

**Lösung:**

1. Überprüfen Sie die Konfigurationsdatei (z. B. `azacsnap.json`) für jede HANA-Instanz, um sicherzustellen, dass die Werte für die SAP HANA-Datenbanken richtig sind.
1. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Befehl `hdbsql` im Pfad ausgeführt wird und eine Verbindung mit dem SAP HANA-Server herstellen kann. Das folgende Beispiel zeigt die korrekte Ausführung des Befehls und die zugehörige Ausgabe.

    ```bash
    hdbsql -n 172.18.18.50 - i 00 -d SYSTEMDB -U AZACSNAP "\s"
    ```

    ```output
    host          : 172.18.18.50
    sid           : H80
    dbname        : SYSTEMDB
    user          : AZACSNAP
    kernel version: 2.00.040.00.1553674765
    SQLDBC version:        libSQLDBCHDB 2.04.126.1551801496
    autocommit    : ON
    locale        : en_US.UTF-8
    input encoding: UTF8
    sql port      : saphana1:30013
    ```

    In diesem Beispiel ist der Befehl `hdbsql` nicht in der `$PATH`-Variable des Benutzers enthalten.

    ```bash
    hdbsql -n 172.18.18.50 - i 00 -U SCADMIN "select version from sys.m_database"
    ```

    ```output
    If 'hdbsql' is not a typo you can use command-not-found to lookup the package that contains it, like this:
    cnf hdbsql
    ```

    In diesem Beispiel wird der Befehl `hdbsql` der `$PATH`-Variable des Benutzers temporär hinzugefügt, aber bei der Ausführung zeigt sich, dass der Verbindungsschlüssel nicht ordnungsgemäß mit dem Befehl `hdbuserstore Set` eingerichtet wurde (Details finden Sie im Leitfaden für die ersten Schritte):

    ```bash
    export PATH=$PATH:/hana/shared/H80/exe/linuxx86_64/hdb/
    ```

    ```bash
    hdbsql -n 172.18.18.50 -i 00 -U SCADMIN "select version from sys.m_database"
    ```

    ```output
    * -10104: Invalid value for KEY (SCADMIN)
    ```

    > [!NOTE]
    > Um der `$PATH`-Variable des Benutzers Befehle dauerhaft hinzuzufügen, aktualisieren Sie die `$HOME/.profile`-Datei des Benutzers.

## <a name="the-hdbuserstore-location"></a>Der Speicherort `hdbuserstore`

Beim Einrichten der Kommunikation mit SAP HANA werden die Einstellungen für eine sichere Kommunikation über das Programm `hdbuserstore` erstellt.  Das Programm `hdbuserstore` befindet sich in der Regel unter `/usr/sap/<SID>/SYS/exe/hdb/` oder `/usr/sap/hdbclient`.  Normalerweise fügt das Installationsprogramm den richtigen Speicherort zur `azacsnap`-Variable des `$PATH`-Benutzers hinzu.

## <a name="failed-test-with-storage"></a>Fehler beim Test mit Speicher

Der Befehl `azacsnap -c test --test storage` wird nicht erfolgreich ausgeführt.

### <a name="azure-large-instance"></a>Azure (große Instanz)

Das folgende Beispiel stammt aus der Ausführung von `azacsnap` auf SAP HANA auf einer großen Azure-Instanz:

```bash
azacsnap -c test --test storage
```

```output
The authenticity of host '172.18.18.11 (172.18.18.11)' can't be established.
ECDSA key fingerprint is SHA256:QxamHRn3ZKbJAKnEimQpVVCknDSO9uB4c9Qd8komDec.
Are you sure you want to continue connecting (yes/no)?
```

**Lösung:** Der oben genannte Fehler wird normalerweise angezeigt, wenn ein Speicherbenutzer einer großen Azure-Instanz keine Zugriff auf den zugrunde liegenden Speicher hat. Um den Zugriff auf den Speicher für den bereitgestellten Speicherbenutzer zu überprüfen, führen Sie den Befehl `ssh` aus. Damit prüfen Sie die Kommunikation mit der Speicherplattform.

```bash
ssh <StorageBackupname>@<Storage IP address> "volume show -fields volume"
```

Ein Beispiel mit der erwarteten Ausgabe:

```bash
ssh clt1h80backup@10.8.0.16 "volume show -fields volume"
```

```output
vserver volume
--------------------------------- ------------------------------
osa33-hana-c01v250-client25-nprod hana_data_h80_mnt00001_t020_vol
osa33-hana-c01v250-client25-nprod hana_data_h80_mnt00002_t020_vol
```

#### <a name="the-authenticity-of-host-172181811-172181811-cant-be-established"></a>The authenticity of host '172.18.18.11 (172.18.18.11)' can't be established.

```bash
azacsnap -c test --test storage
```

```output
BEGIN : Test process started for 'storage'
BEGIN : Storage test snapshots on 'data' volumes
BEGIN : 1 task(s) to Test Snapshots for Storage Volume Type 'data'
The authenticity of host '10.3.0.18 (10.3.0.18)' can't be established.
ECDSA key fingerprint is SHA256:cONAr0lpafb7gY4l31AdWTzM3s9LnKDtpMdPA+cxT7Y.
Are you sure you want to continue connecting (yes/no)?
```

**Lösung:** Wählen Sie nicht „Ja“ aus. Stellen Sie sicher, dass die IP-Adresse Ihres Speichers korrekt ist. Falls das Problem weiterhin besteht, überprüfen Sie die IP-Adresse des Speichers mit dem Microsoft-Betriebsteam.

### <a name="azure-netapp-files"></a>Azure NetApp Files

Das folgende Beispiel entstammt der Ausführung von `azacsnap` auf einer VM mit Azure NetApp Files:

```bash
azacsnap --configfile azacsnap.json.NOT-WORKING -c test --test storage
```

```output
BEGIN : Test process started for 'storage'
BEGIN : Storage test snapshots on 'data' volumes
BEGIN : 1 task(s) to Test Snapshots for Storage Volume Type 'data'
ERROR: Could not create StorageANF object [authFile = 'azureauth.json']
```

**Lösung:**

1. Überprüfen Sie, ob die Dienstprinzipaldatei `azureauth.json` vorhanden ist, wie in der Konfigurationsdatei `azacsnap.json` festgelegt.
1. Überprüfen Sie in der Protokolldatei (z. B. `logs/azacsnap-test-azacsnap.log`), ob der Dienstprinzipal (`azureauth.json`) den richtigen Inhalt aufweist.  Beispiel aus dem Protokoll:

      ```output
      [19/Nov/2020:18:39:49 +13:00] DEBUG: [PID:0020080:StorageANF:659] [1] Innerexception: Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException AADSTS7000215: Invalid client secret is provided.
      ```

1. Überprüfen Sie in der Protokolldatei (z. B. `logs/azacsnap-test-azacsnap.log`), ob der Dienstprinzipal (`azureauth.json`) abgelaufen ist. Beispiel aus dem Protokoll:

      ```output
      [19/Nov/2020:18:41:10 +13:00] DEBUG: [PID:0020257:StorageANF:659] [1] Innerexception: Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException AADSTS7000222: The provided client secret keys are expired. Visit the Azure Portal to create new keys for your app, or consider using certificate credentials for added security: https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials
      ```

## <a name="next-steps"></a>Nächste Schritte

- [Tipps](azacsnap-tips.md)
