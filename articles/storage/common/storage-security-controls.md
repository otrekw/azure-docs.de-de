---
title: Sicherheitskontrollen für Azure Storage
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure Storage
services: storage
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.service: storage
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2cc54077456fce1e7e0f47843a762beee8e715f7
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526755"
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
| Unterstützung der VNet-Einschleusung| – |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | |
| Unterstützung der Tunnelerzwingung| – |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Azure Monitor-Metriken|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene | Ja | Azure Resource Manager-Aktivitätsprotokoll |
| Protokollierung und Überwachung auf Datenebene| Ja | Dienstdiagnoseprotokolle|

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