---
title: Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure AD
description: Auswählen einer Unternehmens-App zum Zuweisen eines Benutzers oder einer Gruppe in der Azure Active Directory-Vorschau
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: mimart
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: b52bc45287e0e3a8f4908630cb6e57130c1725df
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772419"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App in Azure Active Directory

Dieser Artikel zeigt Ihnen, wie Sie Benutzer oder Gruppen in Azure Active Directory (Azure AD) Unternehmensanwendungen zuordnen können, und zwar entweder im Azure-Portal oder über PowerShell. Wenn Sie einen Benutzer einer Anwendung zuweisen, wird diese im [Zugriffsbereich „Meine Apps“](https://myapps.microsoft.com/) des Benutzers angezeigt, um den Zugriff zu erleichtern. Wenn die Anwendung Rollen verfügbar macht, können Sie dem Benutzer auch eine bestimmte Rolle zuweisen.

Für eine bessere Kontrolle können bestimmte Arten von Unternehmensanwendungen so konfiguriert werden, dass [eine Benutzerzuweisung](#configure-an-application-to-require-user-assignment) erforderlich ist. 

Um [einen Benutzer oder eine Gruppe einer Unternehmens-App zuzuweisen](#assign-users-or-groups-to-an-app-via-the-azure-portal), müssen Sie sich als globaler Administrator, Anwendungsadministrator, Cloudanwendungsadministrator oder als der zugewiesene Besitzer der Unternehmens-App anmelden.

> [!NOTE]
> Für die gruppenbasierte Zuweisung ist die Azure Active Directory Premium P1- oder P2-Edition erforderlich. Die gruppenbasierte Zuweisung wird nur für Sicherheitsgruppen unterstützt. Geschachtelte Gruppenmitgliedschaften und Office 365-Gruppen werden aktuell nicht unterstützt. Weitere Informationen zu Lizenzierungsanforderungen für die in diesem Artikel erläuterten Features finden Sie in der [Preisübersicht für Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory). 

## <a name="configure-an-application-to-require-user-assignment"></a>Konfigurieren einer Anwendung für das Anfordern einer Benutzerzuweisung

Bei den folgenden Anwendungstypen haben Sie die Möglichkeit anzufordern, dass Benutzer der Anwendung zugeordnet werden müssen, ehe sie darauf zugreifen können:

- Anwendungen, die für einmaliges Anmelden (SSO) im Verbund mit SAML-basierter Authentifizierung konfiguriert sind
- Anwendungsproxyanwendungen mit Azure Active Directory-Vorauthentifizierung
- Anwendungen, die auf der Azure AD-Anwendungsplattform bei Verwendung der OAuth 2.0/OpenID Connect-Authentifizierung erstellt werden (nachdem ein Benutzer oder Administrator seine Zustimmung für die Anwendung erteilt hat).

Wenn eine Benutzerzuweisung erforderlich ist, können sich nur die Benutzer anmelden, die Sie der Anwendung explizit zuweisen (durch direkte Benutzerzuweisung oder basierend auf Gruppenmitgliedschaft). Sie können auf die App auf ihrer Seite „Meine Apps“ oder über einen direkten Link zugreifen. 

Wenn die Zuweisung *nicht erforderlich* ist, entweder weil Sie diese Option auf **Nein** festgelegt haben oder die Anwendung einen anderen SSO-Modus verwendet, kann jeder Benutzer auf die Anwendung zugreifen. Dazu muss er über einen direkten Link zur Anwendung oder die **Benutzerzugriffs-URL** auf der Seite **Eigenschaften** der Anwendung verfügen. 

Diese Einstellung hat keine Auswirkung darauf, ob eine Anwendung im Zugriffsbereich „Meine Apps“ angezeigt wird. Anwendungen werden im Zugriffsbereich „Meine Apps“ von Benutzern angezeigt, sobald Sie der Anwendung einen Benutzer oder eine Gruppe zugewiesen haben. Hintergrundinformationen finden Sie unter [Verwaltung des Zugriffs auf Apps](what-is-access-management.md).


So fordern Sie eine Benutzerzuweisung für eine Anwendung an

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Administratorkonto oder als Besitzer der Anwendung an.

2. Wählen Sie **Azure Active Directory** aus. Wählen Sie im linken Navigationsmenü **Unternehmensanwendungen** aus.

3. Wählen Sie die Anwendung in der Liste aus. Wenn die Anwendung nicht angezeigt wird, beginnen Sie mit der Eingabe ihres Namens in das Suchfeld. Oder verwenden Sie die Filtersteuerelemente, um den Anwendungstyp, Status oder die Sichtbarkeit auszuwählen, und wählen Sie dann **Übernehmen** aus.

4. Wählen Sie im Navigationsmenü auf der linken Seite **Eigenschaften** aus.

5. Vergewissern Sie sich, dass **Benutzerzuweisung erforderlich?** auf **Ja** festgelegt ist.

   > [!NOTE]
   > Wenn der Umschalter **Benutzerzuweisung erforderlich?** nicht verfügbar ist, können Sie mit PowerShell die Eigenschaft appRoleAssignmentRequired auf den Dienstprinzipal festlegen.

6. Wählen Sie oben auf der Seite die Schaltfläche **Speichern** aus.

## <a name="assign-users-or-groups-to-an-app-via-the-azure-portal"></a>Zuweisen von Benutzern und Gruppen zu einer App im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Anwendungsadministrator, Cloudanwendungsadministrator oder als der zugewiesene Besitzer der Unternehmens-App an.
2. Wählen Sie **Azure Active Directory** aus. Wählen Sie im linken Navigationsmenü **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung in der Liste aus. Wenn die Anwendung nicht angezeigt wird, beginnen Sie mit der Eingabe ihres Namens in das Suchfeld. Oder verwenden Sie die Filtersteuerelemente, um den Anwendungstyp, Status oder die Sichtbarkeit auszuwählen, und wählen Sie dann **Übernehmen** aus.
4. Wählen Sie im Navigationsmenü auf der linken Seite **Benutzer und Gruppen** aus.
   > [!NOTE]
   > Wenn Sie Microsoft-Anwendungen wie Office 365-Apps Benutzer zuweisen möchten, unterstützen einige dieser Apps PowerShell. 
5. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**.
6. Wählen Sie im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
7. Wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie der Anwendung zuordnen möchten, oder beginnen Sie mit der Eingabe des Namens des Benutzers oder der Gruppe in das Suchfeld. Sie können mehrere Benutzer und Gruppen auswählen. Ihre Auswahl wird unter **Ausgewählte Elemente** angezeigt.
8. Klicken Sie abschließend auf **Auswählen**.

   ![Zuweisen eines Benutzers oder einer Gruppe zur App](./media/assign-user-or-group-access-portal/assign-users.png)

9. Wählen Sie im Bereich **Benutzer und Gruppen** mindestens einen Benutzer oder eine Gruppe aus der Liste aus, und wählen Sie dann am unteren Rand des Bereichs die Schaltfläche **Auswählen** aus.
10. Falls von der Anwendung unterstützt, können Sie dem Benutzer oder der Gruppe eine Rolle zuweisen. Wählen Sie im Bereich **Zuweisung hinzufügen** die Option **Rolle auswählen** aus. Wählen Sie dann im Bereich **Rolle auswählen** eine Rolle aus, die den ausgewählten Benutzern oder Gruppen zugeordnet werden soll. Wählen Sie anschließend unten im Bereich **OK** aus. 

    > [!NOTE]
    > Wenn die Anwendung die Rollenauswahl nicht unterstützt, wird die Standardzugriffsrolle zugewiesen. In diesem Fall verwaltet die Anwendung die Zugriffsebene von Benutzern.

2. Wählen Sie im Bereich **Zuweisung hinzufügen** unten die Schaltfläche **Zuweisen** aus.

## <a name="assign-users-or-groups-to-an-app-via-powershell"></a>Zuweisen von Benutzern und Gruppen zu einer App über PowerShell

1. Öffnen Sie eine Windows PowerShell-Eingabeaufforderung mit erhöhten Rechten.

   > [!NOTE]
   > Sie müssen das AzureAD-Modul installieren (verwenden Sie den Befehl `Install-Module -Name AzureAD`). Wenn Sie zum Installieren eines NuGet-Moduls oder des neuen Azure Active Directory V2-PowerShell-Moduls aufgefordert werden, geben Sie „J“ ein, und drücken Sie die EINGABETASTE.

1. Führen Sie `Connect-AzureAD` aus, und melden Sie sich mit dem Benutzerkonto eines globalen Administrators an.
1. Weisen Sie mithilfe des folgenden Skripts einer Anwendung einen Benutzer und eine Rolle zu:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"

    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }

    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

Weitere Informationen zum Zuweisen eines Benutzers zu einer Anwendungsrolle finden Sie in der Dokumentation zu [New-AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0).

Wenn Sie eine Gruppe einer Unternehmens-App zuweisen möchten, müssen Sie `Get-AzureADUser` durch `Get-AzureADGroup` und `New-AzureADUserAppRoleAssignment` durch `New-AzureADGroupAppRoleAssignment` ersetzen.

Weitere Informationen zum Zuweisen einer Gruppe zu einer Anwendungsrolle finden Sie in der Dokumentation zu [New-AzureADGroupAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureadgroupapproleassignment?view=azureadps-2.0).

### <a name="example"></a>Beispiel

In diesem Beispiel wird die Benutzerin Britta Simon mithilfe von PowerShell der Anwendung [Microsoft Workplace Analytics](https://products.office.com/business/workplace-analytics) zugewiesen.

1. Weisen Sie in PowerShell den Variablen „$username“, „$app_name“ und „$app_role_name“ die entsprechenden Werte zu.

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

1. In diesem Beispiel kennen wir den genauen Namen der Anwendungsrolle nicht, die wir Britta Simon zuweisen möchten. Führen Sie die folgenden Befehle aus, um den Benutzer ($user) und den Dienstprinzipal ($sp) abzurufen. Verwenden Sie dabei die Anzeigenamen für den Benutzer-UPN und den Dienstprinzipal.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```

1. Führen Sie den Befehl `$sp.AppRoles` aus, um die für die Workplace Analytics-Anwendung verfügbaren Rollen anzuzeigen. In diesem Beispiel möchten wir Britta Simon die Rolle „Analyst (Limited access)“ (Analyst (Eingeschränkter Zugriff)) zuweisen.

   ![Zeigt die für Benutzer mit Workplace Analytics-Rolle verfügbaren Rollen an](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

1. Weisen Sie den Rollennamen der Variablen `$app_role_name` zu.

    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

1. Führen Sie den folgenden Befehl aus, um den Benutzer der App-Rolle zuzuweisen:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="related-articles"></a>Verwandte Artikel

- [Weitere Informationen zum Endbenutzerzugriff auf Anwendungen](end-user-experiences.md)
- [Planen der Bereitstellung eines Azure AD-Zugriffsbereichs](access-panel-deployment-plan.md)
- [Verwalten des Zugriffs auf Apps](what-is-access-management.md)
 
## <a name="next-steps"></a>Nächste Schritte

- [Alle meine Gruppen anzeigen](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App](remove-user-or-group-access-portal.md)
- [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](disable-user-sign-in-portal.md)
- [Ändern des Namens oder Logos einer Unternehmens-App](change-name-or-logo-portal.md)
