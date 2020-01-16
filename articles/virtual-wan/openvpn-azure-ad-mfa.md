---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 9329ac33a771c829b8d67670bc07d9a353c58ff1
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930597"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Aktivieren von Azure Multi-Factor Authentication (MFA) für VPN-Benutzer

Wenn Sie möchten, dass Benutzer vor der Gewährung des Zugriffs zur Angabe eines zweiten Authentifizierungsfaktor aufgefordert werden, können Sie Azure Multi-Factor Authentication (MFA) für Ihren Azure AD-Mandanten konfigurieren. Die Schritte in diesem Artikel helfen Ihnen, eine Anforderung für die zweistufige Überprüfung zu aktivieren.

## <a name="prereq"></a>Voraussetzung

Voraussetzung für diese Konfiguration ist ein konfigurierter Azure AD-Mandant mithilfe der Schritte in [Konfigurieren eines Mandanten](openvpn-azure-ad-tenant.md).

## <a name="mfa"></a>Öffnen der MFA-Seite

1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zu **Azure Active Directory -> Alle Benutzer**.
3. Wählen Sie **Multi-Factor Authentication** aus, um die Seite für die mehrstufige Authentifizierung zu öffnen.

   ![Anmelden](./media/openvpn-azure-ad-mfa/mfa1.jpg)

## <a name="users"></a> Auswählen von Benutzern

1. Wählen Sie auf der Seite **Multi-Factor Authentication** die Benutzer aus, für die Sie MFA aktivieren möchten.
2. Wählen Sie **Aktivieren** aus.

   ![Select](./media/openvpn-azure-ad-mfa/mfa2.jpg)

## <a name="enableauth"></a>Aktivieren der Authentifizierung

1. Navigieren Sie zu **Azure Active Directory -> Unternehmensanwendungen -> Alle Anwendungen**.
2. Wählen Sie auf der Seite **Unternehmensanwendungen – Alle Anwendungen** die Option **Azure-VPN** aus.

   ![Verzeichnis-ID](./media/openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a> Konfigurieren von Anmeldeeinstellungen

Konfigurieren Sie auf der Seite **Azure-VPN – Eigenschaften** die Anmeldeeinstellungen.

1. Legen Sie **Aktiviert für die Benutzeranmeldung?** auf **Ja** fest. Dadurch können sich alle Benutzer im AD-Mandanten erfolgreich mit dem VPN verbinden.
2. Legen Sie **Benutzerzuweisung erforderlich?** auf **Ja** fest, wenn Sie die Anmeldung auf Benutzer beschränken möchten, die über Berechtigungen für das Azure-VPN verfügen.
3. Speichern Sie die Änderungen.

   ![Berechtigungen](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Informationen finden Sie unter [Konfigurieren der Azure AD-Authentifizierung für eine Point-to-Site-Verbindung mit Azure](virtual-wan-point-to-site-azure-ad.md).
