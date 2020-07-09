---
title: Verwalten von Paketen mit dem Paketspeicher für Azure-SSIS Integration Runtime
description: Informationen zum Verwalten von Paketen mit dem Paketspeicher für Azure-SSIS Integration Runtime
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
ms.date: 05/25/2020
ms.openlocfilehash: fa1ab60ae5690bd3782e4c849ca7cb5c29d640c2
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84198867"
---
# <a name="manage-packages-with-azure-ssis-integration-runtime-package-store"></a>Verwalten von Paketen mit dem Paketspeicher für Azure-SSIS Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Wenn Sie Ihre lokalen SQL Server Integration Services-Workloads (SSIS) in die Cloud verschieben möchten, können Sie Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen einer Azure-SSIS IR-Instanz](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure). Die Azure-SSIS IR unterstützt Folgendes:

- Ausführen von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt werden, wobei zum Hosten ein Azure SQL-Datenbank-Server/eine verwaltete Instanz verwendet wird (Projektbereitstellungsmodell)
- Ausführen von Paketen, die im Dateisystem, in Azure Files oder in SQL Server-Datenbank (MSDB) bereitgestellt werden, wobei zum Hosten Azure SQL Managed Instance verwendet wird (Paketbereitstellungsmodell)

Wenn Sie das Paketbereitstellungsmodell verwenden, können Sie auswählen, ob Sie Ihre Azure-SSIS IR-Instanz mit Paketspeichern bereitstellen möchten, die zusätzlich zum Dateisystem, zu Azure Files bzw. zu MSDB eine Paketverwaltungsebene bieten, die von Azure SQL Managed Instance gehostet wird. Ein Azure-SSIS IR-Paketspeicher ermöglicht Ihnen das Importieren, Exportieren, Löschen und Ausführen von Paketen sowie das Überwachen und Beenden der Ausführung von Paketen über SQL Server Management Studio (SSMS), ähnlich wie im [Legacy-SSIS-Paketspeicher](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service?view=sql-server-2017). 

## <a name="connect-to-azure-ssis-ir"></a>Herstellen einer Verbindung mit Azure-SSIS IR

Nachdem Sie Ihre Azure-SSIS IR-Instanz bereitgestellt haben, können Sie eine Verbindung mit dieser herstellen, um die zugehörigen Paketspeicher in SSMS zu durchsuchen.

![Herstellen einer Verbindung mit Azure-SSIS IR](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect.png)

Klicken Sie im Fenster **Objekt-Explorer** von SSMS im Dropdownmenü **Verbinden** auf **Azure-SSIS Integration Runtime**. Melden Sie sich als Nächstes bei Azure an, und wählen Sie das passende Abonnement, ADF und die Azure-SSIS IR-Instanz aus, an die Ihre Paketspeicher angefügt sind. Dann wird Ihre Azure-SSIS IR-Instanz mit den Knoten **Ausgeführte Pakete** und **Gespeicherte Pakete** angezeigt. Erweitern Sie den Knoten **Gespeicherte Pakete**, um die darunter liegenden Paketspeicher anzuzeigen. Erweitern Sie die Paketspeicher, um die darunter liegenden Ordner und Pakete anzuzeigen. Möglicherweise werden Sie aufgefordert, die Anmeldeinformationen für den Zugriff auf Ihre Paketspeicher einzugeben, wenn SSMS nicht automatisch eine Verbindung mit diesen herstellen kann. Wenn Sie z. B. einen Paketspeicher über MSDB erweitern, werden Sie möglicherweise zuerst aufgefordert, eine Verbindung mit Azure SQL Managed Instance herzustellen.

![Herstellen einer Verbindung mit Azure SQL Managed Instance](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect2.png)

## <a name="manage-folders-and-packages"></a>Verwalten von Ordnern und Paketen

Wenn Sie Ihre Azure-SSIS IR-Instanz in SSMS durchsuchen, können Sie mit der rechten Maustaste auf jeden beliebigen Paketspeicher, Ordner und jedes beliebige Paket klicken, damit ein Popupmenü mit den folgenden Optionen angezeigt wird: **Neuer Ordner**, **Paket importieren**, **Paket exportieren**, **Löschen** und **Aktualisieren**.

   ![Verwalten von Ordnern und Paketen](media/azure-ssis-integration-runtime-package-store/ssms-package-store-manage.png)

   *  Klicken Sie auf **Neuer Ordner**, um einen neuen Ordner für importierte Pakete zu erstellen.

   *  Klicken Sie auf **Paket importieren**, um Pakete aus dem **Dateisystem**, aus **SQL Server** (MSDB) oder dem **Legacy-SSIS-Paketspeicher** in Ihren Paketspeicher zu importieren.

      ![Importieren des Pakets](media/azure-ssis-integration-runtime-package-store/ssms-package-store-import.png)

      Wählen Sie je nach **Paketspeicherort**, aus dem Sie importieren, den betreffenden **Server**/**Authentifizierungstyp** aus, geben Sie wenn nötig die Anmeldeinformationen für den Zugriff ein, wählen Sie den **Paketpfad** aus, und geben Sie den neuen **Paketnamen** ein. Beim Paketimport kann die Schutzebene der Pakete nicht geändert werden. Wenn Sie sie ändern möchten, verwenden Sie SQL Server Data Tools (SSDT) oder das Befehlszeilenprogramm `dtutil`.

   *  Klicken Sie auf **Paket exportieren**, um Pakete aus Ihrem Paketspeicher in das **Dateisystem**, in **SQL Server** (MSDB) oder in den **Legacy-SSIS-Paketspeicher** zu exportieren.

      ![Paket exportieren](media/azure-ssis-integration-runtime-package-store/ssms-package-store-export.png)

      Wählen Sie je nach **Paketspeicherort**, in den Sie exportieren, den betreffenden **Server**/**Authentifizierungstyp** aus, geben Sie wenn nötig die Anmeldeinformationen für den Zugriff ein, und wählen Sie den **Paketpfad** aus. Wenn Sie verschlüsselte Pakete exportieren, geben Sie die Kennwörter ein, um sie zuerst zu entschlüsseln. Anschließend können Sie ihre Schutzebene ändern, z. B. damit vertrauliche Daten nicht gespeichert werden oder um alle/vertrauliche Daten mit einem Benutzerschlüssel/Kennwort zu verschlüsseln.

   *  Klicken Sie auf **Löschen**, um vorhandene Ordner/Pakete aus dem Paketspeicher zu löschen.

   *  Klicken Sie auf **Aktualisieren**, um neu hinzugefügte Ordner/Pakete im Paketspeicher anzuzeigen.

## <a name="execute-packages"></a>Ausführen von Paketen

Wenn Sie Ihre Azure-SSIS IR-Instanz in SSMS durchsuchen, können Sie mit der rechten Maustaste auf ein beliebiges gespeichertes Paket klicken, damit ein Popupmenü angezeigt wird, aus dem Sie die Option **Paket ausführen** auswählen können.  Dann wird das Dialogfeld **Paketausführungsprogramm** geöffnet, in dem Sie die Paketausführungen in Azure-SSIS IR als Aktivitäten vom Typ „SSIS-Paket ausführen“ in ADF-Pipelines konfigurieren können.

![Paketausführungsprogramm, Seite 1 und 2](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute.png)

![Paketausführungsprogramm, Seite 3 und 4](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute2.png)

Die Seiten **Allgemein**, **Konfigurationen**, **Ausführungsoptionen** und **Protokollierung** im Dialogfeld **Paketausführungsprogramm** entsprechen der Registerkarte **Einstellungen** der Aktivität „SSIS-Paket ausführen“, auf der Sie das Verschlüsselungskennwort für Ihr Paket, Zugriffsinformationen für Ihre Paketkonfigurationsdatei, Anmeldeinformationen/Eigenschaften für die Paketausführung sowie Zugriffsinformationen für Ihren Protokollordner eingeben können.  Die Seite **Werte festlegen** des Dialogfelds **Paketausführungsprogramm** entspricht der Registerkarte **Eigenschaftsüberschreibungen** der Aktivität „SSIS-Paket ausführen“, auf der Sie die bestehenden Paketeigenschaften eingeben können, die überschrieben werden sollen. Weitere Informationen finden Sie unter [Ausführen von SSIS-Paketen als Aktivitäten vom Typ „SSIS-Paket ausführen“ in ADF-Pipelines](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Wenn Sie im Dialogfeld **Paketausführungsprogramm** auf **Ausführen** klicken, wird automatisch eine neue ADF-Pipeline mit der Aktivität „SSIS-Paket ausführen“ erstellt und ausgelöst. Wenn bereits eine ADF-Pipeline mit denselben Einstellungen für die Paketausführung vorhanden ist, wird sie erneut ausgeführt, und es wird keine neue Pipeline generiert. Die ADF-Pipeline und die Aktivität „SSIS-Paket ausführen“ werden `Pipeline_SSMS_YourPackageName_HashString` bzw. `Activity_SSMS_YourPackageName`genannt.

![Schaltfläche „Paketausführungsprogramm“](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute3.png)

![Ausführen einer SSIS-Paketaktivität](media/azure-ssis-integration-runtime-package-store/ssis-activity-package-store.png)

## <a name="monitor-and-stop-running-packages"></a>Überwachen und Beenden von ausgeführten Paketen

Wenn Sie Ihre Azure-SSIS IR-Instanz in SSMS durchsuchen, können Sie den Knoten **Pakete ausführen** erweitern, um die Pakete anzuzeigen, die in diesem Moment ausgeführt werden.  Klicken Sie erst mit der rechten Maustaste auf ein beliebiges Popupmenü, und anschließend mit der linken auf **Beenden** oder **Aktualisieren**.

   ![Überwachen und Beenden von ausgeführten Paketen](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor.png)

   *  Klicken Sie auf **Beenden**, um die laufende ADF-Pipeline zu unterbrechen, die das Paket als Aktivität vom Typ „SSIS-Paket ausführen“ ausführt.

   *  Klicken Sie auf **Aktualisieren**, um neu ausgeführte Pakete aus Ihren Paketspeichern anzuzeigen.

## <a name="monitor-azure-ssis-ir-and-edit-package-stores"></a>Überwachen von Azure-SSIS IR und Bearbeiten von Paketspeichern

Wenn Sie Ihre Azure-SSIS IR-Instanz in SSMS durchsuchen, können Sie mit der rechten Maustaste auf diese klicken, damit ein Popupmenü angezeigt wird, aus dem Sie die Optionen **Go to Azure Data Factory portal** (Azure Data Factory-Portal aufrufen) oder **Aktualisieren** auswählen können.

   ![ADF-Portal aufrufen](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor2.png)

   *  Klicken Sie auf **Go to Azure Data Factory portal** (Azure Data Factory-Portal aufrufen), um die Seite **Integration Runtimes** des ADF-Überwachungshubs aufzurufen, auf der Sie Ihre Azure-SSIS IR-Instanz überwachen können. Der Kachel **PAKETSPEICHER** können Sie die Anzahl der Paketspeicher entnehmen, die an Ihre Azure-SSIS IR-Instanz angefügt sind.  Wenn Sie diese Zahl auswählen, wird ein Popupfenster angezeigt, in dem Sie verknüpfte ADF-Dienste bearbeiten können, in denen die Zugriffsinformationen für Ihre Paketspeicher gespeichert werden.

      ![Bearbeiten von Paketspeichern](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor3.png)

   *  Klicken Sie auf **Aktualisieren**, um neu hinzugefügte Ordner/Pakete in Ihren Paketspeichern und die über diese ausgeführten Pakete anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Sie können die automatisch generierten ADF-Pipelines mit Aktivitäten vom Typ „SSIS-Paket ausführen“ erneut ausführen bzw. bearbeiten oder neue Pipelines im ADF-Portal erstellen. Weitere Informationen finden Sie unter [Ausführen von SSIS-Paketen als Aktivitäten vom Typ „SSIS-Paket ausführen“ in ADF-Pipelines](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).