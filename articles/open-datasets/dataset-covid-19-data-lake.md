---
title: 'COVID-19: Data Lake'
titleSuffix: Azure Open Datasets
description: Hier erfahren Sie, wie Sie COVID-19 Data Lake in Azure Open Datasets verwenden.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: e43ea946511efaa313204ca26bf7e05f1b3f923a
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112038866"
---
# <a name="covid-19-data-lake"></a>COVID-19: Data Lake

Der COVID-19 Data Lake enthält auf COVID-19 bezogene Datasets aus verschiedenen Quellen. Es werden Daten zu Tracking- und Testdaten zu Patientenergebnissen, Social-Distancing-Vorgaben, Krankenhauskapazitäten, Mobilität und weitere Informationen abgedeckt.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

Der Data Lake zu COVID-19 wird in der Region „USA, Osten“ von Azure Data Lake Storage gehostet. Für jedes Dataset sind geänderte Versionen im CSV-, JSON-, JSONL- und Parquet-Format verfügbar. Die Rohdaten sind auch nach der Erfassung verfügbar.

ISO 3166-Unterteilungscodes werden hinzugefügt, wo sie nicht vorhanden sind, um das Verknüpfen zu vereinfachen. Spaltennamen werden in Kleinbuchstaben mit Unterstrichtrennzeichen neu formatiert. Die Datasets werden täglich aktualisiert. Dabei wird auch die Versionsgeschichte der geänderten Dateien und Rohdateien zur Verfügung gestellt.

## <a name="datasets"></a>Datasets

| Datasets                                                                 | Beschreibung                                                                                                                                                                                                                                             |
|--------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Bing: COVID-19-Daten](dataset-bing-covid-19.md)                                                       | Diese Bing-Daten zu COVID-19 enthalten bestätigte Fälle, Fälle mit tödlichem Verlauf und genesene Patienten aus allen Regionen. Sie werden täglich aktualisiert.                                                                                                                                                      |
| [COVID-Nachverfolgungsprojekt](dataset-covid-tracking.md)                                                | Das Dataset zum „The COVID Tracking Project“ enthält aktuelle Zahlen zu Tests, bestätigten Fällen, in Krankenhäuser eingewiesenen Patienten und Patientenergebnissen aus allen US-Bundesstaaten und -Gebieten.                                                                                     |
| [Covid-19-Fälle des European Centre for Disease Prevention and Control (ECDC)](dataset-ecdc-covid-cases.md) | Hier finden Sie die aktuell verfügbaren öffentlichen Daten zur geografischen Verbreitung der COVID-19-Fälle weltweit, die vom Europäischen Zentrum für die Prävention und die Kontrolle von Krankheiten (European Centre for Disease Prevention and Control, ECDC) stammen. Jede Zeile bzw. jeder Eintrag enthält die Anzahl der neu gemeldeten Fälle pro Tag und Land oder Region. |
| [Oxford COVID-19 Government Response Tracker](dataset-oxford-covid-government-response-tracker.md)                              | Das Dataset des Oxford-Trackers für Regierungsmaßnahmen angesichts COVID-19 (Oxford Covid-19 Government Response Tracker, OxCGRT) enthält systematische Informationen dazu, welche Regierungen wann welche Maßnahmen ergriffen haben.                                                                                              |

---

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den restlichen Datasets im [Open Datasets-Katalog](dataset-catalog.md) vertraut.