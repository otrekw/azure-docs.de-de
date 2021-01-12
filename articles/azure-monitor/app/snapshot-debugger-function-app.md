---
title: Aktivieren des Momentaufnahmedebuggers für .NET- und .NET Core-Apps in Azure Functions | Microsoft-Dokumentation
description: Aktivieren des Momentaufnahmedebuggers für .NET- und .NET Core-Apps in Azure Functions
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 12/18/2020
ms.openlocfilehash: 3060bd6ea8d7fbc4a4bf005b84cd07d420987ab6
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696318"
---
# <a name="enable-snapshot-debugger-for-net-and-net-core-apps-in-azure-functions"></a>Aktivieren des Momentaufnahmedebuggers für .NET- und .NET Core-Apps in Azure Functions

Der Momentaufnahmedebugger kann derzeit für ASP.NET- und ASP.NET Core-Apps verwendet werden, die unter Azure Functions in Windows-Dienstplänen ausgeführt werden.

Wenn Sie den Debugger für Momentaufnahmen verwenden, empfehlen wir, Ihre Anwendung auf der Dienstebene „Basic“ oder höher auszuführen.

Bei den meisten Anwendungen haben die Dienstebenen „Free“ und „Shared“ nicht genügend Arbeits- oder Festplattenspeicher zum Speichern von Momentaufnahmen.

## <a name="prerequisites"></a>Voraussetzungen

* [Aktivieren der Application Insights-Überwachung in Ihrer Funktions-App](https://docs.microsoft.com/azure/azure-functions/configure-monitoring#add-to-an-existing-function-app)

## <a name="enable-snapshot-debugger"></a> Aktivieren des Momentaufnahmedebuggers

Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anweisungen beim Aktivieren des Momentaufnahmedebuggers auf anderen unterstützten Plattformen weiter:
* [Azure App Service](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-Dienste](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines und VM-Skalierungsgruppen](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Lokale virtuelle oder physische Computer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Um den Momentaufnahmedebugger in Ihrer Funktions-App zu aktivieren, müssen Sie Ihre `host.json`-Datei aktualisieren, indem Sie die Eigenschaft `snapshotConfiguration` wie unten definiert hinzufügen und Ihre Funktion erneut bereitstellen.

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "snapshotConfiguration": {
        "isEnabled": true
      }
    }
  }
}
```

Der Momentaufnahmedebugger ist als Teil der Azure Functions-Runtime vorinstalliert, die standardmäßig deaktiviert ist.

Das der Momentaufnahmedebugger nicht in der Azure Functions-Runtime enthalten ist, ist es nicht erforderlich, zusätzliche NuGet-Pakete oder Anwendungseinstellungen hinzuzufügen.

Bei einer einfachen Funktions-App sehen `.csproj`, `{Your}Function.cs` und `host.json` folgendermaßen aus, nachdem der Momentaufnahmedebugger für diese ausgeführt wurde.

CSPROJ-Projekt

```xml
<Project Sdk="Microsoft.NET.Sdk">
<PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
    <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.31" />
</ItemGroup>
<ItemGroup>
    <None Update="host.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </None>
    <None Update="local.settings.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    <CopyToPublishDirectory>Never</CopyToPublishDirectory>
    </None>
</ItemGroup>
</Project>
```

Function-Klasse

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

namespace SnapshotCollectorAzureFunction
{
    public static class ExceptionFunction
    {
        [FunctionName("ExceptionFunction")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            throw new NotImplementedException("Dummy");
        }
    }
}
```

Hostdatei

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "samplingExcludedTypes": "Request",
      "samplingSettings": {
        "isEnabled": true
      },
      "snapshotConfiguration": {
        "isEnabled": true
      }
    }
  }
}
```

## <a name="disable-snapshot-debugger"></a>Deaktivieren des Momentaufnahmedebuggers

Um den Momentaufnahmedebugger in Ihrer Funktions-App zu deaktivieren, müssen Sie einfach Ihre `host.json`-Datei aktualisieren, indem Sie `false` auf die Eigenschaft `snapshotConfiguration.isEnabled` festlegen.

```json
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "snapshotConfiguration": {
        "isEnabled": false
      }
    }
  }
}
```

Es wird empfohlen, den Momentaufnahmedebugger für alle Ihre Apps zu aktivieren, um die Diagnose von Anwendungsausnahmen zu erleichtern.

## <a name="next-steps"></a>Nächste Schritte

- Generieren Sie Datenverkehr für Ihre Anwendung, der eine Ausnahme auslösen kann. Warten Sie dann 10 bis 15 Minuten, bis die Momentaufnahmen an die Application Insights-Instanz gesendet werden.
- Weitere Informationen finden Sie unter [Momentaufnahmen anzeigen](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) im Azure-Portal.
- Passen Sie die Konfiguration für den Momentaufnahmedebugger basierend auf Ihrem Anwendungsfall in Ihrer Funktions-App an. Weitere Informationen finden Sie unter [Momentaufnahmekonfiguration in host.json](https://docs.microsoft.com/azure/azure-functions/functions-host-json#applicationinsightssnapshotconfiguration).
- Hilfe bei der Behandlung von Problemen mit dem Momentaufnahmedebugger finden Sie unter [Problembehandlung für Momentaufnahmedebugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).
