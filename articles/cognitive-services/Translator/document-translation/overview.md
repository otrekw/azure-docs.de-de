---
title: Was ist die Dokumentübersetzung?
description: Eine Übersicht über den cloudbasierten Batch-Dokumentenübersetzungsdienst und -prozess.
ms.topic: overview
manager: nitinme
ms.author: lajanuar
author: laujan
ms.date: 02/11/2021
ms.openlocfilehash: fc75bfc3b79cd6aed51a44fc47787532a9d61e8b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101713013"
---
# <a name="what-is-document-translation-preview"></a>Was ist die Dokumentübersetzung (Vorschau)?

Die Dokumentübersetzung ist ein cloudbasiertes Feature des [Azure-Übersetzerdiensts](../translator-info-overview.md) und ist Teil der Azure Cognitive Service-Familie von REST-APIs. Die API für die Dokumentübersetzung übersetzt Dokumente in und aus 90 Sprachen und Dialekten, wobei Dokumentstruktur und Datenformat beibehalten werden.

## <a name="document-translation-key-features"></a>Wichtige Features für die Dokumentübersetzung

| Funktion | BESCHREIBUNG |
| ---------| -------------|
| **Große Dateien übersetzen**| Übersetzen Sie ganze Dokumente asynchron.|
|**Zahlreiche Dateien übersetzen**|Übersetzen Sie mehrere Dateien in und aus 90 Sprachen und Dialekten.|
|**Darstellung der Quelldatei beibehalten**| Übersetzen Sie Dateien unter Beibehaltung des ursprünglichen Layouts und Formats.|
|**Benutzerdefinierte Übersetzung anwenden**| Übersetzen Sie Dokumente mithilfe von allgemeinen und [benutzerdefinierten Übersetzungsmodellen](../customization.md#custom-translator).|
|**Benutzerdefinierte Glossare anwenden**|Übersetzen Sie Dokumente mithilfe von benutzerdefinierten Glossaren.|

## <a name="how-to-get-started"></a>Erste Schritte

In unserer Schrittanleitung erfahren Sie, wie Sie schnell mit den ersten Schritten der Dokumentübersetzung beginnen können. Sie benötigen ein aktives [Azure-Konto](https://azure.microsoft.com/free/cognitive-services/), damit Sie beginnen können.  Wenn Sie noch nicht über eines verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free).

> [!div class="nextstepaction"]
> [Erste Schritte](get-started-with-document-translation.md)

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate

Die folgenden Dokumentdateitypen werden von der Dokumentübersetzung unterstützt:

| Dateityp| Dateierweiterung|BESCHREIBUNG|
|---|---|--|
|Adobe PDF|.pdf|Dies ist das Portable Document Format von Adobe Acrobat.|
|HTML|.html|Hyper Text Markup Language.|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Microsoft Excel|.xlsx|Eine Tabellenkalkulationsdatei zur Datenanalyse und Dokumentation.|
|Microsoft Outlook|.msg|Eine E-Mail-Nachricht, die in Microsoft Outlook erstellt oder gespeichert wurde.|
|Microsoft PowerPoint|.pptx| Eine Präsentationsdatei, die zur Anzeige von Inhalten in einem Präsentationsformat verwendet wird.|
|Microsoft Word|.docx| Eine Textdokumentdatei.|
|Per Tabulator getrennte Werte/TAB|.tsv/.tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|
|Text|.txt| Ein unformatiertes Textdokument.|
|Translation Memory Exchange|.tmx|Ein offener XML-Standard, der für den Austausch von TM-Daten (Translation Memory) verwendet wird, die von CAT- (Computer Aided Translation) und Lokalisierungsanwendungen erstellt wurden.|

## <a name="supported-glossary-formats"></a>Unterstützte Glossarformate

Die folgenden Glossardateitypen werden von der Dokumentübersetzung unterstützt:

| Dateityp| Dateierweiterung|BESCHREIBUNG|
|---|---|--|
|Localization Interchange File Format|.xlf. , xliff| Ein Format für parallele Dokumente, das auch Translation Memory-Systeme exportiert. Die verwendeten Sprachen werden in der Datei definiert.|
|Per Tabulator getrennte Werte/TAB|.tsv/.tab| Eine durch Tabulator getrennte Datei mit Rohdaten, die von Tabellenkalkulationsprogrammen verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte bei der Dokumentübersetzung](get-started-with-document-translation.md)
>
>
