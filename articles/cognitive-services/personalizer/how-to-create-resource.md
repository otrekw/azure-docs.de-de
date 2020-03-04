---
title: Erstellen einer Personalisierungsressource
description: Die Dienstkonfiguration umfasst die Art, wie der Dienst Belohnungen behandelt, wie oft der Dienst Untersuchungen durchführt, wie oft das Modell neu trainiert wird und wie viele Daten gespeichert werden.
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: cb14415f3a5950ad1534d9eb8da94198a41f4f91
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624184"
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

1. Sobald Ihre Ressource bereitgestellt ist, wählen Sie die Schaltfläche **Zur Ressource wechseln** aus, um zu Ihrer Personalisierungsressource zu gelangen. Wechseln Sie zur Seite **Konfiguration** der neuen Ressource, um [die Lernschleife zu konfigurieren](how-to-settings.md).

## <a name="create-a-resource-with-the-azure-cli"></a>Erstellen einer Ressource mit der Azure-Befehlszeilenschnittstelle

1. Melden Sie sich mit folgendem Befehl bei der Azure-Befehlszeilenschnittstelle an:

    ```bash
    az login
    ```

1. Erstellen Sie eine Ressourcengruppe, eine logische Gruppierung zur Verwaltung aller Azure-Ressourcen, die Sie mit der Personalisierungsressource verwenden möchten.


    ```bash
    az group create \
        --name your-personalizer-resource-group \
        --location westus2
    ```

1. Erstellen Sie eine neue Personalisierungsressource, _Lernschleife_, mit dem folgenden Befehl für eine vorhandene Ressourcengruppe.

    ```bash
    az cognitiveservices account create \
        --name your-personalizer-learning-loop \
        --resource-group your-personalizer-resource-group \
        --kind Personalizer \
        --sku F0 \
        --location westus2 \
        --yes
    ```
## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren](how-to-settings.md) der Lernschleife der Personalisierung