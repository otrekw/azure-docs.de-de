---
title: App-Schutzrichtlinien mit bedingtem Zugriff – Azure Active Directory
description: Erfahren Sie, wie Sie die App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs in Azure Active Directory erzwingen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 04/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol, rosssmi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b282962cc713487b8ee5113b02b8533a1538fff
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631897"
---
# <a name="how-to-require-app-protection-policy-and-an-approved-client-app-for-cloud-app-access-with-conditional-access"></a>Gewusst wie: Erzwingen einer App-Schutzrichtlinie und einer genehmigten Client-App für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs

Viele Menschen verwenden mobile Geräte sowohl für private als auch für berufliche Zwecke. Organisationen möchten natürlich sicherstellen, dass die Mitarbeiter produktiv arbeiten können, aber gleichzeitig auch verhindern, dass Datenverluste durch potenziell unsichere Anwendungen vermieden werden. Mit bedingtem Zugriff können Organisationen den Zugriff auf genehmigte (modern authentifizierungsfähige) Client-Apps mithilfe von Intune-App-Schutzrichtlinien einschränken, die auf solche Apps angewandt werden.

Dieser Artikel enthält zwei Szenarien zum Konfigurieren von Richtlinien für den bedingten Zugriff auf Ressourcen wie Office 365, Exchange Online und SharePoint Online.

- [Szenario 1: Office 365-Apps erfordern genehmigte Apps mit App-Schutzrichtlinien.](#scenario-1-office-365-apps-require-approved-apps-with-app-protection-policies)
- [Szenario 2: Exchange Online und SharePoint Online erfordern eine genehmigte Client-App und eine App-Schutzrichtlinie.](#scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app-and-app-protection-policy)

Bei bedingtem Zugriff sind diese Client-Apps bekanntermaßen durch eine App-Schutzrichtlinie geschützt. Weitere Informationen zu App-Schutzrichtlinien finden Sie im Artikel [Übersicht über App-Schutzrichtlinien](/intune/apps/app-protection-policy).

Eine Liste der qualifizierten Client-Apps finden Sie unter [App-Schutzrichtlinie als Voraussetzung](concept-conditional-access-grant.md).

> [!NOTE]
>    Die OR-Klausel wird innerhalb der Richtlinie verwendet, um Benutzern die Verwendung von Apps zu ermöglichen, welche die Zugriffserteilungssteuerung **App-Schutzrichtlinie erforderlich** bzw. **Genehmigte Client-App erforderlich** unterstützen. Weitere Informationen zu den Apps, welche die Zugriffserteilungssteuerung **App-Schutzrichtlinie erforderlich** unterstützen, finden Sie unter [App-Schutzrichtlinie als Voraussetzung](concept-conditional-access-grant.md).

## <a name="scenario-1-office-365-apps-require-approved-apps-with-app-protection-policies"></a>Szenario 1: Office 365-Apps erfordern genehmigte Apps mit App-Schutzrichtlinien.

In diesem Szenario hat Contoso beschlossen, dass der gesamte mobile Zugriff auf Office 365-Ressourcen genehmigte Client-Apps wie Outlook Mobile, OneDrive und Microsoft Teams erfordert, die durch eine App-Schutzrichtlinie geschützt sind. Alle Benutzer melden sich bereits mit Azure AD-Anmeldeinformationen an. Außerdem wurden ihnen Lizenzen zugewiesen, die Azure AD Premium P1 oder P2 und Microsoft Intune umfassen.

Organisationen müssen die folgenden Schritte ausführen, um die Verwendung einer genehmigten Client-App auf mobilen Geräten zu erzwingen.

**Schritt 1: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Office 365**

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** oder bestimmte **Benutzer und Gruppen** aus, auf die Sie diese Richtlinie anwenden möchten. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Office 365 (Vorschau)** aus.
1. Wählen Sie unter **Bedingungen** die Option **Geräteplattformen** aus.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Schließen Sie **Android** und **iOS** ein.
1. Wählen Sie unter **Bedingungen** die Option **Client-Apps (Vorschau)** aus.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Wählen Sie **Mobile Apps und Desktopclients** und **Clients mit moderner Authentifizierung** aus.
1. Wählen Sie unter **Zugriffssteuerungen** > **Erteilen** die folgenden Optionen aus:
   - **Genehmigte Client-App erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Alle ausgewählten Kontrollen anfordern**
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

**Schritt 2: Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff für Exchange Online mit ActiveSync (EAS)**

Für die Richtlinie zum bedingten Zugriff in diesem Schritt konfigurieren Sie folgende Komponenten:

1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** oder bestimmte **Benutzer und Gruppen** aus, auf die Sie diese Richtlinie anwenden möchten. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Office 365 Exchange Online** aus.
1. Unter **Bedingungen**:
   1. **Client-Apps (Vorschau):**
      1. Legen Sie **Konfigurieren** auf **Ja** fest.
      1. Wählen Sie **Mobile Apps und Desktopclients** und **Exchange ActiveSync-Clients** aus.
1. Wählen Sie unter **Zugriffssteuerungen** > **Erteilen** die Option **Zugriff erteilen**, **App-Schutzrichtlinie erforderlich** aus, und wählen Sie dann **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

Die Schritte zum Erstellen von App-Schutzrichtlinien für Android und iOS finden Sie im Artikel [Erstellen und Zuweisen von App-Schutzrichtlinien](/intune/apps/app-protection-policies). 

## <a name="scenario-2-exchange-online-and-sharepoint-online-require-an-approved-client-app-and-app-protection-policy"></a>Szenario 2: Exchange Online und SharePoint Online erfordern eine genehmigte Client-App und eine App-Schutzrichtlinie.

In diesem Szenario hat Contoso beschlossen, dass Benutzer auf mobilen Geräten nur auf E-Mail- und SharePoint-Daten zugreifen dürfen, sofern sie eine genehmigte Client-App wie Outlook Mobile verwenden, die vor dem Erteilen des Zugriffs durch eine App-Schutzrichtlinie geschützt wird. Alle Benutzer melden sich bereits mit Azure AD-Anmeldeinformationen an. Außerdem wurden ihnen Lizenzen zugewiesen, die Azure AD Premium P1 oder P2 und Microsoft Intune umfassen.

Organisationen müssen die folgenden drei Schritte ausführen, um die Verwendung einer genehmigten Client-App auf mobilen Geräten und von Exchange ActiveSync-Clients zu erzwingen.

**Schritt 1: Richtlinie für auf Android und iOS basierende moderne Authentifizierungsclients, die die Verwendung einer genehmigten Client-App-Schutzrichtlinie für den Zugriff auf Exchange Online und SharePoint Online erfordern.**

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** oder bestimmte **Benutzer und Gruppen** aus, auf die Sie diese Richtlinie anwenden möchten. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Optionen **Office 365 Exchange Online** und **Office 365 SharePoint Online** aus.
1. Wählen Sie unter **Bedingungen** die Option **Geräteplattformen** aus.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Schließen Sie **Android** und **iOS** ein.
1. Wählen Sie unter **Bedingungen** die Option **Client-Apps (Vorschau)** aus.
   1. Legen Sie **Konfigurieren** auf **Ja** fest.
   1. Wählen Sie **Mobile Apps und Desktopclients** und **Clients mit moderner Authentifizierung** aus.
1. Wählen Sie unter **Zugriffssteuerungen** > **Erteilen** die folgenden Optionen aus:
   - **Genehmigte Client-App erforderlich**
   - **App-Schutzrichtlinie erforderlich (Vorschau)**
   - **Eine der ausgewählten Steuerungen anfordern**
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

**Schritt 2: Richtlinie für Exchange ActiveSync-Clients, die die Verwendung einer genehmigten Client-App erfordern.**

1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** oder bestimmte **Benutzer und Gruppen** aus, auf die Sie diese Richtlinie anwenden möchten. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Office 365 Exchange Online** aus.
1. Unter **Bedingungen**:
   1. **Client-Apps (Vorschau):**
      1. Legen Sie **Konfigurieren** auf **Ja** fest.
      1. Wählen Sie **Mobile Apps und Desktopclients** und **Exchange ActiveSync-Clients** aus.
1. Wählen Sie unter **Zugriffssteuerungen** > **Erteilen** die Option **Zugriff erteilen**, **App-Schutzrichtlinie erforderlich** aus, und wählen Sie dann **Auswählen** aus.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

**Schritt 3: Konfigurieren der Intune-App-Schutzrichtlinie für iOS- und Android-Clientanwendungen**

Die Schritte zum Erstellen von App-Schutzrichtlinien für Android und iOS finden Sie im Artikel [Erstellen und Zuweisen von App-Schutzrichtlinien](/intune/apps/app-protection-policies). 

## <a name="next-steps"></a>Nächste Schritte

[Was ist bedingter Zugriff?](overview.md)

[Bedingter Zugriff auf Komponenten](concept-conditional-access-policies.md)

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

