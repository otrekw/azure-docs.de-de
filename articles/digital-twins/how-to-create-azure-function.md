---
title: Einrichten einer Funktion in Azure zum Verarbeiten von Daten
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie eine Funktion in Azure erstellen, die auf digitale Zwillinge zugreifen und von diesen ausgelöst werden kann.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 4889744347b72603a0f6318f981bc2db4906b835
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102433538"
---
# <a name="connect-function-apps-in-azure-for-processing-data"></a>Verbinden von Funktions-Apps in Azure für die Verarbeitung von Daten

Die Aktualisierung von digitalen Zwillingen auf der Grundlage von Daten wird unter Verwendung von [**Ereignisrouten**](concepts-route-events.md) über Computeressourcen, z. B. eine mithilfe von [Azure Functions](../azure-functions/functions-overview.md) erstellte Funktion, durchgeführt. Azure Functions kann verwendet werden, um als Reaktion auf Folgendes einen digitalen Zwilling zu aktualisieren:
* Von IoT Hub stammende Gerätetelemetriedaten
* Änderung von Eigenschaften oder anderen Daten, die von einem anderen digitalen Zwilling innerhalb des Zwillingsgraphen stammen

Dieser Artikel führt Sie durch die Erstellung einer Funktion in Azure zur Verwendung mit Azure Digital Twins. 

Hier finden Sie eine Übersicht über die darin enthaltenen Schritte:

1. Erstellen eines Azure Functions-Projekts in Visual Studio
2. Schreiben Sie eine Funktion mit einem [Event Grid](../event-grid/overview.md)-Trigger.
3. Hinzufügen von Authentifizierungscode zur Funktion (für den Zugriff auf Azure Digital Twins)
4. Veröffentlichen der Funktions-App in Azure
5. Richten Sie [Sicherheitszugriff](concepts-security.md) für die Funktions-App ein.

## <a name="prerequisite-set-up-azure-digital-twins-instance"></a>Voraussetzung: Einrichten einer Azure Digital Twins-Instanz

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="create-a-function-app-in-visual-studio"></a>Erstellen einer Funktions-App in Visual Studio

Wählen Sie in Visual Studio 2019 _Datei > Neu > Projekt_ aus, und suchen Sie nach der Vorlage _Azure Functions_. Wählen Sie _Weiter_ aus.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld „Neues Projekt“ mit hervorgehobener Azure Functions-Projektvorlage":::

Geben Sie einen Namen für die Funktions-App an, und wählen Sie _Erstellen_ aus.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld zum Konfigurieren eines neues Projekts, einschließlich des Projektnamens, des Speicherorts, der Auswahl zum Erstellen einer neuen Projektmappe und des Projektmappennamens":::

Wählen Sie den Typ der Funktions-App *Event Grid-Trigger* und dann _Erstellen_ aus.

:::image type="content" source="media/how-to-create-azure-function/event-grid-trigger-function.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld zum Erstellen einer neuen Azure Functions-Anwendung mit hervorgehobener Event Grid-Triggeroption":::

Nachdem Ihre Funktions-App erstellt wurde, generiert Visual Studio ein Codebeispiel in der Datei **Function1.cs** in Ihrem Projektordner. Diese kurze Funktion wird zur Protokollierung von Ereignissen verwendet.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Screenshot von Visual Studio im Projektfenster für das neue Projekt, das erstellt wurde, mit einem Code für eine Beispielfunktion namens „Function1“" lightbox="media/how-to-create-azure-function/visual-studio-sample-code.png":::

## <a name="write-a-function-with-an-event-grid-trigger"></a>Schreiben einer Funktion mit einem Event Grid-Trigger

Sie können eine Funktion schreiben, indem Sie das SDK zu Ihrer Funktions-App hinzufügen. Die Funktions-App interagiert mit Azure Digital Twins unter Verwendung des [Azure Digital Twins-SDKs für .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client). 

Wenn Sie das SDK verwenden möchten, müssen Sie die folgenden Pakete in Ihr Projekt einbinden. Sie können die Pakete entweder mit dem NuGet-Paket-Manager von Visual Studio installieren oder die Pakete mithilfe von `dotnet` in einem Befehlszeilentool hinzufügen.

* [Azure.DigitalTwins.Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core/)
* [Azure.Identity](https://www.nuget.org/packages/Azure.Identity/)
* [System.Net.Http](https://www.nuget.org/packages/System.Net.Http/)
* [Azure.Core](https://www.nuget.org/packages/Azure.Core/)

Öffnen Sie als Nächstes in Ihrem Visual Studio-Projektmappen-Explorer die Datei _Function1.cs_, die den Beispielcode enthält, und fügen Sie Ihrer Funktion die folgenden `using`-Anweisungen für diese Pakete hinzu. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>Hinzufügen von Authentifizierungscode zur Funktion

Sie werden jetzt Variablen auf Klassenebene deklarieren und einen Authentifizierungscode hinzufügen, der der Funktion den Zugriff auf Azure Digital Twins ermöglicht. Sie fügen Ihrer Funktion in der Datei _Function1.cs_ Folgendes hinzu.

* Code zum Lesen der Dienst-URL für Azure Digital Twins als **Umgebungsvariable**. Es ist eine bewährte Methode, die Dienst-URL aus einer Umgebungsvariablen auszulesen, anstatt dafür die Hartcodierung in der Funktion zu verwenden. Sie legen den Wert dieser Umgebungsvariablen [weiter unten](#set-up-security-access-for-the-function-app) fest. Weitere Informationen zu Umgebungsvariablen finden Sie unter [*Verwalten Ihrer Funktions-App*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal).

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ADT_service_URL":::

* Eine statische Variable zum Aufnehmen einer HttpClient-Instanz Die Erstellung von HttpClient ist relativ aufwendig, und wir möchten vermeiden, dies für jeden Funktionsaufruf durchführen zu müssen.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="HTTP_client":::

* Sie können die Anmeldeinformationen für die verwaltete Identität in Azure Functions verwenden.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ManagedIdentityCredential":::

* Fügen Sie die lokale Variable _DigitalTwinsClient_ innerhalb Ihrer Funktion hinzu, um Ihre Azure Digital Twins-Clientinstanz aufzunehmen. Verwenden Sie hierfür *keine* statische Variable innerhalb Ihrer Klasse.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="DigitalTwinsClient":::

* Fügen Sie eine NULL-Überprüfung für _adtInstanceUrl_ hinzu, und umschließen Sie Ihre Funktionslogik mit einem Try-Catch-Block, um alle Ausnahmen abzufangen.

Nach diesen Änderungen ähnelt Ihr Funktionscode dem folgenden:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs":::

Nachdem Sie Ihre Anwendung geschrieben haben, können Sie sie mithilfe der Schritte im nächsten Abschnitt in Azure veröffentlichen.

## <a name="publish-the-function-app-to-azure"></a>Veröffentlichen der Funktions-App in Azure

[!INCLUDE [digital-twins-publish-azure-function.md](../../includes/digital-twins-publish-azure-function.md)]

## <a name="set-up-security-access-for-the-function-app"></a>Einrichten des Sicherheitszugriffs für die Funktions-App

Sie können den Sicherheitszugriff für die Funktions-App mithilfe der Azure CLI oder des Azure-Portals einrichten. Führen Sie die folgenden Schritte für Ihre bevorzugte Option aus.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

Sie können diese Befehle in [Azure Cloud Shell](https://shell.azure.com) oder einer [lokalen Azure CLI-Installation](/cli/azure/install-azure-cli) ausführen.

### <a name="assign-access-role"></a>Zuweisen der Zugriffsrolle

Das Funktionsgerüst aus früheren Beispielen erfordert, dass ihm ein Bearertoken übergeben wird, um sich bei Azure Digital Twins authentifizieren zu können. Um sicherzustellen, dass dieses Bearertoken übergeben wird, müssen Sie für die Funktions-App Berechtigungen vom Typ [Verwaltete Dienstidentität (Managed Service Identity, MSI)](../active-directory/managed-identities-azure-resources/overview.md) einrichten, damit der Zugriff auf Azure Digital Twins möglich ist. Dies muss nur einmal für jede Funktions-App durchgeführt werden.

Sie können die systemseitig verwaltete Identität der Funktions-App verwenden, um ihr die Rolle _**Azure Digital Twins-Datenbesitzer**_ für Ihre Azure Digital Twins-Instanz zuzuweisen. Dadurch erhält die Funktions-App die Berechtigung in der Instanz zum Ausführen von Aktivitäten auf Datenebene. Dann gestalten Sie die URL Ihrer Azure Digital Twins-Instanz für Ihre Funktion zugänglich, indem Sie eine Umgebungsvariable festlegen.

1. Verwenden Sie den folgenden Befehl, um die Details der systemseitig verwalteten Identität für die Funktion anzuzeigen. Beachten Sie in der Ausgabe das Feld _principalId_.

    ```azurecli-interactive 
    az functionapp identity show -g <your-resource-group> -n <your-App-Service-(function-app)-name> 
    ```

    >[!NOTE]
    > Falls die Ergebnisanzeige leer ist und die Details einer Identität nicht angezeigt werden, sollten Sie mit diesem Befehl eine neue systemseitig verwaltete Identität erstellen:
    > 
    >```azurecli-interactive    
    >az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>  
    >```
    >
    > In der Ausgabe werden dann Details zur Identität angezeigt, z. B. der für den nächsten Schritt benötigte Wert für _principalId_. 

1. Verwenden Sie den Wert _principalId_ im folgenden Befehl, um die Identität der Funktions-App der Rolle _Azure Digital Twins Data Owner_ (Azure Digital Twins-Datenbesitzer) für Ihre Azure Digital Twins-Instanz zuzuweisen.

    ```azurecli-interactive 
    az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
    ```

### <a name="configure-application-settings"></a>Konfigurieren von Anwendungseinstellungen

Machen Sie die URL Ihrer Azure Digital Twins-Instanz abschließend für Ihre Funktion zugänglich, indem Sie dafür eine **Umgebungsvariable** festlegen. Weitere Informationen zu Umgebungsvariablen finden Sie unter [*Verwalten Ihrer Funktions-App*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

> [!TIP]
> Die URL der Azure Digital Twins-Instanz wird durch das Hinzufügen von *https://* am Anfang des *Hostnamens* der Azure Digital Twins-Instanz erstellt. Um den Hostnamen mit allen Eigenschaften der Instanz anzuzeigen, können Sie Folgendes ausführen: `az dt show --dt-name <your-Azure-Digital-Twins-instance>`.

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-host-name>"
```

# <a name="azure-portal"></a>[Azure-Portal](#tab/portal)

Führen Sie im [Azure-Portal](https://portal.azure.com/) die folgenden Schritte aus.

### <a name="assign-access-role"></a>Zuweisen der Zugriffsrolle

Azure-Ressourcen können über eine systemseitig zugewiesene verwaltete Identität bei Clouddiensten (z. B. Azure Key Vault) authentifiziert werden, ohne dass Anmeldeinformationen im Code gespeichert werden. Einmal aktiviert, können alle erforderlichen Berechtigungen über die rollenbasierte Zugriffssteuerung von Azure erteilt werden. Der Lebenszyklus dieser Art verwalteter Identitäten ist an den Lebenszyklus dieser Ressource gebunden. Zusätzlich kann jede Ressource nur eine systemseitig zugewiesene verwaltete Identität aufweisen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach Ihrer Funktions-App, indem Sie in der Suchleiste ihren Namen eingeben. Wählen Sie in den Ergebnissen Ihre App aus. 

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Screenshot des Azure-Portals: Der Name der Funktions-App wird in der Suchleiste des Portals gesucht, und das Suchergebnis wird hervorgehoben.":::

1. Wählen Sie auf der Seite der Funktions-App links in der Navigationsleiste die Option _Identität_ aus, um eine verwaltete Identität für die Funktion zu verwenden. Vergewissern Sie sich auf der Seite _Systemseitig zugewiesen_, dass der _Status_ auf **Ein** festgelegt ist. (Wenn nicht, sollten Sie dies nun durchführen und *Speichern* auswählen, um die Änderung zu speichern.)

    :::image type="content" source="media/how-to-create-azure-function/verify-system-managed-identity.png" alt-text="Screenshot des Azure-Portals: Seite „Identität“ für die Funktions-App mit Einstellung von „Status“ auf „Ein“" lightbox="media/how-to-create-azure-function/verify-system-managed-identity.png":::

1. Wählen Sie die Schaltfläche _Azure-Rollenzuweisungen_ aus, die die Seite mit den *Azure-Rollenzuweisungen* öffnet.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-1.png" alt-text="Screenshot des Azure-Portals: Eine Hervorhebung um die Schaltfläche „Azure-Rollenzuweisungen“ unter „Berechtigungen“ auf der Identitätsseite der Azure-Funktion" lightbox="media/how-to-create-azure-function/add-role-assignment-1.png":::

    Wählen Sie die Option _+ Rollenzuweisung hinzufügen (Vorschau)_ aus.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-2.png" alt-text="Screenshot des Azure-Portals: Hervorhebung von „+ Rollenzuweisung hinzufügen (Vorschau)“ auf der Seite mit den Azure-Rollenzuweisungen" lightbox="media/how-to-create-azure-function/add-role-assignment-2.png":::

1. Wählen Sie auf der sich öffnenden Seite _Rollenzuweisung hinzufügen (Vorschau)_ die folgenden Werte aus:

    * **Bereich:** Ressourcengruppe
    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie in der Dropdownliste Ihre Ressourcengruppe aus.
    * **Rolle**: Wählen Sie im Dropdownmenü die Option _Azure Digital Twins-Datenbesitzer_ aus.

    Speichern Sie dann Ihre Details, indem Sie auf die Schaltfläche _Speichern_ klicken.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-3.png" alt-text="Screenshot des Azure-Portals: Dialog zum Hinzufügen einer neuen Rollenzuweisung (Vorschau), einschließlich Felder für den Bereich, das Abonnement, die Ressourcengruppe und die Rolle":::

### <a name="configure-application-settings"></a>Konfigurieren von Anwendungseinstellungen

Um die URL Ihrer Azure Digital Twins-Instanz für Ihre Funktion zugänglich zu machen, können Sie dafür eine **Umgebungsvariable** festlegen. Weitere Informationen zu Umgebungsvariablen finden Sie unter [*Verwalten Ihrer Funktions-App*](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). Anwendungseinstellungen werden als Umgebungsvariablen für den Zugriff auf die Azure Digital Twins-Instanz verfügbar gemacht. 

Rufen Sie zunächst die URL ab, indem Sie den Hostnamen Ihrer Azure Digital Twins-Instanz suchen, um dann eine Umgebungsvariable mit der URL dieser Instanz festzulegen. Suchen Sie nach Ihrer Instanz in der Suchleiste des [Azure-Portals](https://portal.azure.com). Wählen Sie dann _Übersicht_ in der linken Navigationsleiste aus, um den _Hostnamen_ anzuzeigen. Kopieren Sie diesen Wert.

:::image type="content" source="media/how-to-create-azure-function/instance-host-name.png" alt-text="Screenshot des Azure-Portals: Hervorgehobener Wert für den Hostnamen auf der Übersichtsseite für die Instanz von Azure Digital Twins":::

Sie können mit diesen Schritten jetzt eine Anwendungseinstellung erstellen:

1. Suchen Sie in der Suchleiste des Portals nach Ihrer Funktions-App, und wählen Sie sie aus den Ergebnissen aus.

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Screenshot des Azure-Portals: Der Name der Funktions-App wird in der Suchleiste des Portals gesucht, und das Suchergebnis wird hervorgehoben.":::

1. Wählen Sie links in der Navigationsleiste die Option _Konfiguration_ aus. Wählen Sie auf der Registerkarte _Anwendungseinstellungen_ die Option _+ Neue Anwendungseinstellung_ aus.

    :::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Screenshot des Azure-Portals: Hervorgehobene Schaltfläche zum Erstellen einer neuen Anwendungseinstellung auf der Konfigurationsseite für die Funktions-App":::

1. Verwenden Sie in dem sich öffnenden Fenster den oben kopierten Hostnamenwert, um eine Anwendung zu erstellen.
    * **Name**: ADT_SERVICE_URL
    * **Value**: https://{your-azure-digital-twins-host-name}
    
    Wählen Sie _OK_ aus, um eine Anwendungseinstellung zu erstellen.
    
    :::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Screenshot des Azure-Portals: Hervorgehobene „OK“-Schaltfläche nach Ausfüllen der Felder „Name“ und „Value“ auf der Seite „Hinzufügen/Bearbeiten“ der Anwendungseinstellung":::

1. Nach dem Erstellen der Einstellung sollte diese auf der Registerkarte _Anwendungseinstellungen_ angezeigt werden. Vergewissern Sie sich, dass *ADT_SERVICE_URL* in der Liste angezeigt wird, und speichern Sie anschließend die neue Anwendungseinstellung, indem Sie die Schaltfläche _Speichern_ auswählen.

    :::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Screenshot des Azure-Portals: Hervorgehobene Anwendungseinstellungsseite mit neuer ADT_SERVICE_URL-Einstellung. Die Schaltfläche „Speichern“ ist ebenfalls hervorgehoben.":::

1. Bei allen Änderungen der Anwendungseinstellungen muss die Anwendung neu gestartet werden, damit diese wirksam werden. Wählen Sie daher _Weiter_ aus, um Ihre Anwendung neu zu starten, wenn die entsprechende Aufforderung angezeigt wird.

    :::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Screenshot des Azure-Portals: Benachrichtigung, dass für Änderungen an Anwendungseinstellungen ein Neustart Ihrer Anwendung vorgenommen wird. Die Schaltfläche „Weiter“ ist hervorgehoben.":::

---

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die Schritte zum Einrichten einer Funktions-App in Azure für die Verwendung mit Azure Digital Twins befolgt.

Als Nächstes erfahren Sie, wie Sie auf Ihrer grundlegenden Funktion aufbauen können, um IoT Hub-Daten in Azure Digital Twins zu erfassen:
* [*Verwenden Erfassen von Telemetriedaten aus IoT Hub*](how-to-ingest-iot-hub-data.md)
