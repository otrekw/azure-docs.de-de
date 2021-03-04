---
title: Steuern lokaler Dienstkonten | Azure Active Directory
description: Leitfaden für das Erstellen und Ausführen von Lebenszyklusprozessen für Dienstkonten
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 2/15/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88fdfa1f449a0b65861ee09f2e78055a606c99d3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101649205"
---
# <a name="governing-on-premises-service-accounts"></a>Steuern lokaler Dienstkonten

In Windows Active Directory gibt es vier Arten lokaler Dienstkonten:

* [Gruppenverwaltete Dienstkonten](service-accounts-group-managed.md) (Group managed service accounts, gMSAs)

* [Eigenständige verwaltete Dienstkonten](service-accounts-standalone-managed.md) (Standalone managed service accounts, sMSAs)

* [Computerkonten](service-accounts-computer.md)

* [Benutzerkonten, die als Dienstkonten fungieren](service-accounts-user-on-premises.md)


Die enge Steuerung von Dienstkonten ist aus folgenden Gründen wichtig: 

* Schützen von Dienstkonten aufgrund der Anforderungen und dem Zweck der Anwendungsfälle

* Verwalten des Lebenszyklus von Dienstkonten und der zugehörigen Anmeldeinformationen

* Bewerten von Dienstkonten anhand des Risikos, dem sie ausgesetzt sind, und den ihnen zugeordneten Berechtigungen 

* Sicherstellen, dass Active Directory und Azure Active Directory keine veralteten Dienstkonten mit potenziell weitreichenden Berechtigungen enthalten

## <a name="principles-for-creating-a-new-service-account"></a>Grundsätze für das Erstellen eines neuen Dienstkontos

Verwenden Sie beim Erstellen eines neuen Dienstkontos die folgenden Kriterien.

| Grundsätze| Überlegungen | 
| - |- | 
| Zuordnen von Dienstkonten| Binden Sie das Dienstkonto an einen einzelnen Dienst, eine einzelne Anwendung oder ein einzelnes Skript. |
| Besitz| Stellen Sie sicher, dass es einen Besitzer gibt, der die Verantwortung für das Konto anfordert und übernimmt. |
| Bereich| Definieren Sie den Bereich eindeutig, und schätzen Sie die Nutzungsdauer für das Dienstkonto. |
| Zweck| Erstellen Sie Dienstkonten für einen bestimmten einzelnen Zweck. |
| Berechtigung| Wenden Sie das Prinzip der geringsten Rechte an, indem Sie Folgendes beachten: <br>Weisen Sie Berechtigungen niemals integrierten Gruppen wie Administratoren zu.<br> Entfernen Sie nach Bedarf die Berechtigungen des lokalen Computers.<br>Passen Sie den Zugriff an, und verwenden Sie für den Verzeichniszugriff die Active Directory-Delegierung.<br>Verwenden Sie genau abgestimmte Zugriffsberechtigungen.<br>Legen Sie Kontoablaufdaten und standortbasierte Einschränkungen für benutzerbasierte Dienstkonten fest. |
| Nutzungsüberwachung| Überwachen Sie die Anmeldedaten, und stellen Sie sicher, dass sie der vorgesehenen Nutzung entsprechen. Legen Sie Warnungen bei ungewöhnlicher Nutzung fest. |

### <a name="enforce-least-privilege-for-user-accounts-and-limit-account-overuse"></a>Erzwingen der geringsten Rechte für Benutzerkonten und Einschränken einer übermäßigen Kontonutzung

Verwenden Sie die folgenden Einstellungen für Benutzerkonten, die als Dienstkonten verwendet werden:

* [**Kontoablaufdatum**](/powershell/module/activedirectory/set-adaccountexpiration?view=winserver2012-ps): Legen Sie für das Dienstkonto fest, dass es im Anschluss an den Überprüfungszeitraum nach einer bestimmten Zeit automatisch abläuft, sofern keine weitere Nutzung explizit festgelegt wird.

*  **Anmeldearbeitsstationen**: Schränken Sie die Berechtigungen auf die Standorte ein, von denen sich das Dienstkonto anmelden kann. Wenn es lokal auf einem Computer ausgeführt wird und nur auf Ressourcen auf diesem Computer zugreift, unterbinden Sie die Anmeldung von einem anderen Standort.

* [**Keine Kennwortänderung**](/powershell/module/addsadministration/set-aduser?view=win10-ps): Verhindern Sie, dass das Dienstkonto sein eigenes Kennwort ändert, indem Sie den Parameter auf „False“ festlegen.

 
## <a name="build-a-lifecycle-management-process"></a>Erstellen eines Lebenszyklusverwaltungsprozesses

Um die Sicherheit Ihrer Dienstkonten zu gewährleisten, ist eine Verwaltung vom Ermitteln des Bedarfs bis zur Außerbetriebnahme der Konten erforderlich. 

Verwenden Sie für die Lebenszyklusverwaltung von Dienstkonten den folgenden Prozess:

1. Sammeln von Nutzungsinformationen für das Konto
1. Integrieren des Dienstkontos und der App in die Konfigurationsverwaltungsdatenbank (Configuration Management Database, CMDB)
1. Durchführen einer Risikobewertung oder formalen Überprüfung
1. Erstellen des Dienstkontos und Anwenden von Einschränkungen
1. Planen und Ausführen wiederholter Überprüfungen Anpassen von Berechtigungen und Bereichen nach Bedarf
1. Aufheben der Kontobereitstellung nach Bedarf

### <a name="collect-usage-information-for-the-service-account"></a>Sammeln von Nutzungsinformationen für das Dienstkonto

Sammeln Sie für jedes Dienstkonto die relevanten geschäftlichen Informationen. In der folgenden Tabelle sind die mindestens zu sammelnden Informationen aufgeführt. Allerdings sollten Sie alle Informationen erfassen, die für das Geschäftsszenario und die Existenz des Kontos erforderlich sind.

| Daten| Details |
| - | - |
| Besitzer| Benutzer oder Gruppe, der bzw. die für das Dienstkonto verantwortlich ist |
| Zweck| Zweck des Dienstkontos |
| Berechtigungen (Bereiche)| Voraussichtlicher Satz von Berechtigungen |
| Links zur Konfigurationsverwaltungsdatenbank (CMDB)| Querverbindungen zwischen Dienstkonto und Zielskript/Zielanwendung und Besitzer(n) |
| Risiko| Bewertung von Risiken und geschäftlicher Auswirkungen anhand der Sicherheitsrisikobewertung |
| Lebensdauer| Voraussichtliche maximale Lebensdauer, um die Planung des Kontoablaufdatums oder der Neuzertifizierung zu ermöglichen |


 

Fordern Sie im Idealfall ein Self-Service-Konto und die entsprechenden Informationen an. Der Besitzer kann Besitzer einer Anwendung oder geschäftlicher Besitzer, Mitglied der IT oder Infrastrukturbesitzer sein. Durch die Verwendung eines Tools wie Microsoft Forms für diese Anforderung und die zugehörigen Informationen wird nach der Genehmigung des Kontos die Portierung zu Ihrem CMDB-Bestandstool vereinfacht.

### <a name="onboard-service-account-to-cmdb"></a>Integrieren von Dienstkonten in die Konfigurationsverwaltungsdatenbank (CMDB)

Speichern Sie die gesammelten Informationen in einer CMDB-artigen Anwendung. Nehmen Sie neben den Geschäftsinformationen auch alle Abhängigkeiten von anderer Infrastruktur, Apps und Prozessen hinzu.  Dieses zentrale Repository vereinfacht folgende Vorgänge:

* Risikobewertung

* Konfigurieren der Dienstkonten mit den erforderlichen Einschränkungen

* Erkennen relevanter Abhängigkeiten in Bezug auf Funktion und Sicherheit

* Ausführen regelmäßiger Überprüfungen auf Sicherheit und weiteren Nutzungsbedarf

* Kontaktaufnahme mit dem/den Besitzer(n) zwecks Überprüfen, Beenden und Ändern des Dienstkontos

Stellen Sie sich ein Dienstkonto vor, das zum Ausführen einer Website verwendet wird und über Berechtigungen zum Herstellen einer Verbindung mit einer oder mehreren SQL-Datenbanken verfügt. Die in der CMDB für dieses Dienstkonto gespeicherten Informationen könnten wie folgt lauten:

|Daten | Details|
| - | - |
| Besitzer, Stellvertreter| John Bloom, Anna Mayers |
| Zweck| Ausführen der HR-Webseite und Herstellen einer Verbindung mit den HR-Datenbanken. Kann beim Zugriff auf Datenbanken die Identität des Endbenutzers annehmen. |
| Berechtigungen, Bereiche| HR-Webserver: lokale Anmeldung, Ausführen der Webseite<br>HR-SQL1: lokale Anmeldung, Lesen aller HR*-Datenbanken<br>HR-SQL2: lokale Anmeldung, Lesen der GEHÄLTER*-Datenbank |
| Kostenstelle| 883944 |
| Risikobewertung| Mittel, Geschäftliche Auswirkungen: Mittel, Private Informationen: Mittel |
| Kontoeinschränkungen| Anmelden bei: nur erwähnte Server, keine Kennwortänderung, MBI-Kennwortrichtlinie |
| Lebensdauer| Nicht eingeschränkt |
| Überprüfungszyklus| Halbjährlich (durch Besitzer, Sicherheitsteam, Datenschutz) |

### <a name="perform-risk-assessment-or-formal-review-of-service-account-usage"></a>Risikobewertung oder formale Überprüfung der Nutzung des Dienstkontos

Bewerten Sie anhand der Berechtigungen und des Kontozwecks das Risiko, das das Konto bei einer Kompromittierung für die zugeordnete Anwendung oder den zugehörigen Dienst und für Ihre Infrastruktur darstellen kann. Berücksichtigen Sie sowohl das direkte als auch das indirekte Risiko. 

* Worauf könnte ein Angreifer direkt zugreifen?

* Auf welche anderen Informationen oder Systeme kann das Dienstkonto zugreifen?

* Kann das Konto zum Erteilen zusätzlicher Berechtigungen verwendet werden?

* Wie erkennen Sie, wenn Berechtigungen geändert werden?

Nach Durchführung und Dokumentation kann die Risikobewertung Auswirkungen auf folgende Bereiche haben:

* Kontoeinschränkungen

* Kontolebensdauer

* Anforderungen an die Kontoüberprüfung (Intervalle und Prüfer)

### <a name="create-a-service-account-and-apply-account-restrictions"></a>Erstellen eines Dienstkontos und Anwenden von Kontoeinschränkungen

Erstellen Sie erst dann ein Dienstkonto, wenn relevante Informationen in ihrer CMDB dokumentiert sind und Sie eine Risikobewertung vorgenommen haben. Die Kontoeinschränkungen sollten sich nach der Risikobewertung richten. Beachten Sie die folgenden Einschränkungen, wenn sie für Ihre Bewertung relevant sind:

* [Kontoablaufdatum](/powershell/module/activedirectory/set-adaccountexpiration?view=winserver2012-ps)

   * Definieren Sie für alle Benutzerkonten, die als Dienstkonten verwendet werden, ein realistisches und definitives Enddatum für die Nutzung. Legen Sie diese Einstellung mit dem Flag „Konto läuft ab“ fest. Weitere Einzelheiten finden Sie unter [Set-ADAccountExpiration](/powershell/module/addsadministration/set-adaccountexpiration?view=win10-ps). 

* Anmelden bei ([Anmeldearbeitsstation](/powershell/module/addsadministration/set-aduser?view=win10-ps))

* Anforderungen für [Kennwortrichtlinien](../../active-directory-domain-services/password-policy.md)

* Erstellung an einem [Standort der Organisationseinheit](/windows-server/identity/ad-ds/plan/delegating-administration-of-account-ous-and-resource-ous), der die Verwaltung nur für berechtigte Benutzer gewährleistet

* Einrichten der Überwachung und Sammeln von Überwachungsdaten zum [Erkennen von Änderungen](/windows/security/threat-protection/auditing/audit-directory-service-changes) am Dienstkonto und zur [Nutzung des Dienstkontos](https://www.manageengine.com/products/active-directory-audit/how-to/audit-kerberos-authentication-events.html).

Wenn Sie bereit sind, das Konto für die Produktion freizugeben, gewähren Sie den Zugriff auf das Dienstkonto auf sichere Art und Weise. 

### <a name="schedule-regular-reviews-of-service-accounts"></a>Planen regelmäßiger Überprüfungen von Dienstkonten

Richten Sie für Dienstkonten, die bei der Risikobewertung als „mittel“ oder „hoch“ eingestuft wurden, regelmäßige Überprüfungen ein. Die Überprüfungen sollten Folgendes umfassen: 

* Bestätigung des Besitzers zur fortgesetzten Notwendigkeit des Kontos sowie Begründung von Berechtigungen und Bereichen

* Überprüfen durch Datenschutz- und Sicherheitsteams, einschließlich der Auswertung von Upstream- und Downstreamverbindungen

* Überwachungsdaten, um sicherzustellen, dass das Konto nur für die beabsichtigten Zwecke verwendet wird

### <a name="deprovision-service-accounts"></a>Aufheben der Bereitstellung von Dienstkonten

Entfernen Sie zum Aufheben der Bereitstellung zunächst die Berechtigungen und die Überwachung und dann das Konto (falls erforderlich).

Die Aufhebung der Bereitstellung von Dienstkonten erfolgt in folgenden Fällen:

* Das Skript oder die Anwendung, für das bzw. die das Dienstkonto erstellt wurde, wird eingestellt.

* Die Funktion innerhalb des Skripts oder der Anwendung, für das bzw. die das Dienstkonto verwendet wird (z. B. Zugriff auf eine bestimmte Ressource), wird eingestellt.

* Das Dienstkonto wurde durch ein anderes Dienstkonto ersetzt.

Nachdem Sie alle Berechtigungen entfernt haben, verwenden Sie diesen Prozess zum Entfernen des Kontos.

1. Nachdem die Bereitstellung der zugehörigen Anwendung oder des zugehörigen Skripts aufgehoben wurde, überwachen Sie die Anmeldungen und den Ressourcenzugriff für das zugehörige Dienstkonto (bzw. die Dienstkonten), um sicherzustellen, dass es nicht in einem anderen Prozess verwendet wird. Wenn Sie sicher sind, dass es nicht mehr benötigt wird, fahren Sie mit dem nächsten Schritt fort.

2. Deaktivieren Sie das Dienstkonto, sodass es sich nicht mehr anmelden kann, und stellen Sie sicher, dass es nicht mehr benötigt wird. Erstellen Sie eine geschäftliche Richtlinie für den Zeitraum, den Konten deaktiviert bleiben sollen.

3. Löschen Sie das Dienstkonto, nachdem die Richtlinie für das Bestehenbleiben deaktivierter Konten erfüllt wurde. 

   * Microsoft-Konten (MSAs) können Sie mithilfe von PowerShell [deinstallieren](/powershell/module/activedirectory/uninstall-adserviceaccount?view=winserver2012-ps) oder manuell aus dem Container für verwaltete Dienstkonten löschen.

   * Computer- oder Benutzerkonten können Sie manuell in Active Directory löschen.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel zum Schützen von Dienstkonten:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)

* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)

* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)

* [Schützen von Computerkonten](service-accounts-computer.md)

* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)

* [Steuern lokaler Dienstkonten](service-accounts-govern-on-premises.md)