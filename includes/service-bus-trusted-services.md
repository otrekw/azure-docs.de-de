---
title: include file
description: include file
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 03/08/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd4d5de5d93e4aea862aaabd10fb5d3c6d45cb1c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102511016"
---
## <a name="trusted-microsoft-services"></a>Vertrauenswürdige Microsoft-Dienste
Wenn Sie die Einstellung **Vertrauenswürdigen Microsoft-Diensten die Umgehung dieser Firewall erlauben** aktivieren, wird den folgenden Diensten Zugriff auf Ihre Service Bus-Ressourcen gewährt.

| Vertrauenswürdiger Dienst | Unterstützte Verwendungsszenarien | 
| --------------- | ------------------------- | 
| Azure Event Grid | Ermöglicht Azure Event Grid das Senden von Ereignissen an Warteschlangen oder Themen in Ihrem Service Bus-Namespace. Sie müssen außerdem die folgenden Schritte ausführen: <ul><li>Aktivieren der vom System zugewiesenen Identität für ein Thema oder eine Domäne</li><li>Hinzufügen der Identität der Rolle „Azure Service Bus-Datenabsender“ für den Service Bus-Namespace</li><li>Konfigurieren Sie dann das Ereignisabonnement, das eine Service Bus-Warteschlange oder ein Thema als Endpunkt verwendet, um die vom System zugewiesene Identität zu verwenden.</li></ul> <p>Weitere Informationen finden Sie unter [Ereignisübermittlung mit einer verwalteten Identität](../articles/event-grid/managed-service-identity.md).</p>|
| Azure API Management | <p>Der API Management-Dienst ermöglicht Ihnen das Senden von Nachrichten an eine Service Bus-Warteschlange bzw. ein -Thema im Service Bus-Namespace.</p><ul><li>Sie können benutzerdefinierte Workflows auslösen, indem Sie Nachrichten an Ihre Service Bus-Warteschlange bzw- Ihr -Thema senden, wenn eine API mithilfe der [send-request-Richtlinie](../articles/api-management/api-management-sample-send-request.md) aufgerufen wird.</li><li>Sie können auch eine Service Bus Warteschlange bzw. ein Thema auch als Back-End in einer API behandeln. Eine Beispielrichtlinie finden Sie unter [Authentifizieren mithilfe einer verwalteten Identität für den Zugriff auf eine Service Bus-Warteschlange oder ein -Thema](https://github.com/Azure/api-management-policy-snippets/blob/master/examples/Authenticate%20using%20Managed%20Identity%20to%20access%20Service%20Bus.xml). Sie müssen außerdem die folgenden Schritte ausführen:<ol><li>Aktivieren einer vom System zugewiesenen Identität für die API Management-Instanz. Anleitungen dazu finden Sie unter [Verwenden von verwalteten Identitäten in Azure API Management](../articles/api-management/api-management-howto-use-managed-service-identity.md).</li><li>Hinzufügen der Identität der Rolle **Azure Service Bus-Datensender** für den Service Bus-Namespace</li></ol></li></ul> | 