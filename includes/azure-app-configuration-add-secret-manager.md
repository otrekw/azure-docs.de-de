---
ms.openlocfilehash: c3c5b8ef94b507cad433e587c9ebfc2ec16c0ff9
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91440402"
---
## <a name="add-secret-manager"></a>Hinzufügen des Geheimnis-Managers

Ein Tool namens „Geheimnis-Manager“ speichert sensible Daten für die Entwicklungsarbeit außerhalb Ihrer Projektstruktur. Mit diesem Ansatz können Sie verhindern, dass App-Geheimnisse versehentlich im Quellcode angegeben werden. Führen Sie die folgenden Schritte aus, um die Verwendung des Geheimnis-Managers im ASP.NET Core-Projekt zu aktivieren:

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

Navigieren Sie zum Stammverzeichnis des Projekts, und führen Sie den folgenden Befehl aus, um das Speichern von Geheimnissen im Projekt zu aktivieren:

```dotnetcli
dotnet user-secrets init
```

Ein `UserSecretsId`-Element, das eine GUID enthält, wird der *.csproj*-Datei hinzugefügt:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

1. Öffnen Sie die Datei *.csproj*.

1. Fügen Sie der *.csproj*-Datei wie hier gezeigt ein `UserSecretsId`-Element hinzu. Sie können dieselbe GUID verwenden, oder Sie können diesen Wert durch ihre eigenen Werte ersetzen.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>
    
    </Project>
    ```
    
1. Speichern Sie die *CSPROJ*-Datei.

---

> [!TIP]
> Weitere Informationen zum Geheimnis-Manager finden Sie unter [Sichere Speicherung von App-Geheimnissen in der Entwicklung in ASP.NET Core](/aspnet/core/security/app-secrets).
