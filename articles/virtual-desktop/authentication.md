---
title: 'Windows Virtual Desktop-Authentifizierung: Azure'
description: Authentifizierungsmethoden für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8328fa9c9e32a22e8e5093aa343f333cd1914ddc
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026120"
---
# <a name="supported-authentication-methods"></a>Unterstützte Authentifizierungsmethoden

In diesem Artikel erhalten Sie einen kurzen Überblick darüber, welche Arten von Authentifizierung Sie in Windows Virtual Desktop verwenden können.

## <a name="session-host-authentication"></a>Sitzungshostauthentifizierung

Windows Virtual Desktop unterstützt sowohl NTLM (NT LAN Manager) als auch Kerberos für Sitzungshostauthentifizierung. Zur Verwendung von Kerberos muss der Client allerdings Kerberos-Sicherheitstickets von einem KDC-Dienst (Key Distribution Center) abrufen, der auf einem Domänencontroller ausgeführt wird. Zum Abrufen von Tickets benötigt der Client eine direkte Sicht auf den Domänencontroller. Sie können eine direkte Sichtlinie einrichten, indem Sie Ihr Unternehmensnetzwerk verwenden. Sie können auch eine VPN-Verbindung mit Ihrem Unternehmensnetzwerk verwenden.

Dies sind die derzeit unterstützten Anmeldemethoden:

- Windows-Desktop-Client
    - Benutzername und Kennwort
    - Smartcard
    - Windows Hello
- Windows Store-Client
    - Benutzername und Kennwort
- Webclient
    - Benutzername und Kennwort
- Android
    - Benutzername und Kennwort
- iOS
    - Benutzername und Kennwort
- macOS
    - Benutzername und Kennwort

>[!NOTE]
>Smartcard und Windows Hello können für die Anmeldung nur Kerberos verwenden. Für die Anmeldung mit Kerberos ist eine Sichtlinie zum Domänencontroller erforderlich.

## <a name="single-sign-on-sso"></a>Einmaliges Anmelden (Single Sign-On, SSO)

Windows Virtual Desktop unterstützt derzeit keine Active Directory-Verbunddienste (AD FS) für Authentifizierung oder einmaliges Anmelden (SSO).

Die einzige Möglichkeit zu vermeiden, dass Sie zur Eingabe Ihrer Anmeldedaten für den Sitzungshost aufgefordert werden, besteht darin, diese im Client zu speichern. Wir empfehlen Ihnen, zu diesem Zweck nur sichere Geräte zu verwenden, um zu verhindern, dass andere Benutzer auf Ihre Ressourcen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Sind Sie neugierig auf andere Möglichkeiten, Ihre Bereitstellung sicher zu gestalten? Weitere Informationen finden Sie unter [Bewährte Methoden für Sicherheit](security-guide.md).
