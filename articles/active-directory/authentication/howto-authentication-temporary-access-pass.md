---
title: Konfigurieren eines befristeten Zugriffspasses in Azure AD für die Registrierung von kennwortlosen Authentifizierungsmethoden
description: Erfahren Sie, wie Sie einen befristeten Zugriffspass (Temporary Access Pass, TAP) konfigurieren und Benutzern die Registrierung von kennwortlosen Authentifizierungsmethoden mithilfe eines befristeten Zugriffspasses ermöglichen.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: justinha
author: inbarckms
manager: daveba
ms.reviewer: inbarckms
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56d45119fa86ab47e6a625c628d8cb9763db83bd
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100520761"
---
# <a name="configure-temporary-access-pass-in-azure-ad-to-register-passwordless-authentication-methods-preview"></a>Konfigurieren eines befristeten Zugriffspasses in Azure AD für die Registrierung von kennwortlosen Authentifizierungsmethoden (Vorschau)

Kennwortlose Authentifizierungsmethoden (wie z. B. FIDO2 und die kennwortlose Anmeldung per Telefon über die Microsoft Authenticator-App) ermöglichen Benutzern die sichere Anmeldung ohne Kennwort. Für das Bootstrapping kennwortloser Methoden stehen Benutzern zwei Möglichkeiten zur Verfügung:

- Verwenden vorhandener Azure AD Multi-Factor Authentication-Methoden 
- Verwenden eines befristeten Zugriffspasses (Temporary Access Pass, TAP) 

Ein TAP ist ein von einem Administrator ausgegebener zeitlich begrenzter Passcode, der strenge Authentifizierungsanforderungen erfüllt und zum Integrieren anderer Authentifizierungsmethoden einschließlich kennwortloser Methoden verwendet werden kann. Ein TAP erleichtert auch die Wiederherstellung, wenn ein Benutzer seinen strengen Authentifizierungsfaktor wie einen FIDO2-Sicherheitsschlüssel oder die Microsoft Authenticator-App verloren oder vergessen hat, sich jedoch anmelden muss, um neue strenge Authentifizierungsmethoden zu registrieren.


In diesem Artikel wird beschrieben, wie Sie im Azure-Portal einen TAP in Azure AD aktivieren und verwenden. Sie können diese Aktionen auch mithilfe der REST-APIs ausführen. 

>[!NOTE]
>Der befristete Zugriffspass befindet sich derzeit in der öffentlichen Vorschauphase. Einige Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 

## <a name="enable-the-tap-policy"></a>Aktivieren der TAP-Richtlinie

In einer TAP-Richtlinie werden Einstellungen definiert, wie z. B. die Gültigkeitsdauer von im Mandanten erstellten Pässen oder die Benutzer und Gruppen, die einen TAP für die Anmeldung verwenden können. Bevor sich Benutzer mit einem TAP anmelden können, müssen Sie die Richtlinie für die Authentifizierungsmethode aktivieren und auswählen, welche Benutzer und Gruppen sich mit einem TAP anmelden können.
Sie können zwar einen TAP für jeden Benutzer erstellen, es können sich aber nur die in der Richtlinie enthaltenen Benutzer damit anmelden.

Globale Administratoren und Authentifizierungsmethoden-Richtlinienadministratoren können die TAP-Authentifizierungsmethodenrichtlinie aktualisieren.
Gehen Sie wie folgt vor, um die TAP-Authentifizierungsmethodenrichtlinie zu konfigurieren:

1. Melden Sie sich als globaler Administrator beim Azure-Portal an, und klicken Sie auf **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Befristeter Zugriffspass**.
1. Klicken Sie auf **Ja**, um die Richtlinie zu aktivieren, wählen Sie aus, auf welche Benutzer die Richtlinie angewendet werden soll, und wählen Sie unter **Allgemein** die gewünschten Einstellungen aus.

   ![Screenshot: Aktivieren der TAP-Authentifizierungsmethodenrichtlinie](./media/how-to-authentication-temporary-access-pass/policy.png)

   Die Standardwerte und der Bereich der zulässigen Werte werden in der folgenden Tabelle beschrieben.


   | Einstellung          | Standardwerte | Zulässige Werte               | Kommentare                                                                                                                                                                                                                                                                 |   |
   |------------------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    Mindestlebensdauer | 1 Stunde         | 10 – 43200 Minuten (30 Tage) | Die minimale Anzahl von Minuten, die der TAP gültig ist.                                                                                                                                                                                                                         |   |
   | Maximale Lebensdauer | 24 Stunden       | 10 – 43200 Minuten (30 Tage) | Die maximale Anzahl von Minuten, die der TAP gültig ist.                                                                                                                                                                                                                         |   |
   | Standardlebensdauer | 1 Stunde         | 10 – 43200 Minuten (30 Tage) | Die Standardwerte können von den einzelnen Pässen innerhalb der in der Richtlinie konfigurierten minimalen und maximalen Lebensdauer überschrieben werden.                                                                                                                                                |   |
   | Einmalige Verwendung     | False          | True/False                 | Wenn die Richtlinie auf „False“ festgelegt ist, können Pässe im Mandanten entweder einmal oder mehrmals während der jeweiligen Gültigkeitsdauer (maximalen Lebensdauer) verwendet werden. Durch Erzwingen der einmaligen Verwendung in der TAP-Richtlinie werden alle im Mandanten erstellten Pässe für die einmalige Verwendung erstellt. |   |
   | Länge           | 8              | 8 – 48 Zeichen              | Definiert die Länge des Passcodes.                                                                                                                                                                                                                                      |   |

## <a name="create-a-tap-in-the-azure-ad-portal"></a>Erstellen eines TAPs im Azure AD-Portal

Nachdem Sie eine TAP-Richtlinie aktiviert haben, können Sie in Azure AD einen TAP für einen Benutzer erstellen. Die folgenden Rollen können die folgenden TAP-bezogenen Aktionen ausführen.

- Globale Administratoren können einen TAP für jeden Benutzer (außer für sich selbst) erstellen, löschen und anzeigen.
- Privilegierte Authentifizierungsadministratoren können TAPs für Administratoren und Mitglieder (außer für sich selbst) erstellen, löschen und anzeigen.
- Authentifizierungsadministratoren können TAPs für Mitglieder (außer für sich selbst) erstellen, löschen und anzeigen.
- Globale Administratoren können die TAP-Details für den Benutzer anzeigen (ohne den Code selbst zu lesen).

Gehen Sie wie folgt vor, um einen TAP zu erstellen:

1. Melden Sie sich als globaler Administrator, als privilegierter Authentifizierungsadministrator oder als Authentifizierungsadministrator beim Portal an. 
1. Klicken Sie auf **Azure Active Directory**, navigieren Sie zu „Benutzer“, wählen Sie einen Benutzer (z. B. *Chris Green*) aus, und wählen Sie dann **Authentifizierungsmethoden** aus.
1. Wählen Sie ggf. die Option zum **Testen der neuen Oberfläche für Benutzerauthentifizierungsmethoden** aus.
1. Wählen Sie die Option **Authentifizierungsmethoden hinzufügen** aus.
1. Klicken Sie unter **Methode auswählen** auf **Befristeter Zugriffspass (Vorschau)** .
1. Definieren Sie eine benutzerdefinierte Aktivierungszeit oder -dauer, und klicken Sie auf **Hinzufügen**.

   ![Screenshot: Erstellen eines TAPs](./media/how-to-authentication-temporary-access-pass/create.png)

1. Nach dem Hinzufügen werden die Details des TAPs angezeigt. Notieren Sie sich den tatsächlichen TAP-Wert. Diesen Wert stellen Sie für den Benutzer bereit. Nachdem Sie auf **OK** geklickt haben, können Sie diesen Wert nicht mehr anzeigen.
   
   ![Screenshot der TAP-Details](./media/how-to-authentication-temporary-access-pass/details.png)

## <a name="use-a-tap"></a>Verwenden eines TAPs

Die häufigste Verwendung für einen TAP ist, dass ein Benutzer bei der ersten Anmeldung Authentifizierungsdaten registrieren kann, ohne zusätzliche Sicherheitsabfragen durchlaufen zu müssen. Authentifizierungsmethoden werden unter [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) registriert. Dort können Benutzer auch vorhandene Authentifizierungsmethoden aktualisieren.

1. Öffnen Sie einen Webbrowser, und navigieren Sie zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).
1. Geben Sie den UPN des Kontos ein, für das Sie den TAP erstellt haben, beispielsweise *tapuser@contoso.com* .
1. Wenn der Benutzer in der TAP-Richtlinie enthalten ist, wird ein Bildschirm zum Eingeben des TAPs angezeigt.
1. Geben Sie den TAP ein, der im Azure-Portal angezeigt wurde.

   ![Screenshot: Eingeben eines TAPs](./media/how-to-authentication-temporary-access-pass/enter.png)

>[!NOTE]
>Für Verbunddomänen wird ein TAP gegenüber dem Verbund bevorzugt. Ein Benutzer mit einem TAP führt die Authentifizierung in Azure AD durch und wird nicht an den Verbundidentitätsanbieter (Identity Provider, IdP) umgeleitet.

Der Benutzer ist jetzt angemeldet und kann eine Methode (z. B. den FIDO2-Sicherheitsschlüssel) aktualisieren oder registrieren. Benutzer, die ihre Authentifizierungsmethoden aktualisieren, weil Sie ihre Anmeldeinformationen oder ihr Gerät verloren haben, sollten sicherstellen, dass sie die alten Authentifizierungsmethoden entfernen.

Benutzer können Ihren TAP auch verwenden, um sich direkt über die Authenticator-App für die kennwortlose Anmeldung per Telefon zu registrieren. Weitere Informationen finden Sie unter [Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App](../user-help/user-help-auth-app-add-work-school-account.md).

![Screenshot: Eingeben eines TAPs mit einem Geschäfts-, Schul- oder Unikonto](./media/how-to-authentication-temporary-access-pass/enter-work-school.png)

## <a name="delete-a-tap"></a>Löschen eines TAPs

Ein abgelaufener TAP kann nicht verwendet werden. Unter den **Authentifizierungsmethoden** für einen Benutzer wird in der Spalte **Details** angezeigt, wann der TAP abgelaufen ist. Sie können diese abgelaufenen TAPs durch Ausführen der folgenden Schritte löschen:

1. Navigieren Sie im Azure AD-Portal zu **Benutzer**, wählen Sie einen Benutzer (z. B. *TAP-Benutzer*) aus, und wählen Sie dann **Authentifizierungsmethoden** aus.
1. Wählen Sie auf der rechten Seite der in der Liste angezeigten Authentifizierungsmethode **Befristeter Zugriffspass (Vorschau)** die Option **Löschen** aus.

## <a name="replace-a-tap"></a>Ersetzen eines TAPs 

- Ein Benutzer kann nur über einen TAP verfügen. Der Passcode kann während der Start- und Endzeit des TAPs verwendet werden.
- Wenn der Benutzer einen neuen TAP benötigt:
  - Wenn der vorhandene TAP gültig ist, muss der Administrator den vorhandenen TAP löschen und einen neuen Pass für den Benutzer erstellen. Wenn Sie einen gültigen TAP löschen, werden die Sitzungen des Benutzers widerrufen. 
  - Wenn der vorhandene TAP abgelaufen ist, wird der vorhandene TAP durch einen neuen TAP außer Kraft gesetzt, und die Sitzungen des Benutzers werden nicht widerrufen.

Weitere Informationen zu NIST-Standards für das Onboarding und die Wiederherstellung finden Sie unter [NIST SP 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html#sec4).

## <a name="limitations"></a>Einschränkungen

Beachten Sie die folgenden Einschränkungen:

- Bei Verwendung eines einmaligen TAPs zum Registrieren einer kennwortlosen Methode wie FIDO2 oder die Anmeldung per Telefon muss der Benutzer die Registrierung innerhalb von 10 Minuten nach der Anmeldung mit dem einmaligen TAP abschließen. Diese Einschränkung gilt nicht für einen TAP, der mehrmals verwendet werden kann.
- Gastbenutzer können sich nicht mit einem TAP anmelden.
- Benutzer im Bereich der Richtlinie für die Self-Service-Kennwortzurücksetzung (Self Service Password Reset, SSPR) müssen eine der SSPR-Methoden registrieren, nachdem sie sich mit einem TAP angemeldet haben. Wenn der Benutzer nur FIDO2-Schlüssel verwendet, schließen Sie ihn von der SSPR-Richtlinie aus, oder deaktivieren Sie die SSPR-Registrierungsrichtlinie. 
- TAPs können nicht mit der Erweiterung für den Netzwerkrichtlinienserver (Network Policy Server, NPS) und dem Adapter für Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) verwendet werden.
- Wenn nahtloses einmaliges Anmelden für den Mandanten aktiviert ist, werden die Benutzer zur Eingabe eines Kennworts aufgefordert. Der Link **Verwenden Sie stattdessen Ihren befristeten Zugriffspass** ist für den Benutzer verfügbar, damit er sich mit einem TAP anmelden kann.

![Screenshot: Verwenden eines TAPs anstelle eines Kennworts](./media/how-to-authentication-temporary-access-pass/alternative.png)

## <a name="troubleshooting"></a>Problembehandlung    

- Wenn einem Benutzer die Verwendung eines TAPs bei der Anmeldung nicht angeboten wird, überprüfen Sie Folgendes:
  - Der Benutzer befindet sich im Bereich der Richtlinie für die TAP-Authentifizierungsmethode.
  - Der Benutzer verfügt über einen gültigen TAP, und wenn es sich um einen einmaligen TAP handelt, wurde er noch nicht verwendet.
- Wenn während der Anmeldung mit einem befristeten Zugriffspass die Meldung angezeigt wird, dass die **TAP-Anmeldung aufgrund der Richtlinie für Benutzeranmeldeinformationen blockiert wurde**, überprüfen Sie Folgendes:
  - Der Benutzer verfügt über einen mehrfach verwendbaren TAP, die Richtlinie für die Authentifizierungsmethode erfordert jedoch einen einmaligen TAP.
  - Ein einmaliger TAP wurde bereits verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Planen einer Bereitstellung mit kennwortloser Authentifizierung in Azure Active Directory](howto-authentication-passwordless-deployment.md)

