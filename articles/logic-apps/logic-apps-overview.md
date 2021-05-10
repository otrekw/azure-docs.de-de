---
title: Übersicht über Azure Logic Apps
description: Bei Logic Apps handelt es sich um eine Cloudplattform für die Erstellung automatisierter Workflows zur Integration von Apps, Daten, Diensten und Systemen für Unternehmensszenarien mit minimalem Code.
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: overview
ms.custom: mvc
ms.date: 04/26/2021
ms.openlocfilehash: 377abf4809e01c3931de393ff97718e0638c5c36
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108065497"
---
# <a name="what-is-azure-logic-apps"></a>Was ist Azure Logic Apps?

[Logic Apps](https://azure.microsoft.com/services/logic-apps) ist eine cloudbasierte Plattform, die Ihnen dabei hilft, Apps, Daten, Systeme und Dienste durch die Erstellung und Ausführung automatisierter [Workflows](#logic-app-concepts) schnell und einfach zu integrieren. Als Teil von [Azure Integration Services](https://azure.microsoft.com/product-categories/integration/) bietet Logic Apps eine einfachere Möglichkeit zum Erstellen, Hosten und Verwalten hochgradig skalierbarer Integrationslösungen für Unternehmens- und B2B-Szenarien (Business-to-Business) in der Cloud, lokal und in Hybridumgebungen.

Die folgende Liste enthält lediglich einige Beispielaufgaben, Geschäftsprozesse und Workloads, die Sie mit dem Logic Apps-Dienst automatisieren können:

* Weiterleiten und Verarbeiten von Kundenbestellungen in lokalen Systemen und Clouddiensten
* Planen und Senden von E-Mail-Benachrichtigungen mit Office 365, wenn ein bestimmtes Ereignis eintritt
* Verschieben von hochgeladenen Dateien von einem SFTP- oder FTP-Server in Azure Storage
* Überwachen von Tweets, Analysieren der Stimmung und Erstellen von Warnungen oder Aufgaben für zu prüfende Elemente

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Introducing-Azure-Logic-Apps/player]

Um einen sicheren Zugriff zu gewährleisten und Vorgänge in Echtzeit für verschiedene Datenquellen ausführen zu können, steht ein [ständig wachsender Katalog](/connectors/connector-reference/connector-reference-logicapps-connectors) mit [von Microsoft verwalteten Connectors](#logic-app-concepts) zur Verfügung. Das Angebot umfasst unter anderem Folgendes:

* Azure-Dienste wie Blob Storage und Service Bus
* Office-Dienste wie Outlook, Excel und SharePoint
* Datenbankserver wie SQL und Oracle
* Unternehmenssysteme wie SAP und IBM MQ
* Dateifreigaben wie FTP und SFTP

Sie können [integrierte Trigger und Aktionen](#logic-app-concepts) verwenden, die nativ innerhalb des Logic Apps-Diensts ausgeführt werden, um mit einem beliebigen Dienstendpunkt zu kommunizieren, Ihren eigenen Code auszuführen, Ihren Workflow zu strukturieren oder Daten zu bearbeiten. Zu den integrierten Triggern zählen beispielsweise „Anforderung“, „HTTP“ und „Serie“. Zu den integrierten Aktionen zählen „Bedingung“, „For each“, „JavaScript-Code ausführen“ und Vorgänge, die in Azure gehostete Azure-Funktionen, Web-Apps oder API-Apps aufrufen, sowie andere Logic Apps-Workflows.

Für B2B-Integrationsszenarien enthält Logic Apps Funktionen von [BizTalk Server](/biztalk/core/introducing-biztalk-server). Sie können ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md) erstellen, in dem Sie Handelspartner, Vereinbarungen, Schemas, Zuordnungen und andere B2B-Artefakte definieren. Wenn Sie dieses Konto mit einer Logik-App verknüpfen, können Sie Workflows erstellen, die diese Artefakte verwenden und Nachrichten über Protokolle wie AS2, EDIFACT und X12 austauschen.

Weitere Informationen dazu, wie Workflows auf Apps, Daten, Dienste und Systeme zugreifen und mit ihnen arbeiten können, finden Sie in der folgenden Dokumentation:

* [Connectors für Azure Logic Apps](../connectors/apis-list.md)
* [Integrierte Trigger und Aktionen für Azure Logic Apps](../connectors/built-in.md)
* [Verwaltete Connectors für Azure Logic Apps](../connectors/managed.md)
* [B2B-Unternehmensintegrationslösungen mit Azure Logic Apps und dem Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)

<a name="logic-app-concepts"></a>

## <a name="key-terms"></a>Schlüsselbegriffe

* **Workflow:** Eine Reihe von Schritten zum Definieren einer Aufgabe oder eines Prozesses – beginnend mit einem einzelnen Trigger und gefolgt von mindestens einer Aktion.

* **Trigger:** Der erste Schritt, durch den jeder Workflow gestartet wird. Gibt die Bedingung an, die erfüllt sein muss, damit Aktionen im Workflow ausgeführt werden. Ein Triggerereignis kann beispielsweise der Eingang einer E-Mail in Ihrem Posteingang oder die Erkennung einer neuen Datei in einem Speicherkonto sein.

* **Aktion:** Jeder weitere Schritt, der nach dem Trigger folgt und einen Vorgang in einem Workflow ausführt.

* **Verwalteter Connector:** Ein von Microsoft verwalteter Connector für den Zugriff auf eine bestimmte App bzw. auf Daten, einen Dienst oder ein System. Bei den meisten verwalteten Connectors muss zunächst eine Verbindung in Ihrem Workflow erstellt und Ihre Identität authentifiziert werden, um den jeweiligen Connector verwenden zu können.

  Sie können beispielsweise einen Workflow mit einem Trigger starten oder eine Aktion für Azure Blob Storage, Office 365, Salesforce oder SFTP-Server einschließen. Weitere Informationen finden Sie unter [Verwaltete Connectors für Azure Logic Apps](../connectors/managed.md).

* **Integrierter Trigger oder integrierte Aktion:** Ein nativ ausgeführter Logic Apps-Vorgang, der es ermöglicht, den Zeitplan oder die Struktur Ihres Workflows zu steuern, Ihren eigenen Code auszuführen, Daten zu verwalten oder zu bearbeiten oder andere Aufgaben in Ihrem Workflow auszuführen. Die meisten integrierten Vorgänge sind keinem Dienst oder System zugeordnet. Bei vielen ist es auch nicht erforderlich, zuerst eine Verbindung in Ihrem Workflow zu erstellen und Ihre Identität zu authentifizieren. Integrierte Vorgänge stehen auch für einige Dienste, Systeme und Protokolle zur Verfügung – etwa für Azure Functions, Azure API Management und Azure-App Services.

  Mithilfe des Triggers „Serie“ lässt sich beispielsweise fast jeder Workflow basierend auf einem Zeitplan starten. Sie können aber auch den Trigger „Anforderung“ verwenden, damit Ihr Workflow erst gestartet wird, wenn er aufgerufen wird. Weitere Informationen finden Sie unter [Integrierte Trigger und Aktionen für Azure Logic Apps](../connectors/built-in.md).

<a name="how-do-logic-apps-work"></a>

## <a name="how-logic-apps-work"></a>Funktionsweise von Logik-Apps

In einer Logik-App beginnt jeder Workflow immer mit einem einzelnen [Trigger](#logic-app-concepts). Ein Trigger wird ausgelöst, wenn eine Bedingung erfüllt ist – also beispielsweise, wenn ein bestimmtes Ereignis eintritt oder wenn Daten bestimmte Kriterien erfüllen. Viele Trigger verfügen über [Planungsfunktionen](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md), mit denen die Häufigkeit der Workflowausführung gesteuert wird. Auf den Trigger folgt mindestens eine [Aktion](#logic-app-concepts) zum Ausführen von Vorgängen, durch die beispielsweise Daten, die den Workflow durchlaufen, verarbeitet, behandelt oder konvertiert werden oder die den nächsten Schritt des Workflows einleiten.

Der folgende Workflow beginnt beispielsweise mit einem Dynamics-Trigger, der über die integrierte Bedingung **Wenn ein Datensatz aktualisiert wird** verfügt. Die Aktionen umfassen das Transformieren von XML, das Aufrufen einer Web-App, die Daten aktualisiert, das Auswerten einer Bedingung, die steuert, welche Aktionen ausgeführt werden sollen, und das Senden einer E-Mail-Benachrichtigung mit den Ergebnissen. Wenn der Trigger ein Ereignis erkennt, das die Bedingung erfüllt, wird der Trigger ausgelöst, und die Aktionen im Workflow werden gestartet. Bei jeder Auslösung des Triggers wird durch den Logic Apps-Dienst eine Workflowinstanz zum Ausführen der Aktionen erstellt.

![Designer für Logik-Apps: Beispielworkflow](./media/logic-apps-overview/azure-logic-apps-designer.png)

Workflows können visuell mithilfe des Designers für Logik-Apps im Azure-Portal, in Visual Studio oder in Visual Studio Code erstellt werden. Jeder Workflow verfügt auch über eine zugrunde liegende Definition, die mithilfe von JavaScript Object Notation (JSON) beschrieben wird. Auf Wunsch können Sie Workflows also auch über den Codeansichts-Editor im Azure-Portal, in Visual Studio oder in Visual Studio Code erstellen und anpassen. Für einige Erstellungs- und Verwaltungsaufgaben werden von Logic Apps auch Azure PowerShell- und Azure CLI-Befehle unterstützt. Für die automatisierte Bereitstellung werden von Logic Apps Azure Resource Manager-Vorlagen unterstützt.

## <a name="why-use-logic-apps"></a>Gründe für die Verwendung von Logik-Apps

Unternehmen setzen immer stärker auf Digitalisierung. Mithilfe der Logic Apps-Plattform können Sie sowohl Legacysysteme als auch hochmoderne Systeme einfacher und schneller miteinander verbinden, indem Sie vordefinierte APIs als von Microsoft verwaltete Connectors angeben. So können Sie sich auf die Geschäftslogik und Funktionalität Ihrer Apps konzentrieren. Sie müssen sich nicht mit dem Erstellen, Hosten, Skalieren, Verwalten, Pflegen und Überwachen Ihrer Apps beschäftigen. Diese Aufgaben übernimmt Logic Apps für Sie. Außerdem zahlen Sie beim nutzungsabhängigen [Preismodell](../logic-apps/logic-apps-pricing.md) nur für die tatsächlich verwendeten Ressourcen.

Normalerweise müssen Sie keinerlei Code schreiben. Falls aber doch, können Sie Codeausschnitte mit [Azure Functions](../azure-functions/functions-overview.md) erstellen und diesen Code bei Bedarf über Workflows ausführen. Wenn Ihre Workflows mit Ereignissen von Azure-Diensten, benutzerdefinierten Apps oder anderen Lösungen interagieren müssen, können Sie [Azure Event Grid](../event-grid/overview.md) mit Ihren Workflows für die Überwachung, das Routing und die Veröffentlichung nutzen.

Logic Apps, Functions und Event Grid werden von Microsoft Azure vollständig verwaltet, sodass Sie sich keine Gedanken über das Erstellen, Hosten, Skalieren, Verwalten, Überwachen und Pflegen Ihrer Lösungen machen müssen. Dank der Funktion zum Erstellen von [„serverlosen“ Apps und Lösungen](../logic-apps/logic-apps-serverless-overview.md) können Sie sich auf die Geschäftslogik konzentrieren. Für diese Dienste wird eine automatische Skalierung durchgeführt, um Ihre Anforderungen zu erfüllen, Integrationen zu beschleunigen und Sie beim Erstellen von stabilen Cloud-Apps mit minimalem Codeaufwand zu unterstützen.

Lesen Sie diese [Kundenberichte](https://aka.ms/logic-apps-customer-stories), um zu erfahren, wie Unternehmen ihre Flexibilität verbessert haben und sich stärker auf ihr Kerngeschäft konzentrieren können, indem sie Logic Apps mit anderen Azure-Diensten und Microsoft-Produkten kombiniert haben.

Hier sind weitere Details zu den Funktionen und Vorteilen von Logic Apps angegeben:

### <a name="visually-build-workflows-with-easy-to-use-tools"></a>Visuelles Erstellen von Workflows mit benutzerfreundlichen Tools

Sparen Sie Zeit, und vereinfachen Sie komplexe Prozesse, indem Sie Tools für das visuelle Entwerfen verwenden. Erstellen Sie Workflows von Anfang bis Ende, indem Sie den Designer für Logik-Apps über Ihren Browser im Azure-Portal oder in Visual Studio verwenden. Starten Sie Ihren Workflow mit einem Trigger, und fügen Sie eine beliebige Anzahl von Aktionen über den [Katalog mit Connectors](../connectors/apis-list.md) hinzu.

### <a name="get-started-faster-with-logic-apps-templates"></a>Schnellerer Einstieg mit Logic Apps-Vorlagen

Erstellen Sie häufig genutzte Lösungen in kürzerer Zeit, indem Sie vordefinierte Workflows aus dem [Vorlagenkatalog](../logic-apps/logic-apps-create-logic-apps-from-templates.md) auswählen. Die Arten von Vorlagen reichen von einfacher Konnektivität für SaaS-Apps (Software-as-a-Service) bis zu erweiterten B2B-Lösungen und „Just-for-fun“-Vorlagen. Informieren Sie sich über das [Erstellen von Logik-App-Workflows aus vorgefertigten Vorlagen](../logic-apps/logic-apps-create-logic-apps-from-templates.md).

### <a name="connect-disparate-systems-across-different-environments"></a>Übergreifendes Verbinden von verschiedenartigen Systemen für mehrere Umgebungen

Einige Muster und Prozesse sind leicht zu beschreiben, lassen sich im Code aber nur schwer implementieren. Mit Logic Apps-Workflows können Sie verschiedenartige Systeme in lokalen und cloudbasierten Umgebungen nahtlos miteinander verbinden. Beispielsweise können Sie eine Cloudmarketinglösung mit einem lokalen Abrechnungssystem verbinden oder das Messaging für APIs und Systeme per Enterprise Service Bus zentralisieren. Die Logic Apps-Plattform ist eine schnelle, zuverlässige und konsistente Lösung für die Bereitstellung wiederverwendbarer und rekonfigurierbarer Lösungen für diese Szenarien.

### <a name="first-class-support-for-enterprise-integration-and-b2b-scenarios"></a>Erstklassige Unterstützung für Unternehmensintegration und B2B-Szenarien

Unternehmen und Organisationen kommunizieren auf elektronischem Wege über Nachrichtenprotokolle und -formate miteinander, die zwar alle dem Branchenstandard entsprechen, aber unterschiedlicher Art sein können, z.B. EDIFACT, AS2 und X12. Mit den Features im [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md) können Sie Workflows erstellen, mit denen von Ihren Partnern verwendete Nachrichtenformate in Formate transformiert werden, die von den Systemen Ihrer Organisation interpretiert und verarbeitet werden können. Mit Logic Apps wird dieser Nachrichtenaustausch reibungslos und außerdem sicher durchgeführt, indem Verschlüsselungen und digitale Signaturen verwendet werden.

Beginnen Sie klein mit Ihren aktuellen Systemen und Diensten, und wachsen Sie Schritt für Schritt so schnell, wie es für Sie machbar ist. Wenn Sie bereit sind, erhalten Sie über Logic Apps und das EIP Unterstützung beim Implementieren und Hochskalieren auf anspruchsvollere Integrationsszenarien, indem diese und weitere Funktionen genutzt werden:

* Nutzen Sie die folgenden Produkte und Dienste:

  * [Microsoft BizTalk Server](/biztalk/core/introducing-biztalk-server)
  * [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)
  * [Azure-Funktionen](../azure-functions/functions-overview.md)
  * [Azure API Management](../api-management/api-management-key-concepts.md)

* Verarbeiten von [XML-Nachrichten](../logic-apps/logic-apps-enterprise-integration-xml.md)

* Verarbeiten von [Flatfiles](../logic-apps/logic-apps-enterprise-integration-flatfile.md)

* Austauschen von Nachrichten mit den Protokollen [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md), [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md) und [X12](../logic-apps/logic-apps-enterprise-integration-x12.md)

* Speichern und verwalten Sie diese und weitere B2B-Artefakte zentral mithilfe von [Integrationskonten](./logic-apps-enterprise-integration-create-integration-account.md):

  * [Partner](../logic-apps/logic-apps-enterprise-integration-partners.md)
  * [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md) 
  * [Zuordnungen für die XML-Datentransformation](../logic-apps/logic-apps-enterprise-integration-maps.md)
  * [Schemas für die XML-Überprüfung](../logic-apps/logic-apps-enterprise-integration-schemas.md)

Wenn Sie beispielsweise Microsoft BizTalk Server nutzen, können Logik-Apps über den [BizTalk Server-Connector](../connectors/managed.md#on-premises-connectors) mit Ihrer BizTalk Server-Instanz kommunizieren. Sie können dann BizTalk-ähnliche Vorgänge in Ihren Workflows erweitern oder ausführen, indem Sie [Connectors für Integrationskonten](../connectors/managed.md#integration-account-connectors) einfügen, die über das Enterprise Integration Pack verfügbar sind.

In der anderen Richtung kann BizTalk Server eine Verbindung mit Logik-Apps herstellen und damit kommunizieren, indem der [Microsoft BizTalk Server Adapter für Logic Apps](https://www.microsoft.com/download/details.aspx?id=54287) verwendet wird. Informieren Sie sich darüber, wie Sie für Ihre BizTalk Server-Instanz den [BizTalk Server Adapter einrichten und nutzen](/biztalk/core/logic-app-adapter).

### <a name="write-once-reuse-often"></a>Einmal schreiben, häufig wiederverwenden

Erstellen Sie Ihre Logik-Apps als Azure Resource Manager-Vorlagen, sodass Sie die [Bereitstellung von Logik-Apps](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) über mehrere Umgebungen und Regionen hinweg automatisieren können.

### <a name="access-resources-inside-azure-virtual-networks"></a>Zugreifen auf Ressourcen in virtuellen Azure-Netzwerken

Logik-Apps können auf geschützte Ressourcen zugreifen, z. B. virtuelle Computer (VMs) und andere Systeme oder Dienste, die sich in einem [virtuellen Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) befinden, wenn Sie eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen. Eine ISE ist eine dedizierte Instanz des Logic Apps-Diensts, die dedizierte Ressourcen verwendet und getrennt vom „globalen“ mehrinstanzenfähigen Logic Apps-Dienst ausgeführt wird.

Das Ausführen von Logik-Apps als eigene separate dedizierte Instanz trägt dazu bei, mögliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. Dies ist auch als [„Noisy Neighbors“-Problem](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors) bekannt. Eine ISE bietet darüber hinaus die folgenden Vorteile:

* Ihre eigenen statischen IP-Adressen. Diese sind gesondert von den statischen IP-Adressen, die von den Logik-Apps im mehrinstanzenfähigen Dienst gemeinsam verwendet werden. Sie können auch eine einzelne öffentliche, statische und vorhersagbare ausgehende IP-Adresse für die Kommunikation mit Zielsystemen einrichten. Auf diese Weise müssen Sie nicht für jede ISE an den Zielsystemen extra Firewallzugänge einrichten.

* Höhere Grenzwerte für Ausführungsdauer, Speicheraufbewahrung, Durchsatz, Zeitlimits für HTTP-Anforderungen und -Antworten, Nachrichtengröße und benutzerdefinierte Connectoranforderungen. Weitere Informationen finden Sie unter [Grenzwerte und Konfiguration für Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md).

Wenn Sie eine ISE erstellen, *injiziert* Azure diese ISE bzw. stellt sie in Ihrem virtuellen Azure-Netzwerk bereit. Anschließend können Sie diese ISE als Speicherort für Logik-Apps und Integrationskonten verwenden, die Zugriff benötigen. Weitere Informationen zum Erstellen einer ISE finden Sie unter [Verbinden mit Azure Virtual Networks über Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

### <a name="built-in-extensibility"></a>Integrierte Erweiterbarkeit

Wenn Sie den gewünschten Connector zum Ausführen von benutzerdefiniertem Code nicht finden, können Sie Logik-Apps erweitern, indem Sie Ihre eigenen Codeausschnitte bedarfsabhängig mit [Azure Functions](../azure-functions/functions-overview.md) erstellen und aufrufen. Erstellen Sie Ihre eigenen [APIs](../logic-apps/logic-apps-create-api-app.md) und [benutzerdefinierten Connectors](../logic-apps/custom-connector-overview.md), die Sie aus Logik-Apps aufrufen können.

### <a name="pay-only-for-what-you-use"></a>Zahlen nur für wirklich genutzte Ressourcen
  
Für Logic Apps werden nutzungsabhängige [Preise und Verbrauchseinheiten](../logic-apps/logic-apps-pricing.md) verwendet, sofern Sie nicht über Logik-Apps verfügen, die zuvor mit App Service-Plänen erstellt wurden.

Informieren Sie sich in diesen Einführungsvideos eingehender über Logic Apps:

* [Integration with Logic Apps – Go from zero to hero](https://channel9.msdn.com/Events/Build/2017/C9R17) (Integration in Logic Apps – Von „Zero“ zu „Hero“)
* [Enterprise integration with Microsoft Azure Logic Apps](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK2188) (Unternehmensintegration mit Microsoft Azure Logic Apps)
* [Building advanced business processes with Logic Apps](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3179) (Erstellen von erweiterten Geschäftsprozessen mit Logic Apps)

## <a name="how-does-logic-apps-differ-from-functions-webjobs-and-power-automate"></a>Wie unterscheidet sich Logic Apps von Functions, WebJobs und Power Automate?

All diese Dienste dienen Ihnen als Hilfe beim Verknüpfen bzw. Verbinden von verschiedenartigen Systemen. Jeder Dienst hat bestimmte Vorteile, sodass eine Kombination der jeweiligen Funktionen die beste Möglichkeit darstellt, schnell ein skalierbares, Integrationssystem mit vollem Funktionsumfang zu erstellen. Weitere Informationen finden Sie unter [Wählen zwischen Logic Apps, Functions, WebJobs und Power Automate](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md).

## <a name="get-started"></a>Erste Schritte

Logic Apps ist einer der vielen Dienste, die unter Microsoft Azure gehostet werden. Bevor Sie beginnen können, benötigen Sie also ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

Wenn Sie über ein Azure-Abonnement verfügen, können Sie [diese Schnellstartanleitung zum Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) ausprobieren. Darin wird beschrieben, wie damit neuer Inhalt auf einer Website per RSS-Feed überwacht und beim Erscheinen von neuem Inhalt E-Mails gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Überprüfen des Datenverkehrs mit einer zeitplanbasierten Logik-App](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
* Weitere Informationen zu [serverlosen Lösungen mit Azure](../logic-apps/logic-apps-serverless-overview.md)
* Weitere Informationen zur [B2B-Integration mit dem Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
