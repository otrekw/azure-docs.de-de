---
author: baanders
description: Include-Datei für Grenzwerte von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 6/9/2020
ms.author: baanders
ms.openlocfilehash: 21d910fb0e0992b35aa19ce65fc216734e30265c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515868"
---
### <a name="functional-limits"></a>Funktionale Grenzwerte

In der folgenden Tabelle sind die funktionalen Grenzwerte von Azure Digital Twins in der aktuellen Vorschau aufgeführt.

| Bereich | Funktion | Standardlimit | Anpassbar? |
| --- | --- | --- | --- |
| Azure-Ressource | Anzahl von Azure Digital Twins-Instanzen in einer Region pro Abonnement | 10 | Ja |
| Digital Twins | Anzahl von Zwillingen in einer Azure Digital Twins-Instanz | 200.000 | Ja |
| Routing | Anzahl von Endpunkten für eine einzelne Azure Digital Twins-Instanz | 6 | Nein |
| Routing | Anzahl von Routen für eine einzelne Azure Digital Twins-Instanz | 6 | Ja |
| Modelle | Anzahl von Modellen innerhalb einer einzelnen Azure Digital Twins-Instanz | 10.000 | Ja |
| Modelle | Anzahl der Modelle, die in einem einzigen API-Aufruf hochgeladen werden können | 250 | Nein |
| Modelle | Anzahl der auf einer einzelnen Seite zurückgegebenen Elemente | 100 | Nein |
| Abfrage | Anzahl der auf einer einzelnen Seite zurückgegebenen Elemente | 100 | Nein |
| Abfrage | Anzahl von `AND` / `OR`-Ausdrücken in einer Abfrage | 50 | Ja |
| Abfrage | Anzahl von Arrayelementen in einer `IN` / `NOT IN`-Klausel | 50 | Ja |
| Abfrage | Anzahl von Zeichen in einer Abfrage | 8\.000 | Ja |
| Abfrage | Anzahl der `JOINS` in einer Abfrage | 1 | Ja |

### <a name="rate-limits"></a>Ratenbegrenzungen

Diese Tabelle gibt die Ratenbegrenzungen verschiedener APIs an.

| API | Funktion | Standardlimit | Anpassbar? |
| --- | --- | --- | --- |
| Modelle-API | Anzahl der Anforderungen pro Sekunde | 100 | Ja |
| Digital Twins-API | Anzahl der Anforderungen pro Sekunde | 1\.000 | Ja |
| Abfrage-API | Anzahl der Anforderungen pro Sekunde | 500 | Ja |
| Abfrage-API | Abfrageeinheiten pro Sekunde | 4\.000 | Ja |
| Ereignisrouten-API | Anzahl der Anforderungen pro Sekunde | 100 | Ja |

### <a name="other-limits"></a>Andere Limits

Grenzwerte für Datentypen und Felder in DTDL-Dokumenten für Azure Digital Twins-Modelle finden Sie in der Dokumentation zu den entsprechenden Spezifikationen in GitHub: [Digital Twins Definition Language (DTDL) – Version 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).
 
Details zur Abfragelatenz und andere Richtlinien zum Schreiben von Abfragen während der Vorschau finden Sie in [*Vorgehensweise: Abfragen des Zwillingsgraphen*](../articles/digital-twins/how-to-query-graph.md).