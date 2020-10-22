---
title: Verwalten eines ausführenden Azure Automation-Kontos
description: In diesem Artikel wird beschrieben, wie Sie mit PowerShell oder über das Azure-Portal Ihr ausführendes Konto verwalten.
services: automation
ms.subservice: shared-capabilities
ms.date: 09/28/2020
ms.topic: conceptual
ms.openlocfilehash: 0849eb0c421883ecb0510451ff81b604538c9cc3
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92069890"
---
# <a name="manage-an-azure-automation-run-as-account"></a>Verwalten eines ausführenden Azure Automation-Kontos

Ausführende Konten in Azure Automation ermöglichen die Authentifizierung für die Verwaltung von Ressourcen mit dem Azure Resource Manager- oder dem klassischen Azure-Bereitstellungsmodell mithilfe von Automation-Runbooks und anderen Automation-Features. Dieser Artikel enthält eine Anleitung zur Verwaltung eines ausführenden oder klassischen ausführenden Kontos.

Weitere Informationen zur Azure Automation-Kontoauthentifizierung sowie zu Szenarien der Prozessautomatisierung finden Sie unter [Übersicht über die Automation-Kontoauthentifizierung](automation-security-overview.md).

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Berechtigungen des ausführenden Kontos

In diesem Abschnitt werden Berechtigungen sowohl für normale ausführende Konten als auch für klassische ausführende Konten definiert.

Um ein ausführendes Konto erstellen oder aktualisieren zu können, müssen Sie über bestimmte Rechte und Berechtigungen verfügen. Ein Anwendungsadministrator in Azure Active Directory und ein Besitzer in einem Abonnement können sämtliche Aufgaben erledigen. In der folgenden Tabelle werden die Aufgaben, das entsprechende Cmdlet und die erforderlichen Berechtigungen für Situationen mit Aufgabentrennung aufgeführt:

|Aufgabe|Cmdlet  |Mindestberechtigungen  |Hier legen Sie die Berechtigungen fest|
|---|---------|---------|---|
|Erstellen einer Azure AD-Anwendung|[New-AzADApplication](/powershell/module/az.resources/new-azadapplication)     | Rolle „Anwendungsentwickler“<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Start > Azure AD > App-Registrierungen |
|Hinzufügen von Anmeldeinformationen zur Anwendung|[New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential)     | „Anwendungsadministrator“ oder „Globaler Administrator“<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Start > Azure AD > App-Registrierungen|
|Erstellen und Abrufen eines Azure AD-Dienstprinzipals|[New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal)</br>[Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)     | „Anwendungsadministrator“ oder „Globaler Administrator“<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Start > Azure AD > App-Registrierungen|
|Zuweisen oder Abrufen der Azure-Rolle für den angegebenen Prinzipal|[New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment)</br>[Get-AzRoleAssignment](/powershell/module/Az.Resources/Get-AzRoleAssignment)      | „Benutzerzugriffsadministrator“ oder „Besitzer“, oder muss die folgenden Berechtigungen besitzen:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Abonnement](../role-based-access-control/role-assignments-portal.md)</br>Start > Abonnements > \<subscription name\> -Access Control (IAM)|
|Erstellen oder Entfernen eines Automation-Zertifikats|[New-AzAutomationCertificate](/powershell/module/Az.Automation/New-AzAutomationCertificate)</br>[Remove-AzAutomationCertificate](/powershell/module/az.automation/remove-azautomationcertificate)     | Mitwirkender der Ressourcengruppe         |Ressourcengruppe des Automation-Kontos|
|Erstellen oder Entfernen einer Automation-Verbindung|[New-AzAutomationConnection](/powershell/module/az.automation/new-azautomationconnection)</br>[Remove-AzAutomationConnection](/powershell/module/az.automation/remove-azautomationconnection)|Mitwirkender der Ressourcengruppe |Ressourcengruppe des Automation-Kontos|

<sup>1</sup> Benutzer Ihres Azure AD-Mandanten, die keine Administratoren sind, können [AD-Anwendungen registrieren](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app), wenn die Option **Benutzer können Anwendungen registrieren** des Azure AD-Mandanten auf der Seite „Benutzereinstellungen“ auf **Ja** festgelegt ist. Wenn die Anwendungsregistrierungseinstellung **Nein** lautet, muss der Benutzer, der diese Aktion ausführt, eine der in dieser Tabelle definierten Rollen besitzen.

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Globaler Administrator“ des Abonnements hinzugefügt werden, werden Sie als Gast hinzugefügt. In diesem Fall wird auf der Seite **Automation-Konto hinzufügen** eine Warnung vom Typ `You do not have permissions to create…` angezeigt.

Wenn Sie Mitglied der Active Directory-Instanz des Abonnements sind, in der die Rolle „Globaler Administrator“ zugewiesen wird, können Sie auch die Warnung `You do not have permissions to create…` auf der Seite **Automation-Konto hinzufügen** erhalten. In diesem Fall können Sie die Entfernung aus der Active Directory-Instanz des Abonnements anfordern und dann anfordern, erneut hinzugefügt zu werden, damit Sie ein Vollbenutzer in Active Directory werden.

So überprüfen Sie, ob die Situation, die die Fehlermeldung erzeugt, behoben wurde

1. Wählen Sie im Azure-Portal im Bereich „Azure Active Directory“ **Benutzer und Gruppen** aus.
2. Wählen Sie **Alle Benutzer**.
3. Wählen Sie Ihren Namen aus, und wählen Sie dann **Profil** aus.
4. Stellen Sie sicher, dass der Wert des Attributs **Benutzertyp** im Profil des Benutzers nicht auf **Gast** festgelegt ist.

### <a name="permissions-required-to-create-or-manage-classic-run-as-accounts"></a><a name="permissions-classic"></a>Erforderliche Berechtigungen zum Erstellen oder Verwalten klassischer ausführender Konten

Zum Konfigurieren oder Erneuern klassischer ausführender Konten benötigen Sie die Rolle „Co-Administrator“ auf Abonnementebene. Weitere Informationen zu klassischen Abonnementberechtigungen finden Sie unter [Administratoren für klassische Azure-Abonnements](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-azure-portal"></a>Erstellen eines ausführenden Kontos im Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihr Azure Automation-Konto im Azure-Portal zu aktualisieren. Erstellen Sie das ausführende Konto und das klassische ausführende Konto separat. Wenn Sie keine klassischen Ressourcen verwalten müssen, ist es nur erforderlich, das ausführende Azure-Konto zu erstellen.

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.

2. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.

3. Wählen Sie auf der Seite „Automation-Konten“ in der entsprechenden Liste Ihr Automation-Konto aus.

4. Wählen Sie im linken Bereich im Abschnitt **Kontoeinstellungen** die Option **Ausführende Konten** aus.

    :::image type="content" source="media/manage-runas-account/automation-account-properties-pane.png" alt-text="Auswählen der Option „Ausführendes Konto“":::

5. Abhängig vom gewünschten Konto verwenden Sie den Bereich **+ Ausführendes Azure-Konto** oder **+ Klassisches ausführendes Azure-Konto**. Klicken Sie nach dem Überprüfen der Übersichtsinformationen auf **Erstellen**.

    :::image type="content" source="media/manage-runas-account/automation-account-create-runas.png" alt-text="Auswählen der Option „Ausführendes Konto“":::

6. Während das ausführende Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen. Es wird auch ein Banner mit dem Hinweis angezeigt, dass das Konto erstellt wird. Der Vorgang kann einige Minuten dauern.

## <a name="create-a-run-as-account-using-powershell"></a>Erstellen eines ausführenden Kontos mit PowerShell

In der folgenden Liste sind die Anforderungen zum Erstellen eines ausführenden Kontos in PowerShell mithilfe eines bereitgestellten Skripts aufgeführt. Diese Anforderungen gelten für beide Typen von ausführenden Konten.

* Windows 10 oder Windows Server 2016 mit Azure Resource Manager-Modul 3.4.1 und höher. Das PowerShell-Skript unterstützt keine früheren Versionen von Windows.
* Azure PowerShell 6.2.4 oder höher. Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps).
* Ein Automation-Konto, auf das als Wert für die Parameter `AutomationAccountName` und `ApplicationDisplayName` verwiesen wird.
* Berechtigungen, die den unter [Berechtigungen zum Konfigurieren von ausführenden Konten](#permissions) aufgelisteten entsprechen.

Führen Sie die folgenden Schritte aus, um die Werte für `AutomationAccountName`, `SubscriptionId` und `ResourceGroupName` abzurufen. Dies sind erforderliche Parameter für das PowerShell-Skript.

1. Wählen Sie im Azure-Portal **Automation-Konten** aus.

1. Wählen Sie auf der Seite „Automation-Konten“ Ihr Automation-Konto aus.

1. Wählen Sie im Abschnitt „Einstellungen“ die Option **Eigenschaften** aus.

1. Notieren Sie sich auf der Seite **Eigenschaften** die Werte für **Name**, **Abonnement-ID** und **Ressourcengruppe**.

   ![Seite „Eigenschaften“ des Automation-Kontos](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>PowerShell-Skript zum Erstellen eines ausführenden Kontos

Das PowerShell-Skript unterstützt mehrere Konfigurationen.

* Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen Sie ein ausführendes Konto und ein klassisches ausführendes Konto, indem Sie ein Zertifikat nutzen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

1. Laden Sie das Skript herunter, und speichern Sie es mit dem folgenden Befehl in einem lokalen Ordner.

    ```powershell
    wget https://raw.githubusercontent.com/azureautomation/runbooks/master/Utility/AzRunAs/Create-RunAsAccount.ps1 -outfile Create-RunAsAccount.ps1
    ```

2. Starten Sie PowerShell mit erhöhten Benutzerrechten, und navigieren Sie zu dem Ordner mit dem Skript.

3. Führen Sie einen der folgenden Befehle aus, um entsprechend Ihren Anforderungen ein ausführendes Konto und/oder ein klassisches ausführendes Konto zu erstellen.

    * Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
        ```

    * Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
        ```

    * Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
        ```

        Wenn Sie ein klassisches ausführendes Konto mit einem öffentlichen Unternehmenszertifikat (CER-Datei) erstellt haben, sollten Sie dieses Zertifikat verwenden. Das Skript erstellt und speichert es im Ordner für temporäre Dateien auf Ihrem Computer unter dem Benutzerprofil `%USERPROFILE%\AppData\Local\Temp`, das Sie zum Ausführen der PowerShell-Sitzung verwendet haben. Siehe [Hochladen eines Verwaltungs-API-Zertifikats in das Azure-Portal](../cloud-services/cloud-services-configure-ssl-certificate-portal.md).

    * Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnvironmentName AzureUSGovernment
        ```

4. Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich mit einem Konto an, das Mitglied der Rolle „Abonnement-Administratoren“ ist. Wenn Sie ein klassisches ausführendes Konto erstellen, muss das Konto Co-Admin des Abonnements sein.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos

In diesem Abschnitt wird beschrieben, wie Sie ein ausführendes Konto oder klassisches ausführendes Konto löschen. Bei dieser Aktion wird das Automation-Konto beibehalten. Nachdem Sie das ausführende Konto gelöscht haben, können Sie es im Azure-Portal oder mit dem bereitgestellten PowerShell-Skript neu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie im linken Bereich im Abschnitt „Kontoeinstellungen“ **Ausführende Konten** aus.

3. Wählen Sie auf der Eigenschaftenseite „Ausführende Konten“ entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten.

4. Klicken Sie auf der Seite „Eigenschaften“ für das ausgewählte Konto auf **Löschen**.

   ![Löschen des ausführenden Azure-Kontos](media/manage-runas-account/automation-account-delete-runas.png)

5. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

## <a name="renew-a-self-signed-certificate"></a><a name="cert-renewal"></a>Verlängern eines selbstsignierten Zertifikats

Das selbstsignierte Zertifikat, das Sie für das ausführende Konto erstellt haben, läuft ein Jahr nach dem Erstellungsdatum ab. Das Zertifikat muss vor Ablauf Ihres ausführenden Kontos erneuert werden. Sie können es vor dem Ablaufdatum jederzeit erneuern.

Beim Erneuern des selbstsignierten Zertifikats wird das aktuell gültige Zertifikat beibehalten, um sicherzustellen, dass sich keine negativen Auswirkungen auf Runbooks ergeben, die sich in der Warteschlange befinden oder aktiv ausgeführt werden und die mit dem ausführenden Konto authentifiziert werden. Das Zertifikat bleibt bis zum Ablaufdatum gültig.

>[!NOTE]
>Wenn Sie der Meinung sind, dass das ausführende Konto kompromittiert wurde, können Sie das selbstsignierte Zertifikat löschen und neu erstellen.

>[!NOTE]
>Wenn Sie Ihr ausführendes Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und Sie verwenden die Option zum Erneuern eines selbstsignierten Zertifikats, wird das Unternehmenszertifikat durch ein selbstsigniertes Zertifikat ersetzt.

Verwenden Sie die folgenden Schritte, um das selbstsignierte Zertifikat zu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

1. Wählen Sie im Abschnitt mit den Kontoeinstellungen **Ausführende Konten** aus.

    ![Eigenschaftenbereich des Automation-Kontos](media/manage-runas-account/automation-account-properties-pane.png)

1. Wählen Sie auf der Eigenschaftenseite „Ausführende Konten“ entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten.

1. Klicken Sie im Bereich „Eigenschaften“ für das ausgewählte Konto auf **Zertifikat erneuern**.

    ![Erneuern des Zertifikats für das ausführende Konto](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

## <a name="limit-run-as-account-permissions"></a>Einschränken der Berechtigungen des ausführenden Kontos

Zum Steuern der Automatisierungsziele für Ressourcen in Azure können Sie das Skript [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8)ausführen. Dieses Skript ändert Ihren vorhandenen Dienstprinzipal für das ausführende Konto, um eine benutzerdefinierte Rollendefinition zu erstellen und zu verwenden. Die Rolle verfügt über Berechtigungen für alle Ressourcen außer [Key Vault](../key-vault/index.yml).

>[!IMPORTANT]
>Nachdem Sie das Skript **Update-AutomationRunAsAccountRoleAssignments.ps1** ausgeführt haben, funktionieren Runbooks, die unter Verwendung von ausführenden Konten auf Key Vault zugreifen, nicht mehr. Überprüfen Sie vor dem Ausführen des Skripts Runbooks in Ihrem Konto auf Aufrufe von Azure Key Vault. Um den Zugriff auf Key Vault aus Azure Automation-Runbooks zu aktivieren, müssen Sie [das ausführende Konto den Berechtigungen von Key Vault hinzufügen](#add-permissions-to-key-vault).

Wenn Sie noch weiter einschränken müssen, was der Dienstprinzipal des ausführenden Kontos darf, können Sie dem `NotActions`-Element der benutzerdefinierten Rollendefinition weitere Ressourcentypen hinzufügen. Im folgenden Beispiel wird der Zugriff auf `Microsoft.Compute/*` eingeschränkt. Wenn Sie diesen Ressourcentyp zu `NotActions` für die Rollendefinition hinzufügen, kann die Rolle auf keine Compute-Ressource zugreifen. Weitere Informationen zu Rollendefinitionen finden Sie unter [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Sie können bestimmen, ob sich der Dienstprinzipal, der von Ihrem ausführenden Konto verwendet wird, in der Rollendefinition „Mitwirkender“ oder in einer benutzerdefinierten befindet.

1. Wechseln Sie zu Ihrem Automation-Konto, und wählen Sie im Abschnitt mit den Kontoeinstellungen **Ausführende Konten** aus.
2. Wählen Sie **Ausführendes Azure-Konto** aus.
3. Wählen Sie **Rolle** aus, um die Rollendefinition zu suchen, die verwendet wird.

:::image type="content" source="media/manage-runas-account/verify-role.png" alt-text="Auswählen der Option „Ausführendes Konto“" lightbox="media/manage-runas-account/verify-role-expanded.png":::

Sie können auch die Rollendefinition bestimmen, die von den ausführenden Konten für mehrere Abonnements oder Automation-Konten verwendet wird. Verwenden Sie hierzu das Skript [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) im PowerShell-Katalog.

### <a name="add-permissions-to-key-vault"></a>Hinzufügen von Berechtigungen zu Key Vault

Sie können Azure Automation gestatten, zu überprüfen, ob Key Vault und Ihr Dienstprinzipal des ausführenden Kontos eine benutzerdefinierte Rollendefinition verwenden. Die Voraussetzungen lauten wie folgt:

* Gewähren von Berechtigungen für Key Vault.
* Festlegen der Zugriffsrichtlinie.

Sie können das Skript [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) im PowerShell-Katalog verwenden, um Ihrem ausführenden Konto Berechtigungen für KeyVault zu erteilen. Weitere Einzelheiten zum Festlegen von Berechtigungen für Key Vault finden Sie unter [Zuweisen einer Key Vault-Zugriffsrichtlinie](../key-vault/general/assign-access-policy-powershell.md).

## <a name="resolve-misconfiguration-issues-for-run-as-accounts"></a>Beheben von Fehlkonfigurationsproblemen bei ausführenden Konten

Einige Konfigurationselemente, die für ein ausführendes oder ein klassisches ausführendes Konto erforderlich sind, können eventuell gelöscht oder während der anfänglichen Einrichtung nicht ordnungsgemäß erstellt worden sein. Mögliche Instanzen für Fehlkonfigurationen können sein:

* Zertifikatasset
* Verbindungsasset
* Ausführendes Kontos wurde aus der Rolle „Mitwirkender“ entfernt.
* Dienstprinzipal oder Anwendung in Azure AD

Bei solchen Fehlkonfigurationsinstanzen erkennt das Automation-Konto die Änderungen und zeigt im Eigenschaftenbereich des ausführenden Kontos den Status *Unvollständig* für das Konto an.

![Konfigurationsstatus „Unvollständig“ für ausführendes Konto](media/manage-runas-account/automation-account-runas-config-incomplete.png)

Wenn Sie das ausführende Konto auswählen, wird im Bereich „Eigenschaften“ die folgende Fehlermeldung angezeigt:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Sie können diese Probleme mit ausführenden Konten schnell beheben, indem Sie das ausführende Konto löschen und neu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/develop/app-objects-and-service-principals.md)
* [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)