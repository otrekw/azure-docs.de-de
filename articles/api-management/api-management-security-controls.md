---
title: Sicherheitskontrollen für Azure API Management
description: Prüfen Sie eine Checkliste mit Sicherheitskontrollen zur Bewertung von API Management. Hierzu gehören Netzwerk-, Identitäts- und Datenschutzkontrollen.
services: api-management
author: vladvino
ms.service: api-management
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: vlvinogr
ms.openlocfilehash: a147179f7b55e43379b3c3fa3a7a0767cc97b198
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87902574"
---
# <a name="security-controls-for-api-management"></a>Sicherheitskontrollen für API Management

In diesem Artikel werden die in API Management integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Nein | |  |
| Unterstützung der VNet-Einschleusung| Ja | |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Mithilfe von Netzwerksicherheitsgruppen (NSGs) bzw. Azure Application Gateway (oder anderen Softwareappliances) |  |
| Unterstützung der Tunnelerzwingung| Ja | Azure-Netzwerke unterstützen die Tunnelerzwingung. |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | | |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | [Azure Monitor-Aktivitätsprotokolle](../azure-monitor/platform/platform-logs-overview.md) | |
| Protokollierung und Überwachung auf Datenebene| Ja | [Azure Monitor-Diagnoseprotokolle](../azure-monitor/platform/platform-logs-overview.md) und (optional) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md)  | |


## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentifizierung| Ja | |  |
| Authorization| Ja | |  |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Vertrauliche Daten, z. B. Zertifikate, Schlüssel und als geheim gekennzeichnete Werte, werden mit vom Dienst verwalteten Schlüsseln für die jeweilige Dienstinstanz verschlüsselt. |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein | Alle Verschlüsselungsschlüssel gelten pro Dienstinstanz und werden vom Dienst verwaltet. |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | [ExpressRoute](../expressroute/index.yml)- und VNET-Verschlüsselung erfolgen über [Azure-Netzwerke](../virtual-network/index.yml). |  |
| Verschlüsselte API-Aufrufe| Ja | Aufrufe auf Verwaltungsebene erfolgen über [Azure Resource Manager](../azure-resource-manager/index.yml) und TLS. Ein gültiges JSON Web Token (JWT) ist erforderlich.  Aufrufe auf Datenebene können mit TLS und einem der unterstützten Authentifizierungsmechanismen (z. B. Clientzertifikat oder JWT) geschützt werden. |   |
 |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Verwendung des [Azure API Management DevOps Resource Kit](https://aka.ms/apimdevops) |  |

## <a name="vulnerability-scans-false-positives"></a>Überprüfungen auf Sicherheitsrisiken: Falsch positive Ergebnisse

In diesem Abschnitt sind häufige Sicherheitsrisiken dokumentiert, die sich nicht auf Azure API Management auswirken.

| Sicherheitsrisiko               | BESCHREIBUNG                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed ist ein Sicherheitsrisiko bei der Implementierung der TLS SessionTicket-Erweiterung in einigen F5-Produkten. Hierbei wird das Abschöpfen („Bleeding“) von bis zu 31 Byte an Daten aus nicht initialisiertem Arbeitsspeicher ermöglicht. Die Ursache ist, dass der TLS-Stapel eine vom Client übergebene Sitzungs-ID mit Daten auffüllt, um eine Länge von 32 Bit zu erzielen. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).