---
title: 'Azure Active Directory: Referenzleitfaden zu den Vorgängen der Identitäts- und Zugriffsverwaltung'
description: In diesem Referenzleitfaden werden die Überprüfungen und Aktionen beschrieben, die Sie zum Schützen von Vorgängen der Identitäts- und Zugriffsverwaltung durchführen sollten.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: 66bce573be5a31641bdff809b8e9a79b617a703a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92371000"
---
# <a name="azure-active-directory-identity-and-access-management-operations-reference-guide"></a>Azure Active Directory: Referenzleitfaden zu den Vorgängen der Identitäts- und Zugriffsverwaltung

In diesem Abschnitt des [Referenzleitfadens zu Azure AD-Vorgängen](active-directory-ops-guide-intro.md) werden die Überprüfungen und Aktionen beschrieben, die Sie erwägen sollten, um den Lebenszyklus von Identitäten und den zugehörigen Zuweisungen zu schützen und zu verwalten.

> [!NOTE]
> Diese Empfehlungen sind auf dem Stand des Veröffentlichungsdatums, können sich aber im Laufe der Zeit ändern. Organisationen sollten ihre Identitätsmethoden fortlaufend überprüfen, da die Produkte und Dienste von Microsoft mit der Zeit weiterentwickelt werden.

## <a name="key-operational-processes"></a>Wichtige Betriebsabläufe

### <a name="assign-owners-to-key-tasks"></a>Zuweisen von Besitzern zu wichtigen Aufgaben

Für die Verwaltung von Azure Active Directory ist die kontinuierliche Ausführung wichtiger betrieblicher Aufgaben und Prozesse erforderlich, die unter Umständen nicht Teil einer Projektveröffentlichung sind. Es ist außerdem wichtig, dass Sie diese Aufgaben einrichten, um Ihre Umgebung zu warten. Im Folgenden werden die wichtigen Aufgaben und empfohlene Besitzer für diese aufgeführt:

| Aufgabe | Besitzer |
| :- | :- |
| Definieren des Prozesses zum Erstellen von Azure-Abonnements | Variiert je nach Organisation |
| Treffen der Entscheidung, wer Enterprise Mobility + Security-Lizenzen erhält | Team für IAM-Vorgänge (Identity & Access Management, Identitäts- und Zugriffsverwaltung) |
| Festlegen, wer Microsoft 365-Lizenzen erhält | Produktivitätsteam |
| Festlegen, wer andere Lizenzen (z. B. Dynamics, Visual Studio Codespaces) erhält | Anwendungsbesitzer |
| Zuweisen von Lizenzen | Team für IAM-Vorgänge (Identity & Access Management, Identitäts- und Zugriffsverwaltung) |
| Behandeln und Beheben von Fehlern mit der Lizenzverwaltung | Team für IAM-Vorgänge (Identity & Access Management, Identitäts- und Zugriffsverwaltung) |
| Bereitstellen von Identitäten für Anwendungen in Azure AD | Team für IAM-Vorgänge (Identity & Access Management, Identitäts- und Zugriffsverwaltung) |

Beim Überprüfen Ihrer Liste stellen Sie ggf. fest, dass Sie entweder einen Besitzer für Aufgaben zuweisen müssen, denen kein Besitzer zugeteilt ist, oder Aufgaben anpassen müssen, deren Besitzer nicht den obigen Empfehlungen entspricht.

#### <a name="assigning-owners-recommended-reading"></a>Zuweisen von Leseempfehlungen zu Besitzern

- [Zuweisen von Administratorrollen in Azure Active Directory](../roles/permissions-reference.md)
- [Governance in Azure](../../governance/index.yml)

## <a name="on-premises-identity-synchronization"></a>Lokale Identitätssynchronisierung

### <a name="identify-and-resolve-synchronization-issues"></a>Identifizieren und Beheben von Synchronisierungsproblemen

Die Empfehlung von Microsoft lautet, dass Sie über gute Grundlagen verfügen und mit den Umständen in Ihrer lokalen Umgebung vertraut sein sollten, die zu Synchronisierungsproblemen für die Cloud führen können. Da automatisierte Tools, z. B. [IdFix](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) und [Azure AD Connect Health](../hybrid/whatis-azure-ad-connect.md#why-use-azure-ad-connect-health), eine große Menge von False Positives generieren können, raten wir Ihnen Folgendes: Identifizieren Sie die Synchronisierungsfehler, die länger als 100 Tage nicht behoben wurden, indem Sie diese Fehlerobjekte bereinigen. Wenn Synchronisierungsfehler längere Zeit nicht behoben werden, kann dies zur Generierung von Supportfällen führen. Unter [Beheben von Fehlern während der Synchronisierung](../hybrid/tshoot-connect-sync-errors.md) erhalten Sie einen Überblick über verschiedene Fehlertypen, die während der Synchronisierung auftreten können, über einige mögliche Szenarien, die solche Fehler verursachen, und über Möglichkeiten, diese Fehler zu beheben.

### <a name="azure-ad-connect-sync-configuration"></a>Konfiguration der Azure AD Connect-Synchronisierung

Zum Aktivieren aller Hybridumgebungen, des gerätebasierten Sicherheitsstatus und der Integration in Azure AD müssen Sie Benutzerkonten synchronisieren, die Ihre Mitarbeiter zum Anmelden an ihren Desktops nutzen.

Wenn Sie die Gesamtstruktur, an der sich die Benutzer anmelden, nicht synchronisieren, sollten Sie die Synchronisierung so ändern, dass sie von der richtigen Gesamtstruktur durchgeführt wird.

#### <a name="synchronization-scope-and-object-filtering"></a>Synchronisierungsbereich und Objektfilterung

Die Entfernung von bekannten Buckets, die nicht synchronisiert werden müssen, hat die folgenden Vorteile für den Betrieb:

- Weniger Quellen für Synchronisierungsfehler
- Schnellere Synchronisierungszyklen
- Weniger „Ausschuss“, der aus der lokalen Umgebung übernommen werden muss, z. B. Verunreinigung der globalen Adressliste für lokale Dienstkonten, die in der Cloud nicht relevant sind

> [!NOTE]
> Wenn Sie merken, dass Sie viele Objekte importieren, die nicht in die Cloud exportiert werden, sollten Sie nach Organisationseinheit oder bestimmten Attributen filtern.

Beispiele für auszuschließende Objekte:

- Dienstkonten, die nicht für Cloudanwendungen verwendet werden
- Gruppen, die nicht für die Verwendung in Cloudszenarien vorgesehen sind, z. B. zum Gewähren des Zugriffs auf Ressourcen
- Benutzer oder Kontakte in Form von externen Identitäten, die per Azure AD B2B-Zusammenarbeit dargestellt werden sollen
- Computerkonten, von denen aus Mitarbeiter nicht auf Cloudanwendungen, z. B. Server, zugreifen sollen

> [!NOTE]
> Wenn für eine einzelne Identität eines Menschen mehrere Konten bereitgestellt wurden, z. B. aufgrund der Migration einer älteren Domäne, einer Fusion oder einer Übernahme, sollten Sie nur das Konto synchronisieren, das vom Benutzer täglich verwendet wird. Dies kann beispielsweise das Konto sein, mit dem dieser sich an seinem Computer anmeldet.

Im Idealfall sollten Sie ein Gleichgewicht zwischen der Reduzierung der Anzahl von zu synchronisierenden Objekten und der Komplexität der Regeln anstreben. Im Allgemeinen ist eine Kombination aus der [Filterung](../hybrid/how-to-connect-sync-configure-filtering.md) von Organisationseinheiten bzw. Containern und einer einfachen Attributzuordnung zum „cloudFiltered“-Attribut ein effektiver Ansatz für die Filterung.

> [!IMPORTANT]
> Falls Sie die Gruppenfilterung in der Produktion nutzen, sollten Sie die Umstellung auf einen anderen Filteransatz durchführen.

#### <a name="sync-failover-or-disaster-recovery"></a>Synchronisieren des Failovers oder der Notfallwiederherstellung

Azure AD Connect spielt eine wichtige Rolle bei der Bereitstellung. Wenn der Synchronisierungsserver ausfällt, können lokale Änderungen nicht in der Cloud aktualisiert werden und zu Problemen für die Benutzer führen. Daher ist es wichtig, eine Failoverstrategie festzulegen, die es Administratoren ermöglicht, die Synchronisierung schnell fortzusetzen, nachdem der Synchronisierungsserver offline geschaltet wurde. Diese Strategien können in die folgenden Kategorien fallen:

- **Bereitstellen von Azure AD Connect-Servern im Stagingmodus**: Ermöglicht einem Administrator das Höherstufen des Stagingservers in die Produktion, indem ein einfacher Konfigurationsschalter verwendet wird.
- **Verwenden der Virtualisierung**: Wenn Azure AD Connect auf einem virtuellen Computer (VM) bereitgestellt wird, können Administratoren ihren Virtualisierungsstapel für eine Livemigration nutzen oder die schnelle erneute Bereitstellung der VM verwenden und die Synchronisierung so fortsetzen.

Falls Ihre Organisation nicht über eine Notfallwiederherstellungs- und Failoverstrategie für die Synchronisierung verfügt, sollten Sie nicht zögern, Azure AD Connect im Stagingmodus bereitzustellen. Ebenso gilt Folgendes: Falls zwischen Ihrer Produktions- und Stagingkonfiguration ein Konflikt besteht, sollten Sie für den Azure AD Connect-Stagingmodus eine neue Baseline festlegen, um die Anpassung an die Produktionskonfiguration vorzunehmen (z. B. Softwareversionen und -konfigurationen).

![Screenshot: Konfiguration des Azure AD Connect-Stagingmodus](./media/active-directory-ops-guide/active-directory-ops-img1.png)

#### <a name="stay-current"></a>Auf dem Laufenden bleiben

Azure AD Connect wird von Microsoft regelmäßig aktualisiert. Bleiben Sie immer auf dem aktuellen Stand, um von Leistungsverbesserungen, Fehlerbehebungen und neuen Funktionen der neuen Versionen profitieren zu können.

Falls Ihre Azure AD Connect-Version mehr als sechs Monate alt ist, sollten Sie ein Upgrade auf die aktuelle Version durchführen.

#### <a name="source-anchor"></a>Quellanker

Die Nutzung von **ms-DS-consistencyguid** als [Quellanker](../hybrid/plan-connect-design-concepts.md) ermöglicht eine einfachere Migration von Objekten über Gesamtstrukturen und Domänen hinweg. Dies ist die übliche Vorgehensweise bei der Konsolidierung/Bereinigung von AD-Domänen, Fusionen, Übernahmen und Veräußerungen.

Wenn Sie derzeit **ObjectGuid** als Quellanker verwenden, empfehlen wir Ihnen die Umstellung auf **ms-DS-ConsistencyGuid**.

#### <a name="custom-rules"></a>Benutzerdefinierte Regeln

Mit den benutzerdefinierten Regeln von Azure AD Connect können Sie den Fluss der Attribute zwischen lokalen Objekten und Cloudobjekten steuern. Die übermäßige oder fehlerhafte Anwendung von benutzerdefinierten Regeln kann aber mit den folgenden Risiken verbunden sein:

- Komplexe Problembehandlung
- Beeinträchtigung der Leistung beim Durchführen von komplexen Vorgängen über mehrere Objekte hinweg
- Höhere Wahrscheinlichkeit einer Abweichung der Konfiguration zwischen Produktions- und Stagingserver
- Mehraufwand beim Aktualisieren von Azure AD Connect, wenn benutzerdefinierte Regeln mit einem höheren Rangfolgenwert als 100 erstellt werden (wird von integrierten Regeln verwendet)

Wenn Sie übermäßig komplexe Regeln verwenden, sollten Sie die Gründe für die Komplexität untersuchen und Möglichkeiten zur Vereinfachung ermitteln. Analog gilt Folgendes: Wenn Sie benutzerdefinierte Regeln mit einem Rangfolgenwert erstellt haben, der über 100 liegt, sollten Sie die Regeln so anpassen, dass kein Risiko und kein Konflikt mit dem Standardsatz besteht.

Beispiele für die fehlerhafte Anwendung von benutzerdefinierten Regeln:

- **Kompensierung von nicht bereinigten Daten im Verzeichnis**: In diesem Fall empfehlen wir Ihnen, zur Problemlösung zusammen mit den Besitzern des AD-Teams die Daten im Verzeichnis zu bereinigen und die Prozesse anzupassen, um die Wiedereinführung fehlerhafter Daten zu vermeiden.
- **Einmalige Bereinigung einzelner Benutzer**: Es kommt häufiger vor, dass sich für Regeln spezielle Ausreißer ergeben. Der Grund ist meist ein Problem mit einem bestimmten Benutzer.
- **Zu komplizierte Cloudfilterung (CloudFiltering)** : Die Reduzierung der Anzahl von Objekten ist zwar eine bewährte Vorgehensweise, aber es besteht das Risiko, dass ein zu komplizierter Synchronisierungsbereich mit vielen Synchronisierungsregeln erstellt wird. Wenn die Logik zum Einschließen bzw. Ausschließen von Objekten über die Filterung von Organisationseinheiten hinaus sehr komplex ist, empfehlen wir Ihnen, diese Logik getrennt von der Synchronisierung zu behandeln und die Objekte mit einem einfachen „cloudFiltered“-Attribut zu bezeichnen, das mit einer einfachen Synchronisierungsregel genutzt werden kann.

#### <a name="azure-ad-connect-configuration-documenter"></a>Azure AD Connect-Konfigurationsdokumentierer

Der [Azure AD Connect-Konfigurationsdokumentierer](https://github.com/Microsoft/AADConnectConfigDocumenter) ist ein Tool, mit dem Sie Dokumentation für eine Azure AD Connect-Installation generieren können. So können Sie das Verständnis der Synchronisierungskonfiguration verbessern, das Vertrauen in die Wahl der richtigen Vorgehensweise erhöhen und leicht erkennen, was sich nach dem Anwenden eines neuen Builds oder einer Konfiguration von Azure AD Connect oder dem Hinzufügen oder Aktualisieren von benutzerdefinierten Synchronisierungsregeln geändert hat. Derzeit verfügt das Tool über die folgenden Funktionen:

- Dokumentation der vollständigen Konfiguration der Azure AD Connect-Synchronisierung.
- Dokumentation aller Änderungen der Konfiguration von zwei Azure AD Connect-Synchronisierungsservern oder Änderungen einer bestimmten Konfigurationsbaseline.
- Generierung eines PowerShell-Bereitstellungsskripts zum Migrieren der Unterschiede oder Anpassungen von Synchronisierungsregeln von einem Server zum anderen.

## <a name="assignment-to-apps-and-resources"></a>Zuweisung zu Apps und Ressourcen

### <a name="group-based-licensing-for-microsoft-cloud-services"></a>Gruppenbasierte Lizenzierung für Microsoft-Clouddienste

Mit Azure Active Directory wird die Verwaltung von Lizenzen per [gruppenbasierter Lizenzierung](./active-directory-licensing-whatis-azure-portal.md) für Microsoft-Clouddienste optimiert. Auf diese Weise wird die Gruppeninfrastruktur und die delegierte Verwaltung dieser Gruppen per IAM für die richtigen Teams von Organisationen bereitgestellt. Es gibt mehrere Möglichkeiten, die Gruppenmitgliedschaft in Azure AD einzurichten, z. B.:

- **Synchronisierung aus lokaler Umgebung**: Gruppen können aus lokalen Verzeichnissen stammen. Dies kann eine gute Option für Organisationen sein, die über etablierte Prozesse für die Gruppenverwaltung verfügen, für die eine Erweiterung zum Zuweisen von Lizenzen in Microsoft 365 möglich ist.

- **Attributbasiert/Dynamisch**: Gruppen können in der Cloud anhand eines Ausdrucks erstellt werden, der auf Benutzerattributen basiert, z. B. Abteilung ist gleich „Sales“. Azure AD verwaltet die Mitglieder der Gruppe und sorgt für Konsistenz mit dem definierten Ausdruck. Diese Art von Gruppen für die Lizenzzuweisung ermöglicht eine attributbasierte Vorgehensweise, die gut für Organisationen geeignet ist, deren Verzeichnisse eine hohe Datenqualität aufweisen.

- **Delegierter Besitz**: Gruppen können in der Cloud erstellt werden, und dabei können festgelegte Besitzer verwendet werden. So können Sie geschäftliche Besitzer, z. B. das Kollaborations- oder BI-Team, in die Lage versetzen, die Personen zu definieren, für die Zugriff gewährt werden soll.

Falls Sie derzeit einen manuellen Prozess nutzen, um Benutzern Lizenzen und Komponenten zuzuweisen, empfehlen wir Ihnen die Implementierung der gruppenbasierten Lizenzierung. Wenn bei Ihrem aktuellen Prozess keine Überwachung auf Lizenzierungsfehler oder des Status „Zugewiesen/Verfügbar“ erfolgt, sollten Sie Verbesserungen für den Prozess definieren, um Lizenzierungsfehler zu beheben und die Lizenzierungszuweisung zu überwachen.

Ein weiterer Aspekt der Lizenzverwaltung ist die Definition von Dienstplänen (Komponenten der Lizenz), die basierend auf Auftragsfunktionen in der Organisation aktiviert werden sollten. Das Gewähren des Zugriffs auf Dienstpläne, die nicht erforderlich sind, kann dazu führen, dass Benutzern im Office-Portal Tools angezeigt werden, für die sie nicht geschult sind oder deren Verwendung nicht vorgesehen ist. Dies kann zu mehr Anfragen beim Helpdesk und unnötigen Bereitstellungen führen und die Erreichung von Konformität und Governance gefährden, z. B. bei der Bereitstellung von OneDrive for Business für Personen, die ggf. nicht zum Freigeben von Inhalten berechtigt sind.

Verwenden Sie die folgenden Richtlinien zum Definieren von Dienstplänen für Benutzer:

- Administratoren sollten „Bündel“ mit Dienstplänen definieren, die Benutzern basierend auf ihrer Rolle angeboten werden, z. B. Mitarbeiter im betriebswirtschaftlichen Bereich oder im Fertigungsbereich.
- Erstellen Sie Gruppen nach Cluster, und weisen Sie die Lizenz mit dem Dienstplan zu.
- Optional kann ein Attribut so definiert werden, dass es die Pakete für Benutzer enthält.

> [!IMPORTANT]
> Mit der gruppenbasierten Lizenzierung in Azure AD wird für Benutzer das Konzept eines Fehlerzustands für die Lizenzierung eingeführt. Falls Sie Lizenzierungsfehler bemerken, sollten Sie sofort alle Probleme mit der Zuweisung von Lizenzen [identifizieren und beheben](../enterprise-users/licensing-groups-resolve-problems.md).

![Screenshot: Automatisch generierte Beschreibung für einen Computerbildschirm](./media/active-directory-ops-guide/active-directory-ops-img2.png)

#### <a name="lifecycle-management"></a>Lebenszyklusverwaltung

Falls Sie derzeit ein Tool verwenden, z. B. [Microsoft Identity Manager](/microsoft-identity-manager/) oder ein Drittanbietersystem, das auf einer lokalen Infrastruktur basiert, empfehlen wir Ihnen die folgende Vorgehensweise: Verlagern Sie die Zuweisung vom vorhandenen Tool, implementieren Sie die gruppenbasierte Lizenzierung, und definieren Sie die Verwaltung des Gruppenlebenszyklus basierend auf [Gruppen](../enterprise-users/licensing-group-advanced.md#use-group-based-licensing-with-dynamic-groups). Ebenso gilt Folgendes: Falls bei Ihrem vorhandenen Prozess keine neuen Mitarbeiter oder ausscheidenden Mitarbeiter berücksichtigt werden, sollten Sie die gruppenbasierte Lizenzierung basierend auf dynamischen Gruppen bereitstellen und einen Lebenszyklus für die Gruppenmitgliedschaft definieren. Falls die gruppenbasierte Lizenzierung für lokale Gruppen bereitgestellt wird, für die keine Lebenszyklusverwaltung vorhanden ist, sollten Sie die Verwendung von Cloudgruppen erwägen, um Funktionen wie delegierter Besitz oder attributbasierte dynamische Mitgliedschaft zu ermöglichen.

### <a name="assignment-of-apps-with-all-users-group"></a>Zuweisung von Apps mit der Gruppe „Alle Benutzer“

Ressourcenbesitzer sind unter Umständen der Meinung, dass die Gruppe **Alle Benutzer** nur **Mitarbeiter des Unternehmens** enthält, während eigentlich sowohl **Mitarbeiter des Unternehmens** als auch **Gäste** enthalten sind. Daher sollten Sie beim Verwenden der Gruppe **Alle Benutzer** für die Anwendungszuweisung und beim Gewähren des Zugriffs auf Ressourcen wie SharePoint-Inhalte oder -Anwendungen mit Bedacht vorgehen.

> [!IMPORTANT]
> Wenn die Gruppe **Alle Benutzer** aktiviert ist und für die Zuweisung von Richtlinien für den bedingten Zugriff, Apps oder Ressourcen verwendet wird, sollten Sie die [Gruppe unbedingt schützen](../external-identities/use-dynamic-groups.md), wenn darin keine Gastbenutzer enthalten sein sollen. Darüber hinaus sollten Sie Ihre Lizenzierungszuweisungen korrigieren, indem Sie nur Gruppen erstellen und für die Zuweisung nutzen, die ausschließlich **Mitarbeiter des Unternehmens** enthalten. Falls Sie dagegen ermitteln, dass die Gruppe **Alle Benutzer** aktiviert ist, aber nicht zum Gewähren des Zugriffs auf Ressourcen verwendet wird, sollten Sie sicherstellen, dass in Ihrer Organisation die Verwendung dieser Gruppe (sowohl mit **Mitarbeitern des Unternehmens** als auch mit **Gästen**) vorgegeben ist.

### <a name="automated-user-provisioning-to-apps"></a>Automatisierte Benutzerbereitstellung für Apps

Die [automatisierte Benutzerbereitstellung](../app-provisioning/user-provisioning.md) für Anwendungen ist die beste Möglichkeit, um für Einheitlichkeit bei der Bereitstellung und Aufhebung der Bereitstellung sowie beim Lebenszyklus von Identitäten über mehrere Systeme hinweg zu sorgen.

Wenn Sie Apps derzeit per Ad-hoc-Ansatz bereitstellen oder CSV-Dateien, JIT oder eine lokale Lösung ohne Lebenszyklusverwaltung nutzen, empfehlen wir Ihnen die [Implementierung der Anwendungsbereitstellung](../app-provisioning/user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) mit Azure AD für unterstützte Anwendungen und die Festlegung eines einheitlichen Musters für Anwendungen, die von Azure AD noch nicht unterstützt werden.

![Azure AD-Bereitstellungsdienst](./media/active-directory-ops-guide/active-directory-ops-img3.png)

### <a name="azure-ad-connect-delta-sync-cycle-baseline"></a>Baseline für Azure AD Connect-Deltasynchronisierungszyklus

Es ist wichtig, den Umfang der Änderungen in Ihrer Organisation zu verstehen und sicherzustellen, dass der Zeitraum nicht zu lang ist, um die Synchronisierungsdauer gut vorhersagen zu können.

Standardmäßig wird die [Deltasynchronisierung](../hybrid/how-to-connect-sync-feature-scheduler.md) alle 30 Minuten durchgeführt. Falls die Deltasynchronisierung immer länger als 30 Minuten dauert oder erhebliche Unterschiede zwischen der Leistung der Deltasynchronisierung für Staging und Produktion bestehen, sollten Sie die [Faktoren, die die Leistung von Azure AD Connect beeinflussen](../hybrid/plan-connect-performance-factors.md), überprüfen.

#### <a name="azure-ad-connect-troubleshooting-recommended-reading"></a>Azure AD Connect-Problembehandlung: Empfohlene Literatur

- [Vorbereiten von Verzeichnisattributen für die Synchronisierung mit Microsoft 365 mithilfe des IdFix-Tools](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix)
- [Azure AD Connect: Beheben von Fehlern während der Synchronisierung](../hybrid/tshoot-connect-sync-errors.md)

## <a name="summary"></a>Zusammenfassung

Sichere Identitätsinfrastrukturen weisen fünf Aspekte auf. Diese Liste dient Ihnen als Hilfe beim schnellen Ermitteln und Ergreifen der erforderlichen Maßnahmen zum Schützen und Verwalten des Lebenszyklus von Identitäten und Berechtigungen in Ihrer Organisation.

- Zuweisen von Besitzern zu wichtigen Aufgaben
- Identifizieren und Beheben von Synchronisierungsproblemen
- Definieren einer Failoverstrategie für die Notfallwiederherstellung
- Optimieren der Verwaltung von Lizenzen und der Zuweisung von Apps
- Automatisieren der Benutzerbereitstellung für Apps

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit den [Überprüfungen und Aktionen für die Authentifizierungsverwaltung](active-directory-ops-guide-auth.md).