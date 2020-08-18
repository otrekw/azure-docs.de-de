---
author: baanders
description: Includedatei für Azure Digital Twins-Abfragevorgänge
ms.service: digital-twins
ms.topic: include
ms.date: 7/28/2020
ms.author: baanders
ms.openlocfilehash: 6dfddd9179569075abdbd94b6c1afa719db9ba1e
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905589"
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
* Verwenden Sie Skalarfunktionen: `IS_BOOL`, `IS_DEFINED`, `IS_NULL`, `IS_NUMBER`, `IS_OBJECT`, `IS_PRIMITIVE`, `IS_STRING`, `STARTSWITH`, `ENDSWITH`.
* Verwenden Sie Vergleichsoperatoren für Abfragen: `IN`/`NIN`, `=`, `!=`, `<`, `>`, `<=`, `>=`.
* Verwenden Sie eine beliebige Kombination (Operator `AND`, `OR`, `NOT`) von `IS_OF_MODEL`, Skalarfunktionen und Vergleichsoperatoren.
* Verwenden Sie die Fortsetzung: Das Abfrageobjekt wird mit einer Seitengröße (bis zu 100) instanziiert. Sie können die digitalen Zwillinge einer Seite gleichzeitig abrufen, indem Sie das Fortsetzungstoken in nachfolgenden Aufrufen der API bereitstellen.