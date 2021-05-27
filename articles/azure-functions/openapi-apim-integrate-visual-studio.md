---
title: Erstellen serverloser APIs in Visual Studio mit Azure Functions und API Management
description: Verwenden Sie Visual Studio, um eine per HTTP ausgelöste Funktion zusammen mit einer OpenAPI-Definition zu erstellen, die die Integration von Azure API Management ermöglicht, sodass andere Apps und Dienste Ihre serverlose funktionsbasierte API aufrufen können.
ms.topic: tutorial
ms.date: 05/07/2021
ms.openlocfilehash: b8c6afc64f15df3124ab4052568bf36d209d4fde
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110383984"
---
# <a name="create-serverless-apis-in-visual-studio-using-azure-functions-and-api-management-integration-preview"></a>Erstellen serverloser APIs in Visual Studio mit Azure Functions und Integration von API Management (Vorschau)

REST-APIs werden häufig mithilfe einer OpenAPI-Definition beschrieben. Diese Datei enthält Informationen zu Vorgängen in einer API sowie zur Strukturierung der Anforderungs- und Antwortdaten für die API.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines serverlosen Funktionsprojekts in Visual Studio
> * Lokales Testen von Funktions-APIs mithilfe der integrierten OpenAPI-Funktionalität
> * Veröffentlichen eines Projekts in einer Funktions-App in Azure mit API Management-Integration 
> * Abrufen des Zugriffsschlüssels für die Funktion und dessen Festlegung in API Management
> * Herunterladen der OpenAPI-Definitionsdatei

Die serverlose Funktion, die Sie erstellen, stellt eine API bereit, mit der Sie ermitteln können, ob eine Notfallreparatur an einer Windturbine kostengünstig ist. Da sowohl die Funktions-App als auch die API Management-Instanz, die Sie erstellen, einen Verbrauchsplan verwendet, sind die Kosten für die Durchführung dieses Tutorials minimal.

> [!NOTE]
> Die in diesem Artikel vorgestellte OpenAPI- und API Management-Integration befindet sich derzeit in der Vorschau. Diese Methode zum Verfügbarmachen einer serverlosen API wird nur für C#-Klassenbibliotheksfunktionen (.NET Core 3.1) unterstützt. Alle anderen Sprachruntimes sollten stattdessen [die Azure API Management-Integration über das Portal verwenden](functions-openapi-definition.md). 

## <a name="prerequisites"></a>Voraussetzungen

+ [Visual Studio 2019](https://azure.microsoft.com/downloads/), Version 16.10 oder höher. Wählen Sie bei der Installation unbedingt die Workload **Azure-Entwicklung** aus. 

+ Wenn Sie nicht über ein aktives [Azure-Abonnement](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/dotnet/) erstellen, bevor Sie beginnen.

## <a name="create-a-functions-project"></a>Erstellen eines Functions-Projekts

Mit der Azure Functions-Projektvorlage in Visual Studio wird ein Projekt erstellt, das Sie in einer Funktions-App in Azure veröffentlichen können. Außerdem erstellen Sie eine per HTTP ausgelöste Funktion, die die Generierung von OpenAPI-Definitionsdateien (ehemals Swagger-Datei) unterstützt.

1. Wählen Sie im Visual Studio-Menü **Datei** > **Neu** > **Projekt** aus.

1. Geben Sie unter **Neues Projekt erstellen** den Suchbegriff *functions* in das Suchfeld ein, und wählen Sie die Vorlage **Azure Functions** und anschließend **Weiter** aus.

1. Geben Sie unter **Neues Projekt konfigurieren** im Feld **Projektname** einen Namen für Ihr Projekt ein, und wählen Sie anschließend `TurbineRepair` und dann **Erstellen** aus. 

1. Verwenden Sie für die Einstellungen zum **Erstellen einer neuen Azure Functions-Anwendung** die Werte in der folgenden Tabelle:

    | Einstellung      | Wert  | Beschreibung                      |
    | ------------ |  ------- |----------------------------------------- |
    | **.NET-Version** | **.NET Core 3 (LTS)** | Dieser Wert erstellt ein Funktionsprojekt, für das Version 3.x der Runtime von Azure Functions verwendet wird. Die OpenAPI-Dateigenerierung wird nur für Version 3.x der Functions-Runtime unterstützt. |
    | **Funktionsvorlage** | **HTTP-Trigger mit OpenAPI** | Dieser Wert erstellt eine Funktion, die durch eine HTTP-Anforderung ausgelöst wird, mit der Möglichkeit, eine OpenAPI-Definitionsdatei zu generieren.  |
    | **Speicherkonto (AzureWebJobsStorage)**  | **Speicheremulator** | Sie können den Emulator für die lokale Entwicklung von HTTP-Triggerfunktionen verwenden. Da für eine Funktions-App in Azure ein Speicherkonto erforderlich ist, wird ein Speicherkonto zugewiesen oder erstellt, wenn Sie Ihr Projekt in Azure veröffentlichen. |
    | **Autorisierungsstufe** | **Funktion** | Bei der Ausführung in Azure müssen Clients beim Zugriff auf den Endpunkt einen Schlüssel angeben. Weitere Informationen zu Schlüsseln und Autorisierung finden Sie unter [Funktionszugriffsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys). |
    
    ![Azure Functions-Projekteinstellungen](./media/openapi-apim-integrate-vs/functions-project-settings.png)

1. Wählen Sie **Erstellen** aus, um das Funktionsprojekt und die HTTP-Triggerfunktion mit Unterstützung für OpenAPI zu erstellen. 

Visual Studio erstellt ein Projekt und eine Klasse namens `Function1` mit Codebausteinen für den Funktionstyp „HTTP-Trigger“. Als Nächstes ersetzen Sie diesen Funktionsvorlagencode durch Ihren eigenen benutzerdefinierten Code. 

## <a name="update-the-function-code"></a>Aktualisieren des Funktionscodes

Die Funktion verwendet einen HTTP-Trigger, der zwei Parameter annimmt:

| Parametername | BESCHREIBUNG|
| ---- | ---- |
| *hours* | Die geschätzte Zeit für eine Turbinenreparatur bis zur nächsten ganzen Stunde. |
| *Kapazität* | Die Kapazität der Turbine in Kilowatt. |

Die Funktion berechnet dann die Kosten einer Reparatur und den Umsatzerlös, der in einem Zeitraum von 24 Stunden von der Turbine generiert werden könnte. Parameter werden entweder in der Abfragezeichenfolge oder in der Nutzlast einer POST-Anforderung angegeben. 

Ersetzen Sie in der Projektdatei „Function1.cs“ den Inhalt des generierten Klassenbibliothekscodes durch den folgenden Code:

:::code language="csharp" source="~/functions-openapi-turbine-repair/TurbineRepair/Function1.cs":::

Von diesem Funktionscode wird durch Rückgabe von `Yes` oder `No` angegeben, ob eine Notfallreparatur kosteneffizient ist. Darüber hinaus werden der mögliche Umsatzerlös der Turbine und die Kosten für die Reparatur zurückgegeben.

## <a name="run-and-verify-the-api-locally"></a>Lokales Ausführen und Überprüfen der API

Wenn Sie die Funktion ausführen, erleichtern die OpenAPI-Endpunkte das lokale Testen der Funktion mithilfe einer generierten Seite. Sie müssen bei der lokalen Ausführung keine Funktionszugriffsschlüssel bereitstellen.

1. Drücken Sie F5, um das Projekt zu starten. Wenn die Functions-Runtime lokal gestartet wird, werden in der Ausgabe eine Reihe von OpenAPI- und Swagger-Endpunkten zusammen mit dem Funktionsendpunkt angezeigt.  

1. Öffnen Sie in Ihrem Browser den RenderSwaggerUI-Endpunkt, der so `http://localhost:7071/api/swagger/ui` aussehen sollte. Eine Seite wird basierend auf Ihren OpenAPI-Definitionen gerendert.

1. Wählen Sie **POST** > **Ausprobieren** aus, geben Sie Werte für `hours` und `capacity` entweder als Abfrageparameter oder im JSON-Anforderungstext ein, und wählen Sie **Ausführen** aus. 

    :::image type="content" source="media/openapi-apim-integrate-vs/swagger-ui-post.png" alt-text="Swagger-Benutzeroberfläche zum Testen der TurbineRepair-API":::

1. Wenn Sie ganzzahlige Werte wie 6 für `hours` und 2500 für `capacity` eingeben, erhalten Sie eine JSON-Antwort, die wie im folgenden Beispiel aussieht:
   
    :::image type="content" source="media/openapi-apim-integrate-vs/swagger-ui-response.png" alt-text="Antwort-JSON-Daten aus der TurbineRepair-Funktion.":::

Sie haben jetzt eine Funktion, die die Kosteneffizienz von Notfallreparaturen ermittelt. Veröffentlichen Sie als Nächstes Ihr Projekt und Ihre API-Definitionen in Azure.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Sie müssen in Ihrem Azure-Abonnement über eine Funktions-App verfügen, um Ihr Projekt veröffentlichen zu können. Bei der Veröffentlichung in Visual Studio wird eine Funktions-App erstellt, wenn Sie das Projekt zum ersten Mal veröffentlichen. Es kann auch eine API Management-Instanz erstellt werden, die in Ihre Funktions-App integriert wird, um die TurbineRepair-API verfügbar zu machen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus. Wählen Sie unter **Ziel** die Option **Azure** und anschließend **Weiter** aus.

1. Wählen Sie für **Bestimmtes Ziel** die Option **Azure-Funktions-App (Windows)** aus, um eine Funktions-App zu erstellen, die unter Windows ausgeführt wird, und wählen Sie dann **Weiter** aus.

1. Wählen Sie unter **Funktionsinstanz** die Option **+ Neue Azure Functions-Instanz erstellen** aus.

    :::image type="content" source="media/openapi-apim-integrate-vs/publish-new-resource.png" alt-text="Erstellen einer neuen Funktions-App-Instanz":::

1. Erstellen Sie eine neue Instanz mit den Werten aus der folgenden Tabelle:

    | Einstellung      | Wert  | Beschreibung                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Name, der Ihre neue Funktions-App eindeutig identifiziert. Übernehmen Sie diesen Namen, oder geben Sie einen neuen Namen ein. Gültige Zeichen: `a-z`, `0-9` und `-`. |
    | **Abonnement** | Ihr Abonnement | Das zu verwendende Azure-Abonnement. Übernehmen Sie dieses Abonnement, oder wählen Sie in der Dropdownliste ein neues Abonnement aus. |
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** | Name Ihrer Ressourcengruppe |  Die Ressourcengruppe, in der Ihre Funktions-App erstellt werden soll. Wählen Sie in der Dropdownliste eine vorhandene Ressourcengruppe aus, oder wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen.|
    | **[Plantyp](functions-scale.md)** | Nutzung | Wenn Sie Ihr Projekt in einer Funktions-App veröffentlichen, die in einem [Verbrauchsplan](consumption-plan.md) ausgeführt wird, bezahlen Sie nur für die Ausführungen Ihrer Funktions-App. Für andere Hostingpläne fallen höhere Kosten an. |
    | **Location** | Standort des Diensts | Wählen Sie einen **Standort** in einer [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe anderer Dienste aus, auf die Ihre Funktionen zugreifen. |
    | **[Azure Storage](storage-considerations.md)** | Universelles Speicherkonto | Für die Functions-Runtime wird ein Azure Storage-Konto benötigt. Wählen Sie **Neu** aus, um ein universelles Speicherkonto zu konfigurieren. Sie können auch ein vorhandenes Konto auswählen, das die [Anforderungen an das Speicherkonto](storage-considerations.md#storage-account-requirements) erfüllt.  |

    :::image type="content" source="media/openapi-apim-integrate-vs/create-function-app-with-storage.png" alt-text="Erstellen einer neuen Funktions-App in Azure mit Storage":::

1. Wählen Sie **Erstellen** aus, um eine Funktions-App und die zugehörigen Ressourcen in Azure zu erstellen. Der Status der Ressourcenerstellung wird links unten im Fenster angezeigt. 

1. Vergewissern Sie sich unter **Funktionsinstanz**, dass die Option **Aus Paketdatei ausführen** aktiviert ist. Ihre Funktions-App wird unter Verwendung der [ZIP-Bereitstellung](functions-deployment-technologies.md#zip-deploy) mit aktiviertem Modus [Run-From-Package](run-functions-from-deployment-package.md) bereitgestellt. Diese Bereitstellungsmethode wird für Ihr Funktionsprojekt empfohlen, da damit eine bessere Leistung erzielt wird. 

1. Wählen Sie **Weiter** und auf der **API Management**-Seite auch **+ API Management-API erstellen** aus.

1.  Erstellen Sie mithilfe der Werte in der folgenden Tabelle eine **API in API Management**:

    | Einstellung      | Wert  | Beschreibung                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **API-Name** | TurbineRepair | Name für die API. |
    | **Abonnementname** | Ihr Abonnement | Das zu verwendende Azure-Abonnement. Übernehmen Sie dieses Abonnement, oder wählen Sie in der Dropdownliste ein neues Abonnement aus. |
    | **Ressourcengruppe** | Name Ihrer Ressourcengruppe | Wählen Sie in der Dropdownliste die gleiche Ressourcengruppe aus, die von Ihrer Funktions-App verwendet wird.   |
    | **API Management-Dienst** | Neue Instanz | Wählen Sie **Neu** aus, um eine neue API Management-Instanz auf der serverlosen Ebene zu erstellen.   |

    :::image type="content" source="media/openapi-apim-integrate-vs/create-api-management-api.png" alt-text="Erstellen einer API Management-Instanz mit API":::

1. Wählen Sie **Erstellen** aus, um die API Management-Instanz mit der TurbineRepair-API aus der Funktionsintegration zu erstellen.

1. Wählen Sie **Fertig stellen** aus, überprüfen Sie, ob auf der Seite „Veröffentlichen“ **Bereit zur Veröffentlichung** angezeigt wird, und wählen Sie dann **Veröffentlichen** aus, um das Paket mit Ihren Projektdateien in der neuen Funktions-App in Azure bereitzustellen. 

    Nach Abschluss der Bereitstellung wird die Stamm-URL der Funktions-App in Azure auf der Registerkarte **Veröffentlichen** angezeigt. 

## <a name="get-the-function-access-key"></a>Abrufen eines Funktionszugriffsschlüssels

1. Wählen Sie auf der Registerkarte **Veröffentlichen** die Auslassungspunkte ( **...** ) neben **Hosting** und dann **Im Azure-Portal öffnen** aus. Die von Ihnen erstellte Funktions-App wird im Azure-Portal in Ihrem Standardbrowser geöffnet. 

1. Wählen Sie unter **Funktionen** die Option **Funktionen** > **TurbineRepair** und dann **Funktionsschlüssel** aus. 

    :::image type="content" source="media/openapi-apim-integrate-vs/get-function-keys.png" alt-text="Abrufen eines Zugriffsschlüssels für die TurbineRepair-Funktion":::

1. Wählen Sie unter **Funktionsschlüssel** die Option **Standard** aus, und kopieren Sie den **Wert**. Sie können diesen Schlüssel jetzt im API Management festlegen, damit er auf den Funktionsendpunkt zugreifen kann.

## <a name="configure-api-management"></a>Konfigurieren von API Management

1. Wählen Sie auf der Registerkarte **Veröffentlichen** die Auslassungspunkte ( **...** ) neben **Hosting** und dann **API im Azure-Portal öffnen** aus. Die API Management-Instanz, die Sie erstellt haben, wird im Azure-Portal in Ihrem Standardbrowser geöffnet. Diese API Management-Instanz ist bereits mit Ihrer Funktions-App verknüpft. 

1. Wählen Sie unter **APIs** die Option **Azure Functions OpenAPI Extension** > **Test** > **POST-Ausführung** und dann unter **Eingangsrichtlinie** die Option **Richtlinie hinzufügen** aus.

    :::image type="content" source="media/openapi-apim-integrate-vs/apim-add-policy.png" alt-text="Hinzufügen einer Eingangsrichtlinie zur API":::

1. Wählen Sie unter **Eingangsrichtlinie hinzufügen** die Option **Abfrageparameter festlegen** aus, geben Sie `code` für **Name** ein, wählen Sie **+Wert** aus, fügen Sie den kopierten Funktionsschlüssel ein, und wählen Sie **Speichern** aus. API Management enthält den Funktionsschlüssel, wenn der Aufruf an den Funktionsendpunkt übergeben wird. 

Da der Funktionsschlüssel nun festgelegt ist, können Sie die API aufrufen, um zu überprüfen, ob er funktioniert, wenn er in Azure gehostet wird.

## <a name="verify-the-api-in-azure"></a>Überprüfen der API in Azure

1. Wählen Sie in der API die Registerkarte **Test** und dann **POST-Ausführung** aus, geben Sie den folgenden Code in **Anforderungstext** > **Roh** ein, und wählen Sie **Senden** aus:

    ```json
    {
        "hours": "6",
        "capacity": "2500"
    }
    ```

    :::image type="content" source="media/openapi-apim-integrate-vs/api-management-test-function-api.png" alt-text="OpenAPI-Testseite in der API Management-API":::

    Wie zuvor können Sie auch dieselben Werte als Abfrageparameter angeben. 

1. Wählen Sie **Senden** aus, und zeigen Sie dann die **HTTP-Antwort** an, um zu überprüfen, ob die gleichen Ergebnisse von der API zurückgegeben werden.

## <a name="download-the-openapi-definition"></a>Herunterladen der OpenAPI-Definition

Wenn Ihre API wie erwartet funktioniert, können Sie die OpenAPI-Definition herunterladen.

1. 1. Wählen Sie unter **APIs** die Option **Azure Functions OpenAPI Extension**, die Auslassungspunkte ( **...** ) und dann **Exportieren** aus.
   
   ![Herunterladen der OpenAPI-Definition](media/openapi-apim-integrate-vs/download-definition.png)

2. Wählen Sie die Mittel für den API-Export einschließlich OpenAPI-Dateien in verschiedenen Formaten aus. Sie können auch [APIs aus Azure API Management auf die Power Platform exportieren](../api-management/export-api-power-platform.md). 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie die Ressourcengruppe löschen.
 
Wählen Sie im Azure-Portalmenü oder auf der **Startseite** die Option **Ressourcengruppen** aus. Wählen Sie dann auf der Seite **Ressourcengruppen** die Gruppe aus, die Sie erstellt haben.

Stellen Sie auf der Seite **myResourceGroup** sicher, dass die Ressourcen aufgelistet sind, die Sie löschen möchten.

Wählen Sie **Ressourcengruppe löschen** aus, geben Sie den Namen Ihrer Gruppe zur Bestätigung im Textfeld ein, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben mit Visual Studio 2019 eine Funktion erstellt, die aufgrund der [OpenAPI Extension](https://github.com/Azure/azure-functions-openapi-extension) selbstdokumentierend und in API Management integriert ist. Als Nächstes können Sie die Definition in API Management über das Portal anpassen. Außerdem können Sie sich [ausführlicher über API Management](../api-management/api-management-key-concepts.md) informieren.

> [!div class="nextstepaction"]
> [Bearbeiten von APIs](../api-management/edit-api.md)
