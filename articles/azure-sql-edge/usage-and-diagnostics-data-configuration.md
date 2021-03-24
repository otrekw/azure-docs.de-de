---
title: Konfiguration von Nutzungs- und Diagnosedaten in Azure SQL Edge
description: Informieren Sie sich, wie Sie Nutzungs- und Diagnosedaten in Azure SQL Edge konfigurieren.
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 08/04/2020
ms.openlocfilehash: 81b2aabbae148faec06c9ab420bdfecde475b4bb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97604998"
---
# <a name="azure-sql-edge-usage-and-diagnostics-data-configuration"></a>Konfiguration von Nutzungs- und Diagnosedaten in Azure SQL Edge

Azure SQL Edge erfasst standardmäßig Informationen darüber, wie Kunden die Anwendung verwenden. Dies bedeutet, dass Azure SQL Edge Informationen zur Bereitstellung, Nutzung und Leistung sammelt. Mit diesen Informationen kann Microsoft besser an die Bedürfnisse der Kunden anpassen. Microsoft erfasst z. B. Informationen zu Fehlercodes von Kunden, sodass damit verknüpfte Probleme behoben werden können, die Dokumentation zu Azure SQL Edge verbessert und bestimmt werden kann, ob dem Produkt weitere Features hinzugefügt werden müssen, um die Nutzung weiter zu optimieren.

Microsoft sendet nicht die folgenden Informationen mit diesem Mechanismus:

- Werte aus Benutzertabellen
- Anmeldeinformationen oder andere Authentifizierungsinformationen
- persönliche Daten oder Kundendaten

Die folgenden Beispielszenarios beinhalten Informationen zur Nutzung von Funktionen, die zur Verbesserung des Produkts beitragen.

Im Folgenden finden Sie eine Beispielabfrage aus den Abfragen, die für die Nutzungs- und Diagnosedatensammlung verwendet werden. Die Abfrage identifiziert die Anzahl und die Typen verschiedener Streamingdatenquellen, die in Azure SQL Edge verwendet werden. Diese Daten helfen Microsoft dabei, zu ermitteln, welche Streamingdatenquellen häufig verwendet werden, damit Microsoft die Leistung und Nutzung der Datenquellen verbessern kann. 

```sql
select 
count(*) as [count], sum(inputs) as inputs, sum(outputs) as outputs, sum(linked_to_job) 
as linked_to_job, data_source_type
from ( 
select isnull(value,'unknown') as data_source_type, inputs, outputs, linked_to_job
from 
    ( 
        select 
        convert(sysname, lower(substring(ds.location, 0, charindex('://', ds.location))), 1) as data_source_type, 
        isnull(inputs, 0) as inputs, isnull(outputs, 0) as outputs, isnull(js.stream_id/js.stream_id, 0) as linked_to_job 
        from sys.external_streams es
        join sys.external_data_sources ds 
             on es.data_source_id = ds.data_source_id
        left join 
            ( 
            select stream_id, max(cast(is_input as int)) inputs, max(cast(is_output as int)) outputs 
            from sys.external_job_streams group by stream_id 
            ) js
             on js.stream_id = es.object_id 
    ) ds
left join 
    (
        select value from string_split('edgehub,sqlserver,kafka', ',')) as known_ep on data_source_type = value 
    ) known_ds
group by data_source_type
```

## <a name="disable-usage-and-diagnostic-data-collection"></a>Deaktivieren der Nutzungs- und Diagnosedatensammlung

Die Nutzungs- und Diagnosedatensammlung in Azure SQL Edge kann mit einer der folgenden Methoden deaktiviert werden.

> [!NOTE]
> Nutzungs- und Diagnosedaten können für die Entwicklerversion nicht deaktiviert werden.

### <a name="disable-usage-and-diagnostics-using-environment-variables"></a>Deaktivieren der Nutzungs- und Diagnosedatensammlung mithilfe von Umgebungsvariablen

Fügen Sie die folgende Umgebungsvariable hinzu, und legen Sie ihren Wert auf `*False*` fest, um die Nutzungs- und Diagnosedatensammlung in Azure SQL Edge zu deaktivieren. Weitere Informationen zum Konfigurieren von Azure SQL Edge mithilfe von Umgebungsvariablen finden Sie unter [Konfigurieren mithilfe von Umgebungsvariablen](configure.md#configure-by-using-environment-variables).

`MSSQL_TELEMETRY_ENABLED = TRUE | FALSE`

- TRUE: Hiermit wird die Nutzungs- und Diagnosedatensammlung aktiviert. Dies ist die Standardkonfiguration.
- FALSE: Hiermit wird die Nutzungs- und Diagnosedatensammlung deaktiviert.

### <a name="disable-usage-and-diagnostics-using-mssqlconf-file"></a>Deaktivieren der Nutzungs- und Diagnosedatensammlung mithilfe der mssql.conf-Datei

Fügen Sie die folgenden Zeilen in der mssql.conf-Datei auf dem persistenten Speicherlaufwerk hinzu, das dem Ordner „/var/opt/mssql/“ im SQL Edge-Modul zugeordnet ist, um die Nutzungs- und Diagnosedatensammlung in Azure SQL Edge zu deaktivieren. Weitere Informationen zum Konfigurieren von Azure SQL Edge mithilfe der mssql.conf-Datei finden Sie unter [Konfigurieren mithilfe der Datei „mssql.conf“](configure.md#configure-by-using-an-mssqlconf-file).

```ini
[telemetry]
customerfeedback = false
```

## <a name="local-audit-of-usage-and-diagnostic-data-collection"></a>Komponente „Lokale Überwachung“ der Nutzungs- und -Diagnosedatensammlung

Die Komponente „Lokale Überwachung“ der Azure SQL Edge-Nutzungs- und Diagnosedatensammlung kann die vom Dienst erfassten Daten, die die an Microsoft zu sendenden Daten (Protokolle) darstellen, in einen bestimmten Ordner schreiben. Der Zweck der lokalen Überwachung besteht darin, dass es Benutzern gestattet wird, alle Daten hinsichtlich Zustimmung, behördlicher Bestimmungen oder aus Datenschutzgründen anzuzeigen, die Microsoft mithilfe dieses Features erfasst.

### <a name="enable-local-audit-of-usage-and-diagnostics-data"></a>Aktivieren der lokalen Überwachung der Nutzungs- und Diagnosedaten

Aktivieren der lokalen Überwachung der Nutzungs- und Diagnosedaten in Azure SQL Edge

1. Erstellen Sie ein Zielverzeichnis für den neuen Speicher für lokale Überwachungsprotokolle. Dieses Zielverzeichnis kann sich entweder auf dem Host oder innerhalb des Containers befinden. Im folgenden Beispiel wird das Zielverzeichnis in demselben Bereitstellungsvolume erstellt, das dem Pfad „/var/opt/mssql/“ in SQL Edge zugeordnet ist.

   ```bash
   sudo mkdir <host mount path>/audit
   ```

2. Konfigurieren Sie die Überwachung von Nutzungs- und Diagnosedaten mithilfe von Umgebungsvariablen oder der Datei „mssql.conf“.

   - Verwenden von Umgebungsvariablen: Fügen Sie die folgende Umgebungsvariable zu Ihrer SQL Edge-Bereitstellung hinzu, und geben Sie das Zielverzeichnis für die Überwachungsdateien an.
   
     `*MSSQL_TELEMETRY_DIR = <host mount path>/audit*`
   
   - Verwenden der Datei „mssql.conf“: Fügen Sie die folgenden Zeilen in der Datei „mssql.conf“ ein, und geben Sie das Zielverzeichnis für die Überwachungsdateien an.
       ```ini
       [telemetry]
       userrequestedlocalauditdirectory  = <host mount path>/audit
       ```  

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung mit Azure SQL Edge](connect.md)
- [Erstellen einer End-to-End-IoT-Lösung mit SQL Edge](tutorial-deploy-azure-resources.md)
