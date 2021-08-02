---
title: Versionsanmerkungen für Application Insights | Microsoft Docs
description: Hier erfahren Sie, wie Sie Anmerkungen zum Nachverfolgen der Bereitstellung oder anderer wichtiger Ereignisse mit Application Insights erstellen.
ms.topic: conceptual
ms.date: 05/27/2021
ms.openlocfilehash: cfd1c9b28a79d68983e49ef5d6dfd70dd357ab47
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112071027"
---
# <a name="release-annotations-for-application-insights"></a>Versionsanmerkungen für Application Insights

Mit Anmerkungen wird angegeben, wo Sie einen neuen Build bereitgestellt haben, oder es wird auf andere wichtige Ereignisse hingewiesen. Dank der Anmerkungen sehen Sie auf einen Blick, ob Ihre Änderungen Auswirkungen auf die Leistung Ihrer Anwendung hatten. Sie können automatisch durch das Buildsystem von [Azure Pipelines](/azure/devops/pipelines/tasks/) erstellt werden. Es können auch Anmerkungen erstellt werden, die alle gewünschten Ereignisse markieren, indem diese aus PowerShell erstellt werden.

## <a name="release-annotations-with-azure-pipelines-build"></a>Releaseanmerkungen mit Azure Pipelines-Build

Releaseanmerkungen sind ein Feature des cloudbasierten Azure Pipelines-Diensts von Azure DevOps.

Wenn alle folgenden Kriterien erfüllt sind, erstellt die Bereitstellungsaufgabe automatisch die Releaseanmerkung:

- Die Ressource, für die die Bereitstellung erfolgt, ist mit Application Insights (über die App-Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY`) verknüpft.
- Die Application Insights-Ressource befindet sich im gleichen Abonnement wie die Ressource, in der die Bereitstellung erfolgt.
- Sie verwenden eine der folgenden Azure DevOps-Pipelineaufgaben:

    | Aufgabencode                 | Aufgabenname                     | Versionen     |
    |---------------------------|-------------------------------|--------------|
    | AzureAppServiceSettings   | Azure App Service-Einstellungen    | Beliebig          |
    | AzureRmWebAppDeployment   | Azure App Service-Bereitstellung      | V3 und höher |
    | AzureFunctionApp          | Azure-Funktionen               | Beliebig          |
    | AzureFunctionAppContainer | Azure Functions für Container | Beliebig          |
    | AzureWebAppContainer      | Azure-Web-App für Container  | Beliebig          |
    | AzureWebApp               | Azure-Web-App                 | Beliebig          |

> [!NOTE]
> Wenn Sie weiterhin die Bereitstellungsaufgabe für Application Insights-Anmerkungen verwenden, sollten Sie sie löschen.

### <a name="configure-release-annotations"></a>Konfigurieren von Versionsanmerkungen

Wenn Sie eine der Bereitstellungsaufgaben im vorherigen Abschnitt nicht verwenden können, müssen Sie in Ihrer Bereitstellungspipeline eine Inlineskriptaufgabe hinzufügen.

1. Navigieren Sie zu einer neuen oder vorhandenen Pipeline, und wählen Sie eine Aufgabe aus.
    :::image type="content" source="./media/annotations/task.png" alt-text="Screenshot: Ausgewählte Aufgabe unter „Phasen“" lightbox="./media/annotations/task.png":::
1. Fügen Sie eine neue Aufgabe hinzu, und wählen Sie **Azure CLI** aus.
    :::image type="content" source="./media/annotations/add-azure-cli.png" alt-text="Screenshot: Hinzufügen einer neuen Aufgabe und Auswählen der Azure CLI" lightbox="./media/annotations/add-azure-cli.png":::
1. Geben Sie das entsprechende Azure-Abonnement an.  Ändern Sie **Skripttyp** in *PowerShell* und **Skriptspeicherort** in *Inline*.
1. Fügen Sie das [PowerShell-Skript aus Schritt 2 im nächsten Abschnitt](#create-release-annotations-with-azure-cli) zu **Inlineskript** hinzu.
1. Fügen Sie unter **Skriptargumente** die folgenden Argumente hinzu, und ersetzen Sie die Platzhalter in eckigen Klammern durch Ihre eigenen Werte. Angaben für „-releaseProperties“ sind optional.

    ```powershell
        -aiResourceId "<aiResourceId>" `
        -releaseName "<releaseName>" `
        -releaseProperties @{"ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
    ```

    :::image type="content" source="./media/annotations/inline-script.png" alt-text="Screenshot: Azure CLI-Aufgabeneinstellungen mit hervorgehobenen Optionen für Skripttyp, Skriptspeicherort, Inlineskript und Skriptargumente" lightbox="./media/annotations/inline-script.png":::

1. Speichern Sie.

## <a name="create-release-annotations-with-azure-cli"></a>Erstellen von Releaseanmerkungen mit der Azure CLI

Mit dem PowerShell-Skript „CreateReleaseAnnotation“ können Sie Anmerkungen auf der Grundlage eines beliebigen Prozesses erstellen, ohne Azure DevOps zu verwenden.

1. Melden Sie sich bei der [Azure CLI](/cli/azure/authenticate-azure-cli) an.

2. Erstellen Sie eine lokale Kopie des folgenden Skripts, und nennen Sie es „CreateReleaseAnnotation.ps1“.

    ```powershell
    param(
        [parameter(Mandatory = $true)][string]$aiResourceId,
        [parameter(Mandatory = $true)][string]$releaseName,
        [parameter(Mandatory = $false)]$releaseProperties = @()
    )
    
    $annotation = @{
        Id = [GUID]::NewGuid();
        AnnotationName = $releaseName;
        EventTime = (Get-Date).ToUniversalTime().GetDateTimeFormats("s")[0];
        Category = "Deployment";
        Properties = ConvertTo-Json $releaseProperties -Compress
    }
    
    $body = (ConvertTo-Json $annotation -Compress) -replace '(\\+)"', '$1$1"' -replace "`"", "`"`""

    az rest --method put --uri "$($aiResourceId)/Annotations?api-version=2015-05-01" --body "$($body) "
    ```

3. Rufen Sie das PowerShell-Skript mithilfe des folgenden Codes auf, und ersetzen Sie dabei die in spitzen Klammern angegebenen Platzhalter durch Ihre eigenen Werte. Angaben für „-releaseProperties“ sind optional.

    ```powershell
         .\CreateReleaseAnnotation.ps1 `
          -aiResourceId "<aiResourceId>" `
          -releaseName "<releaseName>" `
          -releaseProperties @{"ReleaseDescription"="<a description>";
              "TriggerBy"="<Your name>" }
    ```

|Argument | Definition | Hinweis|
|--------------|-----------------------|--------------------|
|aiResourceId | Die Ressourcen-ID der Application Insights-Zielressource | Beispiel:<br> /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyRGName/providers/microsoft.insights/components/MyResourceName|
|releaseName | Der Name, den die erstellte Releaseanmerkung erhalten soll | | 
|releaseProperties | Wird zum Anfügen benutzerdefinierter Metadaten an die Anmerkung verwendet. | Optional|

## <a name="view-annotations"></a>Anmerkungen anzeigen

> [!NOTE]
> Versionsanmerkungen sind im Bereich „Metriken“ von Application Insights derzeit nicht verfügbar.

Wenn Sie nun diese Versionsvorlage zum Bereitstellen einer neuen Version verwenden, wird jedes Mal eine Anmerkung an Application Insights gesendet. Die Anmerkungen können an den folgenden Orten angezeigt werden:

- Leistung

    :::image type="content" source="./media/annotations/performance.png" alt-text="Screenshot: Registerkarte „Leistung“ mit ausgewählter Releaseanmerkung (blauer Pfeil), um die Registerkarte „Releaseanmerkung“ anzuzeigen" lightbox="./media/annotations/performance.png":::

- Fehler

    :::image type="content" source="./media/annotations/failures.png" alt-text="Screenshot: Registerkarte „Fehler“ mit ausgewählter Releaseanmerkung (blauer Pfeil), um die Registerkarte „Releaseanmerkung“ anzuzeigen" lightbox="./media/annotations/failures.png":::
- Verwendung

    :::image type="content" source="./media/annotations/usage-pane.png" alt-text="Screenshot: Registerkartenleiste „Benutzer“ mit ausgewählten Releaseanmerkungen. Releaseanmerkungen werden als blaue Pfeile über dem Diagramm angezeigt und geben den Zeitpunkt an, zu dem ein Release erfolgt ist." lightbox="./media/annotations/usage-pane.png":::

- Arbeitsmappen

    Alle protokollbasierten Arbeitsmappenabfragen, bei denen bei der Visualisierung die Uhrzeit auf der X-Achse angezeigt wird.
    
    :::image type="content" source="./media/annotations/workbooks-annotations.png" alt-text="Screenshot: Arbeitsmappenbereich mit protokollbasierter Zeitreihenabfrage mit Anzeige von Anmerkungen" lightbox="./media/annotations/workbooks-annotations.png":::
    
    Navigieren Sie zum Aktivieren der Anmerkungen in Ihrer Arbeitsmappe zu **Erweiterte Einstellungen**, und wählen Sie **Anmerkungen anzeigen** aus.
    
    :::image type="content" source="./media/annotations/workbook-show-annotations.png" alt-text="Screenshot: Menü „Erweiterte Einstellungen“ mit hervorgehobenem Kontrollkästchen „Anmerkungen anzeigen“":::

Wählen Sie einen Anmerkungsmarker aus, um Details zur Version anzuzeigen, z. B. Anforderer, Quellcodeverwaltungsbranch, Releasepipeline und Umgebung.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Arbeitselements](./diagnostic-search.md#create-work-item)
* [Automation mit PowerShell](./powershell.md)
