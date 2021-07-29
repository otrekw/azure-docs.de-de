---
title: Einrichten von Kerberos-Schlüsselverteilungscenter-Proxys für Azure Virtual Desktop – Azure
description: So richten Sie einen Azure Virtual Desktop-Hostpool für die Verwendung eines Kerberos-Schlüsselverteilungscenter-Proxys ein.
author: Heidilohr
ms.topic: how-to
ms.date: 05/04/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 55c68902cebbb6832a9c09c5390d9f43d381bf21
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111757551"
---
# <a name="configure-a-kerberos-key-distribution-center-proxy"></a>Konfigurieren eines Kerberos-Schlüsselverteilungscenter-Proxys

Sicherheitsbewusste Kunden, z. B. Finanz- oder Regierungsorganisationen, verwenden häufig die Anmeldung mithilfe von Smartcards. Smartcards erhöhen die Sicherheit von Bereitstellungen, da sie eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) verlangen. Für den RDP-Teil einer Azure Virtual Desktop-Sitzung ist bei Smartcards jedoch für die Kerberos-Authentifizierung eine direkte Verbindung („Line of Sight“) mit einem AD-Domänencontroller (Active Directory) erforderlich. Ohne diese direkte Verbindung können sich Benutzer nicht automatisch über Remoteverbindungen beim Netzwerk der Organisation anmelden. Benutzer in einer Azure Virtual Desktop-Bereitstellung können den KDC-Proxydienst verwenden, um einen Proxy für diesen Authentifizierungsdatenverkehr einzurichten und sich per Remotezugang anzumelden. Der KDC-Proxy ermöglicht die Authentifizierung für das Remotedesktopprotokoll einer Azure Virtual Desktop-Sitzung, sodass Benutzer sich sicher anmelden können. Dies vereinfacht eine Arbeit von zu Hause, und ermöglicht eine reibungslosere Durchführung bestimmter Notfallwiederherstellungsszenarien.

Das Einrichten des KDC-Proxys umfasst in der Regel das Zuweisen der Windows Server-Gatewayrolle in Windows Server 2016 oder höher. Wie kann eine Remotedesktopdienste-Rolle zum Anmelden bei Azure Virtual Desktop verwendet werden? Um dies zu beantworten, werfen wir einen kurzen Blick auf die Komponenten.

Es gibt zwei Komponenten für den virtuellen Azure Virtual Desktop-Dienst, die authentifiziert werden müssen:

- Der Feed im Azure Virtual Desktop-Client, mit dem Benutzer eine Liste der verfügbaren Desktops oder Anwendungen erhalten, auf die sie Zugriff haben. Dieser Authentifizierungsvorgang erfolgt in Azure Active Directory. Das bedeutet, dass diese Komponente nicht im Mittelpunkt dieses Artikels steht.
- Die RDP-Sitzung, die sich daraus ergibt, dass ein Benutzer eine dieser verfügbaren Ressourcen auswählt. Diese Komponente verwendet die Kerberos-Authentifizierung und erfordert einen KDC-Proxy für Remotebenutzer.

In diesem Artikel erfahren Sie, wie Sie den Feed im Azure Virtual Desktop-Client im Azure-Portal konfigurieren. Informationen zum Konfigurieren der RD-Gateway-Rolle finden Sie unter [Bereitstellen der RD-Gateway-Rolle](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).

## <a name="requirements"></a>Anforderungen

Zum Konfigurieren eines Azure Virtual Desktop-Sitzungshosts mit einem KDC-Proxy benötigen Sie Folgendes:

- Zugriff auf das Azure-Portal und ein Azure-Administratorkonto.
- Auf den Remoteclientcomputern muss entweder Windows 10 oder Windows 7 ausgeführt werden, und der [Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop) muss installiert sein. Derzeit wird der Webclient nicht unterstützt.
- Auf dem Computer muss bereits ein KDC-Proxy installiert sein. Weitere Informationen hierzu finden Sie unter [Einrichten der RD-Gateway-Rolle für Azure Virtual Desktop](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).
- Betriebssystem des Computers muss Windows Server 2016 oder höher sein.

Nachdem Sie sichergestellt haben, dass Sie diese Anforderungen erfüllen, kann es losgehen.

## <a name="how-to-configure-the-kdc-proxy"></a>Konfigurieren des KDC-Proxys

Konfigurieren Sie den KDC-Proxy wie folgt:

1. Melden Sie sich beim Azure-Portal als Administrator an.

2. Navigieren Sie zur Seite „Azure Virtual Desktop“.

3. Wählen Sie den Hostpool, für den Sie den KDC-Proxy aktivieren möchten, und dann die Option **RDP-Eigenschaften** aus.

4. Wählen Sie die Registerkarte **Erweitert** aus, und geben Sie dann einen Wert im folgenden Format ohne Leerzeichen ein:

    
    > kdcproxyname:s:\<fqdn\>
    

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Ausgewählte Registerkarte „Erweitert“ mit eingegebenem Wert (Schritt 4)](media/advanced-tab-selected.png)

5. Klicken Sie auf **Speichern**.

6. Der ausgewählte Hostpool sollte nun damit beginnen, RDP-Verbindungsdateien mit dem von Ihnen in Schritt 4 eingegebenen Wert für „kdcproxyname“ auszugeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie den KDC-Proxy hinsichtlich Remotedesktopdienste verwalten und die RD-Gateway-Rolle zuweisen, finden Sie unter [Bereitstellen der RD-Gateway-Rolle](/windows-server/remote/remote-desktop-services/remote-desktop-gateway-role).

Wenn Sie Ihre KDC-Proxyserver skalieren möchten, finden Sie weitere Informationen zum Einrichten der Hochverfügbarkeit für den KDC-Proxy unter [Hinzufügen von Hochverfügbarkeit zu RD-Web und RD-Gateway (Webfront)](/windows-server/remote/remote-desktop-services/rds-rdweb-gateway-ha).