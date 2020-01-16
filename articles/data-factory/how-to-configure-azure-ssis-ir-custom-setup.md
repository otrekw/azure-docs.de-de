---
title: Anpassen des Setups für Azure-SSIS Integration Runtime
description: In diesem Artikel wird die Verwendung der Schnittstelle für das benutzerdefinierte Setup von Azure-SSIS Integration Runtime zum Installieren zusätzlicher Komponenten oder Ändern von Einstellungen beschrieben.
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
ms.date: 12/23/2019
ms.openlocfilehash: ccf7ba2fd27dabdb090be87c5438ad68471996da
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75497058"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Anpassen des Setups für Azure-SSIS Integration Runtime

Mit der Schnittstelle für das benutzerdefinierte Setup von Azure-SSIS Integration Runtime (IR) können Sie während der Bereitstellung oder Neukonfiguration von Azure-SSIS IR eigene Setupschritte hinzufügen. 

Durch benutzerdefiniertes Setup können Sie die Standardkonfiguration oder -umgebung für den Betrieb ändern (z.B. zusätzliche Windows-Dienste starten oder Anmeldeinformationen für den Zugriff auf Dateifreigaben beibehalten). Sie können aber auch für jeden Knoten von Azure-SSIS IR zusätzliche Komponenten (z.B. Assemblys, Treiber oder Erweiterungen) installieren.

Es gibt zwei Möglichkeiten, benutzerdefinierte Setups für Ihre Azure-SSIS IR durchzuführen: benutzerdefinierte Express-Setups ohne Skripts und benutzerdefinierte Standard-Setups mit Skripts.

Bei benutzerdefinierten Express-Setups können Sie einige gängige Systemkonfigurationen/Windows-Befehle ausführen oder einige beliebte/empfohlene zusätzliche Komponenten installieren, ohne Skripts zu verwenden.  

Bei benutzerdefinierten Standard-Setups müssen Sie ein Skript und die zugehörigen Dateien vorbereiten und diese zusammen in einen Blobcontainer in Ihr Azure Storage-Konto hochladen. Sie müssen dann einen SAS-URI (Shared Access Signature-Uniform Resource Identifier) für den Container bereitstellen, wenn Sie Azure-SSIS IR bereitstellen oder neu konfigurieren. Die einzelnen Knoten der Azure-SSIS IR laden anschlie0end das Skript und die zugehörigen Dateien aus dem Container herunter und führen das benutzerdefinierte Setup mit erhöhten Rechten aus. Wenn das benutzerdefinierte Setup abgeschlossen wurde, lädt jeder Knoten die standardmäßige Ausgabe der Ausführung und andere Protokolle in den Container hoch.

Sie können sowohl kostenlose/nicht lizenzierte als auch kostenpflichtige/lizenzierte Komponenten mit benutzerdefinierten Express/Standard-Setups installieren. ISVs sollten unsere Dokumentation zur [Entwicklung von kostenpflichtigen oder lizenzierten Komponenten für Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md) durchlesen.

> [!IMPORTANT]
> Knoten der v2-Serie der Azure-SSIS IR eignen sich nicht für ein benutzerdefiniertes Setup, daher sollten Sie stattdessen Knoten der v3-Serie verwenden.  Wenn Sie bereits Knoten der v2-Serie verwenden, wechseln Sie so bald wie möglich zu Knoten der v3-Serie.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

Die folgenden Einschränkungen gelten nur für benutzerdefinierte Standard-Setups:

- Wenn Sie `gacutil.exe` für die Installation von Assemblys im globalen Assemblycache (GAC) verwenden möchten, müssen Sie `gacutil.exe` im Rahmen des benutzerdefinierten Setups angeben oder die im Container „Öffentliche Vorschau“ bereitgestellte Kopie verwenden.

- Wenn Sie auf einen Unterordner in Ihrem Skript verweisen möchten, unterstützt `msiexec.exe` nicht die `.\`-Notation, um auf den Stammordner zu verweisen. Verwenden Sie einen Befehl wie `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` anstelle von `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

- Administrative Freigaben, d. h. verborgene Netzwerkfreigaben, die automatisch von Windows erstellt werden, werden derzeit nicht in der Azure-SSIS IR unterstützt.

- Der ODBC-Treiber für Zugriff auf die den IBM iSeries wird in der Azure-SSIS Integration Runtime nicht unterstützt. Während des benutzerdefinierten Setups werden möglicherweise Installationsfehler angezeigt. Wenden Sie sich an den IBM-Kundendienst.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Zum Anpassen von Azure-SSIS IR benötigen Sie Folgendes:

- [Azure-Abonnement](https://azure.microsoft.com/)

- [Bereitstellen von Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Azure Storage-Konto](https://azure.microsoft.com/services/storage/) Nicht erforderlich für benutzerdefinierte Express-Setups. Für benutzerdefinierte Standard-Setups müssen Sie das benutzerdefinierte Setupskript und die zugehörigen Dateien in einen Blobcontainer hochladen und dort speichern. Auch die Ausführungsprotokolle werden vom benutzerdefinierten Setupvorgang in diesen Blobcontainer hochgeladen.

## <a name="instructions"></a>Instructions

1. Wenn Sie Ihre Azure-SSIS IR mit PowerShell bereitstellen oder neu konfigurieren möchten, müssen Sie [Azure PowerShell](/powershell/azure/install-az-ps) herunterladen und installieren. Fahren Sie bei benutzerdefinierten Express-Setups mit Schritt 4 fort.

1. Bereiten Sie Ihr benutzerdefiniertes Setupskript und die zugehörigen Dateien (z. B. BAT-, CMD-, EXE-, DLL-, MSI- oder PS1-Dateien) vor.

   1. Sie benötigen eine Skriptdatei namens `main.cmd`. Sie ist der Einstiegspunkt für Ihr benutzerdefiniertes Setup.

   1. Sie müssen sicherstellen, dass das Skript im Hintergrund ausgeführt werden kann. Wir empfehlen Ihnen, das Skript zunächst auf dem lokalen Computer zu testen.

   1. Wenn zusätzliche Protokolle von anderen Tools (z. B. `msiexec.exe`) generiert und in den Container hochgeladen werden sollen, geben Sie die vordefinierte Umgebungsvariable `CUSTOM_SETUP_SCRIPT_LOG_DIR` als Protokollordner in Ihren Skripts an (z. B. `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1. Laden Sie [Azure Storage-Explorer](https://storageexplorer.com/) herunter, installieren und starten Sie ihn.

   1. Klicken Sie unter **(Lokal und angefügt)** mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie **Verbindung mit Azure-Speicher herstellen** aus.

      ![Herstellen einer Verbindung mit Azure-Speicher](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. Aktivieren Sie **Einen Speicherkontonamen und -schlüssel verwenden**, und wählen Sie dann **Weiter** aus.

      ![Verwenden eines Speicherkontonamens und -schlüssels](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Geben Sie Ihren Azure Storage-Kontonamen und -schlüssel ein, und wählen Sie **Weiter** und dann **Verbinden** aus.

      ![Bereitstellen von Speicherkontoname und -schlüssel](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Klicken Sie unter Ihrem verbundenen Azure Storage-Konto mit der Maustaste auf **Blobcontainer**, wählen Sie **Blobcontainer erstellen** aus, und benennen Sie den neuen Container.

      ![Erstellen eines Blobcontainers](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Wählen Sie den neuen Container aus, und laden Sie Ihr benutzerdefiniertes Setupskript und die zugehörigen Dateien hoch. Stellen Sie sicher, dass Sie `main.cmd` auf die oberste Ebene des Containers und nicht in einen Ordner hochladen. Vergewissern Sie sich außerdem, dass Ihr Container nur die erforderlichen Dateien für das benutzerdefinierte Setup enthält, damit das spätere Herunterladen in Ihre Azure-SSIS IR nicht so lange dauert. Der maximale Zeitraum für das benutzerdefinierte Setup ist derzeit auf 45 Minuten festgelegt, bevor ein Timeout auftritt, und dies umfasst auch die Zeit zum Herunterladen aller Dateien aus Ihrem Container und deren Installation auf Azure-SSIS IR. Wenn eine längere Frist erforderlich ist, senden Sie ein Supportticket.

      ![Hochladen von Dateien in den Blobcontainer](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Klicken Sie mit der rechten Maustaste auf den Container, und wählen Sie **Shared Access Signature abrufen** aus.

      ![Abrufen der Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Erstellen Sie den SAS-URI für den Container mit einer ausreichend langen Ablaufzeit und mit Lese-, Schreib- und Auflistungsberechtigungen. Den SAS-URI benötigen Sie jedes Mal, wenn für einen Knoten von Azure-SSIS IR ein Reimaging ausgeführt / er neu gestartet wird, zum Herunterladen und Ausführen Ihres benutzerdefinierten Setupskripts und der zugehörigen Dateien. Schreibberechtigungen sind zum Hochladen der Setupausführungsprotokolle erforderlich.

      > [!IMPORTANT]
      > Stellen Sie sicher, dass der SAS-URI nicht abläuft und benutzerdefinierte Installationsressourcen während des gesamten Lebenszyklus Ihres Azure-SSIS IR vom Erstellen bis zum Löschen stets verfügbar sind, insbesondere dann, wenn Sie Ihre Azure-SSIS IR in diesem Zeitraum regelmäßig beenden und starten.

      ![Generieren der Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kopieren und speichern Sie den SAS-URI des Containers.

      ![Kopieren und Speichern der Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Wenn Sie Ihre Azure-SSIS IR mit der Data Factory-Benutzeroberfläche bereitstellen oder neu konfigurieren, können Sie benutzerdefinierte Setups hinzufügen bzw. entfernen, indem Sie das Kontrollkästchen **Azure-SSIS Integration Runtime mit zusätzlichen Systemkonfigurationen/Komponenteninstallationen anpassen** im Bereich **Erweiterte Einstellungen** im Bereich für die Integration Runtime-Einrichtung aktivieren. 

   Wenn Sie benutzerdefinierte Standard-Setups hinzufügen möchten, geben Sie den SAS-URI Ihres Containers in das Feld **SAS-URI des Containers für benutzerdefinierte Setups** ein. 
   
   Wenn Sie benutzerdefinierte Express-Setups hinzufügen möchten, wählen Sie **Neu** aus, um den Bereich **Benutzerdefiniertes Express-Setup hinzufügen** zu öffnen, und wählen Sie dann im Dropdown Menü **Typ des benutzerdefinierten Express-Setups** einen Typ aus:

   1. Wenn Sie den Typ **Befehl „cmdkey“ ausführen** Typ auswählen, können Sie die Zugriffsanmeldeinformationen für Ihre Dateifreigaben/Azure Files der Azure-SSIS IR beibehalten, indem Sie Zielcomputer/Domänenname, Kontoname/Benutzername und Kontoschlüssel/Kennwort in die Felder **/Add**, **/User** bzw. **/Pass** eingeben. Dies ist vergleichbar mit der Ausführung des Windows-Befehls [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) auf dem lokalen Computer.
   
   1. Wenn Sie den Typ **Umgebungsvariable hinzufügen** auswählen, können Sie Windows-Umgebungsvariablen hinzufügen, die in den Paketen verwendet werden sollen, die in der Azure-SSIS IR ausgeführt werden, indem Sie den Namen und den Wert der Umgebungsvariablen in die Felder **Variablenname** und **Variablenwert** eingeben. Dies ist vergleichbar mit der Ausführung des Windows-Befehls [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) auf dem lokalen Computer.

   1. Wenn Sie den Typ **Lizenzierte Komponente installieren** auswählen, können Sie alle integrierten Komponenten unserer ISV-Partner im Dropdown-Menü **Komponentenname** auswählen:

      1. Wenn Sie die Komponente **Task Factory von SentryOne** auswählen, können Sie die [Task Factory](https://www.sentryone.com/products/task-factory/high-performance-ssis-components)-Komponentensuite von SentryOne in Ihrer Azure-SSIS IR installieren, indem Sie den erworbenen Produktlizenzschlüssel im Feld **Lizenzschlüssel** eingeben. Die aktuelle integrierte Version ist **2019.4.3**.

      1. Wenn Sie die Komponente **HEDDA.IO von oh22** auswählen, können Sie die [HEDDA.IO](https://hedda.io/ssis-component/)-Datenqualitäts-/Bereinigungskomponente von oh22 in Ihrer Azure-SSIS IR installieren, nachdem Sie den entsprechenden Dienst erworben haben. Die aktuelle integrierte Version ist **1.0.13**.

      1. Wenn Sie die Komponente **SQLPhonetics.NET von oh22** auswählen, können Sie die [SQLPhonetics.NET](https://sqlphonetics.oh22.is/sqlphonetics-net-for-microsoft-ssis/)-Datenqualitäts-/Zuordnungskomponente von oh22 in Ihrer Azure-SSIS IR installieren, indem Sie den erworbenen Produktlizenzschlüssel im Feld **Lizenzschlüssel** eingeben. Die aktuelle integrierte Version ist **1.0.43**.
   
   Die hinzugefügten benutzerdefinierten Express-Setups werden im Abschnitt **Erweiterte Einstellungen** angezeigt. Um diese zu entfernen, können Sie die entsprechenden Kontrollkästchen aktivieren und dann **Löschen** auswählen.

   ![Erweiterte Einstellungen bei benutzerdefinierten Setups](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

   Wenn Sie Ihre Azure-SSIS IR mit PowerShell bereitstellen oder neu konfigurieren, können Sie benutzerdefinierte Setups hinzufügen bzw. entfernen, indem Sie das `Set-AzDataFactoryV2IntegrationRuntime`-Cmdlet ausführen, bevor Sie die Azure-SSIS IR starten.
   
   Für benutzerdefinierte Standard-Setups können Sie den SAS-URI des Containers als Wert für den `SetupScriptContainerSasUri`-Parameter angeben. Beispiel:

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                         -Name $MyAzureSsisIrName `
                                         -ResourceGroupName $MyResourceGroupName `
                                         -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

   Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                           -Name $MyAzureSsisIrName `
                                           -ResourceGroupName $MyResourceGroupName
   ```
   
   Nachdem das benutzerdefinierte Standard-Setup abgeschlossen und Azure-SSIS IR gestartet wurde, finden Sie die Standardausgabe von `main.cmd` und andere Ausführungsprotokolle im Ordner `main.cmd.log` Ihres Speichercontainers.

1. Um weitere Beispiele für benutzerdefinierte Standard-Setups anzuzeigen, stellen Sie mit Azure Storage-Explorer eine Verbindung zum Container „Öffentliche Vorschau“ her.

   1. Klicken Sie unter **(Lokal und angefügt)** mit der rechten Maustaste auf **Speicherkonten**, und wählen Sie nacheinander **Mit Azure-Speicher verbinden**, **Verbindungszeichenfolge oder SAS-URI verwenden** und **Weiter** aus.

      ![Herstellen einer Verbindung zum Azure-Speicher mit Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   1. Wählen Sie **SAS-URI verwenden** aus, und geben Sie den folgenden SAS-URI für den Container „Öffentliche Vorschau“ ein. Wählen Sie **Weiter** und anschließend **Verbinden** aus.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![Bereitstellen von Shared Access Signature für den Container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   1. Wählen Sie den verbundenen Container „Öffentliche Vorschau“ aus, und doppelklicken Sie auf den Ordner `CustomSetupScript`. In diesem Ordner befinden sich die folgenden Elemente:

      1. Ein Ordner `Sample` mit einem benutzerdefinierten Setup zum Installieren eines einfachen Tasks auf jedem Knoten des Azure-SSIS IR. Der Task hat keine Funktion, bleibt aber ein paar Sekunden im Standbymodus. Der Ordner enthält außerdem einen `gacutil`-Ordner, dessen gesamter Inhalt (`gacutil.exe`, `gacutil.exe.config` und `1033\gacutlrc.dll`) unverändert in Ihren Container kopiert werden kann.

      1. Ein Ordner `UserScenarios` mit mehreren Beispielen für benutzerdefinierte Setups von echten Benutzerszenarios.

      ![Inhalt des Containers „Öffentliche Vorschau“](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   1. Doppelklicken Sie auf den Ordner `UserScenarios`, um die folgenden Elemente zu suchen:

      1. Ein Ordner `.NET FRAMEWORK 3.5` mit einem benutzerdefinierten Setup zum Installieren einer früheren Version von .NET Framework, das möglicherweise für benutzerdefinierte Komponenten auf jedem Knoten von Azure-SSIS IR erforderlich ist.

      1. Ein Ordner `BCP` mit einem benutzerdefinierten Setup zum Installieren von SQL Server-Befehlszeilenprogrammen (`MsSqlCmdLnUtils.msi`), einschließlich des Massenkopierprogramms (`bcp`), auf jedem Knoten von Azure-SSIS IR.

      1. Ein Ordner `EXCEL`, der ein benutzerdefiniertes Setupskript (`main.cmd`) für die Installation von C#-Assemblys/-Bibliotheken enthält, die Sie in Skripttasks zum dynamischen Lesen/Schreiben von Excel-Dateien auf den einzelnen Knoten ihrer Azure-SSIS IR verwenden können. Laden Sie zuerst `ExcelDataReader.dll` [hier](https://www.nuget.org/packages/ExcelDataReader/) und `DocumentFormat.OpenXml.dll` [hier](https://www.nuget.org/packages/DocumentFormat.OpenXml/) herunter und laden Sie dann alles zusammen mit `main.cmd` in Ihren Container hoch. Wenn Sie lediglich den standardmäßigen Excel-Verbindungs-Manager, die Standardquelle bzw. das Standardziel verwenden möchten, ist die erforderliche Access-Redistributable bereits in der Azure-SSIS IR vorinstalliert, sodass kein benutzerdefiniertes Setup notwendig ist.
      
      1. Ein Ordner `MYSQL ODBC` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des MySQL ODBC-Treibers auf allen Knoten der Azure-SSIS IR. Mit diesem Setup können Sie den ODBC-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum MySQL-Server verwenden. Laden Sie zuerst die aktuellen 64-Bit- und 32-Bit-Versionen der Installationsprogramme für den MySQL ODBC-Treiber, z. B. `mysql-connector-odbc-8.0.13-winx64.msi` und `mysql-connector-odbc-8.0.13-win32.msi`, von [MySQL](https://dev.mysql.com/downloads/connector/odbc/) herunter, und laden Sie dann alles zusammen mit `main.cmd` in Ihren Container hoch.

      1. Ein Ordner namens `ORACLE ENTERPRISE` mit einem benutzerdefinierten Setupskript (`main.cmd`) und einer Konfigurationsdatei für die unbeaufsichtigte Installation (`client.rsp`) zum Installieren der Oracle-Connectors und des OCI-Treibers auf jedem Knoten von Azure-SSIS IR, Enterprise Edition. Mit diesem Setup können Sie den Oracle-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum Oracle-Server verwenden. Laden Sie zunächst Microsoft Connectors v5.0 für Oracle (`AttunitySSISOraAdaptersSetup.msi` und `AttunitySSISOraAdaptersSetup64.msi`) aus dem [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55179) und den neuesten Oracle-Client (z.B. `winx64_12102_client.zip`) von [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) herunter, und laden Sie dann alles zusammen mit `main.cmd` und `client.rsp` in Ihren Container hoch. Wenn Sie bei der Herstellung einer Verbindung mit Oracle TNS verwenden, müssen Sie auch `tnsnames.ora` herunterladen, bearbeiten und in Ihren Container hochladen, damit die Datei während des Setups in den Oracle-Installationsordner kopiert werden kann.

      1. Ein Ordner `ORACLE STANDARD ADO.NET` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des Oracle ODP.NET-Treibers auf jedem Knoten von Azure-SSIS IR. Mit diesem Setup können Sie den ADO.NET-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum Oracle-Server verwenden. Laden Sie zunächst den aktuellen Oracle ODP.NET-Treiber, z. B. `ODP.NET_Managed_ODAC122cR1.zip`, von [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) herunter, und laden Sie ihn anschließend zusammen mit `main.cmd` in Ihren Container hoch.
       
      1. Ein Ordner `ORACLE STANDARD ODBC` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des Oracle ODBC-Treibers und Konfigurieren von DSN auf jedem Knoten Ihrer Azure-SSIS IR. Bei diesem Setup können Sie den ODBC-Verbindungs-Manager (Quelle/Ziel) oder den Power Query-Verbindungs-Manager (Quelle) mit ODBC-Datenquellen verwenden, um eine Verbindung mit dem Oracle-Server herzustellen. Laden Sie zunächst den neuesten Oracle Instant Client (Basic-Paket oder Basic Lite-Paket) und das ODBC-Paket, z. B. die 64-Bit-Pakete [hier](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Basic-Paket: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, Basic Lite-Paket: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, ODBC-Paket: `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`) oder die 32-Bit-Pakete [hier](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Basic-Paket: `instantclient-basic-nt-18.3.0.0.0dbru.zip`, Basic Lite-Paket: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, ODBC-Paket: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`) herunter, und laden Sie anschließend alle zusammen mit `main.cmd` in Ihren Container hoch.

      1. Ein Ordner `ORACLE STANDARD OLEDB` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des Oracle OLEDB-Treibers auf allen Knoten der Azure-SSIS IR. Mit diesem Setup können Sie den OLEDB-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum Oracle-Server verwenden. Laden Sie zunächst den aktuellen Oracle OLEDB-Treiber, z. B. `ODAC122010Xcopy_x64.zip`, von [Oracle](https://www.oracle.com/partners/campaign/index-090165.html) herunter, und laden Sie ihn anschließend zusammen mit `main.cmd` in Ihren Container hoch.

      1. Ein Ordner `POSTGRESQL ODBC` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des PostgreSQL ODBC-Treibers auf allen Knoten der Azure-SSIS IR. Mit diesem Setup können Sie den ODBC-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum PostgreSQL-Server verwenden. Laden Sie zuerst die aktuellen 64-Bit- und 32-Bit-Versionen der Installationsprogramme für den PostgreSQL ODBC-Treiber, z. B. `psqlodbc_x64.msi` und `psqlodbc_x86.msi`, von [PostgreSQL](https://www.postgresql.org/ftp/odbc/versions/msi/) herunter, und laden Sie dann alles zusammen mit `main.cmd` in Ihren Container hoch.

      1. Ein Ordner `SAP BW` mit einem benutzerdefinierten Setupskript (`main.cmd`) zum Installieren des .NET-Connector-Assemblys von SAP (`librfc32.dll`) auf allen Knoten von Azure-SSIS IR Enterprise Edition. Mit diesem Setup können Sie den SAP BW-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum SAP BW-Server verwenden. Laden Sie zuerst die 64-Bit- oder 32-Bit-Version von `librfc32.dll` aus dem SAP-Installationsordner zusammen mit `main.cmd` in Ihren Container hoch. Das Skript kopiert dann während des Setups die SAP-Assembly in den Ordner `%windir%\SysWow64` oder `%windir%\System32`.

      1. Ein Ordner `STORAGE` mit einem benutzerdefinierten Setup zum Installieren von Azure PowerShell auf jedem Knoten von Azure-SSIS IR. Bei diesem Setup können Sie SSIS-Pakete bereitstellen und ausführen, die [PowerShell-Skripts zum Bearbeiten Ihres Azure Storage-Kontos](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell) ausführen. Kopieren Sie `main.cmd`, ein `AzurePowerShell.msi`-Beispiel (oder verwenden Sie die neueste Version) und `storage.ps1` in den Container. Verwenden Sie PowerShell.dtsx als Vorlage für Ihre Pakete. In der Paketvorlage sind der [Azure Blob-Download-Task](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), der `storage.ps1` als modifizierbares PowerShell-Skript herunterlädt, und der [Task „Prozess ausführen“](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/), der das Skript auf jedem Knoten ausführt, zusammengefasst.

      1. Ein Ordner `TERADATA`, der ein benutzerdefiniertes Setupskript (`main.cmd` mit der zugehörigen Datei `install.cmd`) und die Installationspakete (`.msi`) enthält. Diese Dateien installieren Teradata-Connectors, die TPT-API und den ODBC-Treiber auf allen Knoten von Azure-SSIS IR, Enterprise Edition. Mit diesem Setup können Sie den Terradata-Verbindungs-Manager, die Quelle bzw. das Ziel für die Verbindung zum PostgreSQL-Server verwenden. Laden Sie zuerst die ZIP-Datei (z. B. `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) mit den Teradata Tools und Utilities 15.x von [Teradata](http://partnerintelligence.teradata.com) herunter, und laden Sie diese dann zusammen mit den oben genannten Dateien `.cmd` und `.msi` in Ihren Container hoch.

      1. Ein Ordner `ZULU OPENJDK` mit einem benutzerdefinierten Setupskript (`main.cmd`) und einer PowerShell-Datei (`install_openjdk.ps1`) zum Installieren von Zulu OpenJDK auf allen Knoten der Azure-SSIS IR. Dieses Setup ermöglicht die Verwendung von Azure Data Lake Store-/Flexible File-Connectors zur Verarbeitung von ORC/Parquet-Dateien. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java). Laden Sie zunächst das aktuelle Zulu OpenJDK, z. B. `zulu8.33.0.1-jdk8.0.192-win_x64.zip`, [hier](https://www.azul.com/downloads/zulu/zulu-windows/) herunter, und laden Sie es anschließend zusammen mit `main.cmd` und `install_openjdk.ps1` in Ihren Container hoch.

      ![Ordner im Ordner „Benutzerszenarios“](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   1. Um diese Beispiele für ein benutzerdefiniertes Setup auszuprobieren, kopieren Sie den Inhalt des ausgewählten Ordners, und fügen Sie ihn in Ihren Container ein.
   
      Wenn Sie Ihre Azure-SSIS IR mit der Data Factory-Benutzeroberfläche bereitstellen oder neu konfigurieren, aktivieren Sie das Kontrollkästchen **Azure-SSIS Integration Runtime mit zusätzlichen Systemkonfigurationen/Komponenteninstallationen anpassen** im Bereich **Erweiterte Einstellungen** und geben Sie den SAS URI Ihres Containers in das Feld **SAS-URI des Containers für benutzerdefinierte Setups** ein.
   
      Wenn Sie Ihre Azure-SSIS IR mit PowerShell bereitstellen oder neu konfigurieren, führen Sie das Cmdlet `Set-AzDataFactoryV2IntegrationRuntime` mit dem SAS-URI des Containers als Wert für den Parameter `SetupScriptContainerSasUri` aus.

## <a name="next-steps"></a>Nächste Schritte

-   [Enterprise Edition von Azure SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Entwicklung von kostenpflichtigen oder lizenzierten benutzerdefinierten Komponenten für Azure-SSIS Integration Runtime](how-to-develop-azure-ssis-ir-licensed-components.md)
