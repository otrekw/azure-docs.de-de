---
title: Erstellen einer Funktion in Azure, die nach einem Zeitplan ausgeführt wird
description: Erfahren Sie, wie Sie in Azure eine Funktion erstellen können, die nach einem von Ihnen definierten Zeitplan ausgeführt wird.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 04/16/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 9e542c493c02174364072f91d092f05ad9ec69cf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90973086"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird

Hier erfahren Sie, wie Sie mithilfe von Azure Functions eine [serverlose](https://azure.microsoft.com/solutions/serverless/) Funktion erstellen, die nach einem von Ihnen definierten Zeitplan ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-azure-function-app"></a>Erstellen einer Azure Function-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ihre neue Funktions-App ist einsatzbereit. Nun erstellen Sie eine Funktion in der neuen Funktions-App.

:::image type="content" source="./media/functions-create-scheduled-function/function-app-create-success.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Erstellen einer Funktion mit Auslösung per Timer

1. Wählen Sie in Ihrer Funktions-App **Funktionen** und dann **+ Hinzufügen** aus. 

   :::image type="content" source="./media/functions-create-scheduled-function/function-add-function.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Wählen Sie die Vorlage **Timertrigger** aus. 

    :::image type="content" source="./media/functions-create-scheduled-function/function-select-timer-trigger.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Konfigurieren Sie den neuen Trigger mit den Einstellungen, die in der Tabelle unter der folgenden Abbildung enthalten sind, und wählen Sie dann **Funktion erstellen** aus.

    :::image type="content" source="./media/functions-create-scheduled-function/function-configure-timer-trigger.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::
    
    | Einstellung | Vorgeschlagener Wert | Beschreibung |
    |---|---|---|
    | **Name** | Standard | Definiert den Namen der Funktion mit Auslösung per Timer |
    | **Zeitplan** | 0 \*/1 \* \* \* \* | Ein sechs Felder umfassender [CRON-Ausdruck](functions-bindings-timer.md#ncrontab-expressions), der die minütliche Ausführung der Funktion festlegt. |

## <a name="test-the-function"></a>Testen der Funktion

1. Wählen Sie in ihrer Funktion **Programmieren und testen** aus, und erweitern Sie die Protokolle.

    :::image type="content" source="./media/functions-create-scheduled-function/function-test-timer-trigger.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Überprüfen Sie die Ausführung, indem Sie sich die Informationen in den Protokollen ansehen.

    :::image type="content" source="./media/functions-create-scheduled-function/function-view-timer-logs.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Nun ändern Sie den Zeitplan der Funktion so, dass sie nicht mehr einmal pro Minute sondern einmal pro Stunde ausgeführt wird.

## <a name="update-the-timer-schedule"></a>Aktualisieren des Timerzeitplans

1. Wählen Sie in ihrer Funktion **Integration** aus. Hier können Sie Eingabe- und Ausgabebindungen für Ihre Funktion definieren und den Zeitplan festlegen. 

1. Wählen Sie **Timer (myTimer)** aus.

    :::image type="content" source="./media/functions-create-scheduled-function/function-update-timer-schedule.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

1. Aktualisieren Sie den Wert für **Zeitplan** in `0 0 */1 * * *`, und wählen Sie dann **Speichern** aus.  

    :::image type="content" source="./media/functions-create-scheduled-function/function-edit-timer-schedule.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

Eine im Stundentakt ausgeführte Funktion wird in der gewählten Sprache erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die basierend auf einem Zeitplan ausgeführt wird. Weitere Informationen zu Triggern mit Timern finden Sie unter [Planen der Ausführung von Code mit Azure Functions](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
