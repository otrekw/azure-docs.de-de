---
title: Fortlaufende Zugriffsevaluierung in Azure AD
description: Schnellere Reaktion auf Benutzerstatusänderungen mit der fortlaufenden Zugriffsevaluierung in Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jlu
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf904bb2c0d133ea07cd32274fad5b6601da5d9
ms.sourcegitcommit: 2721b8d1ffe203226829958bee5c52699e1d2116
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84148122"
---
# <a name="continuous-access-evaluation"></a>Fortlaufende Zugriffsevaluierung

Microsoft-Dienste wie Azure Active Directory (Azure AD) und Office 365 verwenden offene Standards und Protokolle, um die Interoperabilität zu maximieren. Einer der wichtigsten Standards ist Open ID Connect (OIDC). Wenn eine Clientanwendung wie Outlook eine Verbindung mit einem Dienst wie Exchange Online herstellt, werden die API-Anforderungen mithilfe von OAuth 2.0-Zugriffstoken autorisiert. Standardmäßig sind diese Zugriffstoken eine Stunde lang gültig. Nach Ablauf dieser Zeit wird der Client zurück an Azure AD geleitet, um die Token zu aktualisieren. Das bietet auch eine Möglichkeit, Richtlinien für den Benutzerzugriff neu auszuwerten – möglicherweise soll das Token nicht aktualisiert werden, zum Beispiel aufgrund einer Richtlinie für bedingten Zugriff oder weil der Benutzer im Verzeichnis deaktiviert wurde. 

Tokenablauf und -aktualisierung sind Standardmechanismen der Branche. Viele Kunden haben Bedenken wegen der Verzögerung geäußert, die zwischen der Änderung der Risikobedingungen für Benutzer (z. B. beim Wechsel vom Firmenbüro in ein lokales Café oder beim Entdecken der Benutzeranmeldeinformationen auf dem Schwarzmarkt) und dem Erzwingen von Richtlinien im Zusammenhang mit dieser Änderung auftritt. Wir haben mit dem als „Blunt Object“ (stumpfer Gegenstand) bezeichneten Ansatz mit reduzierten Tokenlebensdauer experimentiert, dabei aber festgestellt, dass die Benutzerfreundlichkeit und Zuverlässigkeit beeinträchtigt wurden, ohne die Risiken auszuschließen.

Die rechtzeitige Reaktion auf Richtlinienverstöße oder Sicherheitsprobleme erfordert einen echten Austausch zwischen dem Tokenaussteller (z. B. Azure AD) und der vertrauenden Seite (z. B. Exchange Online). Dieser bidirektionale Austausch bietet zwei wichtige Funktionen. Die vertrauende Seite kann sehen, wann Änderungen vorgenommen wurden, z. B. wenn sich ein Client an einem neuen Standort befindet, und den Aussteller des Tokens informieren. Außerdem ermöglicht dies dem Tokenaussteller, der vertrauenden Seite mitzuteilen, keine Token für einen bestimmten Benutzer anzunehmen, da dessen Konto kompromittiert oder deaktiviert wurde oder da andere Bedenken bestehen. Der Mechanismus für diesen Austausch ist die fortlaufende Zugriffsevaluierung (Continuous Access Evaluation, CAE).

Microsoft hat sich schon früh im Rahmen der Arbeitsgruppe [Shared Signals and Events](https://openid.net/wg/sse/) bei der OpenID Foundation an der CAEP-Initiative (Continuous Access Evaluation Protocol) beteiligt. Identitätsanbieter und vertrauende Seiten können die Sicherheitsereignisse und Signale, die von der Arbeitsgruppe definiert wurden, zum erneuten Autorisieren oder Beenden des Zugriffs nutzen. Diese Arbeit ist sehr interessant und wird die Sicherheit auf vielen Plattformen und bei vielen Anwendungen verbessern.

Aufgrund der enormen Sicherheitsvorteile führen wir parallel zu unserer ununterbrochenen Arbeit in den Normungsgremien eine erste Microsoft-spezifische Implementierung ein. Während wir an der Bereitstellung der Funktionen für die fortlaufende Zugriffsevaluierung (Continuous Access Evaluation, CAE) für Microsoft-Dienste arbeiten, haben wir viel gelernt und teilen diese Erfahrungen mit den Normungsgremien. Wir hoffen, dass unsere Erfahrungen bei der Bereitstellung zu einem noch besseren Industriestandard beitragen, und sind bestrebt, diesen Standard nach seiner Ratifizierung zu implementieren, damit alle beteiligten Dienste davon profitieren können.

## <a name="how-does-cae-work-in-microsoft-services"></a>Wie funktioniert die fortlaufende Zugriffsevaluierung (CAE) in Microsoft-Diensten?

Wir konzentrieren uns auf die anfängliche Implementierung der fortlaufenden Zugriffsevaluierung in Exchange und Teams. Wir hoffen, die Unterstützung in Zukunft auch auf andere Microsoft-Dienste ausweiten zu können. Wir beginnen mit der Aktivierung der fortlaufenden Zugriffsevaluierung ausschließlich bei Mandanten, die keine Richtlinien für bedingten Zugriff haben. Unsere Erkenntnisse aus dieser Phase der fortlaufenden Zugriffsevaluierung werden in das laufende CAE-Rollout einfließen.

## <a name="service-side-requirements"></a>Dienstseitige Anforderungen

Die fortlaufende Zugriffsevaluierung wird implementiert, indem wir Diensten (Ressourcenanbietern) die Möglichkeit geben, kritische Ereignisse in Azure AD zu abonnieren, sodass diese Ereignisse nahezu in Echtzeit ausgewertet und erzwungen werden können. In diesem ersten CAE-Rollout werden die folgenden Ereignisse erzwungen:

- Benutzerkonto wird gelöscht oder deaktiviert
- Kennwort für einen Benutzer wird geändert oder zurückgesetzt
- Administrator sperrt explizit alle Aktualisierungstoken für einen Benutzer
- Azure AD Identity Protection hat ein erhöhtes Benutzerrisiko erkannt

Wir hoffen, in Zukunft weitere Ereignisse (z. B. Standort- und Gerätestatusänderungen) hinzufügen zu können. **Auch wenn unser Ziel eine sofortige Erzwingung ist, kann in einigen Fällen durch die Ereignispropagierungszeit eine Latenz von bis zu 15 Minuten beobachtet werden**. 

## <a name="client-side-claim-challenge"></a>Clientseitige Anspruchsaufforderung

Vor der fortlaufenden Zugriffsevaluierung versuchten Clients immer, das Zugriffstoken aus dem Cache wiederzugeben, solange es nicht abgelaufen war. Mit der fortlaufenden Zugriffsevaluierung wird ein neuer Fall eingeführt. Ein Ressourcenanbieter kann ein Token ablehnen, auch wenn es nicht abgelaufen ist. Um Clients zu informieren, den Cache zu umgehen, auch wenn die zwischengespeicherten Token nicht abgelaufen sind, führen wir einen Mechanismus mit dem Namen **Anspruchsaufforderung** ein. Für die fortlaufende Zugriffsevaluierung ist ein Client-Update erforderlich, damit der Client die Anspruchsaufforderung verstehen kann. Die aktuelle Version der folgenden Anwendungen unterstützt die Anspruchsaufforderung:

- Outlook für Windows 
- Outlook für iOS 
- Outlook für Android 
- Outlook für Mac 
- Teams für Windows
- Teams für iOS 
- Teams für Android 
- Teams für Mac 

## <a name="token-lifetime"></a>Tokengültigkeitsdauer

Da Risiken und Richtlinien in Echtzeit ausgewertet werden, verlassen sich Clients, die Sitzungen mit fortlaufender Zugriffsevaluierung (CAE) aushandeln, auf CAE und nicht auf die vorhandenen statischen Richtlinien zur Gültigkeitsdauer von Zugriffstoken. Das bedeutet, dass konfigurierbare Richtlinien zur Tokengültigkeitsdauer von CAE-fähigen Clients, die Sitzungen mit CAE-Unterstützung aushandeln, nicht mehr berücksichtigt werden.

In CAE-Sitzungen werden wir die Gültigkeitsdauer von Zugriffstoken auf 24 Stunden erhöhen. Sperrungen werden von kritischen Ereignissen und der Richtlinienauswertung, nicht von einem willkürlichen Zeitraum gesteuert. Diese Änderung erhöht die Stabilität Ihrer Anwendungen, ohne den Sicherheitsstatus zu beeinträchtigen. 

## <a name="example-flows"></a>Beispielabläufe

### <a name="user-revocation-event-flow"></a>Ablauf bei Benutzersperrereignis:

![Ablauf bei Benutzersperrereignis](./media/concept-fundamentals-continuous-access-evaluation/user-revocation-event-flow.png)

1. Ein CAE-fähiger Client stellt Anmeldeinformationen oder ein Aktualisierungstoken für Azure AD bereit und fordert ein Zugriffstoken für eine Ressource an.
1. Ein Zugriffstoken wird zusammen mit anderen Artefakten an den Client zurückgegeben.
1. Ein Administrator [sperrt explizit alle Aktualisierungstoken für den Benutzer](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Von Azure AD wird ein Sperrereignis an den Ressourcenanbieter gesendet.
1. Dem Ressourcenanbieter wird ein Zugriffstoken präsentiert. Der Ressourcenanbieter wertet die Gültigkeit des Tokens aus und überprüft, ob für den Benutzer ein Sperrereignis vorliegt. Der Ressourcenanbieter verwendet diese Informationen, um den Zugriff auf die Ressource zu gewähren oder zu untersagen.
1. In diesem Fall verweigert der Ressourcenanbieter den Zugriff und sendet die Anspruchsaufforderung „401+“ an den Client zurück.
1. Der CAE-fähige Client versteht die Anspruchsaufforderung „401+“. Er umgeht die Caches, geht zurück zu Schritt 1 und sendet das Aktualisierungstoken zusammen mit der Anspruchsaufforderung zurück an Azure AD. Azure AD wertet dann alle Bedingungen erneut aus und fordert in diesem Fall den Benutzer auf, sich erneut zu authentifizieren.
 
## <a name="faqs"></a>Häufig gestellte Fragen

### <a name="what-is-the-lifetime-of-my-access-token"></a>Welche Gültigkeitsdauer hat mein Zugriffstoken?

Wenn Sie keine CAE-fähigen Clients verwenden, beträgt die standardmäßige Gültigkeitsdauer für das Zugriffstoken weiterhin 1 Stunde, es sei denn, Sie haben die Gültigkeitsdauer des Zugriffstokens mit der Previewfunktion [Konfigurierbare Tokengültigkeitsdauer (Configurable Token Lifetime, CTL)](../develop/active-directory-configurable-token-lifetimes.md) konfiguriert.

Wenn Sie CAE-fähige Clients verwenden, die Sitzungen mit CAE-Unterstützung aushandeln, werden Ihre CTL-Einstellungen für die Gültigkeitsdauer von Zugriffstoken außer Kraft gesetzt, und die Gültigkeitsdauer der Zugriffstoken beträgt 24 Stunden.

### <a name="how-quick-is-enforcement"></a>Wie schnell erfolgt die Erzwingung?

Auch wenn unser Ziel eine sofortige Erzwingung ist, kann in einigen Fällen durch die Ereignispropagierungszeit eine Latenz von bis zu 15 Minuten beobachtet werden.

### <a name="how-will-cae-work-with-sign-in-frequency"></a>Wie funktioniert die fortlaufende Zugriffsevaluierung (CAE) mit der Anmeldehäufigkeit?

Die Anmeldehäufigkeit wird mit oder ohne CAE berücksichtigt.

## <a name="next-steps"></a>Nächste Schritte

[Ankündigung der fortlaufenden Zugriffsevaluierung](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/moving-towards-real-time-policy-and-security-enforcement/ba-p/1276933)
