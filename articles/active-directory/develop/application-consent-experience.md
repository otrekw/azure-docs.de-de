---
title: Umgebungen für die Azure AD-App-Zustimmung
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Azure AD-Einwilligungserfahrungen und deren Verwendung beim Verwalten und Entwickeln von Anwendungen in Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 04/06/2021
ms.author: ryanwi
ms.reviewer: jesakowi, asteen
ms.openlocfilehash: c570fc9f30d69f13546353cf6edab4122ae35142
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107105412"
---
# <a name="understanding-azure-ad-application-consent-experiences"></a>Grundlegendes zu Einwilligungserfahrungen für Azure AD-Anwendungen

Erfahren Sie mehr über die Einwilligungsbenutzererfahrung für Azure Active Directory-Anwendungen (Azure AD). Sie können also auf intelligente Weise Anwendungen für Ihre Organisation verwalten und/oder Anwendungen mit einer nahtloseren Einwilligungserfahrung entwickeln.

## <a name="consent-and-permissions"></a>Einwilligung und Berechtigungen

Einwilligung ist der Prozess, mit dem ein Benutzer einer Anwendung die Autorisierung für den Zugriff auf geschützte Ressourcen in seinem Auftrag gewährt. Ein Administrator oder Benutzer kann zur Einwilligung aufgefordert werden, um den Zugriff auf seine Organisations-/individuellen Daten zuzulassen.

Die tatsächliche Benutzererfahrung des Erteilens der Einwilligung variiert in Abhängigkeit von den auf dem Mandanten des Benutzers festgelegten Richtlinien, dem Gültigkeitsbereich der Autorität (oder Rolle) des Benutzers und dem Typ der [Berechtigungen](v2-permissions-and-consent.md), die von der Clientanwendung angefordert werden. Dies bedeutet, dass Anwendungsentwickler und Mandantenadministratoren eine gewisse Kontrolle über die Einwilligungserfahrung haben. Administratoren besitzen die Flexibilität, Richtlinien für einen Mandanten oder eine App festzulegen oder zu deaktivieren, um die Einwilligungserfahrung in ihrem Mandanten zu kontrollieren. Anwendungsentwickler können vorgeben, welche Typen von Berechtigungen angefordert werden, und ob sie Benutzer durch den Benutzereinwilligungsflow bzw. den Administratoreinwilligungsflow führen möchten.

- **Benutzereinwilligungsflow** ist, wenn ein Anwendungsentwickler Benutzer mit der Absicht zum Autorisierungsendpunkt weiterleitet, die Einwilligung nur für den aktuellen Benutzer aufzuzeichnen.
- **Administratoreinwilligungsflow** ist, wenn ein Anwendungsentwickler Benutzer mit der Absicht zum Administratoreinwilligungs-Endpunkt weiterleitet, die Einwilligung für den gesamten Mandanten aufzuzeichnen. Um sicherzustellen, dass der Administratoreinwilligungsflow ordnungsgemäß funktioniert, müssen Anwendungsentwickler alle Berechtigungen in der Eigenschaft `RequiredResourceAccess` im Anwendungsmanifest auflisten. Weitere Informationen finden Sie unter [Anwendungsmanifest](./reference-app-manifest.md).

## <a name="building-blocks-of-the-consent-prompt"></a>Bausteine der Einwilligungsaufforderung

Die Einwilligungsaufforderung ist darauf ausgelegt, sicherzustellen, dass Benutzer über ausreichende Informationen verfügen, um zu bestimmen, ob sie der Clientanwendung so vertrauen, dass sie auf geschützte Ressourcen in ihrem Namen zugreifen darf. Das Verständnis der Bausteine hilft Benutzern, die Ihre Einwilligung geben, fundiertere Entscheidungen zu treffen, und hilft Entwicklern, bessere Benutzererfahrungen zu erstellen.

Das Diagramm und die Tabelle im Folgenden bieten Informationen über die Bausteine der Einwilligungsaufforderung.

:::image type="content" source="./media/application-consent-experience/consent_prompt.png" alt-text="Bausteine der Einwilligungsaufforderung":::

| # | Komponente | Zweck |
| ----- | ----- | ----- |
| 1 | Benutzer-ID | Dieser Bezeichner stellt den Benutzer dar, von dem die Clientanwendung den Zugriff auf geschützte Ressourcen in seinem Auftrag anfordert. |
| 2 | Titel | Der Titel ändert sich basierend darauf, ob die Benutzer den Benutzer- oder Administratoreinwilligungsflow durchlaufen. Im Benutzereinwilligungsflow lautet der Titel „Berechtigungen“, während er im Administratoreinwilligungsflow eine zusätzliche Zeile namens „Für Ihre Organisation zustimmen“ besitzt. |
| 3 | App-Logo | Dieses Bild sollte Benutzern einen visuellen Hinweis geben, ob diese App die App ist, auf die sie zugreifen wollten. Dieses Bild wird von den Anwendungsentwicklern bereitgestellt, und der Besitz für dieses Bild wird nicht überprüft. |
| 4 | App-Name | Dieser Wert sollte Benutzer darüber informieren, welche Anwendung Zugriff auf ihre Daten anfordert. Beachten Sie, dass dieser Name von den Entwicklern bereitgestellt wird, und dass der Besitz an diesem App-Namen nicht überprüft wird. |
| 5 | Herausgeberdomäne | Dieser Wert sollte Benutzern eine Domäne bereitstellen, deren Vertrauenswürdigkeit sie möglicherweise bewerten können. Diese Domäne wird von den Entwicklern bereitgestellt, und der Besitz an dieser Herausgeberdomäne wird überprüft. |
| 6 | Verifizierter Herausgeber | Der blaue Badge „verifiziert“ bedeutet, dass der App-Herausgeber seine Identität mithilfe eines Microsoft Partner Network-Kontos bestätigt und den Verifizierungsprozess abgeschlossen hat.|
| 7 | Informationen zum Herausgeber  | Zeigt an, ob die Anwendung von Microsoft oder Ihrer Organisation veröffentlicht wird. |
| 8 | Berechtigungen | Diese Liste enthält die Berechtigungen, die von der Clientanwendung angefordert werden. Benutzer sollten die Typen der angeforderten Berechtigungen immer bewerten, um zu verstehen, auf welche Daten der Zugriff der Clientanwendung in ihrem Namen autorisiert wird, wenn sie zustimmen. Als Anwendungsentwickler ist es am besten, den Zugriff auf die Berechtigungen mit den geringsten Rechten anzufordern. |
| 9 | Berechtigungsbeschreibung | Dieser Wert wird vom Dienst bereitgestellt, der die Berechtigungen verfügbar macht. Um die Berechtigungsbeschreibung anzuzeigen, müssen Sie das Chevron neben den Berechtigungen umschalten. |
| 10| App-Nutzungsbedingungen | Diese Nutzungsbedingungen enthalten Links zu den Nutzungsbedingungen und Datenschutzbestimmungen der Anwendung. Der Herausgeber ist verantwortlich für die Darlegung ihrer Regeln in ihren Nutzungsbedingungen. Darüber hinaus ist der Herausgeber dafür verantwortlich, in den Datenschutzbestimmungen die Art zu veröffentlichen, in der sie Benutzerdaten verwenden und teilen. Stellt der Verleger Herausgeber keine Links zu diesen Werten für mehrinstanzenfähige Anwendungen bereit, wird eine fett formatierte Warnung in der Einwilligungsaufforderung angezeigt. |
| 11 | https://myapps.microsoft.com | Dies ist der Link, unter dem Benutzer alle Nicht-Microsoft-Anwendungen überprüfen und entfernen können, die zurzeit Zugriff auf ihre Daten haben. |
| 12 | Hier melden | Dieser Link dient zum Melden einer verdächtigen App, wenn Sie der App nicht vertrauen, wenn Sie glauben, dass die App die Identität einer anderen App annimmt oder Ihre Daten missbraucht, oder aus einem anderen Grund. |

## <a name="app-requires-a-permission-within-the-users-scope-of-authority"></a>App erfordert eine Berechtigung innerhalb des Autoritätsbereichs des Benutzers

Ein gängiges Zustimmungsszenario ist, dass der Benutzer auf eine App zugreift, für die ein Berechtigungssatz erforderlich ist, der innerhalb des Autoritätsbereichs des Benutzers liegt. Der Benutzer wird an den Benutzerzustimmungsflow weitergeleitet.

Administratoren wird in der herkömmlichen Einwilligungsaufforderung ein zusätzliches Steuerelement angezeigt, das es ihnen gestattet, die Einwilligung im Auftrag des gesamten Mandanten zu geben. Das Steuerelement ist standardmäßig deaktiviert, weshalb auch nur, wenn Administratoren das Kontrollkästchen ausdrücklich aktivieren, die Einwilligung im Auftrag des gesamten Mandanten erklärt wird. Ab sofort wird dieses Kontrollkästchen nur für die Rolle „Globaler Administrator“ angezeigt, sodass dieses Kontrollkästchen den Rollen „Cloudadministrator“ und „App-Administrator“ nicht angezeigt werden.

:::image type="content" source="./media/application-consent-experience/consent_prompt_1a.png" alt-text="Einwilligungsaufforderung für Szenario 1a":::

Benutzern wird die herkömmliche Einwilligungsaufforderung angezeigt.

:::image type="content" source="./media/application-consent-experience/consent_prompt_1b.png" alt-text="Screenshot der herkömmlichen Einwilligungsaufforderung.":::

## <a name="app-requires-a-permission-outside-of-the-users-scope-of-authority"></a>App erfordert eine Berechtigung außerhalb des Autoritätsbereichs des Benutzers

Ein weiteres häufiges Zustimmungsszenario ist, dass der Benutzer auf eine App zugreift, für die mindestens eine Berechtigung erforderlich ist, die außerhalb des Autoritätsbereichs des Benutzers liegt.

Administratoren wird in der herkömmlichen Einwilligungsaufforderung ein zusätzliches Steuerelement angezeigt, das es ihnen gestattet, die Einwilligung im Auftrag des gesamten Mandanten zu geben.

:::image type="content" source="./media/application-consent-experience/consent_prompt_1a.png" alt-text="Einwilligungsaufforderung für Szenario 1a":::

Benutzer, die keine Administratoren sind, werden am Erteilen ihrer Zustimmung für die Anwendung gehindert und darauf hingewiesen, ihren Administrator um Zugriff auf die App zu bitten.

:::image type="content" source="./media/application-consent-experience/consent_prompt_2b.png" alt-text="Screenshot der Einwilligungsaufforderung, die den Benutzer auffordert, einen Administrator um Zugriff auf die App zu bitten.":::

## <a name="user-is-directed-to-the-admin-consent-flow"></a>Benutzer wird zum Administratoreinwilligungsflow weitergeleitet

Ein weiteres häufiges Szenario ist, wenn der Benutzer zum Administratoreinwilligungsflow navigiert oder zu diesem weitergeleitet wird.

Administratorbenutzern wird die Administratoreinwilligungsaufforderung angezeigt. Der Titel und die Beschreibung der Berechtigungen hat sich in dieser Eingabeaufforderung geändert, und die Änderungen unterstreichen die Tatsache, dass das Annehmen dieser Aufforderung der App Zugriff auf die angeforderten Daten im Auftrag des gesamten Mandanten erteilt.

:::image type="content" source="./media/application-consent-experience/consent_prompt_3a.png" alt-text="Einwilligungsaufforderung für Szenario 3a":::

Benutzer, die keine Administratoren sind, werden am Erteilen ihrer Zustimmung für die Anwendung gehindert und darauf hingewiesen, ihren Administrator um Zugriff auf die App zu bitten.

:::image type="content" source="./media/application-consent-experience/consent_prompt_2b.png" alt-text="Screenshot der Einwilligungsaufforderung, die den Benutzer auffordert, einen Administrator um Zugriff auf die App zu bitten.":::

## <a name="next-steps"></a>Nächste Schritte

- Eine ausführliche Übersicht über die [Zustimmungsimplementierung mit dem Azure AD-Zustimmungsframework](./quickstart-register-app.md)
- Ausführlichere Informationen zur [Verwendung des Zustimmungsframeworks durch mehrinstanzenfähige Anwendungen](./howto-convert-app-to-be-multi-tenant.md) zur Implementierung von Benutzer- und Administratorzustimmung mit Unterstützung für erweiterte Anwendungsmuster mit mehreren Ebenen
- Erfahren Sie, [wie Sie die Herausgeberdomäne der App konfigurieren](howto-configure-publisher-domain.md).