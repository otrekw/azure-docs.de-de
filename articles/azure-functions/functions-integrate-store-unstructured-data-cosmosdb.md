---
title: Speichern von unstrukturierten Daten mit Azure Cosmos DB und Functions
description: Speichern von unstrukturierten Daten mit Azure Functions und Cosmos DB
ms.topic: quickstart
ms.date: 10/01/2020
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: 5bc3895cb219338acde492b871dce806db70622b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91661158"
---
# <a name="store-unstructured-data-using-azure-functions-and-azure-cosmos-db"></a>Speichern von unstrukturierten Daten mit Azure Functions und Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) eignet sich hervorragend zum Speichern von unstrukturierten Daten und JSON-Daten. In Kombination mit Azure Functions ermöglicht Cosmos DB das schnelle und einfache Speichern von Daten. Dabei ist viel weniger Code erforderlich als beim Speichern von Daten in einer relationalen Datenbank.

> [!NOTE]
> Derzeit funktionieren der Azure Cosmos DB-Trigger, Eingabebindungen und Ausgabebindungen ausschließlich mit SQL- und Graph-API-Konten.

Eingabe- und Ausgabebindungen bieten in Azure Functions eine deklarative Möglichkeit, eine Verbindung mit externen Dienstdaten Ihrer Funktion herzustellen. In diesem Artikel erfahren Sie, wie Sie eine bereits vorhandene Funktion aktualisieren, um eine Ausgabebindung hinzuzufügen, die unstrukturierte Daten in einem Azure Cosmos DB-Dokument speichert.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Sie müssen über ein Azure Cosmos DB-Konto verfügen, für das die SQL-API verwendet wird, bevor Sie die Ausgabebindung erstellen.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

1. Navigieren Sie im Azure-Portal zu der zuvor erstellten Funktions-App, und wählen Sie sie aus.

1. Wählen Sie App **Funktionen** und dann „HTTP-Triggerfunktion“ aus.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-select-http-function.png" alt-text="Auswählen Ihrer HTTP-Funktion im Azure-Portal" border="true":::

1. Wählen Sie **Integration** und dann **+ Ausgabe hinzufügen** aus.

     :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-add-output-binding.png" alt-text="Hinzufügen einer Azure Cosmos DB-Ausgabebindung" border="true":::

1. Verwenden Sie die Einstellungen für **Ausgabe erstellen** wie in der folgenden Tabelle angegeben:

     :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-configure-cosmosdb-binding.png" alt-text="Konfigurieren einer Azure Cosmos DB-Ausgabebindung" border="true":::

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Bindungstyp** | Azure Cosmos DB | Der Name des Bindungstyps, der ausgewählt werden soll, um die Ausgabebindung mit Azure Cosmos DB zu erstellen. |
    | **Dokumentparametername** | taskDocument | Name, der auf das Cosmos DB-Objekt im Code verweist |
    | **Datenbankname** | taskDatabase | Name der Datenbank zum Speichern von Dokumenten |
    | **Sammlungsname** | taskCollection | Name der Datenbanksammlung. |
    | **If true, creates the Cosmos DB database and collection** (Erstellt die Cosmos DB-Datenbank und -Sammlung, falls zutreffend) | Ja | Die Sammlung ist noch nicht vorhanden und muss erstellt werden. |
    | **Cosmos DB-Kontoverbindung** | Neue Einstellung | Wählen Sie **Neu** > **Azure Cosmos DB-Konto** und dann das zuvor erstellte **Datenbankkonto** aus. Wählen Sie dann **OK** aus. Eine Anwendungseinstellung für Ihre Kontoverbindung wird erstellt. Diese Einstellung wird von der Bindung verwendet, um die Verbindung mit der Datenbank herzustellen. |

1. Wählen Sie **OK** aus, um die Bindung zu erstellen.

## <a name="update-the-function-code"></a>Aktualisieren des Funktionscodes

Ersetzen Sie den vorhandenen Funktionscode durch den folgenden Code in Ihrer gewählten Sprache:

# <a name="c"></a>[C#](#tab/csharp)

Ersetzen Sie die vorhandene C#-Funktion durch den folgenden Code:

```csharp
#r "Newtonsoft.Json"

using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

public static IActionResult Run(HttpRequest req, out object taskDocument, ILogger log)
{
    string name = req.Query["name"];
    string task = req.Query["task"];
    string duedate = req.Query["duedate"];

    // We need both name and task parameters.
    if (!string.IsNullOrEmpty(name) && !string.IsNullOrEmpty(task))
    {
        taskDocument = new
        {
            name,
            duedate,
            task
        };

        return (ActionResult)new OkResult();
    }
    else
    {
        taskDocument = null;
        return (ActionResult)new BadRequestResult();
    }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Ersetzen Sie die vorhandene JavaScript-Funktion durch den folgenden Code:

```js
module.exports = async function (context, req) {

    // We need both name and task parameters.
    if (req.query.name && req.query.task) {

        // Set the output binding data from the query object.
        context.bindings.taskDocument = req.query;

        // Success.
        context.res = {
            status: 200
        };
    }
    else {
        context.res = {
            status: 400,
            body: "The query options 'name' and 'task' are required."
        };
    }
};
```
---

Dieses Codebeispiel liest die Abfragezeichenfolgen der HTTP-Anforderung und weist sie Feldern im `taskDocument`-Objekt zu. Die `taskDocument`-Bindung sendet die Objektdaten aus diesem Bindungsparameter zur Speicherung an die gebundene Dokumentdatenbank. Die Datenbank wird bei der erstmaligen Ausführung der Funktion erstellt.

## <a name="test-the-function-and-database"></a>Testen der Funktion und der Datenbank

1. Wählen Sie **Testen/Ausführen** aus. Wählen Sie unter **Abfrage** die Option **+ Parameter hinzufügen** aus, und fügen Sie der Abfragezeichenfolge die folgenden Parameter hinzu:

    + `name`
    + `task`
    + `duedate`

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png" alt-text="Testen der Funktion" border="true":::


1. Wählen Sie **Ausführen** aus, und vergewissern Sie sich, dass der Status „200“ zurückgegeben wird.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function-output.png" alt-text="Screenshot: Hervorgehobener HTTP-Antwortcode mit Status „200“ nach der Auswahl von „Ausführen“" border="true":::


1. Suchen Sie im Azure-Portal nach **Azure Cosmos DB**, und wählen Sie den Eintrag aus.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png" alt-text="Suchen nach dem Cosmos DB-Dienst" border="true":::

1. Wählen Sie Ihr Azure Cosmos DB-Konto und dann **Daten-Explorer** aus.

1. Erweitern Sie die **TaskCollection**-Knoten, wählen Sie das neue Dokument aus, und vergewissern Sie sich, dass es Ihre Abfragezeichenfolgenwerte sowie einige zusätzliche Metadaten enthält.

    :::image type="content" source="./media/functions-integrate-store-unstructured-data-cosmosdb/functions-data-explorer-check-document.png" alt-text="Überprüfen der Zeichenfolgenwerte in Ihrem Dokument" border="true":::

Sie haben Ihrem HTTP-Trigger erfolgreich eine Bindung hinzugefügt, um unstrukturierte Daten in einer Azure Cosmos DB zu speichern.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Bindungen an eine Cosmos DB-Datenbank finden Sie unter [Cosmos DB-Bindungen in Azure Functions](functions-bindings-cosmosdb.md).

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps-2.md)]
