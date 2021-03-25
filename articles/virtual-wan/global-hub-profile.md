---
title: Herunterladen von globalen oder hubbasierten VPN-Profilen in Azure Virtual WAN | Microsoft-Dokumentation
description: Erfahren Sie mehr über die zwei Arten von Konnektivität, die Azure Virtual WAN für Remotebenutzer bietet, und das Herunterladen eines Profils.
services: virtual-wan
author: kumudD
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: alzam
ms.openlocfilehash: 6d5c4ba0f1f55119d1ec38296e67ae3e90c52650
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91313721"
---
# <a name="download-a-global-or-hub-based-profile-for-user-vpn-clients"></a>Herunterladen eines globalen oder hubbasierten Profils für Benutzer-VPN-Clients

Azure Virtual WAN bietet zwei Arten von Konnektivität für Remotebenutzer: Global und hubbasiert. In den folgenden Abschnitten können Sie mehr über ein Profil erfahren und es herunterladen. 

> [!IMPORTANT]
> Die RADIUS-Authentifizierung unterstützt nur das hubbasierte Profil.

## <a name="global-profile"></a>Globales Profil

Das Profil verweist auf einen Lastenausgleich, der alle aktiven Benutzer-VPN-Hubs umfasst. Der Benutzer wird zu dem Hub weitergeleitet, der dem geografischen Standort des Benutzers am nächsten liegt. Diese Art der Konnektivität ist hilfreich, wenn Benutzer häufig an verschiedene Standorte reisen. So laden Sie das **globale** Profil herunter

1. Navigieren Sie zum virtuellen WAN.
2. Klicken Sie auf **Benutzer-VPN-Konfiguration**.
3. Markieren Sie die Konfiguration, für die Sie das Profil herunterladen möchten.
4. Klicken Sie auf **VPN-Profil für Virtual WAN-Benutzer herunterladen**.

   ![Globales Profil](./media/global-hub-profile/global1.png)

## <a name="hub-based-profile"></a>Hubbasiertes Profil

Das Profil verweist auf einen einzelnen Hub. Der Benutzer kann mit diesem Profil nur eine Verbindung mit dem bestimmten Hub herstellen. So laden Sie das **hubbasierte** Profil herunter

1. Navigieren Sie zum virtuellen WAN.
2. Klicken Sie auf der Seite „Übersicht“ auf **Hub**.

    ![Hubprofil 1](./media/global-hub-profile/hub1.png)
3. Klicken Sie auf **Benutzer-VPN (Point-to-Site)** .
4. Klicken Sie auf **VPN-Profil für Benutzer des virtuellen Hubs herunterladen**.

   ![Hubprofil 2](./media/global-hub-profile/hub2.png)
5. Aktivieren Sie **EAPTLS**.
6. Klicken Sie auf **Profil generieren und herunterladen**.

   ![Hubprofil 3](./media/global-hub-profile/download.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
