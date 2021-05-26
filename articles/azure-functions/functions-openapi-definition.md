---
title: Mithilfe von Azure API Management APIs aus Funktionen verfügbar machen
description: Erstellen Sie eine OpenAPI-Definition, die anderen Apps und Diensten das Aufrufen Ihrer Funktion in Azure ermöglicht.
ms.topic: how-to
ms.date: 05/04/2021
ms.openlocfilehash: ec25b03b3d93fcd69297899653279fea86eabb02
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110375671"
---
# <a name="expose-serverless-apis-from-http-endpoints-using-azure-api-management"></a>Verfügbar machen von serverlose APIs von HTTP-Endpunkten mithilfe von Azure API Management

Azure Functions integriert Azure API Management im Portal, damit Sie Ihre HTTP-Triggerfunktionsendpunkte als REST-APIs verfügbar machen können. Diese APIs werden mithilfe einer OpenAPI-Definition beschrieben. Diese JSON-Datei (oder YAML)-Datei enthält Informationen darüber, welche Vorgänge in einer API verfügbar sind. Sie enthält Details dazu, wie die Anforderungs- und Antwortdaten für die API strukturiert werden sollen. Durch die Integration Ihrer Funktions-App können Sie API Management OpenAPI-Definitionen generieren.  

In diesem Artikel erfahren Sie, wie Sie Ihre Funktions-App in API Management integrieren. Diese Integration funktioniert für Funktions-Apps, die in einer beliebigen [unterstützten Sprache](supported-languages.md) entwickelt wurden. Sie können auch Ihre [Funktions-App aus Azure API Management importieren](../api-management/import-function-app-as-api.md).

Für C#-Klassenbibliotheksfunktionen können Sie auch [Visual Studio verwenden](openapi-apim-integrate-visual-studio.md), um eine serverlose API zu erstellen und zu veröffentlichen, die in API Management intergriert ist.  

## <a name="create-the-api-management-instance"></a>Erstellen der API Management-Instanz

So erstellen Sie eine API Management-Instanz, die mit Ihrer Funktions-App verknüpft ist:

1. Wählen Sie die Funktions-App aus. Wählen Sie dann im linken Menü die Option **API Management** und anschließend unter **API Management** die Option **Neu erstellen** aus.

    :::image type="content" source="media/functions-openapi-definitions/select-all-settings-openapi.png" alt-text="Auswählen von „API Management“":::


1. Verwenden Sie die API Management-Einstellungen aus der folgenden Tabelle:

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Global eindeutiger Name | Es wird ein Name auf der Grundlage des Namens Ihrer Funktions-App generiert. |
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Ressource erstellt wird. |  
    | **[Ressourcengruppe](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Dieselbe Ressource wie Ihre Funktions-App, die für Sie festgelegt werden sollte. |
    | **Location** | Standort des Diensts | Ziehen Sie in Betracht, den gleichen Speicherort wie für Ihre Funktions-App zu wählen. |
    | **Name der Organisation** | Contoso | Der Name der Organisation, der im Entwicklerportal und für E-Mail-Benachrichtigungen verwendet wird. |
    | **Administrator-E-Mail** | Ihre E-Mail-Adresse | Die E-Mail-Adresse, die Benachrichtigungen des Systems von API Management erhält. |
    | **Preisstufe** | Nutzung | Der Tarif „Consumption“ ist nicht in allen Regionen verfügbar. Vollständige Preisinformationen finden Sie auf der [API Management-Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/api-management/) |

    ![Erstellen eines neuen API Management-Diensts](media/functions-openapi-definitions/new-apim-service-openapi.png)

1. Wählen Sie **Erstellen** aus, um die API Management-Instanz zu erstellen, was ein paar Minuten dauern kann.

1. Nach Erstellung der Instanz durch Azure wird die Option **Application Insights aktivieren** auf der Seite verfügbar. Wählen Sie sie aus, um Protokolle an den gleichen Ort zu senden wie die Funktionsanwendung aus.

## <a name="import-functions"></a>Funktionen importieren

Nachdem die API Management-Instanz erstellt wurde, können Sie Ihre über HTTP ausgelösten Funktionsendpunkte importieren. In diesem Beispiel wird ein Endpunkt namens „TurbineRepair“ importiert.   

1. Wählen Sie im API Management-Seite die Option **API verbinden** aus.

1. **Azure-Funktionen importieren** wird mit hervorgehobener Funktion **Turbinenreparatur** geöffnet. Wählen Sie **Auswählen** aus, um fortzufahren.

    ![Importieren von Azure-Funktionen in API Management](media/functions-openapi-definitions/import-function-openapi.png)

1. Übernehmen Sie auf der Seite **Aus Funktions-App erstellen** die Standardeinstellungen, und wählen Sie **Erstellen** aus.

    :::image type="content" source="media/functions-openapi-definitions/create-function-openapi.png" alt-text="Erstellen auf der Grundlage der Funktions-App":::

    Von Azure wird die API für die Funktion erstellt.

## <a name="download-the-openapi-definition"></a>Herunterladen der OpenAPI-Definition

Nachdem Ihre Funktionen importiert wurden, können Sie die OpenAPI-Definition aus API Management herunterladen.

1. Wählen Sie oben auf der Seite **OpenAPI-Definition herunterladen** aus.
   
   ![Herunterladen der OpenAPI-Definition](media/functions-openapi-definitions/download-definition.png)

2. Speichern Sie die heruntergeladene JSON-Datei, und öffnen Sie sie. Überprüfen Sie die Definition.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie die Definition in API Management über das Portal anpassen. Außerdem können Sie sich [ausführlicher über API Management](../api-management/api-management-key-concepts.md) informieren.

> [!div class="nextstepaction"]
> [Bearbeiten von APIs](../api-management/edit-api.md)
