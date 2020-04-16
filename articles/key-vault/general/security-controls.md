---
title: Sicherheitskontrollen für Azure Key Vault
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure Key Vault
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: cd6602f68b63e2c236e7f3905d33b88fbda36ed2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427026"
---
# <a name="security-controls-for-azure-key-vault"></a>Sicherheitskontrollen für Azure Key Vault

In diesem Artikel werden die in Azure Key Vault integrierten Sicherheitskontrollen beschrieben. 

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Mithilfe von Dienstendpunkten im virtuellen Netzwerk (VNET) |
| Unterstützung der VNet-Einschleusung| Nein |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Mithilfe von VNET-Firewallregeln |
| Unterstützung der Tunnelerzwingung| Nein |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Mithilfe von Log Analytics |
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |
| Protokollierung und Überwachung auf Datenebene| Ja | Mithilfe von Log Analytics |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Authorization| Ja | Mithilfe der Schlüsseltresor-Zugriffsrichtlinie |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Alle Objekte werden verschlüsselt. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja | Der Kunde überwacht alle Schlüssel im Schlüsseltresor. Wenn vom Hardwaresicherheitsmodul (HSM) unterstützte Schlüssel angegeben werden, bietet ein FIPS Level 2 HSM Schutz für Schlüssel, Zertifikate oder Geheimnisse. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Die gesamte Kommunikation erfolgt über verschlüsselte API-Aufrufe. |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS |

## <a name="access-controls"></a>Zugriffssteuerung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Zugriffskontrollen | Ja | Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene – Zugriffskontrollen (auf jeder Dienstebene) | Ja | Schlüsseltresor-Zugriffsrichtlinie |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../../security/fundamentals/security-controls.md).