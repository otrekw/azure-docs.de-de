---
title: Konfigurieren von Netzwerkzugriffssteuerung
titleSuffix: Azure SignalR Service
description: Hiermit wird die Netzwerkzugriffssteuerung für Azure SignalR Service konfiguriert.
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 24e56736e0d033420f9aaf976b0fb7d9727c1a5b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92143228"
---
# <a name="configure-network-access-control"></a>Konfigurieren von Netzwerkzugriffssteuerung

Azure SignalR Service ermöglicht es Ihnen, die Zugriffsebene für Ihren Dienstendpunkt basierend auf dem Anforderungstyp und der Teilmenge der verwendeten Netzwerke zu sichern und zu steuern. Wenn Netzwerkregeln konfiguriert wurden, können nur Anwendungen, die Daten über die angegebene Gruppe von Netzwerken anfordern, auf Azure SignalR Service zugreifen.

Azure SignalR Service verfügt über einen öffentlichen Endpunkt, auf den über das Internet zugegriffen werden kann. Sie können auch [private Endpunkte für Ihren Azure SignalR Service](howto-private-endpoints.md) erstellen. Der private Endpunkt weist Azure SignalR Service eine private IP-Adresse aus Ihrem VNET zu und sichert den gesamten Datenverkehr zwischen Ihrem VNET und Azure SignalR Service über eine private Verbindung. Die Netzwerkzugriffssteuerung von Azure SignalR Service bietet Zugriffssteuerung für öffentliche und für private Endpunkte.

Optional können Sie auch bestimmte Anforderungstypen für den öffentlichen Endpunkt und jeden privaten Endpunkt zulassen oder verweigern. Beispielsweise können Sie alle [Serververbindungen](signalr-concept-internals.md#server-connections) vom öffentlichen Endpunkt blockieren und sicherstellen, dass sie nur aus einem bestimmten VNET stammen.

Eine Anwendung, die bei aktivierten Netzwerk- Zugriffssteuerungsregeln auf Azure SignalR Service zugreift, benötigt weiterhin eine ordnungsgemäße Autorisierung für die Anforderung.

## <a name="scenario-a---no-public-traffic"></a>Szenario A: Kein öffentlicher Datenverkehr

Um den gesamten öffentlichen Datenverkehr vollständig zu verweigern, sollten Sie zunächst die öffentliche Netzwerkregel so konfigurieren, dass kein Anforderungstyp zulässig ist. Anschließend sollten Sie Regeln konfigurieren, die den Zugriff auf Datenverkehr aus bestimmten VNETs gewähren. Mit dieser Konfiguration können Sie eine sichere Netzwerkgrenze für Ihre Anwendungen erstellen.

## <a name="scenario-b---only-client-connections-from-public-network"></a>Szenario B: Nur Clientverbindungen aus öffentlichem Netzwerk

In diesem Szenario können Sie die öffentliche Netzwerkregel so konfigurieren, dass nur [Clientverbindungen](signalr-concept-internals.md#client-connections) aus dem öffentlichen Netzwerk zulässig sind. Anschließend können Sie private Netzwerkregeln konfigurieren, um andere Typen von Anforderungen zu ermöglichen, die von einem bestimmten VNET stammen. Diese Konfiguration blendet ihre App-Server aus dem öffentlichen Netzwerk aus und stellt sichere Verbindungen zwischen Ihren App-Servern und Azure SignalR Service her.

## <a name="managing-network-access-control"></a>Verwalten der Netzwerkzugriffssteuerung

Sie können Netzwerkzugriffssteuerung für Azure SignalR Service über das Azure-Portal verwalten.

### <a name="azure-portal"></a>Azure-Portal

1. Navigieren Sie zu dem Azure SignalR Service, den Sie sichern möchten.

1. Klicken Sie auf das Einstellungsmenü namens **Netzwerkzugriffssteuerung**.

    ![Netzwerk-ACL im Portal](media/howto-network-access-control/portal.png)

1. Um die Standardaktion zu bearbeiten, schalten Sie die Schaltfläche **Zulassen/Verweigern** um.

    > [!TIP]
    > Die Standardaktion ist die Aktion, die ausgeführt wird, wenn keine Übereinstimmungen mit der ACL-Regel vorhanden sind. Wenn die Standardaktion z. B. **Verweigern** ist, werden Anforderungstypen, die nicht explizit genehmigt sind, verweigert.

1. Wählen Sie zum Bearbeiten der öffentlichen Netzwerkregel zulässige Anforderungstypen unter **Öffentliches Netzwerk** aus.

    ![Bearbeiten der ACL für das öffentliche Netzwerk im Portal ](media/howto-network-access-control/portal-public-network.png)

1. Wählen Sie zum Bearbeiten der Netzwerkregeln für den privaten Endpunkt zulässige Anforderungstypen unter **Private Endpunktverbindungen** in jeder Zeile aus.

    ![Bearbeiten der ACL für den privaten Endpunkt im Portal ](media/howto-network-access-control/portal-private-endpoint.png)

1. Klicken Sie zum Übernehmen der Änderungen auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure Private Link](../private-link/private-link-overview.md).