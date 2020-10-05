---
title: Konfigurieren der Art der Endbenutzereinwilligung für Anwendungen in Azure AD
description: Erfahren Sie, wie Sie verwalten können, wie und wann Benutzer Anwendungen zustimmen können, die Zugriff auf die Daten Ihrer Organisation haben.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 05/19/2020
ms.author: kenwith
ms.reviewer: arvindh, luleon, phsignor
ms.openlocfilehash: 433ff5498baeb4c31473e43fc4a5d24f4ba9fd1c
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90605157"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Konfigurieren der Art und Weise, wie Endbenutzer Anwendungen zustimmen können

Sie können Ihre Anwendung in Microsoft Identity Platform integrieren, um Benutzern die Anmeldung mit ihrem Geschäfts-, Schul- oder Unikonto und Zugriff auf die Daten Ihrer Organisation zu ermöglichen, um umfassende datengesteuerte Erfahrungen zu bieten.

Bevor eine Anwendung auf die Daten Ihrer Organisation zugreifen kann, muss ein Benutzer der Anwendung eine entsprechende Berechtigungen erteilen. Unterschiedliche Berechtigungen erlauben verschiedene Zugriffsebenen. Standardmäßig sind alle Benutzer berechtigt, in Berechtigungen für Anwendungen einzuwilligen, die keine Administratoreinwilligung erfordern. Standardmäßig kann ein Benutzer beispielsweise einwilligen, dass eine App auf sein Postfach zugreifen kann, er kann aber nicht einwilligen, dass eine App uneingeschränkten Lese- und Schreibzugriff auf alle Dateien in Ihrer Organisation erhält.

Wenn Sie es Benutzern ermöglichen, Apps Zugriff auf Daten zu gewähren, können Benutzer problemlos nützliche Anwendungen abrufen und produktiv arbeiten. In einigen Situationen kann diese Konfiguration jedoch ein Risiko darstellen, wenn Sie nicht sorgfältig überwacht und gesteuert wird.

## <a name="user-consent-settings"></a>Einstellungen für die Benutzereinwilligung

Um zu steuern, in welchen Fällen Benutzer in Anwendungen einwilligen können, wählen Sie die Einwilligungsrichtlinie aus, die für alle Benutzer gelten soll. Dies sind die drei Optionen für Einwilligungsrichtlinien:

* **Benutzereinwilligung deaktivieren**: Benutzer können Anwendungen keine Berechtigungen erteilen. Benutzer können sich weiterhin bei Apps anmelden, in die sie zuvor einwilligt haben oder in die Administratoren in Ihrem Namen einwilligt haben. Sie sind jedoch nicht berechtigt, in neue Berechtigungen oder neue Apps eigenständig einzuwilligen. Nur Benutzer, denen eine Verzeichnisrolle erteilt wurde, die die Berechtigung zum Erteilen von Einwilligung enthält, können in neue Berechtigungen oder neue Apps einwilligen.

* **Benutzer können in Apps von verifizierten Herausgebern einwilligen, aber nur für Berechtigungen, die Sie auswählen (Vorschau)** : Alle Benutzer können nur in Apps einwilligen, die von einem [verifizierten Herausgeber](../develop/publisher-verification-overview.md) veröffentlicht wurden, sowie Apps, die in Ihrem Mandanten registriert sind. Benutzer können nur in die Berechtigungen einwilligen, die Sie als „geringe Auswirkung“ klassifiziert haben (auch als „geringes Risiko“ bezeichnet). Welche Anwendungen als „geringes Risiko“ angesehen werden, kann von Unternehmen zu Unternehmen stark variieren (wie etwa eine App, die die E-Mail-Adresse des Benutzers anzeigt). Daher werden die Berechtigungen mit „geringem Risiko“ vom Administrator für den Mandanten festgelegt.

  Stellen Sie sicher, dass [Berechtigungen klassifiziert werden](#configure-permission-classifications-preview), um auszuwählen, in welche Berechtigungen Benutzer einwilligen dürfen.

* **Benutzer können in alle Apps einwilligen**: Diese Option ermöglicht es allen Benutzern, für beliebige Anwendungen in eine beliebige Berechtigung einzuwilligen, die keine Administratoreinwilligung erfordert. 

   Um das Risiko zu verringern, dass böswillige Anwendungen versuchen, Benutzern den Zugriff auf Daten Ihrer Organisation zu gewähren, empfiehlt es sich, die Einwilligung des Benutzers nur für Anwendungen zuzulassen, die von einem [verifizierten Herausgeber](../develop/publisher-verification-overview.md) veröffentlicht wurden.

### <a name="configure-user-consent-settings-from-the-azure-portal"></a>Konfigurieren von Einstellungen für die Benutzereinwilligung im Azure-Portal

So konfigurieren Sie die Einstellungen für die Benutzereinwilligung über das Azure-Portal:

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Einwilligung und Berechtigungen** > **Einstellungen für Benutzereinwilligung** aus.
1. Wählen Sie unter **Benutzereinwilligung für Anwendungen** aus, welche Einwilligungseinstellung für alle Benutzer konfiguriert werden soll.
1. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

:::image type="content" source="media/configure-user-consent/setting-for-all-users.png" alt-text="Einstellungen für die Benutzereinwilligung":::

> [!TIP]
> Ziehen Sie in Betracht, den [Workflow für Administratoreinwilligung zu aktivieren](configure-admin-consent-workflow.md), wenn Sie Benutzern das Anfordern einer Überprüfung und Genehmigung einer Anwendung durch einen Administrator ermöglichen möchten, der der Benutzer nicht zustimmen darf (z. B. wenn die Benutzereinwilligung deaktiviert wurde oder die Anwendung Berechtigungen anfordert, die der Benutzer nicht erteilen darf).

### <a name="configure-user-consent-settings-using-powershell"></a>Konfigurieren von Einstellungen für Benutzereinwilligung mithilfe von PowerShell

Sie können das neueste Azure AD PowerShell-Vorschaumodul [AzureADPreview](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) verwenden, um auszuwählen, welche Einwilligungsrichtlinie die Benutzereinwilligung für Anwendungen regelt.

* **Benutzereinwilligung deaktivieren**: Um Benutzereinwilligung zu deaktivieren, legen Sie die Einwilligungsrichtlinien fest, die die Benutzereinwilligung steuern, als leer fest:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @()
  ```

* **Benutzereinwilligung für Apps von verifizierten Herausgebern für ausgewählte Berechtigungen zulassen (Vorschau)** : Um eingeschränkte Benutzereinwilligung nur für Apps von verifizierten Herausgebern und in Ihrem Mandanten registrierten Apps nur für Berechtigungen zu erlauben, die Sie als „Geringe Auswirkung“ klassifizieren, konfigurieren Sie die integrierte Einwilligungsrichtlinie mit dem Namen `microsoft-user-default-low`:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-low")
  ```

   Vergessen Sie nicht, [Berechtigungen zu klassifizieren](#configure-permission-classifications-preview), um auszuwählen, in welche Berechtigungen Benutzer einwilligen dürfen.

* **Benutzereinwilligung für alle Apps zulassen**: Ermöglicht Benutzereinwilligung für alle Apps:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-legacy")
  ```

   Diese Option ermöglicht es allen Benutzern, für beliebige Anwendungen in eine beliebige Berechtigung einzuwilligen, die keine Administratoreinwilligung erfordert. Es wird empfohlen, Benutzereinwilligung nur für Apps von verifizierten Herausgebern zuzulassen.

## <a name="configure-permission-classifications-preview"></a>Konfigurieren von Berechtigungsklassifizierungen (Vorschau)

Mit Berechtigungsklassifizierungen können Sie die Auswirkung ermitteln, die unterschiedliche Berechtigungen gemäß den Richtlinien Ihres Unternehmens und den Risikobewertungen haben. Beispielsweise können Sie Berechtigungsklassifizierungen in Einwilligungsrichtlinien verwenden, um den Berechtigungssatz zu identifizieren, dem Benutzer zustimmen dürfen.

> [!NOTE]
> Derzeit wird nur die Berechtigungsklassifizierung „Geringe Auswirkung“ unterstützt. Nur delegierte Berechtigungen, für die keine Administratoreinwilligung erforderlich ist, können als „Geringe Auswirkung“ klassifiziert werden.

### <a name="classify-permissions-using-the-azure-portal"></a>Klassifizieren von Berechtigungen mithilfe des Azure-Portals

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Einwilligung und Berechtigungen** > **Berechtigungsklassifizierungen** aus.
1. Wählen Sie **Berechtigungen hinzufügen** aus, um eine weitere Berechtigung als „Geringe Auswirkung“ zu klassifizieren. 
1. Wählen Sie die API und dann die delegierten Berechtigungen aus.

In diesem Beispiel haben wir den minimalen Berechtigungssatz klassifiziert, der für einmaliges Anmelden erforderlich ist:

:::image type="content" source="media/configure-user-consent/permission-classifications.png" alt-text="Einstellungen für die Benutzereinwilligung":::

> [!TIP]
> Für die Microsoft Graph-API sind die minimalen Berechtigungen, die für einfaches einmaliges Anmelden erforderlich sind, `openid`, `profile`, `User.Read` und `offline_access`. Mit diesen Berechtigungen kann eine App die Profildetails des angemeldeten Benutzers lesen und diesen Zugriff auch dann beibehalten, wenn der Benutzer die App nicht mehr verwendet.

### <a name="classify-permissions-using-powershell"></a>Klassifizieren von Berechtigungen mithilfe von PowerShell

Sie können das neueste Azure AD PowerShell-Vorschaumodul [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true) verwenden, um Berechtigungen zu klassifizieren. Berechtigungsklassifizierungen werden für das **ServicePrincipal**-Objekt der API konfiguriert, die die Berechtigungen veröffentlicht.

#### <a name="to-read-the-current-permission-classifications-for-an-api"></a>So lesen Sie die aktuellen Berechtigungsklassifizierungen für eine API:

1. Rufen Sie das **ServicePrincipal**-Objekt für die API ab. Hier rufen wir das ServicePrincipal-Objekt für die Microsoft Graph-API ab:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Lesen Sie die Klassifizierungen für delegierte Berechtigungen für die API:

   ```powershell
   Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId | Format-Table Id, PermissionName, Classification
   ```

#### <a name="to-classify-a-permission-as-low-impact"></a>So klassifizieren Sie eine Berechtigung als „Geringe Auswirkung“:

1. Rufen Sie das **ServicePrincipal**-Objekt für die API ab. Hier rufen wir das ServicePrincipal-Objekt für die Microsoft Graph-API ab:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Suchen Sie nach der delegierten Berechtigung, die Sie klassifizieren möchten:

   ```powershell
   $delegatedPermission = $api.OAuth2Permissions | Where-Object { $_.Value -eq "User.ReadBasic.All" }
   ```

1. Legen Sie die Berechtigungsklassifizierung mithilfe des Berechtigungsnamens und der ID fest:

   ```powershell
   Add-AzureADMSServicePrincipalDelegatedPermissionClassification `
      -ServicePrincipalId $api.ObjectId `
      -PermissionId $delegatedPermission.Id `
      -PermissionName $delegatedPermission.Value `
      -Classification "low"
   ```

#### <a name="to-remove-a-delegated-permission-classification"></a>So entfernen Sie eine Klassifizierung für eine delegierte Berechtigung:

1. Rufen Sie das **ServicePrincipal**-Objekt für die API ab. Hier rufen wir das ServicePrincipal-Objekt für die Microsoft Graph-API ab:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Suchen Sie nach der Klassifizierung für eine delegierte Berechtigung, die Sie entfernen möchten:

   ```powershell
   $classifications = Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId
   $classificationToRemove = $classifications | Where-Object {$_.PermissionName -eq "User.ReadBasic.All"}
   ```

1. Löschen Sie die Berechtigungsklassifizierung:

   ```powershell
   Remove-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId `
       -Id $classificationToRemove.Id
   ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Konfigurieren der Gruppenbesitzereinwilligung für Apps, die auf Gruppendaten zugreifen

Gruppenbesitzer können Anwendungen (z. B. von Drittanbietern veröffentlichte Anwendungen) autorisieren, auf die einer Gruppe zugeordneten Daten Ihrer Organisation zuzugreifen. Beispielsweise kann ein Teambesitzer in Microsoft Teams einer App gestatten, alle Teamnachrichten des Teams zu lesen oder das Basisprofil der Mitglieder einer Gruppe aufzulisten.

Sie können konfigurieren, welche Benutzer Apps in den Zugriff auf die Daten ihrer Gruppen einwilligen dürfen, oder Sie können diese Funktion deaktivieren.

### <a name="configure-group-owner-consent-using-the-azure-portal"></a>Konfigurieren der Gruppenbesitzereinwilligung mithilfe des Azure-Portals

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Einwilligung und Berechtigungen** > **Einstellungen für Benutzereinwilligung** aus.
3. Wählen Sie unter **Gruppenbesitzereinwilligung für Apps für Datenzugriff** die Option aus, die Sie aktivieren möchten.
4. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

In diesem Beispiel können alle Gruppenbesitzer einwilligen, dass Apps auf die Daten ihrer Gruppen zugreifen:

:::image type="content" source="media/configure-user-consent/group-owner-consent.png" alt-text="Einstellungen für die Benutzereinwilligung":::

### <a name="configure-group-owner-consent-using-powershell"></a>Konfigurieren der Gruppenbesitzereinwilligung mithilfe von PowerShell

Mit dem Azure AD PowerShell-Vorschaumodul ([AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true)) können Sie aktivieren oder deaktivieren, ob Gruppenbesitzer in Anwendungen einwilligen können, die auf die Daten Ihrer Organisation für die Gruppen zugreifen, die sie besitzen.

1. Stellen Sie sicher, dass Sie das Modul [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true) verwenden. Dieser Schritt ist wichtig, wenn auf Ihrem Computer sowohl das Modul [AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0&preserve-view=true) als auch das Modul [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true) installiert ist.

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

1. Stellen Sie eine Verbindung mit Azure AD PowerShell her.

   ```powershell
   Connect-AzureAD
   ```

1. Rufen Sie den aktuellen Wert für die Verzeichniseinstellungen der **Zustimmungsrichtlinieneinstellungen** in Ihrem Mandanten ab. Dazu muss überprüft werden, ob die Verzeichniseinstellungen für dieses Feature erstellt wurden, und wenn dies nicht der Fall ist, müssen die Werte aus der entsprechenden Vorlage für die Verzeichniseinstellungen verwendet werden.

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

1. Sie müssen die Einstellungswerte verstehen. Es gibt zwei Einstellungswerte, die definieren, welche Benutzer einer App Zugriff auf die Daten ihrer Gruppe gewähren dürfen:

    | Einstellung       | Typ         | BESCHREIBUNG  |
    | ------------- | ------------ | ------------ |
    | _EnableGroupSpecificConsent_   | Boolean | Flag, das angibt, ob Gruppenbesitzer gruppenspezifische Berechtigungen erteilen dürfen. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | Wenn _EnableGroupSpecificConsent_ auf TRUE und dieser Wert auf die Objekt-ID einer Gruppe festgelegt ist, werden die Mitglieder der identifizierten Gruppe autorisiert, den Gruppen, die Sie besitzen, gruppenspezifische Berechtigungen zu erteilen. |

1. Aktualisieren Sie die Einstellungswerte für die gewünschte Konfiguration:

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

1. Speichern Sie die Einstellungen.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="configure-risk-based-step-up-consent"></a>Konfigurieren der risikobasierten hochgestuften Einwilligung

Die risikobasierte hochgestufte Einwilligung trägt zur Reduzierung der Benutzergefährdung durch böswillige Apps bei, die [unrechtmäßige Einwilligungsanforderungen](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants) ausgeben. Wenn Microsoft eine risikobehaftete Anforderung der Endbenutzereinwilligung erkennt, wird die Anforderung stattdessen auf Administratoreinwilligung „hochgestuft“. Diese Funktion ist standardmäßig aktiviert, führt jedoch nur zu einer Änderung des Verhaltens, wenn die Endbenutzereinwilligung aktiviert ist.

Wird eine risikobehaftete Einwilligungsanforderung erkannt, wird in der Einwilligungsaufforderung eine Meldung angezeigt, die besagt, dass eine Administratorgenehmigung erforderlich ist. Wenn der [Workflow zum Anfordern der Administratoreinwilligung](configure-admin-consent-workflow.md) aktiviert ist, kann der Benutzer die Anforderung zur weiteren Überprüfung direkt von der Einwilligungsaufforderung an einen Administrator senden. Ist diese Funktion nicht aktiviert, wird die folgende Meldung angezeigt:

* **AADSTS90094:** &lt;Anzeigename der Client-App&gt; benötigt eine Berechtigung zum Zugriff auf Ressourcen in Ihrer Organisation, die nur ein Administrator erteilen kann. Bitten Sie einen Administrator, dieser App Berechtigungen zu erteilen, bevor Sie sie verwenden können.

In diesem Fall wird auch ein Überwachungsereignis der Kategorie „ApplicationManagement“ mit dem Aktivitätstyp „Einwilligung in Anwendung“ und dem Statusgrund „Riskante Anwendung erkannt“ protokolliert.

> [!IMPORTANT]
> Administratoren sollten vor der Genehmigung einer Anforderung [alle Einwilligungsanforderungen sorgfältig auswerten](manage-consent-requests.md#evaluating-a-request-for-tenant-wide-admin-consent), besonders wenn Microsoft ein Risiko erkannt hat.

### <a name="disable-or-re-enable-risk-based-step-up-consent-using-powershell"></a>Deaktivieren oder erneutes Aktivieren der risikobasierten hochgestuften Einwilligung mit PowerShell

Mit dem Modul „Azure AD PowerShell Preview“ ([AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true)) können Sie die Hochstufung auf Administratoreinwilligung deaktivieren (wenn Microsoft Risiken erkennt) oder erneut aktivieren, wenn sie zuvor deaktiviert wurde.

Hierfür können Sie die gleichen Schritte verwenden, die weiter oben unter [Konfigurieren der Gruppenbesitzereinwilligung mithilfe von PowerShell](#configure-group-owner-consent-using-powershell) beschrieben wurden, müssen aber einen anderen Einstellungswert ersetzen. Die Schritte unterscheiden sich in drei Punkten: 

1. Machen Sie sich mit den Einstellungswerten für die risikobasierte hochgestufte Einwilligung vertraut:

    | Einstellung       | Typ         | BESCHREIBUNG  |
    | ------------- | ------------ | ------------ |
    | _BlockUserConsentForRiskyApps_   | Boolean |  Flag, das angibt, ob die Benutzereinwilligung beim Erkennen einer risikobehafteten Anforderung blockiert wird. |

1. Ersetzen Sie in Schritt 3 den folgenden Wert:

    ```powershell
    $riskBasedConsentEnabledValue = $settings.Values | ? { $_.Name -eq "BlockUserConsentForRiskyApps" }
    ```
    
1. Ersetzen Sie in Schritt 5 einen der folgenden Werte:

    ```powershell
    # Disable risk-based step-up consent entirely
    $riskBasedConsentEnabledValue.Value = "False"
    ```

    ```powershell
    # Re-enable risk-based step-up consent, if disabled previously
    $riskBasedConsentEnabledValue.Value = "True"
    ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

* [Konfigurieren des Workflows für die Administratoreinwilligung (Vorschau)](configure-admin-consent-workflow.md)
* Weitere Informationen zum [Verwalten der Einwilligung zu Anwendungen und Auswerten von Einwilligungsanforderungen](manage-consent-requests.md)
* [Erteilen einer mandantenweiten Administratoreinwilligung für eine Anwendung](grant-admin-consent.md)
* [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](../develop/active-directory-v2-scopes.md)

So erhalten Sie Hilfe oder finden Antworten auf Ihre Fragen:
* [Azure AD bei StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
