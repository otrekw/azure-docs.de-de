---
title: 'Bearbeiten einer Wissensdatenbank: QnA Maker'
description: Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 3cf5dcedd4d2cfa82fda002b71abca1e5bea4b18
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756719"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Bearbeiten von Frage-Antwort-Paaren in Ihrer Wissensdatenbank

Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.

Frage-Antwort-Paare werden aus einer Datenquelle (beispielsweise einer Datei oder URL) oder als redaktionelle Quelle hinzugefügt. Eine redaktionelle Quelle deutet darauf hin, dass das Frage-Antwort-Paar manuell im QnA-Portal hinzugefügt wurde. Alle Frage-Antwort-Paare können bearbeitet werden.

## <a name="add-an-editorial-qna-pair"></a>Hinzufügen eines redaktionellen Frage-Antwort-Paars
1. Melden Sie sich beim [QnA-Portal](https://www.qnamaker.ai/) an, und wählen Sie die Wissensdatenbank aus, der Sie das Frage-Antwort-Paar hinzufügen möchten.
1. Wählen Sie auf der Seite **BEARBEITEN** der Wissensdatenbank die Option **Add QnA pair** (Frage-Antwort-Paar hinzufügen) aus, um ein neues Frage-Antwort-Paar hinzuzufügen.

1. Fügen Sie in der Zeile des neuen Frage-Antwort-Paars die erforderlichen Felder für **Frage** und **Antwort** hinzu. Die anderen Felder sind optional. Alle Felder können jederzeit geändert werden.

1. Fügen Sie optional **alternative Formulierungen** hinzu. Eine alternative Formulierung ist eine beliebige Form der Frage, die sich deutlich von der ursprünglichen Frage unterscheidet, aber die gleiche Antwort liefern sollte.

    Wenn Ihre Wissensdatenbank veröffentlicht wurde und Sie [aktives Lernen](use-active-learning.md) aktiviert haben, sammelt QnA Maker alternative Formulierungen, die Sie akzeptieren können. Diese Optionen werden ausgewählt, um die Vorhersagegenauigkeit zu erhöhen.

1. Fügen Sie optional **Metadaten** hinzu. Wählen Sie im Kontextmenü die Option **Ansichtsoptionen** aus, um Metadaten anzuzeigen. Metadaten bieten Filter für die Antworten, die von der Clientanwendung (beispielsweise von einem Chatbot) bereitgestellt werden.

1. Fügen Sie optional **Folgeaufforderungen** hinzu. Folgeaufforderungen bieten zusätzliche Konversationspfade, die die Clientanwendung dem Benutzer präsentieren kann.

1. Wählen Sie **Speichern und trainieren** aus, um Vorhersagen mit dem neuen Frage-Antwort-Paar zu erhalten.

## <a name="edit-a-qna-pair"></a>Bearbeiten eines Frage-Antwort-Paars

Jedes Feld eines Frage-Antwort-Paars kann bearbeitet werden – unabhängig von der ursprünglichen Datenquelle. Aufgrund der aktuellen **Ansichtsoptionen** (auf der Kontextsymbolleiste) werden unter Umständen nicht alle Felder angezeigt.

## <a name="delete-a-qna-pair"></a>Löschen eines Fragen-und-Antworten-Paars

Zum Löschen eines Fragen-und-Antworten-Paars klicken Sie in der Fragen-und-Antworten-Zeile ganz rechts auf das Symbol **Löschen**. Das Paar wird endgültig gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Es empfiehlt sich gegebenenfalls, vor dem Löschen von Frage-Antwort-Sätzen Ihre Wissensdatenbank auf der Seite **Veröffentlichen** zu exportieren.

![Löschen eines Fragen-und-Antworten-Paars](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-pair-id"></a>Ermitteln der ID eines Frage-Antwort-Paars

Die ID eines Frage-Antwort-Paars kann auf zwei Arten ermittelt werden:

* Zeigen Sie in der Zeile des gewünschten Frage-Antwort-Paars auf das Löschsymbol. Der daraufhin eingeblendete Text enthält die ID des Frage-Antwort-Paars.
* Exportieren Sie die Wissensdatenbank. Die ID des Frage-Antwort-Paars ist im jeweiligen Frage-Antwort-Paar in der Wissensdatenbank enthalten.

## <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen

Fügen Sie alternative Fragen zu einem vorhandenen Fragen-und-Antworten-Paar hinzu, um die Wahrscheinlichkeit einer Übereinstimmung mit einer Benutzerabfrage zu erhöhen.

![Hinzufügen alternativer Fragen](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-pairs"></a>Verknüpfen von Frage-Antwort-Paaren

Zur Verknüpfung von Frage-Antwort-Paaren werden [Folgeaufforderungen](multiturn-conversation.md) verwendet. Hierbei handelt es sich um eine logische Verbindung zwischen Frage-Antwort-Paaren, die auf der Ebene der Wissensdatenbank verwaltet wird. Folgeaufforderungen können im QnA Maker-Portal bearbeitet werden.

Frage-Antwort-Paare können nicht in den Metadaten der Antwort verknüpft werden.

## <a name="add-metadata"></a>Hinzufügen von Metadaten

Fügen Sie Metadatensätze hinzu, indem Sie zuerst **Ansichtsoptionen** und dann **Metadaten anzeigen** auswählen. Dadurch wird die Metadatenspalte angezeigt. Wählen Sie anschließend das Zeichen **+** aus, um einen Metadatensatz hinzuzufügen. Dieser Satz besteht aus einem Schlüssel und einem Wert.

## <a name="save-changes-to-the-qna-pairs"></a>Speichern von Änderungen an Frage-Antwort-Paaren

Wählen Sie bei der Bearbeitung regelmäßig **Save and train** (Speichern und trainieren) aus, damit keine Änderungen verloren gehen.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zusammenarbeiten an einer Wissensdatenbank](./collaborate-knowledge-base.md)

* [Verwalten von QnA Maker-Ressourcen](set-up-qnamaker-service-azure.md)