---
title: Aufrufen von Dienstendpunkten per HTTP oder HTTPS
description: Senden von ausgehenden HTTP- oder HTTPS-Anforderungen an Dienstendpunkte aus Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: conceptual
ms.date: 06/09/2020
tags: connectors
ms.openlocfilehash: 8c7a0ddb80ba28548fc1821cc2063e500af0fa66
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286630"
---
# <a name="call-service-endpoints-over-http-or-https-from-azure-logic-apps"></a>Aufrufen von Dienstendpunkten per HTTP oder HTTPS aus Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem integrierten HTTP-Trigger oder einer HTTP-Aktion können Sie automatisierte Tasks und Workflows erstellen, die Anforderungen per HTTP oder HTTPS an Dienstendpunkte senden. Sie können beispielsweise den Dienstendpunkt für Ihre Website überwachen, indem Sie ihn nach einem bestimmten Zeitplan überprüfen. Wenn das angegebene Ereignis (beispielsweise ein Ausfall Ihrer Website) an diesem Endpunkt auftritt, löst das Ereignis den Workflow Ihrer Logik-App aus und führt die darin enthaltenen Aktionen aus. Falls Sie stattdessen eingehende HTTPS-Aufrufe empfangen und darauf reagieren möchten, verwenden Sie den integrierten [Anforderungstrigger oder die Antwortaktion](../connectors/connectors-native-reqres.md).

* Um einen Endpunkt nach einem wiederkehrenden Zeitplan zu überprüfen oder *abzurufen*, können Sie den [HTTP-Trigger Ihrem Workflow als ersten Schritt hinzufügen](#http-trigger). Jedes Mal, wenn der Trigger den Endpunkt überprüft, führt er einen Aufruf bzw. das Senden einer *Anforderung* an den Endpunkt durch. Die Antwort des Endpunkts bestimmt, ob der Workflow der Logik-App ausgeführt wird. Der Trigger übergibt alle Inhalte aus der Antwort des Endpunkts an die Aktionen in Ihrer Logik-App.

* [Fügen Sie die entsprechende HTTP-Aktion hinzu](#http-action), um einen Endpunkt an einem anderen Punkt Ihres Workflows aufzurufen. Die Antwort des Endpunkts bestimmt, wie die restlichen Aktionen des Workflows ausgeführt werden.

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

<a name="tls-support"></a>

## <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

Je nach Funktionalität des Zielendpunkts unterstützen ausgehende Aufrufe die Versionen 1.0, 1.1 und 1.2 von Transport Layer Security (TLS), früher Secure Sockets Layer (SSL). Logik-Apps handeln mit dem Endpunkt die Verwendung der höchstmöglich unterstützten Version aus.

Wenn der Endpunkt also beispielsweise Version 1.2 unterstützt, verwendet der HTTP-Connector zuerst die Version 1.2. Andernfalls verwendet der Connector die nächsthöhere unterstützte Version.

<a name="self-signed"></a>

## <a name="self-signed-certificates"></a>Selbstsignierte Zertifikate

* Für Logik-Apps in der globalen Azure-Umgebung mit mehren Mandanten lässt der HTTP-Connector keine selbstsignierten TLS/SSL-Zertifikate zu. Wenn Ihre Logik-App per HTTP einen Server aufruft und ein selbstsigniertes TLS/SSL-Zertifikat vorlegt, schlägt der HTTP-Aufruf mit einem `TrustFailure`-Fehler fehl.

* Für Logik-Apps in einer [Integrationsdienstumgebung (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erlaubt der HTTP-Connector hingegen selbstsignierte Zertifikate bei TLS/SSL-Handshakes. Sie müssen jedoch zunächst die [Unterstützung für selbstsignierte Zertifikate für eine vorhandene oder eine neue ISE mithilfe der Logic Apps-REST-API aktivieren](../logic-apps/create-integration-service-environment-rest-api.md#request-body) und das öffentliche Zertifikat am `TrustedRoot`-Speicherort installieren.

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
"HTTP_action": {
   "inputs": {
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

<a name="asynchronous-pattern"></a>

## <a name="asynchronous-request-response-behavior"></a>Asynchrones Anforderungs-Antwort-Verhalten

Alle HTTP-basierten Aktionen in Azure Logic Apps befolgen erst einmal das [Standardmuster für asynchrone Vorgänge](/azure/architecture/patterns/async-request-reply). Laut diesem Muster gibt der Empfänger sofort eine [202 ACCEPTED](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.3)-Antwort zurück, wenn eine HTTP-Aktion einen Endpunkt, einen Dienst, das System oder die API aufruft oder eine Anforderung an ebendiese sendet. Dieser Code bestätigt, dass der Empfänger die Anforderung akzeptiert, aber die Verarbeitung noch nicht abgeschlossen hat. Die Antwort kann einen `location`-Header enthalten, in dem die URL und eine Aktualisierungs-ID angegeben sind, mit denen der Aufrufer den Status der asynchronen Anforderung abrufen oder überprüfen kann, bis der Empfänger die Verarbeitung beendet und eine Antwort mit dem Code [200 OK](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1) oder einem anderen Statuscode zurückgibt, der nicht 202 ist. Der Aufrufer muss jedoch nicht darauf warten, dass die Verarbeitung der Anforderung abgeschlossen wird, und kann mit der Ausführung der nächsten Aktion fortfahren. Weitere Informationen finden Sie unter [Gegenüberstellung von synchronem und asynchronem Messaging](/azure/architecture/microservices/design/interservice-communication#synchronous-versus-asynchronous-messaging).

* Im Logik-App-Designer verfügt die HTTP-Aktion, nicht der Trigger, über eine Einstellung für das **asynchrone Muster**, die automatisch aktiviert ist. Diese Einstellung gibt an, dass der Aufrufer nicht auf den Abschluss der Verarbeitung wartet, sondern mit der nächsten Aktion fortfahren kann, dass er aber weiterhin den Status überprüft, bis die Verarbeitung beendet wird. Wenn diese Einstellung deaktiviert ist, gibt sie an, dass der Aufrufer auf den Abschluss der Verarbeitung wartet, bevor er mit der nächsten Aktion fortfährt.

  Führen Sie die folgenden Schritte aus, um diese Einstellung zu suchen:

  1. Wählen Sie auf der Titelleiste der HTTP-Aktion die Schaltfläche mit den Auslassungspunkten ( **...** ) aus, woraufhin die Einstellungen der Aktion geöffnet werden.

  1. Suchen Sie die Einstellung **Asynchrones Muster**.

     ![Einstellung „Asynchrones Muster“](./media/connectors-native-http/asynchronous-pattern-setting.png)

* In der JSON-Definition (JavaScript Object Notation), die der HTTP-Aktion zugrunde liegt, folgt implizit dem Muster für asynchrone Vorgänge.

<a name="disable-asynchronous-operations"></a>

## <a name="disable-asynchronous-operations"></a>Deaktivieren asynchroner Vorgänge

In bestimmten Szenarios sollten Sie das asynchrone Verhalten von HTTP-Aktionen deaktivieren, z. B.:

* [Vermeiden von HTTP-Timeouts bei Aufgaben mit langer Ausführungszeit](#avoid-http-timeouts)
* [Deaktivieren der Überprüfung von Location-Headern](#disable-location-header-check)

<a name="turn-off-asynchronous-pattern-setting"></a>

### <a name="turn-off-asynchronous-pattern-setting"></a>Deaktivieren der Einstellung **Asynchrones Muster**

1. Klicken Sie im Logik-App-Designer in der Titelleiste der HTTP-Aktion auf die Schaltfläche mit den Auslassungspunkten ( **...** ). Daraufhin werden die Einstellungen der Aktion geöffnet.

1. Suchen Sie die Einstellung **Asynchrones Muster**, deaktivieren Sie diese, indem Sie sie auf **Aus** festlegen (falls sie aktiviert ist), und klicken Sie auf **Fertig**.

   ![Suchen der Einstellung „Asynchrones Muster“](./media/connectors-native-http/disable-asynchronous-pattern-setting.png)

<a name="add-disable-async-pattern-option"></a>

### <a name="disable-asynchronous-pattern-in-actions-json-definition"></a>Deaktivieren des asynchronen Musters in der JSON-Definition einer Aktion

[Fügen Sie in der JSON-Definitionsdatei, die der HTTP-Aktion zugrunde liegt, die Option für den Vorgang `"DisableAsyncPattern"` zur Aktionsdefinition hinzu](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options), damit die Aktion stattdessen das synchrone Vorgangsmuster befolgt. Weitere Informationen finden Sie unter [Ausführen von Aktionen in einem synchronen Vorgangsmuster](../logic-apps/logic-apps-workflow-actions-triggers.md#disable-asynchronous-pattern).

<a name="avoid-http-timeouts"></a>

## <a name="avoid-http-timeouts-for-long-running-tasks"></a>Vermeiden von HTTP-Timeouts bei Aufgaben mit langer Ausführungszeit

HTTP-Anforderungen unterliegen einem [Timeoutlimit](../logic-apps/logic-apps-limits-and-config.md#http-limits). Wenn eine Ihrer HTTP-Aktionen mit langer Ausführungszeit aufgrund dieses Timeoutlimits beendet wird, haben Sie die folgenden Möglichkeiten:

* [Deaktivieren des Musters für asynchrone Vorgänge, das die HTTP-Aktion befolgt](#disable-asynchronous-operations), damit die Aktion nicht fortlaufend den Anforderungsstatus abfragt oder überprüft. Die Aktion wartet dann stattdessen darauf, dass der Empfänger den Status und die Ergebnisse in der Antwort sendet, nachdem die Verarbeitung der Anforderung abgeschlossen wurde.

* Ersetzen der HTTP-Aktion durch eine [HTTP-Webhook-Aktion](../connectors/connectors-native-webhook.md), die darauf wartet, dass der Empfänger den Status und die Ergebnisse in der Antwort sendet, nachdem die Verarbeitung der Anforderung abgeschlossen wurde

<a name="disable-location-header-check"></a>

## <a name="disable-checking-location-headers"></a>Deaktivieren der Überprüfung von Location-Headern

Einige Endpunkte, Dienste, Systeme oder APIs geben eine Antwort mit dem Statuscode „202 ACCEPTED“ zurück. Dieser Antworttyp enthält keinen `location`-Header. Damit eine HTTP-Aktion nicht fortlaufend den Anforderungsstatus überprüft, obwohl kein `location`-Header vorhanden ist, können Sie wie folgt vorgehen:

* [Deaktivieren des Musters für asynchrone Vorgänge, das die HTTP-Aktion befolgt](#disable-asynchronous-operations), damit die Aktion nicht fortlaufend den Anforderungsstatus abfragt oder überprüft. Die Aktion wartet dann stattdessen darauf, dass der Empfänger den Status und die Ergebnisse in der Antwort sendet, nachdem die Verarbeitung der Anforderung abgeschlossen wurde.

* Ersetzen der HTTP-Aktion durch eine [HTTP-Webhook-Aktion](../connectors/connectors-native-webhook.md), die darauf wartet, dass der Empfänger den Status und die Ergebnisse in der Antwort sendet, nachdem die Verarbeitung der Anforderung abgeschlossen wurde

## <a name="known-issues"></a>Bekannte Probleme

<a name="omitted-headers"></a>

### <a name="omitted-http-headers"></a>Ausgelassene HTTP-Header

Wenn ein HTTP-Trigger oder eine HTTP-Aktion diese Header enthält, entfernt Logic Apps sie aus der generierten Anforderungsnachricht, ohne eine Warnung oder einen Fehler anzuzeigen:

* `Accept-*`
* `Allow`
* `Content-*` mit den folgenden Ausnahmen: `Content-Disposition`, `Content-Encoding` und `Content-Type`.
* `Cookie`
* `Expires`
* `Host`
* `Last-Modified`
* `Origin`
* `Set-Cookie`
* `Transfer-Encoding`

Logic Apps verhindert nicht, dass Sie Logik-Apps speichern, in denen ein HTTP-Trigger oder eine HTTP-Aktion mit diesen Headern verwendet wird, sondern diese Header werden von Logic Apps ignoriert.

## <a name="connector-reference"></a>Connector-Referenz

Weitere Informationen zu Trigger- und Aktionsparametern finden Sie in diesen Abschnitten:

* [HTTP-Triggerparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-trigger)
* [HTTP-Aktionsparameter](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)

### <a name="output-details"></a>Ausgabedetails

Hier finden Sie weitere Informationen zu den Ausgaben aus einem HTTP-Trigger oder einer -Aktion, die diese Informationen zurückgeben:

| Eigenschaft | type | BESCHREIBUNG |
|----------|------|-------------|
| `headers` | JSON-Objekt | Die Header aus der Anforderung |
| `body` | JSON-Objekt | Das Objekt mit dem Inhalt des Texts aus der Anforderung |
| `status code` | Integer | Der Statuscode aus der Anforderung |
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

