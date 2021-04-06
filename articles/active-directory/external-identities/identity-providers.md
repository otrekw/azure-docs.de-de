---
title: Identitätsanbieter für externe Identitäten – Azure AD
description: Erfahren Sie, wie Sie Azure AD als Standardidentitätsanbieter für die Freigabe für externe Benutzer verwenden.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdef929b27c636b3908dd7a88eb93adc2382a53f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101687741"
---
# <a name="identity-providers-for-external-identities"></a>Identitätsanbieter für externe Identitäten

> [!NOTE]
> Einige der in diesem Artikel erwähnten Features sind öffentliche Previewfunktionen von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ein *Identitätsanbieter* erstellt und verwaltet die Identitätsinformationen und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit. Wenn Sie Ihre Apps und Ressourcen für externe Benutzer freigeben, ist Azure AD der Standardidentitätsanbieter für die Freigabe. Dies bedeutet Folgendes: Wenn Sie externe Benutzer einladen, die bereits über ein Azure AD- oder Microsoft-Konto verfügen, können sich diese Benutzer automatisch anmelden, ohne dass Sie weitere Konfigurationsschritte ausführen müssen.

Zusätzlich zu Azure AD-Konten bietet External Identities verschiedene Identitätsanbieter.

- **Microsoft-Konten** (Vorschau): Gastbenutzer können ihr eigenes persönliches Microsoft-Konto (MSA) verwenden, um Ihre Einladungen zur B2B-Zusammenarbeit anzunehmen. Wenn Sie einen Benutzerflow für die Self-Service-Registrierung einrichten, können Sie [Microsoft-Konto (Vorschau)](microsoft-account.md) als einen der zulässigen Identitätsanbieter hinzufügen. Es ist keine zusätzliche Konfiguration erforderlich, um diesen Identitätsanbieter für Benutzerflows verfügbar zu machen.

- **Einmalkennung per E-Mail** (Vorschau): Wenn ein Gastbenutzer eine Einladung annehmen oder auf eine freigegebene Ressource zugreifen möchte, kann er einen temporären Code anfordern. Dieser wird dann an seine E-Mail-Adresse gesendet. Anschließend gibt er diesen Code ein, um den Anmeldevorgang fortzusetzen. Mit der Funktion „Einmalkennung per E-Mail“ werden B2B-Gastbenutzer authentifiziert, wenn sie über andere Wege nicht authentifiziert werden können. Wenn Sie einen Benutzerflow für die Self-Service-Registrierung einrichten, können Sie **Einmalkennung per E-Mail (Vorschau)** als einen der zulässigen Identitätsanbieter hinzufügen. Es sind einige wenige Einrichtungsschritte erforderlich. Weitere Informationen finden Sie unter [Authentifizierung mit Einmalkennung per E-Mail](one-time-passcode.md).

- **Google**: Mit dem Google-Verbund können externe Benutzer Ihre Einladungen einlösen, indem sie sich mit ihren eigenen Gmail-Konten bei Ihren Apps anmelden. Der Google-Verbund kann auch in Ihren Benutzerflows mit Self-Service-Registrierung verwendet werden. Weitere Informationen finden Sie unter [Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer](google-federation.md).
   > [!IMPORTANT]
   > **Am 4. Januar 2021** wird Google [die Unterstützung für die WebView-Anmeldung einstellen](https://developers.googleblog.com/2020/08/guidance-for-our-effort-to-block-less-secure-browser-and-apps.html). Wenn Sie einen Google-Verbund oder die Self-Service-Registrierung mit Gmail verwenden, sollten Sie [Ihre nativen Branchenanwendungen auf Kompatibilität testen](google-federation.md#deprecation-of-webview-sign-in-support).

- **Facebook**: Beim Entwickeln einer App können Sie die Self-Service-Registrierung konfigurieren und den Facebook-Verbund aktivieren, damit sich Benutzer mit ihren eigenen Facebook-Konten für Ihre App registrieren können. Facebook kann nur für Benutzerflows mit Self-Service-Registrierung verwendet werden und steht nicht als Anmeldeoption zur Verfügung, wenn Benutzer Ihre Einladungen einlösen. Weitere Informationen finden Sie unter [Hinzufügen von Facebook als Identitätsanbieter für externe Identitäten](facebook-federation.md).

- **Direkter Verbund**: Sie können auch einen direkten Verbund mit einem beliebigen externen Identitätsanbieter einrichten, der das SAML- oder WS-Fed-Protokoll unterstützt. Mit dem direkten Verbund können externe Benutzer Ihre Einladungen einlösen, indem sie sich mit ihren eigenen Social Media- oder Unternehmenskonten bei Ihren Apps anmelden. Weitere Informationen finden Sie unter [Direkter Verbund mit AD FS und Drittanbietern für Gastbenutzer (Preview)](direct-federation.md).
   > [!NOTE]
   > Identitätsanbieter für den direkten Verbund können in Ihren Benutzerflows mit Self-Service-Registrierung nicht verwendet werden.

## <a name="adding-social-identity-providers"></a>Hinzufügen von sozialen Netzwerken als Identitätsanbieter

Azure AD ist standardmäßig für die Self-Service-Registrierung aktiviert. Daher haben Benutzer immer die Möglichkeit, sich mit einem Azure AD-Konto zu registrieren. Sie können jedoch auch andere Identitätsanbieter aktivieren. Hierzu zählen unter anderem auch soziale Netzwerke wie Google und Facebook. Um soziale Netzwerke als Identitätsanbieter in Ihrem Azure AD-Mandanten einzurichten, erstellen Sie bei jedem Identitätsanbieter eine Anwendung und konfigurieren die Anmeldeinformationen. Sie erhalten eine Client- oder App-ID sowie einen geheimen Client- oder App-Schlüssel, den Sie dann zu Ihrem Azure AD-Mandanten hinzufügen können.

Nachdem Sie Ihrem Azure AD-Mandanten einen Identitätsanbieter hinzugefügt haben, ist Folgendes möglich:

- Wenn Sie einen externen Benutzer zu Apps oder Ressourcen in Ihrer Organisation einladen, kann sich der externe Benutzer mit seinem eigenen Konto dieses Identitätsanbieters anmelden.
- Wenn Sie die [Self-Service-Registrierung](self-service-sign-up-overview.md) für Ihre Apps aktivieren, können sich externe Benutzer mit ihren eigenen Konten der von Ihnen hinzugefügten Identitätsanbieter bei Ihren Apps registrieren. Sie können aus den Optionen für soziale Medien als Identitätsanbieter wählen, die Sie auf der Registrierungsseite verfügbar gemacht haben:

   ![Screenshot des Anmeldebildschirms mit den Optionen für Google und Facebook](media/identity-providers/sign-in-with-social-identity.png)

Um für ein optimales Benutzererlebnis bei der Anmeldung zu sorgen, sollten Sie nach Möglichkeit einen Verbund mit Identitätsanbietern einrichten, damit Sie Ihren eingeladenen Gästen eine nahtlose Anmeldung ermöglichen, wenn sie auf Ihre Apps zugreifen.  

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um Informationen zum Hinzufügen von Identitätsanbietern für die Anmeldung bei Ihren Anwendungen zu erhalten:

- [Hinzufügen der Authentifizierung mit Einmalkennung per E-Mail](one-time-passcode.md)
- [Hinzufügen von Google](google-federation.md) als zulässiger Identitätsanbieter
- [Hinzufügen von Facebook](facebook-federation.md) als zulässiger Identitätsanbieter
- [Richten Sie einen direkten Verbund](direct-federation.md) mit jeder Organisation ein, deren Identitätsanbieter das SAML 2.0- oder WS-Fed-Protokoll unterstützt. Für Benutzerflows für die Self-Service-Registrierung ist der direkte Verbund keine Option.
