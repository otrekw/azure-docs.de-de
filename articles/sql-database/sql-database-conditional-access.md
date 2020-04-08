---
title: Bedingter Zugriff
description: In diesem Artikel erfahren Sie, wie der bedingte Zugriff für Azure SQL-Datenbank und Azure Synapse konfiguriert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.custom: sql-data-warehouse
ms.date: 02/06/2020
tag: azure-synpase
ms.openlocfilehash: cd56ccf2e6a4ceb0d81c25b5b9e795176be66b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124910"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-azure-synapse-analytics"></a>Bedingter Zugriff (MFA) mit Azure SQL-Datenbank und Azure Synapse Analytics

Azure [SQL-Datenbank](sql-database-technical-overview.md), die [verwaltete Instanz](sql-database-managed-instance.md) und [Azure Synapse](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) unterstützen den bedingten Zugriff von Microsoft. 

> [!NOTE]
> Dieses Thema gilt für Azure SQL-Server sowie für Datenbanken von SQL-Datenbank und Azure Synapse, die auf dem Azure SQL-Server erstellt werden. Der Einfachheit halber wird „SQL-Datenbank“ verwendet, wenn sowohl auf SQL-Datenbank als auch auf Azure Synapse verwiesen wird.

Die folgenden Schritte zeigen, wie die SQL-Datenbank zum Erzwingen einer Richtlinie für bedingten Zugriff konfiguriert wird.  

## <a name="prerequisites"></a>Voraussetzungen  
- Sie müssen Ihre SQL-Datenbank oder Ihren SQL-Pool in Azure Synapse zur Unterstützung für die Azure Active Directory-Authentifizierung konfigurieren. Informationen zu den jeweiligen Schritten finden Sie unter [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit der SQL-Datenbank oder Azure Synapse](sql-database-aad-authentication-configure.md).  
- Wenn die mehrstufige Authentifizierung aktiviert ist, müssen Sie eine Verbindung mit einem unterstützten Tool wie der neuesten Version von SSMS herstellen. Weitere Informationen finden Sie unter [Konfigurieren der mehrstufigen Authentifizierung in Azure SQL-Datenbank für SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Konfigurieren der Zertifizierungsstelle für Azure SQL DB/DW  
1. Melden Sie sich beim Portal an, wählen Sie **Azure Active Directory** aus, und wählen Sie dann **Bedingter Zugriff**. Weitere Informationen finden Sie unter [Technische Referenz zum bedingten Azure Active Directory-Zugriff](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   ![Blatt „Bedingter Zugriff“](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. Klicken Sie auf dem Blatt **Richtlinien für bedingten Zugriff** auf **Neue Richtlinie**, geben Sie einen Namen ein, und klicken Sie dann auf **Regeln konfigurieren**.  
3. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**, aktivieren Sie **Benutzer und Gruppen auswählen**, und wählen Sie dann den Benutzer oder die Gruppe für „Bedingter Zugriff“ aus. Klicken Sie auf **Auswählen** und dann auf **Fertig**, um Ihre Auswahl zu bestätigen.  
   ![Auswählen von Benutzern und Gruppen](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. Wählen Sie **Cloud-Apps**, und klicken Sie auf **Apps auswählen**. Alle für „Bedingter Zugriff“ verfügbaren Apps werden angezeigt. Wählen Sie **Azure SQL-Datenbank**, und klicken Sie unten auf **Auswählen** und dann auf **Fertig**.  
   ![Auswählen der SQL-Datenbank](./media/sql-database-conditional-access/select-sql-database.png)  
   Wenn Sie die im dritten Screenshot dargestellte Option **Azure SQL-Datenbank** nicht finden können, führen Sie folgende Schritte aus:   
   - Melden Sie sich über SSMS mit einem AAD-Administratorkonto bei Ihrer Azure SQL DB/DW-Instanz an.  
   - Führen Sie `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER` aus.  
   - Melden Sie sich bei AAD an, und überprüfen Sie, ob Azure SQL-Datenbank und Azure Synapse in den Anwendungen in Ihrem AAD aufgeführt sind.  

5. Wählen Sie **Zugriffskontrollen** und **Gewähren**, und aktivieren Sie dann die Richtlinie, die angewendet werden soll. In diesem Beispiel wählen wir **Mehrstufige Authentifizierung anfordern**.  
   ![Auswählen von „Zugriff gewähren“](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Zusammenfassung  
Die ausgewählte Anwendung (Azure SQL-Datenbank), durch die eine Verbindung mit Azure SQL DB/DW über Azure AD Premium hergestellt werden kann, erzwingt nun die ausgewählte Richtlinie für bedingten Zugriff. Hierfür muss die Option **Mehrstufige Authentifizierung anfordern** aktiviert werden.  
Bei Fragen zu Azure SQL-Datenbank und Azure Synapse bezüglich der mehrstufigen Authentifizierung wenden Sie sich unter MFAforSQLDB@microsoft.com an uns.  

## <a name="next-steps"></a>Nächste Schritte  

Ein Tutorial finden Sie unter [Schützen von Azure SQL-Datenbank](sql-database-security-tutorial.md).
