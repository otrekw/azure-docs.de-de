---
title: 'Bearbeiten einer Wissensdatenbank: QnA Maker'
description: Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 223ad3a607adc20bbe608598da9742d56788b2c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131654"
---
# <a name="edit-qna-sets-in-your-knowledge-base"></a>Bearbeiten von Frage-Antwort-Sätzen in Ihrer Wissensdatenbank

Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.

Frage-Antwort-Sätze werden aus einer Datenquelle (beispielsweise eine Datei oder URL) oder als redaktionelle Quelle hinzugefügt. Eine redaktionelle Quelle deutet darauf hin, dass der Frage-Antwort-Satz manuell im QnA-Portal hinzugefügt wurde. Alle Frage-Antwort-Sätze können bearbeitet werden.

## <a name="add-an-editorial-qna-set"></a>Hinzufügen eines redaktionellen Frage-Antwort-Satzes
1. Melden Sie sich beim [QnA-Portal](https://www.qnamaker.ai/) an, und wählen Sie die Wissensdatenbank aus, der Sie den Frage-Antwort-Satz hinzufügen möchten.
1. Wählen Sie auf der Seite **BEARBEITEN** der Wissensdatenbank die Option **Add QnA set** (Frage-Antwort-Satz hinzufügen) aus, um einen neuen Frage-Antwort-Satz hinzuzufügen.

1. Fügen Sie in der Zeile des neuen Frage-Antwort-Satzes die erforderlichen Felder für **Frage** und **Antwort** hinzu. Die anderen Felder sind optional. Alle Felder können jederzeit geändert werden.

1. Fügen Sie optional **alternative Formulierungen** hinzu. Eine alternative Formulierung ist eine beliebige Form der Frage, die sich deutlich von der ursprünglichen Frage unterscheidet, aber die gleiche Antwort liefern sollte.

    Wenn Ihre Wissensdatenbank veröffentlicht wurde und Sie aktives Lernen aktiviert haben, sammelt QnA Maker alternative Formulierungen, die Sie akzeptieren können. Diese Optionen werden ausgewählt, um die Vorhersagegenauigkeit zu erhöhen.

1. Fügen Sie optional **Metadaten** hinzu. Wählen Sie im Kontextmenü die Option **Ansichtsoptionen** aus, um Metadaten anzuzeigen. Metadaten bieten Filter für die Antworten, die von der Clientanwendung (beispielsweise von einem Chatbot) bereitgestellt werden.

1. Fügen Sie optional **Folgeaufforderungen** hinzu. Folgeaufforderungen bieten zusätzliche Konversationspfade, die die Clientanwendung dem Benutzer präsentieren kann.

1. Wählen Sie **Save and train** (Speichern und trainieren) aus, um Vorhersagen mit dem neuen Frage-Antwort-Satz zu erhalten.

## <a name="edit-a-qna-set"></a>Bearbeiten eines Frage-Antwort-Satzes

Jedes Feld eines Frage-Antwort-Satzes kann bearbeitet werden – unabhängig von der ursprünglichen Datenquelle. Aufgrund der aktuellen **Ansichtsoptionen** (auf der Kontextsymbolleiste) werden unter Umständen nicht alle Felder angezeigt.

## <a name="delete-a-qna-set"></a>Löschen eines Frage-Antwort-Satzes

Zum Löschen eines Fragen-und-Antworten-Paars klicken Sie in der Fragen-und-Antworten-Zeile ganz rechts auf das Symbol **Löschen**. Das Paar wird endgültig gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Es empfiehlt sich gegebenenfalls, vor dem Löschen von Frage-Antwort-Sätzen Ihre Wissensdatenbank auf der Seite **Veröffentlichen** zu exportieren.

![Löschen eines Frage-Antwort-Satzes](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-set-id"></a>Ermitteln der ID eines Frage-Antwort-Satzes

Die ID eines Frage-Antwort-Satzes kann auf zwei Arten ermittelt werden:

* Zeigen Sie in der Zeile des gewünschten Frage-Antwort-Satzes auf das Löschsymbol. Der daraufhin eingeblendete Text enthält die ID des Frage-Antwort-Satzes.
* Exportieren Sie die Wissensdatenbank. Die ID des Frage-Antwort-Satzes ist im jeweiligen Frage-Antwort-Satz in der Wissensdatenbank enthalten.

## <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen

Fügen Sie einem vorhandenen Frage-Antwort-Satz alternative Fragen hinzu, um die Wahrscheinlichkeit eines Treffers für eine Benutzerabfrage zu erhöhen.

![Hinzufügen alternativer Fragen](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-sets"></a>Verknüpfen von Frage-Antwort-Sätzen

Zur Verknüpfung von Frage-Antwort-Sätzen werden [Folgeaufforderungen](multiturn-conversation.md) verwendet. Hierbei handelt es sich um eine logische Verbindung zwischen Frage-Antwort-Sätzen, die auf der Ebene der Wissensdatenbank verwaltet wird. Folgeaufforderungen können im QnA Maker-Portal bearbeitet werden.

Frage-Antwort-Sätze können nicht in den Metadaten der Antwort verknüpft werden.

## <a name="add-metadata"></a>Hinzufügen von Metadaten

Fügen Sie Metadatensätze hinzu, indem Sie zuerst **Ansichtsoptionen** und dann **Metadaten anzeigen** auswählen. Dadurch wird die Metadatenspalte angezeigt. Wählen Sie anschließend das Zeichen **+** aus, um einen Metadatensatz hinzuzufügen. Dieser Satz besteht aus einem Schlüssel und einem Wert.

## <a name="save-changes-to-the-qna-sets"></a>Speichern von Änderungen an Frage-Antwort-Sätzen

Wählen Sie bei der Bearbeitung regelmäßig **Save and train** (Speichern und trainieren) aus, damit keine Änderungen verloren gehen.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zusammenarbeiten an einer Wissensdatenbank](./collaborate-knowledge-base.md)

* [Verwalten von QnA Maker-Ressourcen](set-up-qnamaker-service-azure.md)