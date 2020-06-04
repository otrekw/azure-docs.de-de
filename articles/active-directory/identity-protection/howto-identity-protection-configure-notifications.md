---
title: Azure Active Directory Identity Protection Benachrichtigungen
description: Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 200ede6b4c5565a8eab95b0398abaa1c056c612f
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853129"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen

Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoerkennungen dienen:

- E-Mail für erkannte gefährdete Benutzer
- Wöchentliche E-Mail mit Übersicht

Dieser Artikel gibt Ihnen eine Übersicht zu beiden Benachrichtigungs-E-Mails.

## <a name="users-at-risk-detected-email"></a>E-Mail für erkannte gefährdete Benutzer

Als Reaktion auf ein erkanntes gefährdetes Konto erstellt Azure Active Directory Identity Protection eine E-Mail-Warnung mit dem Betreff **Gefährdete Benutzer erkannt**. Die E-Mail enthält einen Link zum Bericht vom Typ **[Benutzer mit Risikokennzeichnung](../reports-monitoring/concept-user-at-risk.md)** . Als bewährte Methode sollten Sie die gefährdeten Benutzer sofort untersuchen.

Durch Konfiguration dieser Warnung können Sie angeben, bei welcher Benutzerrisikostufe die Warnung generiert werden soll. Die E-Mail wird generiert, wenn die Risikostufe des Benutzers den angegebenen Wert erreicht. Allerdings erhalten Sie für diesen Benutzer keine neuen E-Mail-Warnungen des Typs „Gefährdete Benutzer erkannt“ mehr, nachdem der Benutzer diese Risikostufe erreicht hat. Wenn Sie beispielsweise die Richtlinie für Warnungen auf ein mittleres Benutzerrisiko festlegen und der Benutzer Johann die mittlere Risikostufe erreicht, erhalten Sie für Johann eine E-Mail des Typs „Gefährdete Benutzer erkannt“. Sie erhalten allerdings keine zweite Warnung des Typs „Gefährdeter Benutzer erkannt“, wenn Johann anschließend eine höhere Risikostufe erreicht oder zusätzliche Risikoerkennungen auftreten.

![E-Mail für erkannte gefährdete Benutzer](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Konfigurieren von Warnungen zu erkannten gefährdeten Benutzern

Als Administrator können Sie Folgendes festlegen:

- **Die Benutzerrisikostufe, bei der die Erstellung dieser E-Mail ausgelöst wird**: Standardmäßig ist die Risikostufe auf „Hoch“ festgelegt.
- **Die Empfänger dieser E-Mail**: Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte hinzufügen.
   - Optional können Sie **zusätzliche E-Mails für den Empfang von Warnungsbenachrichtigungen hinzufügen**. Dieses Feature ist eine Vorschau, und die definierten Benutzer müssen über die entsprechenden Berechtigungen zum Anzeigen der verknüpften Berichte im Azure-Portal verfügen.

Konfigurieren Sie die E-Mail-Adresse der gefährdeten Benutzers im **Azure-Portal** unter **Azure Active Directory** > **Sicherheit** > **Identitätsschutz** > **Warnungen zu erkannten gefährdeten Benutzern**.

## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht

Die wöchentliche Übersichts-E-Mail enthält eine Zusammenfassung der neuen Risikoerkennungen.  
Sie hat folgenden Inhalt:

- Neue Benutzer mit Risiko erkannt
- Neue Risikoanmeldungen erkannt (in Echtzeit)
- Links zu verwandten Berichten in Identity Protection

![Wöchentliche E-Mail mit Übersicht](./media/howto-identity-protection-configure-notifications/weekly-digest-email.png)

Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte hinzufügen.

### <a name="configure-weekly-digest-email"></a>Konfigurieren von wöchentlichen Zusammenfassung per E-Mail

Als Administrator können Sie das Senden einer wöchentlichen Zusammenfassung per E-Mail aktivieren oder deaktivieren und die Benutzer auswählen, denen die E-Mail gesendet werden soll.

Konfigurieren Sie die wöchentliche Zusammenfassung per E-Mail im **Azure-Portal** unter **Azure Active Directory** > **Sicherheit** > **Informationsschutz** > **Wöchentliche Übersicht**.

## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
