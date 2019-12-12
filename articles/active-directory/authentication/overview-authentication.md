---
title: 'Authentifizieren und Schützen von Benutzern: Azure Active Directory'
description: Wie kann ich als Azure AD-Administrator die Benutzerauthentifizierung schützen, während ich gleichzeitig die Auswirkungen für Endbenutzer gering halte?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: overview
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3077851f5feafdec9d46d9365c45f5729ca2b0b1
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74846673"
---
# <a name="what-methods-are-available-for-authentication"></a>Welche Methoden stehen für die Authentifizierung zur Verfügung?

In den Nachrichten hören wir davon, dass Kennwörter gestohlen und Identitäten kompromittiert werden. Indem zusätzlich zu einem Kennwort die Abfrage eines zweiten Faktors erzwungen wird, wird die Sicherheit für Ihre Organisation sofort erhöht. Microsoft Azure Active Directory (Azure AD) enthält Features, z.B. Azure Multi-Factor Authentication (Azure MFA) und Azure AD-Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR), damit Administratoren ihre Organisationen und Benutzer mit zusätzlichen Authentifizierungsmethoden schützen können.

Es gibt u.a. folgende Szenarien: Anmelden bei einer Anwendung, Zurücksetzen des Kennworts und Aktivieren von Windows Hello. Ihre Benutzer werden unter Umständen zur zusätzlichen Bestätigung ihrer Identität aufgefordert.

Die zusätzliche Verifizierung kann beispielsweise über die folgenden Authentifizierungsmethoden durchgeführt werden:

* Einen Code, der per E-Mail oder SMS bereitgestellt wird
* Einen Telefonanruf
* Eine Benachrichtigung oder einen Code auf dem Smartphone
* Antworten auf Sicherheitsfragen

![Beispiel: Seite „login.microsoftonline.com“ in Chrome](media/overview-authentication/overview-login.png)

Mit Azure MFA und Azure AD-Self-Service-Kennwortzurücksetzung haben Administratoren die Kontrolle über die Bereiche Konfiguration, Richtlinien, Überwachung und Berichterstellung und können Azure AD und das Azure-Portal nutzen, um ihre Organisationen zu schützen.

## <a name="self-service-password-reset"></a>Self-Service-Kennwortzurücksetzung

Mit der Self-Service-Kennwortzurücksetzung können Ihre Benutzer jederzeit ihr Kennwort zurücksetzen, ohne dass ein Administrator eingreifen muss.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

Die Self-Service-Kennwortzurücksetzung umfasst Folgendes:

* **Kennwortänderung:** Mir ist mein Kennwort bekannt, aber ich möchte es in ein neues Kennwort ändern.
* **Kennwortzurücksetzung:** Ich kann mich nicht anmelden und möchte mein Kennwort mithilfe einer oder mehrerer genehmigter Authentifizierungsmethoden zurücksetzen.
* **Kontoentsperrung:** Ich kann mich nicht anmelden, weil mein Konto gesperrt ist, und ich möchte es mithilfe einer oder mehrerer genehmigter Authentifizierungsmethoden entsperren.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Multi-Factor Authentication (MFA) ist die Lösung von Microsoft für die zweistufige Überprüfung. Mit vom Administrator genehmigten Authentifizierungsmethoden trägt Azure MFA zum Schutz Ihres Zugriffs auf Daten und Anwendungen bei, während gleichzeitig die Forderung nach einem einfachen Anmeldeprozess erfüllt wird.

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Nächste Schritte

Der nächste Schritt besteht darin, tiefer einzusteigen und mit dem Konfigurieren der Self-Service-Kennwortzurücksetzung und Azure Multi-Factor Authentication zu beginnen.

Informationen zum Einstieg in die Self-Service-Kennwortzurücksetzung finden Sie in der entsprechenden [Schnellstartanleitung zur Aktivierung](quickstart-sspr.md).

Weitere Informationen zur Self-Service-Kennwortzurücksetzung finden Sie im Artikel [Funktionsweise der Self-Service-Kennwortzurücksetzung in Azure AD](concept-sspr-howitworks.md).

Informationen zur Azure Multi-Factor Authentication finden Sie im Artikel [Funktionsweise: Azure Multi-Factor Authentication](concept-mfa-howitworks.md).
