---
title: Versionsanmerkungen für Application Insights | Microsoft Docs
description: Fügen Sie den Diagrammen im Metrik-Explorer in Application Insights Bereitstellungs- oder Buildmarker hinzu.
ms.topic: conceptual
ms.date: 08/14/2020
ms.openlocfilehash: 9132e65e4705fd9125d97a5e095fe5f0850229a2
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107011049"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anmerkungen zu Metrik-Diagrammen in Application Insights

Mit Anmerkungen wird angegeben, wo Sie einen neuen Build bereitgestellt haben, oder es wird auf andere wichtige Ereignisse hingewiesen. Dank der Anmerkungen sehen Sie auf einen Blick, ob Ihre Änderungen Auswirkungen auf die Leistung Ihrer Anwendung hatten. Sie können automatisch durch das Buildsystem von [Azure Pipelines](/azure/devops/pipelines/tasks/) erstellt werden. Es können auch Anmerkungen erstellt werden, die alle gewünschten Ereignisse markieren, indem diese aus PowerShell erstellt werden.

## <a name="release-annotations-with-azure-pipelines-build"></a>Releaseanmerkungen mit Azure Pipelines-Build

Releaseanmerkungen sind ein Feature des cloudbasierten Azure Pipelines-Diensts von Azure DevOps.

### <a name="install-the-annotations-extension-one-time"></a>Installieren der Erweiterung für Anmerkungen (einmalig)

Um Releaseanmerkungen erstellen zu können, müssen Sie eine der zahlreichen Azure DevOps-Erweiterungen installieren, die im Visual Studio Marketplace zur Verfügung stehen.

1. Melden Sie sich bei Ihrem [Azure DevOps](https://azure.microsoft.com/services/devops/)-Projekt an.
   
1. Wählen Sie im Visual Studio Marketplace auf der Seite [Release Annotations for Azure Application Insights](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) (Releaseanmerkungen für Azure Application Insights) Ihre Azure DevOps-Organisation und anschließend **Install** (Installieren) aus, um die Erweiterung Ihrer Azure DevOps-Organisation hinzuzufügen.
   
   ![Wählen Sie eine Azure DevOps-Organisation und anschließend „Install“ (Installieren) aus.](./media/annotations/1-install.png)
   
Die Erweiterung muss nur einmal für Ihre Azure DevOps-Organisation installiert werden. Nun können Sie Releaseanmerkungen für ein beliebiges Projekt in Ihrer Organisation konfigurieren.

### <a name="configure-release-annotations"></a>Konfigurieren von Versionsanmerkungen

Erstellen Sie für Ihre Azure Pipelines-Versionsvorlagen jeweils einen separaten API-Schlüssel.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie die Application Insights-Ressource, die Ihre Anwendung überwacht. Sollten Sie über keine solche Ressource verfügen, [erstellen Sie eine neue Application Insights-Ressource](./app-insights-overview.md).
   
1. Öffnen Sie die Registerkarte **API-Zugriff**, und kopieren Sie die **Application Insights-ID**.
   
   ![Kopieren Sie die Anwendungs-ID unter „API-Zugriff“.](./media/annotations/2-app-id.png)

1. Öffnen oder erstellen Sie in einem separaten Browserfenster die Versionsvorlage, mit der Ihre Azure Pipelines-Bereitstellungen verwaltet werden.
   
1. Wählen Sie **Aufgabe hinzufügen** und anschließend im Menü die Aufgabe **Application Insights Release Annotation** (Application Insights-Releaseanmerkung) aus.
   
   ![Wählen Sie „Aufgabe hinzufügen“ und anschließend „Application Insights Release Annotation“ (Application Insights-Releaseanmerkung) aus.](./media/annotations/3-add-task.png)

   > [!NOTE]
   > Die Aufgabe „Releaseanmerkung“ unterstützt zurzeit nur Windows-basierte Agents. Sie kann nicht unter Linux, macOS oder anderen Agent-Typen ausgeführt werden.
   
1. Fügen Sie unter **Anwendungs-ID** die Application Insights-ID ein, die Sie auf der Registerkarte **API-Zugriff** kopiert haben.
   
   ![Application Insights-ID einfügen](./media/annotations/4-paste-app-id.png)
   
1. Wählen Sie im Application Insights-Fenster **API-Zugriff** die Option **API-Schlüssel erstellen** aus. 
   
   ![Wählen Sie auf der Registerkarte „API-Zugriff“ die Option „API-Schlüssel erstellen“ aus.](./media/annotations/5-create-api-key.png)
   
1. Geben Sie im Fenster **API-Schlüssel erstellen** eine Beschreibung ein, wählen Sie **Anmerkungen schreiben** aus, und wählen Sie anschließend **Schlüssel generieren** aus. Kopieren Sie den neuen Schlüssel.
   
   ![Geben Sie im Fenster „API-Schlüssel erstellen“ eine Beschreibung ein, wählen Sie „Anmerkungen schreiben“ aus, und wählen Sie anschließend „Schlüssel generieren“ aus.](./media/annotations/6-create-api-key.png)
   
1. Wählen Sie im Fenster für die Versionsvorlage auf der Registerkarte **Variablen** die Option **Hinzufügen** aus, um eine Variablendefinition für den neuen API-Schlüssel zu erstellen.

1. Geben Sie unter **Name** den Namen `ApiKey` ein, und fügen Sie unter **Wert** den API-Schlüssel ein, den Sie auf der Registerkarte **API-Zugriff** kopiert haben.
   
   ![Wählen Sie auf der Azure DevOps-Registerkarte „Variablen“ die Option „Hinzufügen“ aus, nennen Sie die Variable „ApiKey“, und fügen Sie unter „Wert“ den API-Schlüssel ein.](./media/annotations/7-paste-api-key.png)
   
1. Wählen Sie im Hauptfenster der Versionsvorlage die Option **Speichern** aus, um die Vorlage zu speichern.


   > [!NOTE]
   > Einschränkungen für API-Schlüssel werden in der [Dokumentation zur REST-API-Ratenbegrenzungen](https://dev.applicationinsights.io/documentation/Authorization/Rate-limits) beschrieben.

## <a name="view-annotations"></a>Anmerkungen anzeigen


   > [!NOTE]
   > Versionsanmerkungen sind im Bereich „Metriken“ von Application Insights derzeit nicht verfügbar.

Wenn Sie nun diese Versionsvorlage zum Bereitstellen einer neuen Version verwenden, wird jedes Mal eine Anmerkung an Application Insights gesendet. Die Anmerkungen können an den folgenden Orten angezeigt werden:

Bereich **Nutzung**, in dem Sie auch manuell Versionsanmerkungen erstellen können:

![Screenshot: Balkendiagramm mit der Anzahl von Benutzerbesuchen für eine bestimmte Anzahl von Stunden Versionsanmerkungen werden als grüne Häkchen oberhalb des Diagramms angezeigt und geben den Zeitpunkt einer Versionsveröffentlichung an.](./media/annotations/usage-pane.png)

Alle protokollbasierten Arbeitsmappenabfragen, bei denen bei der Visualisierung die Uhrzeit auf der X-Achse angezeigt wird.

![Screenshot: Arbeitsmappenbereich mit protokollbasierter Zeitreihenabfrage mit Anzeige von Anmerkungen](./media/annotations/workbooks-annotations.png)

Navigieren Sie zum Aktivieren der Anmerkungen in Ihrer Arbeitsmappe zu **Erweiterte Einstellungen**, und wählen Sie **Anmerkungen anzeigen** aus.

![Screenshot: Menü „Erweiterte Einstellungen“ mit Hervorhebung von „Anmerkungen anzeigen“ und einem Häkchen zum Aktivieren neben der Einstellung](./media/annotations/workbook-show-annotations.png)

Wählen Sie einen Anmerkungsmarker aus, um Details zur Version anzuzeigen, z. B. Anforderer, Quellcodeverwaltungsbranch, Releasepipeline und Umgebung.

## <a name="create-custom-annotations-from-powershell"></a>Erstellen von benutzerdefinierten Anmerkungen in PowerShell
Mit dem PowerShell-Skript „CreateReleaseAnnotation“ können Sie Anmerkungen auf der Grundlage eines beliebigen Prozesses erstellen, ohne Azure DevOps zu verwenden.

> [!IMPORTANT]
> Wenn Sie PowerShell 7.1 verwenden, fügen Sie `-SkipHttpErrorCheck` am Ende von Zeile 26 hinzu. Beispiel: `$request = Invoke-WebRequest -Uri $fwLink -MaximumRedirection 0 -UseBasicParsing -ErrorAction Ignore -SkipHttpErrorCheck`.

1. Erstellen Sie eine lokale Kopie von CreateReleaseAnnotation.ps1:

    ```powershell
    
    # Copyright (c) Microsoft Corporation. All rights reserved. 
    # Licensed under the MIT License. See License.txt in the project root for license information. 
    
    # Sample usage .\CreateReleaseAnnotation.ps1 -applicationId "<appId>" -apiKey "<apiKey>" -releaseFilePath "<path to .exe with file version>" -releaseProperties @{"ReleaseDescription"="Release with annotation";"TriggerBy"="John Doe"}
    param(
        [parameter(Mandatory = $true)][string]$applicationId,
        [parameter(Mandatory = $true)][string]$apiKey,
        [parameter(Mandatory = $true)][string]$releaseFilePath,
        [parameter(Mandatory = $false)]$releaseProperties
    )
    
    $releaseName = (Get-Item $releaseFilePath).VersionInfo.FileVersion
    Write-Host "Creating release annotation $releaseName in ApplicationInsights" -ForegroundColor Cyan
    
    # background info on how fwlink works: After you submit a web request, many sites redirect through a series of intermediate pages before you finally land on the destination page.
    # So when calling Invoke-WebRequest, the result it returns comes from the final page in any redirect sequence. Hence, I set MaximumRedirection to 0, as this prevents the call to 
    # be redirected. By doing this, we get a response with status code 302, which indicates that there is a redirection link from the response body. We grab this redirection link and 
    # construct the url to make a release annotation.
    # Here's how this logic is going to works
    # 1. Client send http request, such as:  http://go.microsoft.com/fwlink/?LinkId=625115
    # 2. FWLink get the request and find out the destination URL for it, such as:  http://www.bing.com
    # 3. FWLink generate a new http response with status code “302” and with destination URL “http://www.bing.com”. Send it back to Client.
    # 4. Client, such as a powershell script, knows that status code “302” means redirection to new a location, and the target location is “http://www.bing.com”
    function GetRequestUrlFromFwLink($fwLink)
    {
        $request = Invoke-WebRequest -Uri $fwLink -MaximumRedirection 0 -UseBasicParsing -ErrorAction Ignore
        if ($request.StatusCode -eq "302") {
            return $request.Headers.Location
        }
        
        return $null
    }
    
    function CreateAnnotation($grpEnv)
    {
        $retries = 1
        $success = $false
        while (!$success -and $retries -lt 6) {
            $location = "$grpEnv/applications/$applicationId/Annotations?api-version=2015-11"
                
            Write-Host "Invoke a web request for $location to create a new release annotation. Attempting $retries"
            set-variable -Name createResultStatus -Force -Scope Local -Value $null
            set-variable -Name createResultStatusDescription -Force -Scope Local -Value $null
            set-variable -Name result -Force -Scope Local
    
            try {
                $result = Invoke-WebRequest -Uri $location -Method Put -Body $bodyJson -Headers $headers -ContentType "application/json; charset=utf-8" -UseBasicParsing
            } catch {
                if ($_.Exception){
                    if($_.Exception.Response) {
                        $createResultStatus = $_.Exception.Response.StatusCode.value__
                        $createResultStatusDescription = $_.Exception.Response.StatusDescription
                    }
                    else {
                        $createResultStatus = "Exception"
                        $createResultStatusDescription = $_.Exception.Message
                    }
                }
            }
    
            if ($result -eq $null) {
                if ($createResultStatus -eq $null) {
                    $createResultStatus = "Unknown"
                }
                if ($createResultStatusDescription -eq $null) {
                    $createResultStatusDescription = "Unknown"
                }
            }
            else {
                    $success = $true                     
            }
    
            if ($createResultStatus -eq 409 -or $createResultStatus -eq 404 -or $createResultStatus -eq 401) # no retry when conflict or unauthorized or not found
            {
                break
            }
    
            $retries = $retries + 1
            sleep 1
        }
    
        $createResultStatus
        $createResultStatusDescription
        return
    }
    
    # Need powershell version 3 or greater for script to run
    $minimumPowershellMajorVersion = 3
    if ($PSVersionTable.PSVersion.Major -lt $minimumPowershellMajorVersion) {
       Write-Host "Need powershell version $minimumPowershellMajorVersion or greater to create release annotation"
       return
    }
    
    $currentTime = (Get-Date).ToUniversalTime()
    $annotationDate = $currentTime.ToString("MMddyyyy_HHmmss")
    set-variable -Name requestBody -Force -Scope Script
    $requestBody = @{}
    $requestBody.Id = [GUID]::NewGuid()
    $requestBody.AnnotationName = $releaseName
    $requestBody.EventTime = $currentTime.GetDateTimeFormats("s")[0] # GetDateTimeFormats returns an array
    $requestBody.Category = "Deployment"
    
    if ($releaseProperties -eq $null) {
        $properties = @{}
    } else {
        $properties = $releaseProperties    
    }
    $properties.Add("ReleaseName", $releaseName)
    
    $requestBody.Properties = ConvertTo-Json($properties) -Compress
    
    $bodyJson = [System.Text.Encoding]::UTF8.GetBytes(($requestBody | ConvertTo-Json))
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("X-AIAPIKEY", $apiKey)
    
    set-variable -Name createAnnotationResult1 -Force -Scope Local -Value $null
    set-variable -Name createAnnotationResultDescription -Force -Scope Local -Value ""
    
    # get redirect link from fwlink
    $requestUrl = GetRequestUrlFromFwLink("http://go.microsoft.com/fwlink/?prd=11901&pver=1.0&sbp=Application%20Insights&plcid=0x409&clcid=0x409&ar=Annotations&sar=Create%20Annotation")
    if ($requestUrl -eq $null) {
        $output = "Failed to find the redirect link to create a release annotation"
        throw $output
    }
    
    $createAnnotationResult1, $createAnnotationResultDescription = CreateAnnotation($requestUrl)
    if ($createAnnotationResult1) 
    {
         $output = "Failed to create an annotation with Id: {0}. Error {1}, Description: {2}." -f $requestBody.Id, $createAnnotationResult1, $createAnnotationResultDescription
         throw $output
    }
    
    $str = "Release annotation created. Id: {0}." -f $requestBody.Id
    Write-Host $str -ForegroundColor Green
    
    ```
   
1. Gehen Sie wie oben beschrieben vor, um Ihre Application Insights-ID abzurufen und einen API-Schlüssel über die Application Insights-Registerkarte **API-Zugriff** zu erstellen.
   
1. Rufen Sie das PowerShell-Skript mithilfe des folgenden Codes auf, und ersetzen Sie dabei die in spitzen Klammern angegebenen Platzhalter durch Ihre eigenen Werte. Die Releaseeigenschaften (`-releaseProperties`) sind optional. 
   
   ```powershell
   
        .\CreateReleaseAnnotation.ps1 `
         -applicationId "<applicationId>" `
         -apiKey "<apiKey>" `
         -releaseName "<releaseName>" `
         -releaseProperties @{
             "ReleaseDescription"="<a description>";
             "TriggerBy"="<Your name>" }
   ```

Sie können das Skript auch anpassen, um beispielsweise Anmerkungen für die Vergangenheit zu erstellen.


## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Arbeitselements](./diagnostic-search.md#create-work-item)
* [Automation mit PowerShell](./powershell.md)
