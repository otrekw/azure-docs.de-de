---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 5cb345ef2d20f75066e90f9e6478be27f925b1b0
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673371"
---
### <a name="retrieve-the-azure-storage-connection-string"></a>Abrufen der Azure Storage-Verbindungszeichenfolge

Zuvor haben Sie ein Azure Storage-Konto erstellt, das von der Funktions-App verwendet werden kann. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. Indem Sie die Einstellung in die Datei *local.settings.json* herunterladen, können Sie diese Verbindung zum Schreiben in eine Storage-Warteschlange unter demselben Konto verwenden, wenn Sie die Funktion lokal ausführen. 

1. Führen Sie im Stammverzeichnis des Projekts den folgenden Befehl aus, indem Sie `<app_name>` durch den Namen Ihrer Funktions-App aus der vorherigen Schnellstartanleitung ersetzen. Mit diesem Befehl werden alle vorhandenen Werte in der Datei überschrieben.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Öffnen Sie *local.settings.json*, und suchen Sie nach dem Wert mit dem Namen `AzureWebJobsStorage`. Dies ist die Verbindungszeichenfolge des Storage-Kontos. Sie verwenden den Namen `AzureWebJobsStorage` und die Verbindungszeichenfolge noch in anderen Abschnitten dieses Artikels.

> [!IMPORTANT]
> Da *local.settings.json* aus Azure heruntergeladene Geheimnisse enthält, sollten Sie diese Datei immer aus der Quellcodeverwaltung ausschließen. In der *GITIGNORE*-Datei, die für ein lokales Funktionsprojekt erstellt wird, ist die Datei standardmäßig ausgeschlossen.