---
title: 'Zusammenarbeit an der Wissensdatenbank: QnA Maker'
description: QnA Maker ermöglicht mehreren Personen, an einer Wissensdatenbank zusammenzuarbeiten. Diese Funktion wird im Rahmen der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) bereitgestellt.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 8adc02d4b397c2b0bdaf5d3998cfbe229c896d41
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776849"
---
# <a name="collaboration-with-authors-and-editors"></a>Zusammenarbeit mit Autoren und Redakteuren

Die Zusammenarbeit erfolgt auf der Ebene der QnA Maker-Ressource, um die Einschränkung des Zugriffs von Projektmitarbeitern auf der Grundlage ihrer Rolle zu ermöglichen. Weitere Informationen zu den Konzepten im Zusammenhang mit der Authentifizierung von QnA Maker-Projektmitarbeitern finden Sie [hier](../Concepts/role-based-access-control.md).

## <a name="add-azure-role-based-access-control-azure-rbac-to-your-qna-maker-resource"></a>Hinzufügen der rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, Azure RBAC) zu Ihrer QnA Maker-Ressource

Mit QnA Maker können mehrere Personen an allen Wissensdatenbanken in derselben QnA Maker-Ressource zusammenarbeiten. Diese Funktion wird im Rahmen der [rollenbasierten Zugriffssteuerung in Azure (Azure RBAC)](../../../active-directory/role-based-access-control-configure.md) bereitgestellt.

## <a name="access-at-the-qna-maker-resource-level"></a>Zugreifen auf der Ebene der QnA Maker-Ressource

In einem QnA Maker-Dienst kann keine bestimmte Wissensdatenbank freigegeben werden. Sollten Sie eine differenzierte Zugriffssteuerung benötigen, verteilen Sie Ihre Wissensdatenbanken ggf. auf verschiedene QnA Maker-Ressourcen, und fügen Sie ihnen jeweils Rollen hinzu.

## <a name="add-role-to-resource"></a>Hinzufügen einer Rolle zu einer Ressource

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Hinzufügen eines Benutzerkontos zur QnA Maker-Ressource

In den folgenden Schritten wird zwar die Rolle „Projektmitarbeiter“ verwendet, mit den gezeigten Schritten können jedoch auch andere [Rollen](../reference-role-based-access-control.md) hinzugefügt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu Ihrer QnA Maker-Ressource.

    ![QnA Maker-Ressourcenliste](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. Öffnen Sie die Registerkarte **Zugriffssteuerung (IAM)** .

    ![IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. Wählen Sie **Hinzufügen**.

    ![Hinzufügen von IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Wählen Sie eine Rolle aus der folgenden Liste aus:

    |Rolle|
    |--|
    |Besitzer|
    |Mitwirkender|
    |Cognitive Services QnA Maker-Leseberechtigter|
    |Cognitive Services QnA Maker-Editor|
    |Cognitive Services-Benutzer|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="Hinzufügen einer IAM-Rolle in QnA Maker.":::

1. Geben Sie die E-Mail-Adresse des Benutzers ein, und klicken Sie auf **Speichern**.

    ![Hinzufügen einer E-Mail-Adresse für IAM in QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Anzeigen von QnA Maker-Wissensdatenbanken

Wenn sich die Person, für die Sie Ihren QnA Maker-Dienst freigegeben haben, beim [QnA Maker-Portal](https://qnamaker.ai) anmeldet, werden ihr basierend auf ihrer Rolle alle Wissensdatenbanken in diesem Dienst angezeigt.

Wenn sie eine Wissensdatenbank auswählen, wird ihre aktuelle Rolle auf dieser QnA Maker-Ressource neben dem Namen der Wissensdatenbank angezeigt.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Hinzufügen einer IAM-Rolle in QnA Maker.":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Testen einer Wissensdatenbank](./test-knowledge-base.md)

Ausführlichere Informationen zur Zusammenarbeit finden Sie hier:
* [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](../../../active-directory/role-based-access-control-configure.md)
* [Zusammenarbeiten mit anderen Autoren und Redakteuren](../Concepts/role-based-access-control.md)
