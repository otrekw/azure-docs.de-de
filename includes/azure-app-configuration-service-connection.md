---
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: include
ms.date: 05/03/2021
ms.openlocfilehash: 7b35a3c4b26640b872b161bd8130db728645f1d0
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108748276"
---
Über eine [Dienstverbindung](/azure/devops/pipelines/library/service-endpoints) können Sie von Ihrem Azure DevOps-Projekt aus auf Ressourcen in Ihrem Azure-Abonnement zugreifen.

1. Navigieren Sie in Azure DevOps zu dem Projekt, das Ihre Zielpipeline enthält. Wählen Sie in der unteren linken Ecke **Projekteinstellungen** aus.
1. Klicken Sie unter **Pipelines** auf **Dienstverbindungen**. Wählen Sie in der oberen rechten Ecke **Neue Dienstverbindung** aus.
1. Wählen Sie unter **Neue Dienstverbindung** die Option **Azure Resource Manager** aus.

    :::image type="content" source="./media/azure-app-configuration-service-connection/new-service-connection.png" alt-text="Screenshot des Auswählens von Azure Resource Manager in der Dropdownliste „Neue Dienstverbindung“.":::
1. Wählen Sie im Dialogfeld **Authentifizierungsmethode** die Option **Dienstprinzipal (automatisch)** aus, um einen neuen Dienstprinzipal zu erstellen. Alternativ können Sie **Dienstprinzipal (manuell)** auswählen, um [einen vorhandenen Dienstprinzipal zu verwenden](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops#use-spn&preserve-view=true).
1. Geben Sie Ihr Abonnement und Ihre Ressource sowie einen Namen für Ihre Dienstverbindung ein.

Wenn Sie einen neuen Dienstprinzipal erstellt haben, suchen Sie den Namen des Dienstprinzipals, der der Dienstverbindung zugewiesen ist. Diesem Dienstprinzipal wird im nächsten Schritt eine neue Rollenzuweisung hinzugefügt.

1. Navigieren Sie zu **Projekteinstellungen** > **Dienstverbindungen**.
1. Wählen Sie die neue Dienstverbindung aus.
1. Wählen Sie **Dienstprinzipal verwalten** aus.
1. Beachten Sie den Wert unter **Anzeigename**.

    :::image type="content" source="./media/azure-app-configuration-service-connection/service-principal-display-name.png" alt-text="Screenshot des Anzeigenamens des Dienstprinzipals.":::
