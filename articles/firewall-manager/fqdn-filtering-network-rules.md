---
title: FQDN-Filterung von Azure Firewall in Netzwerkregeln (Vorschau)
description: Hier erfahren Sie, wie Sie die FQDN-Filterung in Netzwerkregeln verwenden.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 46d1f2eae1342901a660f99891d1ac1529e8ad77
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85567958"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>FQDN-Filterung in Netzwerkregeln (Vorschau)

> [!IMPORTANT]
> Die FQDN-Filterung in Netzwerkregeln befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ein vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) stellt den Domänennamen eines Hosts dar. Ein Domänenname ist einer einzelnen oder mehreren IP-Adressen zugeordnet. Sie können FQDNs und FQDN-Tags in Anwendungsregeln zulassen oder blockieren. Mithilfe von benutzerdefinierten DNS- und DNS-Proxyeinstellungen können Sie die FQDN-Filterung auch in Netzwerkregeln anwenden.

## <a name="how-it-works"></a>Funktionsweise

In Azure Firewall wird der vollqualifizierte Domänenname mithilfe der DNS-Einstellungen in eine oder mehrere IP-Adressen umgewandelt und die Regelverarbeitung gemäß der Azure DNS-Konfiguration oder einer benutzerdefinierten DNS-Konfiguration ausgeführt.

Für die Verwendung von FQDNs in Netzwerkregeln wird die Aktivierung des DNS-Proxys empfohlen. Andernfalls werden die Regeln möglicherweise nicht korrekt verarbeitet. Bei aktiviertem DNS-Proxy wird der DNS-Datenverkehr zu Azure Firewall geleitet, wo Sie den benutzerdefinierten DNS-Server konfigurieren können. Auf diese Weise verwenden Firewall und Clients denselben konfigurierten DNS-Server. Ist der DNS-Proxy nicht aktiviert, erzeugt Azure Firewall möglicherweise unterschiedliche Antworten, da der Client und die Firewall unterschiedliche Server für die Namensauflösung verwenden. Erhalten der Client und die Firewall unterschiedliche DNS-Antworten, wird die FQDN-Filterung in Netzwerkregeln möglicherweise fehlerhaft oder inkonsistent ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

[DNS-Einstellungen für Azure Firewall](dns-settings.md)
