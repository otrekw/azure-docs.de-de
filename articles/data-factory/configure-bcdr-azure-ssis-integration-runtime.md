---
title: Konfigurieren von Azure-SSIS Integration Runtime für Failover von SQL-Datenbank
description: In diesem Artikel wird beschrieben, wie Azure-SSIS Integration Runtime mit Georeplikation für die Azure SQL-Datenbank und Failover für die SSISDB-Datenbank konfiguriert wird.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: powershell
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/09/2020
ms.openlocfilehash: b4b679b15092531ff9553d221f23d7f030fc1def
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592089"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Konfigurieren von Azure-SSIS Integration Runtime mit Georeplikation der Azure SQL-Datenbank und Failover

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird beschrieben, wie Azure-SSIS Integration Runtime mit Georeplikation für die Azure SQL-Datenbank und die SSISDB-Datenbank konfiguriert wird. Wenn ein Failover auftritt, können Sie sicherstellen, dass Azure-SSIS IR mit der sekundären Datenbank weiterhin funktioniert.

Weitere Informationen zu Georeplikation und Failover für die SQL-Datenbank finden Sie unter [Übersicht: Aktive Georeplikation und Gruppen für automatisches Failover](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-ssis-ir-failover-with-azure-sql-database-managed-instance"></a>Azure-SSIS IR-Failover mit verwalteter Azure SQL-Datenbank-Instanz

### <a name="prerequisites"></a>Voraussetzungen

Verwaltete Azure SQL-Datenbank-Instanzen verwenden den **Datenbankhauptschlüssel** zum Sichern von Daten, Anmeldeinformationen und Verbindungsinformationen, die in der Datenbank gespeichert sind. Um die automatische Entschlüsselung des Hauptschlüssels zu aktivieren, wird eine Kopie des Schlüssels mit dem **Diensthauptschlüssel** verschlüsselt. Der Diensthauptschlüssel wird jedoch nicht in der Failovergruppe repliziert. Deshalb müssen Sie ein zusätzliches Kennwort für die primäre und die sekundäre Instanz der Verschlüsselung des Datenbankhauptschlüssels nach dem Failover hinzufügen.

1. Führen Sie den unten folgenden Befehl auf der SSISDB für die primäre Instanz aus. In diesem Schritt wird ein neues Verschlüsselungskennwort hinzugefügt.

    ```sql
    ALTER MASTER KEY ADD ENCRYPTION BY PASSWORD = 'password'
    ```

2. Erstellen Sie eine Failovergruppe für die verwaltete Azure SQL-Datenbank-Instanz.

3. Führen Sie **sp_control_dbmasterkey_password** auf der sekundären Instanz mit dem neuen Verschlüsselungskennwort aus.

    ```sql
    EXEC sp_control_dbmasterkey_password @db_name = N'SSISDB',   
        @password = N'<password>', @action = N'add';  
    GO
    ```

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Szenario 1: Azure-SSIS IR verweist auf einen Listenerendpunkt mit Lese-/Schreibberechtigung

Wenn Azure-SSIS IR auf den Listenerendpunkt mit Lese-/Schreibzugriff zeigen soll, müssen Sie zuerst auf den primären Serverendpunkt verweisen. Nachdem Sie SSISDB zur Failovergruppe hinzugefügt haben, können Sie den Listenerendpunkt mit Lese-/Schreibzugriff ändern und Azure-SSIS IR neu starten.

#### <a name="solution"></a>Lösung

Bei einem Failover müssen Sie die folgenden Schritte ausführen:

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Geben Sie eine neue Region, eine VNET-Instanz und SAS-URI-Informationen zur benutzerdefinierten Einrichtung für die sekundäre Instanz in Azure-SSIS IR an. Wenn Azure-SSIS IR auf einen Listener mit Lese-/Schreibzugriff verweist und der Endpunkt für Azure-SSIS IR transparent ist, müssen Sie den Endpunkt nicht bearbeiten.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Starten Sie Azure-SSIS IR neu.

### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Szenario 2: Azure-SSIS IR verweist auf den primären Serverendpunkt

Dieses Szenario ist geeignet, wenn Azure-SSIS IR auf den primären Serverendpunkt verweist.

#### <a name="solution"></a>Lösung

Bei einem Failover müssen Sie die folgenden Schritte ausführen:

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Bearbeiten Sie Azure-SSIS IR mit der neuen Region, dem Endpunkt und den VNET-Informationen der sekundären Instanz.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -VNetId "new VNet" `
                -Subnet "new subnet" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Starten Sie Azure-SSIS IR neu.

### <a name="scenario-3---azure-ssis-ir-is-pointing-to-public-endpoint-of-azure-sql-database-managed-instance"></a>Szenario 3: Azure-SSIS IR verweist auf den öffentlichen Endpunkt der verwalteten Azure SQL-Datenbank-Instanz

Das Szenario eignet sich dann, wenn Azure-SSIS IR auf den öffentlichen Endpunkt der verwalteten Azure SQL-Datenbank-Instanz verweist und nicht mit einer VNET-Instanz verknüpft ist. Der einzige Unterschied zwischen Szenario 2 und diesen Szenarios besteht darin, dass Sie nach einem Failover keine VNET-Informationen von Azure-SSIS IR bearbeiten müssen.

#### <a name="solution"></a>Lösung

Bei einem Failover müssen Sie die folgenden Schritte ausführen:

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Geben Sie für Azure-SSIS IR die neue Region und die Endpunktinformationen der sekundären Instanz an.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Starten Sie Azure-SSIS IR neu.

### <a name="scenario-4---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Szenario 4: Anfügen einer vorhandenen SSISDB (SSIS-Katalog) an eine neue Azure-SSIS IR-Instanz

Dieses Szenario ist geeignet, wenn Sie eine neue Azure-SSIS IR-Instanz in der sekundären Region bereitstellen möchten, oder wenn Sie möchten, dass Ihre SSISDB in einer neuen Region weiterhin mit einer neuen Azure-SSIS IR-Instanz arbeitet, wenn ein ADF- oder Azure-SSIS IR-Notfall in der aktuellen Region auftritt.

#### <a name="solution"></a>Lösung

Bei einem Failover müssen Sie die folgenden Schritte ausführen:

> [!NOTE]
> Schritt 4 (Erstellung der IR) muss über PowerShell erfolgen. Im Azure-Portal wird der Fehler gemeldet, dass SSISDB bereits vorhanden ist.

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Führen Sie die gespeicherte Prozedur aus, um die Metadaten in SSISDB so zu aktualisieren, dass Verbindungen von **\<new_data_factory_name\>** und **\<new_integration_runtime_name\>** akzeptiert werden.
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Erstellen Sie eine neue Data Factory mit dem Namen **\<new_data_factory_name\>** in der neuen Region. Weitere Informationen finden Sie unter „Erstellen einer Data Factory“.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                      -Location "new region"`
                      -Name "<new_data_factory_name>"
    ```
    
    Weitere Informationen zu diesem PowerShell-Befehl finden Sie unter [Erstellen einer Azure Data Factory mithilfe von PowerShell](quickstart-create-data-factory-powershell.md).

4. Erstellen Sie eine neue Azure-SSIS IR mit dem Namen **\<new_integration_runtime_name\>** in der neuen Region mit Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Weitere Informationen zu diesem PowerShell-Befehl finden Sie unter [Erstellen der Azure-SSIS-Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md).



## <a name="azure-ssis-ir-failover-with-azure-sql-database"></a>Azure-SSIS IR-Failover mit Azure SQL-Datenbank

### <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Szenario 1: Azure-SSIS IR verweist auf einen Listenerendpunkt mit Lese-/Schreibberechtigung

Dieses Szenario ist geeignet, wenn Azure-SSIS IR auf den Listenerendpunkt mit Lese-/Schreibzugriff der Failovergruppe verweist und der SQL-Datenbank-Server *nicht* mit der VNET-Dienstendpunktregel konfiguriert ist. Wenn Azure-SSIS IR auf den Listenerendpunkt mit Lese-/Schreibzugriff zeigen soll, müssen Sie zuerst auf den primären Serverendpunkt verweisen. Nachdem Sie SSISDB zur Failovergruppe hinzugefügt haben, können Sie den Listenerendpunkt mit Lese-/Schreibzugriff ändern und Azure-SSIS IR neu starten.

#### <a name="solution"></a>Lösung

Wenn ein Failover auftritt, ist dies transparent für Azure-SSIS IR. Azure-SSIS IR stellt automatisch eine Verbindung mit dem neuen primären Endpunkt der Failovergruppe her. Wenn Sie die Region oder andere Informationen in Ihrer Azure-SSIS IR-Instanz aktualisieren möchten, können Sie diese anhalten, bearbeiten und anschließend neu starten.


### <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Szenario 2: Azure-SSIS IR verweist auf den primären Serverendpunkt

Dieses Szenario ist geeignet, wenn Azure-SSIS IR auf den primären Serverendpunkt verweist.

#### <a name="solution"></a>Lösung

Bei einem Failover müssen Sie die folgenden Schritte ausführen:

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Bearbeiten Sie Azure-SSIS IR mit der neuen Region, dem Endpunkt und den VNET-Informationen der sekundären Instanz.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

3. Starten Sie Azure-SSIS IR neu.

### <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Szenario 3: Anfügen einer vorhandenen SSISDB (SSIS-Katalog) an eine neue Azure-SSIS IR

Dieses Szenario ist geeignet, wenn Sie eine neue Azure-SSIS IR-Instanz in der sekundären Region bereitstellen möchten, oder wenn Sie möchten, dass Ihre SSISDB in einer neuen Region weiterhin mit einer neuen Azure-SSIS IR-Instanz arbeitet, wenn ein ADF- oder Azure-SSIS IR-Notfall in der aktuellen Region auftritt.

#### <a name="solution"></a>Lösung

> [!NOTE]
> Schritt 4 (Erstellung der IR) muss über PowerShell erfolgen. Im Azure-Portal wird der Fehler gemeldet, dass SSISDB bereits vorhanden ist.

1. Beenden Sie Azure-SSIS IR in der primären Region.

2. Führen Sie die gespeicherte Prozedur aus, um die Metadaten in SSISDB so zu aktualisieren, dass Verbindungen von **\<new_data_factory_name\>** und **\<new_integration_runtime_name\>** akzeptiert werden.
   
    ```sql
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
    ```

3. Erstellen Sie eine neue Data Factory mit dem Namen **\<new_data_factory_name\>** in der neuen Region. Weitere Informationen finden Sie unter „Erstellen einer Data Factory“.

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
    ```
    
    Weitere Informationen zu diesem PowerShell-Befehl finden Sie unter [Erstellen einer Azure Data Factory mithilfe von PowerShell](quickstart-create-data-factory-powershell.md).

4. Erstellen Sie eine neue Azure-SSIS IR mit dem Namen **\<new_integration_runtime_name\>** in der neuen Region mit Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Weitere Informationen zu diesem PowerShell-Befehl finden Sie unter [Erstellen der Azure-SSIS-Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md).


## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie diese weiteren Konfigurationsoptionen für die Azure-SSIS-IR in Betracht:

- [Konfigurieren der Azure-SSIS-Integration Runtime für hohe Leistung](configure-azure-ssis-integration-runtime-performance.md)

- [Anpassen des Setups für Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Bereitstellen der Enterprise-Edition für die Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)
