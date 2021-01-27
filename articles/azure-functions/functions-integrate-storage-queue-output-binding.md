---
title: Hinzufügen von Meldungen in die Warteschlange von Azure Storage mithilfe von Functions
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die durch eine HTTP-Anforderung aufgerufen wird und eine Meldung in einer Azure Storage-Warteschlange erstellt.
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.topic: how-to
ms.date: 04/24/2020
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: d2821a16e0b72b32cc392b7ae626d782734458a6
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98674200"
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Hinzufügen von Meldungen in die Warteschlange von Azure Storage mithilfe von Functions

Mithilfe von Eingabe- und Ausgabebindungen in Azure Functions können Sie Daten aus externen Diensten deklarativ für Ihren Code verfügbar machen. In dieser Schnellstartanleitung verwenden Sie eine Ausgabebindung, um eine Meldung in einer Warteschlange zu erstellen, wenn eine Funktion durch eine HTTP-Anforderung ausgelöst wird. Die von Ihrer Funktion erstellten Warteschlangenmeldungen zeigen Sie mithilfe des Azure Speichercontainers an.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

- Ein Azure-Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Folgen Sie der Anleitung unter [Erstellen Ihrer ersten Funktion im Azure-Portal](./functions-get-started.md), lassen Sie dabei aber den Schritt **Bereinigen von Ressourcen** weg. In dieser Schnellstartanleitung werden die Funktions-App und die Funktion erstellt, die hier verwendet werden.

## <a name="add-an-output-binding"></a><a name="add-binding"></a>Ausgabebindung hinzufügen

In diesem Abschnitt verwenden Sie die Benutzeroberfläche des Portals, um der zuvor erstellten Funktion eine Queue Storage-Ausgabebindung hinzuzufügen. Durch diese Bindung können Sie mit minimalem Programmieraufwand eine Meldung in einer Warteschlange erstellen. Sie müssen keinen Code für Aufgaben wie das Öffnen einer Speicherverbindung, das Erstellen einer Warteschlange oder das Abrufen eines Verweises auf eine Warteschlange schreiben. Die Azure Functions-Laufzeit und die Warteschlangen-Ausgabebindung nehmen Ihnen diese Aufgaben ab.

1. Öffnen Sie im Azure-Portal die Seite „Funktions-App“ für die Funktions-App, die Sie in [Erstellen Ihrer ersten Funktion im Azure-Portal](./functions-get-started.md) erstellt haben. Um die Seite zu öffnen, suchen Sie nach **Funktions-App** und wählen die Option dann aus. Wählen Sie Ihre Funktions-App dann aus.

1. Wählen Sie Ihre Funktions-App und dann die Funktion aus, die Sie im Rahmen der vorherigen Schnellstartanleitung erstellt haben.

1. Wählen Sie **Integration** aus, und wählen Sie dann **+ Ausgabe hinzufügen** aus.

   :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-create-output-binding.png" alt-text="Erstellen einer Ausgabebindung für die Funktion." border="true":::

1. Wählen Sie den Bindungstyp **Azure Queue Storage** aus, und fügen Sie die Einstellungen aus der Tabelle nach dem folgenden Screenshot hinzu: 

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-create-output-binding-details.png" alt-text="Hinzufügen einer Ausgabebindung von Queue Storage zu einer Funktion im Azure-Portal." border="true":::
    
    | Einstellung      |  Vorgeschlagener Wert   | BESCHREIBUNG                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name des Meldungsparameters** | outputQueueItem | Der Name des Ausgabebindungsparameters | 
    | **Warteschlangenname**   | outqueue  | Der Name der zu verknüpfenden Warteschlange in Ihrem Speicherkonto. |
    | **Speicherkontoverbindung** | AzureWebJobsStorage | Sie können die Speicherkontoverbindung verwenden, die bereits von Ihrer Funktions-App verwendet wird, oder eine neue erstellen.  |

1. Klicken Sie auf **OK**, um die Bindung hinzuzufügen.

Nachdem Sie eine Ausgabebindung definiert haben, müssen Sie den Code aktualisieren, um die Bindung zum Hinzufügen von Meldungen in eine Warteschlange verwenden zu können.  

## <a name="add-code-that-uses-the-output-binding"></a>Hinzufügen von Code, der die Ausgabebindung verwendet

In diesem Abschnitt fügen Sie Code hinzu, der eine Meldung in die Ausgabewarteschlange schreibt. Die Meldung enthält den Wert, der an den HTTP-Trigger in der Abfragezeichenfolge übergeben wird. Wenn die Abfragezeichenfolge also beispielsweise `name=Azure` enthält, lautet die Warteschlangenmeldung *Name passed to the function: Azure* (An die Funktion übergebener Name: Azure).

1. Wählen Sie in Ihrer Funktion **Code+ Test** aus, um den Funktionscode im Editor anzuzeigen.

1. Aktualisieren Sie den Funktionscode gemäß Ihrer Funktionssprache:

    # <a name="c"></a>[C\#](#tab/csharp)

    Fügen Sie der Methodensignatur einen Parameter vom Typ **outputQueueItem** hinzu, wie im folgenden Beispiel zu sehen.

    ```cs
    public static async Task<IActionResult> Run(HttpRequest req,
        ICollector<string> outputQueueItem, ILogger log)
    {
        ...
    }
    ```

    Fügen Sie im Text der Funktion kurz vor der `return`-Anweisung Code hinzu, der unter Verwendung des Parameters eine Warteschlangenmeldung erstellt.

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);
    ```

    # <a name="javascript"></a>[JavaScript](#tab/nodejs)

    Fügen Sie Code hinzu, der die Ausgabebindung für das `context.bindings`-Objekt verwendet, um eine Warteschlangenmeldung zu erstellen. Fügen Sie diesen Code vor der `context.done`-Anweisung hinzu.

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ---

1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

## <a name="test-the-function"></a>Testen der Funktion

1. Nachdem die Codeänderungen gespeichert wurden, klicken Sie auf **Testen**.
1. Vergewissern Sie sich, dass der Test der folgenden Abbildung entspricht, und wählen Sie **Ausführen** aus. 

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png" alt-text="Testen der Queue Storage-Bindung im Azure-Portal." border="true":::

    Beachten Sie, dass der **Anforderungstext** den `name`-Wert *Azure* enthält. Dieser Wert erscheint in der Warteschlangenmeldung, die erstellt wird, wenn die Funktion aufgerufen wird.
    
    Anstatt auf **Ausführen** zu klicken, können Sie die Funktion auch aufrufen, indem Sie eine URL in einen Browser eingeben und den `name`-Wert in der Abfragezeichenfolge angeben. Die Browsermethode wird in der [vorherigen Schnellstartanleitung](./functions-get-started.md) gezeigt.

1. Überprüfen Sie die Protokolle, um sicherzustellen, dass die Funktion erfolgreich durchgeführt wurde. 

Eine neue Warteschlange mit dem Namen **outqueue** wird in Ihrem Speicherkonto von der Laufzeit von Azure Functions erstellt, wenn die Ausgabebindung zum ersten Mal verwendet wird. Sie vergewissern sich mithilfe des Speicherkontos, dass die Warteschlange und eine Meldung darin erstellt wurden.

### <a name="find-the-storage-account-connected-to-azurewebjobsstorage"></a>Suchen nach dem mit AzureWebJobsStorage verbundenen Speicherkonto


1. Wechseln Sie zu ihrer Funktions-App, und wählen Sie **Konfiguration** aus.

1. Wählen Sie unter **Anwendungseinstellungen** die Option **AzureWebJobsStorage** aus.

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-find-storage-account.png" alt-text="Screenshot der Seite „Konfiguration“ mit Auswahl von AzureWebJobsStorage." border="true":::

1. Ermitteln Sie den Kontonamen, und notieren Sie ihn sich.

    :::image type="content" source="./media/functions-integrate-storage-queue-output-binding/function-storage-account-name.png" alt-text="Suchen nach dem mit AzureWebJobsStorage verbundenen Speicherkonto." border="true":::

### <a name="examine-the-output-queue"></a>Überprüfen der Ausgabewarteschlange

1. Wählen Sie in der Ressourcengruppe für Ihre Funktions-App das Speicherkonto aus, das Sie im Rahmen dieses Schnellstarts verwenden.

1. Wählen Sie unter **Warteschlangendienst** die Option **Warteschlangen** aus, und wählen Sie dann die Warteschlange mit dem Namen **outqueue** aus. 

   Die Warteschlange enthält die Meldung, die die Warteschlangen-Ausgabebindung erstellt hat, als Sie die per HTTP ausgelöste Funktion ausgeführt haben. Wenn Sie die Funktion mit dem `name`-Standardwert *Azure* aufgerufen haben, lautet die Warteschlangenmeldung *Name passed to the function: Azure* (An die Funktion übergebener Name: Azure).

1. Führen Sie die Funktion erneut aus. Daraufhin erscheint eine weitere Meldung in der Warteschlange.  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einer vorhandenen Funktion eine Ausgabebindung hinzugefügt. Weitere Informationen zu Bindungen an Queue Storage finden Sie unter [Azure Storage-Warteschlangenbindungen in Azure Functions](functions-bindings-storage-queue.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps-2.md)]
