---
title: Was ist der plastische Reader?
titleSuffix: Azure Cognitive Services
description: Der plastische Reader ist ein Tool, das Personen mit Lernschwierigkeiten sowie Leseanfänger und Sprachenlernende beim Leseverständnis unterstützt.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 01/4/2020
ms.author: metang
ms.custom: cog-serv-seo-aug-2020
keywords: Leser, Sprachenlernende, Bilder anzeigen, Lesen verbessern, Inhalte lesen, übersetzen
ms.openlocfilehash: 1911d3ce62434f1dc24966798e9ace259ae072bc
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104801569"
---
# <a name="what-is-immersive-reader"></a>Worum handelt es sich beim plastischen Reader?

Der [plastische Reader](https://www.onenote.com/learningtools) ist ein für die Inklusion konzipiertes Tool, das bewährte Techniken implementiert, um das Leseverständnis von Leseanfängern, Sprachenlernenden und Personen mit Lernschwierigkeiten (beispielsweise Dyslexie) zu verbessern. Mit der Clientbibliothek für den plastischen Reader können Sie Ihre Webanwendungen mit der gleichen Technologie verbessern, die auch in Microsoft Word und Microsoft OneNote zum Einsatz kommt. 

Diese Dokumentation enthält die folgenden Arten von Artikeln:  

* **[Schnellstarts](quickstarts/client-libraries.md)** sind Anleitungen zu den ersten Schritten, die Sie durch das Senden von Anforderungen an den Dienst führen.
* **[Schrittanleitungen](how-to-create-immersive-reader.md)** enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.

## <a name="use-immersive-reader-to-improve-reading-accessibility"></a>Verbessern der Zugänglichkeit des Lesens mithilfe des plastischen Readers 

Der plastische Reader wurde entwickelt, um das Lesen für alle einfacher und zugänglicher zu machen. Im Anschluss werden einige der wichtigsten Features des plastischen Readers erläutert.

### <a name="isolate-content-for-improved-readability"></a>Isolieren von Inhalten zur besseren Lesbarkeit

Der plastische Reader isoliert Inhalte, um die Lesbarkeit zu verbessern. 

  ![Isolieren von Inhalten zur besseren Lesbarkeit mit dem plastischen Reader](./media/immersive-reader.png)

### <a name="display-pictures-for-common-words"></a>Anzeigen von Bildern für gängige Wörter

Für häufig verwendete Begriffe zeigt der plastische Reader ein Bild an.

  ![Bildwörterbuch des plastischen Readers](./media/picture-dictionary.png)

### <a name="highlight-parts-of-speech"></a>Hervorheben von Wortarten

Der plastische Reader kann Verben, Nomen, Pronomen und Ähnliches hervorheben und so zum besseren Verständnis von Wortarten und Grammatik beitragen.

  ![Anzeigen von Wortarten mit dem plastischen Reader](./media/parts-of-speech.png)

### <a name="read-content-aloud"></a>Lautes Vorlesen von Inhalten

Dank der in den Dienst des plastischen Readers integrierten Sprachsynthese (Text-zu-Sprache) können Leser Text markieren und laut vorlesen lassen. 

  ![Lautes Vorlesen von Text mit dem plastischen Reader](./media/read-aloud.png)

### <a name="translate-content-in-real-time"></a>Übersetzen von Inhalten in Echtzeit

Der plastische Reader kann Text in Echtzeit in eine Vielzahl von Sprachen übersetzen. Dies ist hilfreich, um das Verständnis von Lesern zu verbessern, die eine neue Sprache lernen.

  ![Übersetzen von Text mit dem plastischen Reader](./media/translation.png)

### <a name="split-words-into-syllables"></a>Unterteilen von Wörtern in Silben

Mit dem plastischen Reader können Sie Wörter in Silben unterteilen, um die Lesbarkeit zu verbessern oder neue Wörter zu verlautlichen.

  ![Aufteilen von Wörtern in Silben mit dem plastischen Reader](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Wie funktioniert der plastische Reader?

Der plastische Reader ist eine eigenständige Webanwendung. Nach dem Aufrufen über die Clientbibliothek für den plastischen Reader wird der plastische Reader im Vordergrund Ihrer vorhandenen Webanwendung in einem Element vom Typ `iframe` angezeigt. Wenn Ihre Webanwendung den Dienst des plastischen Readers aufruft, geben Sie den Inhalt für den Reader an. Die Clientbibliothek für den plastischen Reader kümmert sich um die Erstellung und Gestaltung des Elements vom Typ `iframe` sowie um die Kommunikation mit dem Back-End-Dienst des plastischen Readers. Der Dienst des plastischen Readers verarbeitet den Inhalt für die Wortartermittlung, Sprachsynthese, Übersetzung und Ähnliches.

## <a name="get-started-with-immersive-reader"></a>Erste Schritte mit dem plastischen Reader

Die Clientbibliothek für den plastischen Reader ist in C#, JavaScript, Java (Android), Kotlin (Android) und Swift (iOS) verfügbar. Erste Schritte mit:

* [Schnellstart: Verwenden der Clientbibliothek für den plastischen Reader](quickstarts/client-libraries.md)
