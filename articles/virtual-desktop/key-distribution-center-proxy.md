---
title: 'Einrichten von Windows Virtual Desktop zur Verwendung eines Kerberos-Schlüsselverteilungscenter-Proxys: Azure'
description: Es wird beschrieben, wie Sie einen Windows Virtual Desktop-Hostpool für die Verwendung eines Kerberos-Schlüsselverteilungscenter-Proxys einrichten.
author: Heidilohr
ms.topic: how-to
ms.date: 01/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: f123659941eaeb3b8ceeb6a999abf836eb9cf5ea
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798004"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy"></a>Konfigurieren eines Kerberos-Schlüsselverteilungscenter-Proxys

In diesem Artikel wird veranschaulicht, wie Sie einen Kerberos-Schlüsselverteilungscenter-Proxy (Key Distribution Center, KDC) für Ihren Hostpool konfigurieren. Mit diesem Proxy können Organisationen außerhalb ihrer Unternehmensgrenzen die Authentifizierung für Kerberos durchführen. Beispielsweise können Sie den KDC-Proxy nutzen, um die Smartcard-Authentifizierung für externe Clients zu aktivieren.

## <a name="how-to-configure-the-kdc-proxy"></a>Konfigurieren des KDC-Proxys

Konfigurieren Sie den KDC-Proxy wie folgt:

1. Melden Sie sich beim Azure-Portal als Administrator an.

2. Navigieren Sie zur Seite „Windows Virtual Desktop“.

3. Wählen Sie den Hostpool, für den Sie den KDC-Proxy aktivieren möchten, und dann die Option **RDP-Eigenschaften** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Seite im Azure-Portal, auf der ein Benutzer die Option „Hostpools“, den Namen des Beispielhostpools und dann „RDP-Eigenschaften“ auswählt](media/rdp-properties.png)

4. Wählen Sie die Registerkarte **Erweitert** aus, und geben Sie dann einen Wert im folgenden Format ohne Leerzeichen ein:

    > kdcproxyname:s:\<fqdn\>

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Ausgewählte Registerkarte „Erweitert“ mit eingegebenem Wert (Schritt 4)](media/advanced-tab-selected.png)

5. Klicken Sie auf **Speichern**.

6. Der ausgewählte Hostpool sollte nun damit beginnen, RDP-Verbindungsdateien mit dem von Ihnen ausgefüllten Feld „kdcproxyname“ auszugeben.

## <a name="next-steps"></a>Nächste Schritte

Die Remotedesktopgateway-Rolle in Remotedesktopdienste enthält einen KDC-Proxydienst. Informationen zur Einrichtung als Ziel für Windows Virtual Desktop finden Sie unter [Bereitstellen der Remotedesktopgateway-Rolle in Windows Virtual Desktop](rd-gateway-role.md).
