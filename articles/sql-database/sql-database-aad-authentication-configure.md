---
title: Konfigurieren der Azure Active Directory-Authentifizierung
description: Hier erfahren Sie, wie Sie unter Verwendung der Azure Active Directory-Authentifizierung nach der Konfiguration von Azure AD eine Verbindung mit SQL-Datenbank, einer verwalteten Instanz und Azure Synapse Analytics herstellen.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: azure-synapse, has-adal-ref
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto, carlrab
ms.date: 03/27/2020
ms.openlocfilehash: 60a1b0deda75c1fc30a9e3b8255106d2809856ee
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198591"
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql"></a>Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL

In diesem Artikel erfahren Sie, wie Sie Azure AD erstellen, mit Daten auffüllen und mit [Azure SQL-Datenbank](sql-database-technical-overview.md) (SQL DB), einer [verwalteten Instanz](sql-database-managed-instance.md) (MI) und [Azure Synapse Analytics (früher Azure SQL Data Warehouse)](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) verwenden. Eine Übersicht finden Sie unter [Authentifizierung über Azure Active Directory](sql-database-aad-authentication.md).

> [!NOTE]
> Dieser Artikel gilt für Azure SQL-Server sowie für SQL-Datenbank und Azure Synapse. Der Einfachheit halber wird „SQL-Datenbank“ verwendet, wenn sowohl auf SQL-Datenbank als auch auf Azure Synapse verwiesen wird.

> [!IMPORTANT]  
> Das Herstellen einer Verbindung mit einer SQL Server-Instanz, die auf einer Azure-VM ausgeführt wird, wird für ein Azure Active Directory-Konto nicht unterstützt. Verwenden Sie stattdessen ein Active Directory-Domänenkonto.

## <a name="azure-ad-authentication-methods"></a>Azure AD-Authentifizierungsmethoden

Azure AD-Authentifizierung unterstützt die folgenden Authentifizierungsmethoden:

- Reine Azure AD-Cloudidentitäten
- Azure AD-Hybrididentitäten, die Folgendes unterstützen:
  - Cloudauthentifizierung mit zwei Optionen gekoppelt mit nahtlosem SSO (einmaliges Anmelden)
    - Azure AD-Kennworthashauthentifizierung
    - Azure AD-Passthrough-Authentifizierung
  - Verbundauthentifizierung

Weitere Informationen zu Azure AD-Authentifizierungsmethoden sowie zur Auswahl der passenden Methode finden Sie im folgenden Artikel:
- [Auswählen der richtigen Authentifizierungsmethode für Ihre Azure Active Directory-Hybrididentitätslösung](../active-directory/hybrid/choose-ad-authn.md)

Weitere Informationen zu Azure AD-Hybrididentitäten, zur Einrichtung und Synchronisierung finden Sie in den folgenden Artikeln:

- Kennworthashauthentifizierung: [Implementieren von Kennworthashsynchronisierung mit Azure AD Connect-Synchronisierung](../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)
- Passthrough-Authentifizierung: [Azure Active Directory-Passthrough-Authentifizierung](../active-directory/hybrid/how-to-connect-pta-quick-start.md)
- Verbundauthentifizierung: [Bereitstellung von Active Directory-Verbunddiensten in Azure](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs) und [Azure AD Connect und Verbund](../active-directory/hybrid/how-to-connect-fed-whatis.md)

Alle oben genannten Authentifizierungsmethoden werden für SQL-Datenbank (Einzeldatenbank-und Datenbankpools), verwaltete Instanzen und Azure Synapse unterstützt.

## <a name="create-and-populate-an-azure-ad"></a>Erstellen und Auffüllen von Azure AD

Erstellen Sie ein Azure Active Directory-Verzeichnis, und füllen Sie es mit Benutzern und Gruppen. Azure AD kann die verwaltete Azure AD-Anfangsdomäne sein. Azure AD kann auch ein lokaler Active Directory Domain Services im Verbund mit Azure AD sein.

Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure AD](../active-directory/active-directory-domains-add-azure-portal.md), [Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten Ihres Azure AD-Verzeichnisses](../active-directory/fundamentals/active-directory-administer.md)[ sowie unter Verwalten von Azure AD mit Windows PowerShell](/powershell/azure/overview) und [erforderliche Ports und Protokolle für die Hybrid-Identität](../active-directory/hybrid/reference-connect-ports.md).

## <a name="associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory

1. Ordnen Sie Ihr Azure-Abonnement Azure Active Directory zu, indem Sie das Verzeichnis für das Azure-Abonnement, das die Datenbank hostet, als vertrauenswürdiges Verzeichnis kennzeichnen. Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

2. Verwenden Sie den Verzeichnisumschalter im Azure-Portal, um zu dem der Domäne zugeordneten Abonnement zu wechseln.

   > [!IMPORTANT]
   > Jedes Azure-Abonnement weist eine Vertrauensstellung mit einer Azure AD-Instanz auf. Dies bedeutet, dass es diesem Verzeichnis bei der Authentifizierung von Benutzern, Diensten und Geräten vertraut. Mehrere Abonnements können dem gleichen Verzeichnis vertrauen, ein Abonnement vertraut jedoch nur einem Verzeichnis. Diese Vertrauensstellung, die ein Abonnement mit einem Verzeichnis aufweist, unterscheidet sich von der Beziehung, die ein Abonnement mit allen anderen Ressourcen in Azure (Websites, Datenbanken usw.) hat. Diese ähneln eher den untergeordneten Ressourcen eines Abonnements. Wenn ein Abonnement abläuft, wird der Zugriff auf die anderen Ressourcen, die dem Abonnement zugeordnet sind, ebenfalls beendet. Das Verzeichnis verbleibt jedoch in Azure, und Sie können ihm ein anderes Abonnement zuordnen und weiterhin die Benutzer des Verzeichnisses verwalten. Weitere Informationen zu Ressourcen finden Sie unter [Grundlegendes zur Zugriffssteuerung in Azure sowie zur Integration in Azure Active Directory](../active-directory/active-directory-b2b-admin-add-users.md). Weitere Informationen zu dieser Vertrauensstellung finden Sie unter [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Erstellen eines Azure AD-Administrators für die Azure SQL Server-Instanz

Jede Azure SQL Server-Instanz (mit Hosten von SQL-Datenbank oder Azure Synapse) wird zunächst mit einem einzelnen Serveradministratorkonto konfiguriert, das als Administrator für die gesamte Azure SQL Server-Instanz fungiert. Anschließend muss ein zweiter SQL Server-Administrator erstellt werden, bei dem es sich um ein Azure AD-Konto handelt. Dieser Prinzipal wird als eigenständiger Datenbankbenutzer in der Masterdatenbank erstellt. Als Administratoren sind die Serveradministratorkonten Mitglieder der Rolle **db_owner** in jeder Benutzerdatenbank. Der Zugriff auf alle Benutzerdatenbanken erfolgt als Benutzer **dbo**. Weitere Informationen zu den Serveradministratorkonten finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md).

Bei Verwendung von Azure Active Directory mit Georeplikation muss der Azure Active Directory-Administrator sowohl für den primären als auch für die sekundären Server konfiguriert werden. Wenn ein Server über keinen Azure Active Directory-Administrator verfügt, erhalten Benutzer bei der Azure Active Directory-Anmeldung die Fehlermeldung „Keine Verbindung“.

> [!NOTE]
> Benutzer, die nicht auf einem Azure AD-Konto basieren (hierzu gehört auch das Administratorkonto für die Azure SQL Server-Instanz) können keine Azure AD-basierten Benutzer erstellen, da sie keine Berechtigung zum Überprüfen der vorgeschlagenen Datenbankbenutzer mit Azure AD besitzen.

## <a name="provision-an-azure-active-directory-administrator-for-your-managed-instance"></a>Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete Instanz

> [!IMPORTANT]
> Führen Sie diese Schritte nur aus, wenn Sie eine verwaltete Instanz bereitstellen. Dieser Vorgang kann nur von einem globalen Administrator/Unternehmensadministrator oder von einem Administrator für privilegierte Rollen in Azure AD durchgeführt werden. Mit den folgenden Schritten wird der Prozess zum Gewähren von Berechtigungen für Benutzer mit unterschiedlichen Berechtigungen für das Verzeichnis beschrieben.

> [!NOTE]
> Für Azure AD Administratoren gibt es bei verwalteten Instanzen, die vor der allgemeinen Verfügbarkeit erstellt wurden und danach weiter verwendet werden, keine funktionalen Änderung am Verhalten. Weitere Informationen finden Sie im Abschnitt [Neue Azure AD-Administratorfunktionen für verwaltete Instanzen](#new-azure-ad-admin-functionality-for-mi).

Für Ihre verwaltete Instanz sind Leseberechtigungen für Azure AD erforderlich, damit Aufgaben wie die Authentifizierung von Benutzern über die Mitgliedschaft in Sicherheitsgruppen oder die Erstellung neuer Benutzer erfolgreich durchgeführt werden können. Damit das funktioniert, müssen Sie der verwalteten Instanz Leseberechtigungen für Azure AD gewähren. Hierfür gibt es zwei Möglichkeiten: das Portal und PowerShell. Die folgenden Schritte gelten für beide Methoden.

1. Wählen Sie im Azure-Portal in der oberen rechten Ecke Ihre Verbindung aus, um eine Dropdownliste mit möglichen Active Directory-Verzeichnissen zu öffnen.

2. Wählen Sie das richtige Active Directory-Verzeichnis als Standardeinstellung für Azure AD aus.

   In diesem Schritt wird das Abonnement, das Active Directory zugeordnet ist, mit der verwalteten SQL-Datenbank-Instanz verknüpft. So wird sichergestellt, dass dasselbe Abonnement sowohl für Azure AD als auch für die verwaltete SQL-Datenbank-Instanz verwendet wird.

3. Navigieren Sie zu „Verwaltete Instanz“, und wählen Sie die gewünschte Instanz aus, die Sie für die Azure AD-Integration verwenden möchten.

   ![aad](./media/sql-database-aad-authentication/aad.png)

4. Wählen Sie oben auf der Seite „Active Directory-Administrator“ das Banner aus, und erteilen Sie dem aktuellen Benutzer die Berechtigung. Wenn Sie bei Azure AD als globaler bzw. Unternehmensadministrator angemeldet sind, können Sie dies über das Azure-Portal durchführen oder PowerShell mit dem nachfolgenden Skript verwenden.

    ![Erteilen von Berechtigungen – Portal](./media/sql-database-aad-authentication/grant-permissions.png)

    ```powershell
    # Gives Azure Active Directory read permission to a Service Principal representing the managed instance.
    # Can be executed only by a "Company Administrator", "Global Administrator", or "Privileged Role Administrator" type of user.

    $aadTenant = "<YourTenantId>" # Enter your tenant ID
    $managedInstanceName = "MyManagedInstance"

    # Get Azure AD role "Directory Users" and create if it doesn't exist
    $roleName = "Directory Readers"
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    if ($role -eq $null) {
        # Instantiate an instance of the role template
        $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
        Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
        $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
    }

    # Get service principal for managed instance
    $roleMember = Get-AzureADServicePrincipal -SearchString $managedInstanceName
    $roleMember.Count
    if ($roleMember -eq $null) {
        Write-Output "Error: No Service Principals with name '$    ($managedInstanceName)', make sure that managedInstanceName parameter was     entered correctly."
        exit
    }
    if (-not ($roleMember.Count -eq 1)) {
        Write-Output "Error: More than one service principal with name pattern '$    ($managedInstanceName)'"
        Write-Output "Dumping selected service principals...."
        $roleMember
        exit
    }

    # Check if service principal is already member of readers role
    $allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    $selDirReader = $allDirReaders | where{$_.ObjectId -match     $roleMember.ObjectId}

    if ($selDirReader -eq $null) {
        # Add principal to readers role
        Write-Output "Adding service principal '$($managedInstanceName)' to     'Directory Readers' role'..."
        Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId     $roleMember.ObjectId
        Write-Output "'$($managedInstanceName)' service principal added to     'Directory Readers' role'..."

        #Write-Output "Dumping service principal '$($managedInstanceName)':"
        #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
        #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
    }
    else {
        Write-Output "Service principal '$($managedInstanceName)' is already     member of 'Directory Readers' role'."
    }
    ```

5. Nachdem der Vorgang erfolgreich abgeschlossen wurde, wird in der oberen rechten Ecke die folgende Benachrichtigung angezeigt:

    ![success](./media/sql-database-aad-authentication/success.png)

6. Nun können Sie Ihren Azure AD-Administrator für die verwaltete Instanz auswählen. Wählen Sie hierzu auf der Seite „Active Directory-Administrator“ **Administrator festlegen** aus.

    ![Administrator festlegen](./media/sql-database-aad-authentication/set-admin.png)

7. Suchen Sie auf der Azure AD-Administratorseite nach einem Benutzer, wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie als Administrator festlegen möchten, und wählen Sie dann **Auswählen** aus.

   Auf der Seite „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. Abgeblendete Benutzer oder Gruppen können nicht ausgewählt werden, da sie nicht als Azure AD-Administratoren unterstützt werden. Eine Liste mit den unterstützten Administratoren finden Sie unter [Features und Einschränkungen von Azure AD](sql-database-aad-authentication.md#azure-ad-features-and-limitations). Die rollenbasierte Zugriffskontrolle (Role-Based Access Control, RBAC) gilt nur für das Azure-Portal und wird nicht an SQL Server weitergegeben.

    ![Hinzufügen eines Azure Active Directory-Administrators](./media/sql-database-aad-authentication/add-azure-active-directory-admin.png)

8. Wählen Sie oben auf der Seite „Active Directory-Administrator“ **Speichern** aus.

    ![Speichern](./media/sql-database-aad-authentication/save.png)

    Der Vorgang zum Ändern des Administrators kann einige Minuten in Anspruch nehmen. Anschließend wird der neue Administrator im Feld „Active Directory-Administrator“ angezeigt.

Nachdem Sie einen Azure AD-Administrator für die verwaltete Instanz bereitgestellt haben, können Sie unter Verwendung der Syntax <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> mit der Erstellung von Azure AD-Serverprinzipalen (Anmeldungen) beginnen. Weitere Informationen finden Sie in der [Übersicht über verwaltete Instanzen](sql-database-managed-instance.md#azure-active-directory-integration).

> [!TIP]
> Wenn Sie später einen Administrator entfernen möchten, wählen Sie oben auf der Seite „Active Directory-Administrator“ **Administrator entfernen** und anschließend **Speichern** aus.

### <a name="new-azure-ad-admin-functionality-for-mi"></a>Neue Azure AD-Administratorfunktionen für verwaltete Instanzen

In der folgenden Tabelle werden die Administratorfunktionen für die Public Preview für Azure AD-Anmeldungen bei verwalteten Instanzen sowie die neuen Funktionen zusammengefasst, die mit der allgemeinen Verfügbarkeit von Azure AD-Anmeldungen eingeführt werden.

| Azure AD-Anmeldeadministrator für verwaltete Instanzen während der Public Preview | Azure AD-Administratorfunktionen für verwaltete Instanzen seit der allgemeinen Verfügbarkeit |
| --- | ---|
| Das Verhalten ähnelt dem für Azure AD-Administratoren für SQL-Datenbank, die Azure AD-Authentifizierungen ermöglichen. Der Azure AD-Administrator kann jedoch keine Azure AD- oder SQL-Anmeldungen in der Masterdatenbank für die verwaltete Instanz erstellen. | Der Azure AD-Administrator verfügt über die sysadmin-Berechtigung und kann AAD- und SQL-Anmeldungen in der Masterdatenbank für die verwaltete Instanz erstellen. |
| Ist in der Sicht sys.server_principals nicht enthalten | Ist in der Sicht sys.server_principals enthalten |
| Ermöglicht die Einrichtung einzelner Azure AD-Gastbenutzer als Azure AD-Administrator für die verwaltete Instanz. Weitere Informationen finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](../active-directory/b2b/add-users-administrator.md). | Erfordert das Erstellen einer Azure AD-Gruppe mit Gastbenutzern als Mitglieder, damit diese Gruppe als Azure AD-Administrator für die verwaltete Instanz eingerichtet werden kann. Weitere Informationen finden Sie unter [Azure AD-Business-to-Business-Unterstützung](sql-database-ssms-mfa-authentication.md#azure-ad-business-to-business-support). |

Als bewährte Methode für vorhandene Azure AD-Administratoren von verwalteten Instanzen, die vor der allgemeinen Verfügbarkeit erstellt wurden und danach weiter verwendet werden, wird empfohlen, den Azure AD-Administrator im Azure-Portal mithilfe der Optionen „Administrator entfernen“ und „Administrator festlegen“ für denselben Azure AD-Benutzer oder dieselbe Gruppe zurückzusetzen.

### <a name="known-issues-with-the-azure-ad-login-ga-for-mi"></a>Bekannte Probleme seit der allgemeinen Verfügbarkeit von Azure AD-Anmeldungen für verwaltete Instanzen

- Wenn in der Masterdatenbank für die verwaltete Instanz eine Azure AD-Anmeldung vorhanden ist, die mit dem T-SQL-Befehl `CREATE LOGIN [myaadaccount] FROM EXTERNAL PROVIDER` erstellt wurde, kann diese nicht als Azure AD-Administrator für die verwaltete Instanz eingerichtet werden. Es tritt ein Fehler auf, wenn Sie die Anmeldung im Azure-Portal, mit PowerShell oder über CLI-Befehle zum Erstellen der Azure AD-Anmeldung als Azure AD-Administrator festlegen.
  - Die Anmeldung muss in der Masterdatenbank mithilfe des Befehls `DROP LOGIN [myaadaccount]` gelöscht werden, bevor das Konto als Azure AD-Administrator erstellt werden kann.
  - Richten Sie das Azure AD-Administratorkonto im Azure-Portal ein, nachdem `DROP LOGIN` erfolgreich ausgeführt wurde. 
  - Wenn Sie das Azure AD-Administratorkonto nicht einrichten können, suchen Sie in der Masterdatenbank der verwalteten Instanz nach der Anmeldung. Verwenden Sie den folgenden Befehl: `SELECT * FROM sys.server_principals`.
  - Wenn Sie einen Azure AD-Administrator für die verwaltete Instanz einrichten, wird automatisch eine Anmeldung in der Masterdatenbank für dieses Konto erstellt. Durch das Entfernen des Azure AD-Administrators wird die Anmeldung auch automatisch aus der Masterdatenbank gelöscht.

- Einzelne Azure AD-Gastbenutzer werden nicht als Azure AD-Administratoren für verwaltete Instanzen unterstützt. Gastbenutzer müssen Teil einer Azure AD-Gruppe sein, damit sie als Azure AD-Administrator festgelegt werden können. Auf dem Blatt im Azure-Portal werden Gastbenutzer derzeit nicht für eine andere Azure AD-Instanz abgeblendet, sodass Benutzer die Einrichtung von Administratoren durchführen können. Das Speichern von Gastbenutzern als Azure AD-Administratoren führt jedoch zu einem Setupfehler.
  - Wenn Sie einen Gastbenutzer als Azure AD-Administrator für die verwaltete Instanz festlegen möchten, schließen Sie den Gastbenutzer in eine Azure AD-Gruppe ein, und legen Sie diese Gruppe dann als Azure AD-Administrator fest.

### <a name="powershell-for-sql-managed-instance"></a>PowerShell für eine verwaltete SQL-Instanz

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zum Ausführen von PowerShell-Cmdlets muss Azure PowerShell installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

> [!IMPORTANT]
> Das Azure Resource Manager-Modul von PowerShell wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Das AzureRM-Modul erhält mindestens bis Dezember 2020 weiterhin Fehlerbehebungen.  Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch. Weitere Informationen zur Kompatibilität finden Sie in der [Einführung in das neue Azure PowerShell Az-Modul](/powershell/azure/new-azureps-module-az).

Um einen Azure AD-Administrator bereitzustellen, führen Sie die folgenden Azure PowerShell-Befehle aus:

- Connect-AzAccount
- Select-AzSubscription

Cmdlets zum Bereitstellen und Verwalten des Azure AD-Administrators für eine verwaltete SQL-Instanz:

| Cmdlet-Name | BESCHREIBUNG |
| --- | --- |
| [Set-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlinstanceactivedirectoryadministrator) |Stellt einen Azure AD-Administrator für eine verwaltete SQL-Instanz im aktuellen Abonnement bereit. (Muss aus dem aktuellen Abonnement stammen.)|
| [Remove-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlinstanceactivedirectoryadministrator) |Entfernt einen Azure AD-Administrator für eine verwaltete SQL-Instanz im aktuellen Abonnement. |
| [Get-AzSqlInstanceActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlinstanceactivedirectoryadministrator) |Gibt Informationen zu einem Azure AD-Administrator für eine verwaltete SQL-Instanz im aktuellen Abonnement zurück.|

Der folgende Befehl ruft Informationen zu einem Azure AD-Administrator für eine verwaltete Instanz namens „ManagedInstance01“ ab, die einer Ressourcengruppe namens „ResourceGroup01“ zugeordnet ist:

```powershell
Get-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01"
```

Der folgende Befehl stellt eine Azure AD-Administratorgruppe namens „DBAs“ für die verwaltete Instanz „ManagedInstance01“ bereit. Dieser Server ist der Ressourcengruppe „ResourceGroup01“ zugeordnet:

```powershell
Set-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstance01" -DisplayName "DBAs" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353b"
```

Der folgende Befehl entfernt den Azure AD-Administrator für die verwaltete Instanz „ManagedInstanceName01“, die der Ressourcengruppe „ResourceGroup01“ zugeordnet ist:

```powershell
Remove-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName "ResourceGroup01" -InstanceName "ManagedInstanceName01" -Confirm -PassThru
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Ein Azure AD-Administrator für eine verwaltete SQL-Instanz kann auch mithilfe folgender CLI-Befehle bereitgestellt werden:

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[az sql mi ad-admin create](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-create) | Stellt einen Azure Active Directory-Administrator für eine verwaltete SQL-Instanz bereit. (Muss aus dem aktuellen Abonnement stammen.) |
|[az sql mi ad-admin delete](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-delete) | Entfernt einen Azure Active Directory-Administrator für eine verwaltete SQL-Instanz. |
|[az sql mi ad-admin list](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-list) | Gibt Informationen zu einem Azure Active Directory-Administrator zurück, der aktuell für eine verwaltete SQL-Instanz konfiguriert ist. |
|[az sql mi ad-admin update](/cli/azure/sql/mi/ad-admin#az-sql-mi-ad-admin-update) | Aktualisiert den Azure Active Directory-Administrator für eine verwaltete SQL-Instanz. |

Weitere Informationen zu CLI-Befehlen finden Sie unter [az sql mi](/cli/azure/sql/mi).

* * *

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server"></a>Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server

> [!IMPORTANT]
> Führen Sie die folgenden Schritte nur aus, wenn Sie einen Azure SQL-Datenbank-Server oder Azure Synapse Analytics bereitstellen.

Die beiden folgenden Verfahren zeigen Ihnen, wie Sie einen Azure Active Directory-Administrator für Ihre Azure SQL Server-Instanz im Azure-Portal und mithilfe von PowerShell bereitstellen.

### <a name="azure-portal"></a>Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) in der oberen rechten Ecke Ihre Verbindung aus, um eine Dropdownliste mit möglichen Active Directory-Verzeichnissen zu öffnen. Wählen Sie das richtige Active Directory-Verzeichnis als Standardeinstellung für Azure AD aus. Durch diesen Schritt wird die mit dem Abonnement verbundene Active Directory-Instanz mit der Azure SQL Server-Instanz verknüpft. So wird sichergestellt, dass das gleiche Abonnement sowohl für Azure AD als auch für SQL Server verwendet wird. (Die Azure SQL Server-Instanz kann entweder Azure SQL-Datenbank oder Azure Synapse hosten.)

    ![Administrator auswählen][8]

2. Suchen Sie nach **SQL Server**, und wählen Sie diese Option aus.

    ![Suchen nach und Auswählen von „SQL Server“](media/sql-database-aad-authentication/search-for-and-select-sql-servers.png)

    >[!NOTE]
    > Auf dieser Seite können Sie vor der Auswahl von **SQL Server** den **Stern** neben dem Namen auswählen, um die Kategorie den *Favoriten* hinzuzufügen, und **SQL Server** der linken Navigationsleiste hinzufügen.

3. Wählen Sie auf der Seite **SQL Server** die Option **Active Directory-Administrator** aus.

4. Wählen Sie auf der Seite **Active Directory-Administrator** die Option **Administrator festlegen** aus.

    ![SQL Server: Festlegen des Active Directory-Administrators](./media/sql-database-aad-authentication/sql-servers-set-active-directory-admin.png)  

5. Suchen Sie auf der Seite **Administrator hinzufügen** nach einem Benutzer, wählen Sie den Benutzer oder die Gruppe aus, den bzw. die Sie als Administrator festlegen möchten, und wählen Sie dann **Auswählen** aus. Auf der Seite „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Auf dem Blatt „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. (Siehe die Liste von unterstützten Administratoren im Abschnitt **Funktionen und Einschränkungen von Azure AD** von [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder Azure Synapse](sql-database-aad-authentication.md).) Die rollenbasierte Zugriffskontrolle (Role-based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben.

    ![Auswählen des Azure Active Directory-Administrators](./media/sql-database-aad-authentication/select-azure-active-directory-admin.png)  

6. Wählen Sie oben auf der Seite **Active Directory-Administrator** **SPEICHERN** aus.

    ![Administrator speichern](./media/sql-database-aad-authentication/save-admin.png)

Der Vorgang zum Ändern des Administrators kann einige Minuten in Anspruch nehmen. Anschließend wird der neue Administrator im Feld **Active Directory-Administrator** angezeigt.

   > [!NOTE]
   > Beim Einrichten des Azure AD-Administrators darf der Name des neuen Administrators (Benutzer oder Gruppe) in der virtuellen Masterdatenbank noch nicht als SQL Server-Authentifizierungsbenutzer vorhanden sein. Wenn er vorhanden ist, schlägt die Einrichtung des Azure AD-Administrators fehl, wobei die Erstellung rückgängig gemacht und angegeben wird, dass ein solcher Administrator (Name) bereits vorhanden ist. Da dieser SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, sind alle Versuche erfolglos, unter der Azure AD-Authentifizierung eine Verbindung mit dem Server herzustellen.

Wenn Sie später einen Administrator entfernen möchten, wählen Sie oben auf der Seite **Active Directory-Administrator** **Administrator entfernen** und anschließend **Speichern** aus.

### <a name="powershell-for-azure-sql-database-and-azure-synapse"></a>PowerShell für Azure SQL-Datenbank und Azure Synapse

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zum Ausführen von PowerShell-Cmdlets muss Azure PowerShell installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Um einen Azure AD-Administrator bereitzustellen, führen Sie die folgenden Azure PowerShell-Befehle aus:

- Connect-AzAccount
- Select-AzSubscription

Cmdlets zum Bereitstellen und Verwalten des Azure AD-Administrators für Azure SQL-Datenbank und Azure Synapse:

| Cmdlet-Name | BESCHREIBUNG |
| --- | --- |
| [Set-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/set-azsqlserveractivedirectoryadministrator) |Stellt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure Synapse bereit. (Muss aus dem aktuellen Abonnement stammen.) |
| [Remove-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/remove-azsqlserveractivedirectoryadministrator) |Entfernt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure Synapse. |
| [Get-AzSqlServerActiveDirectoryAdministrator](/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator) |Gibt Informationen zu dem Azure Active Directory-Administrator zurück, der für die Azure SQL Server-Instanz oder Azure Synapse derzeit konfiguriert ist. |

Verwenden Sie den PowerShell-Befehl „get-help“, um weitere Informationen zu diesen Befehlen zu erhalten. Beispiel: `get-help Set-AzSqlServerActiveDirectoryAdministrator`.

Das folgende Skript stellt eine Azure AD-Administratorengruppe namens **DBA_Group** (Objekt-ID `40b79501-b343-44ed-9ce7-da4c8cc7353f`) für den Server **demo_server** in einer Ressourcengruppe mit dem Namen **Group-23** bereit:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" -DisplayName "DBA_Group"
```

Der Eingabeparameter **DisplayName** akzeptiert entweder den Azure AD-Anzeigenamen oder den Benutzerprinzipalnamen (UPN). Beispiel: ``DisplayName="John Smith"`` und ``DisplayName="johns@contoso.com"``. Für Azure AD-Gruppen wird nur der Azure AD-Anzeigename unterstützt.

> [!NOTE]
> Der Azure PowerShell-Befehl ```Set-AzSqlServerActiveDirectoryAdministrator``` hindert Sie nicht daran, nicht unterstützte Benutzer als Azure AD-Administratoren bereitzustellen. Ein nicht unterstützter Benutzer kann bereitgestellt werden, jedoch keine Verbindung mit einer Datenbank herstellen.

Im folgenden Beispiel wird der optionale Wert für **ObjectID**verwendet:

```powershell
Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" `
    -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> Ein Wert für **ObjectID** ist in Azure AD erforderlich, wenn **DisplayName**nicht eindeutig ist. Verwenden Sie den Active Directory-Abschnitt im klassischen Azure-Portal, um die Werte für **ObjectID** und **DisplayName** abzurufen um die Eigenschaften für einen Benutzer oder eine Gruppe anzuzeigen.

Das folgende Beispiel gibt Informationen zum aktuellen Azure AD-Administrator für die Azure SQL Server-Instanz zurück:

```powershell
Get-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

Im folgenden Beispiel wird ein Azure AD-Administrator entfernt:

```powershell
Remove-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sie können einen Azure AD-Administrator bereitstellen, indem Sie die folgenden CLI-Befehle aufrufen:

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[az sql server ad-admin create](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-create) | Stellt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure Synapse bereit. (Muss aus dem aktuellen Abonnement stammen.) |
|[az sql server ad-admin delete](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-delete) | Entfernt einen Azure Active Directory-Administrator für die Azure SQL Server-Instanz oder Azure Synapse. |
|[az sql server ad-admin list](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) | Gibt Informationen zu dem Azure Active Directory-Administrator zurück, der für die Azure SQL Server-Instanz oder Azure Synapse derzeit konfiguriert ist. |
|[az sql server ad-admin update](/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-update) | Aktualisiert den Active Directory-Administrator für eine Azure SQL Server-Instanz oder Azure Synapse. |

Weitere Informationen zu CLI-Befehlen finden Sie unter [az sql server](/cli/azure/sql/server).

* * *

> [!NOTE]
> Sie können auch einen Azure Active Directory-Administrator mithilfe der REST-APIs bereitstellen. Weitere Informationen finden Sie unter [Referenz zur REST-API der Dienstverwaltung und Vorgänge für Azure SQL-Datenbank](/rest/api/sql/).

## <a name="configure-your-client-computers"></a>Konfigurieren der Clientcomputer

Auf allen Clientcomputern, über die Ihre Anwendungen oder Benutzer unter Verwendung von Azure AD-Identitäten eine Verbindung mit der Azure SQL-Datenbank oder mit Azure Synapse herstellen, muss die folgende Software installiert werden:

- .NET Framework 4.6 oder höher aus [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory-Authentifizierungsbibliothek für SQL Server (*ADAL.DLL*). Nachstehend finden Sie die Downloadlinks zum Installieren der neuesten SSMS-, ODBC-und OLE DB-Treiber, die die Bibliothek *ADAL.DLL* enthalten.
    1. [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
    1. [ODBC Driver 17 for SQL Server](https://www.microsoft.com/download/details.aspx?id=56567)
    1. [OLE DB Driver 18 for SQL Server](https://www.microsoft.com/download/details.aspx?id=56730)

Sie können diese Anforderungen folgendermaßen erfüllen:

- Durch das Installieren der aktuellen Version von [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) oder [SQL Server Data Tools](/sql/ssdt/download-sql-server-data-tools-ssdt) wird die Anforderung erfüllt, dass .NET Framework 4.6 vorhanden sein muss.
    - SSMS installiert die x86-Version von *ADAL.DLL*.
    - SSDT installiert die amd64-Version von *ADAL.DLL*.
    - Die neueste Visual Studio-Version unter [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) erfüllt die .NET Framework 4.6-Anforderung, die erforderliche amd64-Version von *ADAL.DLL* wird jedoch nicht installiert.

## <a name="create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind

> [!IMPORTANT]
> Die verwaltete Instanz unterstützt jetzt Azure AD-Serverprinzipale (Anmeldungen), sodass Sie Anmeldungen für Azure AD-Benutzer, -Gruppen oder -Anwendungen erstellen können. Mit Azure AD-Serverprinzipalen (Anmeldungen) ist eine Authentifizierung bei Ihrer verwalteten Instanz möglich, ohne dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden müssen. Weitere Informationen finden Sie in der [Übersicht über verwaltete Instanzen](sql-database-managed-instance.md#azure-active-directory-integration). Die Syntax zum Erstellen von Azure AD-Serverprinzipalen (Anmeldungen) finden Sie unter <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>.

Für die Azure Active Directory-Authentifizierung ist es erforderlich, dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden. Ein eigenständiger Datenbankbenutzer basierend auf einer Azure AD-Identität ist ein Datenbankbenutzer, der über keine Anmeldung für die Masterdatenbank verfügt, und der einer Identität im Azure AD-Verzeichnis zugeordnet ist, das mit der Datenbank verknüpft ist. Bei der Azure AD-Identität kann es sich entweder um ein einzelnes Benutzerkonto oder um eine Gruppe handeln. Weitere Informationen zu eigenständigen Datenbankbenutzern finden Sie unter [Eigenständige Datenbankbenutzer – machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht im Azure-Portal erstellt werden. RBAC-Rollen werden nicht auf SQL Server, SQL-Datenbank oder Azure Synapse übertragen. Azure RBAC-Rollen dienen zum Verwalten von Azure-Ressourcen und gelten nicht für Datenbankberechtigungen. Beispielsweise gewährt die Rolle **SQL Server-Mitwirkender** keinen Zugriff zum Herstellen einer Verbindung mit der SQL-Datenbank oder Azure Synapse. Die Zugriffsberechtigung muss direkt in der Datenbank mithilfe von Transact-SQL-Anweisungen erteilt werden.

> [!WARNING]
> Sonderzeichen wie Doppelpunkt `:` oder das Und-Zeichen `&`, die als Benutzernamen in den Anweisungen „T-SQL CREATE LOGIN“ und „CREATE USER“ enthalten sind, werden nicht unterstützt.

Um einen Azure AD-basierten eigenständigen Datenbankbenutzer zu erstellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mit einer Azure AD-Identität, die mindestens über die Berechtigung **BELIEBIGEN BENUTZER ÄNDERN** verfügt, eine Verbindung mit der Datenbank her. Verwenden Sie anschließend die folgende Transact-SQL-Syntax:

```sql
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* kann der Benutzerprinzipalname (UPN) eines Azure AD-Benutzers oder der Anzeigename einer Azure AD-Gruppe sein.

**Beispiele:** So erstellen Sie einen eigenständigen Datenbankbenutzer, der einen Benutzer der Azure AD-Verbunddomäne oder einen Benutzer der verwalteten Azure AD-Domäne repräsentiert:

```sql
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

Um einen eigenständigen Datenbankbenutzer zu erstellen, der eine Azure AD-Gruppe oder eine Gruppe der Verbunddomäne repräsentiert, geben Sie den Anzeigenamen einer Sicherheitsgruppe an:

```sql
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

So erstellen Sie einen eigenständigen Datenbankbenutzer, der eine Anwendung darstellt, die eine Verbindung mit einem Azure AD-Token herstellt:

```sql
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

> [!NOTE]
> Dieser Befehl erfordert, dass SQL auf Azure AD (den „externen Anbieter“) im Namen des angemeldeten Benutzers zugreift. Manchmal treten Umstände auf, die bewirken, dass Azure AD eine Ausnahme an SQL zurückgibt. In diesen Fällen wird dem Benutzer der SQL-Fehler 33134 angezeigt, der die AAD-spezifische Fehlermeldung enthalten sollte. In den meisten Fällen besagt der Fehler, dass der Zugriff verweigert wird oder dass der Benutzer sich für MFA registrieren muss, um auf die Ressource zuzugreifen, oder dass der Zugriff zwischen Erstanbieteranwendungen über Vorautorisierung verarbeitet werden muss. In den ersten beiden Fällen wird das Problem normalerweise durch Richtlinien für bedingten Zugriff verursacht, die im AAD-Mandanten des Benutzers festgelegt werden: Sie verhindern, dass der Benutzer auf den externen Anbieter zugreifen kann. Wenn Sie die Zertifizierungsstellenrichtlinien aktualisieren, um den Zugriff auf die Anwendung „00000002-0000-0000-c000-000000000000“ zu ermöglichen (die Anwendungs-ID der AAD Graph-API), sollte das Problem behoben werden. Wenn der Fehler besagt, dass der Zugriff zwischen Erstanbieteranwendungen über Vorautorisierung verarbeitet werden muss, liegt das Problem daran, dass der Benutzer als Dienstprinzipal angemeldet ist. Der Befehl sollte erfolgreich sein, wenn er stattdessen von einem Benutzer ausgeführt wird.

> [!TIP]
> Sie können einen Benutzer nur direkt aus der Azure Active Directory-Instanz erstellen, die Ihrem Azure-Abonnement zugeordnet ist. Mitglieder anderer Active Directory-Instanzen, die importierte Benutzer der zugeordneten Active Directory-Instanz sind (bekannt als externe Benutzer) können jedoch einer Active Directory-Gruppe des Active Directory-Mandanten hinzugefügt werden. Indem Sie einen eigenständigen Datenbankbenutzer für diese AD-Gruppe erstellen, können die Benutzer aus der externen Active Directory-Instanz auf die SQL-Datenbank zugreifen.

Weitere Informationen zum Erstellen eigenständiger Datenbankbenutzer basierend auf Azure Active Directory-Identitäten finden Sie unter [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Wenn der Azure Active Directory-Administrator für die Azure SQL Server-Instanz entfernt wird, können Azure AD-Authentifizierungsbenutzer keine Verbindung mehr mit dem Server herstellen. Nicht mehr verwendbare Azure AD-Benutzer können bei Bedarf manuell von einem SQL-Datenbankadministrator gelöscht werden.

> [!NOTE]
> Wenn Sie die Benachrichtigung **Das Verbindungstimeout ist abgelaufen.** erhalten, müssen Sie möglicherweise den Parameter `TransparentNetworkIPResolution` der Verbindungszeichenfolge auf FALSE festlegen. Weitere Informationen finden Sie unter [Connection timeout issue with .NET Framework 4.6.1 – TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/20../../connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/) (Verbindungstimeout-Problem mit .NET Framework 4.6.1 – TransparentNetworkIPResolution).

Wenn Sie einen Datenbankbenutzer erstellen, erhält dieser Benutzer die Berechtigung **VERBINDEN** und kann sich als Mitglied der Rolle **ÖFFENTLICH** mit dieser Datenbank verbinden. Anfänglich stehen dem Benutzer nur die Berechtigungen zur Verfügung, die der Rolle **ÖFFENTLICH** gewährt wurden, sowie sämtliche Berechtigungen, die Azure AD-Gruppen erteilt wurden, denen der Benutzer angehört. Nachdem Sie einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitgestellt haben, können Sie dem Benutzer zusätzliche Berechtigungen gewähren. Die Vorgehensweise ist hierbei mit der Berechtigungszuweisung für alle weiteren Benutzertypen identisch. Typischerweise gewähren Sie Datenbankrollen die gewünschten Berechtigungen, und weisen diesen Rollen anschließend Benutzer zu. Weitere Informationen finden Sie unter [Erste Schritte mit Berechtigungen für die Datenbank-Engine](https://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Weitere Informationen zu besonderen SQL-Datenbankrollen finden Sie unter [Verwalten von Datenbanken und Anmeldungen in Azure SQL-Datenbank](sql-database-manage-logins.md).
Ein Verbunddomänen-Benutzerkonto, das als externer Benutzer in eine verwaltete Domäne importiert wird, muss die Identität der verwalteten Domäne verwenden.

> [!NOTE]
> Azure AD-Benutzer werden in den Datenbankmetadaten mit dem Typ E (EXTERNAL_USER) gekennzeichnet, Gruppen mit dem Typ X (EXTERNAL_GROUPS). Weitere Informationen finden Sie unter [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

## <a name="connect-to-the-user-database-or-azure-synapse-by-using-ssms-or-ssdt"></a>Herstellen einer Verbindung mit der Benutzerdatenbank oder Azure Synapse mithilfe von SSMS oder SSDT  

Um zu bestätigen, dass der Azure AD-Administrator ordnungsgemäß eingerichtet ist, stellen Sie mit dem Azure AD-Administratorkonto eine Verbindung mit der Datenbank **master** her.
Um einen Azure AD-basierten eigenständigen Datenbankbenutzer bereitzustellen (bei dem es sich nicht um den Serveradministrator handelt, der Besitzer der Datenbank ist), stellen Sie mithilfe einer Azure AD-Identität, die Zugriff auf die Datenbank hat, eine Verbindung mit der Datenbank her.

> [!IMPORTANT]
> Unterstützung für die Azure Active Directory-Authentifizierung wird über [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio 2015 bereitgestellt. Die SSMS-Version von August 2016 bietet auch Unterstützung für die universelle Active Directory-Authentifizierung, die es Administratoren ermöglicht, die Multi-Factor Authentication per Telefonanruf, SMS, Smartcards mit PIN oder Benachrichtigung in einer mobilen App anzufordern.

## <a name="using-an-azure-ad-identity-to-connect-using-ssms-or-ssdt"></a>Verwenden einer Azure AD-Identität zum Herstellen einer Verbindung mithilfe von SSMS oder SSDT

Nachfolgend wird das Herstellen einer Verbindung mit einer SQL-­Datenbank mithilfe einer Azure AD-Identität und unter Verwendung von SQL Server Management Studio oder SQL Server Data Tools erläutert. 

### <a name="active-directory-integrated-authentication"></a>Integrierte Active Directory-Authentifizierung

Verwenden Sie diese Methode, wenn Sie bei Windows angemeldet sind. Verwenden Sie dazu Ihre Azure Active Directory-Anmeldeinformationen aus einer Verbunddomäne oder einer verwalteten Domäne, die für nahtloses einmaliges Anmelden für eine Passthrough- und Kennworthashauthentifizierung konfiguriert ist. Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbank-Engine herstellen**) im Feld **Authentifizierung** die Einstellung **Azure Active Directory: integriert** aus. Es ist kein Kennwort erforderlich bzw. es kann kein Kennwort eingegeben werden, weil Ihre vorhandenen Anmeldeinformationen zur Verbindungsherstellung verwendet werden.

    ![Integrierte AD-Authentifizierung auswählen][11]

2. Wählen Sie die Schaltfläche **Optionen** aus, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der die Verbindung hergestellt werden soll. Weitere Informationen finden Sie im Artikel [AAD-MFA-Authentifizierung](sql-database-ssms-mfa-authentication.md#azure-ad-domain-name-or-tenant-id-parameter), in dem die Unterschiede zwischen den Verbindungseigenschaften für SSMS 17.x und 18.x behandelt werden. 

    ![Datenbanknamen auswählen][13]

### <a name="active-directory-password-authentication"></a>Active Directory-Kennwortauthentifizierung

Verwenden Sie diese Methode, wenn Sie unter Verwendung der verwalteten Azure AD-Domäne und mit einem Azure AD-Prinzipalnamen eine Verbindung herstellen. Sie können diese Authentifizierungsmethode auch für ein Verbundkonto ohne Zugriff auf die Domäne verwenden, beispielsweise wenn Sie an einem Remotestandort arbeiten.

Verwenden Sie diese Methode, um sich bei SQL-Datenbank oder verwalteten Instanzen mit reinen Azure AD-Cloudidentitätsbenutzern oder Benutzern zu authentifizieren, die Azure AD-Hybrididentitäten verwenden. Diese Methode unterstützt Benutzer, die ihre Windows-Anmeldeinformationen verwenden möchten, deren lokaler Computer aber nicht mit der Domäne verbunden ist (z. B. über Remotezugriff). In diesem Fall kann ein Windows-Benutzer sein Domänenkonto mit Kennwort angeben und sich bei SQL DB, verwalteten Instanzen oder Azure Synapse mithilfe von Anmeldeinformationen als Verbundbenutzer authentifizieren.

1. Starten Sie Management Studio oder Data Tools, und wählen Sie im Dialogfeld **Verbindung mit dem Server herstellen** (oder **Verbindung mit Datenbank-Engine herstellen**) im Feld **Authentifizierung** die Einstellung **Azure Active Directory: Kennwort** aus.

2. Geben Sie im Feld **Benutzername** Ihren Azure Active Directory-Benutzernamen im Format **Benutzername\@domain.com** ein. Bei Benutzernamen muss es sich um ein Konto aus Azure Active Directory oder um ein Konto aus einer verwalteten Domäne oder einer Domäne handeln, die im Verbund mit Azure Active Directory konfiguriert ist.

3. Geben Sie im Feld **Kennwort** Ihr Benutzerkennwort für das Azure Active Directory-Konto oder das Konto der verwalteten Domäne oder Verbunddomäne ein.

    ![AD-Kennwortauthentifizierung auswählen][12]

4. Wählen Sie die Schaltfläche **Optionen** aus, und geben Sie auf der Seite **Verbindungseigenschaften** im Feld **Mit Datenbank verbinden** den Namen der Benutzerdatenbank ein, mit der die Verbindung hergestellt werden soll. (Siehe Grafik in der vorherigen Option.)

### <a name="active-directory-interactive-authentication"></a>Interaktive Active Directory-Authentifizierung

Verwenden Sie diese Methode für die interaktive Authentifizierung mit oder ohne mehrstufige Authentifizierung (Multi-Factor Authentication, MFA), wobei das Kennwort interaktiv angefordert wird. Diese Methode kann verwendet werden, um sich bei SQL-Datenbank, verwalteten Instanzen und Azure Synapse mit reinen Azure AD-Cloudidentitätsbenutzern oder Benutzern zu authentifizieren, die Azure AD-Hybrididentitäten verwenden.

Weitere Informationen finden Sie unter [Verwenden mehrstufiger AAD-Authentifizierung mit Azure SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).

## <a name="using-an-azure-ad-identity-to-connect-from-a-client-application"></a>Verwenden einer Azure AD-Identität, um von einer Clientanwendung aus eine Verbindung herzustellen

Die folgenden Verfahren zeigen Ihnen, wie Sie mit einer Azure AD-Identität von einer Clientanwendung aus eine Verbindung mit einer SQL-Datenbank herstellen.

### <a name="active-directory-integrated-authentication"></a>Integrierte Active Directory-Authentifizierung

Um integrierte Windows-Authentifizierung verwenden zu können, muss das Active Directory der Domäne einen Verbund mit Azure Active Directory bilden oder sollte eine verwaltete Domäne sein, die für nahtloses einmaliges Anmelden (SSO) für Passthrough- oder Kennworthashauthentifizierung konfiguriert ist. Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](../active-directory/hybrid/how-to-connect-sso.md).

> [!NOTE]
> [MSAL.NET (Microsoft.Identity.Client)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap) für die integrierte Windows-Authentifizierung wird nicht für nahtloses einmaliges Anmelden für die Pass-Through- und Kennworthashauthentifizierung unterstützt.

Ihre Client-Anwendung (oder ein Dienst), die eine Verbindung mit der Datenbank herstellt, muss unter den Domänenanmeldeinformationen eines Benutzers auf einem Computer ausgeführt werden, der der Domäne beigetreten ist.

Um mithilfe der integrierten Authentifizierung und einer Azure AD-Identität eine Verbindung mit einer Datenbank herzustellen, muss das Authentifizierungsschlüsselwort in der Verbindungszeichenfolge für die Datenbank auf `Active Directory Integrated` festgelegt sein. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

```csharp
string ConnectionString = @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Das Schlüsselwort `Integrated Security=True` in der Verbindungszeichenfolge wird nicht für die Verbindungsherstellung mit Azure SQL-Datenbank unterstützt. Beim Herstellen einer ODBC-Verbindung müssen Sie Leerzeichen entfernen und Authentifizierung auf ActiveDirectoryIntegrated festlegen.

### <a name="active-directory-password-authentication"></a>Active Directory-Kennwortauthentifizierung

Zum Herstellen einer Verbindung mit einer Datenbank mithilfe reiner Azure AD-Cloudidentitäts-Benutzerkonten oder Konten, die Azure AD-Hybrididentitäten verwenden, muss das Authentifizierungsschlüsselwort auf `Active Directory Password`festgelegt werden. Die Verbindungszeichenfolge muss Benutzer-ID/UID und Kennwort/PWD-Schlüsselwörter enthalten. Im folgenden C#-Codebeispiel wird ADO .NET verwendet.

```csharp
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Informieren Sie sich über Azure AD-Authentifizierungsmethoden, indem Sie die Demo-Codebeispiele unter [Azure AD Authentication GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth)(GitHub-Demo zur Azure AD-Authentifizierung) verwenden.

## <a name="azure-ad-token"></a>Azure AD-Token

Mit dieser Authentifizierungsmethode können Dienste der mittleren Ebene [JSON Webtoken (JWT)](../active-directory/develop/id-tokens.md) abrufen, um eine Verbindung mit Azure SQL-Datenbank oder Azure Synapse herzustellen, indem sie ein Token aus Azure Active Directory (AAD) abrufen. Diese Methode ermöglicht verschiedene Anwendungsszenarien einschließlich Dienstidentitäten, Dienstprinzipalen und Anwendungen, die zertifikatbasierte Authentifizierung verwenden. Sie müssen vier grundlegende Schritte zum Verwenden der Azure AD-Token-Authentifizierung ausführen:

1. Registrieren Sie Ihre Anwendung in Azure Active Directory, und rufen Sie die Client-ID für Ihren Code ab.
2. Erstellen Sie einen Datenbankbenutzer, der die Anwendung darstellt. (Bereits in Schritt 6 abgeschlossen.)
3. Erstellen Sie ein Zertifikat auf dem Clientcomputer, auf dem die Anwendung ausgeführt wird.
4. Fügen Sie das Zertifikat als Schlüssel für die Anwendung hinzu.

Beispiel-Verbindungszeichenfolge:

```csharp
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
conn.AccessToken = "Your JWT token"
conn.Open();
```

Weitere Informationen finden Sie im [SQL Server Security Blog](https://blogs.msdn.microsoft.com/sqlsecurity/20../../token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)(Blog zur Sicherheit von SQL Server). Informationen zum Hinzufügen eines Zertifikats finden Sie unter [Erste Schritte mit zertifikatbasierter Authentifizierung in Azure Active Directory](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).

### <a name="sqlcmd"></a>sqlcmd

Die folgenden Anweisungen stellen eine Verbindung mithilfe von Version 13.1 von SQLCMD her, die im [Download Center](https://www.microsoft.com/download/details.aspx?id=53591)verfügbar ist.

> [!NOTE]
> `sqlcmd` mit dem `-G`-Befehl funktioniert nicht mit Systemidentitäten und erfordert eine Benutzerprinzipalanmeldung.

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="troubleshooting-azure-ad-authentication"></a>Problembehandlung bei der Azure AD-Authentifizierung

Eine Anleitung zur Behebung von Problemen bei der Azure AD-Authentifizierung finden Sie im folgenden Blog: <https://techcommunity.microsoft.com/t5/azure-sql-database/troubleshooting-problems-related-to-azure-ad-authentication-with/ba-p/1062991>

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Anmeldungen, Benutzer, Datenbankrollen und Berechtigungen in SQL-Datenbank finden Sie unter [Anmeldungen, Benutzer, Datenbankrollen und Benutzerkonten](sql-database-manage-logins.md).
- Weitere Informationen zu Datenbankprinzipalen finden Sie unter [Prinzipale](https://msdn.microsoft.com/library/ms181127.aspx).
- Weitere Informationen zu Datenbankrollen finden Sie unter [Datenbankrollen](https://msdn.microsoft.com/library/ms189121.aspx).
- Weitere Informationen zu Firewallregeln in SQL-Datenbank finden Sie unter [Übersicht über Firewallregeln für Azure SQL-Datenbank](sql-database-firewall-configure.md).

<!--Image references-->
[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png
