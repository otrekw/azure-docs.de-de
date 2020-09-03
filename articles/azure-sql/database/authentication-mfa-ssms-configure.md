---
title: Konfigurieren der mehrstufigen Authentifizierung
titleSuffix: Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics
description: Erfahren Sie, wie die mehrstufige Authentifizierung mit SSMS für Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse Analytics verwendet wird.
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: has-adal-ref, sqldbrb=3
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 08/27/2019
ms.openlocfilehash: 791402f5d9648c8d235f8853de1b6c41f8082e1b
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89018290"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Konfigurieren der mehrstufigen Authentifizierung für SQL Server Management Studio und Azure AD
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

In diesem Artikel wird beschrieben, wie Sie die mehrstufige Authentifizierung (MFA) von Azure Active Directory (Azure AD) mit SQL Server Management Studio (SSMS) verwenden. Die mehrstufige Azure AD-Authentifizierung kann bei Verbindungen von SSMS oder „SqlPackage.exe“ mit [Azure SQL-Datenbank](sql-database-paas-overview.md), [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) und [Azure Synapse Analytics (früher Azure SQL Data Warehouse)](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) verwendet werden. Eine Übersicht über die mehrstufige Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank, SQL Managed Instance und Azure Synapse (SSMS-Unterstützung für MFA)](../database/authentication-mfa-ssms-overview.md).

> [!IMPORTANT]
> Datenbanken in Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse (früher Azure SQL Data Warehouse) werden im restlichen Teil dieses Artikels gemeinsam als Datenbanken bezeichnet. Server bezieht sich auf den [Server](logical-servers.md), der Datenbanken für Azure SQL-Datenbank und Azure Synapse hostet.

## <a name="configuration-steps"></a>Konfigurationsschritte

1. **Konfigurieren eines Azure Active Directory-Verzeichnisses:** Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](../../active-directory/hybrid/whatis-hybrid-identity.md), [Hinzufügen eigener Domänennamen zu Azure AD](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure now supports federation with Windows Server Active Directory (Microsoft Azure unterstützt jetzt den Verbund mit Windows Server Active Directory)](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/) und [Verwalten von Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurieren der mehrstufigen Authentifizierung:** Eine ausführliche Anleitung finden Sie unter [Was ist mehrstufige Azure-Authentifizierung?](../../active-directory/authentication/multi-factor-authentication.md) und [Bedingter Zugriff (mehrstufige Authentifizierung) mit Azure SQL-Datenbank und Data Warehouse](conditional-access-configure.md). (Für eine vollständige Zugangsberechtigung ist Azure Active Directory im Tarif Premium erforderlich. Begrenzte mehrstufige Authentifizierung ist mit Azure AD im Tarif Standard verfügbar.)
3. **Konfigurieren der Azure AD-Authentifizierung:** Eine schrittweise Anleitung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank, SQL Managed Instance oder Azure Synapse mithilfe der Azure Active Directory-Authentifizierung](authentication-aad-overview.md).
4. **Herunterladen von SSMS:** Laden Sie die neueste SSMS-Version von [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx) auf den Clientcomputer herunter.

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Herstellen einer Verbindung mit SSMS mithilfe der universellen Authentifizierung

Die folgenden Schritte veranschaulichen, wie Sie eine Verbindung mit der aktuellen SSMS-Version herstellen.

1. Um eine Verbindung mithilfe der universellen Authentifizierung herzustellen, wählen Sie in SQL Server Management Studio (SSMS) im Dialogfeld **Verbindung mit dem Server herstellen** die Option **Active Directory: universell mit MFA-Unterstützung** aus. (Wenn **Universelle Active Directory-Authentifizierung** angezeigt wird, verwenden Sie nicht die aktuelle Version von SSMS.)

   ![1mfa-universal-connect](./media/authentication-mfa-ssms-configure/mfa-no-tenant-ssms.png)  
2. Geben Sie im Feld **Benutzername** die Azure Active Directory-Anmeldeinformationen im Format `user_name@domain.com` ein.

   ![1mfa-universal-connect-user](./media/authentication-mfa-ssms-configure/1mfa-universal-connect-user.png)
3. Wenn Sie als Gastbenutzer eine Verbindung herstellen, müssen Sie das Feld „AD-Domänenname“ oder „Mandanten-ID“ für Gastbenutzer nicht mehr ausfüllen, da es von SSMS 18. x oder höher automatisch erkannt wird. Weitere Informationen finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank, SQL Managed Instance und Azure Synapse (SSMS-Unterstützung für MFA)](../database/authentication-mfa-ssms-overview.md).

   ![mfa-no-tenant-ssms](./media/authentication-mfa-ssms-configure/mfa-no-tenant-ssms.png)

   Wenn Sie jedoch eine Verbindung als Gastbenutzer mit SSMS 17.x oder früher herstellen, müssen Sie auf **Optionen** klicken und dann im Dialogfeld **Verbindungseigenschaften** das Feld **AD-Domänenname oder Mandanten-ID** ausfüllen.

   ![mfa-tenant-ssms](./media/authentication-mfa-ssms-configure/mfa-tenant-ssms.png)

4. Wählen Sie **Optionen** aus, und geben Sie im Dialogfeld **Optionen** die Datenbank an. (Wenn der verbundene Benutzer ein Gastbenutzer ist (d. h. joe@outlook.com), müssen Sie das Kontrollkästchen aktivieren und den aktuellen AD-Domänennamen oder die Mandanten-ID als Teil der Optionen hinzufügen. Siehe [Universelle Authentifizierung bei SQL-Datenbank und SQL Data Warehouse (SSMS-Unterstützung für MFA)](../database/authentication-mfa-ssms-overview.md). Klicken Sie auf **Verbinden**.  
5. Wenn das Dialogfeld **Bei Ihrem Konto anmelden** angezeigt wird, geben Sie den Kontonamen und das Kennwort Ihrer Azure Active Directory-Identität ein. Es ist kein Kennwort erforderlich, wenn ein Benutzer Mitglied einer Domäne im Verbund mit Azure AD ist.

   ![2mfa-sign-in](./media/authentication-mfa-ssms-configure/2mfa-sign-in.png)  

   > [!NOTE]
   > Bei der universellen Authentifizierung mit einem Konto, das keine MFA erfordert, stellen Sie an diesem Punkt die Verbindung her. Fahren Sie für Benutzer, für die MFA erforderlich ist, mit den folgenden Schritten fort:
   >  

6. Es werden möglicherweise zwei Dialogfelder für die Einrichtung von MFA angezeigt. Dieser einmalige Vorgang richtet sich nach der Administratoreinstellung für MFA und ist daher möglicherweise optional. In einer Domäne mit aktivierter MFA ist dieser Schritt manchmal vordefiniert (beispielsweise muss ein Benutzer sich in der Domäne möglicherweise mit einer Smartcard und einer PIN authentifizieren).

   ![3mfa-setup](./media/authentication-mfa-ssms-configure/3mfa-setup.png)
  
7. Das zweite möglicherweise einmalig angezeigte Dialogfeld ermöglicht es Ihnen, die Details Ihrer Authentifizierungsmethode auszuwählen. Die möglichen Optionen werden von Ihrem Administrator konfiguriert.

   ![4mfa-verify-1](./media/authentication-mfa-ssms-configure/4mfa-verify-1.png)  
8. Das Azure Active Directory-Verzeichnis sendet die Bestätigungsinformationen an Sie. Wenn Sie den Prüfcode erhalten haben, geben Sie ihn in das Feld **Prüfcode eingeben** ein, und klicken Sie auf **Anmelden**.

   ![5mfa-verify-2](./media/authentication-mfa-ssms-configure/5mfa-verify-2.png)  

Wenn die Überprüfung abgeschlossen ist, stellt SSMS normalerweise eine Verbindung her, vorausgesetzt, die Anmeldeinformationen sind gültig und der Firewallzugriff ist möglich.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die mehrstufige Authentifizierung finden Sie unter [Universelle Authentifizierung bei SQL-Datenbank, SQL Managed Instance und Azure Synapse (SSMS-Unterstützung für MFA)](../database/authentication-mfa-ssms-overview.md).  
- Gewähren Sie anderen Benutzern Zugriff auf Ihre Datenbank: [SQL-Datenbankauthentifizierung und -Autorisierung: Gewähren von Zugriff](logins-create-manage.md)  
- Stellen Sie sicher, dass andere Benutzer über die Firewall eine Verbindung herstellen können: [Konfigurieren einer Firewallregel auf Serverebene mit dem Azure-Portal](https://docs.microsoft.com/azure/azure-sql/database/firewall-configure)  
- Bei Verwendung der Authentifizierung **Active Directory: universell mit MFA-Unterstützung** ist ab [SSMS 17.3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) die ADAL-Ablaufverfolgung verfügbar. Die ADAL-Ablaufverfolgung ist standardmäßig deaktiviert. Sie können sie aktivieren, indem Sie unter **Azure-Dienste** > **Azure-Cloud** > **Ablaufverfolgungsebene für ADAL-Ausgabefenster** im Menü **Optionen** die **Tools** verwenden und im Menü **Ansicht** die Option **Ausgabe** aktivieren. Die Ablaufverfolgungen im Ausgabefenster sind verfügbar, wenn Sie die Option **Azure Active Directory** aktivieren.
