---
title: Registrieren von Bindungserweiterungen von Azure Functions
description: Lernen Sie, wie Sie eine Azure Functions-Bindungserweiterung abhängig von Ihrer Umgebung registrieren.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 43bc278ea3cbd14690f1a9ac9263872536b5b174
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88224780"
---
# <a name="register-azure-functions-binding-extensions"></a>Registrieren von Bindungserweiterungen von Azure Functions

Seit Azure Functions Version 2.x sind [Bindungen](./functions-triggers-bindings.md) als separate Pakete aus der Functions-Runtime verfügbar. Während .NET-Funktionen über NuGet-Pakete auf Bindungen zugreifen, ermöglichen Erweiterungsbundles anderen Funktionen den Zugriff auf alle Bindungen über eine Konfigurationseinstellung.

Beachten Sie die folgenden Punkte im Zusammenhang mit Bindungserweiterungen:

- Bindungserweiterungen werden in Functions 1.x nicht explizit registriert, mit Ausnahme der [Erstellung einer C#-Klassenbibliothek mithilfe von Visual Studio](#local-csharp).

- HTTP und Zeitgebertrigger werden standardmäßig unterstützt und erfordern keine Erweiterung.

In der folgenden Tabelle ist angegeben, wann und wie Sie Bindungen registrieren.

| Entwicklungsumgebung |Registrierung<br/> in Functions 1.x  |Registrierung<br/> in Functions 3.x/2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure-Portal|Automatic|Automatic<sup>*</sup>|
|Nicht zu .NET gehörende Sprachen oder lokale Azure Core Tools-Entwicklung|Automatic|[Verwenden von Azure Functions Core Tools und Erweiterungsbundles](#extension-bundles)|
|C#-Klassenbibliothek mit Visual Studio 2019|[Verwendung von NuGet-Tools](#vs)|[Verwendung von NuGet-Tools](#vs)|
|C#-Klassenbibliothek mit Visual Studio Code|–|[Verwendung der .NET Core-CLI](#vs-code)|

<sup>*</sup> Das Portal verwendet Erweiterungsbündel.

## <a name="extension-bundles"></a><a name="extension-bundles"></a>Erweiterungsbündel

Mit Erweiterungsbündeln können Sie kompatible Bindungserweiterungen von Azure Functions Ihrer Funktions-App hinzufügen. Bei der Verwendung von Bündeln wird beim Erstellen der App ein vordefinierter Satz von Erweiterungen hinzugefügt. In einem Bündel definierte Erweiterungspakete werden auf ihre Kompatibilität miteinander überprüft, um Konflikte zwischen Paketen zu vermeiden. Mit Erweiterungsbündeln vermeiden Sie, .NET-Projektcode mit einem Nicht-.NET-Funktionsprojekt zu veröffentlichen. Sie aktivieren Erweiterungsbündel in der Datei „host.json“ des Hosts der App.  

Sie können Erweiterungsbündel mit Version 2.x und höheren Versionen der Functions-Runtime verwenden. 

Nutzen Sie Erweiterungsbündel für die lokale Entwicklung mit Azure Functions Core Tools oder Visual Studio Code sowie bei der Remoteerstellung. Stellen Sie beim lokalen Entwickeln sicher, dass Sie die aktuelle Version der [Azure Functions Core Tools](functions-run-local.md#v2) verwenden. Erweiterungsbündel werden auch zum Entwickeln von Funktionen im Azure-Portal verwendet. 

Wenn Sie keine Erweiterungsbündel verwenden, müssen Sie das .NET Core 2.x SDK auf Ihrem lokalen Computer installieren, bevor Sie [explizit Bindungserweiterungen installieren](#explicitly-install-extensions). Eine Datei „extensions.csproj“, die explizit die erforderlichen Erweiterungen definiert, wird dem Projekt hinzugefügt. Erweiterungsbündel heben diese Anforderungen für die lokale Entwicklung auf. 

Um die Erweiterungsbündel zu verwenden, aktualisieren Sie die Datei *host.json* so, dass sie den folgenden Eintrag für `extensionBundle` enthält:
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="explicitly-install-extensions"></a>Explizites Installieren von Erweiterungen

[!INCLUDE [functions-extension-register-core-tools](../../includes/functions-extension-register-core-tools.md)]

## <a name="nuget-packages"></a><a name="local-csharp"></a>NuGet-Pakete

Bei einem auf C#-Klassenbibliotheken basierenden Functions-Projekt sollten Sie Erweiterungen direkt installieren. Erweiterungsbündel wurden eigens für Projekte konzipiert, die nicht auf C#-Klassenbibliotheken basieren.

### <a name="c-class-library-with-visual-studio"></a><a name="vs"></a>C\#-Klassenbibliothek mit Visual Studio

In **Visual Studio** können Sie Pakete mithilfe des Befehls [Install-Package](/nuget/tools/ps-ref-install-package) aus der Paket-Manager-Konsole installieren, wie im folgenden Beispiel gezeigt wird:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Der Name des Pakets, das für eine bestimmte Bindung verwendet wird, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#functions-1x).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

Wenn Sie `Install-Package` verwenden, um auf eine Bindung zu verweisen, müssen Sie keine [Erweiterungsbündel](#extension-bundles) verwenden. Diese Vorgehensweise ist spezifisch für Klassenbibliotheken, die in Visual Studio erstellt werden.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a> C#-Klassenbibliothek mit Visual Studio Code

Installieren Sie in **Visual Studio Code** Pakete für eine C#-Klassenbibliothek über die Eingabeaufforderung. Verwenden Sie hierfür den Befehl [dotnet add package](/dotnet/core/tools/dotnet-add-package) in der .NET Core-CLI. Im folgenden Beispiel wird veranschaulicht, wie Sie eine Bindung hinzufügen:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

Die .NET Core-CLI kann nur für die Entwicklung in Azure Functions 2.x verwendet werden.

Ersetzen Sie `<BINDING_TYPE_NAME>` durch den Namen des Pakets, das die benötigte Bindung enthält. Den gewünschten Bindungsreferenzartikel finden Sie in der [Liste der unterstützten Bindungen](./functions-triggers-bindings.md#supported-bindings).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Trigger- und Bindungsbeispiel für Azure Functions](./functions-bindings-example.md)
