---
title: Was ist die Dokumentübersetzung?
description: Eine Übersicht über den cloudbasierten Batch-Dokumentenübersetzungsdienst und -prozess.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 05/20/2021
ms.openlocfilehash: 7359be792d8a832f6dcddc2c4d3caaf000930e83
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110453560"
---
# <a name="what-is-document-translation"></a>Was ist die Dokumentübersetzung?

Die Dokumentübersetzung ist ein cloudbasiertes Feature des [Azure-Übersetzerdiensts](../translator-info-overview.md) und ist Teil der Azure Cognitive Service-Familie von REST-APIs. Die API für die Dokumentübersetzung übersetzt Dokumente in und aus 90 Sprachen und Dialekten, wobei Dokumentstruktur und Datenformat beibehalten werden.

Diese Dokumentation enthält die folgenden Arten von Artikeln:  

* [**Schnellstarts**](get-started-with-document-translation.md) sind Anleitungen zu den ersten Schritten, die Sie durch das Senden von Anforderungen an den Dienst führen.
* [**Schrittanleitungen**](create-sas-tokens.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.  

## <a name="document-translation-key-features"></a>Wichtige Features für die Dokumentübersetzung

| Funktion | BESCHREIBUNG |
| ---------| -------------|
| **Große Dateien übersetzen**| Übersetzen Sie ganze Dokumente asynchron.|
|**Zahlreiche Dateien übersetzen**|Übersetzen Sie mehrere Dateien in und aus 90 Sprachen und Dialekten.|
|**Darstellung der Quelldatei beibehalten**| Übersetzen Sie Dateien unter Beibehaltung des ursprünglichen Layouts und Formats.|
|**Benutzerdefinierte Übersetzung anwenden**| Übersetzen Sie Dokumente mithilfe von allgemeinen und [benutzerdefinierten Übersetzungsmodellen](../customization.md#custom-translator).|
|**Benutzerdefinierte Glossare anwenden**|Übersetzen Sie Dokumente mithilfe von benutzerdefinierten Glossaren.|
|**Dokumentsprache automatisch erkennen**|Lassen Sie den Dienst „Dokumentübersetzung“ die Sprache des Dokuments bestimmen.|
|**Übersetzen von Dokumenten mit Inhalt in mehreren Sprachen**|Mit dem Feature zur automatischen Erkennung können Sie Dokumente mit mehrsprachigem Inhalt in Ihre Zielsprache übersetzen.|

> [!NOTE]
> Bei der Übersetzung von Dokumenten mit mehrsprachigem Inhalt ist das Feature für vollständige Sätze in einer einzelnen Sprache vorgesehen. Wenn Sätze aus Teilen in mehreren Sprachen zusammengesetzt sind, werden ggf. nicht alle in die Zielsprache übersetzt.
> 
## <a name="how-to-get-started"></a>Erste Schritte

In unserer Schrittanleitung erfahren Sie, wie Sie schnell mit den ersten Schritten der Dokumentübersetzung beginnen können. Sie benötigen ein aktives [Azure-Konto](https://azure.microsoft.com/free/cognitive-services/), damit Sie beginnen können.  Wenn Sie noch nicht über eines verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free).

> [!div class="nextstepaction"]
> [Erste Schritte](get-started-with-document-translation.md)

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate

Die folgenden Dokumentdateitypen werden von der Dokumentübersetzung unterstützt:

| Dateityp| Dateierweiterung|BESCHREIBUNG|
|---|---|--|
|Adobe PDF|.pdf|Dies ist das Portable Document Format von Adobe Acrobat.|
|Durch Trennzeichen getrennte Werte |.csv| Eine durch Trennzeichen getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|HTML|.html, .htm|Hyper Text Markup Language.|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Microsoft Excel|.xls, .xlsx|Eine Tabellenkalkulationsdatei zur Datenanalyse und Dokumentation.|
|Microsoft Outlook|.msg|Eine E-Mail-Nachricht, die in Microsoft Outlook erstellt oder gespeichert wurde.|
|Microsoft PowerPoint|.ppt, .pptx| Eine Präsentationsdatei, die zur Anzeige von Inhalten in einem Präsentationsformat verwendet wird.|
|Microsoft Word|.doc, .docx| Eine Textdokumentdatei.|
|OpenDocument-Text|.odt|Eine Open-Source-Textdokumentdatei.|
|OpenDocument-Präsentation|.odp|Eine Open-Source-Präsentationsdatei.|
|OpenDocument-Arbeitsblatt|.ods|Eine Open-Source-Arbeitsblattdatei.|
|Rich Text Format|.rtf|Ein Textdokument mit Formatierungen.|
|Per Tabulator getrennte Werte/TAB|.tsv/.tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|Text|.txt| Ein unformatiertes Textdokument.|

## <a name="supported-glossary-formats"></a>Unterstützte Glossarformate

Die folgenden Glossardateitypen werden von der Dokumentübersetzung unterstützt:

| Dateityp| Dateierweiterung|BESCHREIBUNG|
|---|---|--|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Per Tabulator getrennte Werte/TAB|.tsv/.tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte bei der Dokumentübersetzung](get-started-with-document-translation.md)
