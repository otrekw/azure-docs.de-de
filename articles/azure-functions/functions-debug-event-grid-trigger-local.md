---
title: Lokales Debuggen von Azure-Funktionen mit Event Grid
description: Informationen zum lokalen Debuggen von Azure-Funktionen, die durch ein Event Grid-Ereignis ausgelöst werden
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: cshoe
ms.openlocfilehash: 7483a097b188b9f96221a13964992c7b02332258
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107892037"
---
# <a name="azure-function-event-grid-trigger-local-debugging"></a>Lokales Debuggen von Azure-Funktionen mit Event Grid-Trigger

In diesem Artikel wird das Debuggen einer lokalen Funktion veranschaulicht, die ein Azure Event Grid-Ereignis verarbeitet, das von einem Speicherkonto ausgelöst wurde. 

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen oder Verwenden einer vorhandenen Funktions-App
- Erstellen oder Verwenden eines vorhandenen Speicherkontos
- Herunterladen von [ngrok](https://ngrok.com/) zum Ermöglichen des Aufrufs Ihrer lokalen Funktion in Azure

## <a name="create-a-new-function"></a>Erstellen einer neuen Funktion

Öffnen Sie Ihre Funktions-App in Visual Studio, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und klicken Sie auf **Hinzufügen > Neue Azure-Funktion**.

Wählen Sie im Fenster *Neue Azure-Funktion* die Option **Event Grid-Trigger** aus, und klicken Sie auf **OK**.

![Erstellen einer neuen Funktion](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-add-function.png)

Nachdem die Funktion erstellt wurde, öffnen Sie die Codedatei, und kopieren Sie die URL, die am Anfang der Datei auskommentiert ist. Dieser Speicherort wird beim Konfigurieren des Event Grid-Triggers verwendet.

![Speicherort kopieren](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-copy-location.png)

Legen Sie dann einen Haltepunkt in der Zeile fest, die mit `log.LogInformation` beginnt.

![Haltepunkt setzen](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-set-breakpoint.png)


Drücken Sie anschließend **F5**, um eine Debugsitzung zu starten.

[!INCLUDE [functions-event-grid-local-dev](../../includes/functions-event-grid-local-dev.md)]

## <a name="debug-the-function"></a>Debuggen der Funktion

Sobald Event Grid erkennt, dass eine neue Datei in den Speichercontainer hochgeladen wurde, wird der Haltepunkt in der lokalen Funktion erreicht.

![ngrok starten](./media/functions-debug-event-grid-trigger-local/functions-debug-event-grid-trigger-local-breakpoint.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um die in diesem Artikel erstellten Ressourcen zu bereinigen, löschen Sie den Container **test** in Ihrem Speicherkonto.

## <a name="next-steps"></a>Nächste Schritte

- [Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md)
- [Event Grid-Trigger für Azure Functions](./functions-bindings-event-grid.md)
