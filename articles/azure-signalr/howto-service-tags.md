---
title: Verwenden von Diensttags
titleSuffix: Azure SignalR Service
description: Verwenden von Diensttags für das Zulassen von ausgehendem Datenverkehr zu Ihrer Azure SignalR Service-Instanz
services: signalr
author: ArchangelSDY
ms.service: signalr
ms.topic: article
ms.date: 05/06/2020
ms.author: dayshen
ms.openlocfilehash: 8810309fef5dbbb35465a2af15d42fa8a59d5401
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84302102"
---
# <a name="use-service-tags-for-azure-signalr-service"></a>Verwenden von Diensttags für Azure SignalR Service

Sie können [Diensttags](../virtual-network/security-overview.md#service-tags) für Azure SignalR Service verwenden, wenn Sie [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md#network-security-groups) konfigurieren. So können Sie Sicherheitsregeln für ausgehenden Datenverkehr zu Azure SignalR Service-Endpunkten definieren, ohne dass dafür IP-Adressen hartcodiert werden müssen.

Azure SignalR Service verwaltet diese Diensttags. Sie können kein eigenes Diensttag erstellen oder ein vorhandenes Tag ändern. Microsoft verwaltet diese Adresspräfixe, die dem Diensttag entsprechen, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

## <a name="use-service-tag-on-portal"></a>Verwenden von Diensttags im Portal

Sie können ausgehenden Datenverkehr zu Azure SignalR Service zulassen, indem Sie eine neue Sicherheitsregel für ausgehenden Datenverkehr im Netzwerk festlegen:

1. Rufen Sie die Netzwerksicherheitsgruppe auf.

1. Klicken Sie auf das Einstellungsmenü namens **Ausgangssicherheitsregeln**.

1. Klicken Sie oben auf die Schaltfläche **+ Hinzufügen**.

1. Klicken Sie unter **Ziel** auf die Option **Diensttag**.

1. Klicken Sie unter **Zieldiensttag** auf **AzureSignalR**.

1. Geben Sie für **Zielportbereiche** den Port **443** ein.

    ![Erstellen einer Ausgangssicherheitsregel](media/howto-service-tags/portal-add-outbound-security-rule.png)

1. Nehmen Sie an den anderen Feldern entsprechend Ihrer Anforderungen Anpassungen vor.

1. Klicken Sie auf **Hinzufügen**.


## <a name="next-steps"></a>Nächste Schritte

- [Netzwerksicherheitsgruppen: Diensttags](../virtual-network/security-overview.md#security-rules)
