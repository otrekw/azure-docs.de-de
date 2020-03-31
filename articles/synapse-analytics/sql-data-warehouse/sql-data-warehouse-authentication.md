---
title: Authentication
description: Es wird beschrieben, wie Sie die Authentifizierung für Azure Synapse Analytics durchführen, indem Sie Azure Active Directory (AAD) oder die SQL Server-Authentifizierung nutzen.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-synapse
ms.openlocfilehash: c998e3789a29d3cfeaf18a583913871f7edc5af1
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350725"
---
# <a name="authenticate-to-azure-synapse-analytics"></a>Authentifizieren bei Azure Synapse Analytics
Es wird beschrieben, wie Sie die Authentifizierung für SQL Analytics durchführen, indem Sie Azure Active Directory (AAD) oder die SQL Server-Authentifizierung nutzen.

Zum Herstellen einer Verbindung mit einem SQL-Pool müssen Sie Sicherheitsanmeldeinformationen zu Authentifizierungszwecken übergeben. Beim Herstellen einer Verbindung werden bestimmte Verbindungseinstellungen im Rahmen der Einrichtung Ihrer Abfragesitzung konfiguriert.  

Weitere Informationen zur Sicherheit und zum Aktivieren von Verbindungen mit Ihrem Data Warehouse finden Sie in der Dokumentation zu [Sichern einer Datenbank](sql-data-warehouse-overview-manage-security.md).

## <a name="sql-authentication"></a>SQL-Authentifizierung
Zum Herstellen einer Verbindung mit Ihrem SQL-Pool müssen Sie die folgenden Informationen angeben:

* Vollqualifizierter Servername
* SQL-Authentifizierung
* Username
* Kennwort
* Standarddatenbank (optional)

Standardmäßig wird die Verbindung mit der *Masterdatenbank* hergestellt und nicht mit Ihrer Benutzerdatenbank. Sie haben zwei Möglichkeiten, um eine Verbindung mit Ihrer Benutzerdatenbank herzustellen:

* Geben Sie die Standarddatenbank an, wenn Sie Ihren Server mit dem SQL Server-Objekt-Explorer in SSDT oder SSMS registrieren, oder geben Sie die Datenbank in Ihrer Verbindungszeichenfolge für die Anwendung an. Beziehen Sie für eine ODBC-Verbindung beispielsweise den InitialCatalog-Parameter ein.
* Markieren Sie die Benutzerdatenbank, bevor Sie eine Sitzung in SSDT erstellen.

> [!NOTE]
> Die Transact-SQL-Anweisung **USE MyDatabase;** wird nicht zum Ändern der Datenbank für eine Verbindung unterstützt. Eine Anleitung zum Herstellen einer Verbindung mit einem SQL-Pool über SSDT finden Sie im Artikel [Abfragen mit Visual Studio](sql-data-warehouse-query-visual-studio.md).
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Authentifizierung über Azure Active Directory (AAD)
[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md)-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit einem SQL-Pool, wozu Identitäten in Azure Active Directory (Azure AD) verwendet werden. Mithilfe der Azure Active Directory-Authentifizierung können Sie die Identitäten von Datenbankbenutzern und andere Microsoft-Dienste an einer zentralen Stelle verwalten. Die zentrale ID-Verwaltung ermöglicht eine einheitliche Verwaltung von Azure Synapse-Benutzern und vereinfacht die Berechtigungsverwaltung. 

### <a name="benefits"></a>Vorteile
Azure Active Directory bietet folgende Vorteile:

* Es wird eine Alternative zur SQL Server-Authentifizierung bereitgestellt.
* Es wird einer unkontrollierten Ausbreitung von Benutzeridentitäten über Datenbankserver hinweg Einhalt geboten.
* Es wird eine Kennwortrotation über eine zentrale Stelle ermöglicht.
* Datenbankberechtigungen können mithilfe externer Gruppen (AAD) verwaltet werden.
* Das Aktivieren der integrierten Windows-Authentifizierung und andere von Azure Active Directory unterstützte Authentifizierungsformen machen das Speichern von Kennwörtern überflüssig.
* Eigenständige Datenbankbenutzer werden zum Authentifizieren von Identitäten auf Datenbankebene verwendet.
* Unterstützt tokenbasierte Authentifizierung für Anwendungen, die eine Verbindung mit dem SQL-Pool herstellen.
* Die Multi-Factor Authentication über die universelle Active Directory-Authentifizierung wird für verschiedene Tools einschließlich [SQL Server Management Studio](../../sql-database/sql-database-ssms-mfa-authentication.md) und [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/azure-active-directory?toc=/azure/sql-data-warehouse/toc.json) unterstützt.

> [!NOTE]
> Azure Active Directory ist immer noch relativ neu und weist einige Einschränkungen auf. Um sicherzustellen, dass Azure Active Directory für Ihre Umgebung geeignet ist, lesen Sie [Funktionen und Einschränkungen von Azure AD](../../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), und beachten Sie insbesondere den Abschnitt „Zusätzliche Überlegungen“.
> 
> 

### <a name="configuration-steps"></a>Konfigurationsschritte
Befolgen Sie diese Schritte zum Konfigurieren der Azure Active Directory-Authentifizierung.

1. Erstellen und Auffüllen eines Azure Active Directory-Verzeichnisses
2. Optional: Zuordnen oder Ändern des aktiven Verzeichnisses für Ihr Azure-Abonnement
3. Erstellen eines Azure Active Directory-Administrators für Azure Synapse
4. Konfigurieren der Clientcomputer
5. Erstellen eigenständiger Datenbankbenutzer in der Datenbank, die Azure AD-Identitäten zugeordnet sind
6. Herstellen einer Verbindung mit Ihrem SQL-Pool unter Verwendung von Azure AD-Identitäten

Azure Active Directory-Benutzer werden derzeit nicht im SSDT-Objekt-Explorer angezeigt. Sie können die Benutzer in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx) anzeigen, um dieses Problem zu umgehen.

### <a name="find-the-details"></a>Suchen der Details
* Die Schritte zum Konfigurieren und Verwenden der Azure Active Directory-Authentifizierung sind fast identisch für die Azure SQL-Datenbank und SQL Analytics in Azure Synapse. Befolgen Sie die ausführlichen Schritte im Thema [Herstellen einer Verbindung mit SQL-Datenbank oder SQL-Pool unter Verwendung der Azure Active Directory-Authentifizierung](../../sql-database/sql-database-aad-authentication.md).
* Erstellen Sie benutzerdefinierte Datenbankrollen, und fügen Sie den Rollen Benutzer hinzu. Weisen Sie anschließend präzise Berechtigungen für die Rollen zu. Weitere Informationen finden Sie unter [Erste Schritte mit Berechtigungen für die Datenbank-Engine](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Abfragen mit Visual Studio und anderen Anwendungen finden Sie unter [Abfragen mit Visual Studio](sql-data-warehouse-query-visual-studio.md).
