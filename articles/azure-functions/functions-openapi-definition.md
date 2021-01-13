---
title: Verfügbarmachen Ihrer Funktionen mit OpenAPI unter Verwendung von Azure API Management
description: Erstellen Sie eine OpenAPI-Definition, die anderen Apps und Diensten das Aufrufen Ihrer Funktion in Azure ermöglicht.
ms.topic: tutorial
ms.date: 04/21/2020
ms.reviewer: sunayv
ms.custom: devx-track-csharp, mvc, cc996988-fb4f-47, references_regions
ms.openlocfilehash: 9083ff7d8f65c68ce8d173973a4eda650ac355aa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88212909"
---
# <a name="create-an-openapi-definition-for-a-serverless-api-using-azure-api-management"></a>Erstellen einer OpenAPI-Definition für eine serverlose API mithilfe von Azure API Management

REST-APIs werden häufig mithilfe einer OpenAPI-Definition beschrieben. Diese Definition enthält Informationen zu den in einer API verfügbaren Vorgängen sowie zur Strukturierung der Anforderungs- und Antwortdaten für die API.

In diesem Tutorial erstellen Sie eine Funktion, die ermittelt, ob eine Notfallreparatur einer Windturbine kosteneffizient ist. Anschließend erstellen Sie eine OpenAPI-Definition für die Funktions-App mithilfe von [Azure API Management](../api-management/api-management-key-concepts.md), damit die Funktion von anderen Apps und Diensten aufgerufen werden kann.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Funktion in Azure
> * Generieren einer OpenAPI-Definition mit Azure API Management
> * Testen der Definition durch Aufrufen der Funktion
> * Herunterladen der OpenAPI-Definition

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Sie können mit einer Funktions-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung, Skalierung und Freigabe von Ressourcen.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

## <a name="create-the-function"></a>Erstellen der Funktion

In diesem Tutorial wird eine per HTTP ausgelöste Funktion verwendet, die zwei Parameter akzeptiert:

* Die geschätzte Zeit für die Durchführung einer Turbinenreparatur in Stunden.
* Die Kapazität der Turbine in Kilowatt. 

Die Funktion berechnet dann die Kosten einer Reparatur und den Umsatzerlös, der in einem Zeitraum von 24 Stunden von der Turbine generiert werden könnte. So erstellen Sie die per HTTP ausgelöste Funktion über das [Azure-Portal](https://portal.azure.com):

1. Wählen Sie im linken Menü Ihrer Funktions-App die Option **Funktionen** und anschließend im oberen Menü die Option **Hinzufügen** aus.

1. Wählen Sie im Fenster **Neue Funktion** die Option **HTTP-Trigger** aus.

1. Geben Sie unter **Neue Funktion** Folgendes ein: `TurbineRepair`. 

1. Wählen Sie in der Dropdownliste **[Autorisierungsstufe](functions-bindings-http-webhook-trigger.md#http-auth)** die Option **Funktion** und anschließend **Funktion erstellen** aus.

    :::image type="content" source="media/functions-openapi-definition/select-http-trigger-openapi.png" alt-text="Erstellen einer HTTP-Funktion für OpenAPI&quot;:::

1. Wählen Sie **Programmieren und testen** und anschließend in der Dropdownliste die Option **run.csx** aus. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r &quot;Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    const double revenuePerkW = 0.12;
    const double technicianCost = 250;
    const double turbineCost = 100;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        // Get query strings if they exist
        int tempVal;
        int? hours = Int32.TryParse(req.Query["hours"], out tempVal) ? tempVal : (int?)null;
        int? capacity = Int32.TryParse(req.Query["capacity"], out tempVal) ? tempVal : (int?)null;
    
        // Get request body
        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
    
        // Use request body if a query was not sent
        capacity = capacity ?? data?.capacity;
        hours = hours ?? data?.hours;
    
        // Return bad request if capacity or hours are not passed in
        if (capacity == null || hours == null){
            return new BadRequestObjectResult("Please pass capacity and hours on the query string or in the request body");
        }
        // Formulas to calculate revenue and cost
        double? revenueOpportunity = capacity * revenuePerkW * 24;  
        double? costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;
    
        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        };
    
        return (ActionResult)new OkObjectResult(new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix
        });
    }
    ```

    Von diesem Funktionscode wird durch Rückgabe von `Yes` oder `No` angegeben, ob eine Notfallreparatur kosteneffizient ist. Darüber hinaus werden der mögliche Umsatzerlös der Turbine und die Kosten für die Reparatur zurückgegeben.

1. Wählen Sie zum Testen der Funktion **Testen** aus, wählen Sie die Registerkarte **Eingabe** aus, geben Sie für den **Hauptteil** Folgendes ein, und wählen Sie anschließend **Ausführen** aus:

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    :::image type="content" source="media/functions-openapi-definition/test-function.png" alt-text="Erstellen einer HTTP-Funktion für OpenAPI&quot;:::

1. Wählen Sie **Programmieren und testen** und anschließend in der Dropdownliste die Option **run.csx** aus. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r &quot;Newtonsoft.Json":::

    Auf der Registerkarte **Ausgabe** wird die folgende Ausgabe zurückgegeben:

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Sie haben jetzt eine Funktion, die die Kosteneffizienz von Notfallreparaturen ermittelt. Als Nächstes generieren Sie eine OpenAPI-Definition für die Funktions-App.

## <a name="generate-the-openapi-definition"></a>Generieren der OpenAPI-Definition

So generieren Sie die OpenAPI-Definition:

1. Wählen Sie die Funktions-App aus. Wählen Sie dann im linken Menü die Option **API Management** und anschließend unter **API Management** die Option **Neu erstellen** aus.

    :::image type="content" source="media/functions-openapi-definition/select-all-settings-openapi.png" alt-text="Erstellen einer HTTP-Funktion für OpenAPI&quot;:::

1. Wählen Sie **Programmieren und testen** und anschließend in der Dropdownliste die Option **run.csx** aus. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r &quot;Newtonsoft.Json":::


1. Verwenden Sie die API Management-Einstellungen aus der folgenden Tabelle:

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Es wird ein Name auf der Grundlage des Namens Ihrer Funktions-App generiert. |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Ressource erstellt wird. |  
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Dieselbe Ressource wie Ihre Funktions-App, die für Sie festgelegt werden sollte. |
    | **Location** | USA (Westen) | Wählen Sie den Standort „USA, Westen“ aus. |
    | **Name der Organisation** | Contoso | Der Name der Organisation, der im Entwicklerportal und für E-Mail-Benachrichtigungen verwendet wird. |
    | **Administrator-E-Mail** | Ihre E-Mail-Adresse | Die E-Mail-Adresse, die Benachrichtigungen des Systems von API Management erhält. |
    | **Preisstufe** | Nutzung | Der Tarif „Consumption“ ist nicht in allen Regionen verfügbar. Vollständige Preisinformationen finden Sie auf der [API Management-Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/api-management/) |

    ![Erstellen eines neuen API Management-Diensts](media/functions-openapi-definition/new-apim-service-openapi.png)

1. Wählen Sie **Erstellen** aus, um die API Management-Instanz zu erstellen, was ein paar Minuten dauern kann.

1. Nach Erstellung der Instanz durch Azure wird die Option **Application Insights aktivieren** auf der Seite verfügbar. Wählen Sie sie aus, um Protokolle an den gleichen Ort zu senden wie die Funktionsanwendung, und wählen Sie anschließend **API verknüpfen** aus.

1. **Azure-Funktionen importieren** wird mit hervorgehobener Funktion **Turbinenreparatur** geöffnet. Wählen Sie **Auswählen** aus, um fortzufahren.

    ![Importieren von Azure-Funktionen in API Management](media/functions-openapi-definition/import-function-openapi.png)

1. Übernehmen Sie auf der Seite **Aus Funktions-App erstellen** die Standardeinstellungen, und wählen Sie **Erstellen** aus.

    :::image type="content" source="media/functions-openapi-definition/create-function-openapi.png" alt-text="Erstellen einer HTTP-Funktion für OpenAPI&quot;:::

1. Wählen Sie **Programmieren und testen** und anschließend in der Dropdownliste die Option **run.csx** aus. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r &quot;Newtonsoft.Json":::

    Von Azure wird die API für die Funktion erstellt.

## <a name="test-the-api"></a>Testen der API

Vor der Verwendung der OpenAPI-Definition sollten Sie sich vergewissern, dass die API funktioniert.

1. Wählen Sie auf der Seite der Funktions-App **API Management** > Registerkarte **Test** > **POST TurbineRepair** aus. 

1. Geben Sie unter **Anforderungstext** den folgenden Code ein:

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

1. Wählen Sie **Senden** aus, und sehen Sie sich die **HTTP-Antwort** an.

    :::image type="content" source="media/functions-openapi-definition/test-function-api-openapi.png" alt-text="Erstellen einer HTTP-Funktion für OpenAPI&quot;:::

1. Wählen Sie **Programmieren und testen** und anschließend in der Dropdownliste die Option **run.csx** aus. Ersetzen Sie den Inhalt der C#-Skriptdatei „run.csx“ durch den folgenden Code, und wählen Sie dann **Speichern** aus:

    ```csharp
    #r &quot;Newtonsoft.Json":::

## <a name="download-the-openapi-definition"></a>Herunterladen der OpenAPI-Definition

Wenn Ihre API wie erwartet funktioniert, können Sie die OpenAPI-Definition herunterladen.

1. Wählen Sie oben auf der Seite **OpenAPI-Definition herunterladen** aus.
   
   ![Herunterladen der OpenAPI-Definition](media/functions-openapi-definition/download-definition.png)

2. Speichern Sie die heruntergeladene JSON-Datei, und öffnen Sie sie. Überprüfen Sie die Definition.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben mithilfe der API Management-Integration eine OpenAPI-Definition Ihrer Funktionen generiert. Als Nächstes können Sie die Definition in API Management über das Portal bearbeiten. Außerdem können Sie sich [ausführlicher über API Management](../api-management/api-management-key-concepts.md) informieren.

> [!div class="nextstepaction"]
> [Bearbeiten von APIs](../api-management/edit-api.md)
