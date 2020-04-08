---
title: Autorisieren des Server- und Datenbankzugriffs mithilfe von Anmeldungen und Benutzerkonten
description: Erfahren Sie, wie Azure SQL-Datenbank und Azure Synapse Analytics Benutzer mithilfe von Anmeldungen und Benutzerkonten für den Zugriff authentifizieren. Außerdem erfahren Sie, wie Sie Datenbankrollen und explizite Berechtigungen verwenden, um Anmeldungen und Benutzer zum Durchführen von Aktionen und Abfragen von Daten autorisieren.
keywords: Sicherheit für SQL-Datenbank,Datenbanksicherheitsverwaltung,Anmeldesicherheit,Datenbanksicherheit,Datenbankzugriff
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 03/23/2020
ms.openlocfilehash: 98c15fe11b64e8c177e60a2ea1eb7c50eaf69353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124799"
---
# <a name="authorizing-database-access-to-authenticated-users-to-sql-database-and-azure-synapse-analytics-using-logins-and-user-accounts"></a>Autorisieren des Datenbankzugriffs für in SQL-Datenbank und Azure Synapse Analytics authentifizierte Benutzer mithilfe von Anmeldungen und Benutzerkonten

In diesem Artikel lernen Sie Folgendes:

- Optionen zum Konfigurieren von Azure SQL-Datenbank und Azure Synapse Analytics (zuvor Azure SQL Data Warehouse), um Benutzern das Durchführen administrativer Aufgaben und den Zugriff auf in diesen Datenbanken gespeicherte Daten zu erlauben
- Konfiguration des Zugriffs und der Autorisierung nach dem ersten Erstellen eine Azure SQL-Datenbank
- Hinzufügen von Anmeldungen und Benutzerkonten in der Masterdatenbank und Gewähren von Administratorberechtigungen für diese Konten
- Hinzufügen von Benutzerkonten zu Benutzerdatenbanken im Zusammenhang mit Anmeldungen oder als eigenständige Benutzerkonten
- Konfigurieren von Benutzerkonten mit Berechtigungen in Benutzerdatenbanken mithilfe von Datenbankrollen und expliziten Berechtigungen

> [!IMPORTANT]
> Datenbanken in Azure SQL-Datenbank und Azure Synapse Analytics werden der Einfachheit halber im restlichen Teil dieses Artikels entweder als Datenbanken oder als Azure SQL bezeichnet.

## <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Die [**Authentifizierung**](sql-database-security-overview.md#authentication) ist der Prozess, in dem die Identität des Benutzers bestätigt wird. Ein Benutzer stellt mithilfe eines Benutzerkontos eine Verbindung zu einer Datenbank her.
Wenn ein Benutzer versucht, eine Verbindung mit einer Datenbank herzustellen, gibt er ein Benutzerkonto und Authentifizierungsinformationen an. Der Benutzer wird mit einer der folgenden zwei Authentifizierungsmethoden authentifiziert:

- [SQL-Authentifizierung](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode#connecting-through-sql-server-authentication)

  Bei dieser Authentifizierungsmethode übergibt der Benutzer einen Benutzerkontonamen und das zugehörige Kennwort, um eine Verbindung herzustellen. Dieses Kennwort wird in der Masterdatenbank für Benutzerkonten, die mit einer Anmeldung verknüpft sind, oder in der Datenbank gespeichert, die die Benutzerkonten enthält, die nicht mit einer Anmeldung verknüpft sind.
- [Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md)

  Bei dieser Authentifizierungsmethode gibt der Benutzer einen Benutzerkontonamen an und fordert an, dass der Dienst die Anmeldeinformationen verwendet, die in Azure Active Directory gespeichert sind.

**Anmeldungen und Benutzer:** In Azure SQL kann ein Benutzerkonto in einer Datenbank einer Anmeldung zugeordnet werden, die in der Masterdatenbank gespeichert wird oder ein Benutzername ist, der in einer individuellen Datenbank gespeichert wird.

- Eine **Anmeldung** ist ein individuelles Konto in der Masterdatenbank, mit dem ein Benutzerkonto in mehreren Datenbanken verknüpft werden kann. Die Anmeldeinformationen für das Benutzerkonto werden zusammen mit der Anmeldung gespeichert.
- Ein **Benutzerkonto** ist ein individuelles Konto in einer Datenbank, das mit einer Anmeldung verknüpft sein kann. Dies ist jedoch nicht erforderlich. Bei Benutzerkonten, die nicht mit einer Anmeldung verknüpft sind, werden die Anmeldeinformationen mit dem Benutzerkonto gespeichert.

Die [**Autorisierung**](sql-database-security-overview.md#authorization) für den Zugriff auf Daten und das Durchführen verschiedener Aktionen wird mithilfe von Datenbankrollen und expliziten Berechtigungen verwaltet. Der Begriff „Autorisierung“ bezieht sich auf Berechtigungen, die einem Benutzer zugewiesen werden. Die Autorisierung wird durch die [Rollenmitgliedschaften](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) und [Berechtigungen auf Objektebene](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) der Datenbank Ihres Benutzerkontos gesteuert. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen.

## <a name="existing-logins-and-user-accounts-after-creating-a-new-database"></a>Vorhandene Anmeldungen und Benutzerkonten nach Erstellung einer Datenbank

Wenn Sie Ihre erste Azure SQL-Bereitstellung erstellen, legen Sie eine Administratoranmeldung und ein zugehöriges Kennwort für diese Anmeldung fest. Dieses administrative Konto wird als **Serveradministrator** bezeichnet. Die folgende Konfiguration der Anmeldungen und Benutzer in den Master- und Benutzerdatenbank erfolgt während der Bereitstellung:

- Eine SQL-Anmeldung mit Administratorberechtigungen wird mit dem von Ihnen angegebenen Anmeldenamen erstellt. Eine [Anmeldung](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine#sa-login) ist ein individuelles Benutzerkonto für die Anmeldung bei SQL-Datenbank.
- Dieser Anmeldung werden vollständige Administratorberechtigungen für alle Datenbanken als [Serverebenenprinzipal](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) gewährt. Diese Anmeldung verfügt über alle in SQL-Datenbank verfügbaren Berechtigungen und kann nicht eingeschränkt werden. In einer verwalteten Instanz wird diese Anmeldung zur [festen Serverrolle „sysadmin“](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) hinzugefügt (diese Rolle ist in Einzel- und Pooldatenbanken nicht vorhanden).
- Ein [Benutzerkonto](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions#database-users) namens `dbo` wird in allen Benutzerdatenbank für diese Anmeldung erstellt. Der Benutzer [dbo](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/principals-database-engine) verfügt über alle Datenbankberechtigungen in der Datenbank und ihm wird die feste Datenbankrolle `db_owner` zugeordnet. Im späteren Verlauf dieses Artikels werden weitere feste Datenbankrollen behandelt.

Öffnen Sie das Azure-Portal, und navigieren Sie zur Registerkarte **Eigenschaften** Ihres Servers oder Ihrer verwalteten Instanz, um die Administratorkonten für eine Datenbank zu identifizieren.

![SQL Server-Administratoren](media/sql-database-manage-logins/sql-admins.png)

![SQL Server-Administratoren](media/sql-database-manage-logins/sql-admins2.png)

> [!IMPORTANT]
> Die Anmelde-ID des Administrators kann nach der Erstellung nicht geändert werden. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, klicken Sie auf **SQL Server**, wählen Sie den Server aus der Liste aus, und klicken Sie dann auf **Kennwort zurücksetzen**, um das Kennwort für den Administrator des logischen Servers zurückzusetzen. Rufen Sie das Azure-Portal auf, klicken Sie auf die Instanz, und wählen Sie dann **Kennwort zurücksetzen** aus, um das Kennwort für den Server einer verwalteten Instanz zurückzusetzen. Sie können auch PowerShell oder die Azure CLI verwenden.

## <a name="create-additional-logins-and-users-having-administrative-permissions"></a>Erstellen weiterer Anmeldungen und Benutzer mit Administratorberechtigungen

An diesem Punkt ist Ihre Azure SQL-Instanz lediglich für den Zugriff mit einer einzigen SQL-Anmeldung und einem Benutzerkonto konfiguriert. Zum Erstellen weiterer Anmeldungen mit vollständigen oder partiellen Administratorberechtigungen stehen Ihnen die folgenden Optionen zur Auswahl (je nach Bereitstellungsmodus):

- **Erstellen eines Azure Active Directory-Administratorkontos mit vollständigen Administratorberechtigungen**

  Aktivieren Sie die Azure Active Directory-Authentifizierung, und erstellen Sie eine Azure AD-Administratoranmeldung. Ein Azure Active Directory-Konto kann als Administrator der SQL-Datenbank-Bereitstellung mit vollständigen Administratorberechtigungen konfiguriert werden. Bei diesem Konto kann es sich um ein einzelnes Konto oder ein Sicherheitsgruppenkonto handeln. Ein Azure AD-Administrator **muss** konfiguriert werden, wenn Sie Azure AD-Konten zum Herstellen einer Verbindung mit SQL-Datenbank verwenden möchten. Ausführliche Informationen zum Aktivieren der Azure AD-Authentifizierung für alle SQL-Datenbank-Bereitstellungstypen finden Sie in den folgenden Artikeln:

  - [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL](sql-database-aad-authentication.md)
  - [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL](sql-database-aad-authentication-configure.md)

- **Erstellen von SQL-Anmeldungen mit vollständigen Administratorberechtigungen in der Bereitstellung einer verwalteten Instanz**

  - Erstellen Sie eine zusätzliche SQL Server-Anmeldung in der verwalteten Instanz.
  - Fügen Sie die Anmeldung mit der Anweisung [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) zur [festen Serverrolle „sysadmin“](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) hinzu. Diese Anmeldung verfügt über vollständige Administratorberechtigungen.
  - Alternativ können Sie eine [Azure AD-Anmeldung](sql-database-aad-authentication-configure.md?tabs=azure-powershell#new-azure-ad-admin-functionality-for-mi) mithilfe der Syntax <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a> erstellen.

- **Erstellen von SQL-Anmeldungen mit eingeschränkten Administratorberechtigungen in Einzel- oder Poolbereitstellungen**

  - Erstellen Sie eine zusätzliche SQL-Anmeldung in der Masterdatenbank für eine Einzel- oder Pooldatenbankbereitstellung oder der Bereitstellung einer verwalteten Instanz.
  - Erstellen Sie ein Benutzerkonto in der Masterdatenbank, das dieser neuen Anmeldung zugeordnet ist.
  - Fügen Sie das Benutzerkonto mithilfe der Anweisung [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql) zu `dbmanager`, der Rolle `loginmanager` oder beiden in der `master`-Datenbank hinzu (verwenden Sie für Azure Synapse Analytics die Anweisung [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql)).

  > [!NOTE]
  > Die Rollen `dbmanager` und `loginmanager` gelten **nicht** für Bereitstellungen verwalteter Instanzen.

  Mitglieder dieser [speziellen Masterdatenbankrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles#special-roles-for--and-) für Einzel- und Pooldatenbanken ermöglichen Benutzern, über Berechtigungen zum Erstellen und Verwalten von Datenbanken oder Anmeldungen zu verfügen. In Datenbanken, die von einem Benutzer erstellt wurden, der Mitglied der `dbmanager`-Rolle ist, wird das Mitglied der festen Datenbankrolle `db_owner` zugeordnet, sodass es sich mit dem Benutzerkonto `dbo` bei der Datenbank anmelden kann, um sie zu verwalten. Diese Rollen verfügen außerhalb der Masterdatenbank über keine expliziten Berechtigungen.

  > [!IMPORTANT]
  > Sie können keine zusätzliche SQL-Anmeldung mit vollständigen Administratorberechtigungen in einer Einzel- oder Pooldatenbank erstellen.

## <a name="create-accounts-for-non-administrator-users"></a>Erstellen von Konten für Benutzer ohne Administratorberechtigungen

Sie können Konten für Benutzer ohne Administratorberechtigungen mithilfe einer der folgenden zwei Methoden erstellen:

- **Erstellen einer Anmeldung**

  Erstellen Sie eine SQL-Anmeldung in der Masterdatenbank. Erstellen Sie dann ein Benutzerkonto in allen Datenbanken, auf die der Benutzer zugreifen können muss, und verknüpfen Sie das Benutzerkonto mit der Anmeldung. Dieser Ansatz wird bevorzugt, wenn der Benutzer auf mehrere Datenbanken zugreifen muss und die Kennwörter synchronisiert bleiben sollen. Bei diesem Ansatz kommt es bei Verwendung mit Georeplikation zu Komplexitäten, da die Anmeldung sowohl auf dem primären Server als auch auf den sekundären Servern erstellt werden muss. Weitere Informationen finden Sie unter [Konfigurieren und Verwalten der Sicherheit von Azure SQL-Datenbank für die Geowiederherstellung oder den Failover](sql-database-geo-replication-security-config.md).
- **Erstellen eines Benutzerkontos**

  Erstellen Sie ein Benutzerkonto in der Datenbank, auf die ein Benutzer zugreifen muss (dieser wird auch als [eigenständiger Benutzer](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) bezeichnet).

  - Bei Einzel- und Pooldatenbanken können Sie diese Art von Benutzerkonto immer erstellen.
  - Bei verwalteten Instanzdatenbanken, die [Azure AD-Serverprinzipale](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) nicht unterstützen, können Sie diese Art von Benutzerkonto nur in [eigenständigen Datenbanken](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) erstellen. Bei verwalteten Instanzen, die [Azure AD-Serverprinzipale](sql-database-aad-authentication-configure.md?tabs=azure-powershell#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) unterstützen, können Sie Benutzerkonten für die Authentifizierung bei der verwalteten Instanz erstellen, ohne dass Datenbankbenutzer als eigenständige Datenbankbenutzer erstellt werden müssen.

  Bei diesem Ansatz werden die Benutzerauthentifizierungsinformationen in den einzelnen Datenbanken gespeichert und automatisch in georeplizierten Datenbanken repliziert. Wenn jedoch dasselbe Konto in mehreren Datenbanken vorhanden ist und Sie die SQL-Authentifizierung verwenden, müssen Sie die Kennwörter manuell synchronisieren. Wenn ein Benutzer über ein Konto in verschiedenen Datenbanken mit unterschiedlichen Kennwörtern verfügt, kann das Merken dieser Kennwörter ebenfalls zu einem Problem werden.

> [!IMPORTANT]
> Sie müssen mit einem Azure AD-Konto angemeldet sein, das über Administratorberechtigungen in der SQL-Datenbank-Instanz verfügt, um eigenständige Benutzer zu erstellen, die Azure AD-Identitäten zugeordnet sind. Bei verwalteten Instanzen kann auch eine SQL-Anmeldung mit `sysadmin`-Berechtigungen eine Azure AD-Anmeldung oder einen -Benutzer erstellen.

Beispiele zum Erstellen von Anmeldungen und Benutzern finden Sie unter:

- [Erstellen einer Anmeldung für Einzel- oder Pooldatenbanken](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-current#examples-1)
- [Erstellen einer Anmeldung für eine verwaltete Instanzdatenbank](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current#examples-2)
- [Erstellen einer Anmeldung für eine Azure Synapse Analytics-Datenbank](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azure-sqldw-latest#examples-3)
- [Benutzer erstellen](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql#examples)
- [Erstellen eigenständiger Azure AD-Benutzer](sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)

> [!TIP]
> Ein Tutorial zur Sicherheit, das auch das Erstellen eigenständiger SQL Server-Benutzer in einer Einzel- oder Pooldatenbank umfasst, finden Sie unter [Tutorial: Schützen einer Einzel- oder Pooldatenbank](sql-database-security-tutorial.md).

## <a name="using-fixed-and-custom-database-roles"></a>Verwenden fester und benutzerdefinierter Datenbankrollen

Nachdem Sie ein Benutzerkonto basierend auf einer Anmeldung oder als eigenständigen Benutzer in einer Datenbank erstellt haben, können Sie diesen Benutzer zum Durchführen verschiedener Aktionen und für den Zugriff auf Daten in einer bestimmten Datenbank autorisieren. Sie können die folgenden Methoden zum Autorisieren des Zugriffs verwenden:

- **Feste Datenbankrollen**

  Fügen Sie das Benutzerkonto zu einer [festen Datenbankrolle](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) hinzu. Es gibt 9 feste Datenbankrollen, die jeweils über definierte Berechtigungen verfügen. Die folgenden festen Datenbankrollen werden am häufigsten verwendet: **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** und **db_denydatareader**. **db_owner** wird häufig verwendet, um nur einigen Benutzern uneingeschränkte Berechtigungen zu erteilen. Die anderen festen Datenbankrollen sind hilfreich, um bei der Entwicklung schnell eine einfache Datenbank zu erhalten, aber sie sind auch für die meisten Produktionsdatenbanken zu empfehlen. Die feste Datenbankrolle **db_datareader** gewährt beispielsweise Lesezugriff auf alle Tabellen in der Datenbank. Dies ist im nicht unbedingt erforderlich.

  - So fügen Sie einen Benutzer zu einer festen Datenbankrolle hinzu:

    - Verwenden Sie die Anweisung [ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql) in Azure SQL-Datenbank. Beispiele finden Sie unter [Beispiele zu ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql#examples).
    - Verwenden Sie die Anweisung [sp_addrolemember](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql) in Azure Synapse Analytics. Beispiele finden Sie unter [Beispiele zu sp_addrolemember](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql).

- **Benutzerdefinierte Datenbankrolle**

  Erstellen Sie mit der Anweisung [CREATE ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql) eine benutzerdefinierte Datenbankrolle. Mit einer benutzerdefinierten Rolle können Sie Ihre eigenen benutzerdefinierte Datenbankrollen erstellen und jeder Rolle sorgfältig die mindestens erforderlichen Berechtigungen für die geschäftlichen Aufgaben zuweisen. Anschließend können Sie Benutzer zur benutzerdefinierten Rolle hinzufügen. Wenn ein Benutzer Mitglied mehrerer Rollen ist, verfügt er über die zusammengefassten Berechtigungen all dieser Rollen.
- **Direktes Zuweisen von Berechtigungen**

  Erteilen Sie die [Berechtigungen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) direkt dem Benutzerkonto. Es gibt mehr als 100 Berechtigungen, die in SQL-Datenbank individuell gewährt oder verweigert werden können. Viele dieser Berechtigungen sind geschachtelt. Die `UPDATE`-Berechtigung für ein Schema enthält beispielsweise für jede Tabelle des Schemas die `UPDATE`-Berechtigung. Wie bei den meisten Berechtigungssystemen wird eine Gewährung durch die Verweigerung einer Berechtigung außer Kraft gesetzt. Aufgrund der Schachtelung und der Anzahl von Berechtigungen muss ein geeignetes Berechtigungssystem sorgfältig entworfen werden, um für Ihre Datenbank den richtigen Schutz sicherzustellen. Beginnen Sie mit der Liste der Berechtigungen unter [Berechtigungen (Datenbank-Engine)](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine), und sehen Sie sich die [Grafik in Postergröße](https://docs.microsoft.com/sql/relational-databases/security/media/database-engine-permissions.png) mit den Berechtigungen an.

## <a name="using-groups"></a>Verwenden von Gruppen

Bei der effizienten Zugriffsverwaltung werden Berechtigungen verwendet, die Active Directory-Sicherheitsgruppen sowie festen oder benutzerdefinierten Rollen anstelle einzelner Benutzer zugewiesen werden.

- Wenn Sie die Active Directory-Authentifizierung verwenden, fügen Sie Azure Active Directory-Benutzer in Azure Active Directory-Sicherheitsgruppen ein. Erstellen Sie einen eigenständigen Datenbankbenutzer für die Gruppe. Weisen Sie mindestens einem Datenbankbenutzer eine benutzerdefinierte Datenbankrolle mit spezifischen Berechtigungen zu, die der jeweiligen Benutzergruppe entspricht.

- Wenn Sie die SQL-Authentifizierung verwenden, erstellen Sie eigenständige Datenbankbenutzer in der Datenbank. Weisen Sie mindestens einem Datenbankbenutzer eine benutzerdefinierte Datenbankrolle mit spezifischen Berechtigungen zu, die der jeweiligen Benutzergruppe entspricht.

  > [!NOTE]
  > Sie können auch nicht eigenständige Datenbankbenutzer gruppieren.

Machen Sie sich mit den folgenden Features zum Einschränken oder Erweitern von Berechtigungen vertraut:

- Mithilfe von [Identitätswechsel](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) und [Modulsignierung](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) können Berechtigungen ohne Sicherheitsbedenken vorübergehend erhöht werden.
- [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) lassen sich die Zeilen beschränken, auf die ein Benutzer Zugriff hat.
- [Daten Masking](sql-database-dynamic-data-masking-get-started.md) kann zum Schutz vor unautorisierter Offenlegung von sensiblen Daten verwendet werden.
- [Gespeicherten Prozeduren](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über alle Sicherheitsfunktionen von SQL-Datenbank finden Sie unter [Securing your SQL Database](sql-database-security-overview.md) (Schützen Ihrer SQL-Datenbank).
