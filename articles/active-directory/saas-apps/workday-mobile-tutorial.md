---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Workday Mobile Application | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workday Mobile Application konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: jeedes
ms.openlocfilehash: 256da169761da486d8ac064a2f58a59be43bb5df
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754181"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workday-mobile-application"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Workday Mobile Application

In diesem Tutorial erfahren Sie, wie Sie Azure Active Directory (Azure AD), bedingten Zugriff und Intune in mobile Workday-Apps integrieren. Die Microsoft-Integration für mobile Workday-Apps ermöglicht Folgendes:

* Überprüfen Sie vor der Anmeldung, ob Geräte Ihren Richtlinien entsprechen.
* Fügen Sie der App von Workday Kontrollen hinzu, um sicherzustellen, dass Benutzer sicher auf Unternehmensdaten zugreifen. 
* Steuern Sie in Azure AD, wer Zugriff auf Workday hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Workday anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Integrieren von Workday mit Azure AD
* Tutorial: [Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-tutorial)

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial werden Microsoft-Richtlinien für den bedingten Zugriff sowie Intune mit mobilen Anwendungen von Workday konfiguriert und getestet.

* Workday-Verbundanwendungen können nun mit Azure AD konfiguriert werden, um SSO zu ermöglichen. Weitere Informationen zum Konfigurieren finden Sie unter [diesem Link](workday-tutorial.md).

> [!NOTE] 
> Workday unterstützt keine App-Schutzrichtlinien von Intune. Für bedingten Zugriff muss die mobile Geräteverwaltung verwendet werden.


## <a name="ensure-users-have-access-to-the-workday-mobile-app"></a>Sicherstellen, dass Benutzer auf die mobile Workday-App zugreifen können:

Konfigurieren Sie Workday, um den Zugriff auf die Angebote für mobile Apps zuzulassen. Für mobile Apps müssen folgende Richtlinien konfiguriert werden:

Gehen Sie zum Konfigurieren wie folgt vor:

1. Greifen Sie auf den Bericht „Domänensicherheitsrichtlinien für Funktionsbereich“ zu.
2. Wählen Sie eine Sicherheitsrichtlinie aus.
    * Mobile Verwendung: Android
    * Mobile Verwendung: iPad
    * Mobile Verwendung: iPhone
3. Klicken Sie auf Berechtigungen bearbeiten.
4. Aktivieren Sie das Kontrollkästchen für Anzeige- oder Änderungsvorgänge, um den Sicherheitsgruppen Zugriff auf die sicherungsfähigen Elemente des Berichts oder der Aufgabe zu gewähren.
5. Aktivieren Sie das Kontrollkästchen für Get- oder Put-Vorgänge, um den Sicherheitsgruppen Zugriff auf die Integration sowie auf sicherungsfähige Elemente des Berichts oder der Aufgabe zu gewähren.

Aktivieren Sie ausstehende Sicherheitsrichtlinienänderungen durch Ausführen der Aufgabe **Ausstehende Sicherheitsrichtlinienänderungen aktivieren**.

## <a name="open-workday-login-page-in-mobile-browser"></a>Öffnen der Workday-Anmeldeseite im mobilen Browser:

Um bedingten Zugriff auf die mobile App von Workday anzuwenden, muss die App in einem externen Browser geöffnet werden. Aktivieren Sie hierzu unter **Edit Tenant Setup > Security** (Mandantensetup bearbeiten >Sicherheit) das Kontrollkästchen **Enable Mobile Browser SSO for Native Apps** (SSO über mobilen Browser für native Apps aktivieren). Dadurch wird festgelegt, dass auf dem Gerät für iOS und im Arbeitsprofil für Android ein von Intune genehmigter Browser installiert werden muss.

![Anmeldung über mobilen Browser](./media/workday-tutorial/mobile-browser.png)

## <a name="setup-conditional-access-policy"></a>Einrichten der Richtlinie für bedingten Zugriff:

Diese Richtlinie wirkt sich nur auf die Anmeldung auf einem iOS- oder Android-Gerät aus. Wenn Sie alle Plattformen abdecken möchten, wählen Sie einfach **Jedes Gerät** aus. Diese Richtlinie erfordert, dass das Gerät mit der Richtlinie konform ist, und überprüft dies über Microsoft Intune. Aufgrund der Arbeitsprofile von Android sollten alle Benutzer daran gehindert werden, sich bei Workday (Web oder App) anzumelden, es sei denn, sie melden sich über ihr Arbeitsprofil an und haben die App über das Intune-Unternehmensportal installiert. Für iOS muss noch ein weiterer Schritt ausgeführt werden, um das gleiche Ergebnis zu erzielen. Im Anschluss folgen einige Screenshots der Einrichtung des bedingten Zugriffs.

**Von Workday werden folgende Zugriffssteuerungen unterstützt:**
* Mehrstufige Authentifizierung erforderlich
* Markieren des Geräts als kompatibel erforderlich

**Folgendes wird von der Workday-App nicht unterstützt:**
* Genehmigte Client-App erforderlich
* App-Schutzrichtlinie erforderlich (Vorschau)

Führen Sie die folgenden Schritte aus, um **Workday** als **Verwaltetes Gerät** einzurichten:

![Einrichten der Richtlinie für bedingten Zugriff](./media/workday-tutorial/managed-devices-only.png)

1. Klicken Sie auf **Start > Microsoft Intune > Richtlinien für bedingten Zugriff > Nur verwaltete Geräte**. 

1. Geben Sie auf der Seite **Nur verwaltete Geräte** im Feld **Name** den Wert `Managed Devices Only` ein, und klicken Sie auf **Cloud-Apps oder -aktionen**.

1. Führen Sie unter **Cloud-Apps oder -aktionen** die folgenden Schritte aus:

    a. Legen Sie **Wählen Sie aus, worauf diese Richtlinie angewendet werden soll.** auf **Cloud-Apps** fest.

    b. Klicken Sie auf der Registerkarte „Einschließen“ auf **Apps auswählen**.

    c. Wählen Sie in der Auswahlliste die Option **Workday** aus.

    d. Klicken Sie auf **Fertig**.

1. Legen Sie **Richtlinie aktivieren** auf „Ein“ fest.

1. Klicken Sie auf **Speichern**.

Führen Sie unter **Zuweisung** die folgenden Schritte aus:

![Workday: Einrichten der Richtlinie für bedingten Zugriff](./media/workday-tutorial/managed-devices-only-2.png)

1. Klicken Sie auf **Start > Microsoft Intune > Richtlinien für bedingten Zugriff > Nur verwaltete Geräte**. 

1. Geben Sie auf der Seite **Nur verwaltete Geräte** im Feld **Name** den Wert `Managed Devices Only` ein, und klicken Sie auf **Zugriffssteuerungen > Zuweisung**.

1. Führen Sie auf der Seite **Zuweisung** die folgenden Schritte aus:

    a. Wählen Sie für die zu erzwingenden Steuerungen die Option **Zugriff gewähren** aus.

    b. Aktivieren Sie das Kontrollkästchen **Markieren des Geräts als kompatibel erforderlich**.

    c. Wählen Sie **Eine der ausgewählten Steuerungen anfordern** aus.

    d. Klicken Sie auf **Auswählen**.

1. Legen Sie **Richtlinie aktivieren** auf „Ein“ fest.

1. Klicken Sie unten auf der Seite auf **Speichern**.

## <a name="set-up-device-compliance-policy"></a>Einrichten der Richtlinie zur Gerätekonformität:

Um zu gewährleisten, dass die Anmeldung bei iOS-Geräten nur über eine MDM-verwaltete Workday-App möglich ist, müssen Sie die App Store-App blockieren, indem Sie **com.workday.workdayapp** der Liste mit den eingeschränkten Apps hinzufügen. Dadurch wird sichergestellt, dass nur Geräte, auf denen die Workday-App über das Unternehmensportal installiert wurde, auf Workday zugreifen können. Bei Browsern ist der Zugriff auf Workday nur möglich, wenn das Gerät von Intune verwaltet und ein verwalteter Browser verwendet wird.

![Workday: Einrichten der Richtlinie zur Gerätekonformität](./media/workday-tutorial/ios-policy.png)

## <a name="set-up-microsoft-intune-app-configuration-policies"></a>Einrichten von Microsoft Intune-App-Konfigurationsrichtlinien:

| Szenario | Schlüssel-Wert-Paare |
|----------------------------------------------------------------------------------------   |-----------|
| Automatisches Ausfüllen der Mandanten- und Webadressfelder für Folgendes:<br>● Workday unter Android, wenn Sie Arbeitsprofile für Android aktivieren<br>● Workday auf iPad und iPhone     | Konfigurieren Sie Ihren Mandanten mit folgenden Werten: <br>● Konfigurationsschlüssel: UserGroupCode<br>●   Werttyp: Zeichenfolge <br>●   Konfigurationswert: Name Ihres Mandanten. Beispiel: gms<br>Konfigurieren Sie Ihre Webadresse mit folgenden Werten:<br>●    Konfigurationsschlüssel: AppServiceHost<br>● Werttyp: Zeichenfolge<br>●    Konfigurationswert: Basis-URL für Ihren Mandanten. Ein Beispiel: https://www.myworkday.com                              |   |
| Deaktivieren Sie für Workday auf iPad und iPhone die folgenden Aktionen:<br>●    Ausschneiden, Kopieren und Einfügen<br>●   Drucken                       | Legen Sie den (booleschen) Wert für die folgenden Schlüssel auf „False“ fest, um die entsprechenden Funktionen zu deaktivieren:<br>● AllowCutCopyPaste<br>●  AllowPrint  |
| Deaktivieren Sie Screenshots für Workday unter Android. |Legen Sie den (booleschen) Wert für den Schlüssel „AllowScreenshots“ auf „False“ fest, um die entsprechende Funktion zu deaktivieren.|
| Deaktivieren Sie vorgeschlagene Updates für Ihre Benutzer.|Legen Sie den (booleschen) Wert für den Schlüssel „AllowSuggestedUpdates“ auf „False“ fest, um die entsprechende Funktion zu deaktivieren.|
|Passen Sie die App Store-URL an, um mobile Benutzer zum App Store Ihrer Wahl weiterzuleiten.|Ändern Sie die App Store-URL wie folgt:<br>● Konfigurationsschlüssel: AppUpdateURL<br>●   Werttyp: Zeichenfolge<br> ●   Konfigurationswert: App Store-URL|
|       |


## <a name="ios-configuration-policies"></a>iOS-Konfigurationsrichtlinien:

1. Navigieren Sie zu https://portal.azure.com/, und melden Sie sich an.
2. Suchen Sie nach **Intune** , oder klicken Sie in der Liste auf das Widget.
3. Navigieren Sie zu **Client-Apps > Apps > App-Konfigurationsrichtlinien > + Hinzufügen > Verwaltete Geräte**.
4. Geben Sie einen Namen ein.
5. Wählen Sie unter **Plattform** die Option **iOS/iPadOS** aus.
6. Wählen Sie unter **Zugeordnete App** die von Ihnen hinzugefügte Workday-App für iOS aus.
7. Klicken Sie auf **Konfigurationseinstellungen** , und wählen Sie unter **Format der Konfigurationseinstellungen** die Option **XML-Daten eingeben** aus.
8. Hier sehen Sie ein Beispiel für eine XML-Datei: Fügen Sie die gewünschten Konfigurationen hinzu. Ersetzen Sie **STRING_VALUE** durch die zu verwendende Zeichenfolge. Ersetzen Sie „`<true />` or `<false />`“ durch „`<true />`“ oder „`<false />`“. Wenn Sie keine Konfiguration hinzufügen, wird standardmäßig „True“ verwendet.

    ```
    <dict>
    <key>UserGroupCode</key>
    <string>STRING_VALUE</string>
    <key>AppServiceHost</key>
    <string>STRING_VALUE</string>
    <key>AllowCutCopyPaste</key>
    <true /> or <false />
    <key>AllowPrint</key>
    <true /> or <false />
    <key>AllowSuggestedUpdates</key>
    <true /> or <false />
    <key>AppUpdateURL</key>
    <string>STRING_VALUE</string>
    </dict>

    ```
9. Klicken Sie auf "Hinzufügen".
10. Aktualisieren Sie die Seite, und klicken Sie auf die neu erstellte Richtlinie.
11. Klicken Sie auf „Zuweisungen“, und wählen Sie aus, auf wen die App angewendet werden soll.
12. Klicken Sie auf Speichern.

## <a name="android-configuration-policies"></a>Android-Konfigurationsrichtlinien:

1. Navigieren Sie zu `https://portal.azure.com/`, und melden Sie sich an.
2. Suchen Sie nach **Intune** , oder klicken Sie in der Liste auf das Widget.
3. Navigieren Sie zu **Client-Apps > Apps > App-Konfigurationsrichtlinien > + Hinzufügen > Verwaltete Geräte**.
5. Geben Sie einen Namen ein. 
6. Wählen Sie unter **Plattform** die Option **Android** aus.
7. Wählen Sie unter **Zugeordnete App** die von Ihnen hinzugefügte Workday-App für Android aus.
8. Klicken Sie auf **Konfigurationseinstellungen** , und wählen Sie unter **Format der Konfigurationseinstellungen** die Option **JSON-Daten eingeben** aus.

