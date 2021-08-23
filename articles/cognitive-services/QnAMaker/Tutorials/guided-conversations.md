---
title: Hinzufügen strukturierter Unterhaltungen mit mehrteiligen Äußerungen
description: In diesem Tutorial erfahren Sie, wie Sie strukturierte Unterhaltungen mit mehrteiligen Äußerungen führen.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/29/2021
ms.openlocfilehash: 5c664dd2d08bb743ebe3e813808c559a30d901fc
ms.sourcegitcommit: 285d5c48a03fcda7c27828236edb079f39aaaebf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113235220"
---
# <a name="add-guided-conversations-with-multi-turn-prompts"></a>Hinzufügen strukturierter Unterhaltungen mit mehrteiligen Äußerungen

 In diesem Tutorial verwenden wir [Surface Pen-FAQ](https://support.microsoft.com/surface/how-to-use-your-surface-pen-8a403519-cd1f-15b2-c9df-faa5aa924e98), um eine Wissensdatenbank zu erstellen.

In diesem Beispiel gehen wir davon aus, dass Benutzer nach zusätzlichen Details zum Surface Pen-Produkt fragen (insbesondere zur Problembehandlung), aber nicht die richtigen Antworten erhalten. Deshalb fügen wir weitere Äußerungen hinzu, um zusätzliche Szenarien zu unterstützen und die Benutzer mithilfe der mehrteiligen Äußerungen zu den richtigen Antworten weiterzuleiten.

## <a name="view-qnas-with-context"></a>Anzeigen von F&As mit Kontext
Für die Erstellung der Wissensdatenbank auf Grundlage der [Surface Pen-FAQ](https://support.microsoft.com/surface/how-to-use-your-surface-pen-8a403519-cd1f-15b2-c9df-faa5aa924e98) aktivieren wir die Mehrfachdurchlauf-Extrahierung aus dem Quelldokument. Weitere Informationen finden Sie unter [Erstellen einer mehrteiligen Unterhaltung aus einem Dokument](../how-to/multiturn-conversation.md#create-a-multi-turn-conversation-from-a-documents-structure). Hierdurch werden die mehrteiligen Äußerungen aufgelistet, die F&A-Paaren zugeordnet sind. Diese können dann über **Kontext anzeigen** im Bereich **Ansichtsoptionen** angezeigt werden.

> [!div class="mx-imgBorder"]
> [ ![Screenshot der Ansichtsoptionen zum Anzeigen des Kontexts (rot umrandet) ]( ../media/guided-conversations/show-context.png) ]( ../media/guided-conversations/show-context.png#lightbox)

Dadurch wird die Kontextstruktur mit allen Folgeäußerungen angezeigt, die mit einem F&A-Paar verknüpft sind: 

> [!div class="mx-imgBorder"]
> [ ![Screenshot der FAQ-Seite von Surface Pen mit Antworten auf häufig gestellte Fragen ]( ../media/guided-conversations/source.png) ]( ../media/guided-conversations/source.png#lightbox)

## <a name="add-new-qna-pair-with-follow-up-prompts"></a>Hinzufügen eines neuen Frage- und Antwortpaars mit Folgeäußerungen

Um den Benutzer bei der Lösung von Problemen mit dem Surface Pen zu unterstützen, fügen wir Folgeäußerungen hinzu:

1.  Hinzufügen eines neuen F&A-Paars mit zwei Folgeäußerungen
2.  Hinzufügen einer Folgeäußerung zu einer der neu hinzugefügten Äußerungen

**Schritt 1:** Hinzufügen eines neuen F&A-Paars mit zwei Folgeäußerungen: **Kompatibilität überprüfen** und **Stifteinstellungen überprüfen** Mithilfe des Editors fügen wir ein neues F&A-Paar mit einer Folgeäußerung hinzu, indem wir auf **Add QnA pair** (F&A-Paar hinzufügen) klicken.

> [!div class="mx-imgBorder"]
> [ ![Screenshot der Benutzeroberfläche mit rot umrandetem Feld „F&A-Paar hinzufügen“]( ../media/guided-conversations/add-pair.png) ]( ../media/guided-conversations/add-pair.png#lightbox)

Im **Editorial** wird eine neue Zeile erstellt, in der wir das F&A-Paar wie nachfolgend dargestellt eingeben:

> [!div class="mx-imgBorder"]
> [ ![Screenshot der Benutzeroberfläche mit rot umrandetem Feld „Folgeäußerung hinzufügen“]( ../media/guided-conversations/follow-up.png) ]( ../media/guided-conversations/follow-up.png#lightbox)

Anschließend fügen wir dem neu erstellten F&A-Paar eine Folgeäußerung hinzu, indem wir **Folgeäußerung hinzufügen** auswählen. Danach geben wir wie dargestellt die Details für die Äußerung ein:

> [!div class="mx-imgBorder"]
> [ ![Screenshot der Benutzeroberfläche mit rot umrandetem Feld „Folgeäußerung hinzufügen“]( ../media/guided-conversations/follow-up.png) ]( ../media/guided-conversations/follow-up.png#lightbox)

Wir stellen **Kompatibilität überprüfen** als „Anzeigetext“ für die Äußerung zur Verfügung und versuchen, sie mit einem F&A-Paar zu verknüpfen. Da kein entsprechendes F&A-Paar verfügbar ist, das mit der Äußerung verknüpft werden kann, wenn wir nach „Check your Surface Pen Compatibility“ (Kompatibilität Ihres Surface Pens überprüfen) suchen, erstellen wir ein neues F&A-Paar, indem wir auf **Create new** (Neu erstellen) klicken. Nach dem **Speichern** der Änderungen wird der folgende Bildschirm angezeigt, auf dem wie unten dargestellt ein neues F&A-Paar für die Folgeäußerung eingegeben werden kann:

> [!div class="mx-imgBorder"]
> [ ![Screenshot einer Frage und Antwort in Verbindung mit der Kompatibilitätsüberprüfung Ihres Surface Pens]( ../media/guided-conversations/check-compatibility.png) ]( ../media/guided-conversations/check-compatibility.png#lightbox)

Auf ähnliche Weise fügen wir eine weitere Äußerung **Stifteinstellungen überprüfen** hinzu, um den Benutzer bei der Problembehandlung des Produkts zu unterstützen. Auch hier fügen wir wieder ein F&A-Paar hinzu.

> [!div class="mx-imgBorder"]
> [ ![Screenshot einer Frage und Antwort in Verbindung mit der Einstellungsüberprüfung Ihres Surface Pens]( ../media/guided-conversations/check-pen-settings.png) ]( ../media/guided-conversations/check-pen-settings.png#lightbox)

**Schritt 2**: Hinzufügen einer Folgeäußerung zu einer der neu hinzugefügten Äußerungen Wir fügen nun „Stiftspitze ersetzen“ als Folgeäußerung zu der zuvor erstellten Äußerung „Stifteinstellungen überprüfen“ hinzu.

> [!div class="mx-imgBorder"]
> [ ![Screenshot einer Frage und Antwort in Verbindung mit der Einstellungsüberprüfung Ihres Surface Pens. Die Informationen zum Ersetzen der Stiftspitze sind rot umrandet.]( ../media/guided-conversations/replace-pen-tips.png) ]( ../media/guided-conversations/replace-pen-tips.png#lightbox)

Abschließend speichern wir die Änderungen und testen die Äußerungen im Testbereich: 

> [!div class="mx-imgBorder"]
> [ ![Screenshot des QnAMaker-Testbereichs]( ../media/guided-conversations/test-pane.png) ]( ../media/guided-conversations/test-pane.png#lightbox)

Für die Benutzerabfrage **Probleme mit dem Surface Pen** gibt das System eine Antwort zurück und stellt dem Benutzer die neu hinzugefügten Äußerungen vor. Der Benutzer wählt dann eine der Äußerungen aus **(Stifteinstellungen überprüfen)** und daraufhin wird ihm die zugehörige Antwort mit einer weiteren Äußerung **(Stiftspitze ersetzen)** zurückgegeben. Wenn diese Option ausgewählt wird, erhält der Benutzer weitere Informationen. Mehrteilige Äußerungen werden also verwendet, um den Benutzer bei der Suche nach der benötigten Antwort zu unterstützen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erweitern Ihrer Wissensdatenbank durch aktives Lernen](active-learning.md)