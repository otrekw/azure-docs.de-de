---
title: SQL-Skripts in Azure Synapse Studio (Vorschauversion)
description: Einführung in SQL-Skripts in Azure Synapse Studio (Vorschauversion)
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 9d130c2a2db9ccead7180b6248398a84fcb34c3f
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441237"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Verwenden von SQL-Skripts in Azure Synapse Studio (Vorschauversion)

Azure Synapse Studio (Vorschauversion) stellt eine Webschnittstelle für SQL-Skripts zum Erstellen von SQL-Abfragen zur Verfügung. Sie können eine Verbindung mit dem SQL-Pool (Vorschauversion) oder mit SQL On-Demand (Vorschauversion) herstellen. 

## <a name="begin-authoring-in-sql-script"></a>Beginnen der Erstellung in SQL-Skripts 

Es gibt mehrere Möglichkeiten, die Benutzeroberfläche für die Erstellung in SQL-Skripts zu starten. Sie können mit einer der folgenden Methoden ein neues SQL-Skript erstellen.

1. Klicken Sie im Menü „Entwickeln“ auf das Symbol **+** , und wählen Sie **SQL-Skript** aus.

![Neues SQL-Skript](media/author-sql-script/newsqlscript.png)

2. Klicken Sie im **Aktionsmenü** auf **New SQL script** (Neues SQL-Skript).
> [!div class="mx-imgBorder"]
> ![Neues SQL-Skript: Zwei Aktionen](media/author-sql-script/newsqlscript2actions.png)

Alternativ können Sie 

3. Wählen Sie unter „SQL-Skripts entwickeln“ im Menü **Aktionen** die Option **Importieren** aus. Wählen Sie in Ihrem lokalen Speicher ein vorhandenes SQL-Skript aus.
![Neues SQL-Skript: Drei Aktionen](media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>Erstellen Ihres SQL-Skripts

1. Geben Sie einen Namen für Ihr SQL-Skript an, indem Sie auf die Schaltfläche für die **Eigenschaften** klicken und den dem SQL-Skript zugewiesenen Standardnamen ersetzen. 
![Neues SQL-Skript: Umbenennen](media/author-sql-script/newsqlscriptrename.png)

2. Wählen Sie im Dropdownmenü **Verbinden mit** einen bestimmten SQL-Pool oder SQL On-Demand aus. Sofern erforderlich, können Sie alternativ die gewünschte Datenbank unter **Use database** (Datenbank verwenden) auswählen. 
![Neues SQL-Skript: Pool auswählen](media/author-sql-script/newsqlchoosepool.png)

3. Beginnen Sie mit der Erstellung Ihres SQL-Skripts mithilfe des IntelliSense-Features.
![Neues SQL-Skript: IntelliSense](media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>Ausführen Ihres SQL-Skripts

Klicken Sie auf die Schaltfläche **Ausführen**, um Ihr SQL-Skript auszuführen. Die Ergebnisse werden standardmäßig in einer Tabelle angezeigt.

![Neues SQL-Skript: Tabelle mit Ergebnissen](media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Exportieren Ihrer Ergebnisse

Sie können die Ergebnisse in verschiedenen Formaten (einschließlich CSV, Excel, JSON, XML) in Ihren lokalen Speicher exportieren, indem Sie „Ergebnisse exportieren“ und dann die Erweiterung auswählen.

Sie können die Ergebnisse des SQL-Skripts auch in einem Diagramm visualisieren, indem Sie auf die Schaltfläche **Diagramm** klicken. Wählen Sie unter „Diagrammtyp“ und **Category column** (Kategoriespalte) die gewünschten Werte aus. Sie können das Diagramm als Bild exportieren, indem Sie auf **Save as image** (Als Bild speichern) klicken. 

![Neues SQL-Skript: Diagramm mit Ergebnissen](media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Untersuchen von Daten aus einer Parquet-Datei

Sie können Parquet-Dateien in einem Speicherkonto mithilfe eines SQL-Skripts untersuchen, um eine Vorschau des Dateiinhalts anzuzeigen.

![Neues SQL-Skript: sqlod-Parquet-Datei](media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL-Tabellen, externe Tabellen, Sichten

Durch Klicken auf das **Aktionsmenü** unter „Daten“ können Sie verschiedene Aktionen auswählen, z. B.:

- New SQL script (Neues SQL-Skript)
- Select TOP 1000 rows (Erste 1.000 Zeilen auswählen)
- CREATE
- DROP und CREATE 
 
Untersuchen Sie die verfügbare Geste, indem Sie mit der rechten Maustaste auf die Knoten von SQL-Pool und SQL On-Demand klicken.
 
![Neues SQL-Skript: Datenbank](media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen eines SQL-Skripts finden Sie unter [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).
