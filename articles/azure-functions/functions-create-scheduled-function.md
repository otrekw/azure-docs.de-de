---
title: Erstellen einer Funktion in Azure, die gemäß einem Zeitplan ausgeführt wird
description: Erfahren Sie, wie Sie mithilfe des Azure-Portals eine Funktion erstellen können, die gemäß einem von Ihnen definierten Zeitplan ausgeführt wird.
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.topic: how-to
ms.date: 04/16/2020
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: 14d918cc41f49b954f5cabf48572db5df829fd10
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98035188"
---
# <a name="create-a-function-in-the-azure-portal-that-runs-on-a-schedule"></a>Erstellen einer Funktion im Azure-Portal, die gemäß einem Zeitplan ausgeführt wird

Erfahren Sie, wie Sie mithilfe des Azure-Portals eine Funktion erstellen können, die gemäß einem von Ihnen definierten Zeitplan [serverlos](https://azure.microsoft.com/solutions/serverless/) in Azure ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Ihre neue Funktions-App ist einsatzbereit. Nun erstellen Sie eine Funktion in der neuen Funktions-App.

:::image type="content" source="./media/functions-create-scheduled-function/function-app-create-success.png" alt-text="Die Funktions-App wurde erfolgreich erstellt." border="true":::

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Erstellen einer Funktion mit Auslösung per Timer

1. Wählen Sie in Ihrer Funktions-App **Funktionen** und dann **+ Hinzufügen** aus. 

   :::image type="content" source="./media/functions-create-scheduled-function/function-add-function.png" alt-text="Hinzufügen einer Funktion im Azure-Portal." border="true":::

1. Wählen Sie die Vorlage **Timertrigger** aus. 

    :::image type="content" source="./media/functions-create-scheduled-function/function-select-timer-trigger.png" alt-text="Auswählen des Timertriggers im Azure-Portal." border="true":::

1. Konfigurieren Sie den neuen Trigger mit den Einstellungen, die in der Tabelle unter der folgenden Abbildung enthalten sind, und wählen Sie dann **Funktion erstellen** aus.

    :::image type="content" source="./media/functions-create-scheduled-function/function-configure-timer-trigger.png" alt-text="Screenshot: Seite „Neue Funktion“ mit ausgewählter Vorlage „Zeitgebertrigger“" border="true":::
    
    | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    |---|---|---|
    | **Name** | Standard | Definiert den Namen der Funktion mit Auslösung per Timer |
    | **Zeitplan** | 0 \*/1 \* \* \* \* | Ein sechs Felder umfassender [CRON-Ausdruck](functions-bindings-timer.md#ncrontab-expressions), der die minütliche Ausführung der Funktion festlegt. |

## <a name="test-the-function"></a>Testen der Funktion

1. Wählen Sie in ihrer Funktion **Programmieren und testen** aus, und erweitern Sie die Protokolle.

    :::image type="content" source="./media/functions-create-scheduled-function/function-test-timer-trigger.png" alt-text="Testen des Timertriggers im Azure-Portal." border="true":::

1. Überprüfen Sie die Ausführung, indem Sie sich die Informationen in den Protokollen ansehen.

    :::image type="content" source="./media/functions-create-scheduled-function/function-view-timer-logs.png" alt-text="Anzeigen des Timertriggers im Azure-Portal." border="true":::

Nun ändern Sie den Zeitplan der Funktion so, dass sie nicht mehr einmal pro Minute sondern einmal pro Stunde ausgeführt wird.

## <a name="update-the-timer-schedule"></a>Aktualisieren des Timerzeitplans

1. Wählen Sie in ihrer Funktion **Integration** aus. Hier können Sie Eingabe- und Ausgabebindungen für Ihre Funktion definieren und den Zeitplan festlegen. 

1. Wählen Sie **Timer (myTimer)** aus.

    :::image type="content" source="./media/functions-create-scheduled-function/function-update-timer-schedule.png" alt-text="Aktualisieren des Timerzeitplans im Azure-Portal." border="true":::

1. Aktualisieren Sie den Wert für **Zeitplan** in `0 0 */1 * * *`, und wählen Sie dann **Speichern** aus.  

    :::image type="content" source="./media/functions-create-scheduled-function/function-edit-timer-schedule.png" alt-text="Aktualisieren des Zeitplans des Funktionstimers im Azure-Portal." border="true":::

Eine im Stundentakt ausgeführte Funktion wird in der gewählten Sprache erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktion erstellt, die basierend auf einem Zeitplan ausgeführt wird. Weitere Informationen zu Triggern mit Timern finden Sie unter [Planen der Ausführung von Code mit Azure Functions](functions-bindings-timer.md).

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
