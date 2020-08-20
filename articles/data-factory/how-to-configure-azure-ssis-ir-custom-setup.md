---
title: Anpassen des Setups für eine Azure-SSIS Integration Runtime
description: In diesem Artikel wird beschrieben, wie über die Schnittstelle für das benutzerdefinierte Setup für eine Azure-SSIS Integration Runtime zusätzliche Komponenten installiert oder Einstellungen geändert werden können.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 07/09/2020
ms.openlocfilehash: 8b29708dbcc6bbb0d127b4277fd4b2d962da6e6a
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87986388"
---
# <a name="customize-the-setup-for-an-azure-ssis-integration-runtime"></a>Anpassen des Setups für eine Azure-SSIS Integration Runtime

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Das benutzerdefinierte Setup für eine Integration Runtime (IR) für Azure-SQL Server Integration Services (Azure-SSIS) in Azure Data Factory (ADF) bietet eine Schnittstelle zum Hinzufügen Ihrer eigenen Schritte während der Einrichtung oder Neukonfiguration Ihrer Azure-SSIS IR. 

Mithilfe des benutzerdefinierten Setups können Sie die Standardkonfiguration des Betriebssystems oder der Umgebung ändern, um beispielsweise zusätzliche Windows-Dienste zu starten, die Zugriffsanmeldeinformationen für Dateifreigaben beizubehalten oder eine starke Kryptografie/ein sichereres Netzwerkprotokoll (TLS 1.2) zu verwenden. Sie können auch zusätzliche benutzerdefinierte oder Drittanbieterkomponenten auf jedem Knoten Ihrer Azure-SSIS IR installieren, z. B. Assemblys, Treiber oder Erweiterungen. Weitere Informationen zu integrierten/vorinstallierten Komponenten finden Sie unter [Integrierte/vorinstallierte Komponenten für Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/built-in-preinstalled-components-ssis-integration-runtime).

Es gibt zwei Möglichkeiten zur Durchführung benutzerdefinierter Setups auf Ihrer Azure-SSIS IR: 
* **Benutzerdefiniertes Standard-Setup mit einem Skript**: Bereiten Sie ein Skript und die zugehörigen Dateien vor, und laden Sie alle zusammen in einen Blobcontainer in Ihrem Azure Storage-Konto hoch. Sie müssen dann einen SAS-URI (Shared Access Signature-Uniform Resource Identifier) für Ihren Container bereitstellen, wenn Sie Azure-SSIS IR einrichten oder neu konfigurieren. Anschließend lädt jeder Knoten Ihrer Azure-SSIS IR das Skript und die zugehörigen Dateien aus Ihrem Container herunter und führt Ihr benutzerdefiniertes Setup mit erhöhten Berechtigungen aus. Wenn Ihr benutzerdefiniertes Setup abgeschlossen ist, lädt jeder Knoten die standardmäßige Ausgabe der Ausführung und andere Protokolle in den Container hoch.
* **Benutzerdefiniertes Express-Setup ohne ein Skript**: Führen Sie einige gängige Systemkonfigurationen und Windows-Befehle aus, oder installieren Sie einige beliebte oder empfohlene zusätzliche Komponenten, ohne Skripts zu verwenden.

Sie können sowohl kostenlose, nicht lizenzierte als auch kostenpflichtige, lizenzierte Komponenten mit benutzerdefinierten Standard- und Express-Setups installieren. Informationen für unabhängige Softwareanbieter (Independent Software Vendor, ISV) finden Sie unter [Installieren kostenpflichtiger oder lizenzierter benutzerdefinierter Komponenten für Azure-SSIS Integration Runtime](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Um von zukünftigen Verbesserungen zu profitieren, empfehlen wir die Verwendung von mindestens V3 für Ihre Azure-SSIS IR mit benutzerdefiniertem Setup.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

Die folgenden Einschränkungen gelten nur für benutzerdefinierte Standard-Setups:

- Wenn Sie *gacutil.exe* in Ihrem Skript verwenden möchten, um Assemblys im globalen Assemblycache (GAC) zu installieren, müssen Sie *gacutil.exe* als Teil Ihres benutzerdefinierten Setups bereitstellen. Oder Sie können die Kopie verwenden, die in unserem Container *Öffentliche Vorschau* bereitgestellt wird. Dies wird weiter unten im Abschnitt „Anweisungen“ erläutert.

- Wenn Sie in Ihrem Skript auf einen Unterordner verweisen möchten, unterstützt *msiexec.exe* nicht die `.\`-Notation zum Verweisen auf den Stammordner. Verwenden Sie einen Befehl, z. B. `msiexec /i "MySubfolder\MyInstallerx64.msi" ...`, anstelle von `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

- Administrative Freigaben oder verborgene Netzwerkfreigaben, die von Windows automatisch erstellt werden, werden in der Azure-SSIS IR derzeit nicht unterstützt.

- Der ODBC-Treiber „IBM iSeries Access“ wird in der Azure-SSIS IR nicht unterstützt. Während Ihres benutzerdefinierten Setups werden möglicherweise Installationsfehler angezeigt. Sollte dies der Fall sein, wenden Sie sich an den IBM-Kundendienst.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Zum Anpassen Ihrer Azure-SSIS IR benötigen Sie die folgenden Elemente:

- [Ein Azure-Abonnement](https://azure.microsoft.com/)

- [Bereitstellung von Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Ein Azure-Speicherkonto](https://azure.microsoft.com/services/storage/). Nicht erforderlich für benutzerdefinierte Express-Setups. Für benutzerdefinierte Standard-Setups müssen Sie das benutzerdefinierte Setupskript und die zugehörigen Dateien in einen Blobcontainer hochladen und dort speichern. Auch die Ausführungsprotokolle werden vom benutzerdefinierten Setupvorgang in diesen Blobcontainer hochgeladen.

## <a name="instructions"></a>Instructions

Sie können Ihre Azure-SSIS IR mit benutzerdefinierten Setups auf der ADF-Benutzeroberfläche bereitstellen oder neu konfigurieren. Wenn Sie diese Vorgänge über PowerShell ausführen möchten, laden Sie [Azure PowerShell](/powershell/azure/install-az-ps) herunter, und installieren Sie das Tool.

### <a name="standard-custom-setup"></a>Benutzerdefiniertes Standardsetup

Um Ihre Azure-SSIS IR mit benutzerdefinierten Standardsetups bereitzustellen oder neu zu konfigurieren, führen Sie die folgenden Schritte aus.

1. Bereiten Sie Ihr benutzerdefiniertes Setupskript und die zugehörigen Dateien (z. B. BAT-, CMD-, EXE-, DLL-, MSI- oder PS1-Dateien) vor.

   * Sie benötigen dazu die Skriptdatei *main.cmd*. Sie ist der Einstiegspunkt für Ihr benutzerdefiniertes Setup.  
   * Um sicherzustellen, dass das Skript im Hintergrund ausgeführt werden kann, empfehlen wir, es zuerst auf Ihrem lokalen Computer zu testen.  
   * Wenn Sie möchten, dass zusätzliche Protokolle, die von anderen Tools (z. B. *msiexec.exe*) generiert wurden, in Ihren Container hochgeladen werden, geben Sie die vordefinierte Umgebungsvariable, `CUSTOM_SETUP_SCRIPT_LOG_DIR`, als Protokollordner in Ihren Skripts an (z. B. *msiexec/i xxx.msi/quiet/lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log*).

1. Laden Sie [Azure Storage-Explorer](https://storageexplorer.com/) herunter, installieren und öffnen Sie ihn.

   a. Klicken Sie unter **(Lokal und angefügt)** mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie dann **Verbindung mit Azure Storage herstellen** aus.

      ![Verbinden mit Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   b. Aktivieren Sie **Einen Speicherkontonamen und -schlüssel verwenden**, und klicken Sie dann auf **Weiter**.

      ![Verwenden eines Speicherkontonamens und -schlüssels](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   c. Geben Sie Ihren Azure Storage-Kontonamen und -schlüssel ein, und wählen Sie **Weiter** und dann **Verbinden** aus.

      ![Bereitstellen des Speicherkontonamens und -schlüssels](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   d. Klicken Sie unter Ihrem verbundenen Azure Storage-Konto mit der rechten Maustaste auf **Blobcontainer**, wählen Sie **Blobcontainer erstellen** aus, und benennen Sie den neuen Container.

      ![Erstellen eines Blobcontainers](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   e. Wählen Sie den neuen Container aus, und laden Sie Ihr benutzerdefiniertes Setupskript und die zugehörigen Dateien hoch. Stellen Sie sicher, dass Sie *main.cmd* auf die oberste Ebene Ihres Containers und nicht in einen Ordner hochladen. Vergewissern Sie sich außerdem, dass Ihr Container nur die erforderlichen Dateien für das benutzerdefinierte Setup enthält, damit das spätere Herunterladen in Ihre Azure-SSIS IR nicht lange dauert. Die maximale Dauer für ein benutzerdefiniertes Setup ist zurzeit auf 45 Minuten festgelegt, bevor ein Timeout eintritt. Diese Dauer umfasst die Zeit zum Herunterladen aller Dateien aus Ihrem Container und deren Installation in der Azure-SSIS IR. Wenn das Setup mehr Zeit erfordert, erstellen Sie ein Supportticket.

      ![Hochladen von Dateien in den Blobcontainer](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   f. Klicken Sie mit der rechten Maustaste auf den Container, und wählen Sie dann **Shared Access Signature abrufen** aus.

      ![Abrufen der Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   g. Erstellen Sie den SAS-URI für Ihren Container mit einer ausreichend langen Ablaufzeit sowie mit Lese-, Schreib- und Auflistungsberechtigungen. Den SAS-URI benötigen Sie jedes Mal zum Herunterladen und Ausführen Ihres benutzerdefinierten Setupskripts und der zugehörigen Dateien, wenn für einen Knoten Ihrer Azure-SSIS IR ein Reimaging durchgeführt oder er neu gestartet wird. Schreibberechtigungen sind zum Hochladen der Setupausführungsprotokolle erforderlich.

      > [!IMPORTANT]
      > Stellen Sie sicher, dass der SAS-URI nicht abläuft und dass die benutzerdefinierten Setupressourcen während des gesamten Lebenszyklus Ihrer Azure-SSIS IR (vom Erstellen bis zum Löschen) immer verfügbar sind – insbesondere dann, wenn Sie Ihre Azure-SSIS IR während dieses Zeitraums regelmäßig beenden und starten.

      ![Generieren der Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   h. Kopieren und speichern Sie den SAS-URI des Containers.

      ![Kopieren und Speichern der Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Wenn Sie Ihre Azure-SSIS IR über die ADF-Benutzeroberfläche bereitstellen oder neu konfigurieren, aktivieren Sie das Kontrollkästchen **Azure-SSIS Integration Runtime mit zusätzlichen Systemkonfigurationen/Komponenteninstallationen anpassen** auf der Seite **Erweiterte Einstellungen** des Bereichs **Setup für Integration Runtime**, und geben Sie den SAS-URI Ihres Containers in das Feld **SAS-URI des Containers für benutzerdefinierte Setups** ein.

   ![Erweiterte Einstellungen bei benutzerdefinierten Setups](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

### <a name="express-custom-setup"></a>Benutzerdefiniertes Express-Setup

Um Ihre Azure-SSIS IR mit benutzerdefinierten Express-Setups bereitzustellen oder neu zu konfigurieren, führen Sie die folgenden Schritte aus.

1. Wenn Sie Ihre Azure-SSIS IR über die ADF-Benutzeroberfläche bereitstellen oder neu konfigurieren, aktivieren Sie das Kontrollkästchen **Azure-SSIS Integration Runtime mit zusätzlichen Systemkonfigurationen/Komponenteninstallationen anpassen** auf der Seite **Erweiterte Einstellungen** des Bereichs **Setup für Integration Runtime**. 

1. Wählen Sie **Neu** aus, um den Bereich **Benutzerdefiniertes Express-Setup hinzufügen** zu öffnen, und wählen Sie dann in der Dropdownliste **Typ des benutzerdefinierten Express-Setups** den gewünschten Typ aus:

   * Wenn Sie den Typ **Befehl „cmdkey“ ausführen** auswählen, können Sie die Zugriffsanmeldeinformationen für Ihre Dateifreigaben oder Azure Files-Freigaben in Azure-SSIS IR beibehalten, indem Sie in den Feldern **/Add**, **/User** und **/Pass** die Namen Ihres Zielcomputers oder Ihrer Domäne, Ihres Kontos oder Ihres Benutzers sowie den Kontoschlüssel oder das Kennwort eingeben. Dies ist vergleichbar mit der Ausführung des Windows-Befehls [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) auf Ihrem lokalen Computer.
   
   * Wenn Sie den Typ **Umgebungsvariable hinzufügen** auswählen, können Sie Windows-Umgebungsvariablen hinzufügen, die in Ihren in der Azure-SSIS IR ausgeführten Paketen verwendet werden sollen, indem Sie in den Feldern **Variablenname** und **Variablenwert** den Namen und den Wert Ihrer Umgebungsvariablen eingeben. Dies ist vergleichbar mit der Ausführung des Windows-Befehls [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) auf Ihrem lokalen Computer.

   * Wenn Sie den Typ **Lizenzierte Komponente installieren** auswählen, können Sie in der Dropdownliste **Komponentenname** eine integrierte Komponente unserer ISV-Partner auswählen:

     * Wenn Sie die Komponente **Task Factory von SentryOne** auswählen, können Sie die [Task Factory](https://www.sentryone.com/products/task-factory/high-performance-ssis-components)-Komponentensuite von SentryOne in Ihrer Azure-SSIS IR installieren, indem Sie im Feld **Lizenzschlüssel** den erworbenen Produktlizenzschlüssel eingeben. Die aktuelle integrierte Version ist **2020.1.3**.

     * Wenn Sie die Komponente **HEDDA.IO von oh22** auswählen, können Sie die [HEDDA.IO](https://hedda.io/ssis-component/)-Datenqualitäts-/Bereinigungskomponente von oh22 in Ihrer Azure-SSIS IR installieren, nachdem Sie den entsprechenden Dienst erworben haben. Die aktuelle integrierte Version ist **1.0.14**.

     * Wenn Sie die Komponente **SQLPhonetics.NET von oh22** auswählen, können Sie die [SQLPhonetics.NET](https://appsource.microsoft.com/product/web-apps/oh22.sqlphonetics-ssis)-Datenqualitäts-/Zuordnungskomponente von oh22 in Ihrer Azure-SSIS IR installieren, indem Sie im Feld **Lizenzschlüssel** den erworbenen Produktlizenzschlüssel eingeben. Die aktuelle integrierte Version ist **1.0.45**.

     * Wenn Sie die Komponente **KingswaySoft's SSIS Integration Toolkit** auswählen, können Sie die Connector-Sammlung [SSIS Integration Toolkit](https://www.kingswaysoft.com/products/ssis-integration-toolkit-for-microsoft-dynamics-365) für Apps für CRM/ERP/Marketing/Zusammenarbeit, z. B. Microsoft Dynamics/SharePoint/Project Server, Oracle/Salesforce Marketing Cloud usw., von KingswaySoft in Ihrer Azure-SSIS Integration Runtime installieren, indem Sie den erworbenen Produktlizenzschlüssel im Feld **Lizenzschlüssel** eingeben. Die aktuelle integrierte Version ist **2019.2**.

     * Wenn Sie die Komponente **KingswaySoft's SSIS Productivity Pack** auswählen, können Sie die Komponentensammlung [SSIS Productivity Pack](https://www.kingswaysoft.com/products/ssis-productivity-pack) von KingswaySoft in Ihrer Azure-SSIS Integration Runtime installieren, indem Sie den erworbenen Produktlizenzschlüssel im Feld **Lizenzschlüssel** eingeben. Die aktuelle integrierte Version ist **10.0**.

     * Wenn Sie die Komponente **Theobald Software's Xtract IS** auswählen, können Sie die [Xtract IS](https://theobald-software.com/en/xtract-is/)-Suite von Connectors für das SAP-System (ERP, S/4HANA, BW) aus Theobald Software in Ihrer Azure-SSIS IR installieren, indem Sie die erworbene Produktlizenzdatei in das Feld **Lizenzdatei** ziehen und ablegen/hochladen. Die aktuelle integrierte Version ist **6.1.1.3**.

Ihre hinzugefügten benutzerdefinierten Express-Setups werden auf der Seite **Erweiterte Einstellungen** angezeigt. Wenn Sie sie entfernen möchten, können Sie die entsprechenden Kontrollkästchen aktivieren und dann **Löschen** auswählen.

### <a name="azure-powershell"></a>Azure PowerShell

Um Ihre Azure-SSIS IR über Azure PowerShell mit benutzerdefinierten Setups bereitzustellen oder neu zu konfigurieren, führen Sie die folgenden Schritte aus.

1. Wenn Ihre Azure-SSIS IR bereits ausgeführt wird, halten Sie sie zuerst an.

1. Dann können Sie benutzerdefinierte Setups hinzufügen oder entfernen, indem Sie das Cmdlet `Set-AzDataFactoryV2IntegrationRuntime` ausführen, bevor Sie Ihre Azure-SSIS IR starten.

   ```powershell
   $ResourceGroupName = "[your Azure resource group name]"
   $DataFactoryName = "[your data factory name]"
   $AzureSSISName = "[your Azure-SSIS IR name]"
   # Custom setup info: Standard/express custom setups
   $SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
   $ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO|KingswaySoft.IntegrationToolkit|KingswaySoft.ProductivityPack|Theobald.XtractIS or leave it empty]" # OPTIONAL to configure an express custom setup without script

   # Add custom setup parameters if you use standard/express custom setups
   if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
   {
       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -SetupScriptContainerSasUri $SetupScriptContainerSasUri
   }
   if(![string]::IsNullOrEmpty($ExpressCustomSetup))
   {
       if($ExpressCustomSetup -eq "RunCmdkey")
       {
           $addCmdkeyArgument = "YourFileShareServerName or YourAzureStorageAccountName.file.core.windows.net"
           $userCmdkeyArgument = "YourDomainName\YourUsername or azure\YourAzureStorageAccountName"
           $passCmdkeyArgument = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourPassword or YourAccessKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.CmdkeySetup($addCmdkeyArgument, $userCmdkeyArgument, $passCmdkeyArgument)
       }
       if($ExpressCustomSetup -eq "SetEnvironmentVariable")
       {
           $variableName = "YourVariableName"
           $variableValue = "YourVariableValue"
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.EnvironmentVariableSetup($variableName, $variableValue)
       }
       if($ExpressCustomSetup -eq "SentryOne.TaskFactory")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.SQLPhonetics.NET")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.HEDDA.IO")
       {
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup)
       }
       if($ExpressCustomSetup -eq "KingswaySoft.IntegrationToolkit")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "KingswaySoft.ProductivityPack")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }    
       if($ExpressCustomSetup -eq "Theobald.XtractIS")
       {
           $jsonData = Get-Content -Raw -Path YourLicenseFile.json
           $jsonData = $jsonData -replace '\s',''
           $jsonData = $jsonData.replace('"','\"')
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString($jsonData)
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       # Create an array of one or more express custom setups
       $setups = New-Object System.Collections.ArrayList
       $setups.Add($setup)

       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -ExpressCustomSetup $setups
   }
   Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
       -DataFactoryName $DataFactoryName `
       -Name $AzureSSISName `
       -Force
   ```

1. Nachdem Ihr benutzerdefiniertes Standardsetup abgeschlossen und Ihre Azure-SSIS IR gestartet wurde, finden Sie die Standardausgabe von *main.cmd* und andere Ausführungsprotokolle im Ordner *main.cmd.log* Ihres Containers.

### <a name="standard-custom-setup-samples"></a>Beispiele für benutzerdefinierte Standardsetups

Um Beispiele für benutzerdefinierte Standardsetups anzuzeigen und wiederzuverwenden, führen Sie die folgenden Schritte aus.

1. Stellen Sie über Azure Storage-Explorer eine Verbindung mit unserem Public Preview-Container her.

   a. Klicken Sie unter **(Lokal und angefügt)** mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie nacheinander **Mit Azure-Speicher verbinden**, **Verbindungszeichenfolge oder SAS-URI verwenden** und **Weiter** aus.

      ![Herstellen einer Verbindung zum Azure-Speicher mit Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b. Wählen Sie **SAS-URI verwenden** aus, und geben Sie dann im Feld **URI** den folgenden SAS-URI ein:

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

      ![Bereitstellen von Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Wählen Sie **Weiter** und anschließend **Verbinden** aus.

   d. Wählen Sie im linken Bereich den verbundenen Container **publicpreview** aus, und doppelklicken Sie dann auf den Ordner *CustomSetupScript*. In diesem Ordner befinden sich die folgenden Elemente:

      * Der Ordner *Beispiel* mit einem benutzerdefinierten Setup zum Installieren eines einfachen Tasks auf jedem Knoten Ihrer Azure-SSIS IR. Der Task hat keine Funktion, bleibt aber ein paar Sekunden im Standbymodus. Außerdem enthält der Ordner den Ordner *gacutil*, dessen gesamter Inhalt (*gacutil.exe*, *gacutil.exe.config* und *1033\ gacutlrc.dll*) unverändert in Ihren Container kopiert werden kann.

      * Der Ordner *UserScenarios* mit mehreren Beispielen für benutzerdefinierte Setups von echten Benutzerszenarien.

        ![Inhalt des Containers „Öffentliche Vorschau“](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   e. Doppelklicken Sie auf den Ordner *UserScenarios*, um die folgenden Elemente zu suchen:

      * Den Ordner *.NET FRAMEWORK 3.5* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren einer früheren Version von .NET Framework, das für benutzerdefinierte Komponenten auf jedem Knoten Ihrer Azure-SSIS IR möglicherweise erforderlich ist.

      * Den Ordner *BCP* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren von SQL Server-Befehlszeilenprogrammen (*MsSqlCmdLnUtils.msi*), einschließlich des Massenkopierprogramms (*bcp*), auf jedem Knoten Ihrer Azure-SSIS IR.

      * Den Ordner *EXCEL* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren von C#-Assemblys und -Bibliotheken, die Sie in Skripttasks zum dynamischen Lesen und Schreiben von Excel-Dateien auf jedem Knoten Ihrer Azure-SSIS IR verwenden können. 
      
        Laden Sie zuerst [*ExcelDataReader.dll*](https://www.nuget.org/packages/ExcelDataReader/) und [*DocumentFormat.OpenXml.dll*](https://www.nuget.org/packages/DocumentFormat.OpenXml/) herunter, und laden Sie dann alle zusammen mit *main.cmd* in Ihren Container hoch. Wenn Sie nur den standardmäßigen Excel-Verbindungs-Manager, die Excel-Quelle und das Excel-Ziel verwenden möchten, ist die erforderliche Access-Redistributable bereits in Ihrer Azure-SSIS IR vorinstalliert, sodass Sie kein benutzerdefiniertes Setup durchführen müssen.
      
      * Den Ordner *MYSQL ODBC* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren der MySQL ODBC-Treibers auf jedem Knoten Ihrer Azure-SSIS IR. Mit diesem Setup können Sie den ODBC-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum MySQL-Server verwenden. 
     
        [Laden Sie zuerst die neuesten 64-Bit- und 32-Bit-Versionen der MySQL ODBC-Treiberinstallationsprogramme](https://dev.mysql.com/downloads/connector/odbc/) herunter (z. B. *mysql-connector-odbc-8.0.13-winx64.msi* und *mysql-connector-odbc-8.0.13-win32.msi*), und laden Sie dann alle zusammen mit *main.cmd* in Ihren Container hoch.

      * Den Ordner *ORACLE ENTERPRISE* mit einem benutzerdefinierten Setupskript (*main.cmd*) und einer Konfigurationsdatei für die unbeaufsichtigte Installation (*client.rsp*) zum Installieren der Oracle-Connectors und des OCI-Treibers auf jedem Knoten Ihrer Azure-SSIS IR Enterprise Edition. Mit diesem Setup können Sie den Oracle-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum Oracle-Server verwenden. 
      
        Laden Sie zuerst Microsoft Connectors v5.0 für Oracle (*AttunitySSISOraAdaptersSetup.msi* und *AttunitySSISOraAdaptersSetup64.msi*) aus [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55179) sowie den neuesten Oracle-Client (z. B *winx64_12102_client.zip*) aus [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) herunter, und laden Sie dann alle zusammen mit *main.cmd* und *client.rsp* in Ihren Container hoch. Wenn Sie TNS bei der Herstellung einer Verbindung mit Oracle verwenden, müssen Sie auch die Datei *tnsnames.ora* herunterladen, bearbeiten und in Ihren Container hochladen, damit sie während des Setups in den Oracle-Installationsordner kopiert werden kann.

      * Den Ordner *ORACLE STANDARD ADO.NET* mit einem benutzerdefinierten Setupskript *(main.cmd*) zum Installieren des Oracle ODP.NET-Treibers auf jedem Knoten Ihrer Azure-SSIS IR. Mit diesem Setup können Sie den ADO.NET-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum Oracle-Server verwenden. 
      
        [Laden Sie zuerst den neuesten Oracle ODP.NET-Treiber herunter](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) (z. B. *ODP.NET_Managed_ODAC122cR1.zip*), und laden Sie ihn dann zusammen mit *main.cmd* in Ihren Container hoch.
       
      * Den Ordner *ORACLE STANDARD ODBC* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren des Oracle ODBC-Treibers und Konfigurieren des Datenquellennamens (DSN) auf jedem Knoten Ihrer Azure-SSIS IR. Bei diesem Setup können Sie den ODBC-Verbindungs-Manager, die Quelle und das Ziel oder den Power Query-Verbindungs-Manager und die Quelle mit dem ODBC-Datenquellentyp verwenden, um eine Verbindung zum Oracle-Server herzustellen. 
      
        Laden Sie zuerst das neueste Oracle Instant Client-Paket (Basic-Paket oder Basic Lite-Paket) und das ODBC-Paket herunter, und laden Sie dann alle zusammen mit *main.cmd* in Ihren Container hoch:
        * [Herunterladen von 64-Bit-Paketen](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Basic-Paket: *instantclient-basic-windows.x64-18.3.0.0.0dbru.zip*; Basic Lite-Paket: *instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip*; ODBC-Paket: *instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip*) 
        * [Herunterladen von 32-Bit-Paketen](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Basic-Paket: *instantclient-basic-nt-18.3.0.0.0dbru.zip*; Basic Lite-Paket: *instantclient-basiclite-nt-18.3.0.0.0dbru.zip*; ODBC-Paket: *instantclient-odbc-nt-18.3.0.0.0dbru.zip*)

      * Den Ordner *ORACLE STANDARD OLEDB* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren des Oracle OLE DB-Treibers auf jedem Knoten Ihrer Azure-SSIS IR. Mit diesem Setup können Sie den OLE DB-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum Oracle-Server verwenden. 
     
        [Laden Sie zuerst den neuesten Oracle OLE DB-Treiber herunter](https://www.oracle.com/partners/campaign/index-090165.html) (z. B. *ODAC122010Xcopy_x64.zip*), und laden Sie ihn dann zusammen mit *main.cmd* in Ihren Container hoch.

      * Den Ordner *POSTGRESQL ODBC* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren des PostgreSQL ODBC-Treibers auf jedem Knoten Ihrer Azure-SSIS IR. Mit diesem Setup können Sie den ODBC-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum PostgreSQL-Server verwenden. 
     
        [Laden Sie zuerst die neuesten 64-Bit- und 32-Bit-Versionen der PostgreSQL-ODBC-Treiberinstallationsprogramme herunter](https://www.postgresql.org/ftp/odbc/versions/msi/) (z. B. *psqlodbc_x64. msi* und *psqlodbc_x86. msi*), und laden Sie dann alle zusammen mit *main.cmd* in Ihren Container hoch.

      * Den Ordner *SAP BW* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren der .NET-Connector-Assembly von SAP (*librfc32.dll*) auf jedem Knoten Ihrer Azure-SSIS IR Enterprise Edition. Mit diesem Setup können Sie den SAP Business Warehouse (BW)-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum SAP BW-Server verwenden. 
      
        Laden Sie zuerst die 64-Bit- oder 32-Bit-Version von *librfc32.dll* aus dem SAP-Installationsordner zusammen mit *main.cmd* in Ihren Container hoch. Das Skript kopiert dann die SAP-Assembly während des Setups in den Ordner *%windir%\SysWow64* oder *%windir%\System32*.

      * Den Ordner *STORAGE* mit einem benutzerdefinierten Setupskript (*main.cmd*) zum Installieren von Azure PowerShell auf jedem Knoten Ihrer Azure-SSIS IR. Bei diesem Setup können Sie SSIS-Pakete bereitstellen und ausführen, die [PowerShell-Skripts zum Bearbeiten Ihres Azure Storage-Kontos](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell) ausführen. 
      
        Kopieren Sie *main.cmd*, ein Beispiel für *AzurePowerShell.msi* (oder verwenden Sie die neueste Version) und *storage.ps1* in Ihren Container. Verwenden Sie *PowerShell.dtsx* als Vorlage für Ihre Pakete. In der Paketvorlage sind der [Azure Blob-Download-Task](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), der *storage.ps1* als modifizierbares PowerShell-Skript herunterlädt, und der [Task „Prozess ausführen“](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) zusammengefasst, der das Skript auf jedem Knoten ausführt.

      * Der Ordner *TERADATA*, der ein benutzerdefiniertes Setupskript (*main.cmd*), die zugehörige Datei (*install.cmd*) und die Installer-Pakete ( *.msi*) enthält. Diese Dateien installieren die Teradata-Connectors, die Teradata Parallel Transporter (TPT)-API und den ODBC-Treiber auf jedem Knoten Ihrer Azure-SSIS IR Enterprise Edition. Mit diesem Setup können Sie den Teradata-Verbindungs-Manager, die Quelle und das Ziel für die Verbindung zum Teradata-Server verwenden. 
      
        [Laden Sie zuerst die ZIP-Datei „Teradata Tools und Utilities 15.x“ herunter](http://partnerintelligence.teradata.com) (z. B. *TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip*), und laden Sie sie dann zusammen mit den zuvor erwähnten Dateien *.cmd* und *.msi* Dateien in Ihren Container hoch.

      * Den Ordner *TLS 1.2* mit einem benutzerdefinierten Setupskript (*main.cmd*) zur Verwendung einer starken Kryptografie/eines sichereren Netzwerkprotokolls (TLS 1.2) und zum Deaktivieren von älteren SSL/TLS-Versionen auf jedem Knoten Ihrer Azure-SSIS IR.

      * Den Ordner *ZULU OPENJDK* Ordner mit einem benutzerdefinierten Setupskript (*main.cmd*) und einer PowerShell-Datei (*install_openjdk.ps1*) zum Installieren des Zulu OpenJDK auf jedem Knoten Ihrer Azure-SSIS IR. Dieses Setup ermöglicht Ihnen die Verwendung von Azure Data Lake Store- und Flexible File-Connectors zur Verarbeitung von ORC- und Parquet-Dateien. Weitere Informationen finden Sie unter [Azure Feature Pack für Integration Services](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java). 
      
        [Laden Sie zuerst das neueste Zulu OpenJDK herunter](https://www.azul.com/downloads/zulu/zulu-windows/) (z. B. *zulu8.33.0.1-jdk8.0.192-win_x64.zip*), und laden Sie es dann zusammen mit *main.cmd* und *install_openjdk.ps1* in Ihren Container hoch.

        ![Ordner im Ordner „Benutzerszenarios“](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   f. Um diese Beispiele für benutzerdefinierte Standardsetups wiederzuverwenden, kopieren Sie den Inhalt des ausgewählten Ordners in Ihren Container.

1. Wenn Sie Ihre Azure-SSIS IR über die ADF-Benutzeroberfläche bereitstellen oder neu konfigurieren, aktivieren Sie das Kontrollkästchen **Azure-SSIS Integration Runtime mit zusätzlichen Systemkonfigurationen/Komponenteninstallationen anpassen** auf der Seite **Erweiterte Einstellungen** des Bereichs **Setup für Integration Runtime**, und geben Sie den SAS-URI Ihres Containers in das Feld **SAS-URI des Containers für benutzerdefinierte Setups** ein.
   
1. Wenn Sie Ihre Azure-SSIS IR über Azure PowerShell bereitstellen oder neu konfigurieren, halten Sie die Runtime an, falls sie bereits ausgeführt wird. Führen Sie dann das Cmdlet `Set-AzDataFactoryV2IntegrationRuntime` mit dem SAS-URI Ihres Containers als Wert für den Parameter `SetupScriptContainerSasUri` aus, und starten Sie die Azure-SSIS IR neu.

1. Nachdem Ihr benutzerdefiniertes Standardsetup abgeschlossen und Ihre Azure-SSIS IR gestartet wurde, finden Sie die Standardausgabe von *main.cmd* und andere Ausführungsprotokolle im Ordner *main.cmd.log* Ihres Containers.

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten der Enterprise Edition der Azure-SSIS IR](how-to-configure-azure-ssis-ir-enterprise-edition.md)
- [Entwickeln von zahlungspflichtigen oder lizenzierten Komponenten für die Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md)
