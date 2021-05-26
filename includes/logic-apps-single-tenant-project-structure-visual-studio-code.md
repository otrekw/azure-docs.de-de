---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 05/25/2021
ms.openlocfilehash: 29b1bbfab92ffbb97086f01c23913f01aaac1d05
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110369109"
---
In Visual Studio Code verfügt Ihr Logik-App-Projekt über einen der folgenden Typen:

* Erweiterungs-Bundle-basiert (Node.js), was der Standardtyp ist
* NuGet-paketbasiertes (.NET), das Sie vom Standardtyp konvertieren können

Basierend auf diesen Typen enthält Ihr Projekt leicht unterschiedliche Ordner und Dateien. Ein NuGet-basiertes Projekt enthält einen .bin-Ordner, der Pakete und andere Bibliotheksdateien enthält. Ein Bundle-basiertes Projekt enthält nicht den .bin Ordner und andere Dateien. Einige Szenarien erfordern ein NuGet-basiertes Projekt, damit Ihre App ausgeführt werden kann, z. B. wenn Sie benutzerdefinierte integrierte Vorgänge entwickeln und ausführen möchten. Weitere Informationen zum Konvertieren Ihres Projekts zur Verwendung von NuGet finden Sie unter [Aktivieren der Erstellung Connector-Entwicklung](../articles/logic-apps/create-single-tenant-workflows-visual-studio-code.md#enable-built-in-connector-authoring).

Für das standardmäßige Bundle-basierte Projekt verfügt Ihr Projekt über eine Ordner- und Dateistruktur, die dem folgenden Beispiel ähnelt:

```text
MyBundleBasedLogicAppProjectName
| .vscode
| Artifacts
  || Maps 
     ||| MapName1
     ||| ...
  || Schemas
     ||| SchemaName1
     ||| ...
| WorkflowName1
  || workflow.json
  || ...
| WorkflowName2
  || workflow.json
  || ...
| workflow-designtime
| .funcignore
| connections.json
| host.json
| local.settings.json
```

Auf der Stammebene Ihres Projekts finden Sie die folgenden Dateien und Ordner mit anderen Elementen:

| Name | Ordner oder Datei | Beschreibung |
|------|----------------|-------------|
| **.vscode** | Ordner | Enthält Visual Studio Code Einstellungsdateien, z. B. extensions.json, launch.json, settings.json und tasks.json Dateien. |
| **Artefakte** | Ordner | Enthält Integrationskontoartefakte, die Sie definieren und in Workflows verwenden, die Business-to-Business-Szenarien (B2B-Szenarien) unterstützen. Die Beispielstruktur enthält beispielsweise Zuordnungen und Schemas für XML-Transformations- und -Validierungsvorgänge. |
| **<WorkflowName>** | Ordner | Für jeden Workflow enthält der <WorkflowName>-Ordner eine workflow.json Datei, die die zugrunde liegende JSON-Definition dieses Workflows enthält. |
| **workflow-designtime** | Ordner | Enthält Entwicklungsumgebungsbezogene Einstellungsdateien. |
| **.funcignore** | File | Enthält Informationen zu Ihren installierten [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md). |
| **connections.json** | File | Enthält die Metadaten, Endpunkte und Schlüssel für alle verwalteten Verbindungen und Azure-Funktionen, die von Ihren Workflows verwendet werden. <p><p>**Wichtiger Hinweis**: Um unterschiedliche Verbindungen und Funktionen in jeder Umgebung zu verwenden, stellen Sie sicher, dass Sie die Datei **connections.json** parametrisieren und die Endpunkte aktualisieren. |
| **host.json** | File | Enthält laufzeitspezifische Konfigurationseinstellungen und -werte, z. B. die Standardgrenzwerte für die Azure Logic Apps-Einzelmandanten-Plattform, Logik-Apps, Workflows, Auslöser und Aktionen. Auf der Stammebene Ihres Logik-App-Projekts enthält die **host.json** Metadatendatei die Konfigurationseinstellungen und Standardwerte, die *alle Workflows* in derselben Logik-App während der Ausführung verwenden, ob lokal oder in Azure. |
| **local.settings.json** | File | Enthält App-Einstellungen, Verbindungszeichenfolgen und andere Einstellungen, die Ihre Workflows bei der lokalen Ausführung verwenden. Anders ausgedrückt, geltend diese Einstellungen und Werte *nur*, wenn Sie Ihre Projekte in Ihrer lokalen Entwicklungsumgebung ausführen. Während der Bereitstellung in Azure werden die Datei und die Einstellungen ignoriert und nicht in Ihre Bereitstellung einbezogen. <p><p>Diese Datei speichert Einstellungen und Werte als *lokale Umgebungsvariablen*, die von Ihren lokalen Bereitstellungstools als `appSettings`-Werte verwendet werden. Sie können diese Umgebungsvariablen sowohl zur Laufzeit als auch zur Bereitstellungszeit aufrufen und beziehen, indem Sie *App-Einstellungen* und *Parameter* verwenden. <p><p>**Wichtiger Hinweis**: Die Datei **local.settings.json** kann Geheimnisse enthalten. Stellen Sie daher sicher, dass Sie diese Datei auch aus der Quellcodeverwaltung Ihres Projekts ausschließen. |
||||