---
title: Sicherheitskontrollen
titleSuffix: Azure Storage
description: Zeigen Sie eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure Storage an. Die behandelten Bereiche sind Datenschutz, Netzwerk, Überwachung und Protokollierung, Identität und Konfiguration.
services: storage
author: msmbaldwin
ms.author: mbaldwin
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: b816e7bd5e00b21700bc994fc0860195d39f2915
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87826516"
---
# <a name="security-controls-for-azure-storage"></a>Sicherheitskontrollen für Azure Storage

In diesem Artikel werden die in Azure Storage integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja | Weitere Informationen finden Sie unter [Speicherdienstverschlüsselung mit von Kunden verwalteten Schlüsseln im Azure Key Vault](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Unterstützung für die Standardverfahren HTTPS/TLS.  Benutzer können Daten auch verschlüsseln, bevor sie mit dem Dienst übertragen werden. |
| Verschlüsselte API-Aufrufe| Ja |  |

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung von Diensttags| Ja | Weitere Informationen zu den von Azure Storage unterstützten Diensttags finden Sie unter [Übersicht über Azure-Diensttags](../../virtual-network/service-tags-overview.md). |
| Unterstützung der VNet-Einschleusung| Nicht zutreffend |  |
| Netzwerkisolation und Firewallunterstützung| Ja | |
| Unterstützung der Tunnelerzwingung| – |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Azure Monitor-Metriken|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene | Ja | Azure-Aktivitätsprotokoll |
| Protokollierung und Überwachung auf Datenebene| Ja | Azure Monitor-Ressourcenprotokolle |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentifizierung| Ja | Azure Active Directory, gemeinsam verwendeter Schlüssel, SAS-Token. |
| Authorization| Ja | Unterstützung für die Autorisierung über rollenbasierte Zugriffssteuerung (RBAC), POSIX-Zugriffssteuerungslisten (ACLs) und SAS-Token |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützung für die Versionierung durch den Ressourcenanbieter über Azure Resource Manager-APIs |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../../security/fundamentals/security-controls.md).