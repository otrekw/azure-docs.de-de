---
title: Konfigurieren einer selbstgehosteten Integration Runtime als Proxy für SSIS
description: Erfahren Sie, wie Sie eine selbstgehostete Integration Runtime als Proxy für Azure-SSIS Integration Runtime konfigurieren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 12/23/2019
ms.openlocfilehash: 48d4df5684c84e195810439912dd610f5af364d4
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964480"
---
# <a name="configure-self-hosted-ir-as-a-proxy-for-azure-ssis-ir-in-adf"></a>Konfigurieren einer selbstgehosteten IR als Proxy für Azure-SSIS IR in ADF

In diesem Artikel wird beschrieben, wie Sie SSIS-Pakete (SQL Server Integration Services) in Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) mit einer selbstgehosteten IR ausführen, die als Proxy konfiguriert ist.  Mit dieser Funktion können Sie auf lokale Daten zugreifen, ohne Ihre [Azure-SSIS IR mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) zu verbinden.  Dies ist hilfreich, wenn Ihr Unternehmensnetzwerk über eine übermäßig komplexe Konfiguration bzw. restriktive Richtlinie verfügt, in die Sie Ihre Azure-SSIS IR einfügen müssen.

Diese Funktion teilt Ihr Paket, das einen Datenflusstask mit lokaler Datenquelle enthält, in zwei Stagingtasks auf: Der erste, der für Ihre selbstgehostete IR ausgeführt wird, verschiebt zunächst Daten aus der lokalen Datenquelle in einen Stagingbereich in Azure Blob Storage, während der zweite, der für Azure-SSIS IR ausgeführt wird, dann Daten aus dem Stagingbereich in das vorgesehene Datenziel verschiebt.

Diese Funktion bietet auch andere Vorteile/Fähigkeiten, da sie es Ihnen ermöglicht, Ihre selbstgehostete IR in Regionen bereitzustellen, die noch nicht von Azure-SSIS IR unterstützt werden, die öffentliche statische IP-Adresse Ihrer selbstgehosteten IR auf der Firewall Ihrer Datenquellen zuzulassen und vieles mehr.

## <a name="prepare-self-hosted-ir"></a>Vorbereiten der selbstgehosteten IR

Um dieses Feature verwenden zu können, müssen Sie zunächst eine ADF erstellen und ihre Azure-SSIS IR unter dieser bereitstellen, sofern dies noch nicht geschehen ist, indem Sie die Anleitungen im Artikel [Bereitstellen einer Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) befolgen.

Anschließend müssen Sie Ihre selbstgehostete IR unter derselben ADF bereitstellen, in der Ihre Azure-SSIS IR bereitgestellt wird, indem Sie die Anleitungen im Artikel [Erstellen einer selbstgehosteten IR](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime) befolgen.

Zum Schluss müssen Sie die neueste Version der selbstgehosteten IR sowie die zusätzlichen Treiber und die Runtime auf Ihren lokalen Computer/virtuellen Azure-Computer (VM) wie folgt herunterladen und dort installieren:
- Laden Sie die neueste Version der selbstgehosteten IR [hier](https://www.microsoft.com/download/details.aspx?id=39717) herunter, und installieren Sie sie.
- Wenn Sie OLEDB-Connectors in ihren Paketen verwenden, laden Sie die relevanten OLEDB-Treiber herunter, und installieren Sie sie auf demselben Computer, auf dem die selbstgehostete IR installiert ist, falls dies noch nicht geschehen ist.  Wenn Sie die frühere Version des OLE DB-Treibers für SQL Server (SQLNCLI) verwenden, können Sie die 64-Bit-Version [hier](https://www.microsoft.com/download/details.aspx?id=50402) herunterladen.  Wenn Sie die neueste Version des OLE DB-Treibers für SQL Server (MSOLEDBSQL) verwenden, können Sie die 64-Bit-Version [hier](https://www.microsoft.com/download/details.aspx?id=56730) herunterladen.  Wenn Sie OLE DB-Treiber für andere Datenbanksysteme wie PostgreSQL, MySQL, Oracle usw. verwenden, können Sie die 64-Bit-Version von den jeweiligen Websites herunterladen.
- Laden Sie die Visual C++-Laufzeit (VC) auf denselben Computer herunter, auf dem auch die selbstgehostete IR installiert ist, und installieren Sie sie (wenn dies noch nicht geschehen ist).  Sie können die 64-Bit-Version [hier](https://www.microsoft.com/download/details.aspx?id=40784) herunterladen.

## <a name="prepare-azure-blob-storage-linked-service-for-staging"></a>Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging

Erstellen Sie einen mit Azure Blob Storage verknüpften Dienst unter der gleichen ADF, in der Ihre Azure-SSIS IR bereitgestellt wird, indem Sie die Anleitungen im Artikel [Erstellen eines mit ADF verknüpften Diensts](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service) befolgen (sofern dies noch nicht geschehen ist).  Stellen Sie Folgendes sicher:
- **Azure Blob Storage** ist als **Datenspeicher** ausgewählt.
- **AutoResolveIntegrationRuntime** ist für die Option **Connect via integration runtime** (Verbindung über Integration Runtime herstellen) ausgewählt.
- Eine der Optionen **Account key (Kontoschlüssel)** /**SAS URI (SAS-URI)** /**Service Principal (Diesntprinzipal)** ist als **Authentifizierungsmethode** ausgewählt.

>[!TIP]
>Gewähren Sie bei Auswahl von  **Dienstprinzipal**  zumindest die  **Rolle als Mitwirkender an Speicherblobdaten** aus. Weitere Informationen finden Sie unter [Azure Blob Storage-Connector](connector-azure-blob-storage.md#linked-service-properties).

![Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-azure-ssis-ir-with-self-hosted-ir-as-a-proxy"></a>Konfigurieren von Azure-SSIS IR mit der selbstgehosteten IR als Proxy

Nachdem Sie Ihre selbstgehostete IR und den mit Azure Blob Storage verknüpften Dienst für das Staging vorbereitet haben, können Sie nun Ihre neue/vorhandene Azure-SSIS IR mit der selbstgehosteten IR als Proxy im ADF-Portal bzw. in der App konfigurieren.  Wenn Ihre vorhandene Azure-SSIS IR ausgeführt wird, müssen Sie sie vor diesem Vorgang beenden und anschließend neu starten.

1. Navigieren Sie im Bereich für die Integration Runtime-Einrichtung durch die Abschnitte **Allgemeine Einstellungen** und **SQL-Einstellungen**, indem Sie die Schaltfläche **Weiter** auswählen. 

1. Gehen Sie im Abschnitt **Erweiterte Einstellungen** folgendermaßen vor:

   1. Aktivieren Sie das Kontrollkästchen **Selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS Integration Runtime**. 

   1. Wählen Sie für die **selbstgehostete Integration Runtime** Ihre vorhandene selbstgehostete IR als Proxy für Azure-SSIS IR aus.

   1. Wählen Sie für den **verknüpften Dienst des Stagingspeichers** den verknüpften Dienst für Ihren vorhandenen Azure-Blobspeicher aus oder erstellen Sie einen neuen Dienst für den Stagingprozess.

   1. Geben Sie für den **Stagingpfad** einen Blobcontainer in Ihrem ausgewählten Azure-Blobspeicherkonto an oder lassen Sie das Feld leer, um einen Standardpfad für den Stagingprozess zu verwenden.

   1. Wählen Sie **Weiter**.

   ![Erweiterte Einstellungen bei einer selbstgehosteten IR](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

Sie können auch Ihre neue/vorhandene Azure-SSIS IR mit selbstgehosteter IR als Proxy mithilfe von PowerShell konfigurieren.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>Aktivieren von SSIS-Paketen für die Verbindungsherstellung über den Proxy

Wenn Sie die neuesten SSDT mit der Erweiterung für SSIS-Projekte für Visual Studio verwenden, die von [hier](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) oder als eigenständiger Installer (von [hier](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)) heruntergeladen werden können, können Sie eine neue **ConnectByProxy**-Eigenschaft nutzen, die in OLEDB-/Flatfile-Verbindungs-Managern hinzugefügt wurde.  

Wenn Sie neue Pakete mit Datenflusstasks mit OLEDB-/Flatfilequellen für den lokalen Zugriff auf Datenbanken/Dateien entwerfen, können Sie diese Eigenschaft aktivieren, indem Sie sie im Eigenschaftenbereich der relevanten Verbindungs-Manager auf **TRUE** festlegen.

![Aktivieren der ConnectByProxy-Eigenschaft](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Sie können diese Eigenschaft auch aktivieren, wenn Sie vorhandene Pakete ausführen, ohne sie manuell einzeln ändern zu müssen.  Es gibt zwei Optionen:
- Öffnen, Neuerstellen und erneutes Bereitstellen des Projekts, das diese Pakete enthält, mit den neuesten SSDT, die in Ihrer Azure-SSIS IR ausgeführt werden sollen: Die Eigenschaft kann dann aktiviert werden, indem sie für die entsprechenden Verbindungs-Manager auf **TRUE** festgelegt wird, die auf der Registerkarte **Connection Managers** (Verbindungs-Manager) des Popupfensters „Execute Package“ (Paket ausführen) angezeigt werden, wenn Pakete aus SSMS ausgeführt werden.

  ![Aktivieren der ConnectByProxy-Eigenschaft (2)](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Die Eigenschaft kann auch aktiviert werden, indem sie für die entsprechenden Verbindungs-Manager auf **TRUE** festgelegt wird, die auf der Registerkarte **Connection Managers** (Verbindungs-Manager) der [Aktivität „SSIS-Paket ausführen](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) angezeigt werden, wenn Pakete in ADF-Pipelines ausgeführt werden.
  
  ![Aktivieren der ConnectByProxy-Eigenschaft (3)](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- Durch erneute Bereitstellung des Projekts, das diese Pakete enthält, für die Ausführung in der SSIS IR: Die Eigenschaft kann dann aktiviert werden, indem ihr Eigenschaftspfad (`\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`) bereitgestellt und auf **TRUE** als Eigenschaftsüberschreibung auf der Registerkarte **Advanced** (Erweitert) des Popupfensters „Execute Package“ (Paket ausführen) festgelegt wird, wenn Pakete aus SSMS ausgeführt werden.

  ![Aktivieren der ConnectByProxy-Eigenschaft (4)](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Die Eigenschaft kann auch aktiviert werden, indem ihr Eigenschaftspfad (`\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`) bereitgestellt und auf **TRUE** als Eigenschaftsüberschreibung auf der Registerkarte **Property Overrides** (Eigenschaftenüberschreibungen) der [Aktivität „SSIS-Paket ausführen“](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) festgelegt wird, wenn Pakete in ADF-Pipelines ausgeführt werden.
  
  ![Aktivieren der ConnectByProxy-Eigenschaft (5)](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>Debuggen der ersten und zweiten Stagingtasks

In ihrer selbstgehosteten IR finden Sie die Laufzeitprotokolle im Ordner `C:\ProgramData\SSISTelemetry` und die Ausführungsprotokolle der ersten Stagingtasks im Ordner `C:\ProgramData\SSISTelemetry\ExecutionLog`.  Die Ausführungsprotokolle der zweiten Stagingtasks finden Sie in Ihrer SSISDB oder in den angegebenen Protokollierungspfaden abhängig davon, ob Sie die Pakete in SSISDB oder im Dateisystem, auf Dateifreigaben bzw. in Azure Files speichern.  Die eindeutigen IDs der ersten Stagingtasks finden Sie auch in den Ausführungsprotokollen der zweiten Stagingtasks. Beispiel: 

![Eindeutige ID des ersten Stagingtasks](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="using-windows-authentication-in-staging-tasks"></a>Verwenden der Windows-Authentifizierung in Stagingtasks

Wenn für Ihre Stagingtasks der selbstgehosteten IR die Windows-Authentifizierung erforderlich ist, müssen Sie die [SSIS-Pakete für die Verwendung derselben Windows-Authentifizierung konfigurieren](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15). Die Stagingtasks werden mit dem Dienstkonto der selbstgehosteten IR (standardmäßig `NT SERVICE\DIAHostService`) aufgerufen und der Zugriff auf die Datenspeicher erfolgt über das Windows-Authentifizierungskonto. Beiden Konten müssen bestimmte Sicherheitsrichtlinien zugewiesen werden. Öffnen Sie auf dem Computer mit der selbstgehosteten IR `Local Security Policy` -> `Local Policies` -> `User Rights Assignment` und führen Sie die folgenden Schritte aus.
- Weisen Sie die Richtlinien **Arbeitsspeicherkontingente für einen Prozess anpassen** und **Token auf Prozessebene ersetzen** dem Dienstkonto der selbstgehosteten IR zu. Dies sollte automatisch erfolgen, wenn die selbstgehostete IR mit dem Standarddienstkonto installiert wird. Wenn Sie ein anderes Dienstkonto verwenden, müssen Sie ihm diese Richtlinien zuweisen.
- Weisen Sie dem Windows-Authentifizierungskonto die Richtlinie **Als Dienst anmelden** zu.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>Abrechnung der ersten und zweiten Stagingtasks

Die ersten Stagingtasks, die für Ihre selbstgehostete IR ausgeführt werden, werden separat auf die gleiche Weise wie alle Datenverschiebungsaktivitäten in Rechnung gestellt, die für die selbst gehostete IR ausgeführt werden. Weitere Informationen dazu finden Sie im Artikel [Preise für die ADF-Datenpipeline](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/).

Die zweiten Stagingtasks, die für ihre Azure-SSIS IR ausgeführt werden, werden nicht separat abgerechnet, aber Ihre Ausführung von Azure-SSIS IR wird wie im Artikel [Preise für Azure-SSIS IR](https://azure.microsoft.com/pricing/details/data-factory/ssis/) beschrieben in Rechnung gestellt.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Zurzeit werden nur Datenflusstasks mit ODBC-/OLEDB-/Flatfile-Verbindungs-Managern und ODBC-/OLEDB-/Flatfilequellen unterstützt. 
- Nur mit Azure Blob Storage verknüpfte Dienste, die mit den Authentifizierungsmethoden **Kontoschlüssel**/**SAS-URI**/**Dienstprinzipal** konfiguriert sind, werden derzeit unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Sobald Sie Ihre selbstgehostete IR als Proxy für Ihre Azure-SSIS IR konfiguriert haben, können Sie Pakete bereitstellen und ausführen, um auf lokale Daten mithilfe von Aktivitäten „SSIS-Paket ausführen“ in ADF-Pipelines zuzugreifen. Siehe hierzu [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).