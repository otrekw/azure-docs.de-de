---
title: Verschieben von Azure Event Grid-Systemthemen in eine andere Region
description: In diesem Artikel erfahren Sie, wie Sie Azure Event Grid-Systemthemen aus der aktuellen Region in eine andere Region verschieben.
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/28/2020
ms.openlocfilehash: eb6029b206e7d47789371ee81e75c4e05c69ee65
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89082202"
---
# <a name="move-azure-event-grid-system-topics-to-another-region"></a>Verschieben von Azure Event Grid-Systemthemen in eine andere Region
Sie möchten Ihre Ressourcen ggf. aus verschiedenen Gründen in eine andere Region verschieben. Beispielsweise, um eine neue Azure-Region zu nutzen, interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf Anforderungen der Kapazitätsplanung. 

Folgende allgemeine Schritte werden in diesem Artikel behandelt: 

- **Exportieren der Ressourcengruppe**, die das Azure Storage-Konto enthält, und des zugehörigen Systemthemas in eine Resource Manager-Vorlage. Sie können auch eine Vorlage nur für das Systemthema exportieren. Wenn Sie sich für diesen Weg entscheiden, denken Sie daran, die Azure-Ereignisquelle (in diesem Beispiel ein Azure Storage-Konto) in die andere Region zu verschieben, bevor Sie das Systemthema verschieben. Aktualisieren Sie dann in der exportierten Vorlage für das Systemthema die externe ID für das Speicherkonto in der Zielregion. 
- **Ändern der Vorlage**, um die `endpointUrl`-Eigenschaft hinzuzufügen, um auf einen Webhook zu verweisen, der das Systemthema abonniert. Beim Exportieren des Systemthemas wird das zugehörige Abonnement (in diesem Fall ein Webhook) ebenfalls in die Vorlage exportiert, aber die `endpointUrl`-Eigenschaft wird nicht eingeschlossen. Daher müssen Sie sie aktualisieren, um auf den Endpunkt zu verweisen, der das Thema abonniert. Aktualisieren Sie außerdem den Wert der `location`-Eigenschaft auf den neuen Speicherort oder die neue Region. Für andere Typen von Ereignishandlern müssen Sie nur den Speicherort aktualisieren. 
- **Verwenden der Vorlage zum Bereitstellen von Ressourcen** in der Zielregion. Sie geben Namen für das Speicherkonto und das Systemthema an, das in der Zielregion erstellt werden soll. 
- **Überprüfen der Bereitstellung**. Vergewissern Sie sich, dass der Webhook aufgerufen wird, wenn Sie eine Datei in den Blobspeicher in der Zielregion hochladen. 
- Um den **Verschiebevorgang abzuschließen**, löschen Sie Ressourcen (Ereignisquelle und Systemthema) aus der Quellregion. 

## <a name="prerequisites"></a>Voraussetzungen
- Schließen Sie den [Schnellstart: Weiterleiten von Blob Storage-Ereignissen an einen Webendpunkt über das Azure-Portal](blob-event-quickstart-portal.md) in der Quellregion ab. Dieser Schritt ist **optional**. Führen Sie ihn aus, um die Schritte in diesem Artikel zu testen. Behalten Sie das Speicherkonto in einer separaten Ressourcengruppe vom App Service und App Service-Plan bei. 
- Stellen Sie sicher, dass der Event Grid-Dienst in der Zielregion verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

## <a name="prepare"></a>Vorbereiten
Exportieren Sie als ersten Schritt eine Resource Manager-Vorlage für die Ressourcengruppe, die die Systemereignisquelle (Azure Storage-Konto) und das zugehörige Systemthema enthält. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü die Option **Ressourcengruppen** aus. Wählen Sie anschließend die Ressourcengruppe aus, die die Ereignisquelle enthält, für die das Systemthema erstellt wurde. Im folgenden Beispiel handelt es sich um das **Azure Storage**-Konto. Die Ressourcengruppe enthält das Speicherkonto und das zugehörige Systemthema. 

    :::image type="content" source="./media/move-system-topics-across-regions/resource-group-page.png" alt-text="Seite „Ressourcengruppe“":::        
3. Wählen Sie im Menü auf der linken Seite **Vorlage exportieren** unter **Einstellungen** aus, und wählen Sie dann **Herunterladen** auf der Symbolleiste aus. 

    :::image type="content" source="./media/move-system-topics-across-regions/export-template-menu.png" alt-text="Speicherkonto: Seite „Vorlage exportieren“":::        
5. Suchen Sie die aus dem Portal heruntergeladene **ZIP**-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl. Diese ZIP-Datei enthält die JSON-Dateien mit Vorlagen und Parametern. 
1. Öffnen Sie die Datei **template.json** in einem Editor Ihrer Wahl. 
1. Die URL für den Webhook wird nicht in die Vorlage exportiert. Führen Sie daher die folgenden Schritte aus:
    1. Suchen Sie in der Vorlagendatei nach **WebHook**. 
    1. Fügen Sie im Abschnitt **Properties** (Eigenschaften) am Ende der letzten Zeile ein Komma (`,`) hinzu. In diesem Beispiel lautet er `"preferredBatchSizeInKilobytes": 64`. 
    1. Fügen Sie die `endpointUrl`-Eigenschaft mit dem Wert hinzu, der auf Ihre Webhook-URL festgelegt ist, wie im folgenden Beispiel gezeigt. 

        ```json
        "destination": {
            "properties": {
                "maxEventsPerBatch": 1,
                "preferredBatchSizeInKilobytes": 64,
                "endpointUrl": "https://mysite.azurewebsites.net/api/updates"
            },
            "endpointType": "WebHook"
        }
        ```

        > [!NOTE]
        > Für andere Typen von Ereignishandlern werden alle Eigenschaften in die Vorlage exportiert. Sie müssen die nur `location`-Eigenschaft in die Zielregion aktualisieren, wie im nächsten Schritt gezeigt. 
7. Aktualisieren Sie `location` für die **Speicherkontoressource** in die Zielregion oder den Zielspeicherort. Informationen zum Abrufen von Standortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/). Der Code für eine Region ist der Name der Region ohne Leerzeichen, z. B. ist `West US` gleich `westus`.

    ```json
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-06-01",
    "name": "[parameters('storageAccounts_spegridstorage080420_name')]",
    "location": "westus",
    ```
8. Wiederholen Sie den Schritt zum Aktualisieren von `location` für die **Systemthemaressource** in der Vorlage. 

    ```json
    "type": "Microsoft.EventGrid/systemTopics",
    "apiVersion": "2020-04-01-preview",
    "name": "[parameters('systemTopics_spegridsystopic080420_name')]",
    "location": "westus",
    ```
1. **Speichern** Sie die Vorlage. 

## <a name="recreate"></a>Neu erstellen 
Stellen Sie die Vorlage zum Erstellen eines Speicherkontos und eines Systemthemas für das Speicherkonto in der Zielregion bereit. 

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.
3. Wählen Sie **Vorlagenbereitstellung** aus.
4. Klicken Sie auf **Erstellen**.
5. Wählen Sie **Eigene Vorlage im Editor erstellen**.
6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen haben.
7. Wählen Sie **Speichern** aus, um die Vorlage zu speichern. 
8. Gehen Sie auf der Seite **Benutzerdefinierte Bereitstellung** wie folgt vor. 
    1. Wählen Sie ein Azure-**Abonnement** aus. 
    1. Wählen Sie eine vorhandene **Ressourcengruppe** in der Zielregion aus, oder erstellen Sie eine Ressourcengruppe. 
    1. Wählen als **Region** die Zielregion aus. Wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben, ist diese Einstellung schreibgeschützt.
    1. Geben Sie als **Systemthemanamen** einen Namen für das Systemthema ein, der dem Speicherkonto zugeordnet wird.  
    1. Geben Sie als **Speicherkontonamen** einen Namen für das Speicherkonto ein, das in der Zielregion erstellt werden soll. 

        :::image type="content" source="./media/move-system-topics-across-regions/deploy-template.png" alt-text="Bereitstellen der Resource Manager-Vorlage":::
    5. Wählen Sie am unteren Rand der Seite die Option **Bewerten + erstellen** aus. 
    1. Überprüfen Sie die Einstellungen auf der Seite **Überprüfen und erstellen**, und wählen Sie **Erstellen** aus. 

## <a name="verify"></a>Überprüfung
1. Wählen Sie nach der erfolgreichen Bereitstellung **Zu Ressourcengruppe wechseln** aus. 
1. Überprüfen Sie auf der Seite **Ressourcengruppe**, ob die Ereignisquelle (in diesem Beispiel das Azure Storage-Konto) und das Systemthema erstellt wurden. 
1. Laden Sie eine Datei in einen Container in Azure Blob Storage hoch, und bestätigen Sie, dass der Webhook das Ereignis empfangen hat. Weitere Informationen finden Sie unter [Senden eines Ereignisses an Ihren Endpunkt](blob-event-quickstart-portal.md#send-an-event-to-your-endpoint).

## <a name="discard-or-clean-up"></a>Verwerfen oder Bereinigen
Um den Verschiebevorgang abzuschließen, löschen Sie die Ressourcengruppe, die das Speicherkonto und das zugehörige Systemthema in der Quellregion enthält.  

Wenn Sie von vorn beginnen möchten, löschen Sie die Ressourcengruppe in der Zielregion, und wiederholen Sie die Schritte in den Abschnitten [Vorbereiten](#prepare) und [Neu erstellen](#recreate) dieses Artikels.

So löschen Sie eine Ressourcengruppe (Quelle oder Ziel) im Azure-Portal:

1. Geben Sie im Suchfenster oben im Azure-Portal **Ressourcengruppen** ein, und wählen Sie in den Suchergebnissen **Ressourcengruppen** aus. 
2. Wählen Sie die zu löschende Ressourcengruppe und dann auf der Symbolleiste **Löschen** aus. 

    :::image type="content" source="./media/move-system-topics-across-regions/delete-resource-group-button.png" alt-text="Ressourcengruppe löschen":::
3. Geben Sie auf der Bestätigungsseite den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.  

## <a name="next-steps"></a>Nächste Schritte
Sie haben gelernt, wie Sie eine Azure-Ereignisquelle und das zugehörige Systemthema aus einer Region in eine andere Region verschieben. In den folgenden Artikeln finden Sie Informationen zum regionsübergreifenden Verschieben von benutzerdefinierten Themen, Domänen und Partnernamespaces.

- [Regionsübergreifendes Verschieben von benutzerdefinierten Themen](move-custom-topics-across-regions.md). 
- [Regionsübergreifendes Verschieben von Domänen](move-domains-across-regions.md). 
- [Regionsübergreifendes Verschieben von Partnernamespaces](move-partner-namespaces-across-regions.md). 

Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie im folgenden Artikel: [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/management/move-resource-group-and-subscription.md).
