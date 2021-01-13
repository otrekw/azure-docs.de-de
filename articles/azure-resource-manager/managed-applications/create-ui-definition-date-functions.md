---
title: Erstellen von Benutzeroberflächendefinitions-Datumsfunktionen
description: Beschreibt die Funktionen, die beim Arbeiten mit Datumswerten verwendet werden können.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: 80484fd15e51bae7036c43bd3ca8fe8167387ede
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87094444"
---
# <a name="createuidefinition-date-functions"></a>CreateUiDefinition-Datumsfunktionen

Die Funktionen, die beim Arbeiten mit Datumswerten verwendet werden können.

## <a name="addhours"></a>addHours

Fügt dem angegebenen Zeitstempel eine ganzzahlige Anzahl von Stunden hinzu.

Das folgende Beispiel gibt `"1991-01-01T00:59:59.000Z"`zurück:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="addminutes"></a>addMinutes

Fügt dem angegebenen Zeitstempel eine ganzzahlige Anzahl von Minuten hinzu.

Das folgende Beispiel gibt `"1991-01-01T00:00:59.000Z"`zurück:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

## <a name="addseconds"></a>addSeconds
Fügt dem angegebenen Zeitstempel eine ganzzahlige Anzahl von Sekunden hinzu.

Das folgende Beispiel gibt `"1991-01-01T00:00:00.000Z"`zurück:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

## <a name="utcnow"></a>utcNow

Gibt eine Zeichenfolge im ISO 8601-Format des aktuellen Datums und der aktuellen Uhrzeit auf dem lokalen Computer zurück.

Im folgenden Beispiel könnte `"1990-12-31T23:59:59.000Z"` zurückgegeben werden:

```json
"[utcNow()]"
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](../management/overview.md).
