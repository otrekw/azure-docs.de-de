---
title: Transparent Data Encryption
description: Eine Übersicht über transparente Datenverschlüsselung für SQL-Datenbank und Synapse SQL in Azure Synapse Analytics. Das Dokument behandelt die Vorteile dieser Verschlüsselung sowie die Konfigurationsoptionen. Diese umfassen die von einem Dienst verwaltete transparente Datenverschlüsselung und Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 04/10/2020
ms.openlocfilehash: 87abdb37ff7773b0205a2ce3ee21689a10c459d7
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113551"
---
# <a name="transparent-data-encryption-for-sql-database-and-azure-synapse"></a>Transparente Datenverschlüsselung für SQL-Datenbank und Azure Synapse

Die [transparente Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) (Transparent Data Encryption, TDE) verschlüsselt ruhende Daten und trägt so zum Schutz von Azure SQL-Datenbank, verwalteten Azure SQL-Instanzen und Synapse SQL in Azure Synapse Analytics vor Bedrohungen durch schädliche Offlineaktivitäten bei. TDE ver- und entschlüsselt die Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit, ohne dass Änderungen an der Anwendung erforderlich sind. TDE ist für alle neu bereitgestellten Azure SQL-Datenbank-Instanzen standardmäßig aktiviert und muss für ältere Datenbanken – Azure SQL-Datenbank, verwaltete Azure SQL-Instanz oder Azure Synapse – manuell aktiviert werden.

Die TDE führt die E/A-Verschlüsselung und -Entschlüsselung der Daten und auf Seitenebene durch. Jede Seite wird entschlüsselt, wenn sie in den Speicher gelesen wird, und dann verschlüsselt, bevor sie auf den Datenträger geschrieben wird. TDE verschlüsselt den Speicher einer gesamten Datenbank mithilfe eines symmetrischen Schlüssels, der als Datenbank-Verschlüsselungsschlüssel (Database Encryption Key, DEK) bezeichnet wird. Beim Datenbankstart wird der verschlüsselte DEK entschlüsselt und dann für die Entschlüsselung und erneute Verschlüsselung der Datenbankdateien im SQL Server-Datenbank-Engine-Prozess verwendet. Der DEK ist durch die TDE-Schutzvorrichtung geschützt. Beim TDE-Schutzvorrichtung handelt es sich entweder um ein von einem Dienst verwaltetes Zertifikat (dienstseitig verwaltete transparente Datenverschlüsselung) oder um einen asymmetrischen Schlüssel, der in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault) gespeichert ist (kundenseitig verwaltete transparente Datenverschlüsselung). 

Bei Azure SQL-Datenbank und Azure Synapse ist die TDE-Schutzvorrichtung auf der logischen SQL Server-Ebene festgelegt und wird von allen Datenbanken geerbt, die diesem Server zugeordnet sind. Bei einer verwalteten Azure SQL-Instanz (BYOK-Feature in der Vorschau) ist die TDE-Schutzvorrichtung auf Instanzebene festgelegt und wird von allen verschlüsselten Datenbanken in dieser Instanz geerbt. In diesem Dokument bezieht sich der Begriff *Server* sowohl auf den Server als auch die Instanz (sofern nicht anders angegeben).

> [!IMPORTANT]
> Alle neu erstellten Azure SQL-Datenbank-Instanzen werden standardmäßig mithilfe der von einem Dienst verwalteten transparenten Datenverschlüsselung verschlüsselt. Vorhandene SQL-Datenbanken, die vor Mai 2017 erstellt wurden, und SQL-Datenbanken, die durch Wiederherstellung, Georeplikation und Datenbankkopie erstellt wurden, sind standardmäßig nicht verschlüsselt. Vorhandene Datenbanken verwalteter Instanzen, die vor Februar 2019 erstellt wurden, sind standardmäßig nicht verschlüsselt. Datenbanken verwalteter Instanzen, die durch Wiederherstellen erstellt wurden, erben den Verschlüsselungsstatus von der Quelle.

> [!NOTE]
> TDE kann nicht zum Verschlüsseln der **master**-Datenbank in SQL-Datenbank verwendet werden.  Die **master**-Datenbank enthält Objekte, die zum Ausführen der TDE-Vorgänge für die Benutzerdatenbanken erforderlich sind.


## <a name="service-managed-transparent-data-encryption"></a>Von einem Dienst verwaltete transparente Datenverschlüsselung

In Azure ist die TDE standardmäßig so eingerichtet, dass der DEK durch ein integriertes Serverzertifikat geschützt ist. Das integrierte Serverzertifikat ist für jeden Server eindeutig, und der verwendete Verschlüsselungsalgorithmus ist AES 256. Wenn sich eine Datenbank in einer Georeplikationsbeziehung befindet, werden sowohl die primäre als auch die sekundäre Geodatenbank vom übergeordneten Serverschlüssel der primären Datenbank geschützt. Sind zwei Datenbanken mit dem gleichen Server verbunden, verwenden sie auch das gleiche integrierte Zertifikat.  Microsoft rotiert diese Zertifikate gemäß der internen Sicherheitsrichtlinie automatisch, und der Stammschlüssel wird von einem Microsoft-internen Geheimnisspeicher geschützt.  Kunden können die Compliance von SQL-Datenbank mit internen Sicherheitsrichtlinien in Überwachungsberichten unabhängiger Drittanbieter überprüfen, die in [Microsoft Trust Center](https://servicetrust.microsoft.com/) zur Verfügung stehen.

Microsoft verschiebt und verwaltet auch die Schlüssel nahtlos, die für die Georeplikation und Wiederherstellung benötigt werden.


## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Vom Kunden verwaltete Transparent Data Encryption – BYOK (Bring Your Own Key)

Die vom Kunden verwaltete transparente Datenverschlüsselung wird auch als BYOK-Unterstützung für TDE bezeichnet. In diesem Szenario ist die TDE-Schutzvorrichtung, die den DEK verschlüsselt, ein kundenseitig verwalteter asymmetrischer Schlüssel, der in einer kundeneigenen und kundenseitig verwalteten Azure Key Vault-Instanz – dem cloudbasierten externen Schlüsselverwaltungssystem von Azure – gespeichert ist und diesen Schlüsseltresor niemals verlässt. Die TDE-Schutzvorrichtung kann [vom Schlüsseltresor generiert oder von einem lokalen HSM-Gerät (Hardwaresicherheitsmodul) in den Schlüsseltresor übertragen](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) werden. SQL-Datenbank müssen die Berechtigungen für den Schlüsseltresor des Kunden gewährt werden, damit der DEK ver- und entschlüsselt werden kann. Wenn die Berechtigungen des logischen SQL-Servers für den Schlüsseltresor widerrufen werden, ist kein Zugriff mehr auf die Datenbank möglich, und alle Daten bleiben verschlüsselt.

Mit TDE und Azure Key Vault-Integration können Benutzer Aufgaben der Schlüsselverwaltung steuern, z.B. Schlüsselrotationen, Schlüsseltresorberechtigungen und Schlüsselsicherungen. Außerdem können sie die Überwachung und Berichterstellung für alle TDE-Schutzvorrichtungen aktivieren, die Azure Key Vault-Funktionalität nutzen. Key Vault bietet eine zentrale Schlüsselverwaltung, verwendet streng überwachte HSMs und ermöglicht die Aufgabentrennung zwischen dem Verwalten von Schlüsseln und Daten, um Sicherheitsrichtlinien einzuhalten.
Weitere Informationen zu BYOK für Azure SQL-Datenbank und Azure Synapse finden Sie unter [Transparente Datenverschlüsselung mit Azure Key Vault-Integration](transparent-data-encryption-byok-azure-sql.md).

Wenn Sie mit der Verwendung von TDE mit Azure Key Vault-Integration beginnen möchten, sehen Sie sich die Schrittanleitung [Aktivieren der transparenten Datenverschlüsselung mithilfe Ihres eigenen Schlüssels aus Key Vault](transparent-data-encryption-byok-azure-sql-configure.md) an.

## <a name="move-a-transparent-data-encryption-protected-database"></a>Verschieben einer durch die transparente Datenverschlüsselung gesicherte Datenbank

Für Vorgänge in Azure müssen Datenbanken nicht entschlüsselt werden. Die TDE-Einstellungen für die Quelldatenbank oder die primäre Datenbank werden transparent an das Ziel vererbt. Dies gilt für folgende Vorgänge:

- Geowiederherstellung
- Self-Service-Point-in-Time-Wiederherstellung
- Wiederherstellung einer gelöschten Datenbank
- Aktive Georeplikation
- Erstellung einer Datenbankkopie
- Wiederherstellen einer Sicherungsdatei in der verwalteten Azure SQL-Instanz

> [!IMPORTANT]
> Die Verwendung einer manuellen Kopiesicherung einer Datenbank, die über einen vom Dienst verwalteten TDE-Vorgang verschlüsselt wurde, ist auf einer verwalteten Azure SQL-Instanz nicht zulässig, da nicht auf das für die Verschlüsselung genutzte Zertifikat zugegriffen werden kann. Verwenden Sie die Point-in-Time-Wiederherstellung, um diese Art von Datenbank auf eine andere verwaltete Instanz zu verschieben.

Wenn Sie eine TDE-geschützte Datenbank exportieren, wird der exportierte Inhalt der Datenbank nicht verschlüsselt. Diese exportierten Inhalte werden in unverschlüsselten BACPAC-Dateien gespeichert. Achten Sie darauf, die BACPAC-Dateien in geeigneter Weise zu schützen und TDE zu aktivieren, sobald der Import der neuen Datenbank abgeschlossen ist.

Wenn die BACPAC-Datei z.B. von einer lokalen Instanz von SQL Server exportiert wird, erfolgt keine automatische Verschlüsselung des importierten Inhalts der neuen Datenbank. Wenn die BACPAC-Datei auf eine lokale Instanz von SQL Server exportiert wird, erfolgt ebenfalls keine automatische Verschlüsselung der neuen Datenbank.

Eine Ausnahme besteht im Exportieren nach und aus SQL-Datenbank. TDE ist in der neuen Datenbank aktiviert, die BACPAC-Datei selbst ist allerdings noch nicht verschlüsselt.


## <a name="manage-transparent-data-encryption"></a>Verwalten von Transparent Data Encryption
# <a name="portal"></a>[Portal](#tab/azure-portal)
Verwalten Sie TDE im Azure-Portal.

Um TDE über das Azure-Portal zu konfigurieren, müssen Sie als Azure-Besitzer, Azure-Mitwirkender oder SQL-Sicherheitsmanager verbunden sein.

Sie aktivieren und deaktivieren TDE auf Datenbankebene. Um TDE in einer Datenbank zu aktivieren, wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Administrator- oder Azure-Mitwirkender-Konto an. Suchen Sie die TDE-Einstellungen in Ihrer Benutzerdatenbank. Standardmäßig wird die von einem Dienst verwaltete transparente Datenverschlüsselung verwendet. Für den Server, der die Datenbank enthält, wird automatisch ein TDE-Zertifikat generiert. Bei einer verwalteten Azure SQL-Instanz verwenden Sie T-SQL, um TDE in einer Datenbank zu aktivieren oder deaktivieren.

![Von einem Dienst verwaltete transparente Datenverschlüsselung](./media/transparent-data-encryption-azure-sql/service-managed-transparent-data-encryption.png)  

Den TDE-Hauptschlüssel, der auch als TDE-Schutzvorrichtung bezeichnet wird, legen Sie auf Serverebene fest. Öffnen Sie die TDE-Einstellungen in Ihrem Server, um TDE mit der BYOK-Unterstützung zu verwenden und Ihre Datenbanken mit einem Schlüssel aus Key Vault zu schützen.

![Transparente Datenverschlüsselung mit Bring Your Own Key-Unterstützung](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Verwalten Sie TDE mit PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind im Wesentlichen identisch.

Um TDE über PowerShell zu konfigurieren, müssen Sie als Azure-Besitzer, Azure-Mitwirkender oder SQL-Sicherheitsmanager verbunden sein.

### <a name="cmdlets-for-azure-sql-database-and-azure-synapse"></a>Cmdlets für Azure SQL-Datenbank und Azure Synapse

Verwenden Sie die folgenden Cmdlets für Azure SQL-Datenbank und Azure Synapse:

| Cmdlet | BESCHREIBUNG |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Aktiviert oder deaktiviert die transparente Datenverschlüsselung für eine Datenbank|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Ruft den Status der transparenten Datenverschlüsselung für eine Datenbank ab |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Überprüft den Verschlüsselungsfortschritt für eine Datenbank |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |Fügt einer SQL Server-Instanz einen Key Vault-Schlüssel hinzu |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Ruft die Key Vault-Schlüssel für einen Azure SQL-Datenbank-Server ab  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |Legt die Schutzvorrichtung der transparenten Datenverschlüsselung für eine SQL Server-Instanz fest |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Ruft die Schutzvorrichtung für die transparente Datenverschlüsselung ab |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |Entfernt einen Key Vault-Schlüssel aus einer SQL Server-Instanz |
|  | |

> [!IMPORTANT]
> Verwenden Sie bei einer verwalteten Azure SQL-Instanz den T-SQL-Befehl [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database), um die TDE auf Datenbankebene zu aktivieren oder deaktivieren, und sehen Sie sich das [PowerShell-Beispielskript](transparent-data-encryption-byok-azure-sql-configure.md) an, um TDE auf Instanzebene zu verwalten.

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)
Verwalten Sie TDE mithilfe von Transact-SQL.

Stellen Sie mit einem Konto, das ein Administratorkonto oder ein Mitglied der Rolle **dbmanager** in der Masterdatenbank ist, eine Verbindung mit der Datenbank her.

| Get-Help | BESCHREIBUNG |
| --- | --- |
| [ALTER DATABASE (Azure SQL-Datenbank)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | Mit SET ENCRYPTION ON/OFF wird eine Datenbank verschlüsselt oder entschlüsselt. |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Gibt Informationen über den Verschlüsselungsstatus einer Datenbank und die ihr zugeordneten Verschlüsselungsschlüssel zurück. |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Gibt Informationen über den Verschlüsselungsstatus jedes Azure Synapse-Knotens und der zugehörigen Verschlüsselungsschlüssel der Datenbank zurück. |
|  | |

Sie können die TDE-Schutzvorrichtung nicht per Transact-SQL durch einen Schlüssel von Key Vault ersetzen. Verwenden Sie PowerShell oder das Azure-Portal.

# <a name="rest-api"></a>[REST-API](#tab/azure-RESTAPI)
Verwalten Sie TDE mithilfe der REST-API.

Um TDE über die REST-API zu konfigurieren, müssen Sie als Azure-Besitzer, Azure-Mitwirkender oder SQL-Sicherheitsmanager verbunden sein.
Verwenden Sie die folgenden Befehle für Azure SQL-Datenbank und Azure Synapse:

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[Server erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Fügt einer SQL Server-Instanz, die für den Zugriff auf Key Vault verwendet wird, Azure Active Directory-Identität hinzu.|
|[Serverschlüssel erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Fügt einer SQL Server-Instanz einen Key Vault-Schlüssel hinzu|
|[Serverschlüssel entfernen](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Entfernt einen Key Vault-Schlüssel aus einer SQL Server-Instanz|
|[Serverschlüssel abrufen](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Ruft einen bestimmten Key Vault-Schlüssel aus einer SQL Server-Instanz ab|
|[Serverschlüssel nach Server auflisten](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Ruft die Key Vault-Schlüssel für eine SQL Server-Instanz ab |
|[Verschlüsselungsschutzvorrichtung erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Legt die TDE-Schutzvorrichtung für eine SQL Server-Instanz fest.|
|[Verschlüsselungsschutzvorrichtung abrufen](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Ruft die TDE-Schutzvorrichtung für eine SQL Server-Instanz ab.|
|[Verschlüsselungsschutzvorrichtungen nach Server auflisten](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Ruft die TDE-Schutzvorrichtungen für eine SQL Server-Instanz ab. |
|[Transparent Data Encryption-Konfiguration erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Aktiviert oder deaktiviert TDE für eine Datenbank.|
|[Transparent Data Encryption-Konfiguration abrufen](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Ruft die TDE-Konfiguration für eine Datenbank ab.|
|[Ergebnisse der Transparent Data Encryption-Konfiguration auflisten](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Ruft das Verschlüsselungsergebnis für eine Datenbank ab|

## <a name="see-also"></a>Weitere Informationen
- SQL Server kann bei der Ausführung auf einem virtuellen Azure-Computer auch einen asymmetrischen Schlüssel aus Key Vault verwenden. Die Konfigurationsschritte unterscheiden sich von der Verwendung eines asymmetrischen Schlüssels in SQL-Datenbank und einer verwalteten SQL-Instanz. Weitere Informationen finden Sie unter [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).
- Eine allgemeine Beschreibung von TDE finden Sie unter [Transparente Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Weitere Informationen TDE mit BYOK-Unterstützung für Azure SQL-Datenbank, verwaltete Azure SQL-Instanzen und Azure Synapse finden Sie unter [Transparente Datenverschlüsselung mit Bring Your Own Key-Unterstützung](transparent-data-encryption-byok-azure-sql.md).
- Wenn Sie mit der Verwendung von TDE mit BYOK-Unterstützung beginnen möchten, sehen Sie sich die Schrittanleitung [Aktivieren von Transparent Data Encryption mithilfe eines vom Kunden verwalteten Azure Key Vault-Schlüssels](transparent-data-encryption-byok-azure-sql-configure.md) an.
- Weitere Informationen zu Key Vault finden Sie unter [Sicherer Zugriff auf einen Schlüsseltresor](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
