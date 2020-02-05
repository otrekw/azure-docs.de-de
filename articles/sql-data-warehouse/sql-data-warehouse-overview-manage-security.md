---
title: Sichern einer Datenbank
description: Tipps für das Sichern einer Datenbank in Azure SQL Data Warehouse für die Entwicklung von Lösungen.
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/17/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 8e9ab9dddad35708b58d32802452789adf84a19e
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759464"
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>Sichern einer Datenbank in SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Dieser Artikel beschreibt die Grundlagen zum Sichern der Azure SQL Data Warehouse-Datenbank. Insbesondere erhalten Sie in diesem Artikel erste Informationen über Ressourcen zum Einschränken des Zugriffs, zum Schützen von Daten und zum Überwachen von Aktivitäten in einer Datenbank.

## <a name="connection-security"></a>Verbindungssicherheit
Verbindungssicherheit bezieht sich darauf, auf welche Weise Sie die Verbindungen zu Ihrer Datenbank mithilfe von Firewallregeln und Verbindungsverschlüsselung einschränken und sichern.

Firewallregeln werden vom Server und der Datenbank verwendet, um Verbindungsversuche von IP-Adressen abzuwehren, die nicht explizit der weißen Liste hinzugefügt wurden. Damit von Ihrer Anwendung oder von der öffentlichen IP-Adresse Ihres Clientcomputers aus Verbindungen hergestellt werden können, müssen Sie zuerst über das Azure-Portal, über REST-API oder über PowerShell eine Firewallregel auf Serverebene erstellen. 

Eine bewährte Methode besteht darin, die von der Server-Firewall zugelassenen IP-Adressbereiche so weit wie möglich einzuschränken.  Um von Ihrem lokalen Computer aus auf Azure SQL Data Warehouse zuzugreifen, stellen Sie sicher, dass die Firewall im Netzwerk und auf dem lokalen Computer eine ausgehende Kommunikation an TCP-Port 1433 zulässt.  

Azure Synapse verwendet IP-Firewallregeln auf Serverebene. IP-Firewallregeln auf Datenbankebene werden nicht unterstützt. Weitere Informationen finden Sie unter [Firewallregeln für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).

Verbindungen mit Ihrem SQL Data Warehouse sind standardmäßig verschlüsselt.  Das Ändern von Verbindungseinstellungen zum Deaktivieren der Verschlüsselung wird ignoriert.

## <a name="authentication"></a>Authentifizierung
Authentifizierung bezieht sich darauf, auf welche Weise Sie Ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL Data Warehouse unterstützt derzeit die SQL Server-Authentifizierung mit Benutzername und Kennwort sowie mit Azure Active Directory. 

Bei der Erstellung des logischen Servers für die Datenbank haben Sie eine "Server Admin"-Anmeldung mit Benutzername und Kennwort angegeben. Mit diesen Anmeldeinformationen können Sie sich mittels SQL Server-Authentifizierung bei jeder Datenbank auf diesem Server als Datenbankbesitzer bzw. „dbo“ (database owner) authentifizieren.

Allerdings sollten die Benutzer in Ihrer Organisation als bewährte Methode ein anderes Konto für die Authentifizierung verwenden. Auf diese Weise können Sie die Berechtigungen für die Anwendung beschränken und die Risiken schädlicher Aktivitäten reduzieren, falls Ihr Anwendungscode für einen SQL-Injection-Angriff anfällig ist. 

Um einen authentifizierten SQL Server-Benutzer zu erstellen, stellen Sie mit den Anmeldeinformationen des Serveradministrators eine Verbindung mit der **Masterdatenbank** auf dem Server her, und erstellen Sie eine neue Serveranmeldung.  Es ist eine gute Idee, auch einen Benutzer in der Masterdatenbank für Azure Synapse-Benutzer zu erstellen. Das Erstellen eines Benutzers in der Masterdatenbank ermöglicht einem Benutzer, sich mit Tools wie SSMS ohne Angabe eines Datenbanknamens anzumelden.  Außerdem kann er mit dem Objekt-Explorer alle Datenbanken auf einer SQL Server-Instanz anzeigen.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Stellen Sie dann mit den Anmeldedaten für den Serveradministrator eine Verbindung mit der **SQL Data Warehouse-Datenbank** her, und erstellen Sie einen Datenbankbenutzer basierend auf der erstellten Serveranmeldung.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Um einem Benutzer die Berechtigung zu erteilen, zusätzliche Vorgänge wie das Erstellen von Anmeldedaten oder neuer Datenbanken auszuführen, weisen Sie dem Benutzer die Rollen `Loginmanager` und `dbmanager` in der Masterdatenbank zu. 

Weitere Informationen zu diesen zusätzlichen Rollen sowie zur Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Anmeldungen in der Azure SQL-Datenbank](../sql-database/sql-database-manage-logins.md).  Weitere Informationen zum Herstellen einer Verbindung mithilfe von Azure Active Directory finden Sie unter [Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung](sql-data-warehouse-authentication.md).

## <a name="authorization"></a>Autorisierung
Die Autorisierung bezieht sich darauf, was Sie innerhalb einer Datenbank tun können, sobald Sie authentifiziert und verbunden sind. Autorisierungsrechte werden anhand von Rollenmitgliedschaften und -berechtigungen festgelegt. Als bewährte Methode sollten Sie Benutzern nur die minimal erforderlichen Berechtigungen erteilen. Um Rollen zu verwalten, können Sie die folgenden gespeicherten Prozeduren verwenden:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Das Server-Admin-Konto, mit dem Sie eine Verbindung herstellen, ist Mitglied von db_owner und verfügt daher über alle Berechtigungen in der Datenbank. Speichern Sie dieses Konto für die Bereitstellung von Schemaänderungen und andere Verwaltungsvorgänge. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkteren Berechtigungen, um eine Verbindung von Ihrer Anwendung zur Datenbank mit den geringsten Berechtigungen herzustellen, die von Ihrer Anwendung benötigt werden.

Es gibt Möglichkeiten, Benutzerberechtigungen innerhalb eines Data Warehouse weiter einzuschränken:

* Mithilfe von granularen [Berechtigungen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine?view=sql-server-ver15) können Sie steuern, welche Vorgänge in einzelnen Spalten, Tabellen, Sichten, Schemas, Prozeduren und anderen Objekten in der Datenbank ausgeführt werden dürfen. Verwenden Sie präzise Berechtigungen, um die größtmögliche Kontrolle zu haben und die geringstmöglichen Berechtigungen zu erteilen. 
* [Datenbankrollen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles?view=sql-server-ver15) können, mit Ausnahme von „db_datareader“ und „db_datawriter“, dazu verwendet werden, Anwendungsbenutzerkonten mit mehr Berechtigungen oder Verwaltungskonten mit weniger Berechtigungen zu erstellen. Die integrierten festen Datenbankrollen bieten eine einfache Möglichkeit zum Erteilen von Berechtigungen, können jedoch auch dazu führen, dass mehr Berechtigungen als nötig erteilt werden.
* [Gespeicherten Prozeduren](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine?redirectedfrom=MSDN&view=sql-server-ver15) können verwendet werden, um die Aktionen zu begrenzen, die in der Datenbank ausgeführt werden können.

Im folgenden Beispiel wird Lesezugriff für ein benutzerdefiniertes Schema erteilt.
```sql
--CREATE SCHEMA Test
GRANT SELECT ON SCHEMA::Test to ApplicationUser
```

Die Verwaltung von Datenbanken und logischen Servern über das Azure-Portal oder mit der Azure Resource Manager-API wird durch die Rollenzuweisungen Ihres Portal-Benutzerkontos gesteuert. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung im Azure-Portal](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure).

## <a name="encryption"></a>Verschlüsselung
Transparent Data Encryption (TDE) ist ein zusätzlicher Schutz vor der Bedrohung durch schädliche Aktivitäten. Hierzu wird die Verschlüsselung und Entschlüsselung Ihrer ruhenden Daten ausgeführt. Wenn Sie Ihre Datenbank verschlüsseln, werden zugehörige Sicherungen und Transaktionsprotokolle verschlüsselt, ohne dass Änderungen an Ihren Anwendungen erforderlich sind. TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. 

In SQL-Datenbank wird der Datenbankverschlüsselungsschlüssel mit einem integrierten Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden SQL-Datenbank-Server eindeutig. Microsoft tauscht diese Zertifikate mindestens alle 90 Tage automatisch untereinander. Der verwendete Verschlüsselungsalgorithmus ist AES-256. Eine allgemeine Beschreibung von TDE finden Sie unter [Transparent Data Encryption (Transparente Datenverschlüsselung, TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver15).

Sie können Ihre Datenbank mit dem [Azure-Portal](sql-data-warehouse-encryption-tde.md) oder mit [T-SQL](sql-data-warehouse-encryption-tde-tsql.md) verschlüsseln.

## <a name="next-steps"></a>Nächste Schritte
Informationen und Beispiele zum Herstellen einer Verbindung mit Ihrem Warehouse anhand von verschiedenen Protokollen finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse](sql-data-warehouse-connect-overview.md).
