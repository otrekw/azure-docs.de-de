---
title: Erweitern Ihrer Wissensdatenbank durch aktives Lernen
description: In diesem Tutorial erfahren Sie, wie Sie Ihre Wissensdatenbanken mit aktivem Lernen erweitern.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/29/2021
ms.openlocfilehash: f2af689bc0052decd91e8e8e5238ab88f828d261
ms.sourcegitcommit: 285d5c48a03fcda7c27828236edb079f39aaaebf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113235243"
---
# <a name="enrich-your-knowledge-base-with-active-learning"></a>Erweitern Ihrer Wissensdatenbank durch aktives Lernen

In diesem Tutorial erfahren Sie, wie Sie Ihre Wissensdatenbank durch aktives Lernen erweitern. Das kann beispielsweise nötig sein, wenn Ihre Kunden Fragen stellen, die nicht Teil Ihrer Wissensdatenbank sind. Von vielen Fragen gibt es unterschiedlich formulierte Varianten. 

Wenn diese Varianten als Alternativfragen zum entsprechenden Frage-Antwort-Paar hinzugefügt werden, kann die Wissensdatenbank für die Beantwortung echter Benutzerfragen optimiert werden. Sie können Alternativfragen manuell über den Editor zum Frage-Antwort-Paar hinzufügen. Sie können auch das Feature für aktives Lernen verwenden, um Vorschläge zu generieren, die auf Benutzerfragen basieren. Für das aktive Lernen ist es jedoch notwendig, dass die Wissensdatenbank regelmäßig Benutzerdatenverkehr empfängt, damit Vorschläge generiert werden können.

## <a name="enable-active-learning"></a>Aktivieren des aktiven Lernens
Das aktive Lernen ist für das benutzerdefinierte Feature „Fragen und Antworten“ standardmäßig aktiviert. Sie müssen die Einstellung „Aktives Lernen“ für die allgemein verfügbare Version von QnA Maker jedoch manuell aktualisieren. Weitere Informationen finden Sie unter [Aktivieren des aktiven Lernens](../how-to/use-active-learning.md#turn-on-active-learning-for-alternate-questions).

Importieren Sie die Datei [SampleActiveLearning.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/SampleActiveLearning.tsv), um die Vorschläge durch aktives Lernen zu testen. Weitere Informationen zum Importieren einer Wissensdatenbank finden Sie unter [Importieren der Wissensdatenbank](migrate-knowledge-base.md).

## <a name="view-and-addreject-active-learning-suggestions"></a>Anzeigen, Hinzufügen und Ablehnen von Vorschlägen durch aktives Lernen
Sobald die Vorschläge durch aktives Lernen verfügbar sind, können sie über **View Options** > **Show active learning suggestions** (Optionen anzeigen > Vorschläge durch aktives Lernen anzeigen) angezeigt werden.

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Optionen „Optionen anzeigen“ und „Vorschläge durch aktives Lernen anzeigen“ mit roter Umrandung]( ../media/active-learning/view-options.png) ]( ../media/active-learning/view-options.png#lightbox)

Wenn Sie auf **Vorschläge durch aktives Lernen anzeigen** klicken, wird eine Filteroption nach Frage-Antwort-Paaren mit Vorschlägen aktiviert. Wenn das aktive Lernen deaktiviert ist oder keine Vorschläge vorhanden sind, ist auch die Option **Vorschläge durch aktives Lernen anzeigen** deaktiviert.

> [!div class="mx-imgBorder"]
> [ ![Screenshot: „Nach Vorschlägen filtern“ in roter Umrandung]( ../media/active-learning/filter-by-suggestions.png) ]( ../media/active-learning/filter-by-suggestions.png#lightbox)

Sie können nur nach den Frage-Antwort-Paaren mit durch aktives Lernen vorgeschlagenen Alternativfragen filtern, damit die folgende gefilterte Liste der Frage-Antwort-Paare angezeigt wird:

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Frage „help with surface pen“ in roter Umrandung]( ../media/active-learning/help.png) ]( ../media/active-learning/help.png#lightbox)


Sie können diese Vorschläge jetzt über das Häkchen annehmen oder über das Kreuz ablehnen. Das ist für jedes Frage-Antwort-Paar einzeln oder über die Optionen **Akzeptieren/Ablehnen** im oberen Bereich der Seite möglich.

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Optionen zum Akzeptieren oder Ablehnen in roter Umrandung]( ../media/active-learning/accept-reject.png) ]( ../media/active-learning/accept-reject.png#lightbox)

Die Wissensdatenbank ändert sich nur, wenn Sie die Vorschläge des aktiven Lernens hinzufügen oder bearbeiten. Klicken Sie abschließend auf „Speichern und trainieren“, um die Änderungen zu speichern.

> [!NOTE] 
> Im Artikel [Aktives Lernen](../how-to/use-active-learning.md) erfahren Sie, wie Sie die Version und die Diensteinstellungen für das aktive Lernen überprüfen.

## <a name="add-alternate-questions-using-editor"></a>Hinzufügen von Alternativfragen mit dem Editor

Das aktive Lernen schlägt zwar automatisch Alternativfragen auf Grundlage von Benutzerfragen vor, die mit der Wissensdatenbank übereinstimmen, doch Sie können auch über den Editor Varianten für eine Frage hinzufügen.
Wählen Sie das Frage-Antwort-Paar aus, für das eine Alternativfrage hinzugefügt werden soll, und klicken Sie auf **Add alternative phrasing** (Alternative Formulierung hinzufügen).

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Option „Alternative Formulierung hinzufügen“ in roter Umrandung]( ../media/active-learning/add-alternative-phrasing.png) ]( ../media/active-learning/add-alternative-phrasing.png#lightbox)

Zum Frage-Antwort-Paar hinzugefügte Alternativfragen werden folgendermaßen dargestellt:

> [!div class="mx-imgBorder"]
> [ ![Screenshot: Frage „draw with your pen“ in roter Umrandung]( ../media/active-learning/draw-with-pen.png) ]( ../media/active-learning/draw-with-pen.png#lightbox)

Wenn Sie zusätzlich zum aktiven Lernen Alternativfragen hinzufügen, erweitern Sie die Wissensdatenbank durch Fragenvarianten, damit die gleiche Antwort auf ähnliche Benutzerfragen gegeben werden kann.


> [!NOTE] 
> Alternativfragen können viele Stoppwörter enthalten und sich daher nicht wie erwartet auf die Genauigkeit der Antwort auswirken. Wenn der Unterschied bei den Alternativfragen nur in diesen Stoppwörtern besteht, sind keine Alternativfragen notwendig.

Sehen Sie sich die [Liste der Stoppwörter](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/STOPWORDS.md) an.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verbessern der Antwortqualität mit Synonymen](adding-synonyms.md)