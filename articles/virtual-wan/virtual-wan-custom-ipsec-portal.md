---
title: 'Konfigurieren einer benutzerdefinierten IPsec-Richtlinie für Azure Virtual WAN: Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie eine benutzerdefinierte IPsec-Richtlinie für Azure Virtual WAN über das Portal konfigurieren.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 0ea4523d1558f6887e1aef344198026591dac617
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84752611"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Konfigurieren einer benutzerdefinierten IPsec-Richtlinie für Virtual WAN über das Portal

Sie können eine benutzerdefinierte IPsec-Richtlinie für Virtual WAN über das Azure-Portal konfigurieren. Benutzerdefinierte Richtlinien sind hilfreich, wenn beide Seiten (lokale und Azure-VPN-Gateways) die gleichen Einstellungen für IKE Phase 1 und IKE Phase 2 verwenden sollen.

## <a name="working-with-custom-policies"></a>Arbeiten mit benutzerdefinierten Richtlinien

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>Konfigurieren einer Richtlinie

1. **Suchen Sie den virtuellen Hub.** Navigieren Sie in einem Browser zum [Azure-Portal](https://aka.ms/azurevirtualwanpreviewfeatures) , und melden Sie sich mit Ihrem Azure-Konto an. Suchen Sie den virtuellen Hub für Ihre Website.
2. **Wählen Sie den VPN-Standort aus.** Wählen Sie auf der Seite für den Hub den VPN-Standort aus, für den Sie eine benutzerdefinierte Richtlinie einrichten möchten.

   ![select](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **Bearbeiten Sie die VPN-Verbindung.** Wählen Sie im **Kontextmenü** zuerst **...** und dann  **VPN-Verbindung bearbeiten** aus.

   ![Bearbeiten](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Konfigurieren Sie die Einstellungen.** Konfigurieren Sie auf der Seite **VPN-Verbindung bearbeiten** die Einstellungen. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

   ![Konfigurieren und speichern](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).