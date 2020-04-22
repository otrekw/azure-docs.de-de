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
ms.openlocfilehash: 940c6d6d96c5c1aa062397d21ea96dace2c09bae
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427398"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Verwenden von SQL-Skripts in Azure Synapse Studio (Vorschauversion)

Azure Synapse Studio (Vorschauversion) stellt eine Webschnittstelle für SQL-Skripts zum Erstellen von SQL-Abfragen zur Verfügung. Sie können eine Verbindung mit dem SQL-Pool (Vorschauversion) oder mit SQL On-Demand (Vorschauversion) herstellen. 

## <a name="begin-authoring-in-sql-script"></a>Beginnen der Erstellung in SQL-Skripts 

Es gibt mehrere Möglichkeiten, die Benutzeroberfläche für die Erstellung in SQL-Skripts zu starten. Sie können mit einer der folgenden Methoden ein neues SQL-Skript erstellen.

1. Wählen Sie das Symbol „+“ und dann „SQL-Skript“ aus.

    > [!div class="mx-imgBorder"] 
    >![newsqlscript](./media/author-sql-script/newsqlscript.png)

2. Wählen Sie im Menü „Aktionen“ unter „SQL-Skripts entwickeln“ die Option „Neues SQL-Skript“ aus. 

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript2actions.png)

oder 

3. Wählen Sie „Importieren“ aus dem Menü „Aktionen“ unter „SQL-Skripts entwickeln“ und dann ein vorhandenes SQL-Skript aus Ihrem lokalen Speicher aus.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>Erstellen Ihres SQL-Skripts

1. Wählen Sie einen Namen für Ihr SQL-Skript aus, indem Sie die Schaltfläche „Eigenschaft“ auswählen und den dem SQL-Skript zugewiesenen Standardnamen ersetzen.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscriptrename.png)

1. Wählen Sie den bestimmten SQL-Pool oder SQL On-Demand aus dem Dropdownmenü „Verbinden mit“ aus. Oder wählen Sie gegebenenfalls die Datenbank unter „Datenbank verwenden“ aus.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlchoosepool.png)

1. Beginnen Sie mit der Erstellung Ihres SQL-Skripts mithilfe des IntelliSense-Features.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>Ausführen Ihres SQL-Skripts

Wählen Sie die Schaltfläche „Ausführen“ aus, um Ihr SQL-Skript auszuführen. Die Ergebnisse werden standardmäßig in einer Tabelle angezeigt.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Exportieren Ihrer Ergebnisse

Sie können die Ergebnisse in verschiedenen Formaten (einschließlich CSV, Excel, JSON, XML) in Ihren lokalen Speicher exportieren, indem Sie „Ergebnisse exportieren“ und dann die Erweiterung auswählen.

Sie können die Ergebnisse des SQL-Skripts auch in einem Diagramm visualisieren, indem Sie die Schaltfläche „Diagramm“ auswählen. Wählen Sie „Diagrammtyp“ und „Kategoriespalte“ aus. Sie können das Diagramm in ein Bild exportieren, indem Sie „Als Bild speichern“ auswählen. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Durchsuchen Sie Daten aus einer Parquet-Datei.

Sie können Parquet-Dateien in einem Speicherkonto mithilfe eines SQL-Skripts untersuchen, um eine Vorschau des Dateiinhalts anzuzeigen. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL-Tabellen, externe Tabellen, Sichten

Durch Auswahl des Menüs „Aktionen“ unter „Daten“ können Sie verschiedene Aktionen wie die Folgenden auswählen: „Neues SQL-Skript“, „Oberste 1000 Zeilen auswählen“, „CREATE“, „DROP and CREATE“. Untersuchen Sie die verfügbare Geste, indem Sie mit der rechten Maustaste auf die Knoten von SQL-Pool und SQL On-Demand klicken.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen eines SQL-Skripts finden Sie unter [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics).