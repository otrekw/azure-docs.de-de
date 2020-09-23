---
title: Problembehandlung von Azure SQL Edge-Bereitstellungen
description: Erfahren Sie mehr über mögliche Fehler, wenn Azure SQL Edge bereitgestellt wird.
keywords: SQL Edge, Problembehandlung, Bereitstellungsfehler
services: sql-edge
ms.service: sql-edge
ms.topic: troubleshooting
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: d8da8bcf3d2bb6b2af2b5c69ce003289d83d3884
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931408"
---
# <a name="troubleshooting-azure-sql-edge-deployments"></a>Problembehandlung von Azure SQL Edge-Bereitstellungen 

Dieser Artikel enthält Informationen zu Fehlern, die auftreten können, wenn Azure SQL Edge-Container bereitgestellt und verwendet werden, und beschreibt Problembehandlungstechniken, mit denen diese Probleme behoben werden können. 

Azure SQL Edge unterstützt zwei Bereitstellungsmodelle: 
- Verbundene Bereitstellung über Azure IoT Edge: Azure SQL Edge ist in Azure Marketplace verfügbar und kann als Modul für [Azure IoT Edge](../iot-edge/about-iot-edge.md) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Azure SQL Edge](deploy-portal.md).<br>

- Getrennte Bereitstellung: Azure SQL Edge-Containerimages können aus Docker Hub abgerufen und entweder als eigenständiger Docker-Container oder in einem Kubernetes-Cluster bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Azure SQL Edge mit Docker](disconnected-deployment.md) und [Bereitstellen eines Azure SQL Edge-Containers in Kubernetes](deploy-kubernetes.md).

## <a name="troubleshooting-iot-edge-device-and-deployments"></a>Problembehandlung von IoT Edge-Geräten und -Bereitstellungen

Tritt beim Bereitstellen von SQL Edge über Azure IoT Edge ein Fehler auf, vergewissern Sie sich, dass der `iotedge`-Dienst ordnungsgemäß konfiguriert ist und ausgeführt wird. Die folgenden Dokumente können bei der Behandlung von Problemen im Zusammenhang mit Azure IoT Edge hilfreich sein:
- [Häufig auftretende Probleme und Lösungen für Azure IoT Edge](../iot-edge/troubleshoot-common-errors.md)
- [Behandeln von Problemen bei Ihrem IoT Edge-Gerät](../iot-edge/troubleshoot.md)

## <a name="docker-command-errors"></a>Fehler bei Docker-Befehlen

Wenn Fehler bei `docker`-Befehlen auftreten, sollten Sie sicherstellen, dass der Docker-Dienst ausgeführt wird und versuchen, diesen mit erhöhten Berechtigungen auszuführen.

Unter Linux kann beispielsweise folgender Fehler auftreten, wenn Sie `docker`-Befehle ausführen:

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Wenn dieser Fehler unter Linux auftritt, versuchen Sie, diese Befehle mit `sudo` auszuführen. Wenn dieser Versuch fehlschlägt, sollten Sie überprüfen, ob der Docker-Dienst ausgeführt wird und diesen ggf. starten.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

Vergewissern Sie sich unter Windows, dass Sie PowerShell oder die Eingabeaufforderung als Administrator starten.

## <a name="azure-sql-edge-container-startup-errors"></a>Startfehler bei Azure SQL Edge-Containern

Wenn der SQL Edge-Container nicht ausgeführt werden kann, gehen Sie wie folgt vor:

- Wenn Sie Azure IoT Edge verwenden, stellen Sie sicher, dass die Modulimages erfolgreich heruntergeladen wurden und dass die Umgebungsvariablen und Containererstellungsoptionen im Modulmanifest korrekt angegeben sind.

- Wenn Sie eine Docker-oder Kubernetes-basierte Bereitstellung verwenden, vergewissern Sie sich, dass der `docker run`-Befehl korrekt formatiert ist. Weitere Informationen finden Sie unter [Bereitstellen von Azure SQL Edge mit Docker](disconnected-deployment.md) und [Bereitstellen eines Azure SQL Edge-Containers in Kubernetes](deploy-kubernetes.md).

- Wenn Sie einen Fehler wie `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`erhalten, versuchen Sie, den Containerport 1433 einem bereits verwendeten Port zuzuordnen. Dieser Fall kann eintreten, wenn Sie SQL Edge lokal auf dem Hostcomputer ausführen. Dieser Fall kann außerdem eintreten, wenn Sie zwei SQL Edge-Container starten und versuchen, diese demselben Hostport zuzuordnen. Verwenden Sie in diesem Fall den Parameter `-p`, um den Containerport 1433 einem anderen Hostport zuzuordnen. Beispiel: 

    ```bash
    sudo docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 --name azuresqledge -d mcr.microsoft.com/azure-sql-edge-developer.
    ```

- Wenn Sie beim Versuch, einen Container zu starten, einen Fehler wie `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` erhalten, fügen Sie den Benutzer der Docker-Gruppe in Ubuntu hinzu. Melden Sie sich ab und wieder an, da sich diese Änderung auf neue Sitzungen auswirkt. 

   ```bash
    usermod -aG docker $USER
   ```

- Überprüfen Sie, ob Fehlermeldungen in Bezug auf den Container vorhanden sind.

   ```bash
   docker logs e69e056c702d
   ```

- Wenn Sie Containerverwaltungssoftware verwenden, müssen Sie sicherstellen, dass diese die Ausführung von Containerprozessen als Root unterstützt. Der Prozess „sqlservr“ im Container wird als Root ausgeführt.

- Azure SQL Edge-Container werden standardmäßig als Nicht-root-Benutzer namens `mssql` ausgeführt. Wenn Sie Bereitstellungspunkte oder Datenvolumes verwenden, um Daten zu speichern, stellen Sie sicher, dass der Benutzer `mssql` geeignete Berechtigungen für das Volume hat. Weitere Informationen finden Sie unter [Ausführen als Nicht-Root-Benutzer](configure.md#run-azure-sql-edge-as-non-root-user) und [Persistentes Speichern von Daten](configure.md#persist-your-data).

- Wenn Ihr SQL Edge-Docker-Container sofort nach dem Start beendet wird, überprüfen Sie die Docker-Protokolle. Wenn Sie PowerShell unter Windows mit dem Befehl `docker run` verwenden, verwenden Sie doppelte Anführungszeichen anstelle von einfachen Anführungszeichen. Verwenden Sie mit PowerShell Core einfache Anführungszeichen.

- Überprüfen Sie die [SQL Edge-Fehlerprotokolle](#errorlogs).

## <a name="sql-edge-connection-failures"></a>SQL Edge-Verbindungsfehler

Wenn Sie keine Verbindung mit der SQL Edge-Instanz herstellen können, die in Ihrem Container ausgeführt wird, versuchen Sie Folgendes:

- Stellen Sie sicher, dass Ihr SQL Edge-Container ausgeführt wird, indem Sie die **STATUS**-Spalte der Ausgabe von `docker ps -a` überprüfen. Starten Sie ihn andernfalls mit `docker start <Container ID>`.

- Wenn Sie den Container nicht dem Standardhostport 1433 zugeordnet haben, müssen Sie den Port in der Verbindungszeichenfolge angeben. Die Portzuordnung ist in der Spalte **PORTS** der Ausgabe von `docker ps -a` ersichtlich. Weitere Informationen über das Herstellen einer Verbindung mit Azure SQL Edge finden Sie unter [Herstellen einer Verbindung mit und Abfragen von Azure SQL Edge](connect.md).

- Wenn Sie SQL Edge zuvor mit einem zugeordneten Datenvolume oder Datenvolumecontainer bereitgestellt haben und nun das vorhandene zugeordnete Datenvolume oder den vorhandenen zugeordneten Datenvolumecontainer verwenden, ignoriert SQL Edge den Wert der Umgebungsvariablen `MSSQL_SA_PASSWORD`. Stattdessen wird das zuvor konfigurierte SA-Benutzerkennwort verwendet. Dies passiert, weil SQL Edge die vorhandenen master-Datenbankdateien im zugeordneten Volume oder Datenvolumecontainer wiederverwendet. Wenn dieses Problem auftritt, können Sie die folgenden Optionen verwenden:

    - Herstellen einer Verbindung mit dem zuvor verwendeten Kennwort, wenn es noch verfügbar ist
    - Konfigurieren von SQL Edge, sodass ein anderes zugeordnetes Volume oder ein anderer zugeordneter Datenvolumecontainer verwendet wird
    - Entfernen der vorhandenen master-Datenbankdateien („master.mdf“ und „mastlog.mdf“) aus dem zugeordneten Volume oder Datenvolumecontainer

- Überprüfen Sie die [SQL Edge-Fehlerprotokolle](#errorlogs).

## <a name="sql-edge-setup-and-error-logs"></a><a id="errorlogs"></a> Setup- und Fehlerprotokolle für SQL Edge

Standardmäßig befinden sich SQL Edge-Protokolle im **/var/opt/mssql/log**-Verzeichnis in dem Container, und es kann auf eine der folgenden Arten auf sie zugegriffen werden:

- Wenn Sie bei der Erstellung des Containers ein Hostverzeichnis in **/var/opt/mssql** eingebunden haben, können Sie stattdessen im Unterverzeichnis **log** des auf dem Host zugeordneten Pfads suchen.
- Durch Verwenden einer interaktiven Eingabeaufforderung, um eine Verbindung mit dem Container herzustellen. Wenn der Container nicht ausgeführt wird, starten Sie diesen zunächst. Verwenden Sie dann eine interaktive Eingabeaufforderung, um die Protokolle zu überprüfen. Die Container-ID können Sie durch Ausführen des Befehls `docker ps` abrufen.

    ```bash
    docker start <ContainerID>
    docker exec -it <ContainerID> "/bin/bash"
    ```

    Führen Sie folgende Befehle über die Bash-Sitzung in Ihrem Container aus:

    ```bash
    cd /var/opt/mssql/log
    cat errorlog
    ```
- Wenn der SQL Edge-Container aktiv ist und ausgeführt wird und Sie über Clienttools eine Verbindung mit der Instanz herstellen können, können Sie die gespeicherte Prozedur `sp_readerrorlog` verwenden, um den Inhalt des SQL Edge-Fehlerprotokolls zu lesen.

## <a name="execute-commands-in-a-container"></a>Ausführen von Befehlen in einem Container

Wenn Sie bereits einen Container ausführen, können Sie über ein Hostterminal Befehle innerhalb des Containers ausführen.

So rufen Sie die Container-ID ab:

```bash
docker ps -a
```

So starten Sie ein Bash-Terminal in der Containerausführung:

```bash
docker exec -it <Container ID> /bin/bash
```

Nun können Sie Befehle wie über das Terminal innerhalb des Containers ausführen. Wenn Sie fertig sind, geben Sie `exit` ein. Dadurch wird die interaktive Befehlssitzung beendet, aber der Container wird weiter ausgeführt.

## <a name="troubleshooting-issues-with-data-streaming"></a>Behandeln von Problemen mit Datenstreaming

Standardmäßig werden die Protokolle der Azure SQL Edge-Streaming-Engine in eine Datei namens `current` geschrieben, die sich im Verzeichnis **/var/opt/MSSQL/Log/Services/00000001-0000-0000-0000-000000000000** befindet. Auf die Datei kann entweder direkt über das zugeordnete Volume oder den zugeordneten Datenvolumecontainer oder dadurch zugegriffen werden, dass eine interaktive Eingabeaufforderungssitzung für den SQL Edge-Container gestartet wird. 

Außerdem können Sie, wenn Sie über die Clienttools eine Verbindung mit der SQL Edge-Instanz herstellen können, den folgenden T-SQL-Befehl verwenden, um auf das aktuelle Protokoll der Streaming-Engine zuzugreifen. 

```sql

select value as log, try_convert(DATETIME2, substring(value, 0, 26)) as timestamp 
from 
    STRING_SPLIT
    (
        (
            select BulkColumn as logs
            FROM OPENROWSET (BULK '/var/opt/mssql/log/services/00000001-0000-0000-0000-000000000000/current', SINGLE_CLOB) MyFile
        ),
        CHAR(10)
    ) 
where datalength(value) > 0

```

### <a name="enabling-verbose-logging"></a>Aktivieren der ausführlichen Protokollierung

Wenn die Standardprotokollebene für die Streaming-Engine nicht genügend Informationen bereitstellt, kann die Debugprotokollierung für die Streaming-Engine in SQL Edge aktiviert werden. Um die Debugprotokollierung zu aktivieren, fügen Sie Ihrer SQL Edge-Bereitstellung die Umgebungsvariable `RuntimeLogLevel=debug` hinzu. Nach dem Aktivieren der Debugprotokollierung sollten Sie versuchen, das Problem zu reproduzieren, und überprüfen Sie die Protokolle hinsichtlich relevanter Meldungen oder Ausnahmen. 



## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning und künstliche Intelligenz mit ONNX in SQL Edge](onnx-overview.md)
- [Datenstreaming in Azure SQL Edge](stream-data.md)
- [Datenaufbewahrung und -bereinigung](data-retention-overview.md)
- [Auffüllen von Zeitlücken und Ergänzen von fehlenden Werten](imputing-missing-values.md)







