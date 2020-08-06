---
title: Visualisieren von Azure Cognitive Search-Protokollen und -Metriken mit Power BI
description: Visualisieren von Azure Cognitive Search-Protokollen und -Metriken mit Power BI
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: d529e4e4b79f0c34eb8f95b028cca730b316d5f6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87060306"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Visualisieren von Azure Cognitive Search-Protokollen und -Metriken mit Power BI
Mit [Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-what-is-azure-search) können Sie Vorgangsprotokolle und Dienstmetriken zu Ihrem Suchdienst in einem Azure Storage-Konto speichern. Diese Seite enthält Anweisungen dazu, wie Sie diese Informationen über eine Power BI-Vorlagen-App visualisieren können. Die App bietet detaillierte Einblicke in ihren Suchdienst, einschließlich Informationen zu Suchen, Indizierung, Vorgängen und Dienstmetriken.

Sie finden die Power BI-Vorlagen-App **Azure Cognitive Search: Analyze Logs and Metrics** im [Power BI-Apps-Marketplace](https://appsource.microsoft.com/marketplace/apps).

## <a name="how-to-get-started-with-the-app"></a>Erste Schritte mit der App

1. Aktivieren Sie Metriken und Ressourcenprotokollierung für Ihren Suchdienst:

    1. Erstellen oder identifizieren Sie ein vorhandenes [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account), in dem Sie die Protokolle archivieren können.
    1. Navigieren Sie im Azure-Portal zu Ihrem Azure Cognitive Search-Dienst.
    1. Wählen Sie in der linken Spalte im Abschnitt „Überwachung“ die Option **Diagnoseeinstellungen** aus.

        ![Screenshot, der zeigt, wie die Diagnoseeinstellungen im Abschnitt „Überwachung“ des Azure Cognitive Search-Diensts ausgewählt werden.](media/search-monitor-logs-powerbi/diagnostic-settings.png)

    1. Wählen Sie **+ Diagnoseeinstellung hinzufügen**  aus.
    1. Aktivieren Sie **In einem Speicherkonto archivieren**, geben Sie Ihre Speicherkontoinformationen an, und aktivieren Sie **OperationLogs** und **AllMetrics**.

        ![Screenshot zur Auswahl von Metriken und Ressourcenprotokollierung auf der Seite mit den Diagnoseeinstellungen.](media/search-monitor-logs-powerbi/add-diagnostic-setting.png)
    1. Wählen Sie **Speichern** aus.

1. Verwenden Sie nach dem Aktivieren der Protokollierung ihren Suchdienst, um mit dem Generieren von Protokollen und Metriken zu beginnen. Es kann bis zu einer Stunde dauern, bis die Container mit diesen Protokollen im Blobspeicher angezeigt werden. Es wird ein Container **insights-logs-operationlogs** für Suchdatenverkehrsprotokolle und ein Container **insights-metrics-pt1m** angezeigt.

1. Suchen Sie die Power BI-App für Azure Cognitive Search im [Marketplace für Power BI-Apps](https://appsource.microsoft.com/marketplace/apps), und installieren Sie sie in einem neuen oder vorhandenen Arbeitsbereich. Die App wird **Azure Cognitive Search: Analyze Logs and Metrics** genannt.

1. Nachdem Sie die App installiert haben, wählen Sie die App in der Liste der Apps in Power BI aus.

    ![Screenshot mit der Azure Cognitive Search-App zum Auswählen aus der Liste der Apps.](media/search-monitor-logs-powerbi/azure-search-app-tile.png)

1. Wählen Sie **Verbinden** aus, um die Verbindung mit Ihren Daten herzustellen.

    ![Screenshot, der zeigt, wie Sie eine Verbindung mit Ihren Daten in der Azure Cognitive Search-App herstellen.](media/search-monitor-logs-powerbi/get-started-with-your-new-app.png)

1. Geben Sie den Namen des Speicherkontos ein, das die Datei enthält. Standardmäßig betrachtet die App die Daten der letzten 10 Tage, aber dieser Wert kann mit dem Parameter **Tage** geändert werden.

    ![Screenshot, der zeigt, wie der Speicherkontoname und die Anzahl der Abfragetage auf der Seite „Herstellen einer Verbindung mit Azure Cognitive Search“ eingegeben wird.](media/search-monitor-logs-powerbi/connect-to-storage-account.png)

1. Wählen Sie **Schlüssel** als Authentifizierungsmethode aus, und geben Sie Ihren Speicherkontoschlüssel an. Wählen Sie **Privat** als Datenschutzebene aus. Klicken Sie auf „Anmelden“, und beginnen Sie den Ladevorgang.

    ![Screenshot, der zeigt, wie die Authentifizierungsmethode, der Kontoschlüssel und die Datenschutzebene auf der Seite „Herstellen einer Verbindung mit Azure Cognitive Search“ eingegeben werden.](media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png)

1. Warten Sie, bis die Daten aktualisiert sind. Dies kann eine Weile dauern, je nachdem, wie viele Daten Sie nutzen. Der folgende Indikator zeigt Ihnen, ob die Daten noch aktualisiert werden.

    ![Screenshot, der zeigt, wie die Informationen auf der Seite „Datenaktualisierung“ gelesen werden.](media/search-monitor-logs-powerbi/workspace-view-refreshing.png)

1. Nachdem die Datenaktualisierung abgeschlossen ist, wählen Sie **Azure Cognitive Search-Bericht** aus, um den Bericht anzuzeigen.

    ![Screenshot, der zeigt, wie der Azure Cognitive Search-Bericht auf der Seite „Datenaktualisierung“ ausgewählt wird.](media/search-monitor-logs-powerbi/workspace-view-select-report.png)

1. Aktualisieren Sie die Seite nach dem Öffnen des Berichts unbedingt, damit sie mit Ihren Daten geöffnet wird.

    ![Screenshot des Azure Cognitive Search Power BI-Berichts.](media/search-monitor-logs-powerbi/powerbi-search.png)

## <a name="how-to-change-the-app-parameters"></a>So ändern Sie die App-Parameter
Wenn Sie Daten aus einem anderen Speicherkonto visualisieren oder die Anzahl der Tage, deren Daten abgefragt werden sollen, ändern möchten, führen Sie die folgenden Schritte aus, um die Parameter **Tage** und **StorageAccount** zu ändern.

1. Navigieren Sie zu ihren Power BI-Apps, suchen Sie Ihre Azure Cognitive Search-App, und wählen Sie die Schaltfläche **App bearbeiten** aus, um den Arbeitsbereich anzuzeigen.

    ![Screenshot, der zeigt, wie die Schaltfläche „App bearbeiten“ für die Azure Cognitive Search-App ausgewählt wird.](media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png)

1. Wählen Sie aus den Datasetoptionen **Einstellungen** aus.

    ![Screenshot, der zeigt, wie Sie „Einstellungen“ aus den Azure Congnitive Search-Datasetoptionen auswählen.](media/search-monitor-logs-powerbi/workspace-view-select-settings.png)

1. Ändern Sie auf der Registerkarte „Datasets“ die Parameterwerte, und wählen Sie **Anwenden** aus. Wenn ein Problem mit der Verbindung vorliegt, aktualisieren Sie die Anmeldeinformationen für die Datenquelle auf derselben Seite.

1. Navigieren Sie zurück zum Arbeitsbereich, und wählen Sie **Jetzt aktualisieren** aus den Datasetoptionen aus.

    ![Screenshot, der zeigt, wie Sie „Jetzt aktualisieren“ aus den Azure Congnitive Search-Datasetoptionen auswählen.](media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png)

1. Öffnen Sie den Bericht, um die aktualisierten Daten anzuzeigen. Möglicherweise müssen Sie den Bericht auch aktualisieren, um die neuesten Daten anzuzeigen.

## <a name="troubleshooting"></a>Problembehandlung
Wenn Sie feststellen, dass Ihre Daten nicht angezeigt werden, befolgen Sie diese Schritte zur Problembehandlung:

1. Öffnen Sie den Bericht, und aktualisieren Sie die Seite, um sicherzustellen, dass Sie die neuesten Daten anzeigen. Der Bericht enthält eine Option zum Aktualisieren der Daten. Wählen Sie diese Option aus, um die neuesten Daten zu erhalten.

1. Stellen Sie sicher, dass der angegebene Speicherkontoname und der angegebene Zugriffsschlüssel korrekt sind. Der Speicherkontoname sollte dem Konto entsprechen, das mit ihren Suchdienstprotokollen konfiguriert wurde.

1. Vergewissern Sie sich, dass Ihr Speicherkonto die Container **insights-logs-operationlogs** und **insights-metrics-pt1m** und jeder Container Daten enthält. Die Protokolle und Metriken befinden sich in einigen Ordnerebenen.

1. Überprüfen Sie, ob das Dataset weiterhin aktualisiert wird. Der Aktualisierungsstatusindikator wird oben in Schritt 8 angezeigt. Wenn die Aktualisierung noch immer ausgeführt wird, warten Sie, bis die Aktualisierung beendet ist, um den Bericht zu öffnen und zu aktualisieren.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu Azure Cognitive Search](https://docs.microsoft.com/azure/search/)

[Was ist Power BI?](https://docs.microsoft.com/power-bi/fundamentals/power-bi-overview)

[Grundlegende Konzepte für Designer im Power BI-Dienst](https://docs.microsoft.com/power-bi/service-basic-concepts)