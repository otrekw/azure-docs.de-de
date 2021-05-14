---
title: include file
description: include file
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/21/2021
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f2dcb92d0a600c57d96348413704863285cea3f0
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107987187"
---
## <a name="allow-azure-to-call-your-local-function"></a>Erlauben des Aufrufs Ihrer lokalen Funktion in Azure

Um eine Funktion zu unterbrechen, die auf dem Computer gedebuggt wird, müssen Sie Azure die Kommunikation mit Ihrer lokalen Funktion von der Cloud aus ermöglichen.

Das Hilfsprogramm [ngrok](https://ngrok.com/) bietet Azure eine Möglichkeit zum Aufrufen der Funktion, die auf dem Computer ausgeführt wird. Starten Sie *ngrok* über den folgenden Befehl:

```bash
ngrok http -host-header=localhost 7071
```
Während das Hilfsprogramm eingerichtet wird, sollte das Befehlsfenster in etwa wie im folgenden Screenshot aussehen:

![Screenshot: Eingabeaufforderung nach dem Start des Hilfsprogramms „ngrok“](./media/functions-event-grid-local-dev/functions-event-grid-local-dev-ngrok.png)

Kopieren Sie die **HTTPS**-URL, die beim Ausführen von *ngrok* generiert wird. Dieser Wert wird beim Konfigurieren des Event Grid-Ereignisendpunkts verwendet.

## <a name="add-a-storage-event"></a>Hinzufügen eines Speicherereignisses

1. Öffnen Sie das Azure-Portal, navigieren Sie zu Ihrem Speicherkonto und wählen Sie im linken Menü die **Ereignisse**-Option aus.

    ![Speicherkontoereignis hinzufügen](./media/functions-event-grid-local-dev/functions-event-grid-local-dev-add-event.png)

1. Klicken Sie in der *Ereignisse*-Anzeige auf die Schaltfläche **Ereignisabonnement**. 

1. Wählen Sie in der Anzeige *Ereignis-Abonnement* das Dropdown-Menü *Endpunkttyp* aus und wählen Sie **Webhook** aus.

    ![Abonnementtyp auswählen](./media/functions-event-grid-local-dev/functions-event-grid-local-dev-event-subscription-type.png)

1. Nachdem der Endpunkttyp konfiguriert wurde, klicken Sie auf **Endpunkt auswählen**, um den Endpunktwert zu konfigurieren.

    ![Endpunkttyp auswählen](./media/functions-event-grid-local-dev/functions-event-grid-local-dev-event-subscription-endpoint.png)

    Der Wert *Abonnentenendpunkt* besteht aus drei verschiedenen Werten. Das Präfix ist die HTTPS-URL, die von *ngrok* generiert wird. Der Rest der URL stammt von der zuvor in der Anleitung kopierten localhost-URL, wobei der Funktionsname am Ende hinzugefügt wurde. Ausgehend von der localhost-URL, ersetzt die *ngrok*-URL `http://localhost:7071` und der Funktionsname ersetzt `{functionname}`.

1. Der folgende Screenshot zeigt ein Beispiel dafür, wie die endgültige URL aussehen sollte, wenn ein `Event Grid` Auslösertyp verwendet wird.

    ![Endpunktauswahl](./media/functions-event-grid-local-dev/functions-event-grid-local-dev-event-subscription-endpoint-selection.png)

1. Nachdem Sie den entsprechenden Wert eingegeben haben, klicken Sie auf **Auswahl bestätigen**.

> [!IMPORTANT]
> Bei jedem Start von *ngrok* wird die HTTPS-URL neu generiert, und der Wert ändert sich. Aus diesem Grund müssen Sie jedes Mal ein neues Ereignisabonnement erstellen, wenn Sie Ihre Funktion über *ngrok* für Azure verfügbar machen.

## <a name="upload-a-file"></a>Hochladen einer Datei

Jetzt können Sie eine Datei in Ihr Speicherkonto hochladen, um ein Event Grid-Ereignis zur Verarbeitung durch Ihre lokale Funktion auszulösen. 

Öffnen Sie den [Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) und verbinden Sie ihn mit Ihrem Speicherkonto. 

- Erweitern Sie **Blobcontainer**. 
- Klicken Sie mit der rechten Maustaste, und wählen Sie **Blobcontainer erstellen**.
- Benennen sie den Container als **samples-workitems**
- Wählen Sie den *samples-workitems*-Container aus
- Klicken Sie auf die Schaltfläche **Hochladen**.
- Klicken Sie auf **Dateien hochladen**.
- Wählen Sie eine Datei aus, und laden Sie sie in den Blobcontainer hoch.

