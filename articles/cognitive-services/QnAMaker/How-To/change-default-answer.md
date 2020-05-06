---
title: 'Abrufen der Standardantwort: QnA Maker'
description: Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.
ms.topic: how-to
ms.date: 04/22/2020
ms.openlocfilehash: db5a79ec612a73066ac37365a1815841fafb3862
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82097097"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Ändern der Standardantwort für eine QnA Maker-Ressource

Die Standardantwort wird zurückgegeben, wenn keine Entsprechung für die Frage vorliegt. Sie können die Standardantwort bei Bedarf anpassen.

## <a name="change-default-answer-in-the-azure-portal"></a>Ändern der Standardantwort im Azure-Portal

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und navigieren Sie zu der Ressourcengruppe, die den von Ihnen erstellten QnA Maker-Dienst darstellt.

2. Klicken Sie zum Öffnen auf den **App Service**.

    ![Zugreifen auf den App-Dienst für QnA Maker im Azure-Portal](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Klicken Sie auf **Anwendungseinstellungen**, und ändern Sie den Inhalt des Felds **DefaultAnswer** in die gewünschte Standardantwort. Klicken Sie auf **Speichern**.

    ![Auswählen der Anwendungseinstellungen und Bearbeiten der Standardantwort für QnA Maker](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Starten Sie Ihren App-Dienst neu.

    ![Starten Sie QnA Maker App Service nach dem Ändern der Standardantwort neu.](../media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Wissensdatenbank](../How-to/manage-knowledge-bases.md)