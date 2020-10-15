---
title: 'Inhaltstypen: QnA Maker'
description: Zu den Inhaltstypen gehören viele strukturierte Standarddokumente wie PDF, DOCX und TXT.
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 50924ce035d9a7dc7778cf45668dc993ee5486e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776985"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Inhaltstypen von Dokumenten, die Sie einer Wissensdatenbank hinzufügen können
Zu den Inhaltstypen gehören viele strukturierte Standarddokumente wie PDF, DOC und TXT.

## <a name="file-and-url-data-types"></a>Datei- und URL-Datentypen

Die folgende Tabelle fasst die Inhaltstypen und Dateiformate zusammen, die von QnA Maker unterstützt werden.

|Quellentyp|Inhaltstyp| Beispiele|
|--|--|--|
|URL|Häufig gestellte Fragen<br> (flach, mit Abschnitten oder mit einer Themenstartseite)<br>Supportseiten <br> (kurze Anleitungen, Problembehandlungsartikel und Ähnliches)|[Einfache FAQ](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[FAQ mit Links](https://www.microsoft.com/en-us/software-download/faq),<br> [FAQ mit Themenstartseite](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Supportartikel](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Häufig gestellte Fragen,<br> Produkthandbuch,<br> Broschüren,<br> Artikel,<br> Flyerrichtlinie,<br> Supporthandbuch,<br> strukturierte QnA<br> usw.|**Ohne Mehrfachdurchläufe**<br>[Strukturiertes QnA-Dokument (DOCX-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Beispielprodukthandbuch (PDF-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Semistrukturiertes Beispiel (DOCX-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Beispielwhitepaper (PDF-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)<br><br>**Mehrfachdurchläufe**:<br>[Surface Pro (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Contoso Benefits (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Contoso Benefits (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|*Excel|Strukturierte QnA-Datei<br> (einschließlich RTF- und HTML-Unterstützung)|**Ohne Mehrfachdurchläufe**:<br>[QnA-Beispiel-FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)<br><br>**Mehrfachdurchläufe**:<br>[Strukturierte einfache FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx)<br>[Surface-Laptop-FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-Surface-Pro.xlsx)|
|*TXT/TSV|Strukturierte QnA-Datei|[Beispielgeplauder.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

Wenn eine Authentifizierung für Ihre Datenquelle erforderlich ist, ziehen Sie die folgenden Methoden in Betracht, um diese Inhalte in QnA Maker aufzunehmen:

* Manuelles Herunterladen und Importieren der Datei in QnA Maker
* Hinzufügen der Datei aus einem authentifizierten [SharePoint-Speicherort](../how-to/add-sharepoint-datasources.md)

## <a name="url-content"></a>URL-Inhalt

Zwei Arten von Dokumenten können über die **URL** in QnA Maker importiert werden:

* FAQ-URLs
* Support-URLs

Jeder Typ gibt ein erwartetes Format an.

## <a name="file-based-content"></a>Dateibasierte Inhalte

Sie können im [QnA Maker-Portal](https://www.qnamaker.ai) einer Wissensdatenbank Dateien aus einer öffentlichen Quelle oder Ihrem lokalen Dateisystem hinzufügen.

## <a name="content-format-guidelines"></a>Richtlinien zum Inhaltsformat

Erfahren Sie mehr über die [Formatierungsrichtlinien](../reference-document-format-guidelines.md) für die verschiedenen Dateien.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, welche Informationen in einem [Frage-Antwort-Paar (QnA)](question-answer-set.md) gespeichert sind.
