---
title: Regionen übergreifendes Verschieben Ihrer Dienstressourcen
titleSuffix: Azure Cognitive Search
description: In diesem Artikel erfahren Sie, wie Sie Ihre Azure Cognitive Search-Ressourcen in der Azure-Cloud von einer Region in eine andere verschieben.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 02/18/2020
ms.openlocfilehash: 392c86d8ea24e59d388926d4df581305ea2b531d
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599635"
---
# <a name="move-your-azure-cognitive-search-service-to-another-azure-region"></a>Verschieben Ihres Azure Cognitive Search-Diensts in eine andere Azure-Region

Um Ihr Azure Cognitive Services-Konto von einer Region in eine andere zu verschieben, erstellen Sie eine Exportvorlage zum Verschieben Ihrer Abonnements. Nach dem Verschieben Ihres Abonnements müssen Sie Ihre Daten verschieben und den Dienst neu erstellen.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Exportieren einer Vorlage.
> * Ändern Sie die Vorlage durch Hinzufügen der Namen von Zielregion, Suche und Speicherkonto.
> * Stellen Sie die Vorlage zum Erstellen der neuen Suche und der neuen Speicherkonten bereit.
> * Überprüfen Ihres Dienststatus in der neuen Region
> * Bereinigen Sie die Ressourcen in der Quellregion.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass die Dienste und Features, die von Ihrem Konto verwendet werden, in der Zielregion unterstützt werden.

- Stellen Sie für Previewfunktionen sicher, dass Ihr Abonnement für die Zielregion auf der Whitelist steht. Weitere Informationen zu Previewfunktionen finden Sie unter [Einführung in Wissensspeicher in Azure Cognitive Search](https://docs.microsoft.com/azure/search/knowledge-store-concept-intro), [Einführung in inkrementelle Anreicherung und das Zwischenspeichern in Azure Cognitive Search](https://docs.microsoft.com/azure/search/cognitive-search-incremental-indexing-conceptual) und [Erstellen eines privaten Endpunkts für sichere Verbindungen mit Azure Cognitive Search (Vorschauversion)](https://docs.microsoft.com/azure/search/service-create-private-endpoint).

## <a name="assessment-and-planning"></a>Bewertung und Planung

Wenn Sie Ihren Suchdienst in die neue Region verschieben, müssen Sie [die Daten in den neuen Speicherdienst verschieben](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#configure-the-new-storage-account) und dann Ihre Indizes, Skillsets und Wissensspeicher neu erstellen. Sie sollten die aktuellen Einstellungen aufzeichnen und JSON-Dateien kopieren, um die Neuerstellung Ihres Dienstes zu vereinfachen und zu beschleunigen.

## <a name="moving-your-search-services-resources"></a>Verschieben der Ressourcen Ihres Suchdiensts

Zu Beginn exportieren Sie eine Resource Manager-Vorlage und ändern sie anschließend.

### <a name="export-a-template"></a>Exportieren einer Vorlage

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrer Ressourcengruppenseite.

> [!div class="mx-imgBorder"]
> ![Beispiel für eine Ressourcengruppenseite](./media/search-move-resource/export-template-sample.png)

3. Wählen Sie **Alle Ressourcen**.

3. Wählen Sie im Navigationsmenü auf der linken Seite **Vorlage exportieren** aus.

4. Wählen Sie **Herunterladen** auf der Seite **Vorlage exportieren** aus.

5. Suchen Sie die aus dem Portal heruntergeladene ZIP-Datei, und entpacken Sie sie in einen Ordner Ihrer Wahl.

Die ZIP-Datei enthält die JSON-Dateien, in denen die Vorlage und die Skripts zum Bereitstellen der Vorlage enthalten sind.

### <a name="modify-the-template"></a>Ändern der Vorlage

Sie ändern die Vorlage, indem Sie die Namen und Regionen von Speicherkonto und Suche ändern. Die Namen müssen den Namenskonventionsregeln der einzelnen Dienste und Regionen entsprechen. 

Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **USA, Mitte** = **centralus**.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.

3. Wählen Sie **Vorlagenbereitstellung** aus.

4. Klicken Sie auf **Erstellen**.

5. Wählen Sie **Eigene Vorlage im Editor erstellen**.

6. Wählen Sie **Datei laden** aus, und befolgen Sie dann die Anweisungen zum Laden der Datei **template.json**, die Sie im vorherigen Abschnitt heruntergeladen und entzippt haben.

7. Benennen Sie in der Datei **template.json** die Zielsuche und die Speicherkonten um, indem Sie den Standardwert der Namen von Suche und Speicherkonto festlegen. 

8. Bearbeiten Sie die **location**-Eigenschaft in der Datei **template.json** in der Zielregion für Ihre Such- und Speicherdienste. Dieses Beispiel legt den Zielbereich auf `centralus` fest.

```json
},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Search/searchServices",
            "apiVersion": "2015-08-19",
            "name": "[parameters('searchServices_target_region_search_name')]",
            "location": "centralus",
            "sku": {
                "name": "standard"
            },
            "properties": {
                "replicaCount": 1,
                "partitionCount": 1,
                "hostingMode": "Default"
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "name": "[parameters('storageAccounts_tagetstorageregion_name')]",
            "location": "centralus",
            "sku": {
                "name": "Standard_RAGRS",
                "tier": "Standard"
            },
```

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage

1. Speichern Sie die Datei **template.json**.

2. Geben Sie die Eigenschaftswerte ein oder wählen Sie sie aus:

- **Abonnement**: Wählen Sie ein Azure-Abonnement aus.

- **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und benennen Sie die Ressourcengruppe.

- **Standort**: Wählen Sie einen Azure-Standort aus.

3. Klicken Sie auf das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu** und dann auf die Schaltfläche **Kauf auswählen**.

## <a name="verifying-your-services-status-in-new-region"></a>Überprüfen des Status Ihrer Dienste in einer neuen Region

Wenn Sie zum Überprüfen der Verschiebung die neue Ressourcengruppe öffnen, werden Ihre Dienste in der neuen Region aufgeführt.

Informationen zum Verschieben von Daten aus der Quellregion in die Zielregion finden Sie in den Leitlinien dieses Artikels zum [Verschieben Ihrer Daten in das neue Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-account-move?tabs=azure-portal#move-data-to-the-new-storage-account).

## <a name="clean-up-resources-in-your-original-region"></a>Bereinigen von Ressourcen in ihrer ursprünglichen Region

Um die Änderungen zu übernehmen und das Verschieben Ihres Dienstkontos abzuschließen, löschen Sie das Quelldienstkonto.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Index](https://docs.microsoft.com/azure/search/search-get-started-portal)

[Erstellen eines Skillsets](https://docs.microsoft.com/azure/search/cognitive-search-quickstart-blob)

[Erstellen von Wissensspeichern](https://docs.microsoft.com/azure/search/knowledge-store-create-portal)

