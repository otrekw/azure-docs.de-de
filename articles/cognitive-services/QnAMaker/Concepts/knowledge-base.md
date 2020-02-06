---
title: 'Importieren aus Datenquellen: QnA Maker'
description: Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Sätzen (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: d47d994366a8057521c1cc2ab1ab8a7ec3393965
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843352"
---
# <a name="importing-from-data-sources"></a>Importieren aus Datenquellen

Eine Wissensdatenbank besteht aus Frage-Antwort-Sätzen, die über öffentliche URLs und Dateien eingefügt werden.

## <a name="data-source-locations"></a>Speicherorte von Datenquellen

Der Inhalt wird aus einer Datenquelle in eine Wissensdatenbank übertragen. Speicherorte von Datenquellen sind **öffentliche URLs oder Dateien**, die keine Authentifizierung erfordern.

[SharePoint-Dateien](../how-to/add-sharepoint-datasources.md), die durch Authentifizierung geschützt sind, stellen eine Ausnahme dar. Bei SharePoint-Ressourcen muss es sich um Dateien und nicht um Webseiten handeln. Wenn die URL mit einer Weberweiterung endet (z. B. „.ASPX“), wird sie nicht von SharePoint in QnA Maker importiert.

## <a name="chit-chat-content"></a>Geplauderinhalte

Geplauderinhalte von QnA werden als vollständige Inhaltsdatenquelle in verschiedenen Sprachen und Unterhaltungsformaten bereitgestellt. Dies kann ein Ausgangspunkt für die Persönlichkeit Ihres Bots sein und Ihnen die Zeit und die Kosten ersparen, diese Elemente von Grund auf neu zu schreiben. Erfahren Sie, wie Sie Ihrer Wissensdatenbank diesen Satz von Inhalten [hinzufügen](../how-to/chit-chat-knowledge-base.md).

## <a name="structured-data-format-through-import"></a>Strukturiertes Datenformat durch Import

Beim Importieren einer Wissensdatenbank wird der Inhalt der vorhandenen Wissensdatenbank ersetzt. Der Import erfordert eine strukturierte `.tsv`-Datei, die Fragen und Antworten enthält. Diese Informationen helfen QnA Maker beim Gruppieren der Frage-Antwort-Sätze und beim Zuweisen zu einer bestimmten Datenquelle.

| Frage  | Antwort  | `Source`| Metadaten (1 Schlüssel: 1 Wert) |
|-----------|---------|----|---------------------|
| Frage1 | Antwort1 | URL1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Frage2 | Antwort2 | Redaktionelle Änderung|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>Strukturiertes Mehrfachdurchlauf-Format durch Import

Sie können Unterhaltungen mit Mehrfachdurchläufen in einem `.tsv`-Dateiformat erstellen. Das Format bietet Ihnen die Möglichkeit, die Unterhaltungen mit Mehrfachdurchläufen zu erstellen, indem Sie vorherige Chatprotokolle analysieren (mit anderen Prozessen, nicht mithilfe von QnA Maker), und dann die `.tsv`-Datei durch Automatisierung zu erstellen. Importieren Sie die Datei, um die vorhandene Wissensdatenbank zu ersetzen.

> [!div class="mx-imgBorder"]
> ![Konzeptionelles Modell mit drei Ebenen von Fragen mit Mehrfachdurchläufen](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

Die Spalte für eine `.tsv`-Datei mit Mehrfachdurchläufen, die sich speziell für Mehrfachdurchläufe eignet, ist **Eingabeaufforderungen**. Eine `.tsv`-Beispieldatei in Excel zeigt die Informationen, die zum Definieren der untergeordneten Elemente bei Mehrfachdurchläufen eingeschlossen werden müssen:

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

**displayOrder** ist numerisch, während **displayText** ein Text ist, der kein Markdown enthalten darf.

> [!div class="mx-imgBorder"]
> ![Beispiel für eine Frage für Mehrfachdurchläufe in Excel](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>Exportieren als Beispiel

Wenn Sie nicht sicher sind, wie Sie Ihren QnA-Satz in der `.tsv`-Datei darstellen, erstellen Sie den Satz im QnA Maker-Portal, speichern Sie ihn, und exportieren Sie dann die Wissensdatenbank, um ein Beispiel für die Darstellung der Gruppe zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklungslebenszyklus einer Wissensdatenbank](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

Verwenden Sie die [Markdownreferenz](../reference-markdown-format.md) für QnA Maker als Hilfestellung für die Formatierung Ihrer Antworten.

[Übersicht über QnA Maker](../Overview/overview.md)

Informationen zum Erstellen und Bearbeiten einer Wissensdatenbank finden Sie unter:
* [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Informationen zum Generieren einer Antwort finden Sie unter:
* [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
