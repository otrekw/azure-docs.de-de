---
title: Sicherheitskontrollen für Azure Event Hubs
description: Dieser Artikel bietet eine Prüfliste mit Sicherheitskontrollen für die Evaluierung von Azure Event Hubs (Netzwerk, Identität, Datenschutz usw.).
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: da20778f1e24372e445d635e675df6484905f195
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85315401"
---
# <a name="security-controls-for-azure-event-hubs"></a>Sicherheitskontrollen für Azure Event Hubs

In diesem Artikel werden die in Azure Event Hubs integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Ja |  |  |
| Unterstützung der VNet-Einschleusung| Nein | |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja |  |  |
| Unterstützung der Tunnelerzwingung| Nein |  |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |  |
| Protokollierung und Überwachung auf Datenebene| Ja |   |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentifizierung| Ja | | [Autorisieren des Zugriffs auf Azure Event Hubs](authorize-access-event-hubs.md), [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory](authorize-access-azure-active-directory.md), [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Shared Access Signatures](authorize-access-shared-access-signature.md) |
| Authorization|  Ja | | [Authentifizieren einer verwalteten Identität mit Azure Active Directory für den Zugriff auf Event Hubs-Ressourcen](authenticate-managed-identity.md), [Authentifizieren einer Anwendung mit Azure Active Directory, um auf Azure Event Hubs-Ressourcen zuzugreifen](authenticate-application.md), [Authentifizieren des Zugriffs auf Event Hubs-Ressourcen mit Shared Access Signatures (SAS)](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  Ja | |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja. Für dedizierte Cluster verfügbar. | Ein vom Kunden verwalteter Schlüssel in Azure Key Vault kann zum Verschlüsseln der ruhenden Daten auf einem Event Hub verwendet werden. | [Konfigurieren von kundenseitig verwalteten Schlüsseln für die Verschlüsselung ruhender Azure Event Hubs-Daten mithilfe des Azure-Portals](configure-customer-managed-key.md) |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | |  |
| Verschlüsselte API-Aufrufe| Ja |  |  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | |  |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
