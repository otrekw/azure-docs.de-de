---
title: Erstellen einer Personalisierungsressource
description: In diesem Artikel erfahren Sie, wie Sie im Azure-Portal für jede Feedbackschleife eine Personalisierungsressource erstellen.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0c75d917f1abe72af2f4aa56b0f67dbb7bcd24a1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91303555"
---
# <a name="create-a-personalizer-resource"></a>Erstellen einer Personalisierungsressource

Eine Personalisierungsressource ist dasselbe wie eine Personalisierungslernschleife. Für jede Motivdomäne oder jeden Inhaltsbereich, über die Sie verfügen, wird eine einzelne Ressource bzw. Lernschleife erstellt. Verwenden Sie nicht mehrere Inhaltsbereiche in derselben Schleife, da dies die Lernschleife verwirrt und schlechte Vorhersagen liefert.

Wenn Sie möchten, dass die Personalisierung die besten Inhalte für mehr als einen Inhaltsbereich einer Webseite auswählt, verwenden Sie für jeden Bereich eine andere Lernschleife.


## <a name="create-a-resource-in-the-azure-portal"></a>Erstellen einer Ressource im Azure-Portal

Erstellen Sie eine Personalisierungsressource für jede Feedbackschleife.

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)an. Über den vorherigen Link gelangen Sie zur Seite **Erstellen** für den Personalisierungsdienst.
1. Geben Sie Ihren Dienstnamen ein, und wählen Sie ein Abonnement, einen Standort, einen Tarif und eine Ressourcengruppe aus.

    > [!div class="mx-imgBorder"]
    > ![Verwenden Sie das Azure-Portal, um eine Personalisierungsressource bzw. Lernschleife zu erstellen.](./media/how-to-create-resource/how-to-create-personalizer-resource-learning-loop.png)

1. Wählen Sie **Erstellen** aus, um die Ressource zu erstellen.

1. Wählen Sie nach der Bereitstellung Ihrer Ressource die Schaltfläche **Zu Ressource wechseln** aus, um zu Ihrer Personalisierungsressource zu gelangen.

1. Wählen Sie die Seite **Schnellstart** für Ihre Ressource aus, und kopieren Sie die Werte für den Endpunkt und den Schlüssel. Sie benötigen den Ressourcenendpunkt und -schlüssel, um die Rang- und Relevanz-APIs verwenden zu können.

1. Wählen Sie die Seite **Konfiguration** der neuen Ressource aus, um [die Lernschleife zu konfigurieren](how-to-settings.md).

## <a name="create-a-resource-with-the-azure-cli"></a>Erstellen einer Ressource mit der Azure-Befehlszeilenschnittstelle

1. Melden Sie sich mit folgendem Befehl bei der Azure-Befehlszeilenschnittstelle an:

    ```azurecli-interactive
    az login
    ```

1. Erstellen Sie eine Ressourcengruppe, eine logische Gruppierung zur Verwaltung aller Azure-Ressourcen, die Sie mit der Personalisierungsressource verwenden möchten.


    ```azurecli-interactive
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Erstellen Sie eine neue Personalisierungsressource, _Lernschleife_, mit dem folgenden Befehl für eine vorhandene Ressourcengruppe.

    ```azurecli-interactive
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```

    Dadurch wird ein JSON-Objekt zurückgegeben, das Ihren **Ressourcenendpunkt** enthält.

1. Verwenden Sie zum Abrufen des **Ressourcenschlüssels** den folgenden Azure CLI-Befehl:

    ```azurecli-interactive
        az cognitiveservices account keys list \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group
    ```

    Sie benötigen den Ressourcenendpunkt und -schlüssel, um die Rang- und Relevanz-APIs verwenden zu können.

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren](how-to-settings.md) der Lernschleife der Personalisierung
