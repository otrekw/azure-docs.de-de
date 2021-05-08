---
title: Einrichten einer Funktion in Azure für die Verarbeitung von Daten
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie eine Funktion in Azure erstellen, die auf digitale Zwillinge zugreifen und von diesen ausgelöst werden kann.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5dddaabf47a261f741b3b1cb8d3319d589c4e474
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107480767"
---
# <a name="connect-function-apps-in-azure-for-processing-data"></a>Verbinden von Funktions-Apps in Azure für die Verarbeitung von Daten

Digitale Zwillinge können basierend auf Daten mithilfe von [Ereignisrouten](concepts-route-events.md) über Computeressourcen aktualisiert werden. Beispielsweise kann eine Funktion, die mit [Azure Functions](../azure-functions/functions-overview.md) erstellt wurde, einen digitalen Zwilling als Reaktion auf Folgendes aktualisieren:
* Gerätetelemetriedaten von Azure IoT Hub
* Änderung von Eigenschaften oder anderen Daten, die von einem anderen digitalen Zwilling innerhalb des Zwillingsgraphen stammen

Dieser Artikel führt Sie durch die Erstellung einer Funktion in Azure zur Verwendung mit Azure Digital Twins. Führen Sie die folgenden grundlegenden Schritte aus, um eine Funktion zu erstellen:

1. Erstellen eines Azure Functions-Projekts in Visual Studio
2. Schreiben einer Funktion mit einem [Azure Event Grid](../event-grid/overview.md)-Trigger
3. Hinzufügen von Authentifizierungscode zur Funktion für den Zugriff auf Azure Digital Twins
4. Veröffentlichen der Funktions-App in Azure
5. Einrichten der [Sicherheit](concepts-security.md) für die Funktions-App

## <a name="prerequisite-set-up-azure-digital-twins"></a>Voraussetzung: Einrichten von Azure Digital Twins

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

## <a name="create-a-function-app-in-visual-studio"></a>Erstellen einer Funktions-App in Visual Studio

Wähle in Visual Studio 2019 **Datei** > **Neu** > **Projekt** aus. Suchen Sie die **Azure Functions**-Vorlage. Wählen Sie **Weiter** aus.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld „Neues Projekt“ mit hervorgehobener Azure Functions-Projektvorlage":::

Geben Sie einen Namen für die Funktions-App an, und wählen Sie dann __Erstellen__ aus.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld zum Konfigurieren eines neues Projekts, einschließlich der Einstellungen für Projektname und Speicherort, der Auswahl zum Erstellen einer neuen Projektmappe und des Projektmappennamens":::

Wählen Sie als Typ der Funktions-App **Event Grid-Trigger** und dann __Erstellen__ aus.

:::image type="content" source="media/how-to-create-azure-function/event-grid-trigger-function.png" alt-text="Screenshot von Visual Studio mit dem Dialogfeld zum Erstellen einer neuen Azure Functions-Anwendung mit hervorgehobener Event Grid-Triggeroption":::

Nachdem Ihre Funktions-App erstellt wurde, generiert Visual Studio ein Codebeispiel in der Datei *Function1.cs* in Ihrem Projektordner. Diese kurze Funktion wird zur Protokollierung von Ereignissen verwendet.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Screenshot von Visual Studio mit dem Projektfenster für das neue Projekt. Code für eine Beispielfunktion wird in einer Datei mit dem Namen „Function1“ angezeigt." lightbox="media/how-to-create-azure-function/visual-studio-sample-code.png":::

## <a name="write-a-function-that-has-an-event-grid-trigger"></a>Schreiben einer Funktion mit einem Event Grid-Trigger

Sie können eine Funktion schreiben, indem Sie Ihrer Funktions-App ein SDK hinzufügen. Die Funktions-App interagiert mit Azure Digital Twins unter Verwendung des [Azure Digital Twins SDK für .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client). 

Wenn Sie das SDK verwenden möchten, müssen Sie die folgenden Pakete in Ihr Projekt einbinden. Installieren Sie die Pakete mithilfe des NuGet-Paket-Managers von Visual Studio. Sie können die Pakete auch mithilfe von `dotnet` in einem Befehlszeilentool hinzufügen.

* [Azure.DigitalTwins.Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core/)
* [Azure.Identity](https://www.nuget.org/packages/Azure.Identity/)
* [System.Net.Http](https://www.nuget.org/packages/System.Net.Http/)
* [Azure.Core](https://www.nuget.org/packages/Azure.Core/)

Öffnen Sie als Nächstes im Visual Studio-Projektmappen-Explorer die Datei _Function1.cs_ mit Ihrem Beispielcode. Fügen Sie die folgenden `using`-Anweisungen für die Pakete hinzu.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="Function_dependencies":::

## <a name="add-authentication-code-to-the-function"></a>Hinzufügen von Authentifizierungscode zur Funktion

Nun deklarieren Sie Variablen auf Klassenebene und fügen Authentifizierungscode hinzu, um der Funktion den Zugriff auf Azure Digital Twins zu ermöglichen. Sie fügen diese Variablen und den Code Ihrer Funktion in der Datei _Function1.cs_ hinzu.

* **Code zum Lesen der Dienst-URL für Azure Digital Twins als Umgebungsvariable:** Es hat sich bewährt, die Dienst-URL aus einer Umgebungsvariable zu lesen, anstatt dafür die Hartcodierung in der Funktion zu verwenden. Sie legen den Wert dieser Umgebungsvariablen [weiter unten](#set-up-security-access-for-the-function-app) fest. Weitere Informationen zu Umgebungsvariablen finden Sie unter [Verwalten Ihrer Funktions-App](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal).

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ADT_service_URL":::

* **Eine statische Variable zum Aufnehmen einer HttpClient-Instanz** Die Erstellung von HttpClient ist relativ aufwendig, sodass Sie es vermeiden sollten, dies für jeden Funktionsaufruf durchzuführen.

    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="HTTP_client":::

* **Anmeldeinformationen für die verwaltete Identität:**
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="ManagedIdentityCredential":::

* **Eine lokale Variable _DigitalTwinsClient_:** Fügen Sie die Variable innerhalb Ihrer Funktion hinzu, um Ihre Azure Digital Twins-Clientinstanz aufzunehmen. Verwenden Sie hierfür *keine* statische Variable innerhalb Ihrer Klasse.
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs" id="DigitalTwinsClient":::

* **Eine NULL-Überprüfung für _adtInstanceUrl_:** Fügen Sie eine NULL-Überprüfung hinzu, und umschließen Sie Ihre Funktionslogik mit einem Try-Catch-Block, um alle Ausnahmen abzufangen.

Nach diesen Änderungen sollte Ihr Funktionscode dem folgenden Beispiel ähneln:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/adtIngestFunctionSample.cs":::

Nachdem Sie Ihre Anwendung geschrieben haben, können Sie sie in Azure veröffentlichen.

## <a name="publish-the-function-app-to-azure"></a>Veröffentlichen der Funktions-App in Azure

[!INCLUDE [digital-twins-publish-azure-function.md](../../includes/digital-twins-publish-azure-function.md)]

### <a name="verify-the-publication-of-your-function"></a>Überprüfen der Veröffentlichung Ihrer Funktion

1. Melden Sie sich mit Ihren Anmeldeinformationen im [Azure-Portal](https://portal.azure.com/) an.
2. Suchen Sie im Suchfeld am oberen Rand des Fensters nach dem Namen Ihrer Funktions-App, und wählen Sie ihn aus.

    :::image type="content" source="media/how-to-create-azure-function/search-function-app.png" alt-text="Screenshot des Azure-Portals. Geben Sie im Suchfeld den Namen der Funktions-App ein." lightbox="media/how-to-create-azure-function/search-function-app.png":::

3. Wählen Sie auf der daraufhin geöffneten Seite der **Funktions-App** im Menü auf der linken Seite **Funktionen** aus. Wenn Ihre Funktion veröffentlicht wurde, ist der Funktionsname in der Liste enthalten.

    > [!Note] 
    > Sie müssen möglicherweise einige Minuten warten oder die Seite mehrmals aktualisieren, bevor die Funktion in der Liste der veröffentlichten Funktionen aufgeführt wird.

    :::image type="content" source="media/how-to-create-azure-function/view-published-functions.png" alt-text="Anzeigen veröffentlichter Funktionen im Azure-Portal" lightbox="media/how-to-create-azure-function/view-published-functions.png":::

Damit Ihre Funktions-App auf Azure Digital Twins zugreifen kann, muss sie über eine systemseitig verwaltete Identität mit Zugriffsberechtigungen für Ihre Azure Digital Twins-Instanz verfügen. Dies wird im nächsten Schritt eingerichtet.

## <a name="set-up-security-access-for-the-function-app"></a>Einrichten des Sicherheitszugriffs für die Funktions-App

Sie können den Sicherheitszugriff für die Funktions-App mithilfe der Azure-Befehlszeilenschnittstelle oder über das Azure-Portal einrichten. Führen Sie die folgenden Schritte für Ihre gewählte Option aus.

# <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/cli)

Sie können diese Befehle in [Azure Cloud Shell](https://shell.azure.com) oder einer [lokalen Installation der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) ausführen.
Sie können die systemseitig verwaltete Identität der Funktions-App verwenden, um ihr die Rolle **Azure Digital Twins-Datenbesitzer** für Ihre Azure Digital Twins-Instanz zuzuweisen. Durch diese Rolle erhält die Funktions-App die Berechtigung, in der Instanz Aktivitäten auf Datenebene auszuführen. Anschließend machen Sie die URL Ihrer Instanz für Ihre Funktion zugänglich, indem Sie eine Umgebungsvariable festlegen.

### <a name="assign-an-access-role"></a>Zuweisen einer Zugriffsrolle

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Das Funktionsgerüst aus früheren Beispielen erforderte die Übergabe eines Bearertokens. Wenn kein Bearertoken übergeben wird, kann sich die Funktions-App nicht bei Azure Digital Twins authentifizieren. 

Um sicherzustellen, dass das Bearertoken übergeben wird, richten Sie Berechtigungen für [verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md) ein, sodass die Funktions-App auf Azure Digital Twins zugreifen kann. Sie richten diese Berechtigungen nur einmal für jede Funktions-App ein.


1. Verwenden Sie den folgenden Befehl, um die Details der systemseitig verwalteten Identität für die Funktion anzuzeigen. Beachten Sie in der Ausgabe das Feld `principalId`.

    ```azurecli-interactive 
    az functionapp identity show -g <your-resource-group> -n <your-App-Service-(function-app)-name> 
    ```

    >[!NOTE]
    > Falls die Ergebnisanzeige leer ist und keine Details einer Identität angezeigt werden, sollten Sie mit diesem Befehl eine neue systemseitig verwaltete Identität erstellen:
    > 
    >```azurecli-interactive    
    >az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>  
    >```
    >
    > In der Ausgabe werden Details zur Identität angezeigt, z. B. der für den nächsten Schritt benötigte Wert für `principalId`. 

1. Verwenden Sie den Wert von `principalId` im folgenden Befehl, um die Identität der Funktions-App der Rolle _Azure Digital Twins-Datenbesitzer_ für Ihre Azure Digital Twins-Instanz zuzuweisen:

    ```azurecli-interactive 
    az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
    ```

### <a name="configure-application-settings"></a>Konfigurieren von Anwendungseinstellungen

Machen Sie die URL Ihrer Instanz für Ihre Funktion zugänglich, indem Sie dafür eine Umgebungsvariable festlegen. Weitere Informationen zu Umgebungsvariablen finden Sie unter [Verwalten Ihrer Funktions-App](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

> [!TIP]
> Die URL der Azure Digital Twins-Instanz wird durch das Hinzufügen von *https://* am Anfang des Hostnamens der Instanz erstellt. Um den Hostnamen mit allen Eigenschaften der Instanz anzuzeigen, führen Sie Folgendes aus: `az dt show --dt-name <your-Azure-Digital-Twins-instance>`.

```azurecli-interactive 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-host-name>"
```

# <a name="azure-portal"></a>[Azure-Portal](#tab/portal)

Führen Sie im [Azure-Portal](https://portal.azure.com/) die folgenden Schritte aus.

### <a name="assign-an-access-role"></a>Zuweisen einer Zugriffsrolle

[!INCLUDE [digital-twins-permissions-required.md](../../includes/digital-twins-permissions-required.md)]

Azure-Ressourcen können über eine systemseitig zugewiesene verwaltete Identität bei Clouddiensten (z. B. Azure Key Vault) authentifiziert werden, ohne dass Anmeldeinformationen im Code gespeichert werden. Nachdem Sie die systemseitig zugewiesene verwaltete Identität aktiviert haben, können alle erforderlichen Berechtigungen über die rollenbasierte Zugriffssteuerung von Azure gewährt werden. 

Der Lebenszyklus dieser Art verwalteter Identitäten ist an den Lebenszyklus dieser Ressource gebunden. Zusätzlich darf jede Ressource nur eine systemseitig zugewiesene verwaltete Identität aufweisen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) nach Ihrer Funktions-App, indem Sie im Suchfeld ihren Namen eingeben. Wählen Sie in den Ergebnissen Ihre App aus. 

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Screenshot des Azure-Portals. Der Name der Funktions-App wird auf der Suchleiste des Portals gesucht, und das Suchergebnis ist hervorgehoben.":::

1. Wählen Sie auf der Seite der Funktions-App links im Menü die Option __Identität__ aus, um eine verwaltete Identität für die Funktion zu verwenden. Vergewissern Sie sich auf der Seite __Systemseitig zugewiesen__, dass der __Status__ auf **Ein** festgelegt ist. Wenn nicht, sollten Sie dies nun durchführen und **Speichern** auswählen, um die Änderung zu speichern.

    :::image type="content" source="media/how-to-create-azure-function/verify-system-managed-identity.png" alt-text="Screenshot des Azure-Portals mit der Seite „Identität“ für die Funktions-App. „Status“ ist auf „Ein“ festgelegt." lightbox="media/how-to-create-azure-function/verify-system-managed-identity.png":::

1. Wählen Sie __Azure-Rollenzuweisungen__ aus.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-1.png" alt-text="Screenshot des Azure-Portals. Auf der Seite „Identität“ der Azure-Funktion ist unter „Berechtigungen“ die Schaltfläche „Azure-Rollenzuweisungen“ hervorgehoben." lightbox="media/how-to-create-azure-function/add-role-assignment-1.png":::

    Wählen Sie die Option __+ Rollenzuweisung hinzufügen (Vorschau)__ aus.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-2.png" alt-text="Screenshot des Azure-Portals. Auf der Seite „Azure-Rollenzuweisungen“ ist „+ Rollenzuweisung hinzufügen (Vorschau)“ hervorgehoben." lightbox="media/how-to-create-azure-function/add-role-assignment-2.png":::

1. Wählen Sie auf der Seite __Rollenzuweisung hinzufügen (Vorschau)__ die folgenden Werte aus:

    * **Bereich:** _Ressourcengruppe_
    * **Abonnement**: Wählen Sie Ihr Azure-Abonnement.
    * **Ressourcengruppe:** Wählen Sie Ihre Ressourcengruppe aus.
    * **Rolle:** _Azure Digital Twins-Datenbesitzer_

    Wählen Sie __Speichern__ aus, um die Details zu speichern.

    :::image type="content" source="media/how-to-create-azure-function/add-role-assignment-3.png" alt-text="Screenshot des Azure-Portals, der zeigt, wie eine neue Rollenzuweisung hinzugefügt wird. Im Dialogfeld werden Felder für Bereich, Abonnement, Ressourcengruppe und Rolle angezeigt.":::

### <a name="configure-application-settings"></a>Konfigurieren von Anwendungseinstellungen

Um die URL Ihrer Azure Digital Twins-Instanz für Ihre Funktion zugänglich zu machen, können Sie eine Umgebungsvariable festlegen. Anwendungseinstellungen werden als Umgebungsvariablen verfügbar gemacht, um den Zugriff auf die Azure Digital Twins-Instanz zu ermöglichen. Weitere Informationen zu Umgebungsvariablen finden Sie unter [Verwalten Ihrer Funktions-App](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

Ermitteln Sie zunächst den Hostnamen Ihrer Instanz, um dann eine Umgebungsvariable mit der URL dieser Instanz festzulegen: 

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrer Instanz. 
1. Wählen Sie im Menü auf der linken Seite die Option __Übersicht__ aus. 
1. Kopieren Sie den Wert von __Hostname__.

    :::image type="content" source="media/how-to-create-azure-function/instance-host-name.png" alt-text="Screenshot des Azure-Portals mit hervorgehobenem Hostnamen auf der Übersichtsseite für die Instanz":::

Sie können nun eine Anwendungseinstellung erstellen:

1. Suchen Sie auf der Suchleiste des Portals nach Ihrer Funktions-App, und wählen Sie sie aus den Ergebnissen aus.

    :::image type="content" source="media/how-to-create-azure-function/portal-search-for-function-app.png" alt-text="Screenshot des Azure-Portals mit einer Suche nach dem Namen der Funktions-App auf der Suchleiste. Das Suchergebnis ist hervorgehoben.":::

1. Wählen Sie links die Option __Konfiguration__ aus. Wählen Sie dann auf der Registerkarte __Anwendungseinstellungen__ die Option __+ Neue Anwendungseinstellung__ aus.

    :::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Screenshot des Azure-Portals mit hervorgehobener Schaltfläche zum Erstellen einer neuen Anwendungseinstellung auf der Konfigurationsseite für die Funktions-App":::

1. Verwenden Sie im daraufhin geöffneten Fenster den oben kopierten Hostnamenwert, um eine Anwendungseinstellung zu erstellen.
    * **Name**: ADT_SERVICE_URL
    * **Value**: https://{your-azure-digital-twins-host-name}
    
    Wählen Sie __OK__ aus, um eine Anwendungseinstellung zu erstellen.
    
    :::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Screenshot des Azure-Portals. Auf der Seite „Anwendungseinstellung hinzufügen/bearbeiten“ sind die Felder „Name“ und „Wert“ ausgefüllt. Die Schaltfläche „OK“ ist hervorgehoben.":::

1. Nachdem Sie die Einstellung erstellt haben, sollte sie auf der Registerkarte __Anwendungseinstellungen__ angezeigt werden. Überprüfen Sie, ob **ADT_SERVICE_URL** in der Liste angezeigt wird. Speichern Sie dann die neue Anwendungseinstellung, indem Sie __Speichern__ auswählen.

    :::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Screenshot des Azure-Portals. Auf der Registerkarte „Anwendungseinstellungen“ sind die neue Einstellung „ADT_SERVICE_URL“ und die Schaltfläche „Speichern“ hervorgehoben.":::

1. Bei jeder Änderung an Anwendungseinstellungen muss die Anwendung neu gestartet werden. Wählen Sie daher __Weiter__ aus, um Ihre Anwendung neu zu starten, wenn die entsprechende Aufforderung angezeigt wird.

    :::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Screenshot des Azure-Portals mit einer Benachrichtigung, dass für Änderungen an Anwendungseinstellungen ein Neustart Ihrer Anwendung vorgenommen wird. Die Schaltfläche „Weiter“ ist hervorgehoben.":::

---

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Funktions-App in Azure für die Verwendung mit Azure Digital Twins eingerichtet. Als Nächstes erfahren Sie, wie Sie auf Ihrer grundlegenden Funktion aufbauen können, um [IoT Hub-Daten in Azure Digital Twins zu erfassen](how-to-ingest-iot-hub-data.md).
