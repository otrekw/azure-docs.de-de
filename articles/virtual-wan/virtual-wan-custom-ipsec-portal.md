---
title: 'Konfigurieren einer benutzerdefinierten IPsec-Richtlinie für Azure Virtual WAN: Portal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie eine benutzerdefinierte IPsec-Richtlinie für Azure Virtual WAN über das Portal konfigurieren.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 07eae453441ea8bff81d7cdb60f9c46c08a22829
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91851171"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Konfigurieren einer benutzerdefinierten IPsec-Richtlinie für Virtual WAN über das Portal

Sie können eine benutzerdefinierte IPsec-Richtlinie für eine Virtual WAN-VPN-Verbindung über das Azure-Portal konfigurieren. Benutzerdefinierte Richtlinien sind hilfreich, wenn beide Seiten (lokale und Azure-VPN-Gateways) die gleichen Einstellungen für IKE Phase 1 und IKE Phase 2 verwenden sollen.

## <a name="working-with-custom-policies"></a>Arbeiten mit benutzerdefinierten Richtlinien

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>Konfigurieren einer Richtlinie

1. **Suchen Sie den virtuellen Hub.** Navigieren Sie in einem Browser zum [Azure-Portal](https://aka.ms/azurevirtualwanpreviewfeatures) , und melden Sie sich mit Ihrem Azure-Konto an. Navigieren Sie zu Ihrer Virtual WAN-Ressource, und suchen Sie den virtuellen Hub, mit dem Ihr VPN-Standort verbunden ist.
2. **Wählen Sie den VPN-Standort aus.** Klicken Sie auf der Seite „Übersicht“ des Hubs auf **VPN (Site-to-Site)** , und wählen Sie den VPN-Standort aus, für den Sie eine benutzerdefinierte IPSec-Richtlinie einrichten möchten.

   ![select](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **Bearbeiten Sie die VPN-Verbindung.** Wählen Sie im **Kontextmenü** zuerst **...** und dann  **VPN-Verbindung bearbeiten** aus.

   ![Bearbeiten](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Konfigurieren Sie die Einstellungen.** Ändern Sie auf der Seite **VPN-Verbindung bearbeiten** die IPSec-Einstellung von der Standardeinstellung in „Benutzerdefiniert“, und passen Sie die IPSec-Richtlinie an. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.

   ![Konfigurieren und speichern](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).