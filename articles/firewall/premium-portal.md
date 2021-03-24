---
title: Vorschauversion von Azure Firewall Premium im Azure-Portal
description: Erfahren Sie mehr über die Vorschauversion von Azure Firewall Premium im Azure-Portal.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: 3d56fc73faeb0d48ba7e5b21449c61d6213afa40
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100549444"
---
# <a name="azure-firewall-premium-preview-in-the-azure-portal"></a>Vorschauversion von Azure Firewall Premium im Azure-Portal

> [!IMPORTANT]
> Azure Firewall Premium ist zurzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 Die Vorschauversion von Azure Firewall Premium ist eine Firewall der nächsten Generation mit Funktionen, die für streng vertrauliche und regulierte Umgebungen erforderlich sind. Es umfasst die folgenden Features:

- **TLS-Inspektion** – Entschlüsselt den ausgehenden Datenverkehr, verarbeitet die Daten, verschlüsselt sie dann und sendet sie an das Ziel.
- **IDPS** – Ein System zur Erkennung und Verhinderung von Eindringversuchen in Netzwerke (IDPS) ermöglicht es Ihnen, Netzwerkaktivitäten auf schädliche Aktivitäten zu überwachen, Informationen über diese Aktivitäten zu protokollieren, sie zu melden und optional zu versuchen, sie zu blockieren.
- **URL-Filterung** – Erweitert die FQDN-Filterfunktion von Azure Firewall, um eine gesamte URL zu berücksichtigen. Beispiel: `www.contoso.com/a/c` anstelle von `www.contoso.com`.
- **Webkategorien** – Administratoren können den Benutzerzugriff auf Websitekategorien wie Gaming- oder Social Media-Websites zulassen oder verweigern.

Weitere Informationen finden Sie unter [Azure Firewall Premium-Features](premium-features.md).

## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

Die Bereitstellung einer Vorschauversion von Azure Firewall Premium ist ähnlich wie die Bereitstellung einer standardmäßigen Azure Firewall:

:::image type="content" source="media/premium-portal/premium-portal.png" alt-text="Bereitstellung über das Portal":::

Für den **Firewalltarif** wählen Sie **Premium (Vorschau)** und für die **Firewallrichtlinie** wählen Sie eine vorhandene Premium-Richtlinie aus oder erstellen eine neue.

## <a name="configure-the-premium-policy"></a>Konfigurieren der Premium-Richtlinie

Das Konfigurieren einer Premium-Firewallrichtlinie ähnelt dem Konfigurieren einer Standard-Firewallrichtlinie. Mit einer Premium-Richtlinie können Sie die Premium-Features konfigurieren:

:::image type="content" source="media/premium-portal/premium-policy.png" alt-text="Premium-Richtlinie: Bereitstellung":::

### <a name="rule-configuration"></a>Regelkonfiguration

Wenn Sie Anwendungsregeln in einer Premium-Richtlinie konfigurieren, können Sie zusätzliche Premium-Features konfigurieren:

:::image type="content" source="media/premium-portal/premium-application-rule.png" alt-text="Premium-Regel":::

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den Azure Firewall Premium-Features der Vorschauversion finden Sie unter [Bereitstellen und Konfigurieren der Vorschauversion von Azure Firewall Premium](premium-deploy.md).