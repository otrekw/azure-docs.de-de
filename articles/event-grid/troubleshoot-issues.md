---
title: Behandeln von Event Grid-Problemen
description: Dieser Artikel bietet verschiedene Methoden zur Behandlung von Azure Event Grid-Problemen.
ms.topic: conceptual
ms.date: 02/11/2021
ms.openlocfilehash: d30b8464de90474ad74853cc423de700b41226a4
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "104720558"
---
# <a name="troubleshoot-azure-event-grid-issues"></a>Behandlung von Azure Event Grid-Problemen
Dieser Artikel enthält Informationen, die Ihnen bei der Behandlung von Azure Event Grid-Problemen helfen. 

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Aktivieren Sie Diagnoseeinstellungen für Event Grid-Themen oder -Domänen, um Veröffentlichungs- und Übermittlungsfehlerprotokolle zu erfassen und anzuzeigen. Weitere Informationen finden Sie unter [Diagnoseprotokolle](enable-diagnostic-logs-topic.md).

## <a name="metrics"></a>Metriken
Sie können Metriken für Event Grid-Themen und -Abonnements anzeigen und Warnungen dazu erstellen. Weitere Informationen finden Sie unter [Event Grid-Metriken](monitor-event-delivery.md).

## <a name="alerts"></a>Alerts
Erstellen Sie Warnungen zu Azure Event Grid-Metriken und -Aktivitätsprotokollvorgängen. Weitere Informationen finden Sie unter [Warnungen zu Event Grid-Metriken und Aktivitätsprotokollen](set-alerts.md).

## <a name="subscription-validation-issues"></a>Abonnementüberprüfungsprobleme
Während der Erstellung eines Ereignisabonnements erhalten Sie möglicherweise eine Fehlermeldung, die besagt, dass die Überprüfung des angegebenen Endpunkts fehlgeschlagen ist. Informationen zur Behandlung von Abonnementüberprüfungsproblemen finden Sie unter [Problembehandlung von Event Grid-Abonnementüberprüfungen](troubleshoot-subscription-validation.md). 

## <a name="network-connectivity-issues"></a>Probleme mit der Netzwerkkonnektivität
Es gibt verschiedene Gründe dafür, dass Clientanwendungen keine Verbindung mit einem Event Grid-Thema bzw. einer -Domäne herstellen können. Die auftretenden Konnektivitätsprobleme können dauerhaft oder vorübergehend sein. Informationen zum Beheben dieser Probleme finden Sie unter [Beheben von Konnektivitätsproblemen](troubleshoot-network-connectivity.md).

## <a name="error-codes"></a>Fehlercodes
Wenn Sie Fehlermeldungen mit Fehlercodes wie 400, 409 und 403 erhalten, finden Sie weitere Informationen unter [Beheben von Event Grid-Fehlern](troubleshoot-errors.md). 

## <a name="distributed-tracing-net"></a>Verteilte Ablaufverfolgung (.NET)
Die Event Grid .NET-Bibliothek unterstützt verteilte Ablaufverfolgung. Um dem [Leitfaden der CloudEvents-Spezifikation](https://github.com/cloudevents/spec/blob/master/extensions/distributed-tracing.md) für verteilte Ablaufverfolgung zu genügen, legt die Bibliothek `traceparent` und `tracestate` für die [ExtensionAttributes](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventgrid/Azure.Messaging.EventGrid/src/Customization#L126) eines `CloudEvent` fest, wenn verteilte Ablaufverfolgung aktiviert ist. Weitere Informationen zum Aktivieren von verteilter Ablaufverfolgung in Ihrer Anwendung finden Sie im Azure SDK in der [Dokumentation zur verteilten Ablaufverfolgung](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/core/Azure.Core/samples/Diagnostics.md#Distributed-tracing).

### <a name="sample"></a>Beispiel
Weitere Informationen finden Sie im [Zeilenzählungsbeispiel](/samples/azure/azure-sdk-for-net/line-counter/). Diese Beispiel-App veranschaulicht die Verwendung von Storage, Event Hubs und Event Grid-Clients und zeigt auch ASP.NET Core-Integration, verteilte Ablaufverfolgung und gehostete Dienste. Sie ermöglicht es Benutzern, eine Datei in ein Blob hochzuladen, wodurch ein Event Hubs-Ereignis ausgelöst wird, das den Dateinamen enthält. Der Event Hubs-Prozessor empfängt das Ereignis, die App lädt dann das Blob herunter und zählt die Anzahl der Zeilen in der Datei. Die App zeigt einen Link zu einer Seite an, die die Zeilenanzahl enthält. Wenn auf den Link geklickt wird, wird ein CloudEvent mit dem Namen der Datei mit Event Grid veröffentlicht.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie weitere Hilfe benötigen, veröffentlichen Sie Ihr Problem im [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-eventgrid), oder öffnen Sie ein [Supportticket](https://azure.microsoft.com/support/options/). 
