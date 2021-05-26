---
title: Herstellen einer Verbindung mit IBM MQ-Server
description: Stellen Sie über einen Workflow mit Azure Logic Apps eine Verbindung mit einem lokalen MQ-Server oder in Azure her.
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, estfan, logicappspm
ms.topic: article
ms.date: 05/25/2021
tags: connectors
ms.openlocfilehash: e852f0ce1584a0858f9523c1ea055d4f2cd616d2
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110376253"
---
# <a name="connect-to-an-ibm-mq-server-from-a-workflow-in-azure-logic-apps"></a>Herstellen einer Verbindung mit einem IBM MQ-Server über einen Workflow in Azure Logic Apps

Mit dem MQ-Connector können Sie Ihre Logik-App-Workflows mit einem IBM MQ-Server verbinden, der sich entweder lokal ist oder sich in Azure befindet. Anschließend können Ihre Workflows Nachrichten empfangen und senden, die auf Ihrem MQ-Server gespeichert sind. Dieser Artikel enthält eine Anleitung für die ersten Schritte mit dem MQ-Connector, indem gezeigt wird, wie Sie eine Verbindung mit Ihrem MQ-Server herstellen und Ihrem Workflow eine MQ-Aktion hinzufügen. Sie können beispielsweise zuerst eine einzelne Nachricht in einer Warteschlange suchen und anschließend andere Aktionen ausprobieren.

Dieser Connector umfasst einen Microsoft-Client zum Kommunizieren mit einem MQ-Remoteserver über ein TCP/IP-Netzwerk. Sie können eine Verbindung mit den folgenden IBM WebSphere MQ-Versionen herstellen:

* MQ 7.5
* MQ 8.0
* MQ 9.0, 9.1 und 9.2

<a name="available-operations"></a>

## <a name="available-operations"></a>Verfügbare Vorgänge

* Mehrinstanzenfähige Azure Logic Apps: Wenn Sie eine **Logik-App-Ressource (nutzungsbasiert)** erstellen, können Sie die Verbindung zu einem MQ-Server nur über den *verwalteten* MQ-Connector herstellen. Dieser Connector stellt nur Aktionen und keine Trigger bereit.

* Azure Logic Apps mit nur einem Mandanten: Wenn Sie einen Logik-App-Workflow mit nur einem Mandanten erstellen, können Sie für eine Verbindung mit einem MQ-Server entweder den verwalteten MQ-Connector, der *nur* Aktionen enthält, oder die *integrierten* MQ-Operationen verwenden, die Trigger *und* Aktionen enthalten.

Weitere Informationen zum Unterschied zwischen einem verwalteten Connector und integrierten Vorgängen finden Sie unter den [Schlüsselbedingungen in Logic Apps](../logic-apps/logic-apps-overview.md#logic-app-concepts).

#### <a name="managed"></a>[Verwaltet](#tab/managed)

In der folgenden Liste werden nur einige der verwalteten Vorgänge beschrieben, die für MQ verfügbar sind:

* Durchsuchen Sie eine einzelne Nachricht oder ein Array von Nachrichten, ohne sie vom MQ-Server zu löschen. Für mehrere Nachrichten können Sie die maximale Anzahl von Nachrichten angeben, die pro Batch zurückgegeben werden sollen. Andernfalls werden alle Nachrichten zurückgegeben.
* Löscht eine einzelne Nachricht oder eine Reihe von Nachrichten vom MQ-Server.
* Empfangen einer einzelnen Nachricht oder eine Reihe von Nachrichten und anschließendes Löschen vom MQ-Server.
* Senden einer einzelnen Nachricht an den MQ-Server

Informationen zu allen verwalteten Connectorvorgängen und anderen technischen Informationen wie Eigenschaften, Grenzwerten usw. finden Sie auf der [Referenzseite des MQ-Connectors](/connectors/mq/).

#### <a name="built-in"></a>[Integriert](#tab/built-in)

In der folgenden Liste werden nur einige der integrierten Vorgänge beschrieben, die für MQ verfügbar sind:

* Wenn eine Nachricht in einer Warteschlange verfügbar ist, führen Sie eine Aktion aus.
* Wenn eine oder mehrere Nachrichten von einer Warteschlange eingehen (AutoVervollständigen), führen Sie eine Aktion aus.
* Wenn eine oder mehrere Nachrichten von einer Warteschlange eingehen (Peek-Lock), führen Sie eine Aktion aus.
* Empfangen einer einzelnen Nachricht oder eine Reihe von Nachrichten von einer Warteschlange. Für mehrere Nachrichten können Sie die maximale Anzahl von Nachrichten angeben, die pro Batch zurückgegeben werden sollen, und die maximale Stapelgröße in KB angeben.
* Senden einer einzelnen Nachricht oder eine Reihe von Nachrichten an den MQ-Server.

Diese integrierten MQ-Vorgänge verfügen auch über die folgenden Funktionen sowie die Vorteile aller anderen Funktionen für Logik-Apps im [Einzelmandanten-basierten Logic Apps-Dienst](../logic-apps/single-tenant-overview-compare.md):

* Transport Layer Security (TLS) Verschlüsselung bei der Übertragung
* Nachrichtencodierung für Sendungs- und Empfangsvorgänge
* Unterstützung für die Integration virtueller Azure-Netzwerke, wenn Ihre Logik-App den Azure Functions Premium-Plan verwendet

---

## <a name="limitations"></a>Einschränkungen

Das Feld **Format** der Nachricht wird nicht vom MQ-Connector verwendet. Außerdem wird keine Konvertierung von Zeichensätzen durchgeführt. Der Connector platziert die Daten im Nachrichtenfeld lediglich in einer JSON-Nachricht und übermittelt diese Nachricht.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto und ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Wenn Sie einen lokalen MQ-Server verwenden, [installieren Sie das lokale Datengateway](../logic-apps/logic-apps-gateway-install.md) auf einem Server in Ihrem Netzwerk. Für eine ordnungsgemäße Funktionsweise des MQ-Connectors muss der Server, auf dem sich das lokale Datengateway befindet, auch über .NET Framework 4.6 verfügen.

  Nachdem Sie das Gateway installiert haben, müssen Sie auch eine Datengateway-Ressource in Azure erstellen. Der MQ-Connector verwendet diese Ressource für den Zugriff auf Ihren MQ-Server. Weitere Informationen finden Sie unter [Einrichten der Datengatewayverbindung](../logic-apps/logic-apps-gateway-connection.md). 

  > [!NOTE]
  > In den folgenden Szenarien benötigen Sie das Gateway nicht:
  > 
  > * Sie verwenden die integrierten Vorgänge, nicht den verwalteten Connector.
  > * Ihr MQ-Server ist öffentlich oder in Azure verfügbar.

* Der Logik-App-Workflow, in dem Sie auf Ihren MQ-Server zugreifen möchten. Ihre Logik-App-Ressource muss denselben Standort wie Ihre Gatewayressource in Azure haben.

  Der MQ-Connector hat keine Trigger, sodass entweder Ihr Workflow bereits mit einem Trigger beginnen muss, oder Sie müssen ihrem Workflow zunächst einen Trigger hinzufügen. Sie können beispielsweise den [Wiederholungstrigger](../connectors/connectors-native-recurrence.md) verwenden.

  Wenn Sie noch nicht mit Azure Logic Apps gearbeitet haben, können Sie in dieser [Schnellstartanleitung](../logic-apps/quickstart-create-first-logic-app-workflow.md) einen Beispielworkflow für eine Logik-App erstellen, der im mehrinstanzenfähgen Logic Apps ausgeführt wird.

<a name="create-connection"></a>

## <a name="create-an-mq-connection"></a>Erstellen der MQ-Verbindung 

Wenn Sie zum ersten Mal eine MQ-Aktion hinzufügen, werden Sie aufgefordert, eine Verbindung mit Ihrem MQ-Server herzustellen.

> [!NOTE]
> Der MQ-Connector unterstützt derzeit nur die Serverauthentifizierung, jedoch nicht die Clientauthentifizierung. Weitere Informationen finden Sie unter [Probleme bei der Verbindung und Authentifizierung](#connection-problems).

#### <a name="managed"></a>[Verwaltet](#tab/managed)

1. Wenn Sie eine Verbindung mit einem lokalen MQ-Server herstellen, wählen Sie **Über lokales Datengateway verbinden** aus.

1. Geben Sie die Verbindungsinformationen für den MQ-Server an.

   | Eigenschaft | Lokal oder Azure | Beschreibung |
   |----------|----------------------|-------------|
   | **Gateways** | Nur lokal | Wählen Sie **Verbinden über lokales Datengateway**. |
   | **Verbindungsname** | Beide | Der Name, der für Ihre Verbindung verwendet werden soll |
   | **Server** | Beide | Einer der folgenden Werte: <p><p>- Name des MQ-Serverhosts <br>- hinter der IP-Adresse steht ein Doppelpunkt und eine Portnummer |
   | **Name des Warteschlangen-Managers** | Beide | Der Warteschlangen-Manager, den Sie nutzen möchten |
   | **Kanalname** | Beide | Der Kanal zum Herstellen einer Verbindung mit dem Warteschlangen-Manager |
   | **Der standardmäßige Warteschlangenname** | Beide | Der Standardname für die Warteschlange |
   | **Verbinden als** | Beide | Der Benutzername zum Herstellen einer Verbindung mit dem MQ-Server |
   | **Benutzername** | Beide | Ihre Anmeldeinformationen für den Benutzernamen |
   | **Kennwort** | Beide | Ihr Kennwort in den Anmeldeinformationen |
   | **SSL aktivieren?** | Nur lokal | Verwenden Sie TLS (Transport Layer Security) oder SSL (Secure Sockets Layer) |
   | **Gateway – Abonnement** | Nur lokal | Das Azure-Abonnement, das der Gatewayressource zugeordnet ist |
   | **Gateway - Gatewayverbindung** | Nur lokal | Die zu verwendende Gatewayressource |
   ||||

   Beispiel:

   ![Screenshot: Details der verwalteten MQ-Verbindung.](media/connectors-create-api-mq/managed-connection-properties.png)

1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

#### <a name="built-in"></a>[Integriert](#tab/built-in)

1. Geben Sie die Verbindungsinformationen für den MQ-Server an.

   | Eigenschaft | Lokal oder Azure | BESCHREIBUNG |
   |----------|----------------------|-------------|
   | **Verbindungsname** | Beide | Der Name, der für Ihre Verbindung verwendet werden soll |
   | **Servername** | Beide | Der MQ-Servername oder die IP-Adresse |
   | **Portnummer** | Beide | Die TCP-Portnummer zum Herstellen einer Verbindung mit dem Warteschlangen-Manager auf dem Host |
   | **Kanal** | Beide | Der Kanal zum Herstellen einer Verbindung mit dem Warteschlangen-Manager |
   | **Name des Warteschlangen-Managers** | Beide | Der Warteschlangen-Manager, den Sie nutzen möchten |
   | **Der standardmäßige Warteschlangenname** | Beide | Der Standardname für die Warteschlange |
   | **Verbinden als** | Beide | Der Benutzername zum Herstellen einer Verbindung mit dem MQ-Server |
   | **Benutzername** | Beide | Ihre Anmeldeinformationen für den Benutzernamen |
   | **Kennwort** | Beide | Ihr Kennwort in den Anmeldeinformationen |
   | **Verwenden Sie TLS** | Beide | Verwenden Sie Transport Layer Security (TLS) |
   ||||

   Beispiel:

   ![Screenshot: Details der integrierten MQ-Verbindung.](media/connectors-create-api-mq/built-in-connection-properties.png)

1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

---

<a name="add-action"></a>

## <a name="add-an-mq-action"></a>Fügen Sie eine MQ-Aktion hinzu

In Azure Logic Apps folgt eine Aktion dem Trigger oder einer anderen Aktion und führt einen Vorgang in Ihrem Workflow aus. In den folgenden Schritten wird die allgemeine Vorgehensweise zum Hinzufügen einer Aktion beschrieben, z. B. **Durchsuchen einer Nachricht**.

1. Öffnen Sie im Logic Apps-Designer Ihren Workflow, falls er noch nicht geöffnet ist.

1. Fügen Sie unter dem Trigger oder einer anderen Aktion einen neuen Schritt hinzu.

   Um einen Schritt zwischen vorhandenen Schritten hinzuzufügen, bewegen Sie den Mauszeiger über den Pfeil. Wählen Sie das angezeigte Pluszeichen (+) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie `mq` in das Vorgangssuchfeld ein. Wählen Sie in der Aktionenliste die Aktion namens **Nachricht durchsuchen** aus.

1. Wenn Sie aufgefordert werden, eine Verbindung mit Ihrem MQ-Server herzustellen, [geben Sie die angeforderten Verbindungsinformationen](#create-connection) an.

1. Geben Sie in der Aktion die Eigenschaftswerte an, die die Aktion benötigt.

   Für weitere Eigenschaften öffnen Sie die Liste **Neuen Parameter hinzufügen** und wählen Sie die Eigenschaften aus, die hinzugefügt werden sollen.

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

1. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihren Workflow zu testen.

   Nach Abschluss der Ausführung zeigt der Designer den Ausführungsverlauf des Workflows zusammen mit dem Status für den Schritt an.

1. Erweitern oder wählen Sie den Schritt aus, um die Eingaben und Ausgaben für jeden Schritt zu überprüfen, der bereits ausgeführt (nicht übersprungen) wurde.

   * Um weitere Eingabedetails zu überprüfen, wählen Sie **Rohdateneingaben anzeigen** aus.
   * Um weitere Ausgabedetails zu überprüfen, wählen Sie **Rohdatenausgaben anzeigen** aus. Wenn Sie **IncludeInfo** auf **true** festlegen, werden weitere Ausgabeinformationen eingeschlossen.

## <a name="troubleshoot-problems"></a>Behandeln von Problemen

### <a name="failures-with-browse-or-receive-actions"></a>Fehler beim Durchsuchen oder Empfangen von Aktionen

Wenn Sie eine Aktion zum Durchsuchen oder Empfangen für eine leere Warteschlange ausführen, schlägt die Aktion mit den folgenden Headerausgabe fehl:

![MQ-Fehler „Keine Nachricht“](media/connectors-create-api-mq/mq-no-message-error.png)

<a name="connection-problems"></a>

### <a name="connection-and-authentication-problems"></a>Probleme bei der Verbindung und Authentifizierung

Wenn der Workflow versucht, eine Verbindung mit dem lokalen MQ-Server herzustellen, wird möglicherweise die folgende Fehlermeldung angezeigt:

`"MQ: Could not Connect the Queue Manager '<queue-manager-name>': The Server was expecting an SSL connection."`

* Wenn Sie den MQ-Connector direkt in Azure verwenden, muss auf dem MQ-Server ein Zertifikat verwendet werden, das von einer vertrauenswürdigen [Zertifizierungsstelle](https://www.ssl.com/faqs/what-is-a-certificate-authority/) ausgestellt wurde.

* Für den MQ-Server muss eine Verschlüsselungsspezifikation zur Verwendung mit TLS-Connectors definiert werden. Aus Sicherheitsgründen und um die besten Sicherheitssammlungen zu verwenden, sendet das Windows-Betriebssystem jedoch eine Reihe unterstützter Verschlüsselungsspezifikationen.

  Das Betriebssystem, unter dem der MQ-Server ausgeführt wird, wählt die zu verwendenden Suiten aus. Damit die Konfiguration übereinstimmen kann, müssen Sie das MQ-Server-Setup so ändern, dass die Verschlüsselungsspezifikation der in der TLS-Aushandlung ausgewählten Option entspricht.

  Wenn Sie den Verbindungsaufbau versuchen, protokolliert der MQ-Server eine Ereignismeldung, dass der Verbindungsversuch nicht erfolgreich war, da am MQ-Server die falsche Verschlüsselungsspezifikation verwendet wurde. Die Ereignismeldung enthält die Verschlüsselungsspezifikation, die der MQ-Server aus der Liste ausgewählt hat. Aktualisieren Sie in der Kanalkonfiguration die Chiffre-Spezifikation, damit sie mit der Chiffre-Spezifikation in der Ereignismeldung übereinstimmt.

## <a name="connector-reference"></a>Connector-Referenz

Informationen zu allen Vorgängen im verwalteten Connector und anderen technischen Informationen wie Eigenschaften, Grenzwerten usw. finden Sie auf der [Referenzseite des MQ-Connectors](/connectors/mq/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
