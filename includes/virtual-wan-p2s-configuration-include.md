---
author: cherylmc
ms.author: cherylmc
ms.date: 02/23/2021
ms.service: virtual-wan
ms.topic: include
ms.openlocfilehash: 567c0bb75c30a1f0ccdcde7ec1b0f04f5d6e54c5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102048255"
---
[!INCLUDE [Portal feature rollout](virtual-wan-portal-feature-rollout.md)]

1. Navigieren Sie zu **Alle Ressourcen**, und wählen Sie das von Ihnen erstellte virtuelle WAN und anschließend **Benutzer-VPN-Konfigurationen** im linken Menü aus.
1. Wählen Sie auf der Seite **Benutzer-VPN-Konfigurationen** oben auf der Seite die Option **+ Benutzer-VPN-Konfiguration erstellen** aus, um die Seite **Neue Benutzer-VPN-Konfiguration erstellen** zu öffnen.

   :::image type="content" source="media/virtual-wan-p2s-configuration/user-vpn.png" alt-text="Screenshot der Seite „Benutzer-VPN-Konfigurationen“":::

1. Geben Sie auf der Registerkarte **Basic** (Grundlagen) unter **Instanzdetails** den **Namen** ein, den Sie Ihrer VPN-Konfiguration zuweisen möchten.
1. Wählen Sie für den **Tunneltyp** über das Dropdownmenü den Tunneltyp aus, den Sie verwenden möchten. Die Optionen für den Tunneltyp sind die folgenden: **IKEv2-VPN**, **OpenVpn** und **IkeV2**.
1. Führen Sie die folgenden Schritte aus, die dem ausgewählten Tunneltyp entsprechen. Wenn alle Werte angegeben sind, klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**, um die Konfiguration zu erstellen.

   **IKEv2 VPN**

   * **Anforderungen:** Wenn Sie den Tunneltyp **IKEv2** auswählen, wird eine Meldung angezeigt, die Sie auffordert, eine Authentifizierungsmethode auszuwählen. Bei IKEv2 können Sie nur eine Authentifizierungsmethode angeben. Sie können Azure Certificate, Azure Active Directory oder eine RADIUS-basierte Authentifizierung auswählen.

   * **Benutzerdefinierte IPsec-Parameter:** Wenn Sie die Parameter für IKE Phase 1 und IKE Phase 2 anpassen möchten, legen Sie den IPsec-Umschalter auf **Custom** (Benutzerdefiniert) fest, und wählen Sie die Parameterwerte aus. Weitere Informationen zu anpassbaren Parametern finden Sie im Artikel zu den [Benutzerdefinierten IPsec-Richtlinien](../articles/virtual-wan/point-to-site-ipsec.md).

     :::image type="content" source="media/virtual-wan-p2s-configuration/custom.png" alt-text="Screenshot des IPsec-Umschalters, der auf „custom“ (benutzerdefiniert) festgelegt ist":::

   * **Authentifizierung:** Navigieren Sie zum Authentifizierungsmechanismus, den Sie verwenden möchten, und klicken Sie entweder ganz unten auf der Seite auf **Weiter**, um zur Authentifizierungsmethode zu wechseln, oder klicken Sie auf die entsprechende Registerkarte oben auf der Seite. Verschieben den Umschalter auf **Ja**, um die Methode auszuwählen.

     In diesem Beispiel ist die RADIUS-Authentifizierung ausgewählt. Bei der RADIUS-basierten Authentifizierung können Sie eine sekundäre IP-Adresse des RADIUS-Servers und ein Servergeheimnis bereitstellen.

     :::image type="content" source="media/virtual-wan-p2s-configuration/ike-radius.png" alt-text="Screenshot von IKE":::

   **OpenVPN**

   * **Anforderungen:** Wenn Sie den **OpenVPN**-Tunneltyp auswählen, wird eine Meldung angezeigt, die Sie auffordert, einen Authentifizierungsmechanismus auszuwählen. Wenn OpenVPN als Tunneltyp ausgewählt ist, können Sie mehrere Authentifizierungsmethoden angeben. Sie können eine beliebige Teilmenge von Azure Certificate, Azure Active Directory oder eine RADIUS-basierte Authentifizierung auswählen. Bei der RADIUS-basierten Authentifizierung können Sie eine sekundäre IP-Adresse des RADIUS-Servers und ein Servergeheimnis bereitstellen.

   * **Authentifizierung:** Navigieren Sie zur Authentifizierungsmethode, die Sie verwenden möchten, und klicken Sie entweder ganz unten auf der Seite auf **Weiter**, um zur Authentifizierungsmethode zu wechseln, oder klicken Sie auf die entsprechende Registerkarte oben auf der Seite.
   Schieben Sie für jede Methode, die Sie auswählen möchten, den Umschalter auf **Ja**, und geben Sie die entsprechenden Werte ein.

     In diesem Beispiel ist Azure Active Directory ausgewählt.

     :::image type="content" source="media/virtual-wan-p2s-configuration/openvpn.png" alt-text="Screenshot der OpenVPN-Seite":::
