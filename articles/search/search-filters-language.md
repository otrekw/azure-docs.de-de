---
title: Filtern nach Sprache in einem Suchindex
titleSuffix: Azure Cognitive Search
description: Filterkriterien zur Unterstützung der mehrsprachigen Suche, wobei der Gültigkeitsbereich der Suchabfrage auf sprachspezifische Felder festgelegt wird.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: b0ebbbb64e173e1501f08f8385b14c365759a804
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82116280"
---
# <a name="how-to-filter-by-language-in-azure-cognitive-search"></a>Filtern nach Sprache in der kognitiven Azure-Suche 

In einer mehrsprachigen Suchanwendung ist es wichtig, nach Ergebnissen in der Sprache des Benutzers suchen und entsprechende Ergebnisse abrufen zu können. Um die Sprachanforderungen einer mehrsprachigen App zu erfüllen, kann in der kognitiven Azure-Suche beispielsweise eine Reihe von Feldern für die Speicherung von Zeichenfolgen in einer bestimmten Sprache erstellt und die Volltextsuche dann bei der Abfrage auf diese Felder beschränkt werden.

Mithilfe von Abfrageparametern in der Anforderung wird der Gültigkeitsbereich der Suchabfrage festgelegt, und Ergebnisse aus Feldern, deren Inhalt nicht der gewünschten Sucherfahrung entspricht, werden ignoriert.

| Parameter | Zweck |
|-----------|--------------|
| **searchFields** | Grenzt die Volltextsuche auf die Liste der angegebenen Felder ein. |
| **$select** | Kürzt die Antwort, sodass nur die von Ihnen angegebenen Felder berücksichtigt werden. Standardmäßig werden alle abrufbaren Felder zurückgegeben. Mit dem Parameter **$select** können Sie auswählen, welche Felder berücksichtigt werden sollen. |

Der Erfolg dieser Methode hängt von der Integrität der Feldinhalte ab. Die kognitive Azure-Suche übersetzt keine Zeichenfolgen und führt auch keine Spracherkennung durch. Sie müssen selbst dafür sorgen, dass die Felder die erwarteten Zeichenfolgen enthalten.

## <a name="define-fields-for-content-in-different-languages"></a>Definieren von Feldern für Inhalt in verschiedenen Sprachen

In der kognitiven Azure-Suche sind Abfragen auf einen einzelnen Index ausgerichtet. Entwickler, die sprachspezifische Zeichenfolgen in einer einzelnen Suchumgebung bereitstellen möchten, definieren üblicherweise dedizierte Felder, um die Werte zu speichern – also ein Feld für englische Zeichenfolgen, ein Feld für Französisch und so weiter. 

Das folgende Beispiel stammt aus dem [Immobilienbeispiel](search-get-started-portal.md), das über mehrere Zeichenfolgenfelder verfügt, die Inhalte in verschiedenen Sprachen enthalten. Sehen Sie sich die Sprachanalysezuweisungen für die Felder in diesem Index an. Aus Leistungsgründen empfiehlt es sich, Felder mit Textinhalt bei der Volltextsuche mit einer Analysekomponente zu kombinieren, die die linguistischen Regeln der Zielsprache behandelt.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Codebeispiele zur Veranschaulichung von Felddefinitionen mit Sprachanalysen finden Sie unter [Definieren eines Index (.NET)](https://docs.microsoft.com/azure/search/search-create-index-dotnet) sowie unter [Definieren eines Index (REST)](search-create-index-rest-api.md).

## <a name="build-and-load-an-index"></a>Erstellen und Laden eines Index

Ein (wahrscheinlich naheliegender) Zwischenschritt vor der Erstellung einer Abfrage ist das [Erstellen und Auffüllen des Index](https://docs.microsoft.com/azure/search/search-create-index-dotnet). Er wird daher nur der Vollständigkeit halber erwähnt. Die Verfügbarkeit des Index kann unter anderem durch das Überprüfen der Indexliste im [Portal](https://portal.azure.com) ermittelt werden.

## <a name="constrain-the-query-and-trim-results"></a>Einschränken der Abfrage und Ausschließen von Ergebnissen

Abfrageparameter dienen dazu, die Suche auf bestimmte Felder einzugrenzen und anschließend die Ergebnisse von Feldern auszuschließen, die für Ihr Szenario nicht relevant sind. Wenn Sie die Suche also beispielsweise auf Felder mit französischen Zeichenfolgen einschränken möchten, richten Sie die Abfrage mithilfe von **searchFields** auf Felder aus, die Zeichenfolgen in dieser Sprache enthalten. 

Standardmäßig werden bei einer Suche alle als abrufbar markierten Felder zurückgegeben. Es empfiehlt sich daher, Felder auszuschließen, die für die angestrebte sprachspezifische Sucherfahrung nicht geeignet sind. Wenn Sie die Suche auf ein Feld mit französischen Zeichenfolgen eingeschränkt haben, möchten Sie wahrscheinlich Felder mit englischen Zeichenfolgen aus Ihren Ergebnissen ausschließen. Mithilfe des Abfrageparameters **$select** können Sie steuern, welche Felder an die aufrufende Anwendung zurückgegeben werden sollen.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Die Abfrage enthält zwar kein $filter-Argument, allerdings ist dieser Anwendungsfall eng mit Filterkonzepten verwandt, weshalb er als Filterszenario präsentiert wird.

## <a name="see-also"></a>Weitere Informationen

+ [Filter in Azure Cognitive Search](search-filters.md)
+ [Sprachanalysen](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Funktionsweise der Volltextsuche in Azure Cognitive Search](search-lucene-query-architecture.md)
+ [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Suchen nach Dokumenten (Azure Search Service-REST-API))

