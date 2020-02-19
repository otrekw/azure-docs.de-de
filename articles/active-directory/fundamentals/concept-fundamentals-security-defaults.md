---
title: Azure Active Directory-Sicherheitsstandards
description: Informationen zu Sicherheitsstandardrichtlinien zum Schutz von Organisationen vor gängigen Angriffen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 023a52f373844e026de0e588e9cd46323abdcf34
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149829"
---
# <a name="what-are-security-defaults"></a>Was sind Sicherheitsstandards?

Die Aufrechterhaltung der Sicherheit kann schwierig sein, da gängige identitätsbezogene Angriffe immer zahlreicher auftreten. Zu diesen gängigen Angriffen zählen Kennwortspray, Wiedergabe und Phishing.

Die Sicherheitsstandards in Azure Active Directory (Azure AD) erleichtern es, die Sicherheit zu erhöhen und Ihr Unternehmen vor gängigen Angriffen zu schützen. Sie umfassen vorkonfigurierte Sicherheitseinstellungen für diese verbreiteten Angriffe. 

Microsoft stellt allen Benutzern Sicherheitsstandards zur Verfügung. Das Ziel ist sicherzustellen, dass in allen Organisationen eine Standardsicherheitsebene ohne zusätzliche Kosten aktiviert ist. Sie aktivieren die Sicherheitsstandards im Azure-Portal.

![Screenshot: Azure-Portal mit Umschaltfläche zum Aktivieren der Sicherheitsstandards](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
> [!TIP]
> Wenn Ihr Mandant am 22. Oktober 2019 oder danach erstellt wurde, ist es möglich, dass Sie bereits das neue, sichere Standardverhalten nutzen und die Sicherheitsstandards in Ihrem Mandanten aktiviert wurden. Um alle unsere Benutzer zu schützen, werden die Sicherheitsstandards für alle neu erstellten Mandanten eingeführt.

Weitere Informationen dazu, warum diese Sicherheitsstandards verfügbar gemacht werden, finden Sie im Blogbeitrag von Alex Weinert [Introducing security defaults](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/introducing-security-defaults/ba-p/1061414) (Einführung von Sicherheitsstandards).

## <a name="unified-multi-factor-authentication-registration"></a>Einheitliche Registrierung für die mehrstufige Authentifizierung

Alle Benutzer in Ihrem Mandanten müssen sich über den Azure Multi-Factor Authentication-Dienst für die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) registrieren. Benutzer haben 14 Tage Zeit, sich mithilfe der Microsoft Authenticator-App für MFA zu registrieren. Nach Ablauf der 14 Tage kann der Benutzer sich erst wieder anmelden, nachdem die MFA-Registrierung durchgeführt wurde.

Uns ist bewusst, dass einige Benutzer möglicherweise abwesend sind oder sich unmittelbar nach Aktivierung der Sicherheitsstandards nicht innerhalb von 14 Tagen anmelden werden. Um sicherzustellen, dass jedem Benutzer ausreichend Zeit für die MFA-Registrierung eingeräumt wird, ist der Zeitraum von 14 Tagen für jeden Benutzer individuell festgelegt. Die 14-tägige Frist eines Benutzers beginnt nach seiner ersten erfolgreichen interaktiven Anmeldung nach Aktivierung der Sicherheitsstandards.

## <a name="multi-factor-authentication-enforcement"></a>Erzwingen der mehrstufigen Authentifizierung

### <a name="protecting-administrators"></a>Schützen von Administratoren

Benutzer mit Zugriff auf privilegierte Konten haben erweiterten Zugriff auf Ihre Umgebung. Aufgrund der weitreichenden Befugnisse, die diese Konten haben, sollten Sie sie mit Bedacht verwalten. Eine gängige Methode zur Verbesserung des Schutzes von privilegierten Konten ist eine strengere Form der Kontoüberprüfung für die Anmeldung. In Azure AD können Sie eine striktere Kontoüberprüfung erreichen, indem Sie eine mehrstufige Authentifizierung anfordern.

Nach Abschluss der MFA-Registrierung ist für die folgenden 9 Azure AD-Administratorrollen bei jeder Anmeldung eine zusätzliche Authentifizierung erforderlich:

- Globaler Administrator
- SharePoint-Administrator
- Exchange-Administrator
- Administrator für den bedingten Zugriff
- Sicherheitsadministrator
- Helpdeskadministrator oder Kennwortadministrator
- Rechnungsadministrator
- Benutzeradministrator
- Authentifizierungsadministrator

### <a name="protecting-all-users"></a>Schützen aller Benutzer

Wir gehen häufig davon aus, dass nur Administratorkonten durch eine mehrstufige Authentifizierung geschützt werden müssen. Administratoren haben umfassenden Zugriff auf vertrauliche Informationen und können Änderungen an abonnementweiten Einstellungen vornehmen. Angriffe richten sich jedoch häufig gegen Endbenutzer. 

Nachdem die Angreifer Zugang erhalten haben, können sie im Namen des ursprünglichen Kontoinhabers Zugriff auf privilegierten Informationen anfordern. Sie können sogar das gesamte Verzeichnis herunterladen, um einen Phishing-Angriff auf Ihr gesamtes Unternehmen durchzuführen. 

Eine gängige Methode zur Verbesserung des Schutzes für alle Benutzer besteht in einer strengeren Kontoüberprüfung, beispielsweise durch eine mehrstufige Authentifizierung (MFA). Nachdem die Benutzer die MFA-Registrierung abgeschlossen haben, werden sie bei Bedarf zu einer zusätzlichen Authentifizierung aufgefordert.

### <a name="blocking-legacy-authentication"></a>Blockieren der Legacyauthentifizierung

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure AD eine Vielzahl von Authentifizierungsprotokollen – einschließlich der Legacyauthentifizierung. Der Begriff *Legacyauthentifizierung* bezieht sich auf eine Authentifizierungsanforderung von:

- älteren Office-Clients, die keine moderne Authentifizierung verwenden (z. B. ein Office 2010-Client)
- jedem Client, der ältere E-Mail-Protokolle wie IMAP, SMTP oder POP3 verwendet

Der Großteil aller gefährdenden Anmeldeversuche erfolgt über Legacyauthentifizierungen. Die Legacyauthentifizierung unterstützt keine mehrstufige Authentifizierung. Selbst wenn Sie in Ihrem Verzeichnis eine MFA-Richtlinie aktiviert haben, kann sich ein Angreifer mit einem älteren Protokoll authentifizieren und die mehrstufige Authentifizierung umgehen. 

Nach Aktivierung der Sicherheitsstandards in Ihrem Mandanten werden alle Authentifizierungsanforderungen blockiert, die über ein Legacyprotokoll an einen beliebigen Mandanten gerichtet werden. Durch die Sicherheitsstandards ist die Standardauthentifizierung für Exchange Active Sync blockiert.

> [!WARNING]
> Stellen Sie vor dem Aktivieren von Sicherheitsstandards sicher, dass Ihre Administratoren keine älteren Authentifizierungsprotokolle verwenden. Weitere Informationen finden Sie unter [Blockieren der Legacyauthentifizierung](concept-fundamentals-block-legacy-authentication.md).

### <a name="protecting-privileged-actions"></a>Schützen privilegierter Aktionen

Organisationen verwenden eine Vielzahl von Azure-Diensten, die über die Azure Resource Manager-API verwaltet werden, darunter z. B.:

- Azure-Portal 
- Azure PowerShell 
- Azure-Befehlszeilenschnittstelle

Benutzer, die Dienste mit Azure Resource Manager verwalten, verfügen über weitreichende Berechtigungen. Beispielsweise können mit Azure Resource Manager mandantenweite Konfigurationen wie Diensteinstellungen und die Abonnementabrechnung geändert werden. Die einstufige Authentifizierung ist für eine Vielzahl von Bedrohungen anfällig, beispielsweise für Phishing- und Kennwortspray-Angriffe. 

Es ist wichtig, die Identität der Benutzer zu überprüfen, die auf Azure Resource Manager zugreifen und Konfigurationen aktualisieren möchten. Sie überprüfen die Identität von Benutzern, indem Sie vor der Zugriffserteilung eine zusätzliche Authentifizierung anfordern.

Nach Aktivierung der Sicherheitsstandards in Ihrem Mandanten muss jeder Benutzer, der auf das Azure-Portal, Azure PowerShell oder die Azure CLI zugreift, eine zusätzliche Authentifizierung durchlaufen. Diese Richtlinie gilt für alle Benutzer mit Zugriff auf Azure Resource Manager – nicht nur für Administratoren. 

Wenn der Benutzer nicht für die MFA registriert ist, muss er sich mithilfe der Microsoft Authenticator-App registrieren, um den Vorgang fortzusetzen. Es wird keine 14-tägige Frist für die MFA-Registrierung eingeräumt.

Bei Mandanten vor Exchange Online 2017 ist die moderne Authentifizierung standardmäßig deaktiviert. Sie müssen die [moderne Authentifizierung aktivieren](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online), um eine mögliche Anmeldeschleife bei der Authentifizierung über diese Mandanten zu vermeiden.

> [!NOTE]
> Das Azure AD Connect-Synchronisierungskonto ist von den Sicherheitsstandards ausgenommen. Sie werden nicht aufgefordert, sich für die mehrstufige Authentifizierung zu registrieren bzw. diese auszuführen. Organisationen sollten dieses Konto nicht für andere Zwecke verwenden.

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Nachfolgend werden zusätzliche Aspekte im Zusammenhang mit der Bereitstellung von Sicherheitsstandards für Ihren Mandanten beleuchtet.

### <a name="authentication-methods"></a>Authentifizierungsmethoden

Sicherheitsstandards ermöglichen die Registrierung für die mehrstufige Authentifizierung und deren Nutzung **unter alleiniger Verwendung der Microsoft Authenticator-App mit Benachrichtigungen**. Der bedingte Zugriff ermöglicht die Verwendung und Aktivierung einer beliebigen Authentifizierungsmethode, für die sich der Administrator entscheidet.

|   | Standardwerte für die Sicherheit | Bedingter Zugriff |
| --- | --- | --- |
| Benachrichtigung über mobile App | X | X |
| Prüfcode aus mobiler App oder Hardwaretoken |   | X |
| Textnachricht an Telefon |   | X |
| Auf Telefon anrufen |   | X |
| App-Kennwörter |   | X** |

** App-Kennwörter sind nur in Szenarien mit benutzerbasierter MFA mit Legacyauthentifizierung verfügbar, wenn diese Methode von Administratoren aktiviert wurde.

### <a name="conditional-access"></a>Bedingter Zugriff

Sie können den bedingten Zugriff zum Konfigurieren von Richtlinien verwenden, die Sicherheitsstandards vergleichbar sind, aber eine höhere Granularität (einschließlich Benutzerausschlüsse) bieten, die bei Sicherheitsstandards nicht verfügbar ist. Wenn Sie mit dem bedingtem Zugriff arbeiten und in Ihrer Umgebung die Richtlinien für bedingten Zugriff aktiviert sind, stehen Ihnen die Sicherheitsstandards nicht zur Verfügung. Wenn Sie über eine Lizenz für die Bereitstellung des bedingten Zugriffs verfügen, aber keine Richtlinien für bedingten Zugriff in Ihrer Umgebung aktiviert sind, können Sie die Sicherheitsstandards solange nutzen, bis Sie Richtlinien für bedingten Zugriff aktivieren. Weitere Informationen zur Azure AD-Lizenzierung finden Sie auf der Seite [Azure Active Directory (AD) – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

![Warnmeldung mit dem Hinweis, dass Sicherheitsstandards und bedingter Zugriff nicht gleichzeitig verwendet werden können](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Hier finden Sie Schrittanleitungen dazu, wie Sie mithilfe des bedingten Zugriffs äquivalente Richtlinien konfigurieren können:

- [Vorschreiben der MFA für Administratoren](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [Vorschreiben der MFA für die Azure-Verwaltung](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Blockieren älterer Authentifizierungsmethoden](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [Erzwingen der MFA für alle Benutzer](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)
- [Anfordern von Azure MFA-Registrierung](../identity-protection/howto-identity-protection-configure-mfa-policy.md) – Erfordert Azure AD Identity Protection

## <a name="enabling-security-defaults"></a>Aktivieren von Sicherheitsstandards

So aktivieren Sie Sicherheitsstandards in Ihrem Verzeichnis

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com)  als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Wechseln Sie zu  **Azure Active Directory** > **Eigenschaften**.
1. Wählen Sie **Sicherheitsstandards verwalten** aus.
1. Legen Sie die Option **Sicherheitsstandards aktivieren** auf **Ja** fest.
1. Wählen Sie **Speichern** aus.

## <a name="disabling-security-defaults"></a>Deaktivieren von Sicherheitsstandards

Organisationen, die Richtlinien für den bedingten Zugriff implementieren, die Sicherheitsstandards ersetzen, müssen Sicherheitsstandards deaktivieren. 

![Warnmeldung zur Deaktivierung von Sicherheitsstandards für das Aktivieren des bedingten Zugriffs](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

So deaktivieren Sie Sicherheitsstandards in Ihrem Verzeichnis

1. Melden Sie sich beim  [Azure-Portal](https://portal.azure.com)  als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Wechseln Sie zu  **Azure Active Directory** > **Eigenschaften**.
1. Wählen Sie **Sicherheitsstandards verwalten** aus.
1. Legen Sie die Option **Sicherheitsstandards aktivieren** auf **Nein** fest.
1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](../conditional-access/concept-conditional-access-policy-common.md)
