---
title: 'Abrufen der Standardantwort: QnA Maker'
description: Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.
ms.topic: how-to
ms.date: 07/13/2020
ms.openlocfilehash: d37e63d84be58e6ccd2f1e23a1344961d39ffa01
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87054174"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Ändern der Standardantwort für eine QnA Maker-Ressource

Die Standardantwort für eine Wissensdatenbank sollte zurückgegeben werden, wenn keine Antwort gefunden wird. Wenn Sie eine Clientanwendung verwenden, z. B. den [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs#calling-qna-maker-from-your-bot), verfügt diese möglicherweise über eine separate Standardantwort, in der die Information enthalten ist, dass keine Antwort den Bewertungsschwellenwert erreicht hat.

## <a name="types-of-default-answer"></a>Arten von Standardantworten

In Ihrer Wissensdatenbank gibt es zwei Arten von Standardantworten. Es ist wichtig zu verstehen, wie und wann die einzelne Standardantwort von einer Vorhersageabfrage zurückgegeben wird:


|Art der Frage|Beschreibung der Antwort|
|--|--|
|Antwort der Wissensdatenbank, wenn keine Antwort ermittelt wird|`No good match found in KB.`: Wenn die [GenerateAnswer-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) keine passende Antwort auf die Frage findet, wird die `DefaultAnswer`-Einstellung des App-Diensts zurückgegeben. Alle Wissensdatenbanken in derselben QnA Maker-Ressource verwenden denselben Standardantworttext.<br>Sie können die Einstellung im Azure-Portal, über den App-Dienst oder mit den REST-APIs zum [Abrufen](https://docs.microsoft.com/rest/api/appservice/webapps/listapplicationsettings) oder [Aktualisieren](https://docs.microsoft.com/rest/api/appservice/webapps/updateapplicationsettings) der Einstellung verwalten.|
|Anweisungstext der Folgeäußerung|Wenn Sie eine Folgeäußerung in einem Konversationsfluss verwenden, benötigen Sie möglicherweise keine Antwort im QnA-Paar, da Sie möchten, dass der Benutzer aus den Folgeäußerungen auswählen kann. Legen Sie in diesem Fall bestimmten Text fest, indem Sie den Standardantworttext festlegen, der bei jeder Vorhersage für Folgeäußerungen zurückgegeben wird. Der Text soll als Anweisungstext zur Auswahl von Folgeäußerungen angezeigt werden. Ein Beispiel für diesen Standardantworttext ist `Please select from the following choices`. Diese Konfiguration wird in den nächsten Abschnitten dieses Dokuments erläutert. Kann auch als Teil der Wissensdatenbankdefinition von `defaultAnswerUsedForExtraction` unter Verwendung der [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create) festgelegt werden.|

### <a name="client-application-integration"></a>Clientanwendungsintegration

Für eine Clientanwendung, z. B. für einen Bot mit dem **Azure Bot Service**, können Sie aus den folgenden gängigen Szenarien auswählen:

* Verwenden der Einstellung der Wissensdatenbank
* Verwenden Sie unterschiedlichen Text in der Clientanwendung, um zu unterscheiden, wenn eine Antwort zurückgegeben wird, diese aber nicht den Bewertungsschwellenwert erreicht. Bei diesem Text kann es sich entweder um statischen Text handeln, der im Code gespeichert ist, oder er kann in der Einstellungsliste der Clientanwendung gespeichert werden.

## <a name="set-follow-up-prompts-default-answer-when-you-create-knowledge-base"></a>Festlegen der Standardantwort der Folgeäußerung, wenn Sie eine Wissensdatenbank erstellen

Wenn Sie eine neue Wissensdatenbank erstellen, gehört der Standardantworttext zu den Einstellungen. Wenn Sie diesen nicht während der Erstellung festlegen, können Sie ihn später wie folgt ändern.

## <a name="change-follow-up-prompts-default-answer-in-qna-maker-portal"></a>Ändern der Standardantwort der Folgeäußerung im QnA Maker-Portal

Die Standardantwort der Wissensdatenbank wird zurückgegeben, wenn keine Antwort vom QnA Maker-Dienst zurückgegeben wird.

1. Melden Sie sich beim [QnA Maker](https://www.qnamaker.ai/)-Portal an, und wählen Sie die Wissensdatenbank aus der Liste aus.
1. Klicken Sie in der Navigationsleiste auf **Einstellungen**.
1. Ändern Sie den Wert **Default answer text** (Standardantworttext) im Abschnitt **Manage knowledge base** (Wissensdatenbank verwalten).

    :::image type="content" source="../media/qnamaker-concepts-confidencescore/change-default-answer.png" alt-text="Screenshot: QnA Maker-Portal, Einstellungsseite, Textfeld für die Standardantwort hervorgehoben":::

1. Klicken Sie auf **Speichern und Trainieren**, um die Änderungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Wissensdatenbank](../How-to/manage-knowledge-bases.md)
