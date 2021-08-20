---
title: Was ist die Dokumentübersetzung?
description: Eine Übersicht über den cloudbasierten Batch-Dokumentenübersetzungsdienst und -prozess.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 06/20/2021
ms.openlocfilehash: 267109bcacd8fcb9ff7d44ca6f9a74ca4dc39fda
ms.sourcegitcommit: 54d8b979b7de84aa979327bdf251daf9a3b72964
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2021
ms.locfileid: "112582940"
---
# <a name="what-is-document-translation"></a>Was ist die Dokumentübersetzung?

Die Dokumentübersetzung ist ein cloudbasiertes Feature des [Azure-Übersetzerdiensts](../translator-info-overview.md) und ist Teil der Azure Cognitive Service-Familie von REST-APIs. Die Dokumentübersetzungs-API übersetzt Dokumente in und aus alle [unterstützten Sprachen und Dialekte](../../language-support.md), wobei Dokumentstruktur und Datenformat beibehalten werden. 

Diese Dokumentation enthält die folgenden Arten von Artikeln:  

* [**Schnellstarts**](get-started-with-document-translation.md) sind Anleitungen zu den ersten Schritten, die Sie durch das Senden von Anforderungen an den Dienst führen.
* [**Schrittanleitungen**](create-sas-tokens.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts. 
* Das [**Referenzhandbuch**](reference/rest-api-guide.md) enthält REST-API-Einstellungen, -Werte, -Keywords und -Konfigurationen.

## <a name="document-translation-key-features"></a>Wichtige Features für die Dokumentübersetzung

| Funktion | BESCHREIBUNG |
| ---------| -------------|
| **Große Dateien übersetzen**| Übersetzen Sie ganze Dokumente asynchron.|
|**Zahlreiche Dateien übersetzen**|Übersetzen Sie mehrere Dateien in und aus allen unterstützten Sprachen und Dialekten unter Beibehaltung der Dokumentstruktur und des Datenformats.|
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
|Durch Trennzeichen getrennte Datei |.csv| Eine durch Trennzeichen getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|HTML|.html, .htm|Hyper Text Markup Language.|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Markdown| .markdown, .mdown, .mkdn, .md, .mkd, .mdwn, .mdtxt, .mdtext, .rmd| Eine leichtgewichtige Markupsprache zum Erstellen von formatiertem Text|
|MHTML|.mthml, .mht| Ein Webseitenarchivformat, das zum Kombinieren von HTML-Code und zugehörigen Ressourcen verwendet wird|
|Microsoft Excel|.xls, .xlsx|Eine Tabellenkalkulationsdatei zur Datenanalyse und Dokumentation.|
|Microsoft Outlook|.msg|Eine E-Mail-Nachricht, die in Microsoft Outlook erstellt oder gespeichert wurde.|
|Microsoft PowerPoint|.ppt, .pptx| Eine Präsentationsdatei, die zur Anzeige von Inhalten in einem Präsentationsformat verwendet wird.|
|Microsoft Word|.doc, .docx| Eine Textdokumentdatei.|
|OpenDocument-Text|.odt|Eine Open-Source-Textdokumentdatei|
|OpenDocument-Präsentation|.odp|Eine Open-Source-Präsentationsdatei|
|OpenDocument-Arbeitsblatt|.ods|Eine Open-Source-Tabellendatei|
|Rich Text Format|.rtf|Ein Textdokument mit Formatierungen.|
|Per Tabulator getrennte Werte/TAB|.tsv/.tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|Text|.txt| Ein unformatiertes Textdokument.|

## <a name="supported-glossary-formats"></a>Unterstützte Glossarformate

Die folgenden Glossardateitypen werden von der Dokumentübersetzung unterstützt:

| Dateityp| Dateierweiterung|BESCHREIBUNG|
|---|---|--|
|Durch Trennzeichen getrennte Datei| .csv |Eine durch Trennzeichen getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Durch Tabstopp getrennte Werte|.tsv, .tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte bei der Dokumentübersetzung](get-started-with-document-translation.md)
