---
title: Verwaltete Connectors für Azure Logic Apps
description: Verwenden Sie von Microsoft verwaltete Trigger und Aktionen, um automatisierte Workflows zu erstellen, die andere Apps, Daten, Dienste und Systeme mithilfe von Azure Logic Apps integrieren.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm, azla
ms.topic: conceptual
ms.date: 04/20/2021
ms.openlocfilehash: d50cf1009fb674f2d3d5914715dcbd9699565489
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107796058"
---
# <a name="managed-connectors-for-logic-apps"></a>Verwaltete Connectors für Azure Logic Apps

Mit [verwalteten Connectors](apis-list.md) können Sie auf andere Dienste und Systeme zugreifen, in denen keine [integrierten Trigger und Aktionen](built-in.md) verfügbar sind. Sie können diese Trigger und Aktionen verwenden, um Workflows zu erstellen, die Daten, Apps, cloudbasierte Dienste und lokale Systeme integrieren. Anders als integrierte Trigger und Aktionen sind diese Connectors in der Regel an einen bestimmten Dienst oder ein bestimmtes System wie Azure Blob Storage, Office 365, SQL, Salesforce oder SFTP-Server gebunden. Verwaltete Connectors werden von Microsoft verwaltet und in Azure gehostet und erfordern normalerweise, dass Sie zuerst eine Verbindung in Ihrem Workflow erstellen und Ihre Identität authentifizieren. Es sind sowohl auf Serien als auch auf Webhooks basierende Trigger verfügbar. Wenn Sie einen Trigger verwenden, der auf einer Serie beruht, sollten Sie die [Übersicht über das Wiederholungsverhalten](apis-list.md#recurrence-behavior) lesen.

Für einige Dienste, Systeme und Protokolle wie Azure Service Bus, Azure Functions, SQL, Applicability Statement 2 (AS2) usw. bietet Logic Apps auch integrierte Versionen. Die Anzahl und der Bereich variieren abhängig davon, ob Sie eine Logik-App mit mehreren oder einem einzigen Mandanten erstellen. In einigen Fällen sind sowohl eine integrierte Version als auch eine verwaltete Connectorversion verfügbar. In den meisten Fällen bietet die integrierte Version eine höhere Leistung, bessere Funktionen, niedrigere Preise usw. Zum [Austauschen von B2B-Nachrichten mithilfe des AS2-Protokolls](../logic-apps/logic-apps-enterprise-integration-as2.md) sollten Sie beispielsweise die integrierte Version auswählen, es sei denn, Sie benötigen Nachverfolgungsfunktionen, die nur in der (veralteten) verwalteten Connectorversion verfügbar sind.

Einige verwaltete Connectors für Logic Apps gehören mehreren Unterkategorien an. Der SAP-Connector ist beispielsweise sowohl ein [Unternehmensconnector](#enterprise-connectors) als auch ein [lokaler Connector](#on-premises-connectors).

* [Standardconnectors](#standard-connectors) bieten Zugriff auf Dienste wie Azure Blob Storage, Office 365, SharePoint, Salesforce, Power BI, OneDrive und viele mehr.
* [Lokale Connectors](#on-premises-connectors) bieten Zugriff auf lokale Systeme wie SQL Server, SharePoint Server, SAP, Oracle DB, Dateifreigaben usw.
* [Integrationskonto-Connectors](#integration-account-connectors) ermöglichen Ihnen das Transformieren und Validieren von XML, Codieren und Decodieren von Flatfiles sowie das Verarbeiten von Business-to-Business-Nachrichten (B2B) mit AS2-, EDIFACT- und X12-Protokollen. 

## <a name="standard-connectors"></a>Standardconnectors

Azure Logic Apps stellt die folgenden gängigen Standardconnectors zum Erstellen automatisierter Workflows mit den unten genannten Diensten und Systemen bereit. Einige Standardconnectors unterstützen auch [lokale Systeme](#on-premises-connectors) oder [Integrationskonten](#integration-account-connectors).

Einige Standardconnectors für Logic Apps unterstützen auch [lokale Systeme](#on-premises-connectors) oder [Integrationskonten](#integration-account-connectors).

:::row:::
    :::column:::
        [![Symbol für den verwalteten Azure Service Bus-Connector in Logic Apps][azure-service-bus-icon]][azure-service-bus-doc]
        \
        \
        [**Azure Service Bus**][azure-service-bus-doc]
        \
        \
        Verwalten von asynchronen Nachrichten, Sitzungen und Themenabonnements mit dem am häufigsten verwendeten Connector in Logic Apps.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten SQL Server-Connector in Logic Apps][sql-server-icon]][sql-server-doc]
        \
        \
        [**SQL Server**][sql-server-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrer lokalen SQL Server-Instanz oder einer Azure SQL-Datenbank in der Cloud, sodass Sie Datensätze verwalten, gespeicherte Prozeduren ausführen oder Abfragen ausführen können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Azure Blob Storage-Connector in Logic Apps][azure-blob-storage-icon]][azure-blob-storage-doc]
        \
        \
        [**Azure Blog Storage**][azure-blob-storage-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, damit Sie Blobinhalte erstellen und verwalten können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Office 365 Outlook-Connector in Logic Apps][office-365-outlook-icon]][office-365-outlook-doc]
        \
        \
        [**Office 365 Outlook**][office-365-outlook-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrer Geschäfts-, Schul- oder Uni-E-Mail-Adresse, sodass Sie E-Mails, Aufgaben, Kalenderereignisse und Besprechungen, Kontakte, Anforderungen und mehr erstellen und verwalten können.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den verwalteten STFP-SSH-Connector in Logic Apps][sftp-ssh-icon]][sftp-ssh-doc]
        \
        \
        [**STFP-SSH**][sftp-ssh-doc]
        \
        \
        Herstellen einer Verbindung mit SFTP-Servern, auf die Sie über das Internet mittels SSH zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten SharePoint Online-Connector in Logic Apps][sharepoint-online-icon]][sharepoint-online-doc]
        \
        \
        [**SharePoint Online**][sharepoint-online-doc]
        \
        \
        Herstellen einer Verbindung mit SharePoint Online, sodass Sie Dateien, Anlagen, Ordner und mehr verwalten können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Azure-Warteschlangen-Connector in Logic Apps][azure-queues-icon]][azure-queues-doc]
        \
        \
        [**Azure-Warteschlangen**][azure-queues-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Warteschlangen und Nachrichten erstellen und verwalten können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten FTP-Connector in Logic Apps][ftp-icon]][ftp-doc]
        \
        \
        [**FTP**][ftp-doc]
        \
        \
        Herstellen einer Verbindung mit FTP-Servern, auf die Sie über das Internet zugreifen können, sodass Sie Ihre Dateien und Ordner verwenden können.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den verwalteten Dateisystem-Connector in Logic Apps][file-system-icon]][file-system-doc]
        \
        \
        [**Dateisystem**][file-system-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrer lokalen Dateifreigabe, sodass Sie Dateien erstellen und verwalten können.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Azure Event Hubs-Connector in Logic Apps][azure-event-hubs-icon]][azure-event-hubs-doc]
        \
        \
        [**Azure Event Hubs**][azure-event-hubs-doc]
        \
        \
        Nutzen und Veröffentlichen von Ereignissen über einen Event Hub. Sie können beispielsweise mithilfe von Event Hubs eine Ausgabe Ihrer Logik-App abrufen und die Ausgabe anschließend an einen Echtzeitanalyseanbieter senden.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Azure Event Grid-Connector in Logic Apps][azure-event-grid-icon]][azure-event-grid-doc]
        \
        \
        [**Azure Event Grid**][azure-event-grid-doc]
        \
        \
        Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z.B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen.
    :::column-end:::
    :::column:::
        [![Symbol für den verwalteten Salesforce-Connector in Logic Apps][salesforce-icon]][salesforce-doc]
        \
        \
        [**Salesforce**][salesforce-doc]
        \
        \
        Herstellen einer Verbindung mit Ihrem Salesforce-Konto, sodass Sie Elemente wie Datensätze, Aufträge, Objekte usw. erstellen und verwalten können.
    :::column-end:::
:::row-end:::


## <a name="on-premises-connectors"></a>Lokale Connectors

Damit Sie eine Verbindung mit einem lokalen System herstellen können, müssen Sie zunächst [ein lokales Datengateway herunterladen, installieren und einrichten][gateway-doc]. Dieses Gateway stellt einen sicheren Kommunikationskanal dar, ohne dass die erforderliche Netzwerkinfrastruktur eingerichtet werden muss. 

Im Folgenden sind einige häufig verwendete [Standardconnectors](#standard-connectors) aufgeführt, die Logic Apps für den Zugriff auf Daten und Ressourcen in lokalen Systemen bereitstellt. Die Liste der lokalen Connectors finden Sie unter [Unterstützte Datenquellen](../logic-apps/logic-apps-gateway-connection.md#supported-connections).

:::row:::
    :::column:::
        [![Symbol für den lokalen Biztalk Server-Connector in Logic Apps][biztalk-server-icon]][biztalk-server-doc]
        \
        \
        [**Biztalk Server**][biztalk-server-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen Dateisystem-Connector in Logic Apps][file-system-icon]][file-system-doc]
        \
        \
        [**Dateisystem**][file-system-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen IBM Db2-Connector in Logic Apps][ibm-db2-icon]][ibm-db2-doc]
        \
        \
        [**IBM Db2**][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen IBM Informix-Connector in Logic Apps][ibm-informix-icon]][ibm-informix-doc]
        \
        \
        [**IBM Informix**][ibm-informix-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den lokalen MySQL-Connector in Logic Apps][mysql-icon]][mysql-doc]
        \
        \
        [**MySQL**][mysql-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen Oracle DB-Connector in Logic Apps][oracle-db-icon]][oracle-db-doc]
        \
        \
        [**Oracle Database**][oracle-db-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen PostgreSQL-Connector in Logic Apps][postgre-sql-icon]][postgre-sql-doc]
        \
        \
        [**PostgreSQL**][postgre-sql-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen SharePoint Server-Connector in Logic Apps][sharepoint-server-icon]][sharepoint-server-doc]
        \
        \
        [**SharePoint Server**][sharepoint-server-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den lokalen SQL Server-Connector in Logic Apps][sql-server-icon]][sql-server-doc]
        \
        \
        [**SQL Server**][sql-server-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den lokalen Teradata-Connector in Logic Apps][teradata-icon]][teradata-doc]
        \
        \
        [**Teradata**][teradata-doc]
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="integration-account-connectors"></a>Integrationskonto-Connectors

Integrationskonto-Connectors unterstützen insbesondere [B2B-Kommunikationsszenarien (Business-to-Business)](../logic-apps/logic-apps-enterprise-integration-overview.md) in Azure Logic Apps. Nachdem Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erstellt und Ihre B2B-Artefakte wie Parteien, Vereinbarungen, Zuordnungen und Schemas definiert haben, können Sie Integrationskonto-Connectors verwenden, um Nachrichten zu codieren und zu decodieren, Inhalte zu transformieren usw.

Wenn Sie Microsoft BizTalk Server verwenden, können Sie beispielsweise mit dem [lokalen BizTalk Server-Connector](#on-premises-connectors) eine Verbindung mit Ihrem Workflow herstellen. Danach können Sie mithilfe der folgenden Integrationskonto-Connectors BizTalk-ähnliche Vorgänge in Ihrem Workflow erweitern oder durchführen.

> [!NOTE]
> Bevor Sie Integrationskonto-Connectors verwenden können, müssen Sie Ihre [Logik-App mit einem Integrationskonto verknüpfen](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).


:::row:::
    :::column:::
        [![Aktionssymbol „AS2-Decodierung“ in Logic Apps][as2-icon]][as2-doc]
        \
        \
        [**AS2-Decodierung**][as2-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „AS2-Codierung“ in Logic Apps][as2-icon]][as2-doc]
        \
        \
        [**AS2-Codierung**][as2-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „EDIFACT-Decodierung“ in Logic Apps][edifact-icon]][edifact-decode-doc]
        \
        \
        [**EDIFACT-Decodierung**][edifact-decode-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „EDIFACT-Codierung“ in Logic Apps][edifact-icon]][edifact-encode-doc]
        \
        \
        [**EDIFACT-Codierung**][edifact-encode-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Aktionssymbol „Flatfile-Decodierung“ in Logic Apps][flat-file-decode-icon]][flat-file-decode-doc]
        \
        \
        [**Flatfile-Decodierung**][flat-file-decode-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „Flatfile-Codierung“ in Logic Apps][flat-file-encode-icon]][flat-file-encode-doc]
        \
        \
        [**Flatfile-Codierung**][flat-file-encode-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „Integrationskonto“ in Logic Apps][integration-account-icon]][integration-account-doc]
        \
        \
        [**Integrationskonto**][integration-account-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „Liquid-Transformationen“ in Logic Apps][liquid-icon]][json-liquid-transform-doc]
        \
        \
        [**Liquid-Transformationen**][json-liquid-transform-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Aktionssymbol „X12-Decodierung“ in Logic Apps][x12-icon]][x12-decode-doc]
        \
        \
        [**X12-Decodierung**][x12-decode-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „X12-Codierung“ in Logic Apps][x12-icon]][x12-encode-doc]
        \
        \
        [**X12-Codierung**][x12-encode-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „XML-Transformationen“ in Logic Apps][xml-transform-icon]][xml-transform-doc]
        \
        \
        [**XML-Transformationen**][xml-transform-doc]
    :::column-end:::
    :::column:::
        [![Aktionssymbol „XML-Überprüfung“ in Logic Apps][xml-validate-icon]][xml-validate-doc]
        \
        \
        [**XML-Überprüfung**][xml-validate-doc]
    :::column-end:::
:::row-end:::

## <a name="enterprise-connectors"></a>Enterprise-Connectors

Die folgenden Connectors bieten gegen zusätzliche Gebühren Zugriff auf Unternehmenssysteme:

:::row:::
    :::column:::
        [![Symbol für den IBM 3270-Unternehmensconnector in Logic Apps][ibm-3270-icon]][ibm-3270-doc]
        \
        \
        [**IBM 3270**-Unternehmensconnector][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den IBM MQ-Unternehmensconnector in Logic Apps][ibm-mq-icon]][ibm-mq-doc]
        \
        \
        [**IBM MQ**-Unternehmensconnector][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den SAP-Unternehmensconnector in Logic Apps][sap-icon]][sap-connector-doc]
        \
        \
        [**SAP**-Unternehmensconnector][sap-connector-doc]
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::


## <a name="ise-connectors"></a>ISE-Connectors

In einer Integrationsdienstumgebung (Integration Service Environment, ISE) verfügen diese verwalteten Connectors auch über [ISE-Versionen](apis-list.md#ise-and-connectors), die andere Funktionen als die mehrinstanzenfähigen Versionen bereitstellen:

> [!NOTE]
> Logik-Apps, die in einer ISE ausgeführt werden, und ihre Connectors, unabhängig davon, wo diese Connectors ausgeführt werden, unterliegen einem festgelegten Tarif im Vergleich zum nutzungsbasierten Tarif. Weitere Informationen finden Sie im [Preismodell für Logik-Apps](../logic-apps/logic-apps-pricing.md) und unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

:::row:::
    :::column:::
        [![Symbol für den AS2-ISE-Connector in Logic Apps][as2-icon]][as2-doc]
        \
        \
        [**AS2**-SE][as2-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Automation-ISE-Connector in Logic Apps][azure-automation-icon]][azure-automation-doc]
        \
        \
        [**Azure Automation**-ISE][azure-automation-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Blob Storage-ISE-Connector in Logic Apps][azure-blob-storage-icon]][azure-blob-storage-doc]
        \
        \
        [**Azure Blob Storage**-ISE][azure-blob-storage-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Cosmos DB-ISE-Connector in Logic Apps][azure-cosmos-db-icon]][azure-cosmos-db-doc]
        \
        \
        [**Azure Cosmos DB**-ISE][azure-cosmos-db-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den Azure Event Hubs-ISE-Connector in Logic Apps][azure-event-hubs-icon]][azure-event-hubs-doc]
        \
        \
        [**Azure Event Hubs**-ISE][azure-event-hubs-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Event Grid-ISE-Connector in Logic Apps][azure-event-grid-icon]][azure-event-grid-doc]
        \
        \
        [**Azure Event Grid**-ISE][azure-event-grid-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure File Storage-ISE-Connector in Logic Apps][azure-file-storage-icon]][azure-file-storage-doc]
        \
        \
        [**Azure File Storage**-ISE][azure-file-storage-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Key Vault-ISE-Connector in Logic Apps][azure-key-vault-icon]][azure-key-vault-doc]
        \
        \
        [**Azure Key Vault**-ISE][azure-key-vault-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den Azure Monitor-Protokolle-ISE-Connector in Logic Apps][azure-monitor-logs-icon]][azure-monitor-logs-doc]
        \
        \
        [**Azure Monitor-Protokolle**-ISE][azure-monitor-logs-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Service Bus-ISE-Connector in Logic Apps][azure-service-bus-icon]][azure-service-bus-doc]
        \
        \
        [**Azure Service Bus**-ISE][azure-service-bus-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Synapse Analytics-ISE-Connector in Logic Apps][azure-sql-data-warehouse-icon]][azure-sql-data-warehouse-doc]
        \
        \
        [**Azure Synapse Analytics**-ISE][azure-sql-data-warehouse-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Azure Table Storage-ISE-Connector in Logic Apps][azure-table-storage-icon]][azure-table-storage-doc]
        \
        \
        [**Azure Table Storage**-ISE][azure-table-storage-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den Azure-Warteschlangen-ISE-Connector in Logic Apps][azure-queues-icon]][azure-queues-doc]
        \
        \
        [**Azure-Warteschlangen**-ISE][azure-queues-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den EDIFACT-ISE-Connector in Logic Apps][edifact-icon]][edifact-doc]
        \
        \
        [**EDIFACT**-ISE][edifact-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den Dateisystem-ISE-Connector in Logic Apps][file-system-icon]][file-system-doc]
        \
        \
        [**Dateisystem**-ISE][file-system-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den FTP-ISE-Connector in Logic Apps][ftp-icon]][ftp-doc]
        \
        \
        [**FTP**-ISE][ftp-doc]
    :::column-end:::
:::row-end:::   
:::row:::
    :::column:::
        [![Symbol für den IBM 3270-ISE-Connector in Logic Apps][ibm-3270-icon]][ibm-3270-doc]
        \
        \
        [**IBM 3270**-ISE][ibm-3270-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den IBM DB2-ISE-Connector in Logic Apps][ibm-db2-icon]][ibm-db2-doc]
        \
        \
        [**IBM DB2**-ISE][ibm-db2-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den IBM MQ-ISE-Connector in Logic Apps][ibm-mq-icon]][ibm-mq-doc]
        \
        \
        [**IBM MQ**-ISE][ibm-mq-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den SAP-ISE-Connector in Logic Apps][sap-icon]][sap-connector-doc]
        \
        \
        [**SAP**-ISE][sap-connector-doc]
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [![Symbol für den SFTP-SSH-ISE-Connector in Logic Apps][sftp-ssh-icon]][sftp-ssh-doc]
        \
        \
        [**SFTP-SSH**-ISE][sftp-ssh-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den SMTP-ISE-Connector in Logic Apps][smtp-icon]][smtp-doc]
        \
        \
        [**SMTP**-ISE][smtp-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den SQL Server-ISE-Connector in Logic Apps][sql-server-icon]][sql-server-doc]
        \
        \
        [**SQL Server**-ISE][sql-server-doc]
    :::column-end:::
    :::column:::
        [![Symbol für den X12-ISE-Connector in Logic Apps][x12-icon]][x12-doc]
        \
        \
        [**X12**-ISE][x12-doc]
    :::column-end:::
:::row-end:::

Weitere Informationen finden Sie in den folgenden Themen:

* [Zugriff auf virtuelle Azure-Netzwerkressourcen aus Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)
* [Logic Apps – Preismodell](../logic-apps/logic-apps-pricing.md)
* [Herstellen einer Verbindung mit virtuellen Azure-Netzwerken über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen benutzerdefinierter APIs, die über Azure Logic Apps aufgerufen werden können](/logic-apps/logic-apps-create-api-app)

<!--Managed connector icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-cosmos-db-icon]: ./media/apis-list/azure-cosmos-db.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-devops-icon]: ./media/apis-list/azure-devops.png
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
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
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
[mandrill-icon]: ./media/apis-list/mandrill.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
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
[twilio-icon]: ./media/apis-list/twilio.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[youtube-icon]: ./media/apis-list/youtube.png


<!--Managed connector doc links-->
[azure-automation-doc]: /connectors/azureautomation/ "Erstellen und Verwalten von Automatisierungsaufträgen für Ihre Cloud und lokale Infrastruktur"
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Verwalten von Dateien in Ihrem Blobcontainer mit Azure Blob Storage-Connector"
[azure-cosmos-db-doc]: /connectors/documentdb/ "Herstellen einer Verbindung mit Azure Cosmos DB, damit Sie auf Dokumente und gespeicherte Prozeduren zugreifen können"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md "Überwachen von Ereignissen, die von Event Grid veröffentlicht werden, z. B. bei Änderungen von Azure-Ressourcen oder Drittanbieterressourcen"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Herstellen einer Verbindung mit Azure Event Hubs, sodass Sie Ereignisse zwischen Logik-Apps und Event Hubs empfangen und senden können"
[azure-file-storage-doc]: /connectors/azurefile/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Dateien erstellen, aktualisieren, abrufen und löschen können"
[azure-key-vault-doc]: /connectors/keyvault/ "Herstellen einer Verbindung mit Ihrem Azure Key Vault, sodass Sie Ihre Geheimnisse und Schlüssel verwalten können"
[azure-monitor-logs-doc]: /connectors/azuremonitorlogs/ "Ausführen von Abfragen an Azure Monitor-Protokolle über Log Analytics-Arbeitsbereiche und Application Insights-Komponenten hinweg"
[azure-queues-doc]: /connectors/azurequeues/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Warteschlangen und Nachrichten erstellen und verwalten können"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Senden von Nachrichten aus Service Bus-Warteschlangen und -Themen und Empfangen von Nachrichten aus Service Bus-Warteschlangen und -Abonnements"
[azure-sql-data-warehouse-doc]: /connectors/sqldw/ "Herstellen einer Verbindung mit Azure Synapse Analytics, sodass Sie Ihre Daten anzeigen können"
[azure-table-storage-doc]: /connectors/azuretables/ "Herstellen einer Verbindung mit Ihrem Azure Storage-Konto, sodass Sie Dateien erstellen und aktualisieren, Tabellen abfragen und mehr können"
[biztalk-server-doc]: /connectors/biztalk/ "Herstellen einer Verbindung mit Ihrer BizTalk Server-Instanz, sodass Sie BizTalk-basierte Anwendungen parallel mit Azure Logic Apps ausführen können"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Herstellen einer Verbindung mit einem lokalen Dateisystem"
[ftp-doc]: ./connectors-create-api-ftp.md "Herstellen einer Verbindung mit einem FTP/FTPS-Server für FTP-Aufgaben, z.B. Hochladen, Abrufen, Löschen von Dateien usw."
[github-doc]: ./connectors-create-api-github.md "Herstellen einer Verbindung mit GitHub und Nachverfolgen von Problemen"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Herstellen einer Verbindung mit Google Calendar und zum Verwalten des Kalenders"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Herstellen einer Verbindung mit Google Sheets zur Bearbeitung Ihrer Tabellen"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Herstellen einer Verbindung mit Google Tasks zur Verwaltung Ihrer Aufgaben"
[ibm-3270-doc]: ./connectors-run-3270-apps-ibm-mainframe-create-api-3270.md "Herstellen einer Verbindung mit 3270-Apps auf IBM-Mainframes"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Stellen Sie eine Verbindung mit IBM DB2 in der Cloud oder lokal her. Sie können eine Zeile aktualisieren, eine Tabelle abrufen und vieles mehr."
[ibm-informix-doc]: ./connectors-create-api-informix.md "Stellen Sie eine Verbindung mit Informix in der Cloud oder lokal her. Sie können eine Zeile lesen, die Tabellen auflisten und vieles mehr."
[ibm-mq-doc]: ./connectors-create-api-mq.md "Herstellen einer Verbindung mit IBM MQ (lokal oder in Azure) und Senden und Empfangen von Nachrichten"
[instagram-doc]: ./connectors-create-api-instagram.md "Stellen Sie eine Verbindung mit Instagram her. Lösen Sie Ereignisse aus, oder reagieren Sie darauf."
[mandrill-doc]: ./connectors-create-api-mandrill.md "Herstellen einer Verbindung mit Mandrill zur Kommunikation"
[mysql-doc]: /connectors/mysql/ "Herstellen einer Verbindung mit Ihrer lokalen MySQL-Datenbank, sodass Sie Daten lesen und schreiben können"
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Herstellen einer Verbindung mit Ihrem Geschäfts-, Schul- oder Unikonto, sodass Sie E-Mails senden und empfangen sowie Ihren Kalender und Ihre Kontakte verwalten und mehr können"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Herstellen einer Verbindung mit Ihrem persönlichen Microsoft OneDrive-Ordner, sodass Sie Dateien hochladen, löschen, auflisten und mehr können"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Herstellen einer Verbindung mit Ihrem geschäftlichen Microsoft OneDrive-Ordner, sodass Sie Ihre Dateien hochladen, löschen, auflisten und mehr können"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Herstellen einer Verbindung mit einer Oracle-Datenbank, sodass Sie Zeilen hinzufügen, einfügen, löschen und mehr können"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Herstellen einer Verbindung mit Ihrem Outlook-Postfach, sodass Sie Ihre E-Mails, Kalender, Kontakte und mehr verwalten können"
[postgre-sql-doc]: /connectors/postgresql/ "Herstellen einer Verbindung mit Ihrer PostgreSQL-Datenbank, sodass Sie Daten aus Tabellen lesen können"
[salesforce-doc]: ./connectors-create-api-salesforce.md "Stellen Sie eine Verbindung mit Ihrem Salesforce-Konto her. Sie können Konten, Leads, Vertriebschancen usw. verwalten."
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Herstellen einer Verbindung mit einem lokalen SAP-System"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Stellen Sie eine Verbindung mit SendGrid her. Sie können E-Mails senden und die Liste der Empfänger verwalten."
[sftp-ssh-doc]: ./connectors-sftp-ssh.md "Herstellen einer Verbindung mit Ihrem SFTP-Konto mittels SSH. Sie können Dateien hochladen, abrufen, löschen und vieles mehr."
[sharepoint-server-doc]: ./connectors-create-api-sharepoint.md "Stellen Sie eine Verbindung mit dem lokalen SharePoint-Server her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[sharepoint-online-doc]: ./connectors-create-api-sharepoint.md "Stellen Sie eine Verbindung mit SharePoint Online her. Sie können Dokumente verwalten, Elemente auflisten und vieles mehr."
[slack-doc]: ./connectors-create-api-slack.md "Herstellen einer Verbindung mit Slack zum Veröffentlichen von Nachrichten in Slack-Kanälen"
[smtp-doc]: ./connectors-create-api-smtp.md "Herstellen einer Verbindung mit einem SMTP-Server und Senden von E-Mails mit Anhängen"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Herstellen einer Verbindung mit SparkPost zur Kommunikation"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Stellen Sie eine Verbindung mit Azure SQL-Datenbank oder SQL Server her. Erstellten, Aktualisieren, Abrufen und Löschen von Einträgen in einer SQL-Datenbanktabelle"
[teradata-doc]: /connectors/teradata/ "Herstellen einer Verbindung mit Ihrer Teradata-Datenbank zum Lesen von Daten aus Tabellen"
[twilio-doc]: ./connectors-create-api-twilio.md "Stellen Sie eine Verbindung mit Twilio her. Sie können Nachrichten senden und empfangen, verfügbare Nummern abrufen, eingehende Telefonnummern verwalten und vieles mehr."
[youtube-doc]: ./connectors-create-api-youtube.md "Stellen Sie eine Verbindung mit YouTube her. Sie können Ihre Videos und Kanäle verwalten."

<!--Integration account connector icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png

<!-- Integration account connector docs -->

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


<!--Other doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Herstellen einer Verbindung mit lokalen Datenquellen über Logik-Apps mit dem lokalen Datengateway"



<!--Integration account connector icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png

<!-- Integration account connector docs -->

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
