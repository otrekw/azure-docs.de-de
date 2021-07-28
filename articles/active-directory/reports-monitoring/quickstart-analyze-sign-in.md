---
title: Analysieren von Anmeldungen mit dem Azure AD-Anmeldeprotokoll
description: In diesem Schnellstart erfahren Sie, wie Sie das Anmeldeprotokoll verwenden können, um den Grund für eine fehlgeschlagene Anmeldung bei Azure AD zu ermitteln.
services: active-directory
ms.service: active-directory
ms.subservice: report-monitor
ms.topic: quickstart
ms.date: 06/03/2021
ms.author: markvi
author: MarkusVi
manager: mtillman
ms.reviewer: besiler
ms.collection: M365-identity-device-management
ms.openlocfilehash: beaab0517c1bd0d3aa06cc31cf6c087896f7d52b
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111442032"
---
# <a name="quickstart-analyze-sign-ins-with-the-azure-ad-sign-ins-log"></a>Schnellstart: Analysieren von Anmeldungen mit dem Azure AD-Anmeldeprotokoll 

Mit den Informationen im Azure AD-Anmeldeprotokoll können Sie herausfinden, was passiert ist, wenn bei einer Anmeldung eines Benutzers ein Fehler aufgetreten ist. In diesem Schnellstart erfahren Sie, wie Sie fehlerhafte Anmeldungen mithilfe des Anmeldeprotokolls ermitteln können.


## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung des Szenarios im Rahmen dieses Schnellstarts benötigen Sie Folgendes:

- **Zugriff auf einen Azure AD-Mandanten**: Wenn Sie keinen Zugriff auf einen Azure AD-Mandanten besitzen, lesen Sie [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- **Ein Testkonto unter dem Namen Isabella Simonsen**: Wenn Sie nicht wissen, wie Sie ein Testkonto erstellen, lesen Sie [Hinzufügen von cloudbasierten Benutzern](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="perform-a-failed-sign-in"></a>Ausführen einer fehlerhaften Anmeldung

Das Ziel dieses Schritts besteht im Erstellen eines Datensatzes für eine fehlgeschlagene Anmeldung im Azure AD-Anmeldeprotokoll.

**So führen Sie diesen Schritt aus:**

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/) als Isabella Simonsen mit einem falschen Kennwort an.

2. Warten Sie 5 Minuten, um sicherzustellen, dass Sie einen Datensatz für die Anmeldung im Anmeldeprotokoll finden können. Weitere Informationen finden Sie unter [Aktivitätsberichte](reference-reports-latencies.md#activity-reports).



## <a name="find-the-failed-sign-in"></a>Suchen nach der fehlgeschlagenen Anmeldung

In diesem Abschnitt werden die Schritte zum Analysieren einer fehlgeschlagenen Anmeldung beschrieben:

- **Filtern von Anmeldungen**: Entfernen Sie alle Datensätze, die für Ihre Analyse nicht relevant sind. Legen Sie beispielsweise einen Filter fest, um nur die Datensätze eines bestimmten Benutzers anzuzeigen.
- **Suchen nach zusätzlichen Fehlerinformationen**: Zusätzlich zu den Informationen, die Sie im Anmeldeprotokoll finden, können Sie den Fehler auch mit dem [Tool für die Suche nach Anmeldefehlern](https://login.microsoftonline.com/error) suchen. Dieses Tool enthält möglicherweise zusätzliche Informationen zu einem Anmeldefehler. 


**So überprüfen Sie die fehlgeschlagene Anmeldung:**

1. Navigieren Sie zum [Anmeldeprotokoll](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns).

2. So listen Sie nur Datensätze für Isabella Simonsen auf:

    a. Klicken Sie auf der Symbolleiste auf **Filter hinzufügen**.
    
    ![Hinzufügen eines Benutzerfilters](./media/quickstart-analyze-sign-in/add-filters.png)   

    b. Wählen Sie in der Liste **Feld auswählen** die Option **Benutzer** aus, und klicken Sie dann auf **Übernehmen**.

    c. Geben Sie im Textfeld **Benutzername** den Namen **Isabella Simonsen** ein,und klicken Sie dann auf **Übernehmen**.

    d. Klicken Sie auf der Symbolleiste auf **Aktualisieren**.

3. Klicken Sie zum Analysieren des Problems auf **Problembehandlung und Support**.

    ![Filter hinzufügen](./media/quickstart-analyze-sign-in/troubleshooting-and-support.png)   

4. Kopieren Sie **den Anmeldefehlercode**.

    ![Anmeldefehler](./media/quickstart-analyze-sign-in/sign-in-error-code.png)   


5. Fügen Sie den Fehlercode in das Textfeld des [Anmeldetools für die Fehlersuche](https://login.microsoftonline.com/error) ein, und klicken Sie dann auf **Übermitteln**.

Überprüfen Sie das Ergebnis des Tools, und ermitteln Sie, ob es zusätzliche Informationen bereitstellt.

![Fehlercode-Suchtool](./media/concept-all-sign-ins/error-code-lookup-tool.png)


## <a name="additional-tests"></a>Zusätzliche Tests

Da Sie nun wissen, wie Sie einen Eintrag im Anmeldeprotokoll anhand des Namens finden, sollten Sie auch versuchen, den Datensatz mithilfe der folgenden Filter zu finden:

- **Datum**: Versuchen Sie, mithilfe eines **Start**- und eines **Endwerts** nach Isabella zu suchen.

    ![Datumsfilter](./media/quickstart-analyze-sign-in/start-and-end-filter.png)

- **Status**: Versuchen Sie, nach Isabella zu suchen, indem Sie **Status: Fehler** verwenden.

    ![Status „Fehler“](./media/quickstart-analyze-sign-in/status-failure.png)




## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Testbenutzer nicht mehr benötigen, löschen Sie ihn. Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen von Benutzern aus Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Was sind Azure Active Directory-Berichte?](overview-reports.md)
