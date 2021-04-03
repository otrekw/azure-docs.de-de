---
title: App-Schemadefinition
description: Die LUIS-App wird entweder in `.json` oder `.lu` dargestellt und beinhaltet alle Absichten, Entitäten, Beispieläußerungen, Features und Einstellungen.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 08/22/2020
ms.openlocfilehash: db4fd52dad82542f20e58ebb3b8526c5be7f2f88
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91327322"
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
  "phraselists": [
  ],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

| element                  | Comment                              |
|--------------------------|--------------------------------------|
| "hierarchicals": [],     | Veraltet, [Machine Learning-Entitäten](luis-concept-entity-types.md) verwenden.   |
| "composites": [],        | Veraltet, [Machine Learning-Entitäten](luis-concept-entity-types.md) verwenden. Verweis auf [Entität vom Typ „Composite“](reference-entity-composite.md). |
| "closedLists": [],       | Verweis auf [Listenentitäten](reference-entity-list.md), wird hauptsächlich von Funktionen auf Entitäten verwendet.    |
| "versionId": "0.1",      | Version einer LUIS-App.|
| "name": "example-app",   | Name der LUIS-App. |
| "desc": "",              | Optionale Beschreibung der LUIS-App.  |
| "culture": "en-us",      | [Sprache](luis-language-support.md) der App, wirkt sich auf zugrunde liegende Features wie vorgefertigte Entitäten, Machine Learning und Tokenizer aus.  |
| "tokenizerVersion": "1.0.0", | [Tokenizer](luis-language-support.md#tokenization)  |
| "patternAnyEntities": [],   | [Entität „Pattern.any“](reference-entity-pattern-any.md)    |
| "regex_entities": [],    |  [Entität vom Typ „RegEx“](reference-entity-regular-expression.md)   |
| "phraselists": [],       |  [Begriffslisten (Feature)](luis-concept-feature.md#create-a-phrase-list-for-a-concept)   |
| "regex_features": [],    |  Veraltet, [Machine Learning-Entitäten](luis-concept-entity-types.md) verwenden. |
| "patterns": [],          |  [Verbessern der Vorhersagegenauigkeit mit Mustern](luis-concept-patterns.md) mit [Mustersyntax](reference-pattern-syntax.md)   |
| "settings": []           | [App-Einstellungen](luis-reference-application-settings.md)|

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