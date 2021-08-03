---
title: 'Azure Virtual Desktop-Authentifizierung: Azure'
description: Authentifizierungsmethoden für Azure Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/26/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: bb5341f1f80350ce14b99a6ca5cf27343bd20675
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111745383"
---
# <a name="supported-authentication-methods"></a>Unterstützte Authentifizierungsmethoden

In diesem Artikel erhalten Sie einen kurzen Überblick darüber, welche Arten von Authentifizierung Sie in Azure Virtual Desktop verwenden können.

## <a name="session-host-authentication"></a>Sitzungshostauthentifizierung

Azure Virtual Desktop unterstützt sowohl NTLM (NT LAN Manager) als auch Kerberos für Sitzungshostauthentifizierung. Zur Verwendung von Kerberos muss der Client allerdings Kerberos-Sicherheitstickets von einem KDC-Dienst (Key Distribution Center) abrufen, der auf einem Domänencontroller ausgeführt wird. Zum Abrufen von Tickets benötigt der Client eine direkte Sicht auf den Domänencontroller. Sie können eine direkte Sichtlinie einrichten, indem Sie Ihr Unternehmensnetzwerk verwenden. Sie können auch eine VPN-Verbindung zu Ihrem Unternehmensnetzwerk verwenden oder einen [KDC-Proxyserver](key-distribution-center-proxy.md) einrichten.

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

Azure Virtual Desktop unterstützt [Hybrididentitäten](../active-directory/hybrid/whatis-hybrid-identity.md) über Azure Active Directory (AD), einschließlich solcher Identitäten, die mit Active Directory-Verbunddienste (AD FS) zum Verbund erstellt wurden. Da Benutzer über Azure AD auffindbar sein müssen, unterstützt Azure Virtual Desktop keine eigenständigen Active Directory-Bereitstellungen mit ADFS.

## <a name="single-sign-on-sso"></a>Einmaliges Anmelden (Single Sign-On, SSO)

Azure Virtual Desktop unterstützt das [einmalige Anmelden mittels Active Directory-Verbunddienste (ADFS)](configure-adfs-sso.md) für Windows- und Webclients.

Die einzige Möglichkeit zu vermeiden, dass Sie zur Eingabe Ihrer Anmeldedaten für den Sitzungshost aufgefordert werden, besteht andernfalls darin, diese im Client zu speichern. Wir empfehlen Ihnen, zu diesem Zweck nur sichere Geräte zu verwenden, um zu verhindern, dass andere Benutzer auf Ihre Ressourcen zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Sind Sie neugierig auf andere Möglichkeiten, Ihre Bereitstellung sicher zu gestalten? Weitere Informationen finden Sie unter [Bewährte Methoden für Sicherheit](security-guide.md).
