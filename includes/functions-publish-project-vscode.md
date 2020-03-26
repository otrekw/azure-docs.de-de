---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: 256510f855256e648ae9203f46eb9f66c9ffaed6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "77029112"
---
## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

In diesem Abschnitt erstellen Sie eine Funktions-App sowie zugehörige Ressourcen in Ihrem Azure-Abonnement und stellen anschließend Ihren Code bereit. 

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    ![Veröffentlichen Ihres Projekts in Azure](media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Wählen Sie das Abonnement aus:** Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.

    + **Select Function App in Azure:** (Wählen Sie die Funktions-App in Azure aus:) Wählen Sie `+ Create new Function App` aus (nicht `Advanced`). In diesem Artikel wird der [erweiterte Veröffentlichungsvorgang](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options) nicht unterstützt. 
    
    >[!IMPORTANT]
    > Beim Veröffentlichen in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben. 
    
    + **Enter a globally unique name for the function app:** (Geben Sie einen global eindeutigen Namen für die Funktions-App ein:) Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist. Der eingegebene Name wird überprüft, um sicherzustellen, dass er in Azure Functions eindeutig ist. 
    
    ::: zone pivot="programming-language-python"
    + **Select a runtime:** (Wählen Sie eine Runtime aus:) Wählen Sie die lokal ausgeführte Python-Version aus. Sie können den Befehl `python --version` ausführen, um Ihre Version zu überprüfen.
    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript"
    + **Select a runtime:** (Wählen Sie eine Runtime aus:) Wählen Sie die lokal ausgeführte Node.js-Version aus. Sie können den Befehl `node --version` ausführen, um Ihre Version zu überprüfen.
    ::: zone-end

    + **Wählen Sie einen Speicherort für neue Ressourcen aus:**  Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen. 
    
1.  Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt:

    + **[Ressourcengruppe](../articles/azure-resource-manager/management/overview.md)** : Enthält alle erstellten Azure-Ressourcen. Der Name basiert auf dem Namen Ihrer Funktions-App.
    + **[Speicherkonto](../articles//storage/common/storage-introduction.md#types-of-storage-accounts)** : Ein Standardspeicherkonto wird mit einem eindeutigen Namen erstellt, der auf dem Namen Ihrer Funktions-App basiert.
    + **[Hostingplan](../articles/azure-functions/functions-scale.md)** : Ein Verbrauchsplan wird in der Region „USA, Westen“ erstellt, um Ihre serverlose Funktions-App zu hosten.
    + **Funktions-App**: Ihr Projekt wird in dieser neuen Funktions-App bereitgestellt, wo es auch ausgeführt wird.
    + **Application Insights**: Eine mit Ihrer Funktions-App verbundene Instanz wird auf der Grundlage Ihres Funktionsnamens erstellt.

    Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. 
    
1. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen. Wenn Sie die Benachrichtigung übersehen haben, wählen Sie das Glockensymbol in der unteren rechten Ecke aus, um sie erneut anzuzeigen.

    ![Benachrichtigung nach Abschluss der Erstellung](media/functions-publish-project-vscode/function-create-notifications.png)
