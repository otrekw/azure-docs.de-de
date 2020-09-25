---
title: Benachrichtigungen in Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Senden Sie Benachrichtigungen an Benutzer von Apps, die auf Azure Communication Services basieren.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 5be2ff48ea5510c54c79e76e8bae082bd5085794
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944272"
---
# <a name="communication-services-notifications"></a>Communication Services-Benachrichtigungen

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Der Chat von Azure Communication Services und die Bibliotheken für Telefonie-Clients erstellen einen Kanal für Echtzeitnachrichten, der es ermöglicht, Signalisierungsnachrichten per Push effizient und zuverlässig an verbundene Clients zu übermitteln. Dadurch können Sie umfangreiche Echtzeit-Kommunikationsfunktionen in Ihre Anwendungen integrieren, ohne dass Sie eine komplizierte HTTP-Abruflogik implementieren müssen. Bei mobilen Anwendungen bleibt die Verbindung dieses Signalisierungskanals jedoch nur dann bestehen, wenn die Anwendung im Vordergrund aktiv ist. Wenn Sie möchten, dass Ihre Benutzer eingehende Anrufe oder Chatnachrichten empfangen, während sich die Anwendung im Hintergrund befindet, sollten Sie Pushbenachrichtigungen verwenden.

Mithilfe von Pushbenachrichtigungen können Sie Informationen von der Anwendung an die mobilen Geräte der Benutzer senden. Sie können Pushbenachrichtigungen verwenden, um ein Dialogfeld anzuzeigen, einen Ton wiederzugeben oder die Benutzeroberfläche für eingehenden Anrufe anzuzeigen. Azure Communication Services bietet Integrationen in [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) und [Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-overview), mit denen Sie zu Ihren Apps Pushbenachrichtigungen hinzufügen können.

## <a name="trigger-push-notifications-via-azure-event-grid"></a>Auslösen von Pushbenachrichtigungen über Azure Event Grid

Azure Communication Services ist in [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) integriert, um Echtzeit-Ereignisbenachrichtigungen zuverlässig, skalierbar und sicher bereitzustellen. Sie können diese Integration nutzen, um einen Benachrichtigungsdienst zu erstellen, der mobile Pushbenachrichtigungen an Ihre Benutzer übergibt, indem Sie ein Event Grid-Abonnement erstellen, das eine [Azure-Funktion](https://docs.microsoft.com/azure/azure-functions/functions-overview) oder einen Webhook auslöst.

:::image type="content" source="./media/notifications/acs-events-int.png" alt-text="Diagramm, das die Integration von Communication Services in Event Grid zeigt":::

Erfahren Sie mehr über die [Ereignisbehandlung in Azure Communication Services](./event-handling.md).

## <a name="deliver-push-notifications-via-azure-notification-hubs"></a>Bereitstellen von Pushbenachrichtigungen über Azure Notification Hubs

Sie können einen Azure Notification Hub mit Ihrer Communication Services-Ressource verbinden, um automatisch Pushbenachrichtigungen an das mobile Gerät eines Benutzers zu senden, wenn er einen eingehenden Anruf erhält. Verwenden Sie diese Pushbenachrichtigungen, um Ihre Anwendung im Hintergrund zu reaktivieren und die Benutzeroberfläche anzuzeigen, über die Benutzer den Anruf annehmen oder ablehnen können. 

:::image type="content" source="./media/notifications/acs-anh-int.png" alt-text="Diagramm, das die Integration von Communication Services in Azure Notification Hubs zeigt":::

Communication Services verwendet Azure Notification Hub als Passthroughdienst, um mit den verschiedenen plattformspezifischen Pushbenachrichtigungsdiensten über die [Direct Send](https://docs.microsoft.com/rest/api/notificationhubs/direct-send)-API zu kommunizieren. Auf diese Weise können Sie Ihre vorhandenen Azure Notification Hub-Ressourcen und -Konfigurationen wiederverwenden, um geringe Latenzzeiten und zuverlässige Anrufbenachrichtigungen für Ihre Anwendungen bereitzustellen.

### <a name="notification-hub-provisioning"></a>Bereitstellen von Notification Hub 

Zum Übermitteln von Pushbenachrichtigungen an Clientgeräte mithilfe von Notification Hubs [erstellen Sie einen Notification Hub](https://docs.microsoft.com/azure/notification-hubs/create-notification-hub-portal) innerhalb desselben Abonnements wie Ihre Communication Services-Ressource. Azure Notification Hubs müssen für den Plattformbenachrichtigungsdienst konfiguriert sein, den Sie verwenden möchten. Informationen zum Empfang von Pushbenachrichtigungen von Notification Hubs in Ihrer Client-App finden Sie unter [Erste Schritte mit Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/ios-sdk-get-started). Wählen Sie dort in der Dropdownliste oben auf der Seite die Zielplattform für den Client aus.

Nachdem Ihr Notification Hub konfiguriert wurde, können Sie ihn Ihrer Communication Services-Ressource zuweisen, indem Sie für den Hub mithilfe des Azure Resource Manager-Clients oder des Azure-Portals eine Verbindungszeichenfolge bereitstellen. Die Verbindungszeichenfolge muss Sendeberechtigungen enthalten. Es wird empfohlen, eine andere Zugriffsrichtlinie nur mit Sendeberechtigungen zu erstellen, die speziell für Ihren Hub gilt. Erfahren Sie mehr über die [Sicherheit von Notification Hubs und Zugriffsrichtlinien](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-security).

> HINWEIS:  Damit Sie Apple Push Notification Service-VoIP-Benachrichtigungen aktivieren können, müssen Sie den Namen Ihres Notification Hubs als Anwendungsbündel-ID mit dem Suffix „`.voip`“ festlegen. Weitere Informationen finden Sie unter [Verwenden von APNS-VoIP über Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/voip-apns).

#### <a name="using-the-azure-resource-manager-client-to-configure-the-notification-hub"></a>Konfigurieren des Notification Hubs mithilfe des Azure Resource Manager-Clients

Führen Sie Folgendes aus, um sich bei Azure Resource Manager anzumelden, und melden Sie sich mit Ihren Anmeldeinformationen an.

```console
armclient login
```

 Nach erfolgreicher Anmeldung führen Sie Folgendes aus, um den Notification Hub bereitzustellen:

```console
armclient POST /subscriptions/<sub_id>/resourceGroups/<resource_group>/providers/Microsoft.Communication/CommunicationServices/<resource_id>/linkNotificationHub?api-version=2020-08-20-preview "{'connectionString': '<connection_string>','resourceId': '<resource_id>'}"
```

#### <a name="using-the-azure-portal-to-configure-the-notification-hub"></a>Konfigurieren des Notification Hubs mithilfe des Azure-Portals

Navigieren Sie im Azure-Portal zu Ihrer Azure Communication Services-Ressource. Wählen Sie in der Communication Services-Ressource im linken Menü der Seite „Communication Services“ die Option „Pushbenachrichtigungen“ aus, und stellen Sie eine Verbindung mit dem zuvor bereitgestellten Benachrichtigungshub her. Sie müssen die Verbindungszeichenfolge und die Ressourcen-ID hier angeben:

:::image type="content" source="./media/notifications/acs-anh-portal-int.png" alt-text="Screenshot, der die Einstellungen für Pushbenachrichtigungen im Azure-Portal zeigt":::

#### <a name="device-registration"></a>Geräteregistrierung 

Informationen zum Registrieren Ihres Gerätehandles bei Communication Services finden Sie im [Schnellstart zu Sprachanrufen](../quickstarts/voice-video-calling/getting-started-with-calling.md).

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).
* Weitere Informationen zu den Azure Notification Hub-Konzepten finden Sie in der [Dokumentation zu Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/).
