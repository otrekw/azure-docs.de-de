---
title: 'Abrufen der Standardantwort: QnA Maker'
description: Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: fae5c38fd64435a3fae56862bad04e000916e88b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76843275"
---
# <a name="set-default-answer-for-a-knowledge-base"></a>Festlegen der Standardantwort für eine Wissensdatenbank

Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.

## <a name="change-default-answer"></a>Ändern der Standardantwort

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und navigieren Sie zu der Ressourcengruppe, die den von Ihnen erstellten QnA Maker-Dienst darstellt.

2. Klicken Sie zum Öffnen auf den **App Service**.

    ![Zugreifen auf den App-Dienst für QnA Maker im Azure-Portal](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Klicken Sie auf **Anwendungseinstellungen**, und ändern Sie den Inhalt des Felds **DefaultAnswer** in die gewünschte Standardantwort. Klicken Sie auf **Speichern**.

    ![Auswählen der Anwendungseinstellungen und Bearbeiten der Standardantwort für QnA Maker](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Starten Sie Ihren App-Dienst neu.

    ![Starten Sie QnA Maker App Service nach dem Ändern der Standardantwort neu.](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Bots mit QnA Maker und LUIS](../tutorials/integrate-qnamaker-luis.md)