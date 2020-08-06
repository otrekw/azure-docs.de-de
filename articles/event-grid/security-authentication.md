---
title: Authentifizieren der Ereignisübermittlung an Ereignishandler (Azure Event Grid)
description: In diesem Artikel werden verschiedene Methoden zum Authentifizieren der Übermittlung an Ereignishandler in Azure Event Grid beschrieben.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: abe16c9598c8c10caa832150aafac997dd7f1624
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460642"
---
# <a name="authenticate-event-delivery-to-event-handlers-azure-event-grid"></a>Authentifizieren der Ereignisübermittlung an Ereignishandler (Azure Event Grid)
Dieser Artikel enthält Informationen zum Authentifizieren der Ereignisübermittlung an Ereignishandler. Er zeigt auch das Sichern der Webhook-Endpunkte für das Empfangen von Ereignissen von Event Grid über Azure Active Directory (Azure AD) oder ein gemeinsames Geheimnis.

## <a name="use-system-assigned-identities-for-event-delivery"></a>Verwenden von systemseitig zugewiesenen Identitäten für die Ereignisübermittlung
Sie können eine vom System zugewiesene verwaltete Identität für ein Thema oder eine Domäne aktivieren und mit dieser Identität Ereignisse an unterstützte Ziele weiterleiten, z. B. Service Bus-Warteschlangen und -Themen, Event Hubs und Speicherkonten.

Im Folgenden werden die Schritte aufgeführt: 

1. Erstellen Sie ein Thema oder eine Domäne mit einer vom System zugewiesenen Identität, oder aktualisieren Sie ein vorhandenes Thema oder eine Domäne, um die Identität zu aktivieren. 
1. Fügen Sie die Identität einer geeigneten Rolle (z. B. Azure Service Bus-Datensender) für das Ziel (z. B. Service Bus-Warteschlange) zu.
1. Aktivieren Sie beim Erstellen von Ereignisabonnements die Verwendung der Identität, um Ereignisse an das Ziel zu übermitteln. 

Ausführliche Anweisungen finden Sie unter [Ereignisübermittlung mit einer verwalteten Identität](managed-service-identity.md).


## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>Authentifizieren der Ereignisübermittlung an Webhook-Endpunkte
In den folgenden Abschnitten wird beschrieben, wie die Übermittlung von Ereignissen an Webhook-Endpunkte authentifiziert wird. Sie müssen unabhängig von der verwendeten Methode einen Mechanismus für einen Überprüfungshandshake verwenden. Weitere Informationen finden Sie unter [Webhook-Ereignisbereitstellung](webhook-event-delivery.md). 


### <a name="using-azure-active-directory-azure-ad"></a>Mit Azure Active Directory (Azure AD)
Sie können den Webhook-Endpunkt, der verwendet wird, um Ereignisse von Event Grid zu empfangen, mit Azure AD sichern. Sie müssen eine Azure AD-Anwendung erstellen, eine Rolle und einen Dienstprinzipal zur Autorisierung von Event Grid in Ihrer Anwendung erstellen und das Ereignisabonnement zur Verwendung der Azure AD-Anwendung konfigurieren. Erfahren Sie, wie Sie [Azure Active Directory mit Event Grid konfigurieren](secure-webhook-delivery.md).

### <a name="using-client-secret-as-a-query-parameter"></a>Mit dem geheimen Clientschlüssel als Abfrageparameter
Sie können auch den Webhook-Endpunkt sichern, indem Sie der Webhook-Ziel-URL, die beim Erstellen eines Ereignisabonnements angegeben wurde, Abfrageparameter hinzufügen. Legen Sie einen der Abfrageparameter als geheimen Clientschlüssel, z. B. ein [Zugriffstoken](https://en.wikipedia.org/wiki/Access_token), oder als gemeinsamen geheimem Schlüssel fest. Der Event Grid-Dienst nimmt alle Abfrageparameter in jede Ereignisbereitstellungsanforderung an den Webhook auf. Der Webhookdienst kann das Geheimnis abrufen und überprüfen. Wenn der geheime Clientschlüssel aktualisiert wird, muss das Ereignisabonnement ebenfalls aktualisiert werden. Damit bei dieser Geheimnisrotation keine Übermittlungsfehler auftreten, lassen Sie den Webhook für einen begrenzten Zeitraum sowohl alte als auch neue Geheimnisse akzeptieren, bevor das Ereignisabonnement mit dem neuen Geheimnis aktualisiert wird. 

Da Abfrageparameter geheime Clientschlüssel enthalten können, werden Sie mit besonderer Sorgfalt behandelt. Sie werden verschlüsselt gespeichert und sind für Dienstoperatoren nicht zugänglich. Sie werden nicht im Rahmen der Dienstprotokollierung/Ablaufverfolgung protokolliert. Beim Abrufen der Ereignisabonnementeigenschaften werden die Zielabfrageparameter standardmäßig nicht zurückgegeben. Beispiel: In der Azure [CLI](/cli/azure?view=azure-cli-latest) muss der Parameter [--include-full-endpoint-url](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) verwendet werden.

Weitere Informationen zum Übermitteln von Ereignissen an Webhooks finden Sie unter [Webhook-Ereignisbereitstellung](webhook-event-delivery.md).

> [!IMPORTANT]
Azure Event Grid unterstützt nur **HTTPS**-Webhook-Endpunkte. 


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Authentifizieren von Clients, die Ereignisse an Themen oder Domänen pushen, finden Sie unter [Authentifizieren von Veröffentlichungsclients](security-authenticate-publishing-clients.md). 
