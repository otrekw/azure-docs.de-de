---
title: Erstellen einer Funktion in Azure, die durch Warteschlangenmeldungen ausgelöst wird
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die durch eine Meldung an eine Warteschlange in Azure aufgerufen wird.
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: c4c20579f2306b61741f3c6ab1549285271435a3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123222"
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Erstellen einer Funktion, die durch Azure Queue Storage ausgelöst wird

Erfahren Sie, wie Sie eine Funktion erstellen, die ausgelöst wird, wenn Meldungen an eine Azure Storage-Warteschlange gesendet werden.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-create-success.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Erstellen Sie als Nächstes eine Funktion in der neuen Funktions-App.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Erstellen einer Funktion mit Auslösung durch Warteschlange

1. Wählen Sie **Funktionen** und dann **+ Hinzufügen** aus, um eine neue Funktion hinzuzufügen.

   :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-app-quickstart-choose-template.png" alt-text="Auswählen einer Funktionsvorlage im Azure-Portal." border="true":::

1. Wählen Sie die Vorlage **Azure Queue Storage-Trigger** aus.

1. Verwenden Sie die Einstellungen, die in der Tabelle unter der Abbildung angegeben sind.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png" alt-text="Benennen und Konfigurieren der Funktion, die durch den Warteschlangenspeicher ausgelöst wird." border="true":::


    | Einstellung | Vorgeschlagener Wert | Beschreibung |
    |---|---|---|
    | **Name** | Eindeutig in Ihrer Funktions-App | Der Name dieser Funktion mit Auslösung durch Warteschlange. |
    | **Warteschlangenname**   | myqueue-items    | Der Name der zu verknüpfenden Warteschlange in Ihrem Speicherkonto. |
    | **Speicherkontoverbindung** | AzureWebJobsStorage | Sie können die Speicherkontoverbindung verwenden, die bereits von Ihrer Funktions-App verwendet wird, oder eine neue erstellen.  |    

1. Wählen Sie **Funktion erstellen** aus, um Ihre Funktion zu erstellen.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal-3.png" alt-text="Erstellen der Funktion, die durch Warteschlangenspeicher ausgelöst wird." border="true":::

Als Nächstes stellen Sie eine Verbindung zu Ihrem Azure Storage-Konto her und erstellen die **myqueue-items**-Speicherwarteschlange.

## <a name="create-the-queue"></a>Erstellen der Warteschlange

1. Wählen Sie in Ihrer Funktion auf der Seite **Übersicht** die Ressourcengruppe aus.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-resource-group.png" alt-text="Auswählen der Ressourcengruppe im Azure-Portal." border="true":::

1. Suchen Sie das Speicherkonto Ihrer Ressourcengruppe, und wählen Sie es aus.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-account-access.png" alt-text="Zugriff auf das Speicherkonto." border="true":::

1. Wählen Sie **Warteschlangen** aus, und wählen Sie dann **+ Warteschlangen** aus. 

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-add-queue.png" alt-text="Hinzufügen einer Warteschlange zu Ihrem Speicherkonto im Azure-Portal." border="true":::

1. Geben Sie im Feld **Name** den Namen `myqueue-items` ein, und wählen Sie dann **Erstellen** aus.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-name-queue.png" alt-text="Benennen des Warteschlangenspeicher-Containers." border="true":::

Nun verfügen Sie über eine Speicherwarteschlange und können die Funktion durch Hinzufügen einer Meldung in die Warteschlange testen.

## <a name="test-the-function"></a>Testen der Funktion

1. Kehren Sie zum Azure-Portal zurück, navigieren Sie zu Ihrer Funktion, erweitern Sie die **Protokolle** am unteren Rand der Seite, und vergewissern Sie sich, dass das Protokollstreaming nicht angehalten ist.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-queue-storage-log-expander.png" alt-text="Erweitern des Protokolls im Azure-Portal." border="true":::

1. Navigieren Sie in einem separaten Browserfenster zu Ihrer Ressourcengruppe im Azure-Portal, und wählen Sie das Speicherkonto aus.

1. Wählen Sie **Warteschlangen** aus, und wählen Sie dann den Container **myqueue-items** aus.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue.png" alt-text="Navigieren zur Warteschlange „myqueue-items“ im Azure-Portal." border="true":::

1. Wählen Sie **Meldung hinzufügen** aus, und geben Sie „Hello World!“ in **Meldungstext** ein. Klicken Sie auf **OK**.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/functions-storage-queue-test.png" alt-text="Navigieren zur Warteschlange „myqueue-items“ im Azure-Portal." border="true":::

1. Warten Sie einige Sekunden, wechseln Sie dann zurück zu den Funktionsprotokollen, und stellen Sie sicher, dass die neue Meldung aus der Warteschlange gelesen wurde.

    :::image type="content" source="./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png" alt-text="Anzeigen der Meldung in den Protokollen." border="true":::

1. Wählen Sie in der Speicherwarteschlange **Aktualisieren** aus, und stellen Sie sicher, dass die Meldung verarbeitet wurde und sich nicht mehr in der Warteschlange befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn eine Meldung in eine Speicherwarteschlange hinzugefügt wird. Weitere Informationen zu Queue Storage-Auslösern finden Sie unter [Azure Storage-Warteschlangenbindungen in Azure Functions](functions-bindings-storage-queue.md).

Sie haben Ihre erste Funktion erstellt. Fügen Sie ihr nun eine Ausgabebindung hinzu, die eine Meldung in eine andere Warteschlange schreibt.

> [!div class="nextstepaction"]
> [Hinzufügen von Meldungen in eine Azure Storage-Warteschlange mithilfe von Functions](functions-integrate-storage-queue-output-binding.md)
