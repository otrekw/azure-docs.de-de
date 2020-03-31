---
title: Warten und Reagieren auf Ereignisse
description: Automatisieren Sie mithilfe von Azure Logic Apps Workflows, die basierend auf Ereignissen an einem Dienstendpunkt ausgelöst, angehalten und fortgesetzt werden.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/06/2020
tags: connectors
ms.openlocfilehash: 1578ca030bc8bab971a44e1afcce1d1ab9e1d5e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78674068"
---
# <a name="create-and-run-automated-event-based-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Erstellen und Ausführen automatisierter ereignisbasierter Workflows mithilfe von HTTP-Webhooks in Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem integrierten HTTP-Webhookconnector können Sie Workflows automatisieren, die auf der Grundlage bestimmter Ereignisse warten und ausgeführt werden, die an einem HTTP- oder HTTPS-Endpunkt auftreten, indem Sie Logik-Apps erstellen. So können Sie beispielsweise eine Logik-App erstellen, die einen Dienstendpunkt überwacht, indem sie auf ein bestimmtes Ereignis wartet, bevor sie den Workflow auslöst und die angegebenen Aktionen ausführt, anstatt diesen Endpunkt regelmäßig zu überprüfen oder *abzufragen*.

Hier sind einige Beispiele für ereignisbasierte Workflows:

* Warten, dass ein Element von einem [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) eintrifft, bevor die Ausführung einer Logik-App ausgelöst wird.
* Warten auf eine Genehmigung, bevor ein Workflow fortgesetzt wird.

## <a name="how-do-webhooks-work"></a>Wie funktionieren Webhooks?

Ein HTTP-Webhooktrigger ist ereignisbasiert und erfordert nicht, dass regelmäßig auf neue Elemente geprüft wird oder diese abgefragt werden. Wenn Sie eine Logik-App speichern, die mit einem Webhooktrigger gestartet wird, oder Ihre Logik-App von „deaktiviert“ in „aktiviert“ ändern, *abonniert* der Webhooktrigger einen angegebenen Dienst oder Endpunkt, indem er eine *Rückruf-URL* bei diesem Dienst oder Endpunkt registriert. Der Trigger wartet dann darauf, dass der Dienst oder Endpunkt die URL aufruft, wodurch die Ausführung der Logik-App beginnt. Die Logik-App wird ähnlich wie der [Anforderungstrigger](connectors-native-reqres.md) sofort ausgelöst, wenn das angegebene Ereignis eintritt. Der Trigger *kündigt das Abonnement* des Diensts oder Endpunkts, wenn Sie ihn entfernen und Ihre Logik-App speichern oder wenn Sie Ihre Logik-App von „aktiviert“ in „deaktiviert“ ändern.

Eine Webhookaktion ist ebenfalls ereignisbasiert und *abonniert* den angegebenen Dienst oder Endpunkt durch die Registrierung einer *Rückruf-URL* bei diesem Dienst oder Endpunkt. Die Webhookaktion hält den Workflow der Logik-App an und wartet, bis der Dienst oder der Endpunkt die URL aufruft, bevor die Logik-App die Ausführung fortsetzt. Die Aktionslogik-App *kündigt das Abonnement* für den Dienst oder Endpunkt in den folgenden Fällen:

* Wenn die Webhookaktion erfolgreich abgeschlossen wurde
* Wenn die Logik-App während des Wartens auf eine Antwort abgebrochen wird
* Bevor Ein Timeout für die Logik-App auftritt

Die Aktion zum [**Senden einer Genehmigungs-E-Mail**](connectors-create-api-office365-outlook.md) von Office 365 Outlook ist ein Beispiel für eine Webhookaktion, die diesem Muster folgt. Sie können dieses Muster mithilfe der Webhookaktion auf jeden Dienst erweitern.

> [!NOTE]
> Azure Logic Apps erzwingt Transport Layer Security (TLS) 1.2, wenn der Rückruf auf den HTTP-Webhook-Trigger oder eine Aktion empfangen wird. Wenn SSL-Handshakefehler auftreten, stellen Sie sicher, dass Sie TLS 1.2 verwenden. Für eingehende Aufrufe werden folgende Verschlüsselungssammlungen unterstützt:
>
> * TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

Weitere Informationen finden Sie in den folgenden Themen:

* [Triggerparameter des HTTP-Webhooks](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Webhooks und Abonnements](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Erstellen von benutzerdefinierten APIs, die einen Webhook unterstützen](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die URL für einen bereits bereitgestellten Endpunkt oder eine API, die das Webhookmuster für das Abonnieren und Kündigen von Abonnements für [Webhooktrigger in Logik-Apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) oder [Webhookaktionen in Logik-Apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions) je nach Bedarf unterstützt.

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

* Die Logik-App, in der auf bestimmte Ereignisse am Zielendpunkt gewartet werden soll. Um mit dem HTTP-Webhooktrigger zu beginnen, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um die HTTP-Webhookaktion zu verwenden, starten Sie Ihre Logik-App mit einem beliebigen Trigger. Dieses Beispiel verwendet den HTTP-Trigger als ersten Schritt.

## <a name="add-an-http-webhook-trigger"></a>Hinzufügen eines HTTP-Webhooktriggers

Dieser integrierte Trigger ruft den Abonnementendpunkt im Zieldienst auf und registriert eine Rückruf-URL beim Zieldienst. Ihre Logik-App wartet dann darauf, dass der Zieldienst eine `HTTP POST`-Anforderung an die Rückruf-URL sendet. Wenn dieses Ereignis eintritt, wird der Trigger ausgelöst und übergibt alle Daten in der Anforderung an den Workflow.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre leere Logik-App im Logik-App-Designer.

1. Geben Sie `http webhook` als Filter in das Suchfeld des Designers ein. Wählen Sie aus der Liste **Trigger** den Trigger **HTTP-Webhook** aus.

   ![Auswählen des HTTP-Webhooktriggers](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   Dieses Beispiel benennt den Trigger in `HTTP Webhook trigger` um, damit der Schritt über einen aussagekräftigeren Namen verfügt. Darüber hinaus fügt das Beispiel später eine HTTP-Webhookaktion hinzu, und beide Namen müssen eindeutig sein.

1. Geben Sie die Werte für die [Parameter des HTTP-Webhooktriggers](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) an, die Sie für die Aufrufe zum Abonnieren und Kündigen eines Abonnements verwenden möchten.

   In diesem Beispiel umfasst der Auslöser die Methoden, URIs und Nachrichtentexte, die beim Durchführen der Vorgänge „Abonnieren“ und „Abonnement kündigen“ verwendet werden sollen.

   ![Eingeben von Triggerparametern des HTTP-Webhooks](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Abonnement: Methode** | Ja | Die Methode, die beim Abonnieren des Zielendpunkts verwendet werden soll. |
   | **Abonnieren: URI** | Ja | Die URL, die zum Abonnieren des Zielendpunkts verwendet werden soll. |
   | **Abonnieren: Text** | Nein | Jeder Nachrichtentext, der in die Abonnementanforderung eingeschlossen werden soll. Dieses Beispiel beinhaltet die Rückruf-URL, die den Abonnenten eindeutig identifiziert, bei dem es sich um Ihre Logik-App handelt, indem der `@listCallbackUrl()`-Ausdruck verwendet wird, um die Rückruf-URL Ihrer Logik-App abzurufen. |
   | **Abonnement kündigen: Method** | Nein | Die Methode, die beim Kündigen des Abonnements des Zielendpunkts verwendet werden soll. |
   | **Abonnement kündigen: URI** | Nein | Die URL, die zum Kündigen des Abonnements des Zielendpunkts verwendet werden soll. |
   | **Abonnement kündigen: Text** | Nein | Ein optionaler Nachrichtentext, der in die Anforderung der Abonnementkündigung eingeschlossen werden soll. <p><p>**Hinweis**: Diese Eigenschaft unterstützt nicht die Verwendung der `listCallbackUrl()`-Funktion. Der Trigger schließt jedoch automatisch die Header `x-ms-client-tracking-id` und `x-ms-workflow-operation-name` ein und sendet sie, die der Zieldienst verwenden kann, um den Abonnenten eindeutig zu identifizieren. |
   ||||

1. Öffnen Sie die Liste **Neuen Parameter hinzufügen**, um weitere Triggereigenschaften hinzuzufügen.

   ![Hinzufügen weiterer Triggereigenschaften](./media/connectors-native-webhook/http-webhook-trigger-add-properties.png)

   Wenn Sie beispielsweise Authentifizierung verwenden müssen, können Sie die Eigenschaften **Abonnieren: Authentifizierung** und **Abonnement kündigen: Authentifizierung** hinzufügen. Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP Webhook finden Sie unter [Hinzufügen von Authentifizierung zu ausgehenden Aufrufen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Fahren Sie mit dem Erstellen des Workflows Ihrer Logik-App fort, und fügen Sie weitere Aktionen hinzu, die bei Auslösung des Triggers ausgeführt werden.

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Durch das Speichern Ihrer Logik-App wird der Abonnementendpunkt im Zieldienst aufgerufen und die Rückruf-URL registriert. Ihre Logik-App wartet dann darauf, dass der Zieldienst eine `HTTP POST`-Anforderung an die Rückruf-URL sendet. Wenn dieses Ereignis eintritt, wird der Trigger ausgelöst und übergibt alle Daten in der Anforderung an den Workflow. Wenn diese Aktion erfolgreich abgeschlossen wird, kündigt der Trigger das Abonnement des Endpunkts, und Ihre Logik-App setzt den verbleibenden Workflows fort.

## <a name="add-an-http-webhook-action"></a>Hinzufügen einer HTTP-Webhookaktion

Diese integrierte Aktion ruft den Abonnementendpunkt im Zieldienst auf und registriert eine Rückruf-URL beim Zieldienst. Ihre Logik-App pausiert und wartet dann darauf, dass der Zieldienst eine `HTTP POST`-Anforderung an die Rückruf-URL sendet. Wenn dieses Ereignis eintritt, übergibt die Aktion alle Daten in der Anforderung an den Workflow. Wenn der Vorgang erfolgreich abgeschlossen wird, kündigt die Aktion das Abonnement des Endpunkts, und Ihre Logik-App setzt die Ausführung des verbleibenden Workflows fort.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre Logik-App im Logik-App-Designer.

   Dieses Beispiel verwendet den HTTP-Webhooktrigger als ersten Schritt.

1. Wählen Sie im Schritt zum Hinzufügen der HTTP-Webhookaktion die Option **Neuer Schritt** aus.

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie `http webhook` als Filter in das Suchfeld des Designers ein. Wählen Sie in der Liste **Aktionen** die Aktion **HTTP-Webhook** aus.

   ![Auswählen einer HTTP-Webhookaktion](./media/connectors-native-webhook/select-http-webhook-action.png)

   Dieses Beispiel benennt die Aktion in „HTTP-Webhookaktion“ um, damit der Schritt über einen aussagekräftigeren Namen verfügt.

1. Geben Sie die Werte für die Parameter der HTTP-Webhookaktion an (die ähnlich wie die [Parameter des Webhooktriggers](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) sind), die Sie für die Aufrufe zum Abonnieren und Kündigen eines Abonnements verwenden möchten.

   In diesem Beispiel umfasst die Aktion die Methoden, URIs und Nachrichtentexte, die beim Durchführen der Vorgänge „Abonnieren“ und „Abonnement kündigen“ verwendet werden sollen.

   ![Eingeben der Parameter für die HTTP-Webhookaktion](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Abonnement: Methode** | Ja | Die Methode, die beim Abonnieren des Zielendpunkts verwendet werden soll. |
   | **Abonnieren: URI** | Ja | Die URL, die zum Abonnieren des Zielendpunkts verwendet werden soll. |
   | **Abonnieren: Text** | Nein | Jeder Nachrichtentext, der in die Abonnementanforderung eingeschlossen werden soll. Dieses Beispiel beinhaltet die Rückruf-URL, die den Abonnenten eindeutig identifiziert, bei dem es sich um Ihre Logik-App handelt, indem der `@listCallbackUrl()`-Ausdruck verwendet wird, um die Rückruf-URL Ihrer Logik-App abzurufen. |
   | **Abonnement kündigen: Method** | Nein | Die Methode, die beim Kündigen des Abonnements des Zielendpunkts verwendet werden soll. |
   | **Abonnement kündigen: URI** | Nein | Die URL, die zum Kündigen des Abonnements des Zielendpunkts verwendet werden soll. |
   | **Abonnement kündigen: Text** | Nein | Ein optionaler Nachrichtentext, der in die Anforderung der Abonnementkündigung eingeschlossen werden soll. <p><p>**Hinweis**: Diese Eigenschaft unterstützt nicht die Verwendung der `listCallbackUrl()`-Funktion. Die Aktion schließt jedoch automatisch die Header `x-ms-client-tracking-id` und `x-ms-workflow-operation-name` ein und sendet sie, die der Zieldienst verwenden kann, um den Abonnenten eindeutig zu identifizieren. |
   ||||

1. Öffnen Sie die Liste **Neuen Parameter hinzufügen**, um weitere Aktionseigenschaften hinzuzufügen.

   ![Hinzufügen weiterer Aktionseigenschaften](./media/connectors-native-webhook/http-webhook-action-add-properties.png)

   Wenn Sie beispielsweise Authentifizierung verwenden müssen, können Sie die Eigenschaften **Abonnieren: Authentifizierung** und **Abonnement kündigen: Authentifizierung** hinzufügen. Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP Webhook finden Sie unter [Hinzufügen von Authentifizierung zu ausgehenden Aufrufen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Wenn diese Aktion nun ausgeführt wird, ruft Ihre Logik-App den Abonnementendpunkt im Zieldienst auf und registriert die Rückruf-URL. Die Logik-App hält dann den Workflow an und wartet darauf, dass der Zieldienst eine `HTTP POST`-Anforderung an die Rückruf-URL sendet. Wenn dieses Ereignis eintritt, übergibt die Aktion alle Daten in der Anforderung an den Workflow. Wenn der Vorgang erfolgreich abgeschlossen wird, kündigt die Aktion das Abonnement des Endpunkts, und Ihre Logik-App setzt die Ausführung des verbleibenden Workflows fort.

## <a name="connector-reference"></a>Connector-Referenz

Weitere Informationen zu Triggern und Aktionsparametern, die einander ähnlich sind, finden Sie unter [HTTP-Webhookparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger).

### <a name="output-details"></a>Ausgabedetails

Hier finden Sie weitere Informationen zu den Ausgaben aus einem HTTP-Webhooktrigger oder einer -aktion, die diese Informationen zurückgeben:

| Eigenschaftenname | type | BESCHREIBUNG |
|---------------|------|-------------|
| headers | Objekt (object) | Die Header aus der Anforderung |
| body | Objekt (object) | JSON-Objekt | Das Objekt mit dem Inhalt des Texts aus der Anforderung |
| status code | INT | Der Statuscode aus der Anforderung |
|||

| Statuscode | BESCHREIBUNG |
|-------------|-------------|
| 200 | OK |
| 202 | Zulässig |
| 400 | Ungültige Anforderung |
| 401 | Nicht autorisiert |
| 403 | Verboten |
| 404 | Nicht gefunden |
| 500 | Interner Serverfehler. Unbekannter Fehler. |
|||

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
