---
title: Elemente der B2B-Einladungs-E-Mail – Azure Active Directory | Microsoft-Dokumentation
description: Vorlage für die Einladungs-E-Mail von Azure Active Directory B2B-Zusammenarbeit
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0429cfb62c319675806d76b4759b776a7b32dbcb
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87907455"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>Die Elemente der Einladungs-E-Mail für die B2B-Zusammenarbeit – Azure Active Directory

Einladungs-E-Mails sind eine wichtige Komponente zum Onboarding von Partnern als Benutzer von B2B-Zusammenarbeit in Azure AD. Auch wenn es [nicht erforderlich ist, eine E-Mail zu senden, um eine Person mit B2B Collaboration einzuladen](add-user-without-invite.md), erhalten die Benutzer auf diese Weise alle benötigten Informationen, um entscheiden zu können, ob sie Ihre Einladung akzeptieren. Außerdem erhalten sie einen Link, auf den sie in Zukunft zurückgreifen können, wenn sie zu Ihren Ressourcen zurückkehren möchten.

![Screenshot, der die B2B-Einladungs-E-Mail zeigt](media/invitation-email-elements/invitation-email.png)

> [!NOTE]
> Diese neue E-Mail-Vorlage wird derzeit noch für alle Mandanten eingeführt, sodass einige Mandanten immer noch ein älteres Design verwenden. Ab Ende Mai 2020 wird diese Vorlage in Einladungen aller Mandanten verwendet.

## <a name="explaining-the-email"></a>Erläuterungen zur E-Mail

Sehen wir uns einige Elemente der E-Mail an, damit Sie diese Funktionen optimal nutzen können.

### <a name="subject"></a>Subject

Der Betreff der E-Mail folgt diesem Muster:

&lt;Benutzername&gt; hat Sie zum Zugriff auf Anwendungen in seiner Organisation eingeladen.

### <a name="from-address"></a>Absenderadresse

Wir verwenden ein LinkedIn-ähnliches Muster für die Absenderadresse. Dieses Muster sollte deutlich machen, dass die E-Mail zwar von invites@microsoft.com stammt, die Einladung jedoch von einer anderen Organisation. Das Format lautet: Microsoft-Einladungen <invites@microsoft.com> oder Microsoft-Einladungen im Namen von &lt;Mandantenname&gt; <invites@microsoft.com>. 

### <a name="reply-to"></a>Antworten an

Die Antwort-E-Mail-Adresse wird auf die E-Mail-Adresse des einladenden Benutzers festgelegt (falls verfügbar). Wenn eine Antwort auf die E-Mail gesendet wird, gelangt diese zurück an den einladenden Benutzer.

### <a name="phishing-warning"></a>Phishingwarnung

Die E-Mail beginnt mit einer kurzen Warnung für den Benutzer über Phishing und benachrichtigt ihn, dass er nur erwartete Einladungen akzeptieren sollte. Sie sollten sicherstellen, dass die Partner, die Sie einladen, von Ihrer Einladung nicht überrascht werden, indem Sie sie im Vorfeld erwähnen.

![Abbildung der Phishingwarnung in der E-Mail](media/invitation-email-elements/phishing-warning.png)

### <a name="inviters-information"></a>Informationen zum einladenden Benutzer

Die E-Mail enthält Informationen zu der einladenden Person und der Organisation, von der die Einladung gesendet wird. Dies umfasst den Namen und die E-Mail-Adresse des Absenders sowie den Namen und die primäre Domäne, die der Organisation zugeordnet sind. Alle diese Informationen sollen der eingeladenen Person dabei helfen, eine fundierte Entscheidung zum Akzeptieren der Einladung zu treffen.

![Abbildung der Informationen der einladenden Person in der E-Mail](media/invitation-email-elements/inviters-information.png)

### <a name="invitation-message"></a>Einladungsnachricht

Wenn die einladende Person beim [Einladen eines Gastbenutzers in das Verzeichnis, die Gruppe oder die App](add-users-administrator.md) oder bei [Verwendung der Einladungs-API](customize-invitation-api.md) eine Nachricht einschließt, wird diese im Hauptabschnitt der E-Mail hervorgehoben. Außerdem sind der Name der einladenden Person und ihr Profilbild enthalten, sofern sie eines festgelegt hat. Die Meldung selbst ist ein Textbereich, daher werden aus Sicherheitsgründen keine HTML-Tags verarbeitet.

![Abbildung der Einladungsnachricht in der E-Mail](media/invitation-email-elements/invitation-message.png)

### <a name="accept-button-and-redirect-url"></a>Schaltfläche „Akzeptieren“ und Umleitungs-URL

Der nächste Abschnitt der E-Mail enthält Informationen darüber, wohin die eingeladene Person nach dem Akzeptieren der Einladung geleitet wird, sowie eine Schaltfläche zu diesem Zweck.  In Zukunft kann die eingeladene Person diesen Link immer verwenden, um direkt zu Ihren Ressourcen zurückzukehren.

![Abbildung der Schaltfläche „Akzeptieren“ und der Umleitungs-URL in der E-Mail](media/invitation-email-elements/accept-button.png)

### <a name="footer-section"></a>Fußzeilenabschnitt

Die Fußzeile enthält weitere Informationen zu der gesendeten Einladung. Es gibt immer eine Option, mit der die eingeladene Person zukünftige Einladungen blockieren kann. Wenn die Organisation [eine Datenschutzerklärung festgelegt hat](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-properties-area), wird hier der entsprechende Link angezeigt.  Andernfalls zeigt ein Hinweis an, dass die Organisation keine Datenschutzerklärung festgelegt hat.

![Abbildung des Footerabschnitts in der E-Mail](media/invitation-email-elements/footer-section.png)
 
## <a name="how-the-language-is-determined"></a>So wird die Sprache bestimmt

Die den Gastbenutzern in der Einladungs-E-Mail angezeigte Sprache richtet sich nach den folgenden Einstellungen. Diese Einstellungen sind in der Reihenfolge ihres Auftretens aufgeführt. Wenn eine Einstellung nicht konfiguriert ist, bestimmt die nächste Einstellung in der Liste die Sprache.

- Die **messageLanguage**-Eigenschaft des [invitedUserMessageInfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0)-Objekts, wenn die API zum Erstellen von Einladungen verwendet wird
-   Die **preferredLanguage**-Eigenschaft, die im [user-Objekt](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0) des Gasts angegeben ist
-   Die **Benachrichtigungssprache**, die in den Eigenschaften des Stammmandanten des Gastbenutzers (nur bei Azure AD-Mandanten) festgelegt wurde
-   Die **Benachrichtigungssprache**, die in den Eigenschaften des Ressourcenmandanten festgelegt ist

Wenn keine dieser Einstellungen konfiguriert wurde, wird als Standardsprache „Englisch (USA)“ verwendet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](add-users-administrator.md)
- [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](add-users-information-worker.md)
- [B2B-Zusammenarbeit: Einlösen von Einladungen](redemption-experience.md)
- [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](add-user-without-invite.md)
