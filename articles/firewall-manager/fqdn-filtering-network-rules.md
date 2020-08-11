---
title: Azure Firewall Manager-Filterung in Netzwerkregeln (Vorschauversion)
description: Hier erfahren Sie, wie Sie die FQDN-Filterung in Netzwerkregeln verwenden.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: 28cd26532ca5bdf83902854b7910f7d6c18a4eab
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460149"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>FQDN-Filterung in Netzwerkregeln (Vorschau)

> [!IMPORTANT]
> Die FQDN-Filterung in Netzwerkregeln befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ein vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) stellt den Domänennamen eines Hosts oder von IP-Adressen dar. Sie können FQDNs in Netzwerkregeln verwenden, basierend auf der DNS-Auflösung in Azure Firewall und der Firewallrichtlinie. Diese Funktion ermöglicht es Ihnen, ausgehenden Datenverkehr mit einem beliebigen TCP/UDP-Protokoll (einschließlich NTP, SSH, RDP und mehr) zu filtern. Für die Verwendung von FQDNs in Ihren Netzwerkregeln müssen Sie den DNS-Proxy aktivieren. Weitere Informationen finden Sie unter [DNS-Einstellungen für Azure Firewall-Richtlinie (Vorschau)](dns-settings.md).

## <a name="how-it-works"></a>Funktionsweise

Nachdem Sie definiert haben, welchen DNS-Server Ihre Organisation benötigt (Azure DNS oder Ihr eigenes benutzerdefiniertes DNS), übersetzt Azure Firewall den FQDN in IP-Adressen, basierend auf dem ausgewählten DNS-Server. Diese Übersetzung erfolgt bei der Verarbeitung von Anwendungs- und Netzwerkregeln.

Worin besteht der Unterschied zwischen der Verwendung von Domänennamen in Anwendungsregeln im Vergleich zu der Verwendung in Netzwerkregeln? 

- Die FQDN-Filterung in Anwendungsregeln für HTTP/S und MSSQL basiert auf einem auf Anwendungsebene transparenten Proxy und dem SNI-Header. Daher kann hierbei zwischen zwei FQDNs unterschieden werden, die zur selben IP-Adresse aufgelöst werden. Bei der Verwendung von FQDN-Filterung in Netzwerkregeln ist dies nicht der Fall. Verwenden Sie nach Möglichkeit immer Anwendungsregeln.
- In Anwendungsregeln können Sie HTTP/S und MSSQL als Ihre ausgewählten Protokolle verwenden. In Netzwerkregeln können Sie ein beliebiges TCP/UDP-Protokoll mit ihren Ziel-FQDNs verwenden.

## <a name="next-steps"></a>Nächste Schritte

[DNS-Einstellungen für Azure Firewall](dns-settings.md)
