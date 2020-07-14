---
title: Ausführen von SSIS-Paketen mit den SSDT
description: Erfahren Sie, wie Sie über SSDT SSIS-Pakete in Azure ausführen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: sawinark
author: swinarko
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 07/31/2019
ms.openlocfilehash: 1d8261d05f59c7f40ba6b1e2d59d2b15ad56de95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84424564"
---
# <a name="execute-ssis-packages-in-azure-from-ssdt"></a>Ausführen von SSIS-Paketen in Azure über SSDT

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Dieser Artikel beschreibt das Feature für Azure-fähige SQL Server Integration Services-Projekte (SSIS) in SQL Server Data Tools (SSDT), mit dem Sie Pakete in der Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) ausführen können.  Mit diesem Feature können Sie vorhandene SSIS-Pakete vor einer Lift & Shift-Migration zu Azure testen oder neue SSIS-Pakete zur Ausführung in Azure entwickeln.

Sie können mithilfe dieses Features eine neue Azure-SSIS IR erstellen oder eine vorhandene IR an SSIS-Projekte anfügen und dann Ihr Pakete ausführen.  Unterstützt wird die Ausführung von Paketen, die im SSIS-Katalog (SSISDB) im Projektbereitstellungsmodell bereitgestellt werden, und von Paketen, die in Dateisystemen/Dateifreigaben/Azure Files im Paketbereitstellungsmodell bereitgestellt werden. 

## <a name="prerequisites"></a>Voraussetzungen
Um dieses Feature zu verwenden, laden Sie [hier](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) die aktuelle Version der SSDT mit SSIS-Projekterweiterung für Visual Studio oder [hier](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer) einen eigenständigen Installer herunter. Installieren Sie anschließend SSDT.

## <a name="azure-enable-ssis-projects"></a>Azure-fähige SSIS-Projekte
### <a name="create-new-azure-enabled-ssis-projects"></a>Erstellen neuer Azure-fähiger SSIS-Projekte
In SSDT können Sie mithilfe der Vorlage **Integration Services-Projekt (Azure-fähig)** neue, Azure-fähige SSIS-Projekte erstellen.

   ![Neues Azure-fähiges SSIS-Projekt](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-new-project.png)

Nachdem das Azure-fähige Projekt erstellt wurde, werden Sie in Azure Data Factory aufgefordert, eine Verbindung mit SSIS herzustellen.

   ![Aufforderung zum Verbinden mit der Azure-SSIS IR](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-integration-runtime-connect-prompt.png)

Wenn Sie sofort eine Verbindung mit ihrer Azure-SSIS IR herstellen möchten, finden Sie weitere Informationen unter [Herstellen einer Verbindung mit Azure-SSIS IR](#irconnect). Sie können auch später eine Verbindung herstellen, indem Sie mit der rechten Maustaste auf den Projektknoten im Projektmappen-Explorer-Panel von SSDT klicken, um ein Menü aufzurufen, und dann den Menüpunkt **Mit SSIS in Azure Data Factory verbinden** unter dem Untermenü **SSIS in Azure Data Factory** auswählen.

### <a name="azure-enable-existing-ssis-projects"></a>Vorhandenes SSIS-Projekt für Azure aktivieren
Für bestehende SSIS-Projekte können Sie Azure aktivieren, indem Sie die folgenden Schritte befolgen:

1. Klicken Sie mit der rechten Maustaste auf den Projektknoten im Projektmappen-Explorer-Panel von SSDT, um ein Menü aufzurufen, und wählen Sie dann den Menüeintrag **Azure-fähiges Projekt** unter dem Untermenü **SSIS in Azure Data Factory**, um den **Assistent für Azure-fähige Projekte** zu starten.

   ![Vorhandenes SSIS-Projekt für Azure aktivieren](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-for-existing-project.png)

2. Wählen Sie auf der Seite **Visual Studio-Konfiguration auswählen** die Visual Studio-Konfiguration aus, um die Einstellungen für die Paketausführung in Azure anzuwenden. Eine geeignete Methode besteht darin, eine neue Visual Studio-Konfiguration für die Cloud zu erstellen und Ihr Projekt anhand der Cloudkonfiguration für Azure zu aktivieren. Mit mehreren Konfigurationen können Sie Ihren Parametern unterschiedliche Werte basierend auf den verschiedenen Umgebungen zuweisen (entweder lokal oder in Azure). Weitere Informationen finden Sie in [diesem Beispiel](#example).

   ![Auswählen der Visual Studio-Konfiguration](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-select-visual-studio-configurations.png)

3. Um Ihre vorhandenen SSIS-Projekte für Azure zu aktivieren, müssen Sie die Zielserverversion auf die neueste von Azure-SSIS IR unterstützte Version festlegen. Dies ist aktuell **SQL Server 2017**. Wenn Sie dies also noch nicht getan haben, müssen Sie überprüfen, ob Ihr Paket zusätzliche Komponenten enthält, die von SQL Server 2017 nicht unterstützt werden, und auf die Schaltfläche „Weiter“ klicken, um fortzufahren, wenn es keine Bedenken gibt.

   ![Wechseln der Zielserverversion](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-switch-target-server-version-step.png)

4. Weitere Informationen zum Herstellen der Verbindung mit Azure-SSIS IR finden Sie unter [Herstellen einer Verbindung mit Azure-SSIS IR](#irconnect).

## <a name="connect-azure-enabled-projects-to-ssis-in-azure-data-factory"></a><a name="irconnect"></a> Herstellen einer Verbindung von Azure-fähigen Projekten mit SSIS in Azure Data Factory

Durch die Verbindungsherstellung Ihrer Azure-fähigen Projekte mit SSIS in ADF können Sie Ihre Pakete in Azure Files hochladen und in Azure-SSIS IR ausführen. Führen Sie dazu diese Schritte aus:

1. Lesen Sie auf der Seite **SSIS in ADF – Einführung** die bereitgestellten Informationen, und klicken Sie auf **Weiter**, um den Vorgang fortzusetzen.

   ![SSIS in ADF – Einführung](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard.png)

2. Wählen Sie auf der Seite **SSIS IR in ADF auswählen** Ihre vorhandene ADF-Instanz und Ihre Azure-SSIS IR zur Ausführung von Paketen aus, oder erstellen Sie eine neue Instanz/IR (sofern nicht bereits geschehen).
   - Um Ihre vorhandene Azure-SSIS IR auszuwählen, wählen Sie zunächst das relevante Azure-Abonnement und die ADF-Instanz aus.
   - Wenn Sie eine vorhandene ADF-Instanz auswählen, die über keine Azure-SSIS IR verfügt, klicken Sie auf die Schaltfläche **SSIS IR erstellen**, um eine neue IR im ADF-Portal bzw. der App zu erstellen.
   - Wenn Sie ein vorhandenes Azure-Abonnement auswählen, das keine ADF-Instanz umfasst, klicken Sie auf die Schaltfläche **SSIS IR erstellen**, um den **Assistenten zum Erstellen einer Integration Runtime** zu starten. In diesem Assistenten können Sie den Standort und ein Präfix für die automatische Erstellung einer neuen Azure-Ressourcengruppe, Data Factory-Instanz und SSIS IR in Ihrem Namen eingeben. Das Namensmuster lautet wie folgt: **IhrPräfix-RG/DF/IR-ZeitpunktDerErstellung**.

   ![SSIS IR in ADF auswählen](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard2.png)

3. Wählen Sie auf der Seite **Azure Storage-Instanz auswählen** Ihr vorhandenes Azure Storage-Konto zum Hochladen von Paketen in Azure Files aus, oder erstellen Sie ein neues Konto (sofern nicht bereits geschehen).
   - Um Ihr vorhandenes Azure Storage-Konto auszuwählen, wählen Sie zunächst das relevante Azure-Abonnement aus.
   - Wenn Sie dasselbe Azure-Abonnement auswählen wie für Ihre Azure-SSIS IR ohne Azure Storage-Konto, klicken Sie auf die Schaltfläche **Azure Storage-Instanz erstellen**, damit in Ihrem Namen automatisch ein Azure Storage-Konto am selben Standort erstellt wird wie Ihre Azure-SSIS IR. Hierbei wird zur Benennung ein Namenspräfix Ihrer SSIS IR mit dem Zeitpunkt der Erstellung kombiniert.
   - Wenn Sie ein anderes Azure-Abonnement auswählen, das kein Azure Storage-Konto umfasst, klicken Sie auf die Schaltfläche **Azure Storage-Instanz erstellen**, um ein neues Konto im Azure-Portal zu erstellen.

   ![Auswählen von Azure Storage](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard3.png)

4. Klicken Sie auf die Schaltfläche **Verbinden**, um Ihre Verbindung herzustellen.  Die ausgewählte Azure-SSIS IR und das Azure Storage-Konto werden unterhalb des Knotens **Verknüpfte Azure-Ressourcen** im Projektmappen-Explorer-Panel von SSDT angezeigt.  Außerdem wird der Status Ihrer Azure-SSIS IR aktualisiert. Sie können den Status der IR verwalten, indem Sie mit der rechten Maustaste auf den zugehörigen Knoten klicken, um ein Popupmenü zu öffnen. In diesem Menü können Sie das Menüelement **Starten\Beenden\Verwalten** auswählen, um zur Ausführung der jeweiligen Aufgabe zum ADF-Portal bzw. zur App zu wechseln.

## <a name="execute-ssis-packages-in-azure"></a>Ausführen von SSIS-Paketen in Azure
### <a name="azure-enabled-setting"></a>Für Azure geeignete Einstellung
Bevor Sie Ihre Pakete in Azure ausführen, können Sie Ausführungseinstellungen konfigurieren. Wenn Sie die Windows-Authentifizierung für Ihre SSIS-Pakete aktivieren möchten, befolgen Sie die folgenden Schritte:

1. Klicken Sie mit der rechten Maustaste auf den Projektknoten im Projektmappen-Explorer-Panel von SSDT, um ein Menü aufzurufen, und wählen Sie dann den Menüeintrag **Für Azure geeignete Einstellungen** unter dem Untermenü **SSIS in Azure Data Factory**.

   ![Für Azure geeignete Einstellungen](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-azure-enabled-setting.png)

2. Klicken Sie auf die Dropdownliste **Windows-Authentifizierung aktivieren**, und wählen Sie **True** (Wahr) aus. Klicken Sie dann auf die Schaltfläche „Bearbeiten“ für die Option **Anmeldeinformationen für die Windows-Authentifizierung**, um die Anmeldeinformationen einzugeben.

   ![Aktivieren der Windows-Authentifizierung](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-windows-authentication-open.png)

3. Geben Sie Anmeldeinformationen im Editor für **Anmeldeinformationen für die Windows-Authentifizierung** ein.

   ![Anmeldeinformationen für die Windows-Authentifizierung](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-windows-authentication-credential.png)

### <a name="starting-package-executions"></a>Starten von Paketausführungen
Nachdem Sie Ihre Projekte mit SSIS in ADF verbunden haben, können Sie Pakete in Azure-SSIS IR ausführen.  Sie haben zwei Möglichkeiten, um Paketausführungen zu starten:
-  Klicken Sie auf die Schaltfläche **Starten** in der SSDT-Symbolleiste, und wählen Sie im daraufhin angezeigten Dropdownmenü die Option **In Azure ausführen** aus. 

   ![In Azure ausführen](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package.png)

-  Klicken Sie mit der rechten Maustaste im Projektmappen-Explorer-Panel von SSDT auf den Paketknoten, und wählen Sie im daraufhin angezeigten Popupmenü die Option **Paket in Azure ausführen** aus.

   ![Paket in Azure ausführen](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package2.png)

> [!NOTE]
> Um Ihre Pakete in Azure ausführen zu können, müssen Sie über eine ausgeführte Azure-SSIS IR verfügen. Wenn Ihre Azure-SSIS IR angehalten wurde, wird ein Dialogfeld für ihren Start geöffnet.  Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein. Es kann aber etwa 20 bis 30 Minuten dauern, bis Azure-SSIS IR einem virtuellen Netzwerk beitritt.  Nach dem Ausführen Ihrer Pakete in Azure können Sie Ihre Azure-SSIS IR beenden, um Ausführungskosten einzusparen. Klicken Sie hierzu mit der rechten Maustaste auf den zugehörigen Knoten im Projektmappen-Explorer-Panel von SSDT, um ein Popupmenü zu öffnen. Wählen Sie dann im Popupmenü die Option **Starten\Beenden\Verwalten** aus, um zur Ausführung der jeweiligen Aufgabe zum ADF-Portal bzw. zur App zu wechseln.

### <a name="checking-package-execution-logs"></a>Überprüfen der Protokolle zur Paketausführung
Wenn Sie die Paketausführung starten, wird das zugehörige Protokoll formatiert und im Fortschrittsfenster von SSDT angezeigt.  Bei Paketen mit langer Ausführungsdauer wird das Protokoll in regelmäßigen Abständen aktualisiert.  Sie können die Paketausführung beenden, indem Sie auf die Schaltfläche **Beenden** in der Symbolleiste von SSDT klicken. Der Vorgang wird sofort abgebrochen.  Die unformatierten Protokolldateien finden Sie im zugehörigen UNC-Pfad (Universal Naming Convention): `\\<YourConnectedAzureStorage>.file.core.windows.net\ssdtexecution\<YourProjectName-FirstConnectTime>\<YourPackageName-tmp-ExecutionTime>\logs`, aber diese Daten werden nach einem Tag bereinigt.

### <a name="switching-package-protection-level"></a>Wechsel der Schutzebene für Pakete
Für die Ausführung von SSIS-Paketen in Azure werden die Schutzebenen **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey** nicht unterstützt.  Wenn Ihre Pakete also mit diesen Schutzebenen konfiguriert sind, wird die Schutzebene temporär auf **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** mit zufällig generierten Kennwörtern umgestellt, wenn Ihre Pakete zur Ausführung in Ihrer Azure-SSIS IR in Azure Files hochgeladen werden.

> [!NOTE]
> Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere, mit den Schutzebenen **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey** konfigurierte Pakete verweisen, müssen Sie diese manuell zur Verwendung von **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** rekonfigurieren, bevor Sie die Pakete ausführen.

Wenn Ihre Pakete bereits mit den Schutzebenen **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** konfiguriert sind, bleiben sie unverändert. Es werden aber dennoch zufällig generierte Kennwörter verwendet, wenn Ihre Pakete zur Ausführung in Ihrer Azure-SSIS IR in Azure Files hochgeladen werden.

### <a name="using-package-configuration-file"></a>Verwenden einer Paketkonfigurationsdatei
Wenn Sie im Paketbereitstellungsmodell Paketkonfigurationsdateien verwenden, um zur Laufzeit Variablenwerte zu ändern, werden diese Dateien automatisch in Azure Files hochgeladen, um in Ihrer Azure-SSIS IR ausgeführt zu werden.

### <a name="using-execute-package-task"></a>Verwenden eines Tasks „Paket ausführen“
Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere, in lokalen Dateisystemen gespeicherte Pakete verweisen, müssen Sie die folgenden zusätzlichen Einrichtungsschritte ausführen:

1. Laden Sie die anderen Pakete mit demselben Azure Storage-Konto in Azure Files hoch wie demjenigen zur Verbindung Ihrer Projekte, und rufen Sie den neuen UNC-Pfad ab, z.B. `\\test.file.core.windows.net\ssdtexecution\Package1.dtsx`.

2. Ersetzen Sie den Dateipfad dieser anderen Pakete im Dateiverbindungs-Manager der Tasks „Paket ausführen“ durch den neuen UNC-Pfad.
   - Wenn Ihr Computer zur Ausführung von SSDT den neuen UNC-Pfad nicht verarbeiten kann, können Sie den Dateipfad im Panel „Eigenschaften“ des Dateiverbindungs-Managers ändern.
   - Alternativ können Sie eine Variable für den Dateipfad verwenden, um zur Laufzeit den richtigen Wert zuzuweisen.

Wenn Ihre Pakete Tasks zur Paketausführung enthalten, die auf andere Pakete im selben Projekt verweisen, ist keine zusätzliche Einrichtung erforderlich.

## <a name="switching-package-execution-environments-with-azure-enabled-projects"></a><a name="example"></a> Wechseln von Paketausführungsumgebungen mit Azure-fähigen Projekten

Zum Wechseln von Paketausführungsumgebungen mit Azure-fähigen Projekten können Sie mehrere Visual Studio-Konfigurationen erstellen, um unterschiedliche Werte für umgebungsspezifische Parameter anzuwenden. Es gibt z. B. ein einfaches SSIS-Paket mit einem **Task „Dateisystem“** , der Attribute der angegebenen Datei festlegt. Sie können sie einfach mit den folgenden Schritten in die Cloud migrieren:

1. Definieren Sie einen **FilePath**-Parameter vom Typ „Zeichenfolge“, der den Dateipfad der Zieldatei angibt.

   ![Definieren der Paketparameter](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-define-parameters.png)

2. Ordnen Sie diesem Parameter die **Quellverbindung** zu. 

   ![Aktualisieren der Quellverbindung](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-update-task-with-parameters.png)

3. Erstellen Sie eine neue Visual Studio-Konfiguration für die Cloud in Visual Studio Configuration Manager.

4. Definieren Sie die Werte für diesen Parameter für jede Visual Studio-Konfiguration.

   ![Außerkraftsetzung von Parameterwerten](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-override-parameter.png)

5. Azure hat dieses SSIS-Projekt anhand der Visual Studio-Konfiguration für die Cloud aktiviert.

6. Führen Sie dieses Paket in Azure aus. Sie können einfach wieder zur lokalen Umgebung wechseln, indem Sie die aktuelle Visual Studio-Konfiguration wechseln.

   ![Wechseln der Visual Studio-Konfiguration](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-switch-configurations.png)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Ausführung Ihrer Pakete in Azure aus SSDT erfolgreich abgeschlossen haben, können Sie mithilfe einer Aktivität „SSIS-Paket ausführen“ in ADF-Pipelines bereitstellen und ausführen. Siehe hierzu [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).
