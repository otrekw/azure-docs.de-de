---
title: Funktionen in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwendung von Funktionen beschrieben, um in Azure Monitor aus einer Abfrage eine andere Protokollabfrage aufzurufen.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/31/2020
ms.openlocfilehash: 07a959d4e8ba41652ba4e31ad59cf852659a5926
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103199762"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Verwenden von Funktionen in Azure Monitor-Protokollabfragen

Um eine Protokollabfrage mit einer anderen Abfrage zu verwenden, können Sie diese als Funktion speichern. Dadurch können Sie komplexe Abfragen vereinfachen, indem Sie sie in Einzelteile zerlegen und gemeinsamen Code bei mehreren Abfragen wiederverwenden.

## <a name="create-a-function"></a>Erstellen einer Funktion

Erstellen Sie eine Funktion mit Log Analytics, indem Sie im Azure-Portal auf **Speichern** klicken und dann die Informationen in der folgenden Tabelle angeben.

| Einstellung | BESCHREIBUNG |
|:---|:---|
| Name           | Anzeigename für die Abfrage im **Abfrage-Explorer**. |
| Speichern unter        | Funktion |
| Funktionsalias | Kurzname zur Verwendung der Funktion in anderen Abfragen. Darf keine Leerzeichen enthalten und muss eindeutig sein. |
| Category       | Eine Kategorie zum Organisieren gespeicherter Abfragen und Funktionen im **Abfrage-Explorer**. |

Sie können Funktionen auch mit der [Rest-API](/rest/api/loganalytics/savedsearches/createorupdate) oder mit [PowerShell](/powershell/module/az.operationalinsights/new-azoperationalinsightssavedsearch) erstellen.


## <a name="use-a-function"></a>Verwenden einer Funktion
Sie verwenden eine Funktion, indem Sie den Alias in eine andere Abfrage einbeziehen. Sie kann wie jede andere Tabelle verwendet werden.

## <a name="function-parameters"></a>Funktionsparameter 
Sie können einer Funktion Parameter hinzufügen, sodass Sie beim Aufruf Werte für bestimmte Variablen bereitstellen können. Die einzige Möglichkeit, eine Funktion mit Parametern zu erstellen, stellt die Verwendung einer Resource Manager-Vorlage dar. Ein Beispiel finden Sie unter [Beispiele für Resource Manager-Vorlagen für Protokollabfragen in Azure Monitor](./resource-manager-log-queries.md#parameterized-function).

## <a name="example"></a>Beispiel
Die folgende Beispielabfrage gibt sämtliche fehlenden Sicherheitsupdates zurück, die am Vortag gemeldet wurden. Speichern Sie diese Abfrage als Funktion mit dem Alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Erstellen Sie eine andere Abfrage, und verweisen Sie auf die Funktion _security_updates_last_day_, um nach SQL-bezogenen erforderlichen Sicherheitsupdates zu suchen.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Schreiben von Azure Monitor-Protokollabfragen finden Sie in folgenden weiteren Lektionen:

- [Zeichenfolgenvorgänge](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#string-operations)
- [Datums- und Uhrzeitvorgänge](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#date-and-time-operations)
- [Aggregationsfunktionen](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#aggregations)
- [Erweiterte Aggregationen](/azure/data-explorer/write-queries#advanced-aggregations)
- [JSON und Datenstrukturen](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#json-and-data-structures)
- [Joins](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#joins)
- [Diagramme](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#charts)
