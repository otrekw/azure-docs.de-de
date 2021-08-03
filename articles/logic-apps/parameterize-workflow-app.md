---
title: Erstellen von Parametern für Workflows in Azure Logic Apps-Instanzen mit nur einem Mandanten
description: Definieren Sie Parameter für Werte, die sich in Workflows zwischen Bereitstellungsumgebungen in Azure Logic Apps-Instanzen mit nur einem Mandanten unterscheiden.
services: logic-apps
ms.suite: integration
ms.reviewer: azla
ms.topic: how-to
ms.date: 06/08/2021
ms.openlocfilehash: 7de89605f86e47b3062ec07160288ab14584f42e
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111747417"
---
# <a name="create-parameters-for-values-that-change-in-workflows-across-environments-for-single-tenant-azure-logic-apps"></a>Erstellen von Parametern für Werte, die sich in Workflows zwischen Umgebungen für Azure Logic Apps-Instanzen mit nur einem Mandanten ändern

In Azure Logic Apps können Sie Parameter verwenden, um Werte zu abstrahieren, die sich zwischen Umgebungen ändern könnten. Indem Sie Parameter zur Verwendung in Ihren Workflows definieren, können Sie sich zunächst auf das Entwerfen Ihrer Workflows konzentrieren und Ihre umgebungsspezifischen Variablen dann später einfügen.

In *mehrinstanzenfähigen* Azure Logic Apps-Instanzen können Sie Parameter im Workflow-Designer erstellen und darauf verweisen und dann die Variablen in Ihrer ARM-Vorlage (Azure Resource Manager) und den Parameterdateien festlegen. Parameter werden bei der Bereitstellung definiert und festgelegt. Auch wenn Sie nur eine Variable ändern müssen, müssen Sie die ARM-Vorlage Ihrer Logik-App erneut bereitstellen.

In Azure Logic Apps-Instanzen *mit nur einem Mandanten* können Sie mit Parametern und App-Einstellungen sowohl zur Runtime als auch zur Bereitstellungszeit mit Umgebungsvariablen arbeiten. In diesem Artikel erfahren Sie, wie Sie Umgebungsvariablen mit der neuen Umgebung mit den neuen Parametern für einen einzelnen Mandanten bearbeiten, aufrufen und darauf verweisen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto und ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Ein [Logik-App-Workflow, der in einer Azure Logic Apps-Instanz mit nur einem Mandanten gehostet wird](single-tenant-overview-compare.md).

  Wenn Sie keine Logik-App haben, [erstellen Sie Ihre Logik-App (Standard) im Azure-Portal](create-single-tenant-workflows-azure-portal.md) oder [in Visual Studio Code](create-single-tenant-workflows-visual-studio-code.md).

## <a name="parameters-versus-app-settings"></a>Parameter im Vergleich zu App-Einstellungen

Bevor Sie entscheiden, wo Ihre Umgebungsvariablen gespeichert werden, lesen Sie die folgenden Informationen.

Wenn Sie bereits Azure Functions oder Azure-Web-Apps verwenden, sind Sie möglicherweise mit App-Einstellungen vertraut. In Azure Logic Apps werden App-Einstellungen in Azure Key Vault integriert. Sie können [direkt auf sichere Zeichenfolgen verweisen](../app-service/app-service-key-vault-references.md) (z. B. Verbindungszeichenfolgen und Schlüssel). Ähnlich wie bei ARM-Vorlagen, bei denen Sie Umgebungsvariablen zum Bereitstellungszeitpunkt definieren können, können Sie App-Einstellungen in der [Workflowdefinition für Ihre Logik-App](/azure/templates/microsoft.logic/workflows) definieren. Anschließend können Sie dynamisch generierte Infrastrukturwerte erfassen (z. B. Verbindungsendpunkte oder Speicherzeichenfolgen). App-Einstellungen haben jedoch Größenbeschränkungen, und in bestimmten Azure Logic Apps-Bereichen kann nicht auf sie verwiesen werden.

Wenn Sie mit Workflows in mehrinstanzenfähigen Azure Logic Apps-Instanzen vertraut sind, sind Sie möglicherweise auch mit Parametern vertraut. Sie können Parameter in einem größeren Bereich von Anwendungsfällen als App-Einstellungen verwenden, z. B. zur Unterstützung von komplexen Objekten und Werten großer Größen. Wenn Sie Visual Studio Code als lokales Entwicklungstool verwenden, können Sie auch auf Parameter in den Dateien **workflow.json** und **connections.json** Ihres Logik-App-Projekts verweisen. Wenn Sie beide Optionen in Ihrer Lösung verwenden möchten, können Sie auch mithilfe von Parametern auf App-Einstellungen verweisen.

> [!NOTE]
> Wenn Sie für die Entwicklung die **connections.json**-Datei parametrisieren, wird die Designererfahrung sowohl lokal als auch im Azure-Portal eingeschränkt. Wenn Sie den Designer für die Entwicklung verwenden müssen, verwenden Sie eine nicht parametrisierte **connections.json**-Datei. Ersetzen Sie sie dann in Ihren Bereitstellungspipelines durch die parametrisierte Datei. Die Runtime funktioniert weiterhin mit Parametrisierung. Designerverbesserungen befinden sich in der Entwicklung.

Berücksichtigen Sie die Empfehlung, Parameter als Standardmechanismus für die Parametrisierung zu verwenden. Wenn Sie sichere Schlüssel oder Zeichenfolgen speichern müssen, können Sie auf diese Weise die Empfehlung befolgen, von Ihren Parametern aus auf App-Einstellungen zu verweisen.

## <a name="what-is-parameterization"></a>Was ist Parametrisierung?

Wenn Sie Visual Studio Code verwenden, können Sie in Ihrem Logik-App-Projekt Parameter in der **parameters.json**-Datei definieren. Sie können von einem beliebigen Workflow- oder Verbindungsobjekt in Ihrer Logik-App aus auf jeden Parameter in der **parameters.json**-Datei verweisen. Das Parametrisieren Ihrer Workfloweingaben in Azure Logic Apps-Instanzen mit nur einem Mandanten funktioniert ähnlich wie bei mehrinstanzenfähigen Azure Logic Apps-Instanzen.

Um auf Parameter in Ihren Trigger- oder Aktionseingaben zu verweisen, verwenden Sie den Ausdruck `@parameters('<parameter-name>')`.

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie auch alle Parameter, auf die Sie verweisen, in Ihre **parameters.json**-Datei einschließen.

In Azure Logic Apps-Instanzen *mit nur einem Mandanten* können Sie verschiedene Teile Ihrer **connections.json**-Datei parametrisieren. Anschließend können Sie Ihre **connections.json**-Datei in der Quellcodeverwaltung überprüfen und dann alle Verbindungen über Ihre **parameters.json**-Datei verwalten. Ersetzen Sie zum Parametrisieren Ihrer **connections.json**-Datei die Werte für Literale wie `ConnectionRuntimeUrl` durch einen einzelnen `parameters()`-Ausdruck, z. B. `@parameters('api-runtimeUrl')`.

Sie können auch komplexe Objekte wie das `authentication`-JSON-Objekt parametrisieren. Ersetzen Sie beispielsweise den `authentication`-Objektwert durch eine Zeichenfolge, die einen einzelnen Parameterausdruck enthält, z. B. `@parameters('api-auth')`. 

> [!NOTE]
> Die einzigen gültigen Ausdruckstypen in der **connections.json**-Datei sind `@parameters` und `@appsetting`.

## <a name="define-parameters"></a>Definieren von Parametern

In Workflows, die auf einem einzelnen Mandanten basieren, müssen Sie alle Parameterwerte in einer JSON-Datei auf Stammebene mit dem Namen **parameters.json** speichern. Diese Datei enthält ein Objekt, das Schlüssel-Wert-Paare enthält. Die Schlüssel sind die Namen der einzelnen Parameter, und die Werte sind deren Strukturen. Jede Struktur muss sowohl eine `type`- als auch eine `value`-Deklaration enthalten.

> [!NOTE]
> Der einzige gültige Ausdruckstyp in der **parameters.json**-Datei ist `@appsetting`.

Das folgende Beispiel zeigt eine Datei mit grundlegenden Parametern:

```json
{ 
    "responseString": { 
        "type": "string", 
        "value": "hello" 
    }, 
    "functionAuth": { 
        "type": "object", 
        "value": { 
            "type": "QueryString", 
            "name": "Code", 
            "value": "@appsetting('<AzureFunctionsOperation-FunctionAppKey')" 
        } 
    } 
} 
```

In der Regel müssen Sie mehrere Versionen von Parameterdateien verwalten. Möglicherweise benötigen Sie Werte für verschiedene Bereitstellungsumgebungen, z. B. Entwicklung, Tests und Produktion. Das Verwalten dieser Parameterdateien funktioniert häufig wie das Verwalten von ARM-Vorlagenparameterdateien. Wenn Sie in einer bestimmten Umgebung bereitstellen, bringen Sie die entsprechende Parameterdatei in der Regel über eine Pipeline für DevOps ein.

Führen Sie den folgenden Befehl aus, um Parameterdateien dynamisch mit Azure CLI zu ersetzen:

```azurecli
az functionapp deploy --resource-group MyResourceGroup --name MyLogicApp --src-path C:\parameters.json --type static --target-path parameters.json
```

Bei einem NuGet-basierten Logik-App-Projekt müssen Sie Ihre Projektdatei ( **&lt;logic-app-name&gt;.csproj**) so aktualisieren, dass die Parameterdatei in der Buildausgabe enthalten sind, zum Beispiel:
  
```csproj
<ItemGroup>
  <None Update="parameters.json">
    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
  </None>
</ItemGroup>
```

> [!NOTE]
> Derzeit ist die Funktion zum dynamischen Ersetzen von Parameterdateien im Azure-Portal oder Workflow-Designer noch nicht verfügbar.

Weitere Informationen zur Einrichtung Ihrer Logik-Apps für die DevOps-Bereitstellung finden Sie in der folgenden Dokumentation:

- [Übersicht über die DevOps-Bereitstellung für auf einem einzelnen Mandanten basierende Logik-Apps](devops-deployment-single-tenant-azure-logic-apps.md)
- [Einrichten der DevOps-Bereitstellung für auf einem einzelnen Mandanten basierende Logik-Apps](set-up-devops-deployment-single-tenant-azure-logic-apps.md)

## <a name="manage-app-settings"></a>Verwalten von App-Einstellungen

In Azure Logic Apps-Instanzen mit nur einem Mandanten enthalten App-Einstellungen globale Konfigurationsoptionen für *alle Workflows* in derselben Logik-App. Wenn Sie Workflows lokal in Visual Studio Code ausführen, können Sie auf diese Einstellungen in der **local.settings.json**-Datei als lokale Umgebungsvariablen zugreifen. Anschließend können Sie in Ihren Parametern auf diese App-Einstellungen verweisen.

Informationen zum Hinzufügen, Aktualisieren oder Löschen von App-Einstellungen finden Sie in den folgenden Abschnitten zu Visual Studio Code, dem Azure-Portal, der Azure CLI oder ARM-Vorlagen (Bicep).

### <a name="azure-portal"></a>[Azure portal](#tab/azure-portal)

Führen Sie die folgenden Schritte aus, um die App-Einstellungen für Ihre Logik-App im Azure-Portal zu überprüfen:

1. Suchen Sie mithilfe des Suchfelds im [Azure-Portal](https://portal.azure.com/) Ihre auf einem einzelnen Mandanten basierende Logik-App, und öffnen Sie diese.
1. Wählen Sie im Menü Ihrer Logik-App unter **Einstellungen** die Option **Konfiguration** aus.
1. Überprüfen Sie auf der Seite **Konfiguration** auf der Registerkarte **Anwendungseinstellungen** die App-Einstellungen für Ihre Logik-App.
1. Wählen Sie **Werte anzeigen** aus, um alle Werte anzuzeigen. Alternativ können Sie einen einzelnen Wert auswählen, um ihn anzuzeigen.

Führen Sie die folgenden Schritte aus, um eine neue Einstellung hinzuzufügen:

1. Wählen Sie auf der Registerkarte **Anwendungseinstellungen** unter **Anwendungseinstellungen** die Option **Neue Anwendungseinstellung** aus.
1. Geben Sie für **Name** den *Schlüssel* oder Namen für Ihre neue Einstellung ein.
1. Geben Sie für **Wert** den Wert für Ihre neue Einstellung ein.
1. Wenn Sie Ihr neues *Schlüssel-Wert*-Paar erstellen möchten, wählen Sie **OK** aus.

:::image type="content" source="./media/parameterize-workflow-app/portal-app-settings-values.png" alt-text="Screenshot: Azure-Portal und Konfigurationsbereich mit den App-Einstellungen und Werten für eine auf einem einzelnen Mandanten basierende Logik-App." lightbox="./media/parameterize-workflow-app/portal-app-settings-values.png":::

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Führen Sie den Befehl `az logicapp config appsettings list` aus, um Ihre aktuellen App-Einstellungen mithilfe der Azure CLI zu überprüfen. Stellen Sie sicher, dass Ihr Befehl die Parameter `--name -n` und `--resource-group -g` enthält. Beispiel:

```azurecli
az logicapp config appsettings list --name MyLogicApp --resource-group MyResourceGroup
```

Führen Sie den Befehl `az logicapp config appsettings set` aus, um eine App-Einstellung mithilfe der Azure CLI hinzuzufügen oder zu aktualisieren. Stellen Sie sicher, dass Ihr Befehl die Parameter `--name n` und `--resource-group -g` enthält. Der folgende Befehl erstellt beispielsweise eine Einstellung mit einem Schlüssel namens `CUSTOM_LOGIC_APP_SETTING` mit einem Wert von `12345`:

```azurecli
az logicapp config appsettings set --name MyLogicApp --resource-group MyResourceGroup --settings CUSTOM_LOGIC_APP_SETTING=12345 
```

### <a name="resource-manager-or-bicep-template"></a>[Resource Manager- oder Bicep-Vorlage](#tab/azure-resource-manager)

Um Ihre App-Einstellungen in einer ARM- oder Bicep-Vorlage zu überprüfen und zu definieren, suchen Sie die Ressourcendefinition Ihrer Logik-App, und aktualisieren Sie das `appSettings`-JSON-Objekt. Die vollständige Ressourcendefinition finden Sie in der [ARM-Vorlagenreferenz](/azure/templates/microsoft.web/sites).

Dieses Beispiel zeigt Dateieinstellungen für ARM-Vorlagen oder Bicep-Vorlagen:

```json
"appSettings": [
    {
        "name": "string",
        "value": "string"
    },
    {
        "name": "string",
        "value": "string"
    },
    <...>
], 
```

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Umgebung mit nur einem Mandanten (Vorschau) im Vergleich zu mehrinstanzenfähiger Umgebung und Integrationsdienstumgebung für Azure Logic Apps](single-tenant-overview-compare.md)
