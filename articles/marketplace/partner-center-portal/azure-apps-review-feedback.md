---
title: Umgang mit Prüfungsfeedback für Azure-Apps-Angebote im kommerziellen Marketplace
description: Der Umgang mit Prüfungsfeedback für Azure-Apps-Angebote für das Anbieten oder Verkaufen in Azure Marketplace, AppSource oder über das CSP-Programm (Cloud Solution Provider) mit dem kommerziellen Marketplace-Portal im Microsoft Partner Center
author: MaggiePucciEvans
manager: evansma
ms.author: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 5dd74a7be95ecf92289f2a144c9c509c71ce935b
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75933645"
---
# <a name="handling-review-feedback"></a>Verarbeiten von Prüfungsfeedback

In diesem Artikel wird erläutert, wie Sie auf die Azure DevOps-Umgebung zugreifen können, die vom Microsoft Azure Marketplace-Prüfungsteam verwendet wird. Wenn während des Schritts **Prüfung durch Microsoft** kritische Probleme in Ihrem Azure-Anwendungsangebot gefunden werden, können Sie sich bei diesem System anmelden, um detaillierte Informationen zu diesen Problemen einzusehen (Prüfungsfeedback). Nachdem Sie alle diese Probleme behoben haben, müssen Sie Ihr Angebot erneut übermitteln, damit es in Azure Marketplace weiter veröffentlicht wird. Das folgende Diagramm veranschaulicht, wie sich dieser Feedbackvorgang auf den Veröffentlichungsvorgang auswirkt.

![Der Prozess des Prüfungsfeedbacks](./media/review-feedback-process.png)

In der Regel werden bei der Prüfung festgestellte Probleme als Pull-Requests (PR) bezeichnet. Jeder Pull Request ist mit einem Onlineelement von [Azure DevOps](https://azure.microsoft.com/services/devops/) (zuvor Visual Studio Team Services [VSTS] genannt) verknüpft, das Details zu dem Problem enthält. Die folgende Abbildung zeigt ein Beispiel der Partner Center-Oberfläche, wenn während der Prüfung Fehler gefunden werden. 

![Veröffentlichungsstatus](./media/publishing-status.png)

Der Pull Request, der bestimmte Informationen über die Übermittlung enthält, wird im Link „Zertifizierungsbericht anzeigen“ erwähnt. Für komplexe Szenarien erhalten Sie E-Mails vom Prüfungs- und Supportteam.

## <a name="azure-devops-access"></a>Azure DevOps-Zugriff

Alle Benutzer mit Zugriff auf die Rolle „Entwickler“ im Partner Center können die Pull Request-Elemente im Prüfungsfeedback sehen.

<!---
To view the PR items referenced in review feedback, publishers must first be granted proper authorization. Otherwise, new publishers receive a 401 - Not Authorized response page when trying to view PRs. To request access to this Azure DevOps repository, perform the following steps:

1. Collect the following information:
    * Your publisher name and ID
    * Offer type (Azure app), offer name, and SKU ID
    * The pull request link, for example: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`. This URL can be retrieved from the notification message or the address of the 401 response page.
    * The email address(es) of the individuals from your publishing organization that you want access granted to. This list should include the owner address(es) you provided when registering as a publisher on the Cloud Partner Portal (or Partner Center??).
1. Create a support incident. In the title bar of the Cloud Partner Portal (??), select the **Help** button, then choose **Support** from the menu. Your default web browser should launch and navigate to the Microsoft new support incident page. (You may have to sign in first.)
1. Specify the **Problem type** as **marketplace onboarding** and **Category** as **Access problem**, then select **Start request**.

    ![Create an incident](./media/create-an-incident.png)

1. In **Step 1 of 2** page, supply your contact information and select **Continue**.
1. In **Step 2 of 2** page, specify an incident title (for example Request Azure DevOps access) and supply the information you collected in the first step (above). Read and accept the agreement, then select **Submit**.

If the incident creation was successful, a confirmation page is displayed. Save the confirmation information on this page for your reference. The Microsoft Support Team should reply to your access request within a few business days. --->

## <a name="reviewing-the-pull-request"></a>Prüfen des Pull Requests

Gehen Sie wie folgt vor, um die im Pull Request dokumentierten Probleme zu prüfen.

1. Klicken Sie in den Abschnitten **Prüfung durch Microsoft** des Formulars Veröffentlichungsschritte auf einen Pull Request-Link, um Ihren Browser zu öffnen, und wechseln Sie dann zur Seite **Übersicht** (Startseite) dieses Pull Requests. Die folgende Abbildung zeigt eine exemplarische Startseite mit kritischem Problem für das Beispielangebot einer App von Contoso. Diese Seite enthält nützliche zusammenfassende Informationen zu den bei der Prüfung der Azure-App gefundenen Problemen.

    [![Pull Request-Startseite](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Klicken Sie auf die Abbildung, um sie zu vergrößern.*

1. (Optional) Klicken Sie rechts im Fenster im Abschnitt **Richtlinien** auf die Problemmeldung (in diesem Beispiel: **Fehler bei Validierung der Richtlinie**), um die untergeordneten Details des Problems einschließlich der zugehörigen Protokolldateien zu untersuchen. Fehler werden in der Regel am unteren Rand der Protokolldateien angezeigt.
1. Wählen Sie im Menü auf der linken Seite der Startseite **Dateien**, um die Listendateien anzuzeigen, die die technischen Ressourcen für dieses Angebot darstellen. Die Microsoft-Prüfer sollten Kommentare hinzugefügt haben, die die ermittelten kritischen Probleme beschreiben. Im folgenden Beispiel wurden zwei Probleme ermittelt.

    [![Pull Request-Startseite](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Klicken Sie auf die Abbildung, um sie zu vergrößern.*

1. Klicken Sie in der linken Struktur auf die einzelnen Kommentarknoten, um im Kontext des umgebenden Codes zum Kommentar zu navigieren. Korrigieren Sie Ihren Quellcode im Projekt Ihres Teams, um das im Kommentar beschriebene Problem zu beheben.

>[!Note]
>Sie können die technischen Ressourcen Ihres Angebots nicht in der Azure DevOps-Umgebung des Prüfungsteams bearbeiten. Für Herausgeber ist dies eine schreibgeschützte Umgebung für den enthaltenen Quellcode. Sie können jedoch Antworten auf die Kommentare hinterlassen, die dem Microsoft-Prüfungsteam zugutekommen.

   Im folgenden Beispiel hat der Herausgeber das erste Problem geprüft, korrigiert und mit einer Antwort versehen.

   ![Erste Korrektur und Antwort auf Kommentar](./media/first-comment-reply.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die kritischen Probleme korrigiert haben, die in den Pull Requests der Prüfung dokumentiert sind, müssen Sie [Ihr Azure-App-Angebot erneut veröffentlichen](./create-new-azure-apps-offer.md#publish).
