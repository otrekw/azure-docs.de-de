---
title: Zugreifen auf Azure SQL-Datenbank mit verwalteten Identitäten – Azure Stream Analytics
description: In diesem Artikel wird die Verwendung von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure SQL-Datenbank-Ausgabe beschrieben.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: a8699b3942fe3a4b23f1d72036b7364cdab36f8e
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83651973"
---
# <a name="use-managed-identities-to-access-azure-sql-database-from-an-azure-stream-analytics-job-preview"></a>Zugreifen auf Azure SQL-Datenbank mit verwalteten Identitäten aus einem Azure Stream Analytics-Auftrag (Vorschau)

Azure Stream Analytics unterstützt die [Authentifizierung über verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md) für Azure SQL-Datenbank-Ausgabesenken. Verwaltete Identitäten heben die Einschränkungen benutzerbasierter Authentifizierungsmethoden auf, wie etwa die Notwendigkeit einer erneuten Authentifizierung aufgrund von Kennwortänderungen oder Benutzertoken, die alle 90 Tage ablaufen. Wenn Sie die Notwendigkeit einer manuellen Authentifizierung aufheben, können Ihre Stream Analytics-Bereitstellungen vollständig automatisiert werden.

Bei einer verwalteten Identität handelt es sich um eine in Azure Active Directory registrierte verwaltete Anwendung, die einen bestimmten Stream Analytics-Auftrag repräsentiert. Die verwaltete Anwendung wird zur Authentifizierung bei einer Zielressource verwendet. Dieser Artikel zeigt Ihnen, wie Sie verwaltete Identitäten für Azure SQL-Datenbank-Ausgaben eines Stream Analytics-Auftrags über das Azure-Portal aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Folgendes ist für dieses Feature erforderlich:

- Ein Azure Stream Analytics-Auftrag

- Eine Azure SQL-Datenbank-Ressource

## <a name="create-a-managed-identity"></a>Erstellen einer verwalteten Identität

Erstellen Sie zuerst eine verwaltete Identität für den Azure Stream Analytics-Auftrag.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Azure Stream Analytics-Auftrag.

1. Wählen Sie im linken Navigationsmenü unter **Konfigurieren** die Option **Verwaltete Identität** aus. Aktivieren Sie dann das Kontrollkästchen neben **Systemseitig zugewiesene verwaltete Identität verwenden**, und wählen Sie **Speichern** aus.

   ![Systemseitig zugewiesene verwaltete Identität](./media/sql-db-output-managed-identity/system-assigned-managed-identity.png)


   Ein Dienstprinzipal für die Identität des Stream Analytics-Auftrags wird in Azure Active Directory erstellt. Der Lebenszyklus der neu erstellten Identität wird von Azure verwaltet. Wenn der Stream Analytics-Auftrag gelöscht wird, wird die zugeordnete Identität (also der Dienstprinzipal) von Azure automatisch ebenfalls gelöscht. 

1. Wenn Sie die Konfiguration speichern, wird die Objekt-ID (OID) des Dienstprinzipals als Prinzipal-ID aufgeführt, wie hier gezeigt: 

   ![Als Prinzipal-ID angezeigte Objekt-ID](./media/sql-db-output-managed-identity/principal-id.png)

   Der Dienstprinzipal weist den gleichen Namen auf wie der Stream Analytics-Auftrag. Wenn der Name des Auftrags z. B. *MyASAJob* lautet, erhält auch der Dienstprinzipal den Namen *MyASAJob*.

## <a name="select-an-active-directory-admin"></a>Auswählen eines Active Directory-Administrators

Nachdem Sie eine verwaltete Identität erstellt haben, wählen Sie einen Active Directory-Administrator aus.

1. Navigieren Sie zu Ihrer Azure SQL-Datenbank-Ressource, und wählen Sie die SQL Server-Instanz aus, unter der sich die Datenbank befindet. Der SQL Server-Name ist auf der Seite mit der Ressourcenübersicht neben *Servername* angegeben. 

1. Wählen Sie unter **Einstellungen** die Option **Active Directory-Administrator** aus. Wählen Sie dann **Administrator festlegen** aus. 

   ![Seite „Active Directory-Administrator“](./media/sql-db-output-managed-identity/active-directory-admin-page.png)
 
1. Suchen Sie auf der Seite „Active Directory-Administrator“ nach einem Benutzer oder einer Gruppe, den bzw. die Sie als Administrator für die SQL Server-Instanz festlegen möchten, und klicken Sie auf **Auswählen**.

   ![Hinzufügen eines Active Directory-Administrators](./media/sql-db-output-managed-identity/add-admin.png)

   Auf der Seite „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. Abgeblendete Benutzer oder Gruppen können nicht ausgewählt werden, da sie nicht als Azure AD-Administratoren unterstützt werden. Siehe die Liste von unterstützten Administratoren im Abschnitt  **Funktionen und Einschränkungen von Azure AD** von  [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder Azure Synapse](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations). Die rollenbasierte Zugriffskontrolle (Role-based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben. Der ausgewählte Benutzer bzw. die ausgewählte Gruppe ist außerdem der Benutzer, der den **Benutzer der eigenständigen Datenbank** im nächsten Abschnitt erstellen kann.

1. Wählen Sie auf der Seite **Active Directory-Administrator** die Option **Speichern** aus. Der Prozess zum Ändern des Administrators dauert einige Minuten.

   Beim Einrichten des Azure AD-Administrators darf der Name des neuen Administrators (Benutzer oder Gruppe) in der virtuellen Masterdatenbank nicht als SQL Server-Authentifizierungsbenutzer vorhanden sein. Wenn er vorhanden ist, schlägt die Einrichtung des Azure AD-Administrators fehl. Die Erstellung wird rückgängig gemacht, und es wird angegeben, dass bereits ein Administrator (Name) vorhanden ist. Da der SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, sind alle Versuche erfolglos, unter der Azure AD-Authentifizierung als dieser Benutzer eine Verbindung mit dem Server herzustellen. 

## <a name="create-a-contained-database-user"></a>Erstellen eines Benutzers einer eigenständigen Datenbank

Als Nächstes erstellen Sie einen Benutzer einer eigenständigen Datenbank in Ihrer SQL-Datenbank, der der Azure Active Directory-Identität zugeordnet ist. Der Benutzer einer eigenständigen Datenbank verfügt über keine Anmeldung für die Masterdatenbank, ist aber einer Identität im Verzeichnis zugeordnet, die wiederum mit der Datenbank verknüpft ist. Bei der Azure Active Directory-Identität kann es sich um ein einzelnes Benutzerkonto oder eine Gruppe handeln. In diesem Fall möchten Sie einen Benutzer einer eigenständigen Datenbank für den Stream Analytics-Auftrag erstellen. 

1. Stellen Sie mithilfe von SQL Server Management Studio eine Verbindung mit der SQL-Datenbank her. Der **Benutzername** ist ein Azure Active Directory-Benutzer mit der Berechtigung **ALTER ANY USER**. Der Administrator, den Sie auf dem SQL Server festlegen, ist ein Beispiel dafür. Verwenden Sie die Authentifizierung **Azure Active Directory: universell mit MFA**. 

   ![Verbindung mit SQL Server herstellen](./media/sql-db-output-managed-identity/connect-sql-server.png)

   Der Servername `<SQL Server name>.database.windows.net` kann in verschiedenen Regionen unterschiedlich sein. Beispielsweise sollte in der Region China `<SQL Server name>.database.chinacloudapi.cn` verwendet werden.
 
   Sie können eine bestimmte SQL-Datenbank angeben. Dazu navigieren Sie zu **Optionen > Verbindungseigenschaften > Mit Datenbank verbinden**.  

   ![SQL Server-Verbindungseigenschaften](./media/sql-db-output-managed-identity/sql-server-connection-properties.png)

1. Wenn Sie zum ersten Mal eine Verbindung herstellen, wird möglicherweise das folgende Fenster angezeigt:

   ![Fenster „Neue Firewallregel“](./media/sql-db-output-managed-identity/new-firewall-rule.png)

   1. Navigieren Sie in diesem Fall im Azure-Portal zu Ihrer SQL Server-Ressource. Öffnen Sie im Abschnitt **Sicherheit** die Seite **Firewalls und virtuelle Netzwerke**. 
   1. Fügen Sie eine neue Regel mit einem beliebigen Regelnamen hinzu.
   1. Verwenden Sie die *Von*-IP-Adresse im Fenster **Neue Firewallregel** als *Start-IP*.
   1. Verwenden Sie die *An*-IP-Adresse im Fenster **Neue Firewallregel** als *End-IP*. 
   1. Wählen Sie **Speichern** aus, und versuchen Sie erneut, eine Verbindung von SQL Server Management Studio herzustellen. 

1. Sobald eine Verbindung hergestellt ist, erstellen Sie den Benutzer einer eigenständigen Datenbank. Mit dem folgenden SQL-Befehl wird ein Benutzer einer eigenständigen Datenbank erstellt, der denselben Namen wie der Stream Analytics-Auftrag hat. Stellen Sie sicher, dass Sie *ASA_JOB_NAME* in eckige Klammern einschließen. Verwenden Sie die folgende T-SQL-Syntax, und führen Sie die Abfrage aus. 

   ```sql
   CREATE USER [ASA_JOB_NAME] FROM EXTERNAL PROVIDER; 
   ```

1. Damit Microsoft Azure Active Directory überprüfen kann, ob der Stream Analytics-Auftrag Zugriff auf die SQL-Datenbank hat, müssen wir Azure Active Directory die Erlaubnis zur Kommunikation mit der Datenbank erteilen. Navigieren Sie hierzu im Azure-Portal erneut zur Seite „Firewalls und virtuelles Netzwerk“, und aktivieren Sie die Option „Azure-Diensten und -Ressourcen den Zugriff auf diesen Server erlauben“. 

   ![Firewall und virtuelles Netzwerk](./media/sql-db-output-managed-identity/allow-access.png)

## <a name="grant-stream-analytics-job-permissions"></a>Erteilen von Berechtigungen für einen Stream Analytics-Auftrag

Nachdem Sie einen Benutzer einer eigenständigen Datenbank erstellt und Zugriff auf Azure-Dienste erteilt haben, wie im vorherigen Abschnitt beschrieben, hat Ihr Stream Analytics-Auftrag von der verwalteten Identität die Berechtigung, mithilfe der verwalteten Identität eine **VERBINDUNG** mit Ihrer SQL-Datenbank herzustellen. Es empfiehlt sich, dem Stream Analytics-Auftrag die Berechtigungen SELECT und INSERT zu erteilen, da diese später im Stream Analytics-Workflow benötigt werden. Mithilfe der **SELECT**-Berechtigung kann der Auftrag seine Verbindung mit der Tabelle in der SQL-Datenbank testen. Die Berechtigung **INSERT** ermöglicht das Testen von End-to-End-Abfragen in Stream Analytics, sobald Sie eine Eingabe und die SQL-Datenbankausgabe konfiguriert haben. Sie können dem Auftrag diese Berechtigungen mithilfe von SQL Server Management Studio erteilen. Weitere Informationen finden Sie in der Referenz zu [GRANT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql?view=sql-server-ver15).

Um nur Berechtigung für eine bestimmte Tabelle oder ein bestimmtes Objekt in der Datenbank zu erteilen, verwenden Sie die folgende T-SQL-Syntax, und führen Sie die Abfrage aus. 

```sql
GRANT SELECT, INSERT ON OBJECT::TABLE_NAME TO ASA_JOB_NAME; 
```

Alternativ können Sie in SQL Server Management Studio mit der rechten Maustaste auf Ihre SQL-Datenbank klicken und **Eigenschaften > Berechtigungen** auswählen. Im Berechtigungsmenü wird der zuvor hinzugefügte Stream Analytics-Auftrag angezeigt, und Sie können nach Belieben manuell Berechtigungen erteilen oder verweigern.

## <a name="create-an-azure-sql-database-output"></a>Erstellen einer Azure SQL-Datenbank-Ausgabe

Nachdem die verwaltete Identität konfiguriert wurde, können Sie dem Stream Analytics-Auftrag die Azure SQL-Datenbank als Ausgabe hinzufügen.

Vergewissern Sie sich, dass Sie in Ihrer SQL-Datenbank eine Tabelle mit dem passenden Ausgabeschema erstellt haben. Der Name dieser Tabelle ist eine der erforderlichen Eigenschaften, die eingesetzt werden müssen, wenn Sie dem Stream Analytics-Auftrag die SQL-Datenbank-Ausgabe hinzufügen. Achten Sie außerdem darauf, dass der Auftrag die Berechtigungen **SELECT** und **INSERT** besitzt, um die Verbindung zu testen und Stream Analytics-Abfragen auszuführen. Lesen Sie den Abschnitt [Erteilen von Berechtigungen für einen Stream Analytics-Auftrag](#grant-stream-analytics-job-permissions), wenn Sie dies noch nicht getan haben. 

1. Kehren Sie zum Stream Analytics-Auftrag zurück, und navigieren Sie unter **Auftragstopologie** zur Seite **Ausgaben**. 

1. Wählen Sie **Hinzufügen > SQL-Datenbank** aus. Wählen Sie im Fenster mit den Ausgabeeigenschaften der SQL-Datenbank-Ausgabesenke in der Dropdownliste mit Authentifizierungsmodi die Option **Verwaltete Identität** aus.

1. Geben Sie die restlichen Eigenschaften an. Weitere Informationen zum Erstellen einer SQL-Datenbank-Ausgabe finden Sie unter [Erstellen einer SQL-Datenbank-Ausgabe mit Stream Analytics](stream-analytics-define-outputs.md#sql-database). Wenn Sie fertig sind, wählen Sie **Speichern** aus. 

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zu den Ausgaben von Azure Stream Analytics](stream-analytics-define-outputs.md)
* [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](stream-analytics-sql-output-perf.md)
