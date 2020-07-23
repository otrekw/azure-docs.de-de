---
title: API-Referenz für den Azure Application Insights-.NET-Agent
description: API-Referenz für den Application Insights-Agent. Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 32fd0da0095c34c4ef199eb703881e048473f0a4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86499357"
---
# <a name="azure-monitor-application-insights-agent-api-reference"></a>API-Referenz für den Azure Monitor Application Insights-.NET-Agent

In diesem Artikel wird ein Cmdlet des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) beschrieben.

> [!NOTE] 
> - Für den Einstieg benötigen Sie einen Instrumentierungsschlüssel. Weitere Informationen finden Sie unter [Erstellen einer Ressource](create-new-resource.md#copy-the-instrumentation-key).
> - Dieses Cmdlet erfordert, dass Sie unsere Lizenz- und Datenschutzbestimmungen durchlesen und akzeptieren.

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorrechten und einer Ausführungsrichtlinie mit erhöhten Rechten. Weitere Informationen finden Sie unter [Ausführen von PowerShell als Administrator mit einer Ausführungsrichtlinie mit erhöhten Rechten](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy).
> - Dieses Cmdlet erfordert, dass Sie unsere Lizenz- und Datenschutzbestimmungen durchlesen und akzeptieren.
> - Die Instrumentierungs-Engine erfordert zusätzlichen Aufwand und ist standardmäßig deaktiviert.


## <a name="enable-instrumentationengine"></a>Enable-InstrumentationEngine

Aktiviert die Instrumentierungs-Engine durch Festlegen einiger Registrierungsschlüssel.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

Die Instrumentierungs-Engine kann Daten ergänzen, die mit .NET SDKs gesammelt wurden.
Es werden Ereignisse und Nachrichten gesammelt, die die Ausführung eines verwalteten Prozesses beschreiben. Diese Ereignisse und Nachrichten umfassen Abhängigkeitsergebniscodes, HTTP-Verben und [SQL-Befehlstext](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query).

Aktivieren Sie die Instrumentierungs-Engine in den folgenden Fällen:
- Sie haben mithilfe des Cmdlets „Enable“ bereits die Überwachung, aber nicht die Instrumentierungs-Engine aktiviert.
- Sie haben Ihre App mithilfe der .NET SDKs manuell instrumentiert und möchten zusätzliche Telemetriedaten sammeln.

### <a name="examples"></a>Beispiele

```powershell
PS C:\> Enable-InstrumentationEngine
```

### <a name="parameters"></a>Parameter

#### <a name="-acceptlicense"></a>-AcceptLicense
**Optional.** Verwenden Sie diesen Schalter, um die Lizenz- und Datenschutzbestimmungen in monitorlosen Installationen anzunehmen.

#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.

### <a name="output"></a>Output


##### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Beispielausgabe nach erfolgreicher Aktivierung der Instrumentierungs-Engine

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="enable-applicationinsightsmonitoring"></a>Enable-ApplicationInsightsMonitoring

Ermöglicht die Aktivierung der Überwachung von IIS-Apps auf einem Zielcomputer ohne Code.

Dieses Cmdlet ändert die IIS-Datei „applicationHost.config“ und legt einige Registrierungsschlüssel fest.
Es erstellt ferner die Datei „applicationinsights.ikey.config“, die den Instrumentierungsschlüssel definiert, der von jeder App verwendet wird.
IIS lädt beim Start das RedfieldModule. Dieses fügt das Application Insights SDK in Anwendungen ein, während diese Anwendungen gestartet werden.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

Nachdem Sie die Überwachung aktiviert haben, empfiehlt es sich, [Live Metrics Stream](live-stream.md) zu verwenden, um schnell zu überprüfen, ob Ihre App Telemetrie an uns sendet.

### <a name="examples"></a>Beispiele

#### <a name="example-with-a-single-instrumentation-key"></a>Beispiel mit einem einzelnen Instrumentierungsschlüssel
In diesem Beispiel wird allen Apps auf dem aktuellen Computer ein einziger Instrumentierungsschlüssel zugewiesen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### <a name="example-with-an-instrumentation-key-map"></a>Beispiel mit Zuordnung des Instrumentierungsschlüssels
In diesem Beispiel:
- `MachineFilter` sucht mithilfe des Platzhalters `'.*'` nach dem aktuellen Computer.
- `AppFilter='WebAppExclude'` gibt `null` als Instrumentierungsschlüssel an. Die angegebene App wird nicht instrumentiert.
- `AppFilter='WebAppOne'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- `AppFilter='WebAppTwo'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- Abschließend verwendet `AppFilter` ebenfalls den Platzhalter `'.*'`, um alle anderen Web-Apps zu suchen, die nicht von den früheren Regeln gefunden wurden, und weist einen Standardinstrumentierungsschlüssel zu.
- Zur besseren Lesbarkeit werden Leerzeichen hinzugefügt.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})

```


### <a name="parameters"></a>Parameter

#### <a name="-instrumentationkey"></a>-InstrumentationKey
**Erforderlich.** Verwenden Sie diesen Parameter, um einen einzelnen Instrumentierungsschlüssel für die Verwendung durch alle Apps auf dem Zielcomputer bereitzustellen.

#### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Erforderlich.** Verwenden Sie diesen Parameter, um mehrere Instrumentierungsschlüssel und eine Zuordnung der von jeder einzelnen App verwendeten Instrumentierungsschlüssel bereitzustellen.
Durch Festlegen von `MachineFilter` können Sie ein einzelnes Installationsskript für mehrere Computer erstellen.

> [!IMPORTANT]
> Apps werden anhand der Regeln in der Reihenfolge gesucht, in der diese Regeln bereitgestellt werden. Geben Sie daher zuerst die spezifischsten Regeln und am Schluss die allgemeinsten Regeln an.

##### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'}})`

- **MachineFilter** ist ein erforderlicher regulärer C#-Ausdruck des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ComputerName“ findet nur Computer mit dem exakten angegebenen Namen.
- **AppFilter** ist ein erforderlicher regulärer C#-Ausdruck des IIS-Websitenamens. Mit dem Befehl [get-iissite](/powershell/module/iisadministration/get-iissite) können Sie eine Liste der Websites auf Ihrem Server abrufen.
    - „.*“ findet alle
    - „SiteName“ findet nur die IIS-Website mit dem exakten angegebenen Namen.
- **InstrumentationKey** ist erforderlich, um die Überwachung der Apps zu aktivieren, die den beiden oben genannten Filtern entsprechen.
    - Behalten Sie für diesen Wert NULL bei, wenn Sie Regeln zum Ausschließen der Überwachung definieren möchten.


#### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Optional.** Verwenden Sie diesen Schalter, um die Instrumentierungs-Engine zu aktivieren, sodass sie Ereignisse und Nachrichten über die Vorgänge bei der Ausführung eines verwalteten Prozesses sammelt. Diese Ereignisse und Nachrichten umfassen Abhängigkeitsergebniscodes, HTTP-Verben und SQL-Befehlstext.

Die Instrumentierungs-Engine erfordert Aufwand und ist standardmäßig deaktiviert.

#### <a name="-acceptlicense"></a>-AcceptLicense
**Optional.** Verwenden Sie diesen Schalter, um die Lizenz- und Datenschutzbestimmungen in monitorlosen Installationen anzunehmen.

#### <a name="-ignoresharedconfig"></a>-IgnoreSharedConfig
Wenn Sie über einen Cluster von Webservern verfügen, verwenden Sie möglicherweise eine [Freigabekonfiguration](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
Das HttpModule kann in diese Freigabekonfiguration nicht eingefügt werden.
Bei diesem Skript tritt ein Fehler auf, und es wird gemeldet, dass zusätzliche Installationsschritte erforderlich sind.
Verwenden Sie diesen Schalter, um diese Prüfung zu ignorieren und die Installation der Voraussetzungen fortzusetzen. Weitere Informationen finden Sie unter [Konflikt mit IIS-Freigabekonfiguration](status-monitor-v2-troubleshoot.md#conflict-with-iis-shared-configuration).

#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle anzuzeigen.

#### <a name="-whatif"></a>-WhatIf 
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um Ihre Eingabeparameter zu testen und zu überprüfen, ohne die Überwachung tatsächlich zu aktivieren.

### <a name="output"></a>Output

#### <a name="example-output-from-a-successful-enablement"></a>Beispielausgabe für eine erfolgreiche Aktivierung

```powershell
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
```

## <a name="disable-instrumentationengine"></a>Disable-InstrumentationEngine

Deaktiviert die Instrumentierungs-Engine durch Entfernen einiger Registrierungsschlüssel.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

### <a name="examples"></a>Beispiele

```powershell
PS C:\> Disable-InstrumentationEngine
```

### <a name="parameters"></a>Parameter 

#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.

### <a name="output"></a>Output


##### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Beispielausgabe nach erfolgreicher Deaktivierung der Instrumentierungs-Engine

```powershell
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```

## <a name="disable-applicationinsightsmonitoring"></a>Disable-ApplicationInsightsMonitoring

Deaktiviert die Überwachung auf dem Zielcomputer.
Dieses Cmdlet entfernt Änderungen an der IIS-Datei „applicationHost.config“ und Registrierungsschlüssel.

### <a name="examples"></a>Beispiele

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

### <a name="parameters"></a>Parameter 

#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle anzuzeigen.

### <a name="output"></a>Output


##### <a name="example-output-from-successfully-disabling-monitoring"></a>Beispielausgabe für eine erfolgreiche Deaktivierung der Überwachung

```powershell
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
```


## <a name="get-applicationinsightsmonitoringconfig"></a>Get-ApplicationInsightsMonitoringConfig

Ruft die Konfigurationsdatei ab und gibt die Werte in der Konsole aus.

### <a name="examples"></a>Beispiele

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

### <a name="parameters"></a>Parameter

Es müssen keine Parameter angegeben werden.

### <a name="output"></a>Output


##### <a name="example-output-from-reading-the-config-file"></a>Aus der Konfigurationsdatei ausgelesene Beispielausgabe

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="get-applicationinsightsmonitoringstatus"></a>Get-ApplicationInsightsMonitoringStatus

Dieses Cmdlet bietet Problembehandlungsinformationen zu Statusmonitor.
Sie können mithilfe dieses Cmdlets den Überwachungsstatus, die Version des PowerShell-Moduls und den laufenden Prozess untersuchen.
Dieses Cmdlet meldet Versionsinformationen und Informationen über Schlüsseldateien, die für die Überwachung erforderlich sind.

### <a name="examples"></a>Beispiele

#### <a name="example-application-status"></a>Beispiel: Anwendungsstatus

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus` aus, um den Status der Überwachung von Websites anzuzeigen.

```powershell

PS C:\Windows\system32> Get-ApplicationInsightsMonitoringStatus

IIS Websites:

SiteName               : Default Web Site
ApplicationPoolName    : DefaultAppPool
SiteId                 : 1
SiteState              : Stopped

SiteName               : DemoWebApp111
ApplicationPoolName    : DemoWebApp111
SiteId                 : 2
SiteState              : Started
ProcessId              : not found

SiteName               : DemoWebApp222
ApplicationPoolName    : DemoWebApp222
SiteId                 : 3
SiteState              : Started
ProcessId              : 2024
Instrumented           : true
InstrumentationKey     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123

SiteName               : DemoWebApp333
ApplicationPoolName    : DemoWebApp333
SiteId                 : 4
SiteState              : Started
ProcessId              : 5184
AppAlreadyInstrumented : true
```

In diesem Beispiel gilt Folgendes:
- **Machine Identifier** ist eine anonyme ID, mit der der Server eindeutig identifiziert wird. Wenn Sie eine Supportanfrage erstellen, benötigen wir diese ID, um die Protokolle für Ihren Server zu finden.
- **Default Web Site** wird in IIS beendet.
- **DemoWebApp111** wurde in IIS gestartet, hat aber noch keine Anforderungen empfangen. Dieser Bericht zeigt an, dass dieser Prozess derzeit nicht ausgeführt wird („ProcessId: not found“).
- **DemoWebApp222** wird ausgeführt und überwacht („Instrumented: true“). Basierend auf der Benutzerkonfiguration wurde der Instrumentierungsschlüssel „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123“ für diese Website gefunden.
- **DemoWebApp333** wurde manuell mit dem Application Insights-SDK instrumentiert. Statusmonitor hat das SDK ermittelt und wird diese Website nicht überwachen.


#### <a name="example-powershell-module-information"></a>Beispiel: Informationen zum PowerShell-Modul

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` aus, um Informationen über das aktuelle Modul anzuzeigen:

```powershell

PS C:\> Get-ApplicationInsightsMonitoringStatus -PowerShellModule

PowerShell Module version:
0.4.0-alpha

Application Insights SDK version:
2.9.0.3872

Executing PowerShell Module Assembly:
Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.11432, Culture=neutral, PublicKeyToken=31bf3856ad364e35

PowerShell Module Directory:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.2\content\PowerShell

Runtime Paths:
ParentDirectory (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content

ConfigurationPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config

ManagedHttpModuleHelperPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll

RedfieldIISModulePath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

InstrumentationEngine86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll

InstrumentationEngine64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll

InstrumentationEngineExtensionHost86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll

InstrumentationEngineExtensionHost64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll

InstrumentationEngineExtensionConfig86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config

InstrumentationEngineExtensionConfig64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config

ApplicationInsightsSdkPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll
```

#### <a name="example-runtime-status"></a>Beispiel: Laufzeitstatus

Sie können den Prozess auf dem instrumentierten Computer überprüfen, um herauszufinden, ob alle DLLs geladen wurden. Wenn die Überwachung funktioniert, sollten mindestens 12 DLLS geladen worden sein.

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus -InspectProcess` aus:


```
PS C:\> Get-ApplicationInsightsMonitoringStatus -InspectProcess

iisreset.exe /status
Status for IIS Admin Service ( IISADMIN ) : Running
Status for Windows Process Activation Service ( WAS ) : Running
Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
Status for World Wide Web Publishing Service ( W3SVC ) : Running

handle64.exe -accepteula -p w3wp
BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

listdlls64.exe -accepteula w3wp
0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
```

### <a name="parameters"></a>Parameter

#### <a name="no-parameters"></a>(Keine Parameter)

Standardmäßig meldet dieses Cmdlet den Überwachungsstatus von Webanwendungen.
Verwenden Sie diese Option, um zu überprüfen, ob Ihre Anwendung erfolgreich instrumentiert wurde.
Sie können auch überprüfen, welcher Instrumentierungsschlüssel Ihrer Website zugeordnet wurde.


#### <a name="-powershellmodule"></a>-PowerShellModule
**Optional:** Dieser Schalter meldet die Versionsnummern und Pfade von DLLs, die für die Überwachung erforderlich sind.
Verwenden Sie diese Option, wenn Sie die Version einer DLL ermitteln müssen (einschließlich Application Insights SDK).

#### <a name="-inspectprocess"></a>-InspectProcess

**Optional:** Verwenden Sie diesen Schalter, um zu melden, ob IIS ausgeführt wird.
Damit werden auch externe Tools heruntergeladen, um zu ermitteln, ob die erforderlichen DLLs in die IIS-Laufzeit geladen werden.


Wenn dieser Prozess aus irgendeinem Grund fehlschlägt, können Sie diese Befehle auch manuell ausführen:
- iisreset.exe /status
- [handle64.exe](/sysinternals/downloads/handle) -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
- [listdlls64.exe](/sysinternals/downloads/listdlls) w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"


#### <a name="-force"></a>-Force

**Optional:** Nur mit InspectProcess verwendet. Verwenden Sie diese Option, um die Benutzereingabeaufforderung zu überspringen, die angezeigt wird, bevor zusätzliche Tools heruntergeladen werden.


## <a name="set-applicationinsightsmonitoringconfig"></a>Set-ApplicationInsightsMonitoringConfig

Legt die Konfigurationsdatei fest, ohne eine vollständige Neuinstallation auszuführen.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorberechtigungen.


### <a name="examples"></a>Beispiele

#### <a name="example-with-a-single-instrumentation-key"></a>Beispiel mit einem einzelnen Instrumentierungsschlüssel
In diesem Beispiel wird allen Apps auf dem aktuellen Computer ein einzelner Instrumentierungsschlüssel zugewiesen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### <a name="example-with-an-instrumentation-key-map"></a>Beispiel mit Zuordnung des Instrumentierungsschlüssels
In diesem Beispiel:
- `MachineFilter` sucht mithilfe des Platzhalters `'.*'` nach dem aktuellen Computer.
- `AppFilter='WebAppExclude'` gibt `null` als Instrumentierungsschlüssel an. Die angegebene App wird nicht instrumentiert.
- `AppFilter='WebAppOne'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- `AppFilter='WebAppTwo'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- Abschließend verwendet `AppFilter` ebenfalls den Platzhalter `'.*'`, um alle anderen Web-Apps zu suchen, die nicht von den früheren Regeln gefunden wurden, und weist einen Standardinstrumentierungsschlüssel zu.
- Zur besseren Lesbarkeit werden Leerzeichen hinzugefügt.

```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap `
       @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
          @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
          @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
          @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})
```

### <a name="parameters"></a>Parameter

#### <a name="-instrumentationkey"></a>-InstrumentationKey
**Erforderlich.** Verwenden Sie diesen Parameter, um einen einzelnen Instrumentierungsschlüssel für die Verwendung durch alle Apps auf dem Zielcomputer bereitzustellen.

#### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Erforderlich.** Verwenden Sie diesen Parameter, um mehrere Instrumentierungsschlüssel und eine Zuordnung der von jeder einzelnen App verwendeten Instrumentierungsschlüssel bereitzustellen.
Durch Festlegen von `MachineFilter` können Sie ein einzelnes Installationsskript für mehrere Computer erstellen.

> [!IMPORTANT]
> Apps werden anhand der Regeln in der Reihenfolge gesucht, in der diese Regeln bereitgestellt werden. Geben Sie daher zuerst die spezifischsten Regeln und am Schluss die allgemeinsten Regeln an.

##### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** ist ein erforderlicher regulärer C#-Ausdruck des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ComputerName“ findet nur Computer mit dem angegebenen Namen.
- **AppFilter** ist ein erforderlicher regulärer C#-Ausdruck des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ApplicationName“ findet nur IIS-Apps mit dem angegebenen Namen.
- **InstrumentationKey** ist erforderlich, um die Überwachung der Apps zu aktivieren, die den beiden obigen Filtern entsprechen.
    - Behalten Sie für diesen Wert NULL bei, wenn Sie Regeln zum Ausschließen der Überwachung definieren möchten.


#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle anzuzeigen.


### <a name="output"></a>Output

Standardmäßig erfolgt keine Ausgabe.

##### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Beispielhafte ausführliche Ausgabe beim Festlegen der Konfigurationsdatei mit -InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

##### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Beispielhafte ausführliche Ausgabe beim Festlegen der Konfigurationsdatei mit -InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="start-applicationinsightsmonitoringtrace"></a>Start-ApplicationInsightsMonitoringTrace

Sammelt [ETW-Ereignisse](/windows/desktop/etw/event-tracing-portal) aus der Runtime des Anfügens ohne Code. Dieses Cmdlet ist eine Alternative zur Ausführung von [PerfView](https://github.com/microsoft/perfview).

Gesammelte Ereignisse werden an der Konsole in Echtzeit ausgegeben und in einer ETL-Datei gespeichert. Die ausgegebene ETL-Datei kann zur weiteren Untersuchung von [PerfView](https://github.com/microsoft/perfview) geöffnet werden.

Dieses Cmdlet wird ausgeführt, bis es die Timeoutdauer erreicht (standardmäßig 5 Minuten) oder manuell gestoppt wird (`Ctrl + C`).

### <a name="examples"></a>Beispiele

#### <a name="how-to-collect-events"></a>Vorgehensweise beim Sammeln von Ereignissen

Normalerweise würden wir Sie auffordern, Ereignisse zu sammeln, um zu untersuchen, warum Ihre Anwendung nicht instrumentiert wird.

Die Runtime des Anfügens ohne Code sendet ETW-Ereignisse beim Start von IIS und beim Start Ihrer Anwendung.

So sammeln Sie diese Ereignisse:
1. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /stop` aus, um IIS und alle Webanwendungen zu deaktivieren.
2. Ausführen dieses Cmdlets
3. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /start` aus, um IIS zu starten.
4. Versuchen Sie, zu Ihrer App zu navigieren.
5. Nachdem Ihre App geladen ist, können Sie sie manuell beenden (`Ctrl + C`) oder auf das Timeout warten.

#### <a name="what-events-to-collect"></a>Welche Ereignisse sollen gesammelt werden?

Beim Sammeln von Ereignissen haben Sie drei Möglichkeiten:
1. Verwenden Sie den Schalter `-CollectSdkEvents` zum Sammeln von Ereignissen, die vom Application Insights SDK ausgegeben werden.
2. Verwenden Sie den Schalter `-CollectRedfieldEvents` zum Sammeln von Ereignissen, die von Statusmonitor und Redfield Runtime ausgegeben werden. Diese Protokolle sind hilfreich zur Diagnose von IIS und Start der Anwendung.
3. Verwenden Sie beide Schalter, um beide Ereignistypen zu sammeln.
4. Wenn kein Schalter angegeben ist, werden standardmäßig beide Ereignistypen gesammelt.


### <a name="parameters"></a>Parameter

#### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Optional.** Verwenden Sie diesen Parameter, um festzulegen, wie lange das Skript Ereignisse sammeln soll. Die Standardeinstellung ist 5 Minuten.

#### <a name="-logdirectory"></a>-LogDirectory
**Optional.** Verwenden Sie diesen Schalter, um das Ausgabeverzeichnis der ETL-Datei festzulegen. Standardmäßig wird diese Datei im PowerShell Modules-Verzeichnis erstellt. Der vollständige Pfad wird während der Ausführung des Skripts angezeigt.


#### <a name="-collectsdkevents"></a>-CollectSdkEvents
**Optional.** Verwenden Sie diesen Schalter, um Application Insights SDK-Ereignisse zu sammeln.

#### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**Optional.** Verwenden Sie diesen Schalter zum Sammeln von Ereignissen von Statusmonitor und Redfield Runtime.

#### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.



### <a name="output"></a>Output


#### <a name="example-of-application-startup-logs"></a>Beispiel für Anwendungsstartprotokolle
```powershell
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```

## <a name="next-steps"></a>Nächste Schritte

  Anzeigen der Telemetrie:
 - [Untersuchen Sie Metriken](../platform/metrics-charts.md) zum Überwachen der Leistung und Nutzung.
- [Durchsuchen Sie Ereignisse und Protokolle](../../azure-monitor/app/diagnostic-search.md), um Probleme zu diagnostizieren.
- Verwenden Sie [Analytics](../log-query/log-query-overview.md) für erweiterte Abfragen.
- [Erstellen Sie Dashboards](../../azure-monitor/app/overview-dashboard.md).
 
 Hinzufügen weiterer Telemetrieelemente:
 - [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
- [Fügen Sie Webclient-Telemetriedaten hinzu](../../azure-monitor/app/javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe zu aktivieren.
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.
 
 Weitere Verwendungsmöglichkeiten für den Application Insights-Agent:
 - Verwenden Sie unsere Anleitung zur [Problembehandlung](status-monitor-v2-troubleshoot.md) für den Application Insights-Agent.
