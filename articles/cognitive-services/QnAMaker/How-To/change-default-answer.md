---
title: 'Abrufen der Standardantwort: QnA Maker'
description: Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.
ms.topic: how-to
ms.date: 07/02/2020
ms.openlocfilehash: 005442938167c1bf7927603c44d6f870795cbeee
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85979968"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Ändern der Standardantwort für eine QnA Maker-Ressource

Die Standardantwort für eine Wissensdatenbank sollte zurückgegeben werden, wenn keine Antwort gefunden wird. Wenn Sie eine Clientanwendung verwenden, z. B. den [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs#calling-qna-maker-from-your-bot), verfügt diese möglicherweise über eine separate Standardantwort, in der die Information enthalten ist, dass keine Antwort den Bewertungsschwellenwert erreicht hat.

## <a name="set-default-answer-when-you-create-knowledge-base"></a>Festlegen einer Standardantwort beim Erstellen einer Wissensdatenbank

Wenn Sie eine neue Wissensdatenbank erstellen, gehört der Standardantworttext zu den Einstellungen. Wenn Sie diesen nicht während der Erstellung festlegen, können Sie ihn später wie folgt ändern.

## <a name="change-default-answer-in-qna-maker-portal"></a>Ändern der Standardantwort im QnA Maker-Portal

Die Standardantwort der Wissensdatenbank wird zurückgegeben, wenn keine Antwort vom QnA Maker-Dienst zurückgegeben wird.

1. Melden Sie sich beim [QnA Maker](https://www.qnamaker.ai/)-Portal an, und wählen Sie die Wissensdatenbank aus der Liste aus.
1. Klicken Sie in der Navigationsleiste auf **Einstellungen**.
1. Ändern Sie den Wert **Default answer text** (Standardantworttext) im Abschnitt **Manage knowledge base** (Wissensdatenbank verwalten).

    :::image type="content" source="../media/qnamaker-concepts-confidencescore/change-default-answer.png" alt-text="Screenshot: QnA Maker-Portal, Einstellungsseite, Textfeld für die Standardantwort hervorgehoben":::

1. Klicken Sie auf **Speichern und Trainieren**, um die Änderungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Wissensdatenbank](../How-to/manage-knowledge-bases.md)