---
title: Verwaltete Instanz mit Azure Arc-Unterstützung und Hochverfügbarkeit
titleSuffix: Deploy Azure Arc enabled Managed Instance with high availability
description: Hier erfahren Sie, wie Sie eine verwaltete Instanz mit Azure Arc-Unterstützung und Hochverfügbarkeit bereitstellen.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 03/02/2021
ms.topic: conceptual
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.openlocfilehash: 92f5c900238fc5d40e22870e2f00f8adeb5d335f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102032193"
---
# <a name="azure-arc-enabled-managed-instance-high-availability"></a>Verwaltete Instanz mit Azure Arc-Unterstützung und Hochverfügbarkeit

Eine verwaltete Instanz mit Azure Arc-Unterstützung wird in Kubernetes als Containeranwendung bereitgestellt und verwendet Kubernetes-Konstrukte wie StatefulSet-Objekte und persistenten Speicher, um integrierte Integritätsüberwachung, Fehlererkennung und Failovermechanismen zur Aufrechterhaltung der Dienstintegrität bereitzustellen. Zur Verbesserung der Zuverlässigkeit können Sie auch eine verwaltete Instanz mit Azure Arc-Unterstützung für die Bereitstellung mit zusätzlichen Replikaten in einer Hochverfügbarkeitskonfiguration konfigurieren. Überwachung, Fehlererkennung und automatisches Failover werden durch den Datencontroller der Arc-Datendienste verwaltet. Dieser Dienst wird ohne Benutzereingriff bereitgestellt – vom Einrichten der Verfügbarkeitsgruppe, Konfigurieren von Endpunkten für die Datenbankspiegelung, Hinzufügen von Datenbanken zur Verfügbarkeitsgruppe bis hin zur Failover- und Upgradekoordination. In diesem Dokument werden beide Arten von Hochverfügbarkeit betrachtet.

## <a name="built-in-high-availability"></a>Integrierte Hochverfügbarkeit 

Integrierte Hochverfügbarkeit wird von Kubernetes bereitgestellt, wenn persistenter Remotespeicher konfiguriert und gemeinsam von Knoten genutzt wird, die von der Arc-Datendienstbereitstellung verwendet werden. In dieser Konfiguration spielt Kubernetes die Rolle des Clusterorchestrators. Sollte die verwaltete Instanz in einem Container oder der zugrunde liegende Knoten ausfallen, wird durch den Orchestrator eine neue Instanz des Containers gestartet und an den gleichen persistenten Speicher angefügt. Dieser Typ ist standardmäßig aktiviert, wenn Sie eine verwaltete Instanz mit Azure Arc-Unterstützung bereitstellen.

### <a name="verify-built-in-high-availability"></a>Überprüfen der integrierten Hochverfügbarkeit

In diesem Abschnitt wird die von Kubernetes bereitgestellte integrierte Hochverfügbarkeit überprüft. Um diese Funktion zu testen, löschen Sie den Pod einer vorhandenen verwalteten Instanz und überprüfen, ob Kubernetes diese Situation bewältigen kann. 

### <a name="prerequisites"></a>Voraussetzungen

- Kubernetes-Cluster mit [freigegebenem Remotespeicher](storage-configuration.md#factors-to-consider-when-choosing-your-storage-configuration) 
- Eine verwaltete Instanz mit Azure Arc-Unterstützung, bereitgestellt mit einem einzelnen Replikat (Standard)

1. Zeigen Sie die Pods an. 

   ```console
   kubectl get pods -n <namespace of data controller>
   ```

2. Löschen Sie den Pod der verwalteten Instanz.

   ```console
   kubectl delete pod <name of managed instance>-0 -n <namespace of data controller>
   ```

   Beispiel:

   ```output
   user@pc:/# kubectl delete pod sql1-0 -n arc
   pod "sql1-0" deleted
   ```

3. Zeigen Sie die Pods an, um sich zu vergewissern, dass die verwaltete Instanz wiederhergestellt wird.

   ```console
   kubectl get pods -n <namespace of data controller>
   ```

   Beispiel:

   ```output
   user@pc:/# kubectl get pods -n arc
   NAME                 READY   STATUS    RESTARTS   AGE
   sql1-0               2/3     Running   0          22s
   ```

Nachdem alle Container im Pod wiederhergestellt wurden, können Sie eine Verbindung mit der verwalteten Instanz herstellen.

## <a name="deploy-with-always-on-availability-groups"></a>Bereitstellen mit Always On-Verfügbarkeitsgruppen

Zur Verbesserung der Zuverlässigkeit können Sie eine verwaltete Instanz mit Azure Arc-Unterstützung für die Bereitstellung mit zusätzlichen Replikaten in einer Hochverfügbarkeitskonfiguration konfigurieren. 

Verfügbarkeitsgruppen ermöglichen Folgendes:

- Bei der Bereitstellung mit mehreren Replikaten wird eine einzelne Verfügbarkeitsgruppe namens `containedag` erstellt. Standardmäßig gibt es für `containedag` drei Replikate, einschließlich des primären Replikats. Alle CRUD-Vorgänge für die Verfügbarkeitsgruppe werden intern verwaltet – auch die Erstellung der Verfügbarkeitsgruppe oder die Verknüpfung von Replikaten mit der erstellten Verfügbarkeitsgruppe. In der verwalteten Instanz mit Azure Arc-Unterstützung können keine zusätzlichen Verfügbarkeitsgruppen erstellt werden.

- Alle Datenbanken werden automatisch zur Verfügbarkeitsgruppe hinzugefügt, einschließlich aller Benutzer- und Systemdatenbanken wie `master` und `msdb`. Diese Funktion bietet eine Einzelsystemansicht zu den Replikaten der Verfügbarkeitsgruppen. Beachten die beiden Datenbanken `containedag_master` und `containedag_msdb`, wenn Sie eine direkte Verbindung mit der Instanz herstellen. Die `containedag_*`-Datenbanken repräsentieren die Elemente `master` und `msdb` innerhalb der Verfügbarkeitsgruppe.

- Ein externer Endpunkt wird automatisch für die Verbindung mit Datenbanken innerhalb der Verfügbarkeitsgruppe bereitgestellt. Der Endpunkt `<managed_instance_name>-svc-external` übernimmt die Rolle des Listeners der Verfügbarkeitsgruppe.

### <a name="deploy"></a>Bereitstellen

Führen Sie den folgenden Befehl aus, um eine verwaltete Instanz mit Verfügbarkeitsgruppen bereitzustellen:

```console
azdata arc sql mi create -n <name of instance> --replicas 3
```

### <a name="check-status"></a>Status überprüfen
Führen Sie nach der Bereitstellung der Instanz die folgenden Befehle aus, um den Status Ihrer Instanz zu überprüfen:

```console
azdata arc sql mi list
azdata arc sql mi show -n <name of instance>
```

Beispielausgabe:

```output
user@pc:/# azdata arc sql mi list
ExternalEndpoint    Name    Replicas    State
------------------  ------  ----------  -------
20.131.31.58,1433   sql2    3/3         Ready

user@pc:/#  azdata arc sql mi show -n sql2
{
...
  "status": {
    "AGStatus": "Healthy",
    "externalEndpoint": "20.131.31.58,1433",
    "logSearchDashboard": "link to logs dashboard",
    "metricsDashboard": "link to metrics dashboard",
    "readyReplicas": "3/3",
    "state": "Ready"
  }
}
```

Beachten Sie die zusätzliche Anzahl von Replikaten (`Replicas`) sowie das Feld `AGstatus`, in dem die Integrität der Verfügbarkeitsgruppe angegeben ist. Wenn alle Replikate aktiv und synchronisiert sind, lautet der Wert `healthy`. 

### <a name="restore-a-database"></a>Wiederherstellen einer Datenbank 
Für die Wiederherstellung einer Datenbank in einer Verfügbarkeitsgruppe müssen zusätzliche Schritte ausgeführt werden. Die folgenden Schritte zeigen, wie Sie eine Datenbank in einer verwalteten Instanz wiederherstellen und einer Verfügbarkeitsgruppe hinzufügen. 

1. Machen Sie den externen Endpunkt der primären Instanz verfügbar, indem Sie einen neuen Kubernetes-Dienst erstellen.

    Bestimmen Sie den Pod, der das primäre Replikat hostet, indem Sie eine Verbindung mit der verwalteten Instanz herstellen und Folgendes ausführen:

    ```sql
    SELECT @@SERVERNAME
    ```
    Erstellen Sie den Kubernetes-Dienst für die primäre Instanz durch Ausführen des folgenden Befehls, falls von Ihrem Kubernetes-Cluster nodePort-Dienste verwendet werden. Ersetzen Sie `podName` durch den Namen des im vorherigen Schritt zurückgegebenen Servers und `serviceName` durch den bevorzugten Namen für den erstellten Kubernetes-Dienst.

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=NodePort
    ```

    Führen Sie für einen Lastenausgleichsdienst den gleichen Befehl aus. In diesem Fall wird jedoch ein Dienst vom Typ `LoadBalancer` erstellt. Beispiel: 

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=LoadBalancer
    ```

    Im Anschluss finden Sie ein Beispiel für diesen Befehl, der für Azure Kubernetes Service ausgeführt wird, wobei `sql2-0` der Pod ist, der das primäre Replikat hostet:

    ```bash
    kubectl -n arc-cluster expose pod sql2-0 --port=1533  --name=sql2-0-p --type=LoadBalancer
    ```

    Rufen Sie die IP-Adresse des erstellten Kubernetes-Diensts ab:

    ```bash
    kubectl get services -n <namespaceName>
    ```
2. Stellen Sie die Datenbank am Endpunkt der primären Instanz wieder her.

    Fügen Sie die Datenbanksicherungsdatei dem Container der primären Instanz hinzu.

    ```console
    kubectl cp <source file location> <pod name>:var/opt/mssql/data/<file name> -n <namespace name>
    ```

    Beispiel

    ```console
    kubectl cp /home/WideWorldImporters-Full.bak sql2-1:var/opt/mssql/data/WideWorldImporters-Full.bak -c arc-sqlmi -n arc
    ```

    Führen Sie den folgenden Befehl aus, um die Datenbanksicherungsdatei wiederherzustellen:

    ```sql 
    RESTORE DATABASE test FROM DISK = '/var/opt/mssql/data/<file name>.bak'
    WITH MOVE '<database name>' to '/var/opt/mssql/data/<file name>.mdf'  
    ,MOVE '<database name>' to '/var/opt/mssql/data/<file name>_log.ldf'  
    ,RECOVERY, REPLACE, STATS = 5;  
    GO
    ```
    
    Beispiel

    ```sql
    RESTORE Database WideWorldImporters
    FROM DISK = '/var/opt/mssql/data/WideWorldImporters-Full.BAK'
    WITH
    MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf',
    MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_UserData.ndf',
    MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf',
    MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1',
    RECOVERY, REPLACE, STATS = 5;  
    GO
    ```

3. Fügen Sie die Datenbank der Verfügbarkeitsgruppe hinzu.

    Damit die Datenbank der Verfügbarkeitsgruppe hinzugefügt werden kann, muss sie im vollständigen Wiederherstellungsmodus ausgeführt werden, und es muss eine Protokollsicherung durchgeführt werden. Führen Sie die folgenden TSQL-Anweisungen aus, um die wiederhergestellte Datenbank der Verfügbarkeitsgruppe hinzuzufügen:

    ```sql
    ALTER DATABASE <databaseName> SET RECOVERY FULL;
    BACKUP DATABASE <databaseName> TO DISK='<filePath>'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE <databaseName>
    ```

    Im folgenden Beispiel wird die Datenbank `WideWorldImporters` hinzugefügt, die auf der Instanz wiederhergestellt wurde:

    ```sql
    ALTER DATABASE WideWorldImporters SET RECOVERY FULL;
    BACKUP DATABASE WideWorldImporters TO DISK='/var/opt/mssql/data/WideWorldImporters.bak'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE WideWorldImporters
    ```

> [!IMPORTANT]
> Als bewährte Methode sollten Sie den oben erstellten Kubernetes-Dienst löschen, indem Sie diesen Befehl ausführen:
>
>```bash
>kubectl delete svc sql2-0-p -n arc
>```

### <a name="limitations"></a>Einschränkungen

Für Verfügbarkeitsgruppen einer verwalteten Instanz mit Azure Arc-Unterstützung gelten die gleichen [Einschränkungen wie für Verfügbarkeitsgruppen eines Big Data-Clusters. Klicken Sie hier, um weitere Informationen zu erhalten.](/sql/big-data-cluster/deployment-high-availability#known-limitations)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Features und Funktionen von Azure Arc-aktivierten SQL Managed Instance-Instanzen](managed-instance-features.md)
