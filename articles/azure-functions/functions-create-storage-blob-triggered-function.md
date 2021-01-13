---
title: Erstellen einer Funktion in Azure, die durch Blob Storage ausgelöst wird
description: Erstellen Sie mithilfe von Azure Functions eine serverlose Funktion, die aufgerufen wird, wenn Elemente einem Blob Storage-Container hinzugefügt werden.
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.topic: how-to
ms.date: 10/01/2018
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: bf6865d2756579f457dded90b247326d2eec137c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83123001"
---
# <a name="create-a-function-in-azure-thats-triggered-by-blob-storage"></a>Erstellen einer Funktion in Azure, die durch Blob Storage ausgelöst wird

Erfahren Sie, wie Sie eine Funktion erstellen, die ausgelöst wird, wenn Dateien in einen Blob Storage-Container hochgeladen oder aktualisiert werden.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein Azure-Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Sie haben Ihre neue Funktions-App erfolgreich erstellt.

:::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-create-success.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Erstellen Sie als Nächstes eine Funktion in der neuen Funktions-App.

<a name="create-function"></a>

## <a name="create-an-azure-blob-storage-triggered-function"></a>Erstellen einer Funktion, die durch Azure Blob Storage ausgelöst wird

1. Wählen Sie **Funktionen** und dann **+ Hinzufügen** aus, um eine neue Funktion hinzuzufügen.

   :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-quickstart-choose-template.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Wählen Sie die Vorlage **Azure Blob Storage-Trigger** aus.

1. Verwenden Sie die Einstellungen, die in der Tabelle unter der Abbildung angegeben sind.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-2.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

    | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|
    | **Neue Funktion** | Eindeutig in Ihrer Funktions-App | Der Name dieser durch Blobs ausgelösten Funktion. |
    | **Pfad**   | samples-workitems/{name}    | Der Speicherort in Blob Storage, der überwacht wird. Der Dateiname des Blobs wird in der Bindung als Parameter _name_ übergeben.  |
    | **Speicherkontoverbindung** | AzureWebJobsStorage | Sie können die Speicherkontoverbindung verwenden, die bereits von Ihrer Funktions-App verwendet wird, oder eine neue erstellen.  |

1. Wählen Sie **Funktion erstellen** aus, um Ihre Funktion zu erstellen.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal-3.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Erstellen Sie nun den **samples-workitems**-Container.

## <a name="create-the-container"></a>Erstellen des Containers

1. Wählen Sie in Ihrer Funktion auf der Seite **Übersicht** die Ressourcengruppe aus.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-resource-group.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Suchen Sie das Speicherkonto Ihrer Ressourcengruppe, und wählen Sie es aus.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-account-access.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Wählen Sie **Container** aus, und wählen Sie dann **+ Container** aus. 

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-add-container.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Geben Sie im Feld **Name** den Namen `samples-workitems` ein, und wählen Sie dann **Erstellen** aus.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-name-blob-container.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Nun verfügen Sie über einen Blob-Container und können die Funktion durch Hochladen einer Datei in den Container testen.

## <a name="test-the-function"></a>Testen der Funktion

1. Kehren Sie zum Azure-Portal zurück, navigieren Sie zu Ihrer Funktion, erweitern Sie die **Protokolle** am unteren Rand der Seite, und vergewissern Sie sich, dass das Protokollstreaming nicht angehalten ist.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-log-expander.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Navigieren Sie in einem separaten Browserfenster zu Ihrer Ressourcengruppe im Azure-Portal, und wählen Sie das Speicherkonto aus.

1. Wählen Sie **Container** aus, und wählen Sie dann den Container **samples-workitems** aus.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-container.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Wählen Sie **Hochladen** aus, und wählen Sie dann das Ordnersymbol aus, um eine hochzuladende Datei auszuwählen.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Navigieren Sie zu einer Datei auf dem lokalen Computer, z. B. zu einer Bilddatei, und wählen Sie die Datei aus. Klicken Sie auf **Öffnen** und dann auf **Hochladen**.

1. Kehren Sie zu den Funktionsprotokollen zurück, und überprüfen Sie, ob das Blob gelesen wurde.

    :::image type="content" source="./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

    >[!NOTE]
    > Wenn Ihre Funktions-App im Standardverbrauchsplan ausgeführt wird, kann es zwischen dem Hinzufügen oder Aktualisieren des Blobs und dem Auslösen der Funktion zu einer Verzögerung von mehreren Minuten kommen. Wenn Sie die Wartezeit bei Ihren durch Blobs ausgelösten Funktionen gering halten möchten, können Sie die Funktions-App in einem App Service-Plan ausführen.

## <a name="clean-up-resources"></a>Bereinigen der Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die ausgeführt wird, wenn ein Blob in Blob Storage hinzugefügt oder aktualisiert wird. Weitere Informationen zu Blob Storage-Triggern finden Sie unter [Azure Functions – Blob Storage-Bindungen](functions-bindings-storage-blob.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
