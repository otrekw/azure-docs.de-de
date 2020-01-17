---
title: Angeben von Umgebungsvariablen für Dienste
description: Es wird gezeigt, wie Umgebungsvariablen für Anwendungen in Service Fabric verwendet werden können.
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f4c4f2a1c140e3d0f181c4fd55482056f9f91b62
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614314"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Angeben von Umgebungsvariablen für Dienste in Service Fabric

In diesem Artikel erfahren Sie, wie Sie Umgebungsvariablen für einen Dienst oder Container in Service Fabric angeben können.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Vorgehensweise zum Angeben von Umgebungsvariablen für Dienste

In diesem Beispiel legen Sie eine Umgebungsvariable für einen Container fest. Für den Artikel wird davon ausgegangen, dass Sie bereits eine Anwendung und ein Dienstmanifest haben.

1. Öffnen Sie die Datei „ServiceManifest.xml“.
2. Fügen Sie im `CodePackage`-Element ein neues `EnvironmentVariables`-Element und für jede Umgebungsvariable ein `EnvironmentVariable`-Element hinzu.

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   Umgebungsvariablen können im Anwendungsmanifest überschrieben werden.

3. Um die Umgebungsvariablen im Anwendungsmanifest zu überschreiben, verwenden Sie das `EnvironmentOverrides`-Element.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>Dynamisches Angeben von Umgebungsvariablen mithilfe von Docker Compose

Service Fabric unterstützt die [Verwendung von Docker Compose für die Bereitstellung](service-fabric-docker-compose.md#supported-compose-directives). Compose-Dateien können Umgebungsvariablen aus der Shell als Quellen verwenden. Dieses Verhalten kann verwendet werden, um gewünschte Umgebungswerte dynamisch zu ersetzen:

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu einigen der grundlegenden Konzepte, die in diesem Artikel behandelt werden, finden Sie im Artikel [Verwalten von Anwendungen für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

Informationen zu anderen App-Verwaltungsfunktionen in Visual Studio finden Sie unter [Verwalten Ihrer Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).
