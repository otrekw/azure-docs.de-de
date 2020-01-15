---
title: Konfigurieren der Art der Endbenutzereinwilligung für Anwendungen in Azure AD
description: Erfahren Sie, wie Sie verwalten können, wie und wann Benutzer Anwendungen zustimmen können, die Zugriff auf die Daten Ihrer Organisation haben.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5bd305d2943d1b12756171748f28d32300081d71
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443392"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Konfigurieren der Art und Weise, wie Endbenutzer Anwendungen zustimmen können

Anwendungen können in die Microsoft Identity Platform integriert werden, um Benutzern die Anmeldung mit ihrem Geschäfts-, Schul- oder Unikonto in Azure Active Directory (Azure AD) und den Zugriff auf die Daten Ihrer Organisation zu ermöglichen, um umfassende datengesteuerte Erfahrungen zu bieten. Unterschiedliche Berechtigungen ermöglichen der Anwendung unterschiedliche Zugriffsebenen auf die Daten Ihrer Benutzer und Ihrer Organisation.

Standardmäßig können Benutzer Anwendungen zustimmen, die auf die Daten Ihrer Organisation zugreifen, aber nur mit einigen Berechtigungen. Beispielsweise kann ein Benutzer standardmäßig zustimmen, dass eine App auf sein eigenes Postfach oder auf die Teams-Konversationen für ein Team zugreifen kann, das der Benutzer besitzt, aber er kann nicht zustimmen, dass eine App unbeaufsichtigten Zugriff für Lese- und Schreibvorgänge auf allen SharePoint-Websites in Ihrer Organisation erhält. Zwar können Benutzer durch die Möglichkeit, eine Zustimmung selbst zu erteilen, mühelos nützliche Anwendungen erwerben, die in Microsoft 365, Azure und andere Dienste integriert werden können, dies kann jedoch bei nachlässiger Nutzung und Überwachung ein Risiko darstellen.

Microsoft empfiehlt die Deaktivierung zukünftiger Vorgänge für die Benutzereinwilligung, um die Angriffsfläche und dieses Risiko zu verringern. Wenn die Benutzereinwilligung deaktiviert ist, werden vorherige Einwilligungserteilungen weiterhin berücksichtigt, aber alle zukünftigen Zustimmungsvorgänge müssen von einem Administrator ausgeführt werden. Die mandantenweite Administratoreinwilligung kann von Benutzern über einen integrierten [Workflow zum Anfordern der Administratoreinwilligung](configure-admin-consent-workflow.md) oder über Ihre eigenen Supportprozesse angefordert werden. Weitere Informationen finden Sie unter [Fünf Schritte zum Sichern Ihrer Identitätsinfrastruktur](../../security/fundamentals/steps-secure-identity.md).

## <a name="configure-user-consent-to-applications"></a>Konfigurieren der Benutzereinwilligung für Anwendungen
### <a name="disable-or-enable-user-consent-from-the-azure-portal"></a>Deaktivieren oder Aktivieren der Benutzereinwilligung im Azure-Portal

Im Azure-Portal können Sie deaktivieren oder aktivieren, ob Benutzer Anwendungen zustimmen können, die auf die Daten Ihrer Organisation zugreifen:

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory**, **Unternehmensanwendungen** und dann **Benutzereinstellungen** aus.
3. Aktivieren oder deaktivieren Sie die Benutzereinwilligung mit dem Steuerelement **Benutzer können Apps den Zugriff auf Unternehmensdaten in ihrem Namen gestatten**.
4. (Optional) Konfigurieren Sie den [Workflow zum Anfordern der Administratoreinwilligung](configure-admin-consent-workflow.md), um sicherzustellen, dass Benutzer, die einer App nicht zustimmen dürfen, eine Genehmigung anfordern können.

> [!TIP]
> Wenn Sie Benutzern das Anfordern einer Überprüfung einer Anwendung durch einen Administrator ermöglichen möchten, der der Benutzer nicht zustimmen darf (z. B. weil die Benutzereinwilligung deaktiviert wurde oder die Anwendung Berechtigungen anfordert, die der Benutzer nicht erteilen darf), sollten Sie das [Konfigurieren des Workflows für die Administratoreinwilligung](configure-admin-consent-workflow.md) in Erwägung ziehen.

### <a name="disable-or-enable-user-consent-using-powershell"></a>Deaktivieren oder Aktivieren der Benutzereinwilligung mithilfe von PowerShell

Mit dem Azure AD PowerShell V1-Modul ([MSOnline](https://docs.microsoft.com/powershell/module/msonline/?view=azureadps-1.0)) können Sie aktivieren oder deaktivieren, ob Benutzer Anwendungen zustimmen können, die auf die Daten Ihrer Organisation zugreifen.

1. Melden Sie sich bei Ihrer Organisation an, indem Sie das folgende Cmdlet ausführen:

    ```powershell
    Connect-MsolService
    ```

2. Überprüfen Sie, ob die Benutzereinwilligung aktiviert ist, indem Sie das folgende Cmdlet ausführen:

    ```powershell
    Get-MsolCompanyInformation | Format-List UsersPermissionToUserConsentToAppEnabled
    ```

3. Aktivieren oder deaktivieren Sie die Benutzereinwilligung. Führen Sie beispielsweise zum Deaktivieren der Benutzereinwilligung das folgende Cmdlet aus:

    ```powershell
    Set-MsolCompanySettings -UsersPermissionToUserConsentToAppEnabled $false
    ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Konfigurieren der Gruppenbesitzereinwilligung für Apps, die auf Gruppendaten zugreifen

> [!IMPORTANT]
> Die folgenden Informationen gelten für ein zukünftiges Feature, mit dem Gruppenbesitzer Anwendungen den Zugriff auf die Daten ihrer Gruppen erteilen können. Nach der Veröffentlichung ist diese Funktion standardmäßig aktiviert. Obwohl diese Funktion noch nicht allgemein veröffentlicht wurde, können Sie diese Anweisungen verwenden, um die Funktion vor ihrer Veröffentlichung zu deaktivieren.

Gruppenbesitzer können Anwendungen (z. B. von Drittanbietern veröffentlichte Anwendungen) autorisieren, auf die einer Gruppe zugeordneten Daten Ihrer Organisation zuzugreifen. Beispielsweise kann ein Teambesitzer (der der Besitzer der Office 365-Gruppe für das Team ist) einer App gestatten, alle Teamnachrichten des Teams zu lesen oder das Basisprofil der Mitglieder einer Gruppe aufzulisten.

> [!NOTE]
> Unabhängig von dieser Einstellung kann ein Gruppenbesitzer immer andere Benutzer oder Apps direkt als Gruppenbesitzer hinzufügen.

### <a name="configure-group-owner-consent-using-powershell"></a>Konfigurieren der Gruppenbesitzereinwilligung mithilfe von PowerShell

Mit dem Azure AD PowerShell-Vorschaumodul ([AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview)) können Sie aktivieren oder deaktivieren, ob Gruppenbesitzer Anwendungen zustimmen können, die auf die Daten Ihrer Organisation für die Gruppen zugreifen, die sie besitzen.

1. Stellen Sie sicher, dass Sie das Modul [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) verwenden (dieser Schritt ist wichtig, wenn auf Ihrem Computer sowohl das Modul [AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) als auch das Modul [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview) installiert ist).

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

2. Stellen Sie eine Verbindung mit Azure AD PowerShell her.

   ```powershell
   Connect-AzureAD
   ```

3. Rufen Sie den aktuellen Wert für die Verzeichniseinstellungen der *Zustimmungsrichtlinieneinstellungen* in Ihrem Mandanten ab. Dazu muss überprüft werden, ob die Verzeichniseinstellungen für dieses Feature erstellt wurden, und wenn dies nicht der Fall ist, müssen die Werte aus der entsprechenden Vorlage für die Verzeichniseinstellungen verwendet werden.

    ```powershell
    $consentSettingsTemplateId = "dffd5d46-495d-40a9-8e21-954ff55e198a" # Consent Policy Settings
    $settings = Get-AzureADDirectorySetting -All $true | Where-Object { $_.TemplateId -eq $consentSettingsTemplateId }

    if (-not $settings) {
        $template = Get-AzureADDirectorySettingTemplate -Id $consentSettingsTemplateId
        $settings = $template.CreateDirectorySetting()
    }

    $enabledValue = $settings.Values | ? { $_.Name -eq "EnableGroupSpecificConsent" }
    $limitedToValue = $settings.Values | ? { $_.Name -eq "ConstrainGroupSpecificConsentToMembersOfGroupId" }
    ```

4. Sie müssen die Einstellungswerte verstehen. Es gibt zwei Einstellungswerte, die definieren, welche Benutzer einer App Zugriff auf die Daten ihrer Gruppe gewähren dürfen:

    | Einstellung       | type         | BESCHREIBUNG  |
    | ------------- | ------------ | ------------ |
    | _EnableGroupSpecificConsent_   | Boolean |  Flag, das angibt, ob Gruppenbesitzer gruppenspezifische Berechtigungen erteilen dürfen. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | Wenn _EnableGroupSpecificConsent_ auf „True“ festgelegt ist und dieser Wert auf die Objekt-ID einer Gruppe festgelegt ist, werden die Mitglieder der identifizierten Gruppe autorisiert, den Gruppen, die Sie besitzen, gruppenspezifische Berechtigungen zu erteilen. |

5. Aktualisieren Sie die Einstellungswerte für die gewünschte Konfiguration:

    ```powershell
    # Disable group-specific consent entirely
    $enabledValue.Value = "False"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for all users
    $enabledValue.Value = "True"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for users in a given group
    $enabledValue.Value = "True"
    $limitedToValue.Value = "{group-object-id}"
    ```

6. Speichern Sie die Einstellungen.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren des Workflows für die Administratoreinwilligung](configure-admin-consent-workflow.md)

[Erteilen einer mandantenweiten Administratoreinwilligung für eine Anwendung](grant-admin-consent.md)

[Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](../develop/active-directory-v2-scopes.md)

[Azure AD bei Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
