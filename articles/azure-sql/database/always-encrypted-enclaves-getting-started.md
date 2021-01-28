---
title: 'Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank'
description: In diesem Tutorial erfahren Sie, wie Sie eine grundlegende Umgebung für Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank erstellen, Daten direkt verschlüsseln und umfassende vertrauliche Abfragen für verschlüsselte Spalten mithilfe von SQL Server Management Studio (SSMS) ausgeben.
keywords: Verschlüsseln von Daten, SQL-Verschlüsselung, Datenbankverschlüsselung, vertrauliche Daten, Always Encrypted, Secure Enclaves, SGX, Nachweis
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: d9c2bec575f2c7a948f3eb6e65be6a735a3c03e8
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98733809"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-azure-sql-database"></a>Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted mit Secure Enclaves für Azure SQL-Datenbank befindet sich derzeit in der **öffentlichen Vorschauphase**.

In diesem Tutorial werden Ihnen die erste Schritte mit [Always Encrypted mit Secure Enclaves](/sql/relational-databases/security/encryption/always-encrypted-enclaves) in Azure SQL-Datenbank vermittelt. Es wird Folgendes gezeigt:

> [!div class="checklist"]
> - Erstellen einer Umgebung zum Testen und Auswerten von Always Encrypted mit Secure Enclaves
> - Direktes Verschlüsseln von Daten und Ausgeben umfangreicher vertraulicher Abfragen für verschlüsselte Spalten mithilfe von SQL Server Management Studio (SSMS)

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial sind Azure PowerShell und [SSMS](/sql/ssms/download-sql-server-management-studio-ssms) erforderlich.

### <a name="powershell-requirements"></a>PowerShell-Anforderungen

Weitere Informationen zum Installieren und Ausführen von Azure PowerShell finden Sie unter [Dokumentation zu Azure PowerShell](/powershell/azure). 

Mindestens erforderliche Version der Az-Module zur Unterstützung von Nachweisvorgängen:

- Az 4.5.0
- Az.Accounts 1.9.2
- Az.Attestation 0.1.8

Führen Sie den folgenden Befehl aus, um die installierte Version aller Az-Module zu überprüfen:

```powershell
Get-InstalledModule
```

Führen Sie den Befehl `Update-Module` aus, wenn die Versionen nicht die Mindestanforderungen erfüllen.

Im PowerShell-Katalog wurden die TLS-Versionen (Transport Layer Security) 1.0 und 1.1 als veraltet gekennzeichnet. Empfohlen wird TLS 1.2 oder eine höhere Version. Wenn Sie eine niedrigere TLS-Version als 1.2 verwenden, werden unter Umständen die folgenden Fehler angezeigt:

- `WARNING: Unable to resolve package source 'https://www.powershellgallery.com/api/v2'`
- `PackageManagement\Install-Package: No match was found for the specified search criteria and module name.`

Führen Sie den folgenden Befehl vor den Befehlen vom Typ „Install-Module“ aus, um weiterhin mit dem PowerShell-Katalog zu interagieren.

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

### <a name="ssms-requirements"></a>SSMS-Anforderungen

Informationen zum Herunterladen von SSMS finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms).

Die erforderliche Mindestversion von SSMS ist 18.8.


## <a name="step-1-create-a-server-and-a-dc-series-database"></a>Schritt 1: Erstellen eines Servers und einer Datenbank der DC-Serie

 In diesem Schritt erstellen Sie unter Verwendung der Hardwarekonfiguration der DC-Serie einen neuen logischen Azure SQL-Datenbank-Server und eine neue Datenbank. Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank nutzt Intel SGX-Enklaven, die von der Hardwarekonfiguration der DC-Serie unterstützt werden. Weitere Informationen finden Sie unter [DC-Serie](service-tiers-vcore.md#dc-series).

1. Öffnen Sie eine PowerShell-Konsole, und melden Sie sich bei Azure an. [Wechseln Sie ggf. zu dem Abonnement](/powershell/azure/manage-subscriptions-azureps), das Sie für dieses Tutorial verwenden.

  ```PowerShell
  Connect-AzAccount
  $subscriptionId = <your subscription ID>
  Set-AzContext -Subscription $serverSubscriptionId
  ```

2. Erstellen Sie eine Ressourcengruppe für Ihren Datenbankserver. 

  ```powershell
  $serverResourceGroupName = "<server resource group name>"
  $serverLocation = "<Azure region that supports DC-series in SQL Database>"
  New-AzResourceGroup -Name $serverResourceGroupName -Location $serverLocation 
  ```

  > [!IMPORTANT]
  > Sie müssen Ihre Ressourcengruppe in einer Region erstellen, für die die Hardwarekonfiguration der DC-Serie unterstützt wird. Die Liste mit den derzeit unterstützten Regionen finden Sie unter [DC-Serie](service-tiers-vcore.md#dc-series-1).

3. Erstellen Sie einen Datenbankserver. Geben Sie bei entsprechender Aufforderung den Serveradministratornamen und ein Kennwort ein.

  ```powershell
  $serverName = "<server name>" 
  New-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -Location $serverLocation
  ```

4. Erstellen Sie eine Serverfirewallregel, die den Zugriff aus dem angegebenen IP-Adressbereich zulässt.
  
  ```powershell
  # The ip address range that you want to allow to access your server
  $startIp = "<start of IP range>"
  $endIp = "<end of IP range>"
  $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
  ```

5. Weisen Sie dem Server eine verwaltete Systemidentität zu. Sie benötigen sie später, um Ihrem Server Zugriff auf Microsoft Azure Attestation zu gewähren.

  ```powershell
  Set-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -AssignIdentity 
  ```

6. Rufen Sie eine Objekt-ID der Identität ab, die dem Server zugewiesen ist. Speichern Sie die resultierende Objekt-ID. Sie benötigen die ID in einem späteren Abschnitt.

  > [!NOTE]
  > Es kann einige Sekunden dauern, bis die neu zugewiesene verwaltete Systemidentität in Azure Active Directory weitergegeben wird. Gibt das folgende Skript ein leeres Ergebnis zurück, wiederholen Sie den Vorgang.

  ```PowerShell
  $server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 
  $serverObjectId = $server.Identity.PrincipalId
  $serverObjectId
  ```

7. Erstellen Sie eine Datenbank der DC-Serie.

  ```powershell
  $databaseName = "ContosoHR"
  $edition = "GeneralPurpose"
  $vCore = 2
  $generation = "DC"
  New-AzSqlDatabase -ResourceGroupName $serverResourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $edition -Vcore $vCore -ComputeGeneration $generation
  ```

## <a name="step-2-configure-an-attestation-provider"></a>Schritt 2: Konfigurieren eines Nachweisanbieters

In diesem Schritt erstellen und konfigurieren Sie einen Nachweisanbieter in Microsoft Azure Attestation. Dies ist erforderlich, um einen Nachweis für die Secure Enclave-Instanz auf Ihrem Datenbankserver zu erbringen.

1. Kopieren Sie die nachfolgende Nachweisrichtlinie, und speichern Sie die Richtlinie in einer Textdatei (.txt). Informationen zur folgenden Richtlinie finden Sie unter [Erstellen und Konfigurieren eines Nachweisanbieters](always-encrypted-enclaves-configure-attestation.md#create-and-configure-an-attestation-provider).

  ```output
  version= 1.0;
  authorizationrules 
  {
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
  };
  ```

2. Importieren Sie die erforderlichen Versionen von `Az.Accounts` und `Az.Attestation`.  

  ```powershell
  Import-Module "Az.Accounts" -MinimumVersion "1.9.2"
  Import-Module "Az.Attestation" -MinimumVersion "0.1.8"
  ```

3. Erstellen Sie eine Ressourcengruppe für den Nachweisanbieter.

  ```powershell
  $attestationLocation = $serverLocation
  $attestationResourceGroupName = "<attestation provider resource group name>"
  New-AzResourceGroup -Name $attestationResourceGroupName -Location $location  
  ```

4. Erstellen Sie einen Nachweisanbieter. 

  ```powershell
  $attestationProviderName = "<attestation provider name>" 
  New-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName -Location $attestationLocation
  ```

5. Konfigurieren Sie die Nachweisrichtlinie.
  
  ```powershell
  $policyFile = "<the pathname of the file from step 1 in this section"
  $teeType = "SgxEnclave"
  $policyFormat = "Text"
  $policy=Get-Content -path $policyFile -Raw
  Set-AzAttestationPolicy -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName -Tee $teeType -Policy $policy -PolicyFormat  $policyFormat
  ```

6. Gewähren Sie dem logischen Azure SQL-Server Zugriff auf Ihren Nachweisanbieter. In diesem Schritt verwenden Sie die Objekt-ID der verwalteten Dienstidentität, die Sie dem Server zuvor zugewiesen haben.

  ```powershell
  New-AzRoleAssignment -ObjectId $serverObjectId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName  
  ```

7. Rufen Sie die Nachweis-URL ab.

  ```powershell
  $attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
  $attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
  Write-Host "Your attestation URL is: " $attestationUrl 
  ```

8.  Speichern Sie die resultierende Nachweis-URL, die auf eine Nachweisrichtlinie verweist, die Sie für die SGX-Enclave konfiguriert haben. Sie benötigen die Information später. Die Nachweis-URL sollte wie folgt aussehen: `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`

## <a name="step-3-populate-your-database"></a>Schritt 3: Auffüllen Ihrer Datenbank

In diesem Schritt erstellen Sie eine Tabelle und füllen sie mit einigen Daten auf, die Sie später verschlüsseln und abfragen.

1. Öffnen Sie SSMS, und stellen Sie eine Verbindung mit der Datenbank **ContosoHR** auf dem von Ihnen erstellten logischen Azure SQL-Server her, **ohne** Always Encrypted in der Datenbankverbindung zu aktivieren.
    1. Geben Sie im Dialogfeld **Verbindung mit dem Server herstellen** Ihren Servernamen (z. B. *myserver123.database.windows.net*) und anschließend den Benutzernamen und das Kennwort ein, die Sie zuvor konfiguriert haben.
    2. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Verbindungseigenschaften** aus. Wählen Sie unbedingt die Datenbank **ContosoHR** (nicht die Standardmasterdatenbank) aus. 
    3. Wählen Sie die Registerkarte **Always Encrypted** aus.
    4. Stellen Sie sicher, dass das Kontrollkästchen **Always Encrypted aktivieren (Spaltenverschlüsselung)** **nicht** ausgewählt ist.

        ![Herstellen einer Verbindung ohne Always Encrypted](media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    5. Klicken Sie auf **Verbinden**.

2. Erstellen Sie eine neue Tabelle mit dem Namen **Employees**.

    ```sql
    CREATE SCHEMA [HR];
    GO

    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    GO
    ```

3. Fügen Sie der Tabelle **Employees** einige Mitarbeiterdatensätze hinzu.

    ```sql
    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```


## <a name="step-4-provision-enclave-enabled-keys"></a>Schritt 4: Bereitstellen Enclave-fähiger Schlüssel

In diesem Schritt erstellen Sie einen Spaltenhauptschlüssel und einen Spaltenverschlüsselungsschlüssel, die Enclave-Berechnungen zulassen.

1. Erweitern Sie Ihre Datenbank im **Objekt-Explorer** mit der SSMS-Instanz aus dem vorherigen Schritt, und navigieren Sie zu **Sicherheit** > **Always Encrypted-Schlüssel**.
1. Stellen Sie einen neuen Enclave-fähigen Spaltenhauptschlüssel bereit:
    1. Klicken Sie mit der rechten Maustaste auf **Always Encrypted-Schlüssel**, und wählen Sie **Neuer Spaltenhauptschlüssel** aus.
    2. Wählen Sie den Namen des Spaltenhauptschlüssels aus: **CMK1**.
    3. Stellen Sie sicher, dass Sie entweder **Windows-Zertifikatspeicher (Aktueller Benutzer oder lokaler Computer)** oder **Azure Key Vault** auswählen.
    4. Wählen Sie **Enclave-Berechnungen zulassen**.
    5. Wenn Sie Azure Key Vault ausgewählt haben, melden Sie sich bei Azure an, und wählen Sie Ihren Schlüsseltresor aus. Weitere Informationen zum Erstellen eines Schlüsseltresors für Always Encrypted finden Sie unter [Verwalten Ihrer Schlüsseltresore im Azure-Portal](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Wählen Sie Ihr Zertifikat oder Ihren Azure-Schlüsselwertschlüssel aus, wenn bereits vorhanden, oder klicken Sie auf die Schaltfläche **Zertifikat generieren**, um ein neues zu erstellen.
    7. Klicken Sie auf **OK**.

        ![Enclave-Berechnungen zulassen](media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Erstellen Sie einen neuen Enclave-fähigen Spaltenverschlüsselungsschlüssel:

    1. Klicken Sie mit der rechten Maustaste auf **Always Encrypted-Schlüssel**, und wählen Sie **Neuer Spaltenverschlüsselungsschlüssel**.
    2. Geben Sie einen Namen für den neuen Spaltenverschlüsselungsschlüssel ein: **CEK1**.
    3. Wählen Sie in der Dropdownliste **Spaltenhauptschlüssel** den in den vorherigen Schritten erstellten Spaltenhauptschlüssel.
    4. Klicken Sie auf **OK**.

## <a name="step-5-encrypt-some-columns-in-place"></a>Schritt 5: Direktes Verschlüsseln einiger Spalten

In diesem Schritt verschlüsseln Sie die in den Spalten **SSN** und **Salary** gespeicherten Daten in der serverseitigen Enclave und testen dann eine SELECT-Abfrage der Daten.

1. Öffnen Sie eine neue SSMS-Instanz, und stellen Sie eine Verbindung mit Ihrer Datenbank her. Always Encrypted muss dabei für die Datenbankverbindung **aktiviert** sein.
    1. Starten Sie eine neue SSMS-Instanz.
    2. Geben Sie im Dialogfeld **Mit Server verbinden** Ihren Servernamen, eine Authentifizierungsmethode und Ihre Anmeldeinformationen an.
    3. Klicken Sie auf **Optionen >>** , und wählen Sie die Registerkarte **Verbindungseigenschaften** aus. Wählen Sie unbedingt die Datenbank **ContosoHR** (nicht die Standardmasterdatenbank) aus. 
    4. Wählen Sie die Registerkarte **Always Encrypted** aus.
    5. Stellen Sie sicher, dass das Kontrollkästchen **Always Encrypted aktivieren (Spaltenverschlüsselung)** aktiviert ist.
    6. Geben Sie die Enclave-Nachweis-URL an, die Sie mithilfe der Schritte unter [Schritt 2: Konfigurieren eines Nachweisanbieters](#step-2-configure-an-attestation-provider) abgerufen haben. Sehen Sie sich hierzu folgenden Screenshot an:

        ![Herstellen einer Verbindung mit dem Nachweis](media/always-encrypted-enclaves/connect-to-server-configure-attestation.png)

    7. Wählen Sie **Verbinden**.
    8. Wenn Sie aufgefordert werden, die Parametrisierung für Always Encrypted zu aktivieren, wählen Sie **Aktivieren** aus.



1. Öffnen Sie mit derselben SSMS-Instanz (Always Encrypted ist aktiviert) ein neues Abfragefenster, und verschlüsseln Sie die Spalten **SSN** und **Salary**, indem Sie die folgenden Anweisungen ausführen:

    ```sql
    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Achten Sie auf die ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE-Anweisung, die den Abfrageplancache für die Datenbank im obigen Skript löscht. Nachdem Sie die Tabelle bearbeitet haben, müssen Sie die Pläne für alle Batches und gespeicherten Prozeduren leeren, die Zugriff auf die Tabelle haben, um die Informationen für die Parameterverschlüsselung zu aktualisieren. 

1. Um zu überprüfen, ob die Spalten **SSN** und **Salary** nun verschlüsselt sind, öffnen Sie ein neues Abfragefenster in der SSMS-Instanz, **ohne** dass Always Encrypted für die Datenbankverbindung aktiviert ist, und führen Sie die folgende Anweisung aus. Das Abfragefenster sollte verschlüsselte Werte in den Spalten **SSN** und **Salary** zurückgeben. Wenn Sie die gleiche Abfrage mit der SSMS-Instanz ausführen, bei der Always Encrypted aktiviert ist, sollten die Daten entschlüsselt angezeigt werden.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-6-run-rich-queries-against-encrypted-columns"></a>Schritt 6: Ausführen umfangreicher Abfragen für verschlüsselte Spalten

Sie können umfangreiche Abfragen für verschlüsselte Spalten ausführen. Ein Teil der Abfrageverarbeitung wird in der serverseitigen Enclave ausgeführt. 

1. Stellen Sie in der SSMS-Instanz **mit** aktiviertem Always Encrypted sicher, dass auch die Parametrisierung für Always Encrypted aktiviert ist.
    1. Wählen Sie im Hauptmenü von SSMS **Tools** aus.
    2. Wählen Sie **Optionen...** aus.
    3. Navigieren Sie zu **Abfrageausführung** > **SQL Server** > **Erweitert**.
    4. Stellen Sie sicher, dass **Parametrisierung für Always Encrypted aktivieren** aktiviert ist.
    5. Klicken Sie auf **OK**.
2. Öffnen Sie ein neues Abfragefenster, fügen Sie die folgende Abfrage ein, und führen Sie sie anschließend aus. Die Abfrage sollte Klartextwerte und Zeilen zurückgeben, die den angegebenen Suchkriterien entsprechen.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Probieren Sie dieselbe Abfrage erneut in der SSMS-Instanz aus, in der Always Encrypted nicht aktiviert ist. Ein Fehler sollte auftreten.
 
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie dieses Tutorial abgeschlossen haben, können Sie mit einem der folgenden Tutorials fortfahren:
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](/sql/connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps)
- [Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](/sql/relational-databases/security/tutorial-always-encrypted-enclaves-develop-net-framework-apps)
- [Tutorial: Erstellen und Verwenden von Indizes für Enclave-fähige Spalten mit zufälliger Verschlüsselung](/sql/relational-databases/security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption)

## <a name="see-also"></a>Weitere Informationen

- [Konfigurieren und Verwenden von Always Encrypted mit Secure Enclaves](/sql/relational-databases/security/encryption/configure-always-encrypted-enclaves)