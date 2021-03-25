---
title: 'Azure Application Insights-Agent: ausführliche Anweisungen | Microsoft-Dokumentation'
description: Ausführliche Anweisungen für die ersten Schritte mit dem Application Insights-Agent Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 29922f088a51e4876e5e2ec8fe87c3bbce4482f3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102521678"
---
# <a name="application-insights-agent-formerly-named-status-monitor-v2-detailed-instructions"></a>Application Insights-Agent (ehemals „Statusmonitor v2“): Ausführliche Anleitungen

In diesem Artikel wird beschrieben, wie der PowerShell-Katalog integriert und das ApplicationMonitor-Modul heruntergeladen wird.
Der Artikel enthält die gängigsten Parameter, die für den Einstieg erforderlich sind.
Außerdem finden Sie hier eine Anleitung zum manuellen Herunterladen, falls Sie über keinen Internetzugriff verfügen.

## <a name="get-an-instrumentation-key"></a>Abrufen eines Instrumentierungsschlüssels

Für den Einstieg benötigen Sie einen Instrumentierungsschlüssel. Weitere Informationen finden Sie unter [Erstellen einer Application Insights-Ressource](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>Ausführen von PowerShell als Administrator mit einer Ausführungsrichtlinie mit erhöhten Rechten

### <a name="run-as-admin"></a>Ausführen als Administrator

PowerShell benötigt Berechtigungen auf Administratorebene, um Änderungen an Ihrem Computer vornehmen zu können.
### <a name="execution-policy"></a>Ausführungsrichtlinie
- Beschreibung: Die Ausführung von PowerShell-Skripts ist standardmäßig deaktiviert. Es wird empfohlen, RemoteSigned-Skripts nur für den aktuellen Bereich zuzulassen.
- Referenz: [Informationen zu Ausführungsrichtlinien](/powershell/module/microsoft.powershell.core/about/about_execution_policies) und [Set-ExecutionPolicy](/powershell/module/microsoft.powershell.security/set-executionpolicy).
- Befehl: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- Optionaler Parameter:
    - `-Force`. Umgeht die Bestätigungsaufforderung.

**Fehlerbeispiele**

```output
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.

Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Voraussetzungen für PowerShell

Überwachen Sie Ihre Instanz von PowerShell durch Ausführen des Befehls `$PSVersionTable`.
Dieser Befehl erstellt die folgende Ausgabe:

```output
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Diese Anleitungen wurden auf einem Computer unter Windows 10 mit den oben genannten Versionen geschrieben und getestet.

## <a name="prerequisites-for-powershell-gallery"></a>Voraussetzungen für den PowerShell-Katalog

Diese Schritte bereiten Ihren Server darauf vor, Module aus dem PowerShell-Katalog herunterzuladen.

> [!NOTE] 
> Der PowerShell-Katalog wird unter Windows 10, Windows Server 2016 und PowerShell 6+ unterstützt.
> Informationen zu früheren Versionen finden Sie unter [Installieren von PowerShellGet](/powershell/scripting/gallery/installing-psget).


1. Führen Sie PowerShell als Administrator mit einer Ausführungsrichtlinie mit erhöhten Rechten aus.
2. Installieren Sie den NuGet-Paketanbieter.
    - Beschreibung: Sie benötigen diesen Anbieter für die Interaktion mit NuGet-basierten Repositorys wie dem PowerShell-Katalog.
    - Referenz: [Install-PackageProvider](/powershell/module/packagemanagement/install-packageprovider).
    - Befehl: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - Optionale Parameter:
        - `-Proxy`. Gibt einen Proxyserver für die Anforderung an.
        - `-Force`. Umgeht die Bestätigungsaufforderung.
    
    Sie erhalten diese Eingabeaufforderung, wenn NuGet nicht eingerichtet ist:

    ```output
    NuGet provider is required to continue
    PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. 
    The NuGet provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
    'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
    'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
    the NuGet provider now?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```    

3. Konfigurieren Sie den PowerShell-Katalog als vertrauenswürdiges Repository.
    - Beschreibung: Standardmäßig ist der PowerShell-Katalog ein nicht vertrauenswürdiges Repository.
    - Referenz: [Set-PSRepository](/powershell/module/powershellget/set-psrepository).
    - Befehl: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - Optionaler Parameter:
        - `-Proxy`. Gibt einen Proxyserver für die Anforderung an.

    Sie erhalten diese Eingabeaufforderung, wenn der PowerShell-Katalog nicht vertrauenswürdig ist:

    ```output
    Untrusted repository
    You are installing the modules from an untrusted repository. 
    If you trust this repository, change its InstallationPolicy value 
    by running the Set-PSRepository cmdlet. Are you sure you want to 
    install the modules from 'PSGallery'?
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
    ```

    Sie können diese Änderung bestätigen und alle PSRepositories überprüfen, indem Sie den Befehl `Get-PSRepository` ausführen.

4. Installieren Sie die neueste Version von PowerShellGet.
    - Beschreibung: Dieses Modul enthält die Tools, mit denen andere Module aus dem PowerShell-Katalog abgerufen werden können. Version 1.0.0.1 wird mit Windows 10 und Windows Server ausgeliefert. Erforderlich ist Version 1.6.0 oder höher. Um zu ermitteln, welche Version installiert ist, führen Sie den Befehl `Get-Command -Module PowerShellGet` aus.
    - Referenz: [Installieren von PowerShellGet](/powershell/scripting/gallery/installing-psget).
    - Befehl: `Install-Module -Name PowerShellGet`.
    - Optionale Parameter:
        - `-Proxy`. Gibt einen Proxyserver für die Anforderung an.
        - `-Force`. Umgeht die Warnung „bereits installiert“ und installiert die neueste Version.

    Dieser Fehler wird angezeigt, wenn Sie nicht die neueste Version von PowerShellGet verwenden:

    ```output
    Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
    At line:1 char:20
    Install-Module abc -AllowPrerelease
                   ~~~~~~~~~~~~~~~~
    CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
    FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    ```

5. Starten Sie PowerShell neu. Sie können die neue Version nicht in der aktuellen Sitzung laden. Neue PowerShell-Sitzungen laden die neueste Version von PowerShellGet.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>Herunterladen und Installieren des Moduls über den PowerShell-Katalog

Mit diesen Schritten wird das Az.ApplicationMonitor-Modul aus dem PowerShell-Katalog heruntergeladen.

1. Sorgen Sie dafür, dass alle Voraussetzungen für den PowerShell-Katalog erfüllt sind.
2. Führen Sie PowerShell als Administrator mit einer Ausführungsrichtlinie mit erhöhten Rechten aus.
3. Installieren Sie das Az.ApplicationMonitor-Modul.
    - Referenz: [Install-Module](/powershell/module/powershellget/install-module).
    - Befehl: `Install-Module -Name Az.ApplicationMonitor`.
    - Optionale Parameter:
        - `-Proxy`. Gibt einen Proxyserver für die Anforderung an.
        - `-AllowPrerelease`. Ermöglicht die Installation von Alpha- und Betaversionen.
        - `-AcceptLicense`. Umgeht die Eingabeaufforderung „Lizenz akzeptieren“.
        - `-Force`. Umgeht die Warnung „Nicht vertrauenswürdiges Repository“.

## <a name="download-and-install-the-module-manually-offline-option"></a>Manuelles Herunterladen und Installieren des Moduls (Offline-Option)

Wenn Sie aus irgendeinem Grund keine Verbindung zum PowerShell-Modul herstellen können, können Sie das Az.ApplicationMonitor-Modul manuell herunterladen und installieren.

### <a name="manually-download-the-latest-nupkg-file"></a>Manuelles Herunterladen der neuesten Nupkg-Datei

1. Gehe zu https://www.powershellgallery.com/packages/Az.ApplicationMonitor.
2. Wählen Sie die neueste Version der Datei in der Tabelle **Versionsverlauf** aus.
3. Wählen Sie unter **Installationsoptionen** die Option **Manueller Download** aus.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>Option 1: Installieren in einem PowerShell-Modulverzeichnis
Installieren Sie das manuell heruntergeladene PowerShell-Modul in einem PowerShell-Verzeichnis, damit es von PowerShell-Sitzungen erkannt werden kann.
Weitere Informationen finden Sie unter [Installieren eines PowerShell-Moduls](/powershell/scripting/developer/module/installing-a-powershell-module).


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Entpacken von Nupkg als ZIP-Datei mithilfe von Expand-Archive (v1.0.1.0)

- Beschreibung: Die Basisversion der Microsoft.PowerShell.Archive (v1.0.1.0) kann keine Nupkg-Dateien extrahieren. Benennen Sie die Datei mit der Erweiterung „.zip“ um.
- Referenz: [Expand-Archive](/powershell/module/microsoft.powershell.archive/expand-archive).
- Befehl:

    ```console
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Entpacken von Nupkg mithilfe von Expand-Archive (v1.1.0.0)

- Beschreibung: Verwenden Sie eine aktuelle Version von Expand-Archive, um Nupkg-Dateien zu entpacken, ohne die Erweiterung zu ändern.
- Referenz: [Expand-Archive](/powershell/module/microsoft.powershell.archive/expand-archive) und [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Befehl:

    ```console
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>Option 2: Manuelles Entpacken und Importieren von Nupkg
Installieren Sie das manuell heruntergeladene PowerShell-Modul in einem PowerShell-Verzeichnis, damit es von PowerShell-Sitzungen erkannt werden kann.
Weitere Informationen finden Sie unter [Installieren eines PowerShell-Moduls](/powershell/scripting/developer/module/installing-a-powershell-module).

Wenn Sie das Modul in einem anderen Verzeichnis installieren, importieren Sie es manuell mithilfe von [Import-Module](/powershell/module/microsoft.powershell.core/import-module).

> [!IMPORTANT] 
> DLLs werden über relative Pfade installiert.
> Speichern Sie den Inhalt des Pakets in Ihrem vorgesehenen Runtime-Verzeichnis, und bestätigen Sie, dass die Zugriffsrechte Lese-, aber keine Schreibvorgänge erlauben.

1. Ändern Sie die Erweiterung auf „.zip“, und extrahieren Sie den Inhalt des Pakets in Ihr gewünschtes Installationsverzeichnis.
2. Suchen Sie den Dateipfad zu „Az.ApplicationMonitor.psd1“.
3. Führen Sie PowerShell als Administrator mit einer Ausführungsrichtlinie mit erhöhten Rechten aus.
4. Laden Sie das Modul mit dem Befehl `Import-Module Az.ApplicationMonitor.psd1`.
    

## <a name="route-traffic-through-a-proxy"></a>Weiterleiten von Datenverkehr über einen Proxy

Wenn Sie einen Computer in Ihrem privaten Intranet überwachen, müssen Sie HTTP-Datenverkehr über einen Proxy weiterleiten.

Die PowerShell-Befehle zum Herunterladen und Installieren von Az.ApplicationMonitor aus dem PowerShell-Katalog unterstützen einen `-Proxy`-Parameter.
Lesen Sie die vorstehenden Anleitungen, wenn Sie Ihre Installationsskripts schreiben.

Das Application Insights SDK muss die Telemetrie Ihrer App an Microsoft senden. Es wird empfohlen, dass Sie die Proxyeinstellungen für Ihre App in der Datei „web.config“ konfigurieren. Weitere Informationen finden Sie unter [Application Insights – Häufig gestellte Fragen: Proxy-Passthrough](../faq.md#proxy-passthrough).


## <a name="enable-monitoring"></a>Aktivieren der Überwachung

Verwenden Sie den Befehl `Enable-ApplicationInsightsMonitoring` zum Aktivieren der Überwachung.

Lesen Sie die [API-Referenz](./status-monitor-v2-api-reference.md#enable-applicationinsightsmonitoring) mit einer ausführlichen Beschreibung zur Verwendung dieses Cmdlets.



## <a name="next-steps"></a>Nächste Schritte

 Anzeigen der Telemetrie:

- [Untersuchen Sie Metriken](../essentials/metrics-charts.md) zum Überwachen der Leistung und Nutzung.
- [Durchsuchen Sie Ereignisse und Protokolle](./diagnostic-search.md), um Probleme zu diagnostizieren.
- Verwenden Sie [Analytics](../logs/log-query-overview.md) für erweiterte Abfragen.
- [Erstellen Sie Dashboards](./overview-dashboard.md).

 Hinzufügen weiterer Telemetrieelemente:

- [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
- [Fügen Sie Webclient-Telemetriedaten hinzu](./javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe zu aktivieren.
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](./asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.

Weitere Verwendungsmöglichkeiten für den Application Insights-Agent:

- Verwenden Sie unsere Anleitung zur [Problembehandlung](status-monitor-v2-troubleshoot.md) für den Application Insights-Agent.

