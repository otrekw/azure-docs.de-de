---
title: App-Schemadefinition
description: Die LUIS-App wird entweder in `.json` oder `.lu` dargestellt und beinhaltet alle Absichten, Entitäten, Beispieläußerungen, Features und Einstellungen.
ms.topic: reference
ms.date: 05/05/2020
ms.openlocfilehash: dbbeb4eacbe8e07cf080b3a1527ca39c2b9a5991
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684362"
---
# <a name="app-schema-definition"></a>App-Schemadefinition

Die LUIS-App wird entweder in `.json` oder `.lu` dargestellt und beinhaltet alle Absichten, Entitäten, Beispieläußerungen, Features und Einstellungen.

## <a name="format"></a>Format

Wählen Sie beim Importieren und Exportieren der App entweder `.json` oder `.lu` aus.

|Format|Information|
|--|--|
|`.json`| Standardprogrammierformat|
|`.lu`|Wird von den [Bot Builder-Tools](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown/docs/lu-file-format.md) von Bot Framework unterstützt.|

## <a name="version-7x"></a>Version 7.x

* Nach der Umstellung auf Version 7.x werden die Entitäten als geschachtelte Machine Learning-Entitäten dargestellt.
* Unterstützung für die Erstellung geschachtelter Machine Learning-Entitäten mit der Eigenschaft `enableNestedChildren` für die folgenden Erstellungs-APIs:
    * [Hinzufügen einer Bezeichnung](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c08)
    * [Hinzufügen von Bezeichnungen als Batch](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c09)
    * [Überprüfen von Bezeichnungen](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c0a)
    * [Vorschlagen von Endpunktabfragen für Entitäten](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2e)
    * [Vorschlagen von Endpunktabfragen für Absichten](https://westus.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2d)

```json
{
  "luis_schema_version": "7.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-6x"></a>Version 6.x

* Verwenden Sie nach der Umstellung auf Version 6.x die neue [Machine Learning-Entität](reference-entity-machine-learned-entity.md), um Ihre Entitäten darzustellen.

```json
{
  "luis_schema_version": "6.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-4x"></a>Version 4.x

```json
{
  "luis_schema_version": "4.0.0",
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "intents": [
    {
      "name": "None"
    }
  ],
  "entities": [],
  "composites": [],
  "closedLists": [],
  "patternAnyEntities": [],
  "regex_entities": [],
  "prebuiltEntities": [],
  "model_features": [],
  "regex_features": [],
  "patterns": [],
  "utterances": [],
  "settings": []
}
```

## <a name="next-steps"></a>Nächste Schritte

* Migrieren Sie zu den [V3-Erstellungs-APIs](luis-migration-authoring-entities.md).