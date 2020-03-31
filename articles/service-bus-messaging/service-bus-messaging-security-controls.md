---
title: Sicherheitskontrollen für Azure Service Bus Messaging
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung von Azure Service Bus Messaging
services: service-bus-messaging
ms.service: service-bus-messaging
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: af119ef026b70fcb4a56b4f823d20c0e9eddddc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75903247"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Sicherheitskontrollen für Azure Service Bus Messaging

In diesem Artikel werden die in Azure Service Bus Messaging integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Ja (nur Premium-Tarif) | VNET-Dienstendpunkte werden nur für den [Premium-Tarif von Service Bus](service-bus-premium-messaging.md) unterstützt. |  |
| Unterstützung der VNet-Einschleusung| Nein | |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja (nur Premium-Tarif) |  |  |
| Unterstützung der Tunnelerzwingung| Nein |  |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Unterstützt über [Azure Monitor und -Warnungen](service-bus-metrics-azure-monitor.md). |  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Vorgangsprotokolle sind verfügbar.  | [Service Bus-Diagnoseprotokolle](service-bus-diagnostic-logs.md) |
| Protokollierung und Überwachung auf Datenebene| Nein |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentifizierung| Ja | Verwaltet über die [verwaltete Dienstidentität von Azure Active Directory](service-bus-managed-service-identity.md).| [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md) |
| Authorization| Ja | Unterstützt die Autorisierung über rollenbasierte Zugriffssteuerung ([RBAC](authenticate-application.md)) und SAS-Token. | [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md) |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  Ja, standardmäßig für serverseitige Verschlüsselung ruhender Daten. |  |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja. | Ein vom Kunden verwalteter Schlüssel in Azure Key Vault kann zum Verschlüsseln der ruhenden Daten im Service Bus-Namespace verwendet werden. | [Konfigurieren von kundenseitig verwalteten Schlüsseln für die Verschlüsselung ruhender Azure Service Bus-Daten mithilfe des Azure-Portals](configure-customer-managed-key.md)  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |   |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Unterstützt HTTPS/TLS-Standardverfahren. |   |
| Verschlüsselte API-Aufrufe| Ja | API-Aufrufe erfolgen über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS. |   |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützt Versionsverwaltung durch den Ressourcenanbieter über [Azure Resource Manager-API](/rest/api/resources/).|   |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
