---
title: 'Auf dem Benutzerrisiko basierender bedingter Zugriff: Azure Active Directory'
description: Erstellen von Richtlinien für bedingten Zugriff mithilfe der Azure AD Identity Protection-Funktion für den auf dem Benutzerrisiko basierender bedingter Zugriff
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 916ff4a5e6afd68106d70f386436723c24574df1
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605587"
---
# <a name="conditional-access-user-risk-based-conditional-access"></a>Bedingter Zugriff: Auf dem Benutzerrisiko basierender bedingter Zugriff

Microsoft arbeitet mit Ermittlern, Strafverfolgungsbehörden, mehreren eigenen Sicherheitsteams und anderen vertrauenswürdigen Quellen zusammen, um kompromittierte Paare aus Benutzername und Kennwort zu finden. Organisationen mit Azure AD Premium P2-Lizenzen können Richtlinien für bedingten Zugriff erstellen, die [das Erkennen des Benutzerrisikos mit Azure AD Identity Protection](../identity-protection/concept-identity-protection-risks.md#user-risk) umfassen. 

Es gibt zwei Möglichkeiten, diese Richtlinie zuzuweisen. Organisationen sollten eine der folgenden Optionen wählen, um eine Richtlinie für bedingten Zugriff anhand des Benutzerrisikos zu aktivieren, die die Änderung in ein sicheres Kennwort erfordert.

## <a name="enable-with-conditional-access-policy"></a>Aktivieren mit einer Richtlinie für bedingten Zugriff

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen. Weitere Informationen finden Sie unter [Festlegen von Benennungsstandards für Ihre Richtlinien](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access#set-naming-standards-for-your-policies).
1. Klicken Sie unter **Zuweisungen** auf **Benutzer und Gruppen**.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** aus.
1. Legen Sie unter **Bedingungen** > **Benutzerrisiko** die Option **Konfigurieren** auf **Ja**fest. Wählen Sie unter **Hiermit konfigurieren Sie die Benutzerrisikostufen, die für die Erzwingung der Richtlinie erforderlich sind** die Option **Hoch** und dann **Fertig** aus.
1. Wählen Sie unter **Zugriffssteuerungen** > **Erteilen** die Option **Zugriff erteilen**, dann **Kennwortänderung erforderlich** und anschließend **Auswählen** aus.
1. Bestätigen Sie die Einstellungen, und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="enable-through-identity-protection"></a>Aktivieren über Identity Protection

1. Melden Sie sich beim **Azure-Portal** an.
1. Wählen Sie **Alle Dienste** aus, und navigieren Sie zu **Azure AD Identity Protection**.
1. Wählen Sie **Benutzerrisiko-Richtlinie** aus.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Ausgeschlossene Benutzer auswählen** und dann die Konten für den Notfallzugriff Ihres Unternehmens aus. Abschließend wählen Sie **Auswählen** aus.
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Bedingungen** die Option **Benutzerrisiko** und dann **Hoch** aus.
   1. Wählen Sie **Auswählen** und anschließend **Fertig** aus.
1. Wählen Sie unter **Kontrollen** > **Zugriff** die Option **Zugriff zulassen** und dann **Kennwortänderung anfordern** aus.
   1. Wählen Sie **Auswählen**.
1. Legen Sie **Richtlinie erzwingen** auf **Ein** fest.
1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Risikobasierter bedingter Zugriff beim Anmelden](howto-conditional-access-policy-risk.md)

[Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-report-only.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Was ist Azure Active Directory Identity Protection?](../identity-protection/overview.md)
