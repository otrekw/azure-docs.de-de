---
title: Einrichten von Azure Functions
description: In diesem Tutorial wird beschrieben, wie Sie eine Azure-Funktions-App erstellen und für benutzerdefinierte Azure-Anbieter einrichten.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 55554678047faeedd16b78dea61a42d50fd59491
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98737319"
---
# <a name="set-up-azure-functions-for-azure-custom-providers"></a>Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter

Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt. Mit benutzerdefinierten Anbietern können Sie Workflows in Azure ändern. In diesem Tutorial wird erläutert, wie Sie eine Azure-Funktions-App zur Verwendung als benutzerdefinierter Anbieterendpunkt einrichten.

## <a name="create-the-azure-function-app"></a>Erstellen der Azure-Funktions-App

> [!NOTE]
> In diesem Tutorial erstellen Sie einen einfachen Dienstendpunkt, der eine Azure-Funktions-App verwendet. Ein benutzerdefinierter Anbieter kann jedoch jeden öffentlich zugänglichen Endpunkt verwenden. Zu den Alternativen gehören Azure Logic Apps, Azure API Management und die Web-Apps-Funktion von Azure App Service.

Als Einstieg in dieses Tutorial sollten Sie zuerst das Tutorial [Erstellen Ihrer ersten Funktion im Azure-Portal](../../azure-functions/functions-get-started.md) durcharbeiten. In diesem Tutorial wird eine .NET Core-Webhookfunktion erstellt, die im Azure-Portal geändert werden kann. Sie dient auch als Grundlage für das aktuelle Tutorial.

## <a name="install-azure-table-storage-bindings"></a>Installieren von Azure-Tabellenspeicherbindungen

So installieren Sie die Azure-Tabellenspeicherbindungen:

1. Navigieren Sie für „HttpTrigger“ zur Registerkarte **Integrieren**.
1. Wählen Sie **+ Neue Eingabe** aus.
1. Wählen Sie **Azure Table Storage** aus.
1. Falls die Erweiterung „Microsoft.Azure.WebJobs.Extensions.Storage“ noch nicht installiert ist, installieren Sie sie.
1. Geben Sie ins Feld **Tabellenparametername** den Namen **tableStorage** ein.
1. Geben Sie ins Feld **Tabellenname** den Namen **myCustomResources** ein.
1. Wählen Sie **Speichern** aus, um den aktualisierten Eingabeparameter zu speichern.

![Übersicht über benutzerdefinierte Anbieter mit Tabellenbindungen](./media/create-custom-provider/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Aktualisieren von RESTful-HTTP-Methoden

So richten Sie die Azure-Funktion so ein, dass sie die RESTful-Anforderungsmethoden des benutzerdefinierten Anbieters enthält:

1. Navigieren Sie für „HttpTrigger“ zur Registerkarte **Integrieren**.
1. Wählen Sie unter **Ausgewählte HTTP-Methoden** die Optionen **GET**, **POST**, **DELETE** und **PUT** aus.

![Übersicht über benutzerdefinierte Anbieter mit HTTP-Methoden](./media/create-custom-provider/azure-functions-http-methods.png)

## <a name="add-azure-resource-manager-nuget-packages"></a>Hinzufügen von Azure Resource Manager-NuGet-Paketen

> [!NOTE]
> Fehlt die C#-Projektdatei im Projektverzeichnis, können Sie sie manuell hinzufügen. Sie wird auch angezeigt, nachdem die Erweiterung „Microsoft.Azure.WebJobs.Extensions.Storage“ für die Funktions-App installiert wurde.

Aktualisieren Sie als Nächstes die C#-Projektdatei so, dass Sie nützliche NuGet-Bibliotheken enthält. Diese Bibliotheken vereinfachen das Analysieren eingehender Anforderungen von benutzerdefinierten Anbietern. Führen Sie die Schritte unter [Manuelles Installieren oder Aktualisieren von Azure Functions-Bindungserweiterungen aus dem Portal](../../azure-functions/functions-bindings-register.md) aus, und aktualisieren Sie die C#-Projektdatei so, dass sie die folgenden Paketverweise enthält:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

Das folgende XML-Element ist ein Beispiel für eine C#-Projektdatei:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktions-App zur Verwendung als benutzerdefinierter Anbieterendpunkt eingerichtet.

Informationen zum Erstellen eines benutzerdefinierten RESTful-Anbieterendpunkts finden Sie unter [Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter](./tutorial-custom-providers-function-authoring.md).