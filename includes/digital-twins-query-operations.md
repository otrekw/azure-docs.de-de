---
author: baanders
description: Includedatei für Azure Digital Twins-Abfragevorgänge
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 333a7ec4ae0e5c8cbc94a603e2ccf81ee92e7d48
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078458"
---
## <a name="query-language-features"></a>Funktionen der Abfragesprache

Azure Digital Twins bietet umfassende Abfragefunktionen für den Zwillingsgraphen. Abfragen werden mithilfe einer SQL-ähnlichen Syntax in einer Abfragesprache beschrieben, die der [IoT Hub-Abfragesprache](../articles/iot-hub/iot-hub-devguide-query-language.md) mit vielen vergleichbaren Features ähnelt.

> [!NOTE]
> Bei allen Azure Digital Twins-Abfragevorgängen muss die Groß-/Kleinschreibung beachtet werden.

Diese Vorgänge stehen in der Azure Digital Twins-Abfragesprache zur Verfügung:

Abrufen von digitalen Zwillingen nach...
* ihrem Modell (mithilfe des Operators `IS_OF_MODEL`)
* ihren Eigenschaften (einschließlich [Tageigenschaften](../articles/digital-twins/how-to-use-tags.md))
* Schnittstellen
* relationships
  - Eigenschaften der Beziehungen

Sie können Ihre Abfragen mit den folgenden Vorgängen weiter verbessern:
* Sie können Zwillinge über mehrere Beziehungstypen (`JOIN`-Abfragen) abrufen. 
  - Während der Vorschauphase sind bis zu fünf Ebenen von `JOIN` zulässig.
* Wählen Sie nur die obersten Abfrageergebnisse aus (Operator `Select TOP`).
* Zählen Sie die Anzahl von Elementen in einem Resultset mithilfe von `Select COUNT`.
* Verwenden Sie Projektionen, um auszuwählen, welche Spalten von einer Abfrage zurückgegeben werden.
* Verwenden Sie Skalarfunktionen: `IS_BOOL`, `IS_DEFINED`, `IS_NULL`, `IS_NUMBER`, `IS_OBJECT`, `IS_PRIMITIVE`, `IS_STRING`, `STARTSWITH`, `ENDSWITH`.
* Verwenden Sie Vergleichsoperatoren für Abfragen: `IN`/`NIN`, `=`, `!=`, `<`, `>`, `<=`, `>=`.
* Verwenden Sie eine beliebige Kombination (Operator `AND`, `OR`, `NOT`) von `IS_OF_MODEL`, Skalarfunktionen und Vergleichsoperatoren.
* Verwenden Sie die Fortsetzung: Das Abfrageobjekt wird mit einer Seitengröße (bis zu 100) instanziiert. Sie können die digitalen Zwillinge einer Seite gleichzeitig abrufen, indem Sie das Fortsetzungstoken in nachfolgenden Aufrufen der API bereitstellen.