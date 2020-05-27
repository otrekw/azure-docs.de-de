---
title: Funktionen in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwendung von Funktionen beschrieben, um in Azure Monitor aus einer Abfrage eine andere Protokollabfrage aufzurufen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/18/2018
ms.openlocfilehash: 5fb9e48a6d6a0b95b61478a7877e9b46dd8963e9
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649396"
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




## <a name="use-a-function"></a>Verwenden einer Funktion
Sie verwenden eine Funktion, indem Sie den Alias in eine andere Abfrage einbeziehen. Sie kann wie jede andere Tabelle verwendet werden.

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

- [Zeichenfolgenvorgänge](string-operations.md)
- [Datums- und Uhrzeitvorgänge](datetime-operations.md)
- [Aggregationsfunktionen](aggregations.md)
- [Erweiterte Aggregationen](advanced-aggregations.md)
- [JSON und Datenstrukturen](json-data-structures.md)
- [Joins](joins.md)
- [Diagramme](charts.md)
