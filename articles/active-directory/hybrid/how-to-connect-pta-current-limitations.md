---
title: 'Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure Active Directory-Passthrough-Authentifizierung (Azure AD).
services: active-directory
keywords: Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7db9a12a65cf127cb4c65e24128181ab6c0cb485
ms.sourcegitcommit: 8942cdce0108372d6fc5819c71f7f3cf2f02dc60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2021
ms.locfileid: "113135030"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Die folgenden Szenarien werden unterstützt:

- Benutzeranmeldungen bei webbrowserbasierten Anwendungen.
- Benutzeranmeldungen bei Outlook-Clients über ältere Protokolle wie Exchange ActiveSync, EAS, SMTP, POP und IMAP.
- Benutzeranmeldungen bei älteren Office-Clientanwendungen und Office-Anwendungen, die die [moderne Authentifizierung](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview) unterstützen: Versionen Office 2013 und 2016.
- Benutzeranmeldungen bei älteren Protokollanwendungen, z.B. PowerShell Version 1.0 und andere.
- Azure AD-Joins für Windows 10-Geräte.
- App-Kennwörter für Multi-Factor Authentication

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden _nicht_ unterstützt:

- Erkennen von Benutzern mit [kompromittierten Anmeldeinformationen](../identity-protection/overview-identity-protection.md).
- Für Azure AD Domain Services muss Kennworthashsynchronisierung auf dem Mandanten aktiviert sein. Aus diesem Grund funktionieren Mandanten, die Pass-Through-Authentifizierung verwenden, _nur_ in Szenarien nicht, die Azure AD Domain Services benötigen.
- Pass-Through Authentifizierung ist nicht in [Azure AD Connect Health](./whatis-azure-ad-connect.md) integriert.

> [!IMPORTANT]
> Zur _ausschließlichen_ Umgehung nicht unterstützter Szenarien (Azure AD Connect Health-Integration ausgenommen) können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](how-to-connect-install-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren.
> 
> [!NOTE]
> Durch das Aktivieren der Kennworthashsynchronisierung erhalten Sie die Möglichkeit einer Failoverauthentifizierung, wenn Ihre lokale Infrastruktur unterbrochen wird. Dieses Failover von Passthrough-Authentifizierung zu Kennworthashsynchronisierung erfolgt nicht automatisch. Sie müssen die Anmeldemethode manuell mit Azure AD Connect wechseln. Wenn der Server, auf dem Azure AD Connect ausgeführt wird, ausfällt, benötigen Sie die Hilfe des Microsoft-Support, um die Pass-Through-Authentifizierung zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
- [Schnellstart:](how-to-connect-pta-quick-start.md) Aktivieren und Ausführen der Passthrough-Authentifizierung von Azure AD
- [Migrieren von AD FS zur Passthrough-Authentifizierung](https://aka.ms/ADFSTOPTADPDownload): Ein detaillierter Leitfaden zur Migration von AD FS (oder anderen Verbundtechnologien) zur Passthrough-Authentifizierung
- [Smart Lockout](../authentication/howto-password-smart-lockout.md): Erfahren Sie, wie Sie die Smart Lockout-Funktion für Ihren Mandanten zum Schutz von Benutzerkonten konfigurieren.
- [Technische Einzelheiten](how-to-connect-pta-how-it-works.md): Hier finden Sie Informationen zur Funktionsweise der Passthrough-Authentifizierung.
- [Häufig gestellte Fragen:](how-to-connect-pta-faq.yml) Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung
- [Problembehandlung](tshoot-connect-pass-through-authentication.md): Hier finden Sie Informationen zum Beheben von allgemeinen Problemen, die bei der Passthrough-Authentifizierung auftreten können.
- [Ausführliche Informationen zur Sicherheit](how-to-connect-pta-security-deep-dive.md): Hier erhalten Sie ausführliche technische Informationen zur Pass-Through-Authentifizierung.
- [Nahtloses einmaliges Anmelden mit Azure AD:](how-to-connect-sso.md) Erfahren Sie mehr über diese Ergänzungsfunktion.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Fordern Sie neue Features über das Azure Active Directory-Forum an.
