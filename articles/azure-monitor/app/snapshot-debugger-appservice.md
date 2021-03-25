---
title: Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service | Microsoft-Dokumentation
description: Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 5a637a6f355be32f82878a52a30c77d020190651
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102211616"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Aktivieren des Momentaufnahmedebuggers für .NET-Apps in Azure App Service

Der Momentaufnahmedebugger unterstützt derzeit ASP.NET- und ASP.NET Core-Apps, die unter Azure App Service in Windows-Dienstplänen ausgeführt werden.

Wenn Sie den Momentaufnahmedebugger verwenden, sollten Sie Ihre Anwendung auf der Dienstebene „Basic“ oder höher auszuführen.

Bei den meisten Anwendungen haben die Dienstebenen „Free“ und „Shared“ nicht genügend Arbeits- oder Festplattenspeicher zum Speichern von Momentaufnahmen.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a> Aktivieren des Momentaufnahmedebuggers
Befolgen Sie die unten angegebenen Anweisungen, um den Momentaufnahmedebugger für eine App zu aktivieren.

Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anweisungen beim Aktivieren des Momentaufnahmedebuggers auf anderen unterstützten Plattformen weiter:
* [Azure-Funktion](snapshot-debugger-function-app.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-Dienste](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines und VM-Skalierungsgruppen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Lokale virtuelle oder physische Computer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

> [!NOTE]
> Wenn Sie eine Vorschauversion von .NET Core verwenden oder Ihre Anwendung direkt oder indirekt (über eine abhängige Assembly) auf das Application Insights SDK verweist, führen Sie die Anweisungen unter [Aktivieren des Momentaufnahmedebuggers für andere Umgebungen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) aus, um das NuGet-Paket [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in die Anwendung zu integrieren. Führen Sie danach die übrigen nachstehenden Anweisungen aus. 
>
> Die codefreie Installation des Application Insights-Momentaufnahmedebuggers folgt der Richtlinie zur .NET Core-Unterstützung.
> Weitere Informationen zu unterstützten Runtimes finden Sie in der [.NET Core-Unterstützungsrichtlinie](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

Der Momentaufnahmedebugger ist als Teil der App Services-Runtime zwar bereits vorinstalliert, muss aber noch aktiviert werden, um Momentaufnahmen für Ihre App Service-Apps zu erhalten.

Nachdem Sie eine App bereitgestellt haben, führen Sie die folgenden Schritte aus, um den Momentaufnahmedebugger zu aktivieren:

1. Navigieren Sie zur Azure-Systemsteuerung für Ihre App Service-Instanz.
2. Wechseln Sie zur Seite **Einstellungen > Application Insights**.

   ![Aktivieren von App Insights im App Services-Portal](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Befolgen Sie die Anweisungen auf der Seite, um eine neue Ressource zu erstellen, oder wählen Sie eine vorhandene App Insights-Ressource aus, um Ihre App zu überwachen. Stellen Sie außerdem sicher, dass beide Schalter für den Momentaufnahmedebugger auf **Ein** festgelegt sind.

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

4. Der Momentaufnahmedebugger wurde mithilfe einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für den Momentaufnahmedebugger][snapshot-debugger-app-setting]

## <a name="enable-snapshot-debugger-for-other-clouds"></a>Aktivieren des Momentaufnahmedebuggers für andere Clouds

Derzeit sind [Azure Government](https://docs.microsoft.com/azure/azure-government/compare-azure-government-global-azure#application-insights) und [Azure China](https://docs.microsoft.com/azure/china/resources-developer-guide) die einzigen Regionen, für die Endpunktänderungen über die Application Insights-Verbindungszeichenfolge erforderlich sind.

|Eigenschaft mit der Verbindungszeichenfolge    | US Government-Cloud | China-Cloud |   
|---------------|---------------------|-------------|
|SnapshotEndpoint         | `https://snapshot.monitor.azure.us`    | `https://snapshot.monitor.azure.cn` |

Weitere Informationen zu anderen Verbindungsüberschreibungen finden Sie in der [Application Insights-Dokumentation](https://docs.microsoft.com/azure/azure-monitor/app/sdk-connection-string?tabs=net#connection-string-with-explicit-endpoint-overrides).

## <a name="disable-snapshot-debugger"></a>Deaktivieren des Momentaufnahmedebuggers

Führen Sie die gleichen Schritte wie zum **Aktivieren des Momentaufnahmedebuggers** aus, legen Sie jedoch beide Schalter für den Momentaufnahmedebugger auf **Aus** fest.

Es wird empfohlen, den Momentaufnahmedebugger für alle Ihre Apps zu aktivieren, um die Diagnose von Anwendungsausnahmen zu erleichtern.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

Bei einer Azure App Service-App können Sie App-Einstellungen in einer Azure Resource Manager-Vorlage festlegen, um Momentaufnahmedebugger und Profiler zu aktivieren. Sehen Sie sich dazu den Vorlagenausschnitt unten an:

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr für Ihre Anwendung, der eine Ausnahme auslösen kann. Warten Sie dann 10 bis 15 Minuten, bis die Momentaufnahmen an die Application Insights-Instanz gesendet werden.
- Weitere Informationen finden Sie unter [Momentaufnahmen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) im Azure-Portal.
- Hilfe bei der Behandlung von Problemen mit dem Momentaufnahmedebugger finden Sie unter [Problembehandlung für Momentaufnahmedebugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

