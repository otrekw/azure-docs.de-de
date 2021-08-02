---
title: Automatisieren von Anwendungsbereitstellungen für Azure Spring Cloud
description: Beschreibt die Verwendung des Azure Spring Cloud-Tasks für Azure Pipelines.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 05/12/2021
ms.author: brendm
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 2df1c8f0e091e553e045efb8aea5665d29bbc53f
ms.sourcegitcommit: 1ee13b62c094a550961498b7a52d0d9f0ae6d9c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109839327"
---
# <a name="automate-application-deployments-to-azure-spring-cloud"></a>Automatisieren von Anwendungsbereitstellungen für Azure Spring Cloud

Continuous Integration- und Continuous Delivery-Tools (CI/CD) ermöglichen Ihnen die schnelle Bereitstellung von Updates für vorhandene Anwendungen mit minimalem Aufwand und Risiko. Mit Azure DevOps können Sie diese wichtigen Aufträge organisieren und steuern. 

In diesem Artikel erfahren Sie, wie Sie den [Azure Spring Cloud-Task für Azure Pipelines](/azure/devops/pipelines/tasks/deploy/azure-spring-cloud) zum Bereitstellen von Anwendungen verwenden.

## <a name="create-an-azure-resource-manager-service-connection"></a>Erstellen einer Azure Resource Manager-Dienstverbindung

Lesen Sie [diesen Artikel](/azure/devops/pipelines/library/connect-to-azure), um zu erfahren, wie Sie eine Azure Resource Manager-Dienstverbindung mit Ihrem Azure DevOps-Projekt herstellen. Achten Sie darauf, dasselbe Abonnement auszuwählen, das Sie für Ihre Azure Spring Cloud-Dienstinstanz verwenden.

## <a name="build-and-deploy-apps"></a>Erstellen und Bereitstellen von Apps

::: zone pivot="programming-language-csharp"
### <a name="deploy-artifacts"></a>Bereitstellen von Artefakten

Sie können Ihre Projekte mit einer Reihe von Tasks erstellen und bereitstellen. Dieser Codeausschnitt definiert Variablen, einen .NET Core-Task zum Erstellen der Anwendung und einen Azure Spring Cloud-Task zum Bereitstellen der Anwendung.

```yaml
variables:
  workingDirectory: './steeltoe-sample'
  planetMainEntry: 'Microsoft.Azure.SpringCloud.Sample.PlanetWeatherProvider.dll'
  solarMainEntry: 'Microsoft.Azure.SpringCloud.Sample.SolarSystemWeather.dll'
  planetAppName: 'planet-weather-provider'
  solarAppName: 'solar-system-weather'
  serviceName: '<your service name>'


steps:
# Restore, build, publish and package the zipped planet app
- task: DotNetCoreCLI@2
  inputs:
    command: 'publish'
    publishWebProjects: false
    arguments: '--configuration Release'
    zipAfterPublish: false
    modifyOutputPath: false
    workingDirectory: $(workingDirectory)

# Deploy the planet app
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<Service Connection Name>'
    Action: 'Deploy'
    AzureSpringCloud: $(serviceName)
    AppName: 'testapp'
    UseStagingDeployment: false
    DeploymentName: 'default'
    Package: $(workingDirectory)/src/$(planetAppName)/publish-deploy-planet.zip
    RuntimeVersion: 'NetCore_31'
    DotNetCoreMainEntryPath: $(planetMainEntry)

# Deploy the solar app
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<Service Connection Name>'
    Action: 'Deploy'
    AzureSpringCloud: $(serviceName)
    AppName: 'testapp'
    UseStagingDeployment: false
    DeploymentName: 'default'
    Package: $(workingDirectory)/src/$(solarAppName)/publish-deploy-solar.zip
    RuntimeVersion: 'NetCore_31'
    DotNetCoreMainEntryPath: $(solarMainEntry)
```

::: zone-end
::: zone pivot="programming-language-java"
### <a name="deploy-artifacts"></a>Bereitstellen von Artefakten

#### <a name="to-production"></a>In einer Produktionsumgebung

Sie können Ihre Projekte mit einer Reihe von Tasks erstellen und bereitstellen. In diesem Codeausschnitt wird zunächst ein Maven-Task zum Erstellen der Anwendung definiert, gefolgt von einem zweiten Task, der die JAR-Datei mithilfe des Azure Spring Cloud-Tasks für Azure Pipelines bereitstellt.

```yaml
steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<your service connection name>'
    Action: 'Deploy'
    AzureSpringCloud: <your Azure Spring Cloud service>
    AppName: <app-name>
    UseStagingDeployment: false
    DeploymentName: 'default'
    Package: ./target/your-result-jar.jar
```

#### <a name="blue-green-deployments"></a>Blaugrün-Bereitstellungen

Die im vorherigen Abschnitt gezeigte Bereitstellung empfängt bei der Bereitstellung sofort Anwendungdatenverkehr. Manchmal möchten Entwickler ihre Anwendungen in der Produktionsumgebung testen, bevor die Anwendung Kundendatenverkehr empfängt.

Mit dem folgenden Codeausschnitt wird die Anwendung auf die gleiche Weise wie oben beschrieben erstellt und dann in einer Stagingbereitstellung bereitgestellt. In diesem Beispiel muss die Stagingbereitstellung bereits vorhanden sein. Einen alternativen Ansatz finden Sie unter [Strategien für Blau-Grün-Bereitstellungen](concepts-blue-green-deployment-strategies.md).


```yaml
steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<your service connection name>'
    Action: 'Deploy'
    AzureSpringCloud: <your Azure Spring Cloud service>
    AppName: <app-name>
    UseStagingDeployment: true
    Package: ./target/your-result-jar.jar
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<your service connection name>'
    Action: 'Set Production'
    AzureSpringCloud: <your Azure Spring Cloud service>
    AppName: <app-name>
    UseStagingDeployment: true
```

### <a name="deploy-from-source"></a>Bereitstellen aus der Quelle

Es ist möglich, die Bereitstellung direkt in Azure durchzuführen, ohne einen separaten Buildschritt auszuführen.

```yaml
- task: AzureSpringCloud@0
  inputs:
    azureSubscription: '<your service connection name>'
    Action: 'Deploy'
    AzureSpringCloud: <your Azure Spring Cloud service>
    AppName: <app-name>
    UseStagingDeployment: false
    DeploymentName: 'default'
    Package: $(Build.SourcesDirectory)
```
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Bereitstellen Ihrer ersten Azure Spring Cloud-Anwendung](./quickstart.md)

