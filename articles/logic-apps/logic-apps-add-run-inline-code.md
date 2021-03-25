---
title: Hinzufügen und Ausführen von Codeausschnitten mithilfe von Inlinecode
description: Erfahren Sie, wie Sie Codeausschnitte erstellen und ausführen, indem Sie Inlinecodeaktionen für automatisierte Aufgaben und Workflows verwenden, die Sie mit Azure Logic Apps erstellen.
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: article
ms.date: 12/07/2020
ms.custom: devx-track-js
ms.openlocfilehash: 3f88fa38d62778bc3c4c1e29571d1d0ae4eeb5ff
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98179604"
---
# <a name="add-and-run-code-snippets-by-using-inline-code-in-azure-logic-apps"></a>Hinzufügen und Ausführen von Codeausschnitten mit Inlinecode in Azure Logic Apps

Wenn Sie einen Codeausschnitt in Ihrer Logik-App ausführen möchten, können Sie die integrierte Aktion Inlinecode im Workflow Ihrer Logik-App hinzufügen. Diese Aktion ist auf die Ausführung von Code im folgenden Szenario ausgelegt:

* Ausführung in JavaScript. Weitere Sprachen in Kürze verfügbar.

* Ausführung wird in höchstens fünf Sekunden abgeschlossen.

* Verarbeitet Daten mit einer Größe von bis zu 50 MB.

* Erfordert nicht die Verwendung von [**Variables**-Aktionen](../logic-apps/logic-apps-create-variables-store-values.md), die noch nicht unterstützt werden.

* Verwendung von Node.js-Version 8.11.1. Weitere Informationen finden Sie unter [Integrierte Standardobjekte](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

  > [!NOTE]
  > Die `require()`-Funktion wird von der Inlinecode-Aktion zur Ausführung von JavaScript nicht unterstützt.

Diese Aktion führt den Codeausschnitt aus und gibt die Ausgabe des Codeausschnitts als Token mit dem Namen `Result` zurück. Sie können dieses Token mit nachfolgenden Aktionen im Workflow ihrer Logik-App verwenden. Wenn Sie in anderen Szenarien eine Funktion für Ihren Code erstellen möchten, versuchen Sie, in der Logik-App [stattdessen eine Azure-Funktion über Azure Functions zu erstellen und aufzurufen](../logic-apps/logic-apps-azure-functions.md).

In diesem Artikel wird die Beispiel-Logik-App ausgelöst, wenn eine neue E-Mail in einem Geschäfts-, Schul- oder Unikonto empfangen wird. Der Codeausschnitt extrahiert und gibt alle E-Mail-Adressen zurück, die im E-Mail-Text enthalten sind.

![Screenshot: Beispiel für eine Logik-App](./media/logic-apps-add-run-inline-code/inline-code-example-overview.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die Logik-App, in der der Codeausschnitt hinzugefügt werden soll, einschließlich eines Triggers. Wenn Sie über keine Logik-App verfügen, lesen Sie den Artikel [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

   Das Beispiel in diesem Thema verwendet den Office 365 Outlook-Trigger mit dem Namen **Wenn eine neue E-Mail empfangen wird**.

* Ein mit Ihrer Logik-App verknüpftes [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

  * Achten Sie darauf, dass Sie ein Integrationskonto verwenden, das für Ihren Anwendungsfall oder Ihr Szenario geeignet ist.

    Beispielsweise gilt für Integrationskonten im [Free-Tarif](../logic-apps/logic-apps-pricing.md#integration-accounts), dass sie für explorative Szenarien und Workloads, nicht für Produktionsszenarien vorgesehen sind, in Nutzung und Durchsatz beschränkt sind und von einer Vereinbarung zum Servicelevel (Service-Level Agreement, SLA) nicht unterstützt werden. Andere Tarife verursachen Kosten, beinhalten jedoch SLA-Unterstützung, bieten mehr Durchsatz und haben höhere Grenzwerte. Weitere Informationen finden Sie hier: Integrationskonto-[Tarife](../logic-apps/logic-apps-pricing.md#integration-accounts), -[Preise](https://azure.microsoft.com/pricing/details/logic-apps/) und -[Grenzwerte](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

   * Wenn Sie kein Integrationskonto verwenden möchten, können Sie versuchen, [Azure Logic Apps Vorschau](logic-apps-overview-preview.md) zu verwenden, und eine Logik-App aus dem Ressourcentyp **Logik-App (Vorschau)** erstellen.

     In Azure Logic Apps Vorschau heißt **Inlinecode** jetzt **Inlinecodevorgänge**. Die folgenden Unterschiede gelten ebenfalls:

     * **JavaScript-Code ausführen** heißt jetzt **Inline-JavaScript ausführen**.

     * Unter macOS oder Linux sind Aktionen von Inlinecodevorgängen derzeit nicht verfügbar, wenn Sie die Azure Logic Apps-Erweiterung (Vorschau) in Visual Studio Code verwenden.

     * Die Grenzwerte für Aktionen in Inlinecodevorgängen wurden [aktualisiert](logic-apps-overview-preview.md#inline-code-limits).

     Sie können mit beiden Optionen beginnen:

     * Erstellen Sie die Logik-App aus dem **Ressourcentyp** Logik-App (Vorschau)[ mithilfe des Azure-Portals](create-stateful-stateless-workflows-azure-portal.md).

     * Erstellen Sie ein Projekt für die Logik-App [ mithilfe von Visual Studio Code und der Erweiterung Azure Logic Apps (Vorschau)](create-stateful-stateless-workflows-visual-studio-code.md).

## <a name="add-inline-code"></a>Inlinecode hinzufügen

1. Öffnen Sie, falls noch nicht geschehen, Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Designer für Logik-Apps.

1. Fügen Sie im Designer die Aktion „Inlinecode“ im Workflow der Logik-App hinzu.

   * Wenn Sie die Aktion am Ende des Workflows hinzufügen möchten, wählen Sie **Neuer Schritt** aus.

   * Wenn die Aktion zwischen Schritten hinzugefügt werden soll, bewegen Sie den Mauszeiger über den Pfeil, der die betreffenden Schritte verbindet. Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) aus, und klicken Sie auf **Aktion hinzufügen**.

   In diesem Beispiel wird die Aktion Inline Code unter dem Office 365 Outlook-Trigger hinzugefügt.

   ![Hinzufügen eines neuen Schritts unter dem Trigger](./media/logic-apps-add-run-inline-code/add-new-step.png)

1. Geben Sie unter **Aktion auswählen** den Text `inline code` in das Suchfeld ein. Wählen Sie in der Aktionenliste die Aktion namens **JavaScript-Code ausführen** aus.

   ![Auswählen der Aktion „JavaScript-Code ausführen“](./media/logic-apps-add-run-inline-code/select-inline-code-action.png)

   Die Aktion wird im Designer angezeigt und enthält standardmäßig Beispielcode einschließlich einer `return`-Anweisung.

   ![Inlinecode-Aktion mit Standard-Beispielcode](./media/logic-apps-add-run-inline-code/inline-code-action-default.png)

1. Löschen Sie im Feld **Code** den Beispielcode, und geben Sie Ihren Code ein. Schreiben Sie den Code, den Sie in eine Methode einschließen, jedoch ohne Methodensignatur.

   Wenn Sie ein erkanntes Schlüsselwort eingeben, wird die AutoVervollständigen-Liste angezeigt, sodass Sie unter den verfügbaren Schlüsselwörter auswählen können. Beispiel:

   ![AutoVervollständigen-Liste für Schlüsselwörter](./media/logic-apps-add-run-inline-code/auto-complete.png)

   In diesem Beispielcodeausschnitt wird zuerst eine Variable erstellt, die einen *regulären Ausdruck* speichert, der ein im Eingabetext zu suchendes Muster angibt. Anschließend wird im Code eine Variable erstellt, die die E-Mail-Textdaten aus dem Trigger speichert.

   ![Erstellen von Variablen](./media/logic-apps-add-run-inline-code/save-email-body-variable.png)

   Um das Verweisen auf Ergebnisse des Triggers und früherer Aktionen zu erleichtern, wird die Liste der dynamischen Inhalte angezeigt, wenn sich der Cursor im Feld **Code** befindet. Für dieses Beispiel werden in der Liste verfügbare Ergebnisse aus dem Trigger aufgeführt, u.a. das **Body**-Token, das nun ausgewählt werden kann.

   Nachdem Sie das **Body**-Token ausgewählt haben, löst die Inlinecode-Aktion das Token in ein `workflowContext`-Objekt auf, das auf den `Body`-Eigenschaftswert der E-Mail verweist:

   ![Auswählen des Ergebnisses](./media/logic-apps-add-run-inline-code/inline-code-example-select-outputs.png)

   Im Feld **Code** kann der Codeausschnitt das schreibgeschützte `workflowContext`-Objekt als Eingabe verwenden. Dieses Objekt enthält Eigenschaften auf, die dem Code Zugriff auf die Ergebnisse des Triggers und früherer Aktionen im Workflow gewähren. Weitere Informationen finden Sie unter [Verweisen auf die Ergebnisse von Triggern und Aktionen in Ihrem Code](#workflowcontext) weiter unten in diesem Thema.

   > [!NOTE]
   > Wenn der Codeausschnitt auf Aktionsnamen verweist, in denen der Punktoperator (.) verwendet wird, müssen Sie die betreffenden Aktionsnamen dem [**Actions**-Parameter](#add-parameters) hinzufügen. In diesen Verweise müssen die Aktionsnamen zudem in eckige Klammern ([]) und Anführungszeichen eingeschlossen sein. Beispiel:
   >
   > `// Correct`</br> 
   > `workflowContext.actions["my.action.name"].body`</br>
   >
   > `// Incorrect`</br>
   > `workflowContext.actions.my.action.name.body`

   Die Aktion „Inlinecode“ erfordert keine `return`-Anweisung, die Ergebnisse einer `return`-Anweisung sind jedoch über das **Result**-Token als Verweis in späteren Aktionen verfügbar. Der Codeabschnitt gibt z.B. das Ergebnis durch einen Aufruf der `match()`-Funktion zurück, die Übereinstimmungen im E-Mail-Text anhand des regulären Ausdrucks findet. Die **Compose**-Aktion verweist mithilfe des **Result**-Tokens auf die Ergebnisse der Inlinecode-Aktion und erstellt ein einzelnes Ergebnis.

   ![Fertiggestellte Logik-App](./media/logic-apps-add-run-inline-code/inline-code-complete-example.png)

1. Wenn Sie fertig sind, speichern Sie Ihre Logik-App.

<a name="workflowcontext"></a>

### <a name="reference-trigger-and-action-results-in-your-code"></a>Verweisen auf Ergebnisse von Triggern und Aktionen im Code

Das `workflowContext`-Objekt weist diese Struktur auf und schließt die untergeordneten Eigenschaften `actions`, `trigger` und `workflow` ein:

```json
{
   "workflowContext": {
      "actions": {
         "<action-name-1>": @actions('<action-name-1>'),
         "<action-name-2>": @actions('<action-name-2>')
      },
      "trigger": {
         @trigger()
      },
      "workflow": {
         @workflow()
      }
   }
}
```

Diese Tabelle enthält weitere Informationen zu den untergeordneten Eigenschaften:

| Eigenschaft | type | BESCHREIBUNG |
|----------|------|-------|
| `actions` | Objektsammlung | Ergebnisobjekte von Aktionen, die vor der Ausführung des Codeausschnitts ausgeführt werden. Jedes Objekt verfügt über ein *Schlüssel-Wert*-Paar, bei dem der Schlüssel den Namen einer Aktion angibt und der Wert dem Aufruf der [actions()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#actions) mit `@actions('<action-name>')` entspricht. Als Name der Aktion wird der Aktionsname verwendet, der auch in der zugrunde liegenden Workflowdefinition verwendet wird, wobei Leerzeichen (" ") im Aktionsnamen durch Unterstriche (_) ersetzt werden. Dieses Objekt bietet Zugriff auf Eigenschaftswerte der Aktion aus der aktuellen Ausführung der Workflowinstanz. |
| `trigger` | Object | Ergebnisobjekt aus dem Trigger, das einem Aufruf der [trigger()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#trigger) entspricht. Dieses Objekt bietet Zugriff auf Eigenschaftswerte des Triggers aus der aktuellen Ausführung der Workflowinstanz. |
| `workflow` | Object | Das Workflow-Objekt, das einem Aufruf der [workflow()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#workflow) entspricht. Dieses Objekt bietet Zugriff auf Workflow-Eigenschaftswerte (z.B. Workflowname, Ausführungs-ID usw.) aus der aktuellen Ausführung der Workflowinstanz. |
|||

In Beispiel in diesem Artikel verfügt das `workflowContext`-Objekt über die folgenden Eigenschaften, auf die der Code zugreifen kann:

```json
{
   "workflowContext": {
      "trigger": {
         "name": "When_a_new_email_arrives",
         "inputs": {
            "host": {
               "connection": {
                  "name": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/connections/office365"
               }
            },
            "method": "get",
            "path": "/Mail/OnNewEmail",
            "queries": {
               "includeAttachments": "False"
            }
         },
         "outputs": {
            "headers": {
               "Pragma": "no-cache",
               "Content-Type": "application/json; charset=utf-8",
               "Expires": "-1",
               "Content-Length": "962095"
            },
            "body": {
               "Id": "AAMkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgBGAAAAAABmZwxUQtCGTqSPpjjMQeD",
               "DateTimeReceived": "2019-03-28T19:42:16+00:00",
               "HasAttachment": false,
               "Subject": "Hello World",
               "BodyPreview": "Hello World",
               "Importance": 1,
               "ConversationId": "AAQkADY0NGZhNjdhLTRmZTQtNGFhOC1iYjFlLTk0MjZlZjczMWRhNgAQ",
               "IsRead": false,
               "IsHtml": true,
               "Body": "Hello World",
               "From": "<sender>@<domain>.com",
               "To": "<recipient-2>@<domain>.com;<recipient-2>@<domain>.com",
               "Cc": null,
               "Bcc": null,
               "Attachments": []
            }
         },
         "startTime": "2019-05-03T14:30:45.971564Z",
         "endTime": "2019-05-03T14:30:50.1746874Z",
         "scheduledTime": "2019-05-03T14:30:45.8778117Z",
         "trackingId": "1cd5ffbd-f989-4df5-a96a-6e9ce31d03c5",
         "clientTrackingId": "08586447130394969981639729333CU06",
         "originHistoryName": "08586447130394969981639729333CU06",
         "code": "OK",
         "status": "Succeeded"
      },
      "workflow": {
         "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>",
         "name": "<logic-app-workflow-name>",
         "type": "Microsoft.Logic/workflows",
         "location": "<Azure-region>",
         "run": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<logic-app-workflow-name>/runs/08586453954668694173655267965CU00",
            "name": "08586453954668694173655267965CU00",
            "type": "Microsoft.Logic/workflows/runs"
         }
      }
   }
}
```

<a name="add-parameters"></a>

## <a name="add-parameters"></a>Hinzufügen von Parametern

Gelegentlich müssen Sie explizit fordern, dass die Inline Code-Aktion Ergebnisse des Triggers oder bestimmter Aktionen einschließt, auf die der Code als Abhängigkeiten verweist, indem auf den **Trigger**-Parameter oder **Actions**-Parameter verwiesen wird. Diese Option ist hilfreich in Szenarien, bei denen die Ergebnisse, auf die verwiesen wird, zur Laufzeit nicht gefunden werden.

> [!TIP]
> Wenn der Code wiederverwendet werden soll, fügen Sie über das Feld **Code** Verweise auf Eigenschaften hinzu, sodass der Code die aufgelösten Codeverweise einschließt, anstatt den Trigger oder die Aktionen als explizite Abhängigkeiten hinzuzufügen.

Angenommen, in Ihrem Code wird auf das **SelectedOption**-Ergebnis aus der Aktion **Genehmigungs-E-Mail senden** für den Office 365 Outlook-Connector verwiesen. Zur Erstellungszeit analysiert das Logic Apps-Modul den Code, um zu bestimmen, ob auf Ergebnisse von Triggern oder Aktionen verwiesen wurde, und diese Ergebnisse werden automatisch eingeschlossen. Wird zur Laufzeit ein Fehler ausgegeben, der besagt, dass das Ergebnis des referenzierten Triggers bzw. der referenzierten Aktion im angegebenen `workflowContext`-Objekt nicht verfügbar ist, können Sie den Trigger bzw. die Aktion als explizite Abhängigkeit hinzufügen. In diesem Beispiel fügen Sie den **Actions**-Parameter hinzu und geben an, dass die Inline Code-Aktion explizit das Ergebnis der Aktion **Genehmigungs-E-Mail senden** einschließt.

Öffnen Sie zum Hinzufügen dieser Parameter die Liste **Neuen Parameter hinzufügen**, und wählen Sie die gewünschten Parameter aus:

   ![Hinzufügen von Parametern](./media/logic-apps-add-run-inline-code/inline-code-action-add-parameters.png)

   | Parameter | BESCHREIBUNG |
   |-----------|-------------|
   | **Aktionen** | Schließen Sie Ergebnisse aus früheren Aktionen ein. Siehe [Einschließen der Ergebnisse von Aktionen](#action-results). |
   | **Trigger** | Schließen Sie Ergebnisse des Triggers ein. Siehe [Einschließen der Ergebnisse des Triggers](#trigger-results). |
   |||

<a name="trigger-results"></a>

### <a name="include-trigger-results"></a>Einschließen der Ergebnisse des Triggers

Wenn Sie **Trigger** auswählen, werden Sie gefragt, ob Triggerergebnisse eingeschlossen werden sollen.

* Wählen Sie in der Liste **Trigger** die Option **Ja** aus.

<a name="action-results"></a>

### <a name="include-action-results"></a>Einschließen der Ergebnisse von Aktionen

Wenn Sie **Aktionen** auswählen, werden Sie aufgefordert, die hinzuzufügenden Aktionen anzugeben. Bevor Sie jedoch mit dem Hinzufügen von Aktionen beginnen, benötigen Sie jedoch die Version des Aktionsnamens, der in der Definition des zugrunde liegenden Workflows der Logik-App angezeigt wird.

* Diese Funktion unterstützt keine Variablen, Schleifen und Iterationsindizes.

* In den Namen in der Workflowdefinition der Logik-App wird anstelle eines Leerzeichens ein Unterstrich (_) verwendet.

* Für Aktionsnamen mit dem Punktoperator (.) müssen Sie diese Operatoren einschließen. Beispiel:

  `My.Action.Name`

1. Wählen Sie auf der Symbolleiste des Designers die Option **Codeansicht** aus, und suchen Sie im `actions`-Attribut nach dem Aktionsnamen.

   `Send_approval_email_` ist beispielsweise der JSON-Name für die Aktion **Genehmigungs-E-Mail senden**.

   ![Suchen des Aktionsnamens im JSON-Format](./media/logic-apps-add-run-inline-code/find-action-name-json.png)

1. Wählen Sie auf der Symbolleiste „Codeansicht“ die Option **Designer** aus, um zur Designeransicht zurückzukehren.

1. Um die erste Aktion hinzuzufügen, geben Sie im Feld **Actions Item - 1** (Aktionselement – 1) den JSON-Namen der Aktion ein.

   ![Eingeben der ersten Aktion](./media/logic-apps-add-run-inline-code/add-action-parameter.png)

1. Wählen Sie **Neues Element hinzufügen** aus, um eine weitere Aktion hinzuzufügen.

## <a name="reference"></a>Verweis

Weitere Informationen zu Struktur und Syntax der Aktion **Execute JavaScript Code** in der zugrunde liegenden Workflowdefinition der Logik-App in der Definitionssprache für Workflows finden Sie im [Referenzabschnitt](../logic-apps/logic-apps-workflow-actions-triggers.md#run-javascript-code) dieser Aktion.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Connectors für Azure Logic Apps](../connectors/apis-list.md)
