---
title: Versionsverwaltung der Modelle
titleSuffix: Azure Cognitive Services
description: Angeben von Modellversionen in den V3-Endpunkten
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 4cd8d0901ce23fb227bb6919dee18f5aa3d063ed
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089055"
---
Mit Version 3 der Textanalyse-API können Sie die für Ihre Daten aktuelle Modellversion auswählen. Verwenden Sie den optionalen Parameter `model-version`, um die für Ihre Anforderungen gewünschte Version des Modells auszuwählen. Wenn dieser Parameter nicht angegeben wird, legt die API standardmäßig `latest` fest, die aktuelle stabile Version. Sie können zwar die aktuelle Modellversion in Ihren Anforderungen verwenden, in jeder Version werden aber nur einige Features aktualisiert. In der folgenden Tabelle wird beschrieben, welche Features in den einzelnen Modellversionen aktualisiert wurden:

| Modellversion           | Aktualisierte Features         | Aktuelle Version für:           |
|-------------------------|--------------------------|--------------------------|
| `2020-02-01`            | Entitätserkennung                      | Entitätserkennung                      |
| `2019-10-01`            | Entitätserkennung, Standpunktanalyse  | Spracherkennung, Schlüsselbegriffserkennung und Standpunktanalyse|


Jede Antwort von V3-Endpunkten enthält ein `model-version`-Feld, das die verwendete Modellversion angibt.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
Ausführliche Informationen zu den Updates für diese Modellversionen finden Sie unter [Neuigkeiten bei der Textanalyse-API](../whats-new.md).
