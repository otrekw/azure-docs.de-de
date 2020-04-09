---
title: Aufrufen von Dienstendpunkten per HTTP oder HTTPS
description: Senden von ausgehenden HTTP- oder HTTPS-Anforderungen an Dienstendpunkte aus Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
tags: connectors
ms.openlocfilehash: 8aefe851708c0b8d8780d03e4364e034e783bf4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297197"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Aufrufen von Dienstendpunkten per HTTP oder HTTPS aus Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem integrierten HTTP-Trigger oder einer HTTP-Aktion können Sie automatisierte Tasks und Workflows erstellen, die Anforderungen per HTTP oder HTTPS an Dienstendpunkte senden. Sie können beispielsweise den Dienstendpunkt für Ihre Website überwachen, indem Sie ihn nach einem bestimmten Zeitplan überprüfen. Wenn das angegebene Ereignis (beispielsweise ein Ausfall Ihrer Website) an diesem Endpunkt auftritt, löst das Ereignis den Workflow Ihrer Logik-App aus und führt die darin enthaltenen Aktionen aus. Falls Sie stattdessen eingehende HTTPS-Aufrufe empfangen und darauf reagieren möchten, verwenden Sie den integrierten [Anforderungstrigger oder die Antwortaktion](../connectors/connectors-native-reqres.md).

> [!NOTE]
> Basierend auf den Fähigkeiten des Zielendpunkts unterstützt der HTTP-Connector die TLS-Versionen (Transport Layer Security ) 1.0, 1.1 und 1.2. Logik-Apps handeln mit dem Endpunkt die Verwendung der höchstmöglich unterstützten Version aus. Wenn der Endpunkt also beispielsweise 1.2 unterstützt, verwendet der Connector zuerst 1.2. Andernfalls verwendet der Connector die nächsthöhere unterstützte Version.

Um einen Endpunkt nach einem wiederkehrenden Zeitplan zu überprüfen oder *abzurufen*, können Sie den [HTTP-Trigger Ihrem Workflow als ersten Schritt hinzufügen](#http-trigger). Jedes Mal, wenn der Trigger den Endpunkt überprüft, führt er einen Aufruf bzw. das Senden einer *Anforderung* an den Endpunkt durch. Die Antwort des Endpunkts bestimmt, ob der Workflow der Logik-App ausgeführt wird. Der Trigger übergibt alle Inhalte aus der Antwort des Endpunkts an die Aktionen in Ihrer Logik-App.

[Fügen Sie die entsprechende HTTP-Aktion hinzu](#http-action), um einen Endpunkt an einem anderen Punkt Ihres Workflows aufzurufen. Die Antwort des Endpunkts bestimmt, wie die restlichen Aktionen des Workflows ausgeführt werden.

> [!IMPORTANT]
> Wenn ein HTTP-Trigger oder eine HTTP-Aktion diese Header enthält, entfernt Logic Apps sie aus der generierten Anforderungsnachricht, ohne eine Warnung oder einen Fehler anzuzeigen:
>
> * `Accept-*`
> * `Allow`
> * `Content-*` mit den folgenden Ausnahmen: `Content-Disposition`, `Content-Encoding` und `Content-Type`.
> * `Cookie`
> * `Expires`
> * `Host`
> * `Last-Modified`
> * `Origin`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps verhindert nicht, dass Sie Logik-Apps speichern, in denen ein HTTP-Trigger oder eine HTTP-Aktion mit diesen Headern verwendet wird, sondern diese Header werden von Logic Apps ignoriert.

In diesem Artikel wird veranschaulicht, wie Sie einen HTTP-Trigger bzw. eine HTTP-Aktion dem Workflow Ihrer Logik-App hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die URL für den Zielendpunkt, den Sie aufrufen möchten

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

* Die Logik-App, von der aus Sie den Zielendpunkt aufrufen möchten. Um mit dem HTTP-Trigger zu beginnen, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um die HTTP-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem beliebigen Trigger. Dieses Beispiel verwendet den HTTP-Trigger als ersten Schritt.

<a name="http-trigger"></a>

## <a name="add-an-http-trigger"></a>Hinzufügen eines HTTP-Triggers

Dieser integrierte Trigger führt einen HTTP-Aufruf der angegebenen URL für einen Endpunkt aus und gibt eine Antwort zurück.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre leere Logik-App im Logik-App-Designer.

1. Wählen Sie im Suchfeld des Designers die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `http` als Filter ein. Wählen Sie in der Liste **Trigger** den **HTTP**-Trigger aus.

   ![Auswählen des HTTP-Triggers](./media/connectors-native-http/select-http-trigger.png)

   Dieses Beispiel benennt den Trigger in „HTTP trigger“ um, damit der Schritt über einen aussagekräftigeren Namen verfügt. Darüber hinaus fügt das Beispiel später eine HTTP-Aktion hinzu, und beide Namen müssen eindeutig sein.

1. Geben Sie die Werte für die [HTTP-Triggerparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger) ein, die Sie in den Aufruf des Zielendpunkts aufnehmen möchten. Geben Sie mithilfe einer Wiederholung an, wie oft der Trigger den Zielendpunkt überprüfen soll.

   ![Eingeben der HTTP-Triggerparameter](./media/connectors-native-http/http-trigger-parameters.png)

   Wenn Sie einen anderen Authentifizierungstyp als **Keiner**auswählen, unterscheiden sich die Authentifizierungseinstellungen je nach Ihrer Auswahl. Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP finden Sie unter den folgenden Themen:

   * [Hinzufügen der Authentifizierung zu ausgehenden Aufrufen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Authentifizieren des Zugriffs auf Ressourcen mit verwalteten Identitäten](../logic-apps/create-managed-service-identity.md)

1. Öffnen Sie zum Hinzufügen weiterer verfügbarer Parameter die Liste **Neuen Parameter hinzufügen**, und wählen Sie die gewünschten Parameter aus.

1. Fahren Sie mit dem Erstellen des Workflows Ihrer Logik-App fort, und fügen Sie weitere Aktionen hinzu, die bei Auslösung des Triggers ausgeführt werden.

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

<a name="http-action"></a>

## <a name="add-an-http-action"></a>Hinzufügen einer HTTP-Aktion

Diese integrierte Aktion führt einen HTTP-Aufruf der angegebenen URL für einen Endpunkt aus und gibt eine Antwort zurück.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre Logik-App im Logik-App-Designer.

   Dieses Beispiel verwendet den HTTP-Trigger als ersten Schritt.

1. Wählen Sie im Schritt zum Hinzufügen der HTTP-Aktion die Option **Neuer Schritt** aus.

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `http` als Filter ein. Wählen Sie in der Liste **Aktionen** die **HTTP**-Aktion aus.

   ![Auswählen der HTTP-Aktion](./media/connectors-native-http/select-http-action.png)

   Dieses Beispiel benennt die Aktion in „HTTP action“ um, damit der Schritt über einen aussagekräftigeren Namen verfügt.

1. Geben Sie die Werte für die [HTTP-Aktionsparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) ein, die Sie in den Aufruf des Zielendpunkts aufnehmen möchten.

   ![Eingeben der Parameter für die HTTP-Aktion](./media/connectors-native-http/http-action-parameters.png)

   Wenn Sie einen anderen Authentifizierungstyp als **Keiner**auswählen, unterscheiden sich die Authentifizierungseinstellungen je nach Ihrer Auswahl. Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP finden Sie unter den folgenden Themen:

   * [Hinzufügen der Authentifizierung zu ausgehenden Aufrufen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
   * [Authentifizieren des Zugriffs auf Ressourcen mit verwalteten Identitäten](../logic-apps/create-managed-service-identity.md)

1. Öffnen Sie zum Hinzufügen weiterer verfügbarer Parameter die Liste **Neuen Parameter hinzufügen**, und wählen Sie die gewünschten Parameter aus.

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

## <a name="content-with-multipartform-data-type"></a>Inhalt des Typs „multipart/form-data“

Für die Verarbeitung von Inhalt mit dem Typ `multipart/form-data` in HTTP-Anforderung können Sie ein JSON-Objekt hinzufügen, das die Attribute `$content-type` und `$multipart` im HTTP-Anforderungstext enthält, indem Sie dieses Format verwenden.

```json
"body": {
   "$content-type": "multipart/form-data",
   "$multipart": [
      {
         "body": "<output-from-trigger-or-previous-action>",
         "headers": {
            "Content-Disposition": "form-data; name=file; filename=<file-name>"
         }
      }
   ]
}
```

Angenommen, Sie verfügen über eine Logik-App, die eine HTTP POST-Anforderung für eine Excel-Datei an eine Website sendet, indem sie die API der Website nutzt, die den Typ `multipart/form-data` unterstützt. Diese Aktion könnte folgenderweise aussehen:

![Mehrteilige Formulardaten](./media/connectors-native-http/http-action-multipart.png)

Das folgende Beispiel entspricht der JSON-Definition der HTTP-Aktion in der zugrundeliegenden Workflowdefinition:

```json
{
   "HTTP_action": {
      "body": {
         "$content-type": "multipart/form-data",
         "$multipart": [
            {
               "body": "@trigger()",
               "headers": {
                  "Content-Disposition": "form-data; name=file; filename=myExcelFile.xlsx"
               }
            }
         ]
      },
      "method": "POST",
      "uri": "https://finance.contoso.com"
   },
   "runAfter": {},
   "type": "Http"
}
```

## <a name="connector-reference"></a>Connector-Referenz

Weitere Informationen zu Trigger- und Aktionsparametern finden Sie in diesen Abschnitten:

* [HTTP-Triggerparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [HTTP-Aktionsparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Ausgabedetails

Hier finden Sie weitere Informationen zu den Ausgaben aus einem HTTP-Trigger oder einer -Aktion, die diese Informationen zurückgeben:

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
