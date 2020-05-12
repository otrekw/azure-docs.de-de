---
title: Connectors für Azure Logic Apps
description: Automatisieren von Workflows mit Connectors für Azure Logic Apps wie integrierte, verwaltete, lokale, Integrationskonto-, ISE- und Unternehmensconnectors
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 04/24/2020
ms.openlocfilehash: f2a2ee7a2806a753ffd159c91ed782634e74c704
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838657"
---
# <a name="connectors-for-azure-logic-apps"></a>Connectors für Azure Logic Apps

Connectors ermöglichen den schnellen anwendungs-, dienst-, protokoll- und plattformübergreifenden Zugriff von Azure Logic Apps auf Ereignisse, Daten und Aktionen. Durch Verwenden von Connectors in Ihren Logik-Apps erweitern Sie die Funktionen für Ihre Cloud- und lokalen Apps, um Aufgaben mit zu erstellenden oder bereits vorhandenen Daten auszuführen.

Logic Apps bietet [Hunderte Connectors](https://docs.microsoft.com/connectors). In diesem Artikel werden die *beliebten und häufiger verwendeten* Connectors beschrieben, die in Tausenden von Apps und Millionen von Ausführungen zur Verarbeitung von Daten und Informationen erfolgreich verwendet werden. Eine vollständige Liste der Connectors und die Referenzinformationen zu jedem Connector, wie Trigger, Aktionen und Limits, finden Sie auf den Referenzseiten zu Connectors unter [Übersicht über Connectors](https://docs.microsoft.com/connectors). Informieren Sie sich auch über [Trigger und Aktionen](#triggers-actions), das [Preismodell von Logic Apps](../logic-apps/logic-apps-pricing.md) und die [Preise für Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!TIP]
> Zur Integration mit einem Dienst oder einer API, die keine Connectors haben, können Sie den Dienst entweder direkt über ein Protokoll wie HTTP aufrufen oder einen [benutzerdefinierten Connector](#custom) erstellen.

## <a name="connector-types"></a>Connectortypen

Connectors sind entweder als integrierte Trigger und Aktionen oder als verwaltete Connectors verfügbar.

<a name="built-in"></a>

* [**Integriert**](#built-ins): Integrierte Trigger und Aktionen sind in Azure Logic Apps „nativ“ und helfen Ihnen, diese Aufgaben für Ihre Logik-Apps auszuführen:

  * Ausführen nach benutzerdefinierten und erweiterten Zeitplänen.

  * Organisieren und Kontrollieren des Workflows ihrer Logik-App, z. B. Schleifen und Bedingungen, sowie das Arbeiten mit Variablen und Datenvorgängen.

  * Kommunizieren mit anderen Endpunkten.

  * Empfangen von und Reagieren auf Anforderungen.

  * Aufrufen von Azure Functions, Azure-API-Apps (Web-Apps), Ihren eigenen, mit Azure API Management verwalteten und veröffentlichten APIs sowie geschachtelten Logik-Apps, die Anforderungen empfangen können.

<a name="managed-connectors"></a>

* [**Verwaltete Connectors**](#managed-api-connectors): Diese von Microsoft bereitgestellten und verwalteten Connectors stellen Trigger und Aktionen für den Zugriff auf Clouddienste und lokale Systeme bereit, z. B. Office 365, Azure Blob Storage, SQL Server, Dynamics, Salesforce, SharePoint und andere. Einige Connectors unterstützen insbesondere B2B-Kommunikationsszenarien (Business-to-Business) und erfordern ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), das mit Ihrer Logik-App verknüpft ist. Vor der Verwendung bestimmter Connectors müssen Sie möglicherweise zunächst Verbindungen erstellen, die von Azure Logic Apps verwaltet werden.

  Wenn Sie beispielsweise Microsoft BizTalk Server nutzen, können Ihre Logik-Apps eine Verbindung mit Ihrer BizTalk Server-Instanz herstellen und damit kommunizieren, wenn Sie den [lokalen BizTalk Server-Connector](#on-premises-connectors) verwenden. Sie können dann mithilfe der [Integrationskonto-Connectors](#integration-account-connectors) BizTalk-ähnliche Vorgänge in Ihren Logik-Apps erweitern oder durchführen.

  Connectors werden entweder als „Standard“ oder als „Enterprise“ (Unternehmen) klassifiziert. [Unternehmensconnectors](#enterprise-connectors) bieten gegen zusätzliche Gebühren Zugriff auf Unternehmenssysteme wie SAP, IBM MQ und IBM 3270. Um zu ermitteln, ob ein Connector Standard oder Enterprise ist, sehen Sie sich die technischen Details auf der Referenzseite des Connectors auf der Übersichtsseite [Connectors](https://docs.microsoft.com/connectors) an.

  Sie können Connectors auch mithilfe dieser Kategorien identifizieren, obwohl einige Connectors für mehrere Kategorien verwendet werden können. SAP ist beispielsweise ein Unternehmensconnector und ein lokaler Connector:

  |   |   |
  |---|---|
  | [**Verwaltete Connectors**](#managed-api-connectors) | Erstellen von Logik-Apps, die Dienste wie Azure Blob Storage, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online und viele mehr verwenden. |
  | [**Lokale Connectors**](#on-premises-connectors) | Nach dem Installieren und Einrichten des [lokalen Datengateways][gateway-doc] können Ihre Logik-Apps mithilfe dieser Connectors auf lokale Systeme wie SQL Server, SharePoint Server, Oracle DB, Dateifreigaben und andere zugreifen. |
  | [**Integrationskonto-Connectors**](#integration-account-connectors) | Diese Connectors sind verfügbar, wenn Sie ein Integrationskonto erstellen und bezahlen. Die Connectors transformieren und validieren XML, codieren und decodieren Flatfiles und verarbeiten Business-to-Business-Nachrichten (B2B) mit AS2-, EDIFACT- und X12-Protokollen. |
  |||

  > [!IMPORTANT]
  > Wenn Sie den Gmail-Connector verwenden möchten, können nur G-Suite-Geschäftskonten diesen Connector ohne Einschränkung in Logik-Apps verwenden. Wenn Sie über ein Gmail-Consumerkonto verfügen, können Sie diesen Connector nur mit bestimmten von Google genehmigten Diensten verwenden, oder Sie können [eine Google-Client-App erstellen, die für die Authentifizierung mit Ihrem Gmail-Connector verwendet werden soll](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application). Weitere Informationen finden Sie unter [Datensicherheit und Datenschutzrichtlinien für Google-Connectors in Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

<a name="integration-service-environment"></a>

### <a name="connect-from-an-integration-service-environment"></a>Verbinden aus einer Integrationsdienstumgebung

Für Logik-Apps, die Direktzugriff auf Ressourcen in einem virtuellen Azure-Netzwerk benötigen, können Sie eine isolierte [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen, in der Sie Ihre Logik-Apps auf dedizierten Ressourcen erstellen, bereitstellen und ausführen können. Wenn Sie im Logik-App-Designer die Connectors durchsuchen, die Sie für Logik-Apps in einer ISE verwenden möchten, wird eine **CORE**-Bezeichnung bei integrierten Triggern und Aktionen angezeigt, während bei einigen Connectors die **ISE**-Bezeichnung angezeigt wird:

* **CORE**: Integrierte Trigger und Aktionen mit dieser Bezeichnung werden in derselben Integrationsdienstumgebung wie Ihre Logik-Apps ausgeführt:

  ![Beispiel für einen ISE-Connector](./media/apis-list/example-core-connector.png)

* **ISE**: Verwaltete Connectors mit dieser Bezeichnung werden in derselben ISE wie Ihre Logik-Apps ausgeführt, z. B.:

  ![Beispiel für einen ISE-Connector](./media/apis-list/example-ise-connector.png)

  Wenn Sie ein lokales System haben, das mit einem virtuellen Azure-Netzwerk verbunden ist, ermöglicht eine ISE Ihren Logik-Apps den Direktzugriff auf dieses System ohne das [lokale Datengateway](../logic-apps/logic-apps-gateway-connection.md). Stattdessen können Sie entweder, falls verfügbar, den **ISE**-Connector dieses Systems verwenden, eine HTTP-Aktion oder einen [benutzerdefinierten Connector](#custom). Für lokale Systeme, die keine **ISE**-Connectors aufweisen, verwenden Sie das lokale Datengateway. Informationen zu verfügbaren ISE-Connectors finden Sie unter [ISE-Connectors](#ise-connectors).

* Alle anderen Connectors ohne die Bezeichnung **CORE** oder **ISE**, die Sie weiterhin verwenden können, werden im globalen, mehrinstanzenfähigen Logic Apps-Dienst ausgeführt, z. B.:

  ![Beispiel für einen mehrinstanzenfähigen Connector](./media/apis-list/example-multi-tenant-connector.png)

Logik-Apps, die in einer ISE ausgeführt werden, und ihre Connectors, unabhängig davon, wo diese Connectors ausgeführt werden, unterliegen einem festgelegten Tarif im Vergleich zum nutzungsbasierten Tarif. Weitere Informationen finden Sie auf diesen Seiten:

* [Logic Apps – Preismodell](../logic-apps/logic-apps-pricing.md)
* [Logic Apps – Preisdetails](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Herstellen einer Verbindung mit virtuellen Azure-Netzwerken über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="built-ins"></a>

## <a name="built-in"></a>Integriert

Logic Apps umfasst integrierte Trigger und Aktionen, sodass Sie zeitplanbasierte Workflows erstellen, Ihre Logik-Apps mit anderen Apps und Diensten kommunizieren lassen, den Workflow über die Logik-Apps steuern und Daten verwalten oder bearbeiten können.

|   |   |   |   |
|---|---|---|---|
| [![API-Symbol][schedule-icon]<br>**Zeitplan**][schedule-doc] | – Ausführen einer Logik-App mit einer festgelegten Wiederholung auf Grundlage einfacher bis komplexer Zeitpläne mit dem Trigger [**Wiederholung**][schedule-recurrence-doc]. <p>– Ausführen einer Logik-App, die Daten in kontinuierlichen Blöcken verarbeiten muss, mit dem Trigger [**Gleitendes Fenster**][schedule-sliding-window-doc]. <p>– Anhalten Ihrer Logik-App für einen bestimmten Zeitraum mit der Aktion [**Verzögerung**][schedule-delay-doc]. <p>– Anhalten Ihrer Logik-App bis zum festgelegten Zeitpunkt (Datum und Uhrzeit) mit der Aktion [**Verzögern bis**][schedule-delay-until-doc]. | [![API-Symbol][batch-icon]<br>**Batch**][batch-doc] | - Verarbeiten von Nachrichten in Batches mit dem Trigger **Batchnachrichten**. <p>- Aufrufen von Logik-Apps mit vorhandenen Batchtriggern mit der Aktion **Nachrichten an Batch senden**. |
| [![API-Symbol][http-icon]<br>**HTTP**][http-doc] | Aufrufen von HTTP- oder HTTPS-Endpunkten mit Triggern und Aktionen für HTTP. Andere integrierte HTTP-Trigger und -Aktionen umfassen [HTTP + Swagger][http-swagger-doc] und [HTTP + Webhook][http-webhook-doc]. | [![API-Symbol][http-request-icon]<br>**Anforderung**][http-request-doc] | - Festlegen, dass die Logik-App über andere Apps oder Dienste aufrufbar ist und bei Event Grid-Ressourcenereignissen oder bei Antworten auf Azure Security Center-Warnungen mit dem Trigger **Anforderung** ausgelöst wird. <p>- Senden von Antworten an eine App oder einen Dienst mit der Aktion **Antwort**. |
| [![API-Symbol][azure-api-management-icon]<br>**Azure API <br>Management**][azure-api-management-doc] | Aufrufen von Triggern und Aktionen, die in Ihren eigenen APIs definiert sind, die Sie mit Azure API Management verwalten und veröffentlichen. | [![API-Symbol][azure-app-services-icon]<br>**Azure App <br>Services**][azure-app-services-doc] | Aufrufen von Azure API-Apps oder Web-Apps, die in Azure App Service gehostet werden. Die in diesen Apps definierten Trigger und Aktionen werden wie alle anderen erstklassigen Trigger und Aktionen angezeigt, wenn Swagger enthalten ist.|
| [![API-Symbol][azure-logic-apps-icon]<br>**Azure Logic <br>Apps**][nested-logic-app-doc] | Aufrufen anderer Logik-Apps, die mit dem Trigger **Anforderung** gestartet werden. |
|||||

### <a name="run-code-from-logic-apps"></a>Ausführen von Code aus Logic Apps

Logic Apps bietet integrierte Aktionen zum Ausführen Ihres eigenen Codes im Workflow Ihrer Logik-App:

|   |   |   |   |
|---|---|---|---|
| [![API-Symbol][azure-functions-icon]<br>**Azure Functions**][azure-functions-doc] | Aufrufen von Azure-Funktionen, die benutzerdefinierte Codeausschnitte (C# oder Node.js) aus Ihren Logik-Apps ausführen. | [![API-Symbol][inline-code-icon]<br>**Inlinecode**][azure-functions-doc] | Hinzufügen und Ausführen von JavaScript-Codeausschnitten in Ihren Logik-Apps. |
|||||

### <a name="control-workflow"></a>Steuern des Workflows

Logic Apps bietet integrierte Aktionen zum Strukturieren und Steuern der Aktionen im Workflow Ihrer Logik-App:

|   |   |   |   |
|---|---|---|---|
| [![Symbol für integrierte Aktion][condition-icon]<br>**Condition**][condition-doc] | Auswerten einer Bedingung und Ausführen unterschiedlicher Aktionen abhängig davon, ob die Bedingung TRUE oder FALSE ist. | [![Symbol für integrierte Aktion][for-each-icon]<br>**Foreach**][for-each-doc] | Ausführen der gleichen Aktionen für jedes Element in einem Array. |
| [![Symbol für integrierte Aktion][scope-icon]<br>**Scope**][scope-doc] | Gruppieren von Aktionen in *Bereiche*, die einen eigenen Status erhalten, nachdem die Ausführung der Aktionen im Bereich beendet ist. | [![Symbol für integrierte Aktion][switch-icon]<br>**Switch**][switch-doc] | Gruppieren von Aktionen in *Fälle*, denen mit Ausnahme des Standardfalls eindeutige Werte zugewiesen werden. Es wird nur der Fall ausgeführt, dessen zugewiesener Wert mit dem Ergebnis eines Ausdrucks, Objekts oder Token übereinstimmt. Wenn keine Übereinstimmungen vorhanden sind, wird der Standardfall ausgeführt. |
| [![Symbol für integrierte Aktion][terminate-icon]<br>**Terminate**][terminate-doc] | Beenden des aktiv ausgeführten Workflows einer Logik-App. | [![Symbol für integrierte Aktion][until-icon]<br>**Until**][until-doc] | Wiederholen von Aktionen, bis die angegebene Bedingung TRUE ist oder sich ein Status geändert hat. |
|||||

### <a name="manage-or-manipulate-data"></a>Verwalten oder Bearbeiten von Daten

Logic Apps bietet integrierte Aktionen zur Verwendung von Datenausgaben mit den zugehörigen Formaten:

|   |   |
|---|---|
| [![Symbol für integrierte Aktion][data-operations-icon]<br>**Datenvorgänge**][data-operations-doc] | Ausführen von Vorgängen mit Daten: <p>- **Erstellen**: Erstellen einer einzelnen Ausgabe aus mehreren Eingaben mit verschiedenen Typen. <br>- **CSV-Tabelle erstellen**: Erstellen einer Tabelle mit durch Komma getrennten Werten (CSV-Tabelle) aus einem Array mit JSON-Objekten. <br>- **HTML-Tabelle erstellen**: Erstellen einer HTML-Tabelle aus einem Array mit JSON-Objekten. <br>- **Array filtern**: Erstellen eines Arrays aus Elementen in einem anderen Array, die Ihren Kriterien entsprechen. <br>- **Verknüpfen**: Erstellen einer Zeichenfolge aus allen Elementen in einem Array und Trennen dieser Elemente mit dem angegebenen Trennzeichen. <br>- **JSON analysieren**: Erstellen von benutzerfreundlichen Token aus Eigenschaften und den zugehörigen Werten in JSON-Inhalten, sodass Sie diese Eigenschaften in Ihrem Workflow verwenden können. <br>- **Auswählen**: Erstellen eines Arrays mit JSON-Objekten durch Transformieren von Elementen oder Werten in einem anderen Array und Zuordnen dieser Elemente zu angegebenen Eigenschaften. |
| ![Symbol für integrierte Aktion][date-time-icon]<br>**Datum und Uhrzeit** | Ausführen von Vorgängen mit Zeitstempeln: <p>- **Zu Zeit hinzufügen**: Hinzufügen der angegebenen Anzahl von Einheiten zu einem Zeitstempel. <br>- **Zeitzone konvertieren**: Konvertiert einen Zeitstempel von der Quellzeitzone in die Zielzeitzone. <br>- **Aktuelle Zeit**: Gibt den aktuellen Zeitstempel als Zeichenfolge zurück. <br>- **Zukünftige Zeit abrufen**: Gibt den aktuellen Zeitstempel plus der angegebenen Zeiteinheiten zurück. <br>- **Vergangene Zeit abrufen**: Gibt den aktuellen Zeitstempel abzüglich der angegebenen Zeiteinheiten zurück. <br>- **Von Uhrzeit abziehen**: Subtrahiert eine Anzahl von Zeiteinheiten von einem Zeitstempel. |
| [![Symbol für integrierte Aktion][variables-icon]<br>**Variablen**][variables-doc] | Ausführen von Vorgängen mit Variablen: <p>- **An Arrayvariable anfügen**: Einfügen eines Werts als letztes Element in einem Array, das in einer Variablen gespeichert ist. <br>- **An Zeichenfolgenvariable anfügen**: Einfügen eines Werts als letztes Element in einer Zeichenfolge, die in einer Variablen gespeichert ist. <br>- **Variablenwert verringern**: Verringern einer Variablen um einen konstanten Wert. <br>- **Variable schrittweise erhöhen**: Erhöhen einer Variablen um einen konstanten Wert. <br>- **Variable initialisieren**: Erstellen einer Variablen und Deklarieren ihres Datentyps und Anfangswert. <br>- **Variable festlegen**: Zuweisen eines anderen Werts zu einer vorhandenen Variable. |
|  |  |

<a name="managed-api-connectors"></a>

## <a name="managed-connectors"></a>Verwaltete Connectors

Logic Apps bietet die folgenden beliebten Standardconnectors zur Automatisierung von Aufgaben, Prozessen und Workflows mit folgenden Diensten oder Systemen:

|   |   |   |   |
|---|---|---|---|
| [![API-Symbol][azure-service-bus-icon]<br>**Azure Service Bus**][azure-service-bus-doc] | Verwalten von asynchronen Nachrichten, Sitzungen und Themenabonnements mit dem am häufigsten verwendeten Connector in Logic Apps. | [![API-Symbol][sql-server-icon]<br>**SQL Server**][sql-server-doc] | Herstellen einer Verbindung mit Ihrer lokalen SQL Server-Instanz oder einer Azure SQL-Datenbank in der Cloud, sodass Sie Datensätze verwalten, gespeicherte Prozeduren ausführen oder Abfragen ausführen können. |
| [![API-Symbol][azure-blob-storage-icon]<br>**Azure Blob<br>Storage**][azure-blob-storage-doc] | Herstellen einer Verbindung mit Ihrem Storage-Konto, sodass Sie Blobinhalte erstellen und verwalten können. | [![API-Symbol][office-365-outlook-icon]<br>**Office 365<br>Outlook**][office-365-outlook-doc] | Herstellen einer Verbindung mit Ihrem Office 365-E-Mail-Konto, sodass Sie E-Mails, Aufgaben, Kalenderereignisse und Besprechungen, Kontakte, Anforderungen und mehr erstellen und verwalten können. |
| [![API-Symbol][sftp-ssh-icon]<br>**SFTP-SSH**][sftp-ssh-doc] | Herstellen einer Verbindung mit SFTP-Servern, auf die Sie über das Internet mittels SSH zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können. | [![API-Symbol][sharepoint-online-icon]<br>**SharePoint<br>Online**][sharepoint-online-doc] | Herstellen einer Verbindung mit SharePoint Online, sodass Sie Dateien, Anlagen, Ordner und mehr verwalten können. | 
| [![API-Symbol][dynamics-365-icon]<br>**Dynamics 365<br>** ][dynamics-365-doc] | Herstellen einer Verbindung mit Ihrem Dynamics 365-Konto, sodass Sie Datensätze, Elemente und mehr erstellen und verwalten können. | [![API-Symbol][azure-queues-icon]<br>**Azure <br>Queues**][azure-queues-doc] | Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Warteschlangen und Nachrichten erstellen und verwalten können |
| [![API-Symbol][ftp-icon]<br>**FTP**][ftp-doc] | Herstellen einer Verbindung mit FTP-Servern, auf die Sie über das Internet zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können. | [![API-Symbol][file-system-icon]<br>**Datei<br>system**][file-system-doc] | Herstellen einer Verbindung mit Ihrer lokalen Dateifreigabe, sodass Sie Dateien erstellen und verwalten können. |
| [![API-Symbol][azure-event-hubs-icon]<br>**Azure Event Hubs**][azure-event-hubs-doc] | Nutzen und Veröffentlichen von Ereignissen über einen Event Hub. Sie können beispielsweise mithilfe von Event Hubs eine Ausgabe Ihrer Logik-App abrufen und die Ausgabe anschließend an einen Echtzeitanalyseanbieter senden. | [![API-Symbol][azure-event-grid-icon]<br>**Azure Event**<br>**Grid**][azure-event-grid-doc] | Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z.B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen. |
| [![API-Symbol][salesforce-icon]<br>**Salesforce**][salesforce-doc] | Herstellen einer Verbindung mit Ihrem Salesforce-Konto, sodass Sie Elemente wie Datensätze, Aufträge, Objekte usw. erstellen und verwalten können. | [![API-Symbol][twitter-icon]<br>**Twitter**][twitter-doc] | Herstellen einer Verbindung mit Ihrem Twitter-Konto, sodass Sie Tweets, Follower, die Zeitachse usw. verwalten können. Ihre Tweets können Sie in SQL, Excel oder SharePoint speichern. |
|||||

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Lokale Connectors

Im Folgenden sind einige häufig verwendete Standardconnectors aufgeführt, die Logic Apps für den Zugriff auf Daten und Ressourcen in lokalen Systemen bereitstellt. Damit Sie eine Verbindung mit einem lokalen System herstellen können, müssen Sie zunächst [ein lokales Datengateway herunterladen, installieren und einrichten][gateway-doc]. Dieses Gateway stellt einen sicheren Kommunikationskanal dar, ohne dass die erforderliche Netzwerkinfrastruktur eingerichtet werden muss.

|   |   |   |   |   |
|---|---|---|---|---|
| [![API-Symbol][biztalk-server-icon]<br>**BizTalk** <br>**Server**][biztalk-server-doc] | [![API-Symbol][file-system-icon]<br>**Datei<br>system**][file-system-doc] | [![API-Symbol][ibm-db2-icon]<br>**IBM DB2**][ibm-db2-doc] | [![API-Symbol][ibm-informix-icon]<br>**IBM** <br>**Informix**][ibm-informix-doc] | [![API-Symbol][mysql-icon]<br>**MySQL**][mysql-doc] |
| [![API-Symbol][oracle-db-icon]<br>**Oracle DB**][oracle-db-doc] | [![API-Symbol][postgre-sql-icon]<br>**PostgreSQL**][postgre-sql-doc] | [![API-Symbol][sharepoint-server-icon]<br>**SharePoint <br>Server**][sharepoint-server-doc] | [![API-Symbol][sql-server-icon]<br>**SQL <br>Server**][sql-server-doc] | [![API-Symbol][teradata-icon]<br>**Teradata**][teradata-doc] |
|||||

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Integrationskonto-Connectors

Logic Apps bietet Standardconnectors zum Erstellen von Business-to-Business-Lösungen (B2B) mit Ihren Logik-Apps, wenn Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erstellen und bezahlen. Dieses ist über das Enterprise Integration Pack (EIP) in Azure verfügbar. Mit diesem Konto können Sie B2B-Artefakte wie Handelspartner, Vereinbarungen, Zuordnungen, Schemas, Zertifikate usw. erstellen und speichern. Zur Verwendung dieser Artefakte ordnen Sie Ihre Logik-Apps dem Integrationskonto zu. Wenn Sie derzeit BizTalk Server verwenden, sind Ihnen diese Connectors möglicherweise bereits bekannt.

|   |   |   |   |
|---|---|---|---|
| [![API-Symbol][as2-icon]<br>**AS2-<br>Decodierung**][as2-doc] | [![API-Symbol][as2-icon]<br>**AS2-<br>Codierung**][as2-doc] | [![API-Symbol][edifact-icon]<br>**EDIFACT-<br>Decodierung**][edifact-decode-doc] | [![API-Symbol][edifact-icon]<br>**EDIFACT-<br>Codierung**][edifact-encode-doc] |
| [![API-Symbol][flat-file-decode-icon]<br>**Flatfile-<br>Decodierung**][flat-file-decode-doc] | [![API-Symbol][flat-file-encode-icon]<br>**Flatfile-<br>Codierung**][flat-file-encode-doc] | [![API-Symbol][integration-account-icon]<br>**Integrations<br>konto**][integration-account-doc] | [![API-Symbol][liquid-icon]<br>**Liquid**-<br>**Transformationen**][json-liquid-transform-doc] |
| [![API-Symbol][x12-icon]<br>**X12-<br>Decodierung**][x12-decode-doc] | [![API-Symbol][x12-icon]<br>**X12-<br>Codierung**][x12-encode-doc] | [![API-Symbol][xml-transform-icon]<br>**XML**-<br>**Transformationen**][xml-transform-doc] | [![API-Symbol][xml-validate-icon]<br>**XML-<br>Überprüfung**][xml-validate-doc] |  
|||||

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Enterprise-Connectors

Logic Apps bietet die folgenden Unternehmensconnectors für den Zugriff auf Unternehmenssysteme wie SAP und IBM MQ:

|   |   |   |
|---|---|---|
| [![API-Symbol][ibm-3270-icon]<br>**IBM 3270**][ibm-3270-doc] | [![API-Symbol][ibm-mq-icon]<br>**IBM MQ**][ibm-mq-doc] | [![API-Symbol][sap-icon]<br>**SAP**][sap-connector-doc] |
||||

<a name="ise-connectors"></a>

## <a name="ise-connectors"></a>ISE-Connectors

Für Logik-Apps, die Sie erstellen und in einer isolierten [Integrationsdienstumgebung (ISE)](#integration-service-environment) ausführen, identifiziert der Logik-App-Designer integrierte Trigger und Aktionen, die in ihrer ISE ausgeführt werden, indem die **CORE**-Bezeichnung verwendet wird. Verwaltete Connectors, die in einer ISE ausgeführt werden, weisen die **ISE**-Bezeichnung auf, während Connectors, die im globalen, mehrinstanzenfähigen Logic Apps-Dienst ausgeführt werden, keine Bezeichnung aufweisen. Diese Liste zeigt die Connectors, die derzeit über ISE-Versionen verfügen:

|   |   |   |   |   |
|---|---|---|---|---|
[![API-Symbol][as2-icon]<br>**AS2**][as2-doc] | [![API-Symbol][azure-automation-icon]<br>**Azure <br>Automation**][azure-automation-doc] | [![API-Symbol][azure-blob-storage-icon]<br>**Azure Blob<br>Storage**][azure-blob-storage-doc] | [![API-Symbol][azure-cosmos-db-icon]<br>**Azure Cosmos <br> DB**][azure-cosmos-db-doc] | [![API-Symbol][azure-event-hubs-icon]<br>**Azure Event <br>Hubs**][azure-event-hubs-doc] |
[![API-Symbol][azure-event-grid-icon]<br>**Azure Event <br>Raster**][azure-event-grid-doc] | [![API-Symbol][azure-file-storage-icon]<br>**Azure File<br>Storage**][azure-file-storage-doc] | [![API-Symbol][azure-key-vault-icon]<br>**Azure Key <br>Vault**][azure-key-vault-doc] | [![API-Symbol][azure-monitor-logs-icon]<br>**Azure Monitor-<br>Protokolle**][azure-monitor-logs-doc] | [![API-Symbol][azure-service-bus-icon]<br>**Azure Service <br>Bus**][azure-service-bus-doc] |
| [![API-Symbol][azure-sql-data-warehouse-icon]<br>**Azure SQL Data <br>Warehouse**][azure-sql-data-warehouse-doc] | [![API-Symbol][azure-table-storage-icon]<br>**Azure Table <br>Storage**][azure-table-storage-doc] | [![API-Symbol][azure-queues-icon]<br>**Azure <br>Queues**][azure-queues-doc] | [![API-Symbol][edifact-icon]<br>**EDIFACT**][edifact-doc] | [![API-Symbol][file-system-icon]<br>**Datei<br>system**][file-system-doc] |
| [![API-Symbol][ftp-icon]<br>**FTP**][ftp-doc] | [![API-Symbol][ibm-3270-icon]<br>**IBM 3270**][ibm-3270-doc] | [![API-Symbol][ibm-db2-icon]<br>**IBM DB2**][ibm-db2-doc] | [![API-Symbol][ibm-mq-icon]<br>**IBM MQ**][ibm-mq-doc] | [![API-Symbol][sap-icon]<br>**SAP**][sap-connector-doc] |
| [![API-Symbol][sftp-ssh-icon]<br>**SFTP-SSH**][sftp-ssh-doc] | [![API-Symbol][smtp-icon]<br>**SMTP**][smtp-doc] | [![API-Symbol][sql-server-icon]<br>**SQL <br>Server**][sql-server-doc] | [![API-Symbol][x12-icon]<br>**X12**][x12-doc] |
||||||

Weitere Informationen finden Sie in den folgenden Themen:

* [Zugriff auf virtuelle Azure-Netzwerkressourcen aus Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Logic Apps – Preismodell](../logic-apps/logic-apps-pricing.md)
* [Herstellen einer Verbindung mit virtuellen Azure-Netzwerken über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

<a name="triggers-actions"></a>

## <a name="triggers-and-action-types"></a>Trigger und Aktionstypen

Connectors können *Trigger*, *Aktionen* oder beides bereitstellen. Ein *Trigger* ist der erste Schritt in jeder Logik-App und gibt in der Regel das Ereignis an, das den Trigger auslöst und die Ausführung der Logik-App startet. Der FTP-Connector beispielsweise verfügt über einen Trigger, der Ihre Logik-App startet, „wenn eine Datei hinzugefügt oder geändert wird“. Einige Trigger suchen regelmäßig nach dem angegebenen Ereignis oder den Daten und lösen aus, wenn das angegebene Ereignis oder die Daten erkannt wurden. Andere Trigger befinden sich im Wartezustand, lösen aber sofort aus, wenn ein bestimmtes Ereignis eintritt oder neue Daten verfügbar sind. Trigger übergeben außerdem alle erforderlichen Daten an Ihre Logik-App. Ihre Logik-App kann diese Daten im gesamten Workflow lesen und verwenden. Der Twitter-Connector beispielsweise verfügt über den Trigger „Wenn ein neuer Tweet gepostet wird“, der den Inhalt des Tweets an den Workflow Ihrer Logik-App übergibt.

Wenn ein Trigger ausgelöst wird, erstellt Azure Logic Apps eine Instanz der Logik-App und beginnt mit der Ausführung der *Aktionen* im Workflow Ihrer Logik-App. Aktionen sind die Schritte, die auf den Trigger folgen und Aufgaben im Workflow Ihrer Logik-App ausführen. Sie können beispielsweise eine Logik-App erstellen, die Kundendaten aus einer SQL-Datenbank abruft und diese Daten in späteren Aktionen verarbeitet.

Nachfolgend sind die allgemeinen Triggerarten von Azure Logic Apps aufgelistet:

* *Wiederholungstrigger*: Dieser Trigger wird nach einem festgelegten Zeitplan ausgeführt und ist nicht mit einem bestimmten Dienst oder System verknüpft.

* *Abfragetrigger*: Dieser Trigger fragt anhand des angegebenen Zeitplans regelmäßig einen bestimmten Dienst oder ein bestimmtes System ab und sucht nach neuen Daten oder prüft, ob ein bestimmtes Ereignis aufgetreten ist. Wenn neue Daten verfügbar sind oder das jeweilige Ereignis aufgetreten ist, erstellt der Trigger eine neue Instanz der Logik-App und führt diese aus. Diese kann nun die übergebenen Daten als Eingabe verwenden.

* *Pushtrigger*: Dieser Trigger wartet und lauscht, ob neue Daten verfügbar sind oder ein Ereignis eintritt. Wenn neue Daten verfügbar sind oder das jeweilige Ereignis aufgetreten ist, erstellt der Trigger eine neue Instanz der Logik-App und führt diese aus. Diese kann nun die übergebenen Daten als Eingabe verwenden.

<a name="connections"></a>

## <a name="connector-configuration"></a>Connector-Konfiguration

Die Trigger und Aktionen der einzelnen Connectors verfügen über eigene Eigenschaften, die Sie konfigurieren müssen. Bei vielen Connectors müssen Sie zuerst eine *Verbindung* zum Zieldienst oder -system herstellen und Anmeldeinformationen zur Authentifizierung oder andere Konfigurationsdetails bereitstellen, bevor Sie einen Trigger oder eine Aktion in Ihrer Logik-App verwenden können. Beispielsweise müssen Sie eine Verbindung mit einem Twitter-Konto autorisieren, damit Sie auf Daten zugreifen oder Beiträge in Ihrem Namen posten können.

Bei Connectors, die Azure AD-OAuth (Azure Active Directory) verwenden, bedeutet das Erstellen einer Verbindung die Anmeldung bei dem Dienst (z. B. Office 365, Salesforce oder GitHub), wobei das Zugriffstoken [verschlüsselt](../security/fundamentals/encryption-overview.md) und sicher in einem Azure-Geheimnisspeicher gespeichert wird. Andere Connectors (z.B. FTP und SQL) erfordern eine Verbindung mit Konfigurationsdetails wie Serveradresse, Benutzername und Kennwort. Diese Verbindungskonfigurationsdetails werden ebenfalls verschlüsselt und sicher gespeichert. Weitere Informationen zur Verschlüsselung in Azure finden Sie [hier](../security/fundamentals/encryption-overview.md).

Verbindungen können solange auf den Zieldienst oder das Zielsystem zugreifen, wie der Dienst oder das System dies zulässt. Bei Diensten, die Azure AD-OAuth-Verbindungen verwenden (z. B. Office 365 und Dynamics), aktualisiert Azure Logic Apps die Zugriffstoken unbegrenzt. Bei anderen Diensten gibt es unter Umständen eine zeitliche Begrenzung für die Verwendung eines Tokens durch Azure Logic Apps, nach der eine Aktualisierung erforderlich ist. Bei einigen Aktionen (z.B. Ändern des Kennworts) werden in der Regel alle Zugriffstoken ungültig.

<a name="custom"></a>

## <a name="custom-apis-and-connectors"></a>Benutzerdefinierte APIs und Connectors

Zum Aufrufen von APIs, die benutzerdefinierten Code ausführen oder nicht als Connector verfügbar sind, können Sie die Logic Apps-Plattform erweitern, indem Sie [benutzerdefinierte API-Apps erstellen](../logic-apps/logic-apps-create-api-app.md). Sie können außerdem für *alle* REST- oder SOAP-basierten APIs [benutzerdefinierte Connectors erstellen](../logic-apps/custom-connector-overview.md), mit denen diese APIs in allen Logik-Apps in Ihrem Azure-Abonnement verfügbar werden. Um benutzerdefinierte API-Apps oder Connectors für alle Benutzer in Azure öffentlich zu machen, können Sie [Connectors für die Microsoft-Zertifizierung einreichen](../logic-apps/custom-connector-submit-certification.md).

> [!NOTE]
> Logik-Apps, die Sie in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) bereitstellen und ausführen, können direkt auf Ressourcen in einem virtuellen Azure-Netzwerk zugreifen. Wenn Sie über benutzerdefinierte Connectors verfügen, für die das lokale Datengateway erforderlich ist, und Sie diese Connectors außerhalb einer ISE erstellt haben, können Logik-Apps in einer ISE diese Connectors ebenfalls verwenden.
>
> Benutzerdefinierte Connectors, die in einer ISE erstellt wurden, funktionieren nicht mit dem lokalen Datengateway. Diese Connectors können jedoch direkt auf lokale Datenquellen zugreifen, die mit dem virtuellen Azure-Netzwerk verbunden sind, das die ISE hostet. Deshalb benötigen Logik-Apps in einer ISE sehr wahrscheinlich das Datengateway nicht, wenn sie mit diesen Ressourcen kommunizieren.
>
> Weitere Informationen zum Erstellen von ISEs finden Sie unter [Verbinden mit Azure Virtual Networks über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

## <a name="next-steps"></a>Nächste Schritte

* Anzeigen der [Vollständigen Liste der Connectors](https://docs.microsoft.com/connectors)
* [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Erstellen benutzerdefinierter Connectors für Logik-Apps](https://docs.microsoft.com/connectors/custom-connectors/)
* [Erstellen einer benutzerdefinierten API zur Verwendung mit Logik-Apps](../logic-apps/logic-apps-create-api-app.md)

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[inline-code-icon]: ./media/apis-list/inline-code.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-file-storage-icon]: ./media/apis-list/azure-file-storage.png
[azure-key-vault-icon]: ./media/apis-list/azure-key-vault.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-monitor-logs-icon]: ./media/apis-list/azure-monitor-logs.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[azure-sql-data-warehouse-icon]: ./media/apis-list/azure-sql-data-warehouse.png
[azure-table-storage-icon]: ./media/apis-list/azure-table-storage.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-3270-icon]: ./media/apis-list/ibm-3270.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-ssh-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png

<!--Other doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Herstellen einer Verbindung mit lokalen Datenquellen über Logik-Apps mit dem lokalen Datengateway"

<!--Built-in doc links-->
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Erstellen einer Azure API Management-Dienstinstanz zum Verwalten und Veröffentlichen von APIs"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integration von Logik-Apps in App Service-API-Apps"
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Integration von Logik-Apps in Azure Functions"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Verarbeiten von Nachrichten in Gruppen oder als Batches"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Auswerten einer Bedingung und Ausführen unterschiedlicher Aktionen abhängig davon, ob die Bedingung TRUE oder FALSE ist"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Ausführen der gleichen Aktionen für jedes Element in einem Array"
[http-doc]: ./connectors-native-http.md "Aufrufen von HTTP- oder HTTPS-Endpunkten aus Ihren Logik-Apps"
[http-request-doc]: ./connectors-native-reqres.md "Empfangen von HTTP-Anforderungen in Ihren Logik-Apps"
[http-response-doc]: ./connectors-native-reqres.md "Antworten auf HTTP-Anforderungen aus Ihren Logik-Apps"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Aufrufen von REST-Endpunkten aus Ihren Logik-Apps"
[http-webhook-doc]: ./connectors-native-webhook.md "Warten auf bestimmte Ereignisse von HTTP- oder HTTPS-Endpunkten"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Integrieren von Logik-Apps in geschachtelte Workflows"
[query-doc]: ../logic-apps/logic-apps-perform-data-operations.md#filter-array-action "Auswählen und Filtern von Arrays mit der Abfrageaktion"
[schedule-doc]: ../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md "Ausführen von Logik-Apps, basierend auf einem Zeitplan"
[schedule-delay-doc]: ./connectors-native-delay.md "Verzögern der Ausführung der nächsten Aktion"
[schedule-delay-until-doc]: ./connectors-native-delay.md "Verzögern der Ausführung der nächsten Aktion"
[schedule-recurrence-doc]:  ./connectors-native-recurrence.md "Ausführen von Logik-Apps nach einem sich wiederholenden Zeitplan"
[schedule-sliding-window-doc]: ./connectors-native-sliding-window.md "Ausführen von Logik-Apps, die Daten in zusammenhängenden Blöcken verarbeiten müssen"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Organisieren von Aktionen in Gruppen, die einen eigenen Status erhalten, nachdem die Ausführung der Aktionen in der Gruppe beendet ist"
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organisieren von Aktionen in Fälle, denen eindeutige Werte zugewiesen werden. Es wird nur der Fall ausgeführt, dessen Wert mit dem Ergebnis eines Ausdrucks, Objekts oder Token übereinstimmt. Wenn keine Übereinstimmungen vorhanden sind, wird der Standardfall ausgeführt."
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Beenden oder Abbrechen eines aktiv ausgeführten Workflows für Ihre Logik-App"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Wiederholen von Aktionen, bis die angegebene Bedingung TRUE ist oder sich ein Status geändert hat"
[data-operations-doc]: ../logic-apps/logic-apps-perform-data-operations.md "Ausführen von Datenvorgängen, z.B. das Filtern von Arrays oder das Erstellen von CSV- und HTML-Tabellen"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Ausführen von Vorgängen mit Variablen, z.B. Initialisieren, Festlegen, Erhöhen oder Verringern in einzelnen Schritten sowie Anfügen an Arrayvariable oder Zeichenfolgenvariable"

<!--Managed connector doc links-->
[azure-automation-doc]: https://docs.microsoft.com/connectors/azureautomation/ "Erstellen und Verwalten von Automatisierungsaufträgen für Ihre Cloud und lokale Infrastruktur".
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Verwalten von Dateien in Ihrem Blobcontainer mit Azure Blob Storage-Connector"
[azure-cosmos-db-doc]: https://docs.microsoft.com/connectors/documentdb/ "Herstellen einer Verbindung mit Azure Cosmos DB, damit Sie auf Dokumente und gespeicherte Prozeduren zugreifen können"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z. B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Herstellen einer Verbindung mit Azure Event Hubs, sodass Sie Ereignisse zwischen Logik-Apps und Event Hubs empfangen und senden können"
[azure-file-storage-doc]: https://docs.microsoft.com/connectors/azurefile/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Dateien erstellen, aktualisieren, abrufen und löschen können"
[azure-key-vault-doc]: https://docs.microsoft.com/connectors/keyvault/ "Herstellen einer Verbindung mit Ihrem Azure Key Vault, sodass Sie Ihre Geheimnisse und Schlüssel verwalten können"
[azure-monitor-logs-doc]: https://docs.microsoft.com/connectors/azuremonitorlogs/ "Ausführen von Abfragen an Azure Monitor-Protokolle über Log Analytics-Arbeitsbereiche und Application Insights-Komponenten hinweg"
[azure-queues-doc]: https://docs.microsoft.com/connectors/azurequeues/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Warteschlangen und Nachrichten erstellen und verwalten können"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Senden von Nachrichten aus Service Bus-Warteschlangen und -Themen und Empfangen von Nachrichten aus Service Bus-Warteschlangen und -Abonnements"
[azure-sql-data-warehouse-doc]: https://docs.microsoft.com/connectors/sqldw/ "Herstellen einer Verbindung mit Azure SQL Data Warehouse, sodass Sie Ihre Daten anzeigen können"
[azure-table-storage-doc]: https://docs.microsoft.com/connectors/azuretables/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Dateien erstellen und aktualisieren, Tabellen abfragen und mehr können"
[biztalk-server-doc]: https://docs.microsoft.com/connectors/biztalk/ "Herstellen einer Verbindung mit Ihrem BizTalk Server, sodass Sie BizTalk-basierte Anwendungen parallel mit Azure Logic Apps ausführen können"
[box-doc]: ./connectors-create-api-box.md "Stellen Sie eine Verbindung mit Box her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[dropbox-doc]: ./connectors-create-api-dropbox.md "Stellen Sie eine Verbindung mit Dropbox her. Sie können Dateien hochladen, abrufen, löschen, auflisten und vieles mehr."
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Herstellen einer Verbindung mit Dynamics CRM Online zur Verwendung von CRM Online-Daten"
[facebook-doc]: ./connectors-create-api-facebook.md "Stellen Sie eine Verbindung mit Facebook her. Sie können eine Timeline posten, einen Seitenfeed abrufen und vieles mehr."
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Herstellen einer Verbindung mit einem lokalen Dateisystem"
[ftp-doc]: ./connectors-create-api-ftp.md "Herstellen einer Verbindung mit einem FTP/FTPS-Server für FTP-Aufgaben, z.B. Hochladen, Abrufen, Löschen von Dateien usw."
[github-doc]: ./connectors-create-api-github.md "Herstellen einer Verbindung mit GitHub und Nachverfolgen von Problemen"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Herstellen einer Verbindung mit Google Calendar und zum Verwalten des Kalenders"
[google-drive-doc]: ./connectors-create-api-googledrive.md "Herstellen einer Verbindung mit GoogleDrive zur Verwendung mit Ihren Daten"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Herstellen einer Verbindung mit Google Sheets zur Bearbeitung Ihrer Tabellen"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Herstellen einer Verbindung mit Google Tasks zur Verwaltung Ihrer Aufgaben"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Herstellen einer Verbindung mit 3270-Apps auf IBM-Mainframes"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Stellen Sie eine Verbindung mit IBM DB2 in der Cloud oder lokal her. Sie können eine Zeile aktualisieren, eine Tabelle abrufen und vieles mehr."
[ibm-informix-doc]: ./connectors-create-api-informix.md "Stellen Sie eine Verbindung mit Informix in der Cloud oder lokal her. Sie können eine Zeile lesen, die Tabellen auflisten und vieles mehr."
[ibm-mq-doc]: ./connectors-create-api-mq.md "Herstellen einer Verbindung mit IBM MQ (lokal oder in Azure) und Senden und Empfangen von Nachrichten"
[instagram-doc]: ./connectors-create-api-instagram.md "Stellen Sie eine Verbindung mit Instagram her. Lösen Sie Ereignisse aus, oder reagieren Sie darauf."
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Stellen Sie eine Verbindung mit Ihrem MailChimp-Konto her. Verwalten und automatisieren Sie E-Mails."
[mandrill-doc]: ./connectors-create-api-mandrill.md "Herstellen einer Verbindung mit Mandrill zur Kommunikation"
[mysql-doc]: https://docs.microsoft.com/connectors/mysql/ "Herstellen einer Verbindung mit Ihrer lokalen MySQL-Datenbank, sodass Sie Daten lesen und schreiben können"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Herstellen einer Verbindung mit Ihrem Office 365-Konto, sodass Sie E-Mails senden und empfangen sowie Ihren Kalender und Ihre Kontakte verwalten und mehr können"
[office-365-users-doc]: ./connectors-create-api-office365-users.md
[onedrive-doc]: ./connectors-create-api-onedrive.md "Herstellen einer Verbindung mit Ihrem persönlichen Microsoft OneDrive-Ordner, sodass Sie Dateien hochladen, löschen, auflisten und mehr können"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Herstellen einer Verbindung mit Ihrem geschäftlichen Microsoft OneDrive-Ordner, sodass Sie Ihre Dateien hochladen, löschen, auflisten und mehr können"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Herstellen einer Verbindung mit einer Oracle-Datenbank, sodass Sie Zeilen hinzufügen, einfügen, löschen und mehr können"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Herstellen einer Verbindung mit Ihrem Outlook-Postfach, sodass Sie Ihre E-Mails, Kalender, Kontakte und mehr verwalten können"
[postgre-sql-doc]: https://docs.microsoft.com/connectors/postgresql/ "Herstellen einer Verbindung mit Ihrer PostgreSQL-Datenbank, sodass Sie Daten aus Tabellen lesen können"
[project-online-doc]: ./connectors-create-api-projectonline.md "Herstellen einer Verbindung mit Microsoft Project Online, sodass Sie Ihre Projekte, Vorgänge, Ressourcen und mehr verwalten können"
[rss-doc]: ./connectors-create-api-rss.md "Veröffentlichen und Abrufen von Feedelementen, Auslösen von Vorgängen, wenn ein neues Element in einem RSS-Feed veröffentlicht wird"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Stellen Sie eine Verbindung mit Ihrem Salesforce-Konto her. Sie können Konten, Leads, Vertriebschancen usw. verwalten."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Herstellen einer Verbindung mit einem lokalen SAP-System"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Stellen Sie eine Verbindung mit SendGrid her. Sie können E-Mails senden und die Liste der Empfänger verwalten."
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "Herstellen einer Verbindung mit Ihrem SFTP-Konto mittels SSH. Sie können Dateien hochladen, abrufen, löschen und vieles mehr."
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Stellen Sie eine Verbindung mit dem lokalen SharePoint-Server her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Stellen Sie eine Verbindung mit SharePoint Online her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[slack-doc]: ./connectors-create-api-slack.md "Herstellen einer Verbindung mit Slack zum Veröffentlichen von Nachrichten in Slack-Kanälen"
[smtp-doc]: ./connectors-create-api-smtp.md "Herstellen einer Verbindung mit einem SMTP-Server und Senden von E-Mails mit Anhängen"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Herstellen einer Verbindung mit SparkPost zur Kommunikation"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Stellen Sie eine Verbindung mit Azure SQL-Datenbank oder SQL Server her. Erstellten, Aktualisieren, Abrufen und Löschen von Einträgen in einer SQL-Datenbanktabelle"
[teradata-doc]: https://docs.microsoft.com/connectors/teradata/ "Herstellen einer Verbindung mit Ihrer Teradata-Datenbank zum Lesen von Daten aus Tabellen"
[trello-doc]: ./connectors-create-api-trello.md "Stellen Sie eine Verbindung mit Trello her. Sie können Ihre Projekte verwalten und viele verschiedene Dinge organisieren."
[twilio-doc]: ./connectors-create-api-twilio.md "Stellen Sie eine Verbindung mit Twilio her. Sie können Nachrichten senden und empfangen, verfügbare Nummern abrufen, eingehende Telefonnummern verwalten und vieles mehr."
[twitter-doc]: ./connectors-create-api-twitter.md "Stellen Sie eine Verbindung mit Twitter her. Sie können Timelines abrufen, Tweets posten und vieles mehr."
[yammer-doc]: ./connectors-create-api-yammer.md "Stellen Sie eine Verbindung mit Yammer her. Sie können Nachrichten posten, neue Nachrichten abrufen und vieles mehr."
[youtube-doc]: ./connectors-create-api-youtube.md "Stellen Sie eine Verbindung mit YouTube her. Sie können Ihre Videos und Kanäle verwalten."

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Codieren und Decodieren von Nachrichten, die das AS2-Protokoll verwenden"
[edifact-doc]: ../logic-apps/logic-apps-enterprise-integration-edifact.md "Codieren und Decodieren von Nachrichten, die das EDIFACT-Protokoll verwenden"
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Decodieren von Nachrichten, die das EDIFACT-Protokoll verwenden"
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Codieren von Nachrichten, die das EDIFACT-Protokoll verwenden"
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)"
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Informationen zu Enterprise Integration (Flatfile)"
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Verwalten von Metadaten für Integrationskontoartefakte"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Transformieren von JSON mithilfe einer Liquid-Vorlage"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Codieren und Decodieren von Nachrichten, die das X12-Protokoll verwenden"
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Decodieren von Nachrichten, die das X12-Protokoll verwenden"
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Codieren von Nachrichten, die das X12-Protokoll verwenden"
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Transformieren von XML-Nachrichten"
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Überprüfen von XML-Nachrichten"

