---
title: 'Schnellstart: Senden von SMS-Nachrichten in Azure Logic Apps mithilfe von Azure Communication Services'
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie SMS-Nachrichten in Azure Logic Apps-Workflows mithilfe des Azure Communication Services-Connectors versenden können.
author: tophpalmer
manager: anvalent
services: azure-communication-services
ms.author: chpalm
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 11cce223a0802c20c5fc144eed681327f989a949
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103488379"
---
# <a name="quickstart-send-sms-messages-in-azure-logic-apps-with-azure-communication-services"></a>Schnellstart: Senden von SMS-Nachrichten in Azure Logic Apps mithilfe von Azure Communication Services

Mithilfe des [Azure Communication Services SMS](../../overview.md)-Connectors und [Azure Logic Apps](../../../logic-apps/logic-apps-overview.md) können Sie automatisierte Workflows oder *Logik-Apps* erstellen, die SMS-Nachrichten versenden können. In dieser Schnellstartanleitung erfahren Sie, wie automatisch Textnachrichten als Antwort auf ein Triggerereignis gesendet werden können, was der erste Schritt in einem Logik-App-Workflow ist. Ein Triggerereignis kann eine eingehende E-Mail-Nachricht, ein Wiederholungszeitplan, ein [Azure Event Grid](../../../event-grid/overview.md)-Ressourcenereignis oder jeder andere [Trigger sein, der von Azure Logic Apps](/connectors/connector-reference/connector-reference-logicapps-connectors) unterstützt wird.

:::image type="content" source="./media/logic-app/azure-communication-services-connector.png" alt-text="Der Screenshot zeigt das Azure-Portal, das für den Logic App Designer geöffnet ist, und zeigt ein Logik-App-Beispiel, das die Aktion „SMS senden“ für den Azure Communication Services-Connector verwendet.":::

Obwohl sich dieser Schnellstart darauf konzentriert, den Connector als Antwort auf einen Trigger zu verwenden, können Sie den Connector auch nutzen, um auf andere Aktionen zu reagieren, also auf die Schritte, die in einem Workflow auf den Trigger folgen. Falls Sie noch nicht mit Logic Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../../../logic-apps/logic-apps-overview.md), bevor Sie starten.

> [!NOTE]
> Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement oder [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Eine aktive Azure Communication Services-Ressource oder [eine Communication Services-Ressource](../create-communication-resource.md) erstellen.

- Eine aktive Logic Apps-Ressource (Logik-App), oder [eine leere Logik-App erstellen, jedoch mit dem zu verwendenden Trigger](../../../logic-apps/quickstart-create-first-logic-app-workflow.md). Derzeit bietet der SMS-Connector von Azure Communication Services nur Aktionen, sodass Ihre Logik-App mindestens einen Trigger erfordert.

  Diese Schnellstartanleitung verwendet den Trigger **When a new email arrives (Wenn eine neue E-Mail eingeht)** , der mit dem [Office 365 Outlook-Connector](/connectors/office365/) verfügbar ist.

- Eine SMS-aktivierte Telefonnummer oder [eine Telefonnummer erhalten](./get-phone-number.md).

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

## <a name="add-an-sms-action"></a>Hinzufügen einer SMS-Aktion

Um die Aktion **SMS senden** als neuen Schritt in Ihrem Workflow mithilfe des SMS-Connectors von Azure Communication Services hinzuzufügen, führen Sie die folgenden Schritte im [Azure-Portal](https://portal.azure.com) aus, wenn der Workflow Ihrer Logik-App im Logic App Designer geöffnet ist:

1. Wählen Sie im Designer unter dem Schritt, zu dem Sie die neue Aktion hinzufügen möchten, die Option **Neuer Schritt** aus. Alternativ können Sie die neue Aktion zwischen den Schritten hinzufügen, indem Sie den Mauszeiger über den Pfeil zwischen diesen Schritten bewegen, das Pluszeichen ( **+** ) und dann die Option **Aktion hinzufügen** auswählen.

1. Geben Sie in das Suchfeld **Vorgang auswählen** die Zeichenfolge `Azure Communication Services` ein. Wählen Sie in der Aktionsliste **SMS senden** aus.

   :::image type="content" source="./media/logic-app/select-send-sms-action.png" alt-text="Der Screenshot zeigt den Logic App Designer und den Azure Communication Services-Connector mit der ausgewählten Aktion „SMS senden“.":::

1. Erstellen Sie jetzt eine Verbindung mit Ihrer Communication Services-Ressource.

   1. Geben Sie einen Namen für die Verbindung an.

   1. Wählen Sie Ihre Azure Communication Services-Ressource aus.

   1. Klicken Sie auf **Erstellen**.

   :::image type="content" source="./media/logic-app/send-sms-configuration.png" alt-text="Der Screenshot zeigt die Konfiguration der Aktion „SMS senden“ mit Beispielinformationen.":::

1. Geben Sie in der Aktion **SMS senden** die folgenden Informationen an: 

   * Die Telefonnummern von Quelle und Ziel. Zu Testzwecken können Sie Ihre eigene Telefonnummer als Zielrufnummer verwenden.

   * Der Inhalt der Nachricht, die Sie senden möchten, z. B. „Hallo aus Logic Apps!“.

   Hier ist eine **SMS senden**-Aktion mit Beispielinformationen:

   :::image type="content" source="./media/logic-app/send-sms-action.png" alt-text="Der Screenshot zeigt die Aktion „SMS senden“ mit Beispielinformationen.":::

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

Führen Sie als nächstes Ihre Logik-App zum Testen aus.

## <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten. Oder Sie können auf die Auslösung Ihrer Logik-App warten. In beiden Fällen sollte die Logik-App eine SMS-Nachricht an Ihre angegebene Zieltelefonnummer senden. Weitere Informationen zum Ausführen Ihrer Logik-App finden Sie unter [Ausführen Ihrer Logik-App](../../../logic-apps/quickstart-create-first-logic-app-workflow.md#run-your-logic-app).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Um ein Communication Services-Abonnement zu entfernen, löschen Sie die Communication Services-Ressource oder -Ressourcengruppe. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen in dieser Gruppe gelöscht. Weitere Informationen finden Sie unter [Bereinigen von Communication Services-Ressourcen](../create-communication-resource.md#clean-up-resources).

Zur Bereinigung Ihres Logik-Apps-Workflows und von verwandten Ressourcen finden Sie weitere Informationen unter [Bereinigen von Logic Apps-Ressourcen](../../../logic-apps/quickstart-create-first-logic-app-workflow.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie erfahren, wie Sie Azure Logic Apps und Azure Communication Services zum Senden von SMS-Nachrichten verwenden. Wenn Sie mehr erfahren möchten, setzen Sie das Abonnieren von SMS-Ereignissen fort:

> [!div class="nextstepaction"]
> [Abonnieren von SMS-Ereignissen](./handle-sms-events.md)

Weitere Informationen zu SMS in Azure Communication Services finden Sie in den folgenden Artikeln:

- [SMS-Konzepte](../../concepts/telephony-sms/concepts.md)
- [Telefonnummerntypen](../../concepts/telephony-sms/plan-solution.md)
- [SMS SDK](../../concepts/telephony-sms/sdk-features.md)
