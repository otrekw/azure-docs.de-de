---
title: B2B-Unternehmensintegration
description: Erfahren Sie mehr über das Erstellen automatisierter B2B-Workflows für die Unternehmensintegration mit Azure Logic Apps und dem Enterprise Integration Pack
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, logicappspm
ms.topic: overview
ms.date: 05/11/2021
ms.openlocfilehash: 08de00415229b410e7b88bc4cae26754c466baef
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109789081"
---
# <a name="b2b-enterprise-integration-solutions-with-azure-logic-apps-and-enterprise-integration-pack"></a>B2B-Unternehmensintegrationslösungen mit Azure Logic Apps und dem Enterprise Integration Pack

Für Business-to-Business-Lösungen (B2B) und eine nahtlose Kommunikation zwischen Organisationen können Sie mithilfe des Enterprise Integration Pack (EIP) mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) automatisierte skalierbare Unternehmensintegrationsworkflows erstellen. Organisationen können Nachrichten elektronisch austauschen, auch wenn sie unterschiedliche Protokolle und Formate verwenden. Das EIP wandelt verschiedene Formate in ein Format um, das von den Systemen Ihrer Organisation verarbeitet werden kann, und unterstützt branchenübliche Protokolle wie [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md) und [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md). Sie können die Sicherheit von Nachrichten auch mithilfe von Verschlüsselung und digitalen Signaturen erhöhen. Das EIP unterstützt diese [Unternehmensintegrationsconnectors](../connectors/managed.md#enterprise-connectors) und diese Branchenstandards:

* Electronic Data Interchange (EDI)
* Enterprise Application Integration (EAI)

Wenn Sie mit Microsoft BizTalk Server oder Azure BizTalk Services vertraut sind, sind die Features leicht zu verwenden, da das EIP ähnliche Konzepte befolgt. Ein wesentlicher Unterschied besteht jedoch darin, dass die Architektur des EIP auf Integrationskonten basiert, um die Speicherung und Verwaltung von Artefakten zu vereinfachen, die bei der B2B-Kommunikation verwendet werden. Diese Konten sind cloudbasierte Container, in denen alle Artefakte wie Partner, Vereinbarungen, Schemas, Zuordnungen und Zertifikate gespeichert werden.

## <a name="why-use-the-enterprise-integration-pack"></a>Gründe für die Verwendung des Enterprise Integration Pack

* Mit dem EIP können Sie alle Ihre Artefakte zentral in Ihrem Integrationskonto speichern.

* Sie können B2B-Workflows erstellen und in Software-as-a-Service-Apps (SaaS) von Drittanbietern, lokalen Apps und benutzerdefinierten Apps integrieren, indem Sie Azure Logic Apps und Connectors nutzen.

* Mit Azure Functions können Sie benutzerdefinierten Code für Ihre Logik-Apps erstellen.

## <a name="how-do-i-get-started"></a>Wie fange ich an?

Bevor Sie mit dem EIP mit dem Entwickeln von B2B-Logik-App-Workflows beginnen, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) mit den Artefakten, die Sie verwenden möchten

* Zum Erstellen von Zuordnungen und Schemas können Sie die [Erweiterung „Microsoft Azure Logic Apps Enterprise Integration Tools“](https://aka.ms/vsenterpriseintegrationtools) und Visual Studio 2019 verwenden. 

   > [!IMPORTANT]
   > Installieren Sie diese Erweiterung nicht zusammen mit der BizTalk Server-Erweiterung. Sind beide Erweiterungen installiert, kann dies zu unerwartetem Verhalten führen. Stellen Sie sicher, dass nur eine dieser Erweiterungen installiert ist.

   > [!NOTE]
   > Auf Monitoren mit hoher Auflösung kann ein [Anzeigeproblem des Zuordnungs-Designers](/visualstudio/designers/disable-dpi-awareness) in Visual Studio auftreten. Um dieses Anzeigeproblem zu beheben, [starten Sie Visual Studio im nicht DPI-fähigen Modus neu](/visualstudio/designers/disable-dpi-awareness#restart-visual-studio-as-a-dpi-unaware-process), oder fügen Sie den [DPIUNAWARE-Registrierungswert](/visualstudio/designers/disable-dpi-awareness#add-a-registry-entry) hinzu.


* Bei Verwendung von Visual Studio 2015 können Sie die Erweiterung [Microsoft Azure Logic Apps Enterprise Integration Tools für Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas) verwenden.

Wenn Sie ein Integrationskonto erstellt und Ihre Artefakte hinzugefügt haben, können Sie mithilfe dieser Artefakte B2B-Workflows erstellen, indem Sie eine Logik-App im Azure-Portal erstellen. Wenn Sie mit Logik-Apps noch nicht vertraut sind, [erstellen Sie zunächst eine einfache Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Damit Sie diese Artefakte verwenden können, müssen Sie zunächst Ihr Integrationskonto mit Ihrer Logik-App verknüpfen. Anschließend kann Ihre Logik-App auf Ihr Integrationskonto zugreifen. Sie können Logik-Apps auch mithilfe von Visual Studio oder [PowerShell](/powershell/module/az.logicapp) erstellen, verwalten und bereitstellen.

Im Folgenden finden Sie allgemeine Schritte, um mit dem Erstellen von B2B-Logik-Apps zu beginnen:

![Voraussetzungen für das Erstellen von B2B-Logik-Apps](./media/logic-apps-enterprise-integration-overview/overview.png)

## <a name="try-now"></a>Jetzt testen

[Bereitstellen einer voll funktionsfähigen Beispiel-Logik-App, die AS2-Nachrichten sendet und empfängt](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.logic/logic-app-as2-send-receive)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Handelspartnern](logic-apps-enterprise-integration-partners.md)
* [Erstellen von Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md)
* [Hinzufügen von Schemas](logic-apps-enterprise-integration-schemas.md)
* [Hinzufügen von Zuordnungen](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [Migrieren von BizTalk Services](../logic-apps/logic-apps-move-from-mabs.md)
