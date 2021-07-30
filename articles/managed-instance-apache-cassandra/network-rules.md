---
title: Erforderliche ausgehende Netzwerkregeln für Azure Managed Instance for Apache Cassandra
description: Lernen Sie die erforderlichen ausgehenden Netzwerkregeln und vollqualifizierten Domänennamen (FQDN) für Azure Managed Instance for Apache Cassandra kennen.
author: christopheranderson
ms.service: managed-instance-apache-cassandra
ms.topic: how-to
ms.date: 05/21/2021
ms.author: chrande
ms.openlocfilehash: 8c66657832000d1d0f9e1e9d842f16ce149e314a
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111955976"
---
# <a name="required-outbound-network-rules"></a>Erforderliche Netzwerkregeln für ausgehenden Datenverkehr

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Der Azure Managed Instance for Apache Casandra-Dienst erfordert bestimmte Netzwerkregeln, um den Dienst ordnungsgemäß verwalten zu können. Indem Sie sicherstellen, dass die richtigen Regeln verfügbar gemacht werden, können Sie Ihren Dienst schützen und Betriebsprobleme verhindern.

## <a name="azure-global-required-network-rules"></a>Für Azure Global benötigte Netzwerkregeln

Folgende Netzwerkregeln und IP-Adressabhängigkeiten werden benötigt:

| Zielendpunkt                                                             | Protokoll | Port    | Zweck  |
|----------------------------------------------------------------------------------|----------|---------|------|
|snovap`<region>`.blob.core.windows.net:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) – Azure Storage | HTTPS | 443 | Erforderlich für die sichere Kommunikation zwischen den Knoten und Azure Storage für die Kommunikation und Konfiguration der Steuerungsebene.|
|*.blob.core.windows.net:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) – Azure Storage | HTTPS | 443 | Erforderlich für die sichere Kommunikation zwischen den Knoten und Azure Storage zum Speichern von Sicherungen. *Das Sicherungsfeature (Backup) wird überarbeitet, und der Speichername wird ein Muster der allgemeinen Verfügbarkeit (GA) einhalten.*|
|vmc-p-`<region>`.vault.azure.net:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) � Azure KeyVault | HTTPS | 443 | Erforderlich für die sichere Kommunikation zwischen den Knoten und Azure Key Vault. Zertifikate und Schlüssel werden verwendet, um die Kommunikation innerhalb des Clusters zu sichern.|
|management.azure.com:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) � Azure Virtual Machine Scale Sets/Azure-Verwaltungs-API | HTTPS | 443 | Erforderlich, um Informationen zu Cassandra-Knoten zu sammeln und zu verwalten (z. B. Neustart).|
|*.servicebus.windows.net:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) � Azure EventHub | HTTPS | 443 | Erforderlich zum Weiterleiten von Protokollen an Azure.|
|jarvis-west.dc.ad.msft.net:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) � Azure Monitor | HTTPS | 443 | Erforderlich zum Weiterleiten von Metriken an Azure. |
|login.microsoftonline.com:443</br> oder</br> [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) � Azure AD | HTTPS | 443 | Erforderlich für die Azure Active Directory-Authentifizierung.|
| packages.microsoft.com | HTTPS | 443 | Erforderlich für Updates der Definition und Signaturen der Azure-Sicherheitsscanner. |

## <a name="managed-instance-for-apache-cassandra-internal-port-usage"></a>Verwendung interner Ports von Managed Instance for Apache Cassandra

Auf die folgenden Ports kann nur innerhalb des VNET (oder gepeerte vnets./ExpressRoutes) zugegriffen werden. Managed Instance for Apache Cassandra-Instanzen verfügen über keine öffentliche IP-Adresse und sollten nicht über das Internet zugänglich gemacht werden.

| Port | Zweck |
| ---- | --- |
| 8443 | Intern |
| 9443 | Intern |
| 7001 | Gossip: Wird von Cassandra-Knoten für die gegenseitige Kommunikation verwendet. |
| 9042 | Cassandra: Wird von Clients für Verbindungen mit Cassandra verwendet. |
| 7199 | Intern |

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Netzwerkregeln zum ordnungsgemäßen Verwalten des Diensts kennen gelernt. Weitere Informationen zu Azure Managed Instance for Apache Cassandra finden Sie in den folgenden Artikeln:

* [Was ist Azure Managed Instance for Apache Cassandra? (Vorschau)](introduction.md)
* [Verwalten von Azure Managed Instance for Apache Cassandra-Ressourcen mit der Azure CLI](manage-resources-cli.md)