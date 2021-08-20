---
title: Erstellen eines Mandanten in Azure Virtual Desktop (klassisch) – Azure
description: Hier erfahren Sie, wie Sie in Azure Active Directory Mandanten für Azure Virtual Desktop (klassisch) einrichten.
author: Heidilohr
ms.topic: tutorial
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: f828ce5c246103462ca29066779468c7526e63ee
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111751809"
---
# <a name="tutorial-create-a-tenant-in-azure-virtual-desktop-classic"></a>Tutorial: Erstellen eines Mandanten in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager.

Die Erstellung eines Mandanten in Azure Virtual Desktop ist der erste Schritt bei der Entwicklung Ihrer Desktopvirtualisierungslösung. Ein Mandant umfasst eine Gruppe mit mindestens einem Hostpool. Jeder Hostpool besteht aus mehreren Sitzungshosts, die als virtuelle Computer in Azure ausgeführt und beim Azure Virtual Desktop-Dienst registriert werden. Darüber hinaus umfasst jeder Hostpool mindestens eine App-Gruppe, die verwendet wird, um Remotedesktop- und Remoteanwendungsressourcen für Benutzer zu veröffentlichen. Mit einem Mandanten können Sie Hostpools und App-Gruppen erstellen, Benutzer zuweisen und Verbindungen über den Dienst herstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erteilen von Azure Active Directory-Berechtigungen für den Azure Virtual Desktop-Dienst
> * Zuweisen der Anwendungsrolle „TenantCreator“ für einen Benutzer in Ihrem Azure Active Directory-Mandanten
> * Erstellen eines Azure Virtual Desktop-Mandanten

## <a name="what-you-need-to-set-up-a-tenant"></a>Erforderliche Komponenten zum Einrichten eines Mandanten

Bevor Sie mit der Einrichtung Ihres Azure Virtual Desktop-Mandanten beginnen, stellen Sie sicher, dass Sie über Folgendes verfügen:

* Die [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)-Mandanten-ID für Azure Virtual Desktop-Benutzer
* Ein globales Administratorkonto im Azure Active Directory-Mandanten.
   * Dies gilt auch für CSP-Organisationen (Cloud Solution Provider), die einen Azure Virtual Desktop-Mandanten für ihre Kunden erstellen. Wenn Sie einer CSP-Organisation angehören, müssen Sie sich als globaler Administrator der Azure Active Directory-Instanz des Kunden anmelden können.
   * Das Administratorkonto muss aus dem Azure Active Directory-Mandanten stammen, in dem Sie den Azure Virtual Desktop-Mandanten erstellen möchten. Azure Active Directory B2B-Konten (Gastkonten) werden bei diesem Prozess nicht unterstützt.
   * Das Administratorkonto muss ein Geschäfts-, Schul- oder Unikonto sein.
* Ein Azure-Abonnement.

Sie müssen die Mandanten-ID, das globale Administratorkonto und das Azure-Abonnement bereithalten, damit das in diesem Tutorial beschriebene Verfahren ordnungsgemäß funktioniert.

## <a name="grant-permissions-to-azure-virtual-desktop"></a>Erteilen von Berechtigungen für Azure Virtual Desktop

Wenn Sie Azure Virtual Desktop bereits Berechtigungen für diese Azure Active Directory-Instanz erteilt haben, überspringen Sie diesen Abschnitt.

Wenn Sie dem Azure Virtual Desktop-Dienst diese Berechtigungen erteilen, können Sie Azure Active Directory für Verwaltungs- und Endbenutzeraufgaben abfragen.

Gewähren Sie die Dienstberechtigungen wie folgt:

1. Öffnen Sie einen Browser, und starten Sie den Administratoreinwilligungsflow für die [Azure Virtual Desktop-Server-App](https://login.microsoftonline.com/common/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
   > [!NOTE]
   > Wenn Sie einen Kunden verwalten und die Administratoreinwilligung für das Verzeichnis des Kunden erteilen müssen, geben Sie die folgende URL in den Browser ein. Ersetzen Sie dabei „{tenant}“ durch den Azure AD-Domänennamen des Kunden. Hat die Organisation des Kunden beispielsweise den Azure AD-Domänennamen „contoso.onmicrosoft.com“ registriert, ersetzen Sie „{tenant}“ durch „contoso.onmicrosoft.com“.
   >```
   >https://login.microsoftonline.com/{tenant}/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback
   >```

2. Melden Sie sich auf der Azure Virtual Desktop-Seite für die Einwilligung mit dem Konto eines globalen Administrators an. Wenn Sie für die Organisation Contoso arbeiten, lautet Ihr Kontoname beispielsweise admin@contoso.com oder admin@contoso.onmicrosoft.com.
3. Wählen Sie **Akzeptieren** aus.
4. Warten Sie eine Minute, damit Azure AD die Zustimmung aufzeichnen kann.
5. Öffnen Sie einen Browser, und starten Sie den Administratoreinwilligungsflow für die [Azure Virtual Desktop-Client-App](https://login.microsoftonline.com/common/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback).
   >[!NOTE]
   > Wenn Sie einen Kunden verwalten und die Administratoreinwilligung für das Verzeichnis des Kunden erteilen müssen, geben Sie die folgende URL in den Browser ein. Ersetzen Sie dabei „{tenant}“ durch den Azure AD-Domänennamen des Kunden. Hat die Organisation des Kunden beispielsweise den Azure AD-Domänennamen „contoso.onmicrosoft.com“ registriert, ersetzen Sie „{tenant}“ durch „contoso.onmicrosoft.com“.
   >```
   > https://login.microsoftonline.com/{tenant}/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback
   >```

6. Melden Sie sich wie in Schritt 2 auf der Azure Virtual Desktop-Seite für die Einwilligung als globaler Administrator an.
7. Wählen Sie **Akzeptieren** aus.

## <a name="assign-the-tenantcreator-application-role"></a>Zuweisen der Anwendungsrolle „TenantCreator“

Wenn die Anwendungsrolle „TenantCreator“ einem Azure Active Directory-Benutzer zugewiesen wird, kann dieser einen Azure Virtual Desktop-Mandanten erstellen, der der entsprechenden Azure Active Directory-Instanz zugeordnet ist. Sie müssen Ihr globales Administratorkonto verwenden, um die Rolle „TenantCreator“ zuzuweisen.

So weisen Sie die Anwendungsrolle „TenantCreator“ zu

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um die Anwendungsrolle „TenantCreator“ zu verwalten. Suchen Sie nach **Unternehmensanwendungen**, und wählen Sie die entsprechende Option aus. Wenn Sie mehrere Azure Active Directory-Mandanten verwenden, empfiehlt es sich, eine private Browsersitzung zu öffnen, die URLs zu kopieren und auf der Adressleiste einzufügen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Suchen nach Unternehmensanwendungen im Azure-Portal](../media/azure-portal-enterprise-applications.png)

2. Suchen Sie unter **Unternehmensanwendungen** nach **Azure Virtual Desktop**. Die beiden Anwendungen, für die Sie im vorherigen Abschnitt die Einwilligung erteilt haben, werden angezeigt. Wählen Sie bei diesen beiden Apps **Azure Virtual Desktop** aus.

3. Wählen Sie **Benutzer und Gruppen**. Unter Umständen wird der Administrator, der die Zustimmung für die Anwendung erteilt hat, bereits mit der zugewiesenen Rolle **Standardzugriff** aufgeführt. Für die Erstellung eines Azure Virtual Desktop-Mandanten ist dies jedoch nicht ausreichend. Führen Sie die weiteren Schritte dieser Anleitung aus, um einem Benutzer die Rolle **TenantCreator** hinzuzufügen.

4. Wählen Sie die Schaltfläche **Benutzer hinzufügen** und anschließend auf der Registerkarte **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
5. Suchen Sie nach einem Benutzerkonto, das zum Erstellen Ihres Azure Virtual Desktop-Mandanten verwendet wird. Der Einfachheit halber kann dies das globale Administratorkonto sein.
   - Wenn Sie einen Microsoft-Identitätsanbieter wie contosoadmin@live.com oder contosoadmin@outlook.com verwenden, können Sie sich möglicherweise nicht bei Azure Virtual Desktop anmelden. Stattdessen wird die Verwendung eines domänenspezifischen Kontos wie admin@contoso.com oder admin@contoso.onmicrosoft.com empfohlen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Auswählen eines Benutzers, der als „TenantCreator“ hinzugefügt werden soll](../media/tenant-assign-user.png)

   > [!NOTE]
   > Sie müssen einen Benutzer (oder eine Gruppe mit einem Benutzer) auswählen, der aus dieser Azure Active Directory-Instanz stammt. Sie können keinen Gastbenutzer (B2B) und keinen Dienstprinzipal auswählen.

6. Wählen Sie das Benutzerkonto, die Schaltfläche **Auswählen** und dann **Zuweisen** aus.
7. Vergewissern Sie sich auf der Seite **Azure Virtual Desktop – Benutzer und Gruppen**, dass für den Benutzer, der den Azure Virtual Desktop-Mandanten erstellt, ein neuer Eintrag mit der zugewiesenen Rolle **TenantCreator** angezeigt wird.

Bevor Sie mit der Erstellung Ihres Azure Virtual Desktop-Mandanten fortfahren, benötigen Sie die beiden folgenden Angaben:

   - ID Ihres Azure Active Directory-Mandanten (oder die **Verzeichnis-ID**)
   - ID Ihres Azure-Abonnements

So ermitteln Sie die ID Ihres Azure Active Directory-Mandanten (oder die **Verzeichnis-ID**)
1. Suchen Sie in der gleichen Sitzung im [Azure-Portal](https://portal.azure.com) nach **Azure Active Directory**, und wählen Sie die entsprechende Option aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Suchergebnisse für „Azure Active Directory“ im Azure-Portal. Das Suchergebnis unter „Dienste“ ist hervorgehoben.](../media/tenant-search-azure-active-directory.png)

2. Scrollen Sie nach unten zu **Eigenschaften**, und wählen Sie die Option aus.
3. Suchen Sie nach der **Verzeichnis-ID**, und wählen Sie das Symbol für die Zwischenablage aus. Fügen Sie sie an einem gut erreichbaren Ort ein, um sie später als **AadTenantId** verwenden zu können.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Azure Active Directory-Eigenschaften. Der Mauszeiger zeigt auf das Symbol, mit dem die Verzeichnis-ID in die Zwischenablage kopiert werden kann.](../media/tenant-directory-id.png)

So ermitteln Sie Ihre Azure-Abonnement-ID:
1. Suchen Sie in der gleichen Sitzung im [Azure-Portal](https://portal.azure.com) nach **Abonnements**, und wählen Sie die entsprechende Option aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Suchergebnisse für „Azure Active Directory“ im Azure-Portal. Das Suchergebnis für „Dienste“ ist hervorgehoben.](../media/tenant-search-subscription.png)

2. Wählen Sie das Azure-Abonnement aus, mit dem Sie Benachrichtigungen des Azure Virtual Desktop-Diensts empfangen möchten.
3. Zeigen Sie auf den Wert der **Abonnement-ID**, bis ein Symbol für die Zwischenablage angezeigt wird. Wählen Sie das Symbol für die Zwischenablage aus, und fügen Sie den Wert an einem gut erreichbaren Ort ein, um ihn später als Wert für **AzureSubscriptionId** verwenden zu können.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Eigenschaften des Azure-Abonnements. Der Mauszeiger zeigt auf das Symbol, mit dem die Abonnement-ID in die Zwischenablage kopiert werden kann.](../media/tenant-subscription-id.png)

## <a name="create-a-azure-virtual-desktop-tenant"></a>Erstellen eines Azure Virtual Desktop-Mandanten

Nachdem Sie die Azure Virtual Desktop-Dienstberechtigungen zum Abfragen der Azure Active Directory-Instanz gewährt und die Rolle „TenantCreator“ einem Benutzerkonto zugewiesen haben, können Sie nun einen Azure Virtual Desktop-Mandanten erstellen.

Sofern noch nicht geschehen, müssen Sie zunächst das [Azure Virtual Desktop-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Verwenden Sie das folgende Cmdlet, um sich mit dem TenantCreator-Benutzerkonto bei Azure Virtual Desktop anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Erstellen Sie anschließend einen neuen Azure Virtual Desktop-Mandanten, der dem Azure Active Directory-Mandanten zugeordnet ist:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Ersetzen Sie die Werte in Klammern durch die Werte, die für Ihre Organisation und den Mandanten relevant sind. Der Name, den Sie für Ihren neuen Azure Virtual Desktop-Mandanten festlegen, muss global eindeutig sein. Angenommen, Sie sind der TenantCreator für Azure Virtual Desktop für die Organisation Contoso. Das auszuführende Cmdlet sieht dann wie folgt aus:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

Es ist ratsam, einem zweiten Benutzer Administratorzugriff zuzuweisen, falls Sie sich einmal aus Ihrem Konto aussperren oder Urlaub nehmen und ein anderer Benutzer in Ihrer Abwesenheit die Aufgaben des Mandantenadministrators übernehmen muss. Führen Sie zum Zuweisen des Administratorzugriffs zu einem zweiten Benutzer das folgende Cmdlet mit `<TenantName>` und `<Upn>` aus. Ersetzen Sie diese Platzhalter durch Ihren Mandantennamen und den UPN des zweiten Benutzers.

```powershell
New-RdsRoleAssignment -TenantName <TenantName> -SignInName <Upn> -RoleDefinitionName "RDS Owner"
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihren Mandanten erstellt haben, müssen Sie in Azure Active Directory einen Dienstprinzipal erstellen und ihm eine Rolle in Azure Virtual Desktop zuweisen. Dieser Dienstprinzipal ermöglicht Ihnen eine erfolgreiche Bereitstellung des Azure Marketplace-Angebots für Azure Virtual Desktop zur Erstellung eines Hostpools. Weitere Informationen zu Hostpools finden Sie im Tutorial zum Erstellen eines Hostpools in Azure Virtual Desktop.

> [!div class="nextstepaction"]
> [Erstellen von Dienstprinzipalen und Rollenzuweisungen mit PowerShell](create-service-principal-role-powershell.md)
