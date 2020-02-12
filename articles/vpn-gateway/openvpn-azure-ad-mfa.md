---
title: 'Aktivieren Sie MFA für VPN-Benutzer: Azure AD-Authentifizierung'
description: Aktivieren der mehrstufigen Authentifizierung für VPN-Benutzer
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 771dea2d9ae2979bc71880368ed3a9a538e8a803
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964709"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Aktivieren von Azure Multi-Factor Authentication (MFA) für VPN-Benutzer

Wenn Benutzer vor der Zugriffsgewährung zu einem zweiten Authentifizierungsfaktor aufgefordert werden sollen, können Sie Azure Multi-Factor Authentication (MFA) auf Benutzerbasis konfigurieren oder Multi-Factor Authentication (MFA) über [bedingten Zugriff](../active-directory/conditional-access/overview.md) für eine differenziertere Kontrolle nutzen. Die Konfiguration von Multi-Factor Authentication auf Benutzerbasis kann ohne zusätzliche Kosten aktiviert werden. Wenn jedoch MFA auf Benutzerbasis aktiviert wird, wird der Benutzer zur Authentifizierung mit dem zweiten Faktor für alle Anwendungen aufgefordert, die an den Azure AD-Mandanten gebunden sind. Der bedingte Zugriff ermöglicht eine differenziertere Kontrolle darüber, wie ein zweiter Faktor höher gestuft werden soll, und kann die Zuweisung von MFA nur an VPN und nicht an andere Anwendungen, die an den Azure AD-Mandanten gebunden sind, ermöglichen.

## <a name="enableauth"></a>Aktivieren der Authentifizierung

1. Navigieren Sie zu **Azure Active Directory -> Unternehmensanwendungen -> Alle Anwendungen**.
2. Wählen Sie auf der Seite **Unternehmensanwendungen – Alle Anwendungen** die Option **Azure-VPN** aus.

   ![Verzeichnis-ID](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a> Konfigurieren von Anmeldeeinstellungen

Konfigurieren Sie auf der Seite **Azure-VPN – Eigenschaften** die Anmeldeeinstellungen.

1. Legen Sie **Aktiviert für die Benutzeranmeldung?** auf **Ja** fest. Dadurch können sich alle Benutzer im AD-Mandanten erfolgreich mit dem VPN verbinden.
2. Legen Sie **Benutzerzuweisung erforderlich?** auf **Ja** fest, wenn Sie die Anmeldung auf Benutzer beschränken möchten, die über Berechtigungen für das Azure-VPN verfügen.
3. Speichern Sie die Änderungen.

   ![Berechtigungen](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="option-1---enable-multi-factor-authentication-mfa-via-conditional-access"></a>Option 1 – Aktivieren von Multi-Factor Authentication (MFA) über bedingten Zugriff

Der bedingte Zugriff ermöglicht eine differenzierte Zugriffssteuerung auf Anwendungsbasis.  Beachten Sie, dass Sie für die Nutzung des bedingten Zugriffs eine Azure AD Premium 1-Lizenz oder höher auf die Benutzer, die den Regeln des bedingten Zugriffs unterliegen, anwenden sollten.

1. Wählen Sie auf der Seite **Unternehmensanwendungen – Alle Anwendungen** die Option **Azure-VPN** und dann **Bedingter Zugriff** aus, und klicken Sie anschließend auf **Neue Richtlinie**.
2. Aktivieren Sie unter „Benutzer und Gruppen“ auf der Registerkarte *Einschließen* die Option **Benutzer und Gruppen auswählen**, aktivieren Sie **Benutzer und Gruppen**, und wählen Sie eine Gruppe oder einen Satz von Benutzern aus, die MFA unterliegen sollen.  Klicken Sie auf **Fertig**.
![Zuweisungen](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. Aktivieren Sie unter **Erteilen** die Option **Zugriff gewähren** sowie die Optionen **Multi-Factor Authentication erforderlich** und **Alle ausgewählten Kontrollen anfordern**, und klicken Sie anschließend auf die Schaltfläche **Auswählen**.
![Zugriff gewähren – MFA](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. Aktivieren Sie **Ein** unter **Richtlinie aktivieren**, und klicken Sie auf die Schaltfläche **Erstellen**.
![Richtlinie aktivieren](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)

## <a name="option-2---enable-multi-factor-authentication-mfa-per-user"></a>Option 2 – Aktivieren von Multi-Factor Authentication (MFA) auf Benutzerbasis

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="next-steps"></a>Nächste Schritte

Um eine Verbindung mit Ihrem virtuellen Netzwerk herzustellen, müssen Sie ein VPN-Clientprofil erstellen und konfigurieren. Entsprechende Informationen finden Sie unter [Konfigurieren eines VPN-Clients für P2S-VPN-Verbindungen](openvpn-azure-ad-client.md).
