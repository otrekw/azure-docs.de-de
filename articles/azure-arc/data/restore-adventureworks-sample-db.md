---
title: Wiederherstellen der AdventureWorks-Beispieldatenbank in SQL Managed Instance
description: Wiederherstellen der AdventureWorks-Beispieldatenbank in SQL Managed Instance
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a6b3577fe820c9dadcb3c8415aa7a951a3283284
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105641802"
---
# <a name="restore-the-adventureworks-sample-database-into-sql-managed-instance---azure-arc"></a>Wiederherstellen der AdventureWorks-Beispieldatenbank in SQL Managed Instance: Azure Arc

[AdventureWorks](/sql/samples/adventureworks-install-configure) ist eine Beispieldatenbank, die eine OLTP-Datenbank enthält, die häufig in Tutorials und Beispielen verwendet wird. Sie wird von Microsoft als Teil des [GitHub-Repositorys für SQL Server-Beispiele](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases) bereitgestellt und verwaltet.

In diesem Dokument wird ein einfacher Prozess beschrieben, mit dem die AdventureWorks-Beispieldatenbank in Ihrer SQL Managed Instance-Instanz (Azure Arc) wiederhergestellt werden kann.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="download-the-adventureworks-backup-file"></a>Herunterladen der AdventureWorks-Sicherungsdatei

Laden Sie die AdventureWorks-Sicherungsdatei (BAK-Datei) in Ihren SQL Managed Instance-Container herunter. Verwenden Sie in diesem Beispiel den Befehl `kubectl exec`, um einen Befehl remote innerhalb des SQL Managed Instance-Containers auszuführen und die BAK-Datei in den Container herunterzuladen. Laden Sie diese Datei von einem beliebigen Speicherort herunter, auf den `wget` zugreifen kann, wenn Sie über andere Datenbanksicherungsdateien verfügen, die Sie pullen möchten, damit sie sich im SQL Managed Instance-Container befinden. Wenn sich die Datei innerhalb des SQL Managed Instance-Containers befindet, ist die Wiederherstellung mithilfe des T-SQL-Standardbefehls `RESTORE DATABASE` ganz einfach.

Führen Sie einen Befehl wie diesen aus, um die BAK-Datei herunterzuladen, und ersetzen Sie den Wert des Podnamens und des Namespace, bevor Sie sie ausführen.
> [!NOTE]
>  Ihr Container muss über eine Internetverbindung über Port 443 verfügen, um die Datei von GitHub herunterladen zu können.

```console
kubectl exec <SQL pod name> -n <namespace name> -c arc-sqlmi -- wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak -O /var/opt/mssql/data/AdventureWorks2019.bak
```

Beispiel

```console
kubectl exec sqltest1-0 -n arc -c arc-sqlmi -- wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak -O /var/opt/mssql/data/AdventureWorks2019.bak
```

## <a name="restore-the-adventureworks-database"></a>Wiederherstellen der AdventureWorks-Datenbank

Auf ähnliche Weise können Sie einen `kubectl` exec-Befehl ausführen, um das CLI-Tool `sqlcmd` zu verwenden, das im SQL Managed Instance-Container enthalten ist, um den T-SQL-Befehl RESTORE DATABASE zum Wiederherstellen der Datenbank auszuführen.

Führen Sie einen Befehl wie diesen aus, um die Datenbank wiederherzustellen. Ersetzen Sie den Wert des Podnamens, das Kennwort und den Namespacenamen, bevor Sie den Befehl ausführen.

```console
kubectl exec <SQL pod name> -n <namespace name> -c arc-sqlmi -- /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <password> -Q "RESTORE DATABASE AdventureWorks2019 FROM  DISK = N'/var/opt/mssql/data/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf', MOVE 'AdventureWorks2017_Log' TO '/var/opt/mssql/data/AdventureWorks2019_Log.ldf'"
```
Beispiel

```console
kubectl exec sqltest1-0 -n arc -- -c arc-sqlmi /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P MyPassword! -Q "RESTORE DATABASE AdventureWorks2019 FROM  DISK = N'/var/opt/mssql/data/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf', MOVE 'AdventureWorks2017_Log' TO '/var/opt/mssql/data/AdventureWorks2019_Log.ldf'"
```
