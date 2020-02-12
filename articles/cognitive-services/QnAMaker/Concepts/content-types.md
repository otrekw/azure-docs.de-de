---
title: 'Inhaltstypen: QnA Maker'
description: Zu den Inhaltstypen gehören viele strukturierte Standarddokumente wie PDF, DOC und TXT.
services: cognitive-services
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: 3020c0623cff73e5a907f4d63f08f5866afd52dc
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902088"
---
# <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>Inhaltstypen von Dokumenten, die Sie einer Wissensdatenbank hinzufügen können
Zu den Inhaltstypen gehören viele strukturierte Standarddokumente wie PDF, DOC und TXT.

## <a name="file-and-url-data-types"></a>Datei- und URL-Datentypen

Die folgende Tabelle fasst die Inhaltstypen und Dateiformate zusammen, die von QnA Maker unterstützt werden.

|Quellentyp|Inhaltstyp| Beispiele|
|--|--|--|
|URL|Häufig gestellte Fragen<br> (flach, mit Abschnitten oder mit einer Themenstartseite)<br>Supportseiten <br> (kurze Anleitungen, Problembehandlungsartikel und Ähnliches)|[Einfache FAQ](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[FAQ mit Links](https://www.microsoft.com/en-us/software-download/faq),<br> [FAQ mit Themenstartseite](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Supportartikel](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF/DOC|Häufig gestellte Fragen,<br> Produkthandbuch,<br> Broschüren,<br> Artikel,<br> Flyerrichtlinie,<br> Supporthandbuch,<br> strukturierte QnA<br> usw.|**Ohne Mehrfachdurchläufe**<br>[Strukturiertes QnA-Dokument (DOC-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Beispielprodukthandbuch (PDF-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Semistrukturiertes Beispiel (DOC-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Beispielwhitepaper (PDF-Datei)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)<br><br>**Mehrfachdurchläufe**:<br>[Surface Pro (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Contoso Benefits (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Contoso Benefits (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|*Excel|Strukturierte QnA-Datei<br> (einschließlich RTF- und HTML-Unterstützung)|[QnA-Beispiel-FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
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

QnA Maker fügt Datei- und URL-Inhalte hinzu und konvertiert diese Inhalte in QnA-Sätze, die als Markdown (`.md`) gespeichert werden. Inhalte eignen sich dann für eine Wissensdatenbank, wenn sie in einer strukturierten Form organisiert ist und in wohldefinierten Abschnitten dargestellt werden. Die Abschnitte können weiter in Unterabschnitte oder Unterthemen unterteilt werden. Die Extraktion funktioniert am besten für Inhalte mit klarer Struktur und hierarchischen Überschriften.

QnA Maker identifiziert Abschnitte und Unterabschnitte sowie Beziehungen in den Inhalten anhand von visuellen Hinweisen wie Schriftgröße, Schriftstil, Nummerierung, Farben usw. Semistrukturierte Inhalte können Handbücher, FAQs, Leitfäden, Richtlinien, Broschüren, Flyer und viele andere Arten von Dateien sein.

## <a name="faq-urls"></a>FAQ-URLs

QnA Maker kann Webseiten mit häufig gestellten Fragen auf drei Arten unterstützen: Einfache FAQ-Seiten, FAQ-Seiten mit Links, FAQ-Seiten mit einer Themenstartseite.


### <a name="plain-faq-pages"></a>Einfache FAQ-Seiten

Dies ist der am häufigsten verwendete Typ einer FAQ-Seite, bei der auf der gleichen Seite die Antworten unmittelbar auf die Fragen folgen.

Unten sehen Sie ein Beispiel für eine einfache FAQ-Seite:

![Beispiel für eine einfache FAQ-Seite für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>FAQ-Seiten mit Links

Bei diesem FAQ-Seitentyp sind Fragen zusammengefasst und mit Antworten verknüpft, die sich in verschiedenen Abschnitten auf derselben Seite oder auf verschiedenen Seiten befinden.

Unten sehen Sie ein Beispiel für eine FAQ-Seite mit Links in Abschnitten, die sich auf derselben Seite befinden:

 ![Beispiel für eine FAQ-Seite mit Abschnittsverknüpfung für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="faq-pages-with-a-topics-homepage"></a>FAQ-Seiten mit einer Themenstartseite

Dieser Typ von FAQ weist eine Startseite mit den Themen auf, wobei jedes Thema ein Link zu seinen relevanten QnAs auf einer anderen Seite ist. In diesem Fall durchsucht QnA Maker alle verknüpften Seiten, um die entsprechenden Fragen und Antworten zu extrahieren.

Unten sehen Sie ein Beispiel für eine FAQ-Seite, auf der eine Themenstartseite Links zu FAQ-Abschnitten auf verschiedenen Seiten enthält.

 ![Beispiel für eine FAQ-Seite mit Deep-Link für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/topics-faq.png)


### <a name="support-urls"></a>Support-URLs

QnA Maker kann semistrukturierte Supportwebseiten verarbeiten. Hierzu zählen beispielsweise Webartikel, in denen die Vorgehensweise zum Ausführen einer bestimmte Aufgabe, die Diagnostizierung oder Behebung eines bestimmten Problems oder die Verwendung bewährter Methoden für einen bestimmten Prozess beschrieben wird. Die Extraktion funktioniert am besten für Inhalte mit klarer Struktur und hierarchischen Überschriften.

> [!NOTE]
> Die Extraktion für Supportartikel ist ein neues Feature und befindet sich noch in einer frühen Phase. Sie funktioniert am besten für einfache, gut strukturierte Seiten ohne komplexe Kopf- und Fußzeilen.

![QnA Maker unterstützt die Extraktion von semistrukturierten Webseiten, die über eine klare Struktur mit hierarchischen Überschriften verfügen.](../media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)


## <a name="pdf-doc-files"></a>PDF-/DOC-Dateien

QnA Maker kann semistrukturierte Inhalte in einer PDF- oder DOC-Datei verarbeiten und in QnAs konvertieren. Eine geeignete Datei, die gut extrahiert werden kann, ist eine Datei, in der der Inhalt in einer strukturierten Form organisiert ist und in wohldefinierten Abschnitten dargestellt wird. Die Abschnitte können weiter in Unterabschnitte oder Unterthemen unterteilt werden. Die Extraktion funktioniert am besten für Dokumente, die eine klare Struktur mit hierarchischen Überschriften aufweisen.

QnA Maker identifiziert Abschnitte und Unterabschnitte sowie Beziehungen in der Datei anhand von visuellen Hinweisen wie Schriftgröße, Schriftstil, Nummerierung, Farben usw. Semistrukturierte PDF- oder DOC-Dateien können Handbücher, FAQs, Leitfäden, Richtlinien, Broschüren, Flyer und viele andere Dateitypen sein. Nachfolgend finden Sie einige Beispieltypen für diese Dateien.

### <a name="product-manuals"></a>Produkthandbücher

Bei einem Handbuch handelt es sich in der Regel um Anleitungen, die mit einem Produkt geliefert werden. Es hilft dem Benutzer beim Einrichten, Verwenden und Warten des Produkts sowie der Problembehandlung. Wenn QnA Maker ein Handbuch verarbeitet, werden die Überschriften und Unterüberschriften als Fragen und die nachfolgenden Inhalte als Antworten extrahiert. Ein Beispiel finden Sie [hier](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Unten sehen Sie ein Beispiel für ein Handbuch mit einer Indexseite und hierarchischen Inhalten.

 ![Beispiel für ein Produkthandbuch für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> Das Extrahieren funktioniert am besten bei Handbüchern, die über ein Inhaltsverzeichnis und/oder eine Indexseite verfügen und eine klare Struktur mit hierarchischen Überschriften aufweisen.

### <a name="brochures-guidelines-papers-and-other-files"></a>Broschüren, Richtlinien, Artikel und andere Dateien

Viele andere Arten von Dokumenten können ebenfalls verarbeitet werden, um QA-Paare zu generieren, vorausgesetzt, sie weisen eine klare Struktur und ein klares Layout auf. Dazu gehören: Broschüren, Leitfäden, Berichte, Whitepaper, wissenschaftliche Veröffentlichungen, Richtlinien, Bücher usw. Ein Beispiel finden Sie [hier](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Unten sehen Sie ein Beispiel für ein semistrukturiertes Dokument ohne Index:

 ![Semistrukturiertes Azure Blob Storage-Dokument](../media/qnamaker-concepts-datasources/semi-structured-doc.png)

### <a name="structured-qna-document"></a>Strukturiertes QnA-Dokument

Das Format für strukturierte Fragen/Antworten in DOC-Dateien besteht aus abwechselnden Fragen und Antworten pro Zeile: eine Frage pro Zeile, gefolgt von ihrer Antwort in der folgenden Zeile, wie unten gezeigt:

```text
Question1

Answer1

Question2

Answer2
```

Unten sehen Sie ein Beispiel für ein strukturiertes QnA-Word-Dokument:

 ![Beispiel für ein strukturiertes QnA-Dokument für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/structured-qna-doc.png)

## <a name="structured-txt-tsv-and-xls-files"></a>Strukturierte *TXT*-, *TSV*- und *XLS*-Dateien

QnAs in Form von strukturierten *TXT*-, *TSV*- oder *XLS*-Dateien können auch in QnA Maker hochgeladen werden, um eine Wissensdatenbank zu erstellen oder zu erweitern.  Dabei kann es sich um Nur-Text-Dateien oder um Dateien mit Inhalten im RTF- oder HTML-Format handeln.

| Frage  | Antwort  | Metadaten (1 Schlüssel: 1 Wert) |
|-----------|---------|-------------------------|
| Frage1 | Antwort1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Frage2 | Antwort2 |      `Key:Value`           |

Alle weiteren Spalten in der Quelldatei werden ignoriert.

### <a name="example-of-structured-excel-file"></a>Beispiel für eine strukturierte Excel-Datei

Unten sehen Sie ein Beispiel für eine strukturierte QnA-*XLS*-Datei mit HTML-Inhalten:

 ![Beispiel für eine strukturierte QnA-Excel-Datei für eine Wissensdatenbank](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Beispiel für alternative Fragen zu einer einzelnen Antwort in einer Excel-Datei

Unten sehen Sie ein Beispiel für eine strukturierte QnA-*XLS*-Datei mit mehreren alternativen Fragen zu einer einzelnen Antwort:

 ![Beispiel für alternative Fragen zu einer einzelnen Antwort in einer Excel-Datei](../media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Nachdem die Datei importiert wurde, befindet sich das Frage- und Antwortpaar in der Wissensdatenbank wie nachfolgend gezeigt:

 ![Screenshot alternativer Fragen zu einer einzelnen Antwort, die in die Wissensdatenbank importiert wurden](../media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="formatting-considerations"></a>Überlegungen zur Formatierung

Nach dem Importieren einer Datei oder URL konvertiert QnA Maker den Inhalt im [Markdown-Format](https://en.wikipedia.org/wiki/Markdown) und speichert diesen. Beim Konvertierungsvorgang werden neue Zeilen im Text hinzugefügt, z. B. `\n\n`. Wenn Sie sich bereits mit dem Markdown-Format auskennen, können Sie den konvertierten Inhalt eher verstehen und den Inhalt der Wissensdatenbank verwalten.

Wenn Sie Ihren Inhalt direkt in der Wissensdatenbank hinzufügen oder bearbeiten, können Sie das **Markdown-Format** verwenden, um umfangreiche Textinhalte zu erstellen oder Inhalte im Markdown-Format zu ändern, die bereits in der Antwort vorhanden sind. QnA Maker unterstützt einen Großteil des Markdown-Formats, um umfassende Textfunktionen für Ihre Inhalte bereitzustellen. Die Clientanwendung, z. B. ein Chatbot, unterstützt jedoch möglicherweise nicht dieselben Markdown-Formate. Es ist wichtig, die Anzeige von Antworten in der Clientanwendung zu testen.

Weitere Informationen finden Sie in der [Markdown-Referenz zu QnA Maker](../reference-markdown-format.md).

## <a name="testing-your-markdown"></a>Testen Ihres Markdowncodes

Sie können Ihren Markdowncode anhand des Tutorials **[CommonMark](https://commonmark.org/help/tutorial/index.html)** überprüfen. Das Tutorial verfügt über eine Funktion **Ausprobieren** für die schnelle Überprüfung per Kopieren und Einfügen.

## <a name="next-steps"></a>Nächste Schritte

* Grundlegendes zum Entwerfen und Verwalten von [Frage-Antwort-Sätzen (QnA)](question-answer-set.md)