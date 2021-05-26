---
title: Einrichten von DevOps für Azure Logic Apps-Instanzen mit nur einem Mandanten
description: Vorgehensweise zum Einrichten der DevOps-Bereitstellung für Workflows in Azure Logic Apps-Instanzen mit nur einem Mandanten.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: how-to
ms.date: 05/25/2021
ms.openlocfilehash: bd35af7ac6602bba7e23bd10eda2f2b0fdff71db
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110379753"
---
# <a name="set-up-devops-deployment-for-single-tenant-azure-logic-apps"></a>Einrichten der DevOps-Bereitstellung für Azure Logic Apps-Instanzen mit nur einem Mandanten

In diesem Artikel erfahren Sie, wie Sie mithilfe von DevOps-Tools und -Prozessen ein auf einem einzelnen Mandanten basierendes Logik-App-Projekt aus Visual Studio Code in Ihrer Infrastruktur bereitstellen. Wählen Sie in Abhängigkeit davon, ob Sie GitHub oder Azure DevOps für die Bereitstellung bevorzugen, den Pfad und die Tools aus, die für Ihr Szenario am besten geeignet sind. Sie können die enthaltenen Beispiele verwenden, die Logik-App-Beispielprojekte sowie Beispiele für die Azure-Bereitstellung mithilfe von GitHub oder Azure DevOps enthalten. Weitere Informationen zu DevOps für einzelne Mandanten finden Sie unter [DevOps-Bereitstellungsübersicht für Azure Logic Apps-Instanzen mit nur einem Mandanten](devops-deployment-single-tenant-azure-logic-apps.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Ein auf einem einzelnen Mandanten basierendes Logik-App-Projekt, das mit [Visual Studio Code und der Azure Logic Apps-Erweiterung (Standard)](create-single-tenant-workflows-visual-studio-code.md#prerequisites) erstellt wurde.

  Wenn Sie noch kein Logik-App-Projekt oder keine Infrastruktur eingerichtet haben, können Sie die enthaltenen Beispielprojekte verwenden, um eine Beispiel-App und -infrastruktur auf Grundlage der Quell- und Bereitstellungsoptionen bereitzustellen, die Sie verwenden möchten. Weitere Informationen zu diesen Beispielprojekten und den enthaltenen Ressourcen für die Ausführung der Beispiel-Logik-App, finden Sie unter [Bereitstellen Ihrer Infrastruktur](#deploy-infrastructure).

- Wenn Sie eine Bereitstellung in Azure durchführen möchten, benötigen Sie eine vorhandene **Logik-App-Ressource (Standard)** , die in Azure erstellt wurde. Informationen zum schnellen Erstellen einer leeren Logik-App-Ressource finden Sie unter [Erstellen von auf einzelnen Mandanten basierenden Logik-App-Workflows – Portal](create-single-tenant-workflows-azure-portal.md).

<a name="deploy-infrastructure"></a>

## <a name="deploy-infrastructure-resources"></a>Bereitstellen von Infrastrukturressourcen

Wenn Sie noch kein Logik-App-Projekt oder keine Infrastruktur eingerichtet haben, können Sie die folgenden Beispielprojekte verwenden, um eine Beispiel-App und -infrastruktur auf Grundlage der Quell- und Bereitstellungsoptionen bereitzustellen, die Sie verwenden möchten.

- [GitHub-Beispiel für Azure Logic Apps-Instanzen mit nur einem Mandanten](https://github.com/Azure/logicapps/tree/master/github-sample)

  Dieses Beispiel enthält ein Logik-App-Beispielprojekt für Azure Logic Apps-Instanzen mit nur einem Mandanten sowie Beispiele für die Azure-Bereitstellung und GitHub Actions.

- [Azure DevOps-Beispiel für Azure Logic Apps-Instanzen mit nur einem Mandanten](https://github.com/Azure/logicapps/tree/master/azure-devops-sample)

  Dieses Beispiel enthält ein Logik-App-Beispielprojekt für Azure Logic Apps-Instanzen mit nur einem Mandanten sowie Beispiele für die Azure-Bereitstellung und Azure Pipelines.
  
Beide Beispiele enthalten die folgenden Ressourcen, die eine Logik-App für die Ausführung verwendet.

| Ressourcenname | Erforderlich | BESCHREIBUNG |
|---------------|----------|-------------|
| Logik-App (Standard) | Ja | Diese Azure-Ressource enthält die Workflows, die in Azure Logic Apps-Instanzen mit nur einem Mandanten ausgeführt werden. |
| Functions Premium- oder App Service-Hostingplan | Ja | Diese Azure-Ressource gibt die Hostingressourcen an, die zum Ausführen Ihrer Logik-App verwendet werden soll, z. B. Compute, Verarbeitung, Speicher, Netzwerk usw. <p><p>**Wichtig**: In der aktuellen Erfahrung erfordert die **Logik-App-Ressource (Standard)** den [**Workflow Standard**-Hostingplan](logic-apps-pricing.md#standard-pricing), der auf dem Functions Premium-Hostingplan basiert. |
| Azure-Speicherkonto | Ja, für zustandslose Workflows. | Diese Azure-Ressource speichert die Metadaten, den Zustand, die Ein- und Ausgaben, den Ausführungsverlauf sowie andere Informationen zu Ihren Workflows. |
| Application Insights | Optional | Diese Azure-Ressource bietet Überwachungsfunktionen für Ihre Workflows. |
| API-Verbindungen | Optional, wenn nicht vorhanden. | Diese Azure-Ressourcen definieren alle verwalteten API-Verbindungen, die Ihre Workflows zum Ausführen verwalteter Connectorvorgänge verwenden, z. B. Office 365, SharePoint usw. <p><p>**Wichtig**: In Ihrem Logik-App-Projekt enthält die Datei **connections.json** Metadaten, Endpunkte und Schlüssel für alle verwalteten API-Verbindungen und Azure-Funktionen, die von Ihren Workflows verwendet werden. Um unterschiedliche Verbindungen und Funktionen in jeder Umgebung zu verwenden, stellen Sie sicher, dass Sie die Datei **connections.json** parametrisieren und die Endpunkte aktualisieren. <p><p>Weitere Informationen finden Sie unter [API-Verbindungsressourcen und Zugriffsrichtlinien](#api-connection-resources). |
| ARM-Vorlage (Azure Resource Manager) | Optional | Diese Azure-Ressource definiert eine Baseline-Infrastrukturbereitstellung, die Sie wiederverwenden oder [exportieren](../azure-resource-manager/templates/template-tutorial-export-template.md) können. Die Vorlage enthält außerdem die erforderlichen Zugriffsrichtlinien, z. B. für die Verwendung verwalteter API-Verbindungen. <p><p>**Wichtig**: Beim Exportieren der ARM-Vorlage sind nicht alle zugehörigen Parameter für alle API-Verbindungsressourcen enthalten, die von Ihren Workflows verwendet werden. Weitere Informationen finden Sie unter [Suchen von API-Verbindungsparametern](#find-api-connection-parameters). |
||||

<a name="api-connection-resources"></a>

## <a name="api-connection-resources-and-access-policies"></a>API-Verbindungsressourcen und Zugriffsrichtlinien

In Azure Logic Apps-Instanzen mit nur einem Mandanten muss für jede verwaltete oder API-Verbindungsressource in Ihren Workflows eine zugeordnete Zugriffsrichtlinie vorhanden sein. Diese Richtlinie benötigt die Identität Ihrer Logik-App, um die richtigen Berechtigungen für den Zugriff auf die verwaltete Connectorinfrastruktur bereitzustellen. Die enthaltenen Beispielprojekte beinhalten eine ARM-Vorlage, die alle erforderlichen Infrastrukturressourcen enthält, einschließlich dieser Zugriffsrichtlinien.

Das folgende Diagramm zeigt die Abhängigkeiten zwischen Ihrem Logik-App-Projekt und den Infrastrukturressourcen:

![Konzeptionelles Diagramm der Infrastrukturabhängigkeiten für ein Logik-App-Projekt im Azure Logic Apps-Modell mit nur einem Mandanten.](./media/set-up-devops-deployment-single-tenant-azure-logic-apps/infrastructure-dependencies.png)

<a name="find-api-connection-parameters"></a>

### <a name="find-api-connection-parameters"></a>Suchen von API-Verbindungsparametern

Wenn Ihre Workflows verwaltete API-Verbindungen verwenden, werden bei Verwendung der Exportvorlagenfunktion nicht alle zugehörigen Parameter eingeschlossen. In einer ARM-Vorlage hat jede [API-Verbindungsressourcendefinition](logic-apps-azure-resource-manager-templates-overview.md#connection-resource-definitions) das folgende allgemeine Format:

```json
{
   "type": "Microsoft.Web/connections",
   "apiVersion": "2016–06–01",
   "location": "[parameters('location')]",
   "name": "[parameters('connectionName')]",
   "properties": {}
}
```

Um die Werte zu ermitteln, die Sie im `properties`-Objekt zum Abschließen der Verbindungsressourcendefinition verwenden müssen, können Sie die folgende API für einen bestimmten Connector verwenden:

`PUT https://management.azure.com/subscriptions/{subscription-ID}/providers/Microsoft.Web/locations/{location}/managedApis/{connector-name}?api-version=2018–07–01-preview`

Suchen Sie in der Antwort nach dem `connectionParameters`-Objekt, das alle Informationen enthält, die Sie zum Abschließen der Ressourcendefinition für diesen spezifischen Connector benötigen. Das folgende Beispiel zeigt eine Beispielressourcendefinition für eine verwaltete SQL-Verbindung:

```json
{
   "type": "Microsoft.Web/connections",
   "apiVersion": "2016–06–01",
   "location": "[parameters('location')]",
   "name": "[parameters('connectionName')]",
   "properties": {
      "displayName": "sqltestconnector",
      "api": {
         "id": "/subscriptions/{subscription-ID}/providers/Microsoft.Web/locations/{location}/managedApis/sql"
      },
      "parameterValues": {
         "authType": "windows", 
         "database": "TestDB",
         "password": "TestPassword",
         "server": "TestServer",
         "username": "TestUserName"
      }
   }
}
```

Alternativ können Sie die Netzwerkablaufverfolgung überprüfen, wenn Sie eine Verbindung im Logik-App-Designer erstellen. Suchen Sie den `PUT`-Aufruf an die verwaltete API für den Connector, wie zuvor beschrieben, und überprüfen Sie den Anforderungstext auf alle benötigten Informationen.

## <a name="deploy-logic-app-resources-zip-deploy"></a>Bereitstellen von Logik-App-Ressourcen (ZIP-Bereitstellung)

Nachdem Sie Ihr Logik-App-Projekt in Ihr Quellrepository gepusht haben, können Sie Build- und Releasepipelines einrichten, die Logik-Apps in der Infrastruktur innerhalb oder außerhalb von Azure bereitstellen.

### <a name="build-your-project"></a>Erstellen Ihres Projekts

Um eine Buildpipeline auf Grundlage Ihres Logik-App-Projekttyps einzurichten, führen Sie die entsprechenden Aktionen aus:

* NuGet-basiert: Die NuGet-basierte Projektstruktur basiert auf .NET Framework. Um diese Projekte zu erstellen, stellen Sie sicher, dass Sie die Buildschritte für .NET Standard einhalten. Weitere Informationen finden Sie in der Dokumentation zu [Erstellen eines NuGet-Pakets mit MSBuild](/nuget/create-packages/creating-a-package-msbuild).

* Paketbasiert: Das paketbasierte Erweiterungsprojekt ist nicht sprachspezifisch und erfordert keine sprachspezifischen Buildschritte. Sie können eine beliebige Methode verwenden, um Ihre Projektdateien zu zippen.

  > [!IMPORTANT]
  > Stellen Sie sicher, dass die ZIP-Datei alle Workflowordner, Konfigurationsdateien wie host.json und connections.json sowie alle anderen zugehörigen Dateien enthält.

### <a name="release-to-azure"></a>Release in Azure

Um eine Releasepipeline einzurichten, die in Azure bereitstellt, wählen Sie die zugehörige Option für GitHub, Azure DevOps oder Azure CLI aus.

> [!NOTE]
> Azure Logic Apps unterstützt derzeit keine Azure-Bereitstellungsslots.

#### <a name="github"></a>[GitHub](#tab/github)

Für GitHub-Bereitstellungen können Sie Ihre Logik-App mithilfe von [GitHub Actions](https://docs.github.com/actions) bereitstellen, z. B. die GitHub-Aktion in Azure Functions. Diese Aktion erfordert, dass Sie die folgenden Informationen übergeben:

* Ihr Buildartefakt
* Den Namen der für die Bereitstellung zu verwendenden Logik-App
* Ihr Veröffentlichungsprofil

```yaml
- name: 'Run Azure Functions Action'
  uses: Azure/functions-action@v1
  id: fa
  with:
   app-name: {your-logic-app-name}
   package: '{your-build-artifact}.zip'
   publish-profile: {your-logic-app-publish-profile}
```

Weitere Informationen finden Sie in der Dokumentation zu [Continuous Delivery mithilfe einer GitHub-Aktion](../azure-functions/functions-how-to-github-actions.md).

#### <a name="azure-devops"></a>[Azure DevOps](#tab/azure-devops)

Für Azure DevOps-Bereitstellungen können Sie Ihre Logik-App mithilfe der [Azure Function-App-Bereitstellungsaufgabe (app deploy)](/devops/pipelines/tasks/deploy/azure-function-app) in Azure Pipelines bereitstellen. Diese Aktion erfordert, dass Sie die folgenden Informationen übergeben:

* Ihr Buildartefakt
* Den Namen der für die Bereitstellung zu verwendenden Logik-App
* Ihr Veröffentlichungsprofil

```yaml
- task: AzureFunctionApp@1
  displayName: 'Deploy logic app workflows'
  inputs:
     azureSubscription: '{your-service-connection}'
     appType: 'workflowapp'
     appName: '{your-logic-app-name}'
     package: '{your-build-artifact}.zip'
     deploymentMethod: 'zipDeploy'
```

Weitere Informationen finden Sie in der Dokumentation zum [Bereitstellen einer Azure-Funktion mittels Azure Pipelines](/devops/pipelines/targets/azure-functions-windows).

#### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wenn Sie andere Bereitstellungstools verwenden, können Sie Ihre Logik-App bereitstellen, indem Sie die Azure CLI-Befehle für Azure Logic Apps-Instanzen mit nur einem Mandanten verwenden. Führen Sie beispielsweise den folgenden CLI-Befehl aus, um Ihr gezipptes Artefakt in einer Azure-Ressourcengruppe bereitzustellen:

`az logicapp deployment source config-zip -g {your-resource-group} --name {your-logic-app-name} --src {your-build-artifact}.zip`

---

### <a name="release-to-containers"></a>Release in Containern

Wenn Sie Ihre Logik-App containerisieren, funktioniert die Bereitstellung größtenteils wie jeder andere Container, den Sie bereitstellen und verwalten.

Beispiele für die Implementierung einer End-to-End-Containerbuild- und -bereitstellungspipeline finden Sie unter [CI/CD für Container](https://azure.microsoft.com/solutions/architecture/cicd-for-containers/).

## <a name="next-steps"></a>Nächste Schritte

* [DevOps-Bereitstellung für Azure Logic Apps-Instanzen mit nur einem Mandanten](devops-deployment-single-tenant-azure-logic-apps.md)

Teilen Sie uns bitte Ihre Erfahrungen mit den Azure Logic Apps-Instanzen mit nur einem Mandanten mit.

- Bei Fehlern oder Problemen [erstellen Sie Ihre Probleme in GitHub](https://github.com/Azure/logicapps/issues).
- Bei Fragen, Anforderungen, Kommentaren und anderem Feedback [verwenden Sie dieses Feedbackformular](https://aka.ms/logicappsdevops).