---
title: Steuern lokaler Dienstkonten | Azure Active Directory
description: In diesem Leitfaden erfahren Sie, wie Sie einen Lebenszyklusprozess für Dienstkonten erstellen und ausführen.
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
ms.openlocfilehash: e02755f748cffcf7c523ce3b9c6fc651d2b1b02f
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111438916"
---
# <a name="govern-on-premises-service-accounts"></a>Steuern lokaler Dienstkonten

Active Directory bietet vier Arten von lokalen Dienstkonten:

* [Gruppenverwaltete Dienstkonten](service-accounts-group-managed.md) (Group managed service accounts, gMSAs)  
* [Eigenständige verwaltete Dienstkonten](service-accounts-standalone-managed.md) (Standalone managed service accounts, sMSAs)  
* [Computerkonten](service-accounts-computer.md)  
* [Benutzerkonten, die als Dienstkonten fungieren](service-accounts-user-on-premises.md)


Eine präzise Steuerung von Dienstkonten ist aus folgenden Gründen wichtig: 

* Schützen der Konten auf der Grundlage der Anforderungen und des Zwecks ihres jeweiligen Anwendungsfalls  
* Verwalten des Lebenszyklus der Konten sowie der zugehörigen Anmeldeinformationen  
* Bewerten der Konten anhand des Risikos, dem sie ausgesetzt sind, und der ihnen zugeordneten Berechtigungen  
* Sicherstellen, dass Active Directory und Azure Active Directory keine veralteten Dienstkonten mit potenziell weitreichenden Berechtigungen enthalten

## <a name="principles-for-creating-a-new-service-account"></a>Grundsätze für das Erstellen eines neuen Dienstkontos

Beim Erstellen eines Dienstkontos müssen die in der folgenden Tabelle aufgeführten Aspekte berücksichtigt werden:

| Prinzip| Aspekt | 
| - |- | 
| Zuordnen von Dienstkonten| Binden Sie das Dienstkonto an einen einzelnen Dienst, eine einzelne Anwendung oder ein einzelnes Skript. |
| Besitz| Stellen Sie sicher, dass es einen Besitzer gibt, der die Verantwortung für das Konto anfordert und übernimmt. |
| Bereich| Definieren Sie den Bereich eindeutig, und antizipieren Sie die Nutzungsdauer für das Dienstkonto. |
| Zweck| Erstellen Sie Dienstkonten für einen einzelnen bestimmten Zweck. |
| Berechtigungen | Folgen Sie dem Prinzip der *geringsten Berechtigungen*. Gehen Sie folgendermaßen vor:<li>Weisen Sie niemals integrierten Gruppen (beispielsweise Administratoren) Berechtigungen zu.<li>Entfernen Sie gegebenenfalls lokale Computerberechtigungen.<li>Passen Sie den Zugriff an, und verwenden Sie für den Verzeichniszugriff die Active Directory-Delegierung.<li>Verwenden Sie präzise Zugriffsberechtigungen.<li>Legen Sie Kontoablaufdaten und standortbasierte Einschränkungen für benutzerbasierte Dienstkonten fest. |
| Nutzungsüberwachung| Überwachen Sie Anmeldedaten, und stellen Sie sicher, dass sie der vorgesehenen Nutzung entsprechen. Legen Sie Warnungen bei ungewöhnlicher Nutzung fest. |
| | |

### <a name="set-restrictions-for-user-accounts"></a>Festlegen von Einschränkungen für Benutzerkonten

Wenden Sie für Benutzerkonten, die als Dienstkonten verwendet werden, die folgenden Einstellungen an:

* [**Kontoablauf**](/powershell/module/activedirectory/set-adaccountexpiration?view=winserver2012-ps&preserve-view=true): Legen Sie fest, dass das Dienstkonto im Anschluss an den Überprüfungszeitraum nach einer bestimmten Zeit automatisch ablaufen soll, wenn keine weitere Verwendung vorgesehen ist.

*  **LogonWorkstations**: Schränken Sie die Anmeldemöglichkeiten für das Dienstkonto ein. Wenn es lokal auf einem Computer ausgeführt wird und nur auf Ressourcen auf diesem Computer zugreift, unterbinden Sie anderweitige Anmeldungen.

* [**Keine Kennwortänderung**](/powershell/module/activedirectory/set-aduser): Verhindern Sie, dass das Dienstkonto sein eigenes Kennwort ändert, indem Sie den Parameter auf „False“ festlegen.
 
## <a name="build-a-lifecycle-management-process"></a>Erstellen eines Lebenszyklusverwaltungsprozesses

Um die Sicherheit Ihrer Dienstkonten zu gewährleisten, ist eine Verwaltung vom Ermitteln des Bedarfs bis zur Außerbetriebnahme der Konten erforderlich. 

Verwenden Sie für die Lebenszyklusverwaltung von Dienstkonten den folgenden Prozess:

1. Sammeln von Nutzungsinformationen für das Konto
1. Verschieben des Dienstkontos und der App in die Konfigurationsverwaltungsdatenbank (Configuration Management Database, CMDB)
1. Durchführen einer Risikobewertung oder formalen Überprüfung
1. Erstellen des Dienstkontos und Anwenden von Einschränkungen
1. Planen und Ausführen wiederholter Überprüfungen Anpassen von Berechtigungen und Bereichen nach Bedarf
1. Aufheben der Kontobereitstellung nach Bedarf

### <a name="collect-usage-information-for-the-service-account"></a>Sammeln von Nutzungsinformationen für das Dienstkonto

Sammeln Sie für jedes Dienstkonto relevante geschäftliche Informationen. In der folgenden Tabelle sind die erforderlichen Mindestinformationen aufgeführt. Sie sollten jedoch alles erfassen, was die Existenz der einzelnen Konten geschäftlich rechtfertigt.

| Daten| BESCHREIBUNG |
| - | - |
| Besitzer| Der für das Dienstkonto verantwortliche Benutzer oder die entsprechende Gruppe. |
| Zweck| Der Zweck des Dienstkontos. |
| Berechtigungen (Bereiche)| Die erwarteten Berechtigungen. |
| CMDB-Links| Das Querverbindungsdienstkonto mit dem Zielskript oder der Zielanwendung und Besitzern. |
| Risiko| Die Bewertung des Risikos und der geschäftlichen Auswirkungen auf der Grundlage der Sicherheitsrisikobewertung. |
| Gültigkeitsdauer| Die voraussichtliche maximale Lebensdauer, um die Planung des Kontoablaufs oder der Neuzertifizierung zu ermöglichen. |
| | |

Fordern Sie im Idealfall ein Self-Service-Konto und die entsprechenden Informationen an. Bei dem Besitzer kann es sich um einen Anwendungs- oder Unternehmensbesitzer, um ein Mitglied der IT-Abteilung oder um einen Infrastrukturbesitzer handeln. Wenn Sie für diese Anforderung und die zugehörigen Informationen ein Tool wie Microsoft Forms verwenden, vereinfacht dies die Portierung zu Ihrem CMDB-Bestandstool nach der Genehmigung des Kontos.

### <a name="onboard-service-account-to-cmdb"></a>Integrieren von Dienstkonten in die Konfigurationsverwaltungsdatenbank (CMDB)

Speichern Sie die gesammelten Informationen in einer CMDB-artigen Anwendung. Schließen Sie neben den geschäftlichen Informationen auch alle Abhängigkeiten von anderen Infrastrukturen, Apps und Prozessen ein.  Dieses zentrale Repository vereinfacht folgende Vorgänge:

* Risikobewertung  
* Konfigurieren der Dienstkonten mit den erforderlichen Einschränkungen  
* Erkennen aller relevanten Abhängigkeiten in Bezug auf Funktion und Sicherheit  
* Ausführen regelmäßiger Überprüfungen auf Sicherheit und weiteren Nutzungsbedarf  
* Kontaktieren der Besitzer zwecks Überprüfung, Beendigung und Änderung des Dienstkontos

Stellen Sie sich ein Dienstkonto vor, das zum Ausführen einer Website verwendet wird und über Berechtigungen zum Herstellen einer Verbindung mit einer oder mehreren SQL-Personaldatenbanken (Human Resources, HR) verfügt. Die folgende Tabelle gibt Aufschluss über die Informationen, die in Ihrer CMDB-Instanz für dieses Dienstkonto gespeichert werden (einschließlich Beispielbeschreibungen):

|Daten | Beispielbeschreibung|
| - | - |
| Besitzer, Stellvertreter| John Bloom, Anna Mayers |
| Zweck| Ausführen der HR-Webseite und Herstellen einer Verbindung mit HR-Datenbanken. Kann beim Zugriff auf Datenbanken die Identität von Endbenutzern annehmen. |
| Berechtigungen, Bereiche| HR-WEBServer: Lokale Anmeldung; Ausführen der Webseite<br>HR-SQL1: Lokale Anmeldung; Leseberechtigungen für alle HR-Datenbanken<br>HR-SQL2: Lokale Anmeldung; nur Leseberechtigungen für Gehaltsdatenbank |
| Kostenstelle| 883944 |
| Risikobewertung| Mittel, Geschäftliche Auswirkungen: Mittel, Private Informationen: Mittel |
| Kontoeinschränkungen| Anmelden bei: nur erwähnte Server, keine Kennwortänderung, MBI-Kennwortrichtlinie |
| Lebensdauer| Nicht eingeschränkt |
| Überprüfungszyklus| Halbjährlich (durch Besitzer, Sicherheitsteam, Datenschutzteam) |
| | |

### <a name="perform-a-risk-assessment-or-formal-review-of-service-account-usage"></a>Durchführen einer Risikobewertung oder formalen Überprüfung der Nutzung des Dienstkontos

Angenommen, Ihr Konto wird durch eine nicht autorisierte Quelle kompromittiert. Bewerten Sie die Risiken, die das Konto möglicherweise für die zugehörige Anwendung oder den zugehörigen Dienst sowie für Ihre Infrastruktur darstellt. Berücksichtigen Sie sowohl das direkte als auch das indirekte Risiko. 

* Worauf würde ein nicht autorisierter Benutzer Direktzugriff erhalten?  
* Auf welche anderen Informationen oder Systeme kann das Dienstkonto zugreifen?  
* Kann das Konto zum Erteilen zusätzlicher Berechtigungen verwendet werden?  
* Wie erkennen Sie, wenn die Berechtigungen geändert werden?

Nach Abschluss und Dokumentation der Risikobewertung kommen Sie möglicherweise zu dem Ergebnis, dass die Risiken Auswirkungen auf folgende Bereiche haben:

* Kontoeinschränkungen  
* Kontolebensdauer  
* Kontoüberprüfungsanforderungen (Intervalle und Prüfer)

### <a name="create-a-service-account-and-apply-account-restrictions"></a>Erstellen eines Dienstkontos und Anwenden von Kontoeinschränkungen

Erstellen Sie ein Dienstkonto erst, nachdem Sie die Risikobewertung abgeschlossen und die relevanten Informationen in Ihrer CMDB-Instanz dokumentiert haben. Orientieren Sie sich bei den Kontoeinschränkungen an der Risikobewertung. Erwägen Sie die folgenden Einschränkungen, sofern sie für Ihre Bewertung relevant sind:

* Definieren Sie für alle als Dienstkonten verwendeten Benutzerkonten ein realistisches und definitives Enddatum. Legen Sie das Datum mithilfe des Flags **Konto läuft ab** fest. Weitere Informationen finden Sie unter [Set-ADAccountExpiration](/powershell/module/activedirectory/set-adaccountexpiration). 

* Anmeldung bei der Anmeldearbeitsstation ([LogonWorkstation](/powershell/module/activedirectory/set-aduser))

* [Anforderungen für Kennwortrichtlinien](../../active-directory-domain-services/password-policy.md)

* Kontoerstellung an einem [Ort der Organisationseinheit](/windows-server/identity/ad-ds/plan/delegating-administration-of-account-ous-and-resource-ous), durch den die Verwaltung auf berechtigte Benutzer beschränkt wird

* Einrichtung und Erfassung von Überwachungsdaten zur [Erkennung von Änderungen](/windows/security/threat-protection/auditing/audit-directory-service-changes) am Dienstkonto sowie zur [Nutzung des Dienstkontos](https://www.manageengine.com/products/active-directory-audit/how-to/audit-kerberos-authentication-events.html)

Wenn Sie bereit sind, das Dienstkonto in der Produktion zu nutzen, gewähren Sie sichereren Zugriff darauf. 

### <a name="schedule-regular-reviews-of-service-accounts"></a>Planen regelmäßiger Überprüfungen von Dienstkonten

Richten Sie für Dienstkonten, die bei der Risikobewertung als „mittel“ oder „hoch“ eingestuft wurden, regelmäßige Überprüfungen ein. Die Überprüfungen sollten Folgendes umfassen: 

* Nachweis des Besitzers hinsichtlich der fortgesetzten Notwendigkeit des Kontos sowie Begründung für Berechtigungen und Bereiche

* Überprüfung durch Datenschutz- und Sicherheitsteams einschließlich Auswertung von Upstream- und Downstreamverbindungen

* Überwachungsdaten, um sicherzustellen, dass das Konto nur für die beabsichtigten Zwecke verwendet wird

### <a name="deprovision-service-accounts"></a>Aufheben der Bereitstellung von Dienstkonten

Entfernen Sie zum Aufheben der Bereitstellung zunächst die Berechtigungen und die Überwachung und dann das Konto (sofern erforderlich).

Eine Aufhebung der Bereitstellung von Dienstkonten wird in folgenden Fällen durchgeführt:

* Das Skript oder die Anwendung, für das bzw. für die das Dienstkonto erstellt wurde, wird ausgemustert.

* Die Funktion innerhalb des Skripts oder der Anwendung, für das bzw. für die das Dienstkonto verwendet wird (z. B. Zugriff auf eine bestimmte Ressource), wird ausgemustert.

* Das Dienstkonto wurde durch ein anderes Dienstkonto ersetzt.

Nachdem Sie alle Berechtigungen entfernt haben, können Sie das Konto wie folgt entfernen:

1. Wenn die Bereitstellung der zugehörigen Anwendung oder des zugehörigen Skripts aufgehoben wurde, überwachen Sie die Anmeldungen und den Ressourcenzugriff für die zugeordneten Dienstkonten, um sicherzustellen, dass sie nicht in einem anderen Prozess verwendet werden. Wenn Sie sicher sind, dass es nicht mehr benötigt wird, fahren Sie mit dem nächsten Schritt fort.

1. Deaktivieren Sie das Dienstkonto, um Anmeldungen zu verhindern, und vergewissern Sie sich, dass es nicht mehr benötigt wird. Erstellen Sie eine geschäftliche Richtlinie, die angibt, wie lange Konten deaktiviert bleiben müssen.

1. Löschen Sie das Dienstkonto, nachdem die Richtlinie für den erforderlichen Deaktivierungszeitraum erfüllt wurde. 

   * **MSAs:** [Deinstallieren Sie das Konto](/powershell/module/activedirectory/uninstall-adserviceaccount?view=winserver2012-ps&preserve-view=true) mithilfe von PowerShell, oder löschen Sie es manuell aus dem Container für verwaltete Dienstkonten.

   * **Computer- oder Benutzerkonten:** Löschen Sie das Konto manuell in Active Directory.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Schutz von Dienstkonten finden Sie in den folgenden Artikeln:

* [Einführung in lokale Dienstkonten](service-accounts-on-premises.md)  
* [Schützen von gruppenverwalteten Dienstkonten](service-accounts-group-managed.md)  
* [Schützen von eigenständigen verwalteten Dienstkonten](service-accounts-standalone-managed.md)  
* [Schützen von Computerkonten](service-accounts-computer.md)  
* [Schützen von Benutzerkonten](service-accounts-user-on-premises.md)
