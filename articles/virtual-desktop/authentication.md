---
title: 'Windows Virtual Desktop-Authentifizierung: Azure'
description: Authentifizierungsmethoden für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 04a4366bfee6b1d9c5f52d649910163269962684
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101709256"
---
# <a name="supported-authentication-methods"></a>Unterstützte Authentifizierungsmethoden

In diesem Artikel erhalten Sie einen kurzen Überblick darüber, welche Arten von Authentifizierung Sie in Windows Virtual Desktop verwenden können.

## <a name="session-host-authentication"></a>Sitzungshostauthentifizierung

Windows Virtual Desktop unterstützt sowohl NTLM (NT LAN Manager) als auch Kerberos für Sitzungshostauthentifizierung. Zur Verwendung von Kerberos muss der Client allerdings Kerberos-Sicherheitstickets von einem KDC-Dienst (Key Distribution Center) abrufen, der auf einem Domänencontroller ausgeführt wird. Zum Abrufen von Tickets benötigt der Client eine direkte Sicht auf den Domänencontroller. Sie können eine direkte Sichtlinie einrichten, indem Sie Ihr Unternehmensnetzwerk verwenden. Sie können auch eine VPN-Verbindung zu Ihrem Unternehmensnetzwerk verwenden oder einen [KDC-Proxyserver](key-distribution-center-proxy.md) einrichten.

Dies sind die derzeit unterstützten Anmeldemethoden:

- Windows-Desktop-Client
    - Benutzername und Kennwort
    - Smartcard
    - Windows Hello for Business (nur Zertifikatvertrauensstellung)
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
>Smartcard und Windows Hello for Business können nur Kerberos für die Anmeldung verwenden. Für die Anmeldung mit Kerberos ist eine Sichtverbindung zum Domänencontroller oder einem [KDC-Proxyserver](key-distribution-center-proxy.md) erforderlich.

## <a name="hybrid-identity"></a>Hybrididentität

Windows Virtual Desktop unterstützt [Hybrididentitäten](../active-directory/hybrid/whatis-hybrid-identity.md) über Azure Active Directory (AD), einschließlich solcher Identitäten, die mit Active Directory-Verbunddienste (AD FS) zum Verbund erstellt wurden. Da Benutzer über Azure AD auffindbar sein müssen, unterstützt Windows Virtual Desktop keine eigenständigen Active Directory Bereitstellungen mit ADFS.

## <a name="single-sign-on-sso"></a>Einmaliges Anmelden (Single Sign-On, SSO)

Windows Virtual Desktop unterstützt derzeit keine Active Directory-Verbunddienste (AD FS) für einmaliges Anmelden (SSO).

Die einzige Möglichkeit zu vermeiden, dass Sie zur Eingabe Ihrer Anmeldedaten für den Sitzungshost aufgefordert werden, besteht darin, diese im Client zu speichern. Wir empfehlen Ihnen, zu diesem Zweck nur sichere Geräte zu verwenden, um zu verhindern, dass andere Benutzer auf Ihre Ressourcen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Sind Sie neugierig auf andere Möglichkeiten, Ihre Bereitstellung sicher zu gestalten? Weitere Informationen finden Sie unter [Bewährte Methoden für Sicherheit](security-guide.md).
