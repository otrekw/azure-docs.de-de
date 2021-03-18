---
title: Bereitstellen von Privileged Identity Management (PIM) – Azure AD | Microsoft-Dokumentation
description: Beschreibt die Planung der Bereitstellung von Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/27/2020
ms.author: curtand
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b1d18982a4f2a9ee8ba585af56a5e9ded7c1c62
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102036825"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Bereitstellen von Azure AD Privileged Identity Management (PIM)

Dieser Artikel enthält eine Schritt-für-Schritt-Anleitung, in der beschrieben wird, wie Sie die Bereitstellung von Privileged Identity Management (PIM) in Ihrer Azure Active Directory-Organisation (Azure AD-Organisation) planen können. Sie weisen Benutzern in Rollen mit hohen Berechtigungen nach Möglichkeit weniger privilegierte integrierte oder benutzerdefinierte Rollen zu und planen Just-In-Time-Rollenzuweisungen für Ihre privilegiertesten Rollen. In diesem Artikel finden Sie Empfehlungen sowohl für die Planung als auch für die Implementierung der Bereitstellung.

> [!TIP]
> In diesem Artikel sind einige Elemente folgendermaßen gekennzeichnet:
>
> :heavy_check_mark: **Microsoft-Empfehlung**
>
> Hierbei handelt es sich um allgemeine Empfehlungen, die nur umgesetzt werden sollten, wenn sie Ihren individuellen Unternehmensanforderungen entsprechen.

## <a name="licensing-requirements"></a>Lizenzanforderungen

Um Privileged Identity Management verwenden zu können, muss Ihr Verzeichnis über eine der folgenden kostenpflichtigen Lizenzen oder Testlizenzen verfügen. Weitere Informationen finden Sie unter [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md).

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 Education A5
- Microsoft 365 Enterprise E5

## <a name="how-pim-works"></a>Funktionsweise von PIM

Dieser Abschnitt bietet eine Übersicht über die Planungszwecke der relevanten Teile des Privileged Identity Management-Prozesses. Unter [Was ist Azure AD Privileged Identity Management?](pim-configure.md) finden Sie weitere Informationen.

1. Beginnen Sie mit der Verwendung von Privileged Identity Management, um dafür zu sorgen, dass Benutzer für privilegierte Rollen berechtigt sind.
1. Wenn ein berechtigter Benutzer seine privilegierte Rolle verwenden muss, aktiviert er die Rolle mithilfe von Privileged Identity Management.
1. In den Einstellungen kann festgelegt sein, dass der Benutzer bestimmte Anforderungen erfüllen muss:

    - Verwenden der mehrstufigen Authentifizierung
    - Anfordern der Genehmigung für die Aktivierung
    - Angeben eines geschäftlichen Grunds für die Aktivierung

1. Nachdem der Benutzer seine Rolle erfolgreich aktiviert hat, verfügt er für eine festgelegte Dauer über die entsprechenden Rollenberechtigungen.
1. Administratoren können einen Verlauf aller Privileged Identity Management-Aktivitäten im Überwachungsprotokoll anzeigen. Außerdem können Administratoren mithilfe von Privileged Identity Management-Funktionen wie Zugriffsüberprüfungen und Warnungen ihre Azure AD-Organisationen noch besser schützen und Complianceanforderungen erfüllen.

## <a name="roles-that-can-be-managed-by-pim"></a>Rollen, die von PIM verwaltet werden können

**Azure AD-Rollen** sind Rollen in Azure Active Directory (z. B. „Globaler Administrator“, „Exchange-Administrator“ und „Sicherheitsadministrator“). Unter [Berechtigungen der Administratorrolle in Azure Active Directory](../roles/permissions-reference.md) erfahren Sie mehr über die Rollen und ihre Funktionen. [Administratorrollen nach Administratoraufgabe in Azure Active Directory](../roles/delegate-by-task.md) hilft Ihnen dabei, Ihren Administratoren geeignete Rollen zuzuweisen.

**Azure-Rollen** sind mit einer Azure-Ressource, einer Ressourcengruppe, einem Abonnement oder einer Verwaltungsgruppe verknüpfte Rollen. Mit PIM können Sie Just-In-Time-Zugriff auf integrierte Azure-Rollen wie „Besitzer“, „Benutzerzugriffsadministrator“ und „Mitwirkender“ sowie auf [benutzerdefinierte Rollen](../../role-based-access-control/custom-roles.md) bereitstellen. Weitere Informationen zu Azure-Rollen finden Sie im Artikel über die [rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/overview.md).

Weitere Informationen finden Sie unter [Rollen, die nicht in Privileged Identity Management verwaltet werden können](pim-roles.md).

## <a name="deployment-plan"></a>Bereitstellungsplan

Bevor Sie Privileged Identity Management in Ihrer Organisation bereitstellen, sollten Sie die Anweisungen und die Konzepte in diesem Abschnitt lesen und verstehen, um einen auf die Anforderungen von privilegierten Identitäten Ihrer Organisation zugeschnittenen Plan erstellen zu können.

### <a name="identify-your-stakeholders"></a>Identifizieren der Beteiligten

Der folgende Abschnitt hilft Ihnen beim Identifizieren aller am Projekt beteiligten Personen, die das Projekt genehmigen, überprüfen oder darüber informiert bleiben müssen. Er enthält separate Tabellen für die Bereitstellung von PIM für Azure AD-Rollen und PIM für Azure-Rollen. Fügen Sie der folgenden Tabelle je nach Bedarf Projektbeteiligte für Ihre Organisation hinzu.

- SO = Dieses Projekt genehmigen
- R = Dieses Projekt überprüfen und Beiträge bereitstellen
- I = Über dieses Projekt informiert

#### <a name="stakeholders-privileged-identity-management-for-azure-ad-roles"></a>Beteiligte: Privileged Identity Management für Azure AD-Rollen

| Name | Role | Aktion |
| --- | --- | --- |
| Name und E-Mail-Adresse | **Identitätsarchitekt oder globaler Azure-Administrator**<br/>Ein Vertreter des Identitätsverwaltungsteams, der definiert, wie diese Änderung in die Kerninfrastruktur für die Identitätsverwaltung in Ihrer Organisation integriert wird. | SO/R/I |
| Name und E-Mail-Adresse | **Dienstbesitzer/Linienmanager**<br/>Ein Vertreter der IT-Besitzer eines Diensts oder eine Gruppe von Diensten. Er ist der Hauptentscheidungsträger und in erster Linie für den Rollout von Privileged Identity Management für sein Team verantwortlich. | SO/R/I |
| Name und E-Mail-Adresse | **Sicherheitsbesitzer**<br/>Ein Vertreter des Sicherheitsteams, der abzeichnen kann, dass der Plan die Sicherheitsanforderungen Ihrer Organisation erfüllt. | SO/R |
| Name und E-Mail-Adresse | **IT-Supportmanager/Helpdesk**<br/>Ein Vertreter der IT-Supportorganisation, der Feedback zu den Unterstützungsmöglichkeiten dieser Änderung aus einer Helpdesk-Perspektive geben kann. | R/I |
| Name und E-Mail-Adresse für Pilotbenutzer | **Benutzer mit privilegierter Rolle**<br/>Die Gruppe von Benutzern, für die privilegierte Identitätsverwaltung implementiert wird. Sie müssen wissen, wie ihre Rollen nach der Implementierung von Privileged Identity Management aktiviert werden. | I |

#### <a name="stakeholders-privileged-identity-management-for-azure-roles"></a>Beteiligte: Privileged Identity Management für Azure-Rollen

| Name | Role | Aktion |
| --- | --- | --- |
| Name und E-Mail-Adresse | **Abonnement/Ressourcenbesitzer**<br/>Ein Vertreter der IT-Besitzer jedes Abonnements oder jeder Ressource, für das oder die Sie Privileged Identity Management bereitstellen möchten | SO/R/I |
| Name und E-Mail-Adresse | **Sicherheitsbesitzer**<br/>Ein Vertreter des Sicherheitsteams, der abzeichnen kann, dass der Plan die Sicherheitsanforderungen Ihrer Organisation erfüllt. | SO/R |
| Name und E-Mail-Adresse | **IT-Supportmanager/Helpdesk**<br/>Ein Vertreter der IT-Supportorganisation, der Feedback zu den Unterstützungsmöglichkeiten dieser Änderung aus einer Helpdesk-Perspektive geben kann. | R/I |
| Name und E-Mail-Adresse für Pilotbenutzer | **Benutzer mit Azure-Rolle**<br/>Die Gruppe von Benutzern, für die privilegierte Identitätsverwaltung implementiert wird. Sie müssen wissen, wie ihre Rollen nach der Implementierung von Privileged Identity Management aktiviert werden. | I |

### <a name="start-using-privileged-identity-management"></a>Einstieg in Privileged Identity Management

Im Rahmen des Planungsprozesses müssen Sie zuerst Privileged Identity Management vorbereiten. Befolgen Sie dazu die Anweisungen im Artikel [Einstieg in Privileged Identity Management](pim-getting-started.md). Mit Privileged Identity Management erhalten Sie Zugriff auf einige Funktionen, die für die Unterstützung Ihrer Bereitstellung konzipiert sind.

Wenn Sie Privileged Identity Management für Azure-Ressourcen bereitstellen möchten, lesen Sie den Artikel [Ermitteln von Azure-Ressourcen zur Verwaltung in PIM](pim-resource-roles-discover-resources.md). Nur Besitzer von Abonnements und Verwaltungsgruppen können diese Ressourcen in die Verwaltung in Privileged Identity Management einbinden. Nach der Integration in die Verwaltung ist die PIM-Funktionalität für Besitzer auf allen Ebenen verfügbar, einschließlich Verwaltungsgruppe, Abonnement, Ressourcengruppe und Ressource. Wenn Sie als globaler Administrator versuchen, Privileged Identity Management für Ihre Azure-Ressourcen bereitzustellen, können Sie [die Zugriffsrechte zum Verwalten aller Azure-Abonnements erhöhen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json), um sich selbst Zugriff auf alle Azure-Ressourcen im Verzeichnis für die Ermittlung zu erteilen. Wir raten jedoch dazu, die Genehmigung aller Ihrer Abonnementbesitzer einzuholen, bevor Sie deren Ressourcen mit Privileged Identity Management verwalten.

### <a name="enforce-principle-of-least-privilege"></a>Durchsetzen des Prinzips der geringsten Rechte

Stellen Sie unbedingt sicher, dass Sie das Prinzip der geringsten Rechte in Ihrer Organisation für Ihre Azure AD- und Azure-Rollen durchgesetzt haben.

#### <a name="plan-least-privilege-delegation"></a>Planen der Delegierung der geringsten Rechte

Bei Azure AD-Rollen ist es üblich, dass Organisationen die Rolle „Globaler Administrator“ mehreren Administratoren zuweisen, obwohl die meisten Administratoren nur eine oder zwei bestimmte und weniger umfangreiche Administratorrollen benötigen. Bei einer großen Anzahl globaler Administratoren oder anderer Rollen mit hohen Berechtigungen ist es schwierig, Ihre privilegierten Rollenzuweisungen genau genug nachzuverfolgen.

Führen Sie die folgenden Schritte aus, um das Prinzip der geringsten Rechte für Ihre Azure AD-Rollen zu implementieren.

1. Lesen Sie den Artikel [Integrierte Rollen in Azure AD](../roles/permissions-reference.md), um die Granularität der Rollen zu verstehen. Sie und Ihr Team sollten außerdem den Artikel [Administratorrollen nach Administratoraufgabe in Azure Active Directory](../roles/delegate-by-task.md) lesen, in dem die am wenigsten privilegierte Rolle für bestimmte Aufgaben erläutert wird.

1. Erstellen Sie eine Liste der Benutzer mit privilegierten Rollen in Ihrer Organisation. Mit der Privileged Identity Management-Funktion [Ermittlung und Erkenntnisse (Vorschau)](pim-security-wizard.md) können Sie das Risiko reduzieren.

    ![Seite „Ermittlung und Erkenntnisse (Vorschau)“ zum Reduzieren des Risikos mithilfe privilegierter Rollen](./media/pim-deployment-plan/new-preview-page.png)

1. Ermitteln Sie für alle globalen Administratoren in Ihrer Organisation, warum sie die Rolle benötigen. Entfernen Sie diese dann aus der Rolle „Globaler Administrator“, und weisen Sie in Azure Active Directory integrierte Rollen oder benutzerdefinierte Rollen mit geringeren Berechtigungen zu. Hinweis: Bei Microsoft gibt es derzeit nur ungefähr 10 Administratoren mit der Rolle „Globaler Administrator“. Weitere Informationen finden Sie unter [Verwenden von Azure AD Privileged Identity Management für erhöhte Zugriffsrechte](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access).

1. Für alle anderen Azure AD-Rollen überprüfen Sie die Liste der Zuweisungen, identifizieren Administratoren, die die Rolle nicht mehr benötigen, und entfernen diese aus ihren Zuweisungen.

Wenn Sie die letzten beiden Schritte automatisieren möchten, können Sie Zugriffsüberprüfungen in Privileged Identity Management verwenden. Durch Ausführen der unter [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-start-security-review.md) beschriebenen Schritte können Sie eine Zugriffsüberprüfung für alle Azure AD-Rollen mit mindestens einem Mitglied einrichten.

![Erstellen eines Zugriffsüberprüfungsbereich für Azure AD-Rollen](./media/pim-deployment-plan/create-access-review.png)

Legen Sie die Prüfer auf **Mitglieder (selbst)** fest. Alle Benutzer mit der Rolle erhalten eine E-Mail, in der sie aufgefordert werden, zu bestätigen, dass sie den Zugriff benötigen. Aktivieren Sie außerdem in den erweiterten Einstellungen die Option **Bei Genehmigung Grund anfordern**, damit Benutzer angeben müssen, warum sie die Rolle benötigen. Basierend auf diesen Informationen können Sie Benutzer aus nicht erforderlichen Rollen entfernen oder genauer abgestimmte Administratorrollen an sie delegieren.

Bei Zugriffsüberprüfungen werden E-Mails gesendet, um Personen dazu aufzufordern, ihren Zugriff auf die Rollen zu überprüfen. Wenn Sie privilegierte Konten haben, mit denen keine E-Mail-Adressen verknüpft sind, achten Sie darauf, für diese Konten das Feld für die sekundäre E-Mail-Adresse auszufüllen. Weitere Informationen finden Sie im Artikel zum [proxyAddresses-Attribut in Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="plan-azure-resource-role-delegation"></a>Planen der Delegierung von Azure-Ressourcenrollen

Für Azure-Abonnements und _Ressourcen können Sie einen ähnlichen Zugriffsüberprüfungsprozess einrichten, um die Rollen in den einzelnen Abonnements oder Ressourcen zu überprüfen. Das Ziel dieses Prozesses besteht in der Minimierung der Zuweisungen der Rollen „Besitzer“ und „Benutzerzugriffsadministrator“ für die einzelnen Abonnements oder Ressourcen und in der Entfernung unnötiger Zuweisungen. Organisationen delegieren solche Aufgaben jedoch häufig an den Besitzer eines Abonnements oder einer Ressource, da sie ein besseres Verständnis der bestimmten Rollen (insbesondere benutzerdefinierten Rollen) haben.

Wenn Sie als Benutzer mit der Rolle „Globaler Administrator“ versuchen, PIM für Azure-Rollen in Ihrer Organisation bereitzustellen, können Sie [die Zugriffsrechte zum Verwalten aller Azure-Abonnements erhöhen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json), um Zugriff auf die einzelnen Abonnements zu erhalten. Anschließend können Sie die Besitzer der einzelnen Abonnements ermitteln und in Zusammenarbeit mit ihnen unnötige Zuweisungen entfernen und die Zuweisung der Besitzerrolle minimieren.

Benutzer mit der Rolle „Besitzer“ für ein Azure-Abonnement können auch [Zugriffsüberprüfungen für Azure-Ressourcen](pim-resource-roles-start-access-review.md) verwenden, um unnötige Rollenzuweisungen mit einem ähnlichen Vorgang wie oben für Azure AD-Rollen beschrieben zu überprüfen und zu entfernen.

### <a name="decide-which-role-assignments-should-be-protected-by-privileged-identity-management"></a>Entscheiden, welche Rollenzuweisungen von Privileged Identity Management geschützt werden sollen

Nach dem Bereinigen der Zuweisungen privilegierter Rollen in Ihrer Organisation müssen Sie entscheiden, welche Rollen mit Privileged Identity Management geschützt werden sollen.

Wenn eine Rolle von Privileged Identity Management geschützt wird, müssen berechtigte Benutzer, die ihr zugewiesen sind, die Rechte erhöhen, um die von der Rolle gewährten Berechtigungen zu nutzen. Die Erhöhung von Rechten kann auch das Einholen einer Genehmigung, das Verwenden einer mehrstufigen Authentifizierung und das Angeben eines Grunds für die Aktivierung umfassen. Privileged Identity Management kann Erhöhungen von Rechten auch über Benachrichtigungen sowie über die Privileged Identity Management- und Azure AD-Überwachungsereignisprotokolle nachverfolgen.

Die Auswahl der mit Privileged Identity Management zu schützenden Rollen kann schwierig sein und unterscheidet sich je nach Organisation. Dieser Abschnitt enthält unsere empfohlenen bewährten Methoden für Azure AD-Rollen und Azure-Rollen.

#### <a name="azure-ad-roles"></a>Azure AD-Rollen

Das Priorisieren des Schutzes von Azure AD-Rollen mit den meisten Berechtigungen ist wichtig. Basierend auf den Nutzungsmustern aller Privileged Identity Management-Kunden ergeben sich die folgenden 10 wichtigsten von Privileged Identity Management verwalteten Azure AD-Rollen:

1. Globaler Administrator
1. Sicherheitsadministrator
1. Benutzeradministrator
1. Exchange-Administrator
1. SharePoint-Administrator
1. Intune-Administrator
1. Sicherheitsleseberechtigter
1. Dienstadministrator
1. Rechnungsadministrator
1. Skype for Business-Administrator

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie zuerst alle globalen Administratoren und Sicherheitsadministratoren mithilfe von Privileged Identity Management, weil diese Benutzer bei einer Kompromittierung den meisten Schaden anrichten können.

Es ist wichtig zu berücksichtigen, welche Daten und Berechtigungen für Ihre Organisation am sensibelsten sind. Beispielsweise sollten einige Organisationen ihre Power BI-Administratorrolle oder ihre Teams-Administratorrolle mit Privileged Identity Management schützen, weil diese Administratoren auf Daten zugreifen und zentrale Workflows ändern können.

Anfällig für Angriffe sind Rollen, denen Gastbenutzer zugewiesen sind.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie alle Rollen für Gastbenutzer mit Privileged Identity Management, um Risiken im Zusammenhang mit kompromittierten Gastbenutzerkonten zu reduzieren.

Leserrollen wie „Verzeichnisleseberechtigter“, „Nachrichtencenter-Leseberechtigter“ und „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“ werden manchmal als weniger wichtig angesehen als andere Rollen, weil sie nicht über Schreibberechtigungen verfügen. Einige unserer Kunden schützen jedoch auch diese Rollen, weil Angreifer, die Zugriff auf diese Konten haben, möglicherweise vertrauliche Daten wie z. B. personenbezogene Daten lesen können. Berücksichtigen Sie dieses Risiko, wenn Sie entscheiden, ob Leserrollen in Ihrer Organisation mit Privileged Identity Management verwaltet werden sollen.

#### <a name="azure-roles"></a>Azure-Rollen

Bei der Entscheidung, welche Rollenzuweisungen mithilfe von Privileged Identity Management für Azure-Ressourcen verwaltet werden sollen, müssen Sie zunächst die Abonnements/Ressourcen identifizieren, die für Ihre Organisation am wichtigsten sind. Beispiele für solche Abonnements/Ressourcen sind:

- Ressourcen, welche die sensibelsten Daten hosten
- Ressourcen, von denen zentrale kundenorientierte Anwendungen abhängig sind

Wenn Sie sich als globaler Administrator unsicher sind, welche Abonnements und Ressourcen am wichtigsten sind, bitten Sie die Abonnementbesitzer in Ihrer Organisation, eine Liste der von jedem Abonnement verwalteten Ressourcen zusammenzustellen. Gruppieren Sie dann in Zusammenarbeit mit den Abonnementbesitzern die Ressourcen basierend auf dem Schweregrad im Fall einer Kompromittierung (niedrig, mittel, hoch). Priorisieren Sie die Verwaltung von Ressourcen mit Privileged Identity Management basierend auf diesem Schweregrad.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Arbeiten Sie mit Abonnement-/Ressourcenbesitzern von kritischen Diensten zusammen, um den Privileged Identity Management-Workflow für alle Rollen in sensiblen Abonnements/Ressourcen einzurichten.

Privileged Identity Management für Azure-Ressourcen unterstützt zeitgebundene Dienstkonten. Behandeln Sie Dienstkonten genauso, wie Sie ein normales Benutzerkonto behandeln würden.

Bei Abonnements/Ressourcen, die nicht so kritisch sind, müssen Sie Privileged Identity Management nicht für alle Rollen einrichten. Die Rollen „Besitzer“ und „Benutzerzugriffsadministrator“ sollten Sie jedoch weiterhin mit Privileged Identity Management schützen.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Verwalten Sie die Rollen „Besitzer“ und „Benutzerzugriffsadministrator“ aller Abonnements/Ressourcen mit Privileged Identity Management.

### <a name="decide-whether-to-use-a-group-to-assign-roles"></a>Entscheiden, ob Rollen einer Gruppe zugewiesen werden sollen

Ob eine Rolle einer Gruppe (statt einzelnen Benutzern) zugewiesen werden soll, ist eine strategische Entscheidung. Ziehen Sie in den folgenden Fällen bei der Planung in Betracht, einer Gruppe eine Rolle zuzuweisen, um Rollenzuweisungen zu verwalten:

- Viele Benutzer sind einer Rolle zugewiesen
- Sie möchten die Zuweisung der Rolle delegieren

#### <a name="many-users-are-assigned-to-a-role"></a>Viele Benutzer sind einer Rolle zugewiesen

Die Nachverfolgung, welchen Benutzern eine Rolle zugewiesen ist, und die bedarfsorientierte Verwaltung der jeweiligen Zuweisungen sind Aufgaben, die bei manueller Ausführung Zeit in Anspruch nehmen können. Wenn Sie eine Rolle einer Gruppe zuweisen möchten, [erstellen Sie zuerst eine Gruppe, der Rollen zugewiesen werden können](../roles/groups-create-eligible.md), und weisen Sie dann die Gruppe als für eine Rolle berechtigt zu. Bei dieser Aktion unterliegen alle Benutzer in der Gruppe demselben Aktivierungsprozess wie einzelne Benutzer, die für eine Erhöhung in die Rolle berechtigt sind. Gruppenmitglieder aktivieren ihre Zuweisungen für die Gruppe einzeln mithilfe des Aktivierungsanforderungs- und Genehmigungsprozesses von Privileged Identity Management. Die Gruppe wird nicht aktiviert, sondern nur die Gruppenmitgliedschaft des Benutzers.

#### <a name="you-want-to-delegate-assigning-the-role"></a>Sie möchten die Zuweisung der Rolle delegieren

Ein Gruppenbesitzer kann die Mitgliedschaft für eine Gruppe verwalten. Für Gruppen, denen Azure AD-Rollen zugewiesen werden können, sind nur der Administrator für privilegierte Rollen, der globale Administrator und die Gruppenbesitzer in der Lage, die Gruppenmitgliedschaft zu verwalten. Wenn der Gruppe neue Mitglieder hinzugefügt werden, erhalten diese Mitglieder Zugriff auf die Rollen, die der Gruppe zugewiesen sind, unabhängig davon, ob die Zuweisung berechtigt oder aktiv ist. Verwenden Sie Gruppenbesitzer, um die Verwaltung der Gruppenmitgliedschaft für eine zugewiesene Rolle zu delegieren, damit der erforderliche Umfang der Berechtigungen reduziert werden kann. Weitere Informationen zum Zuweisen eines Besitzers zu einer Gruppe beim Erstellen der Gruppe finden Sie unter [Erstellen einer Gruppe in Azure Active Directory für das Zuweisen von Rollen](../roles/groups-create-eligible.md).

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Binden Sie Gruppen, denen Azure AD-Rollen zugewiesen werden können, in die Verwaltung in Privileged Identity Management ein. Nach der Integration einer Gruppe, der Rollen zugewiesen werden können, in die Verwaltung in PIM wird sie als Gruppe mit privilegiertem Zugriff bezeichnet. Verwenden Sie PIM, um zu erzwingen, dass Gruppenbesitzer ihre Rollenzuweisung „Besitzer“ aktivieren, bevor sie Gruppenmitgliedschaften verwalten können. Weitere Informationen zum Einbinden von Gruppen in die PIM-Verwaltung finden Sie unter [Verwalten von Gruppen mit privilegiertem Zugriff (Vorschau) in Privileged Identity Management](groups-discover-groups.md).

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Entscheiden, welche Rollenzuweisungen permanent oder berechtigt sein sollen

Nachdem Sie die Liste der von Privileged Identity Management zu verwaltenden Rollen festgelegt haben, müssen Sie entscheiden, welche Benutzer die berechtigte Rolle anstatt der dauerhaft aktiven Rolle erhalten sollen. Dauerhaft aktive Rollen sind die normalen über Azure Active Directory und Azure-Ressourcen zugewiesenen Rollen, während berechtigte Rollen nur in Privileged Identity Management zugewiesen werden können.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie für Azure AD-Rollen und Azure-Rollen keine anderen dauerhaft aktiven Zuweisungen fest als die empfohlenen [zwei Konten für den Notfallzugriff](../roles/security-emergency-access.md), die über die permanente Rolle „Globaler Administrator“ verfügen müssen.

Wir empfehlen zwar, überhaupt keinen ständigen Administratorzugriff bereitzustellen, für Organisationen ist es jedoch mitunter schwierig, dies sofort umzusetzen. Folgende Aspekte sind bei dieser Entscheidung zu berücksichtigen:

- Häufigkeit der Rechteerhöhung: Wenn der Benutzer die privilegierte Zuweisung nur einmal benötigt, sollte er nicht die permanente Zuweisung haben. Wenn der Benutzer andererseits die Rolle für seine tägliche Arbeit benötigt und seine Produktivität durch die Verwendung von Privileged Identity Management erheblich verringert würde, kann er für die permanente Rolle in Betracht gezogen werden.
- Für Ihre Organisation spezifische Fälle: Wenn die Person, der die berechtigte Rolle zugewiesen wird, einem entfernten Team angehört oder eine hochrangige Führungskraft ist, sodass die Kommunikation und die Durchsetzung der Erhöhung der Rechte schwierig ist, kann die Person für die permanente Rolle in Betracht gezogen werden.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie laufende Zugriffsüberprüfungen für Benutzer mit permanenten Rollenzuweisungen ein (falls vorhanden). Im letzten Abschnitt dieses Bereitstellungsplans erfahren Sie mehr über laufende Zugriffsüberprüfungen.

### <a name="draft-your-privileged-identity-management-settings"></a>Entwerfen Ihrer Privileged Identity Management-Einstellungen

Vor dem Implementieren Ihrer Privileged Identity Management-Lösung empfiehlt es sich, einen Entwurf der Privileged Identity Management-Einstellungen für jede privilegierte Rolle zu erstellen, die in Ihrer Organisation verwendetet wird. Dieser Abschnitt enthält einige Beispiele für Privileged Identity Management-Einstellungen für bestimmte Rollen (diese dienen nur als Referenz und können für Ihre Organisation anders ausfallen). Jede dieser Einstellungen wird nach den Tabellen mit den Empfehlungen von Microsoft ausführlich erläutert.

#### <a name="privileged-identity-management-settings-for-azure-ad-roles"></a>Privileged Identity Management-Einstellungen für Azure AD-Rollen

| Role | Anfordern von MFA | Benachrichtigung | Vorfallsticket | Genehmigung anfordern | Genehmigende Person | Aktivierungsdauer | Permanenter Administrator |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Globaler Administrator | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere globale Administratoren | 1 Stunde | Konten für den Notfallzugriff |
| Exchange-Administrator | :heavy_check_mark: | :heavy_check_mark: | :x: | :x: | Keine | 2 Stunden | Keine |
| Helpdeskadministrator | :x: | :x: | :heavy_check_mark: | :x: | Keine | 8 Stunden | Keine |

#### <a name="privileged-identity-management-settings-for-azure-roles"></a>Privileged Identity Management-Einstellungen für Azure-Rollen

| Role | Anfordern von MFA | Benachrichtigung | Genehmigung anfordern | Genehmigende Person | Aktivierungsdauer | Aktiver Administrator | Ablauf der Aktivierung | Ablauf der Berechtigung |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Besitzer von wichtigen Abonnements | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere Besitzer des Abonnements | 1 Stunde | Keine | – | 3 Monate |
| Benutzerzugriffsadministrator von weniger wichtigen Abonnements | :heavy_check_mark: | :heavy_check_mark: | :x: | Keine | 1 Stunde | Keine | – | 3 Monate |
| Mitwirkender von virtuellen Computern | :x: | :heavy_check_mark: | :x: | Keine | 3 Stunden | Keine | – | 6 Monate |

In der folgenden Tabelle sind die einzelnen Einstellungen beschrieben.

| Einstellung | BESCHREIBUNG |
| --- | --- |
| Role | Name der Rolle, für die Sie die Einstellungen definieren. |
| Anfordern von MFA | Gibt an, ob der berechtigte Benutzer vor dem Aktivieren der Rolle eine mehrstufige Authentifizierung durchführen muss.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Erzwingen Sie die mehrstufige Authentifizierung für alle Administratorrollen, insbesondere wenn die Rollen Gastbenutzer enthalten. |
| Benachrichtigung | Ist diese Einstellung auf „true“ festgelegt, erhalten globale Administratoren, Administratoren für privilegierte Rollen und Sicherheitsadministratoren in der Organisation eine E-Mail-Benachrichtigung, wenn ein berechtigter Benutzer die Rolle aktiviert.<br/><br/>**Hinweis:** Einige Organisationen haben keine E-Mail-Adressen mit ihren Administratorkonten verknüpft. Um diese E-Mail-Benachrichtigungen zu erhalten müssen Sie eine alternative E-Mail-Adresse festlegen, damit Administratoren diese E-Mails erhalten. |
| Vorfallsticket | Gibt an, ob der berechtigte Benutzer beim Aktivieren seiner Rolle eine Vorfallsticketnummer erfassen muss. Mit dieser Einstellung kann eine Organisation jede Aktivierung anhand einer internen Vorfallnummer identifizieren, um unerwünschte Aktivierungen zu verringern.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Nutzen Sie Incidentticketnummern, um Privileged Identity Management mit Ihrem internen System zu verknüpfen. Diese Methode kann für genehmigende Personen nützlich sein, die Kontext für die Aktivierung benötigen. |
| Genehmigung anfordern | Gibt an, ob der berechtigte Benutzer eine Genehmigung zum Aktivieren der Rolle einholen muss.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie die Genehmigung für Rollen mit der höchsten Berechtigung ein. Basierend auf den Nutzungsmustern aller Privileged Identity Management-Kunden handelt es sich bei den Rollen „Globaler Administrator“, „Benutzeradministrator“, „Exchange-Administrator“, „Sicherheitsadministrator“ und „Kennwortadministrator“ um die am häufigsten verwendeten Rollen mit eingerichteter Genehmigung. |
| Genehmigende Person | Wenn zum Aktivieren der berechtigten Rolle eine Genehmigung erforderlich ist, listen Sie die Personen auf, von denen die Anforderung genehmigt werden muss. Standardmäßig legt Privileged Identity Management alle Benutzer als genehmigende Person fest, die Administrator für privilegierte Rollen sind, unabhängig davon, ob es sich um eine permanente oder berechtigte Zuweisung handelt.<br/><br/>**Hinweis:** Wenn ein Benutzer sowohl für eine Azure AD-Rolle berechtigt als auch eine genehmigende Person der Rolle ist, kann er sich nicht selbst genehmigen.<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Wählen Sie als genehmigende Personen statt eines globalen Administrators Benutzer aus, die am besten über die Rolle und deren häufige Benutzer Bescheid wissen. |
| Aktivierungsdauer | Die Zeitspanne, in der ein Benutzer in der Rolle aktiviert ist, bevor sie abläuft. |
| Permanenter Administrator | Liste der Benutzer, die ein permanenter Administrator für die Rolle sein werden (d. h. sie nie aktivieren müssen).<br/><br/> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie keine ständigen Administratoren für Rollen fest, mit Ausnahme von globalen Administratoren. Mehr dazu erfahren Sie in diesem Plan in den Abschnitten dazu, wer berechtigte und wer dauerhaft aktive Rollenzuweisungen erhalten sollte. |
| Aktiver Administrator | Für Azure-Ressourcen ist „Aktiver Administrator“ die Liste der Benutzer, die zur Verwendung der Rolle nie eine Aktivierung durchführen müssen. Diese wird nicht als permanenter Administrator wie bei Azure AD-Rollen bezeichnet, weil Sie eine Ablaufzeit festlegen können, nach der ein Benutzer diese Rolle verliert. |
| Ablauf der Aktivierung | Aktive Rollenzuweisungen für Azure-Rollen laufen nach der konfigurierten Dauer ab. Sie können zwischen 15 Tagen, 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr und dauerhafter Aktivität wählen. |
| Ablauf der Berechtigung | Berechtigte Rollenzuweisungen für Azure-Rollen laufen nach dieser Dauer ab. Sie können zwischen 15 Tagen, 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr und dauerhafter Berechtigung wählen. |

## <a name="implementation-plan"></a>Implementierungsplan

Die Grundlage für eine gründliche Planung ist die Basis, auf der Sie eine Anwendung in Azure Active Directory erfolgreich bereitstellen können.  Sie bietet intelligente Sicherheit und Integration, die das Onboarding vereinfacht und den Zeitaufwand für erfolgreiche Bereitstellungen reduziert.  Mit dieser Kombination wird sichergestellt, dass Ihre Anwendung problemlos integriert und gleichzeitig Ausfallzeiten für Ihre Endbenutzer vorgebeugt wird.

### <a name="identify-test-users"></a>Identifizieren von Testbenutzern

Verwenden Sie diesen Abschnitt, um einen Satz von Benutzern und Gruppen identifizieren, um die Implementierung zu überprüfen. Identifizieren Sie basierend auf den Einstellungen, die Sie im Planungsabschnitt ausgewählt haben, die Benutzer, die Sie für jede Rolle testen möchten.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Legen Sie Dienstbesitzer der einzelnen Azure AD-Rollen als Testbenutzer fest, damit sie mit dem Prozess vertraut werden und die Einführung intern unterstützen können.

Identifizieren Sie in dieser Tabelle die Testbenutzer, die überprüfen, ob die Einstellungen für die Rollen funktionieren.

| Rollenname | Testbenutzer |
| --- | --- |
| &lt;Rollenname&gt; | &lt;Benutzer zum Testen der Rolle&gt; |
| &lt;Rollenname&gt; | &lt;Benutzer zum Testen der Rolle&gt; |

### <a name="test-implementation"></a>Testimplementierung

Nachdem Sie nun die Testbenutzer identifiziert haben, konfigurieren Sie in diesem Schritt Privileged Identity Management für Ihre Testbenutzer. Wenn Ihre Organisation einen Privileged Identity Management-Workflow in Ihre eigene interne Anwendung integrieren möchte, statt Privileged Identity Management im Azure-Portal zu verwenden, werden alle Vorgänge in Privileged Identity Management auch über unsere [Graph-API](/graph/api/resources/privilegedidentitymanagement-root) unterstützt.

#### <a name="configure-privileged-identity-management-for-azure-ad-roles"></a>Konfigurieren von Privileged Identity Management für Azure AD-Rollen

1. [Konfigurieren Sie die Einstellungen für Azure AD-Rollen](pim-how-to-change-default-settings.md) basierend auf Ihrer Planung.

1. Navigieren Sie zu **Azure AD-Rollen**, wählen Sie **Rollen** aus, und wählen Sie dann die von Ihnen konfigurierte Rolle aus.

1. Wenn die Gruppe von Testbenutzern bereits ein permanenter Administrator ist, können Sie diese als berechtigt festlegen, indem Sie nach den Benutzern suchen und sie von permanent in berechtigt ändern. Wählen Sie hierzu die drei Punkte in der jeweils zugehörigen Zeile aus. Wenn sie die Rollenzuweisungen noch nicht haben, können Sie [neue berechtigte Zuweisung erstellen](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role).

1. Wiederholen Sie die Schritte 1 bis 3 für alle Rollen, die Sie testen möchten.

1. Nachdem Sie die Testbenutzer eingerichtet haben, müssen Sie ihnen den Link mit den Anweisungen zum [Aktivieren ihrer Azure AD-Rolle](pim-how-to-activate-role.md) senden.

#### <a name="configure-privileged-identity-management-for-azure-roles"></a>Konfigurieren von Privileged Identity Management für Azure-Rollen

1. [Konfigurieren Sie die Einstellungen für die Azure-Ressourcenrolle](pim-resource-roles-configure-role-settings.md) für eine Rolle in einem Abonnement oder einer Ressource, die Sie testen möchten.

1. Navigieren Sie zu **Azure-Ressourcen** für dieses Abonnement, und wählen Sie **Rollen** und dann die von Ihnen konfigurierte Rolle aus.

1. Wenn die Gruppe von Testbenutzern bereits ein aktiver Administrator ist, können Sie sie als berechtigt festlegen. Dazu suchen Sie nach den Benutzern und [aktualisieren ihre Rollenzuweisung](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment). Wenn sie die Rolle noch nicht haben, können Sie [eine neue Rolle zuweisen](pim-resource-roles-assign-roles.md#assign-a-role).

1. Wiederholen Sie die Schritte 1 bis 3 für alle Rollen, die Sie testen möchten.

1. Nachdem Sie die Testbenutzer eingerichtet haben, müssen Sie ihnen den Link mit den Anweisungen zum [Aktivieren ihrer Azure-Ressourcenrolle](pim-resource-roles-activate-your-roles.md) senden.

Überprüfen Sie in dieser Phase, ob die gesamte von Ihnen für die Rollen festgelegte Konfiguration ordnungsgemäß funktioniert. Verwenden Sie die folgende Tabelle zum Dokumentieren Ihrer Tests. Nutzen Sie diese Phase auch zur Optimierung der Kommunikation mit betroffenen Benutzern.

| Role | Erwartetes Verhalten während der Aktivierung | Tatsächliche Ergebnisse |
| --- | --- | --- |
| Globaler Administrator | (1) Mehrstufige Authentifizierung erfordern<br/>(2) Genehmigung erfordern<br/>(3) Genehmigende Person erhält Benachrichtigung und kann genehmigen<br/>(4) Rolle läuft nach voreingestellter Zeit ab |  |
| Besitzer von Abonnement *X* | (1) Mehrstufige Authentifizierung erfordern<br/>(2) Berechtigte Zuweisung läuft nach konfiguriertem Zeitraum |  |

### <a name="communicate-privileged-identity-management-to-affected-stakeholders"></a>Informieren der betroffenen Projektbeteiligten über Privileged Identity Management

Die Bereitstellung von Privileged Identity Management bringt zusätzliche Schritte für Benutzer von privilegierten Rollen mit sich. Zwar werden Sicherheitsprobleme im Zusammenhang mit privilegierten Identitäten durch Privileged Identity Management erheblich reduziert, die Änderung muss jedoch vor der organisationsweiten Bereitstellung effektiv kommuniziert werden. Je nach Anzahl der betroffenen Administratoren entscheiden sich Organisationen häufig für die Erstellung eines internen Dokuments, eines Videos oder einer E-Mail zu dieser Änderung. Folgende Informationen sind in diesen Mitteilungen häufig enthalten:

- Was ist PIM?
- Was ist der Vorteil für die Organisation?
- Wer ist betroffen?
- Wann wird PIM eingeführt?
- Welche zusätzlichen Schritte müssen Benutzer zum Aktivieren ihrer Rolle ausführen?
    - Senden Sie Link zu unserer Dokumentation:
    - [Aktivieren von Azure AD-Verzeichnisrollen in PIM](pim-how-to-activate-role.md)
    - [Aktivieren von Azure-Rollen](pim-resource-roles-activate-your-roles.md)
- Kontaktinformationen oder Helpdesk-Link für Probleme im Zusammenhang mit PIM

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Vereinbaren Sie eine Zeit mit den Mitgliedern Ihres Helpdesk-/Supportteams, um sie Schritt für Schritt durch den Privileged Identity Management-Workflow zu führen (falls Ihre Organisation über ein internes IT-Supportteam verfügt). Stellen Sie ihnen die entsprechende Dokumentationen sowie Ihre Kontaktdaten zur Verfügung.

### <a name="move-to-production"></a>Überführen in die Produktion

Nachdem Sie Ihre Tests erfolgreich abgeschlossen haben, verschieben Sie Privileged Identity Management in die Produktionsumgebung. Dazu wiederholen Sie alle Schritte der Testphase für alle Benutzer jeder Rolle, die Sie in Ihrer Privileged Identity Management-Konfiguration definiert haben. Bei Privileged Identity Management für Azure AD-Rollen entscheiden sich Organisationen häufig dazu, Privileged Identity Management zuerst für globale Administratoren zu testen und einzuführen, bevor die Tests und Rollouts für andere Rollen erfolgen. Bei Azure-Ressourcen(rollen) dagegen nehmen Organisationen die Tests und Rollouts von Privileged Identity Management in der Regel für jeweils ein Azure-Abonnement vor.

### <a name="if-a-rollback-is-needed"></a>Wenn ein Rollback erforderlich ist

Wenn Privileged Identity Management in der Produktionsumgebung nicht wie gewünscht funktioniert, können Ihnen die folgenden Rollback-Schritte bei der Zurücksetzung auf einen bekannten fehlerfreien Zustand vor der Einrichtung von Privileged Identity Management helfen:

#### <a name="azure-ad-roles"></a>Azure AD-Rollen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Wählen Sie **Azure AD-Rollen** aus, und wählen Sie dann **Rollen** aus.
1. Wählen Sie für jede von Ihnen konfigurierte Rolle die Auslassungspunkte ( **...** ) für alle Benutzer mit einer berechtigten Zuweisung aus.
1. Wählen Sie die Option **Als permanent festlegen** aus, um die Rollenzuweisung dauerhaft einzurichten.

#### <a name="azure-roles"></a>Azure-Rollen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Wählen Sie **Azure-Ressourcen** aus, und wählen Sie dann ein Abonnement oder eine Ressource aus, für das/die Sie ein Rollback ausführen möchten.
1. Wählen Sie **Rollen** aus.
1. Wählen Sie für jede von Ihnen konfigurierte Rolle die Auslassungspunkte ( **...** ) für alle Benutzer mit einer berechtigten Zuweisung aus.
1. Wählen Sie die Option **Als permanent festlegen** aus, um die Rollenzuweisung dauerhaft einzurichten.

## <a name="next-steps-after-deploying"></a>Nächste Schritte nach der Bereitstellung

Die erfolgreiche Bereitstellung von Privileged Identity Management in der Produktionsumgebung ist ein wichtiger Schritt im Hinblick auf den Schutz der privilegierten Identitäten Ihrer Organisation. Durch die Bereitstellung von Privileged Identity Management erhalten Sie zusätzliche Privileged Identity Management-Features, die Sie für die Sicherheit und Compliance verwenden sollten.

### <a name="use-privileged-identity-management-alerts-to-safeguard-your-privileged-access"></a>Verwenden von Privileged Identity Management-Warnungen zum Schützen des privilegierten Zugriffs

Weitere Informationen zum Verwenden der integrierten Warnungsfunktion von Privileged Identity Management zum Schutz Ihrer Organisation finden Sie unter [Sicherheitswarnungen](pim-how-to-configure-security-alerts.md#security-alerts). Diese Warnungen umfassen Folgendes: Administratoren verwenden keine privilegierten Rollen, Rollen sind außerhalb von Privileged Identity Management zugewiesen, Rollen werden zu häufig aktiviert und mehr. Zum vollständigen Schutz Ihrer Organisation sollten Sie die Liste der Warnungen regelmäßig prüfen und die Probleme beheben. So können Sie Ihre Warnungen anzeigen und beheben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Öffnen Sie **Azure AD Privileged Identity Management**.
1. Wählen Sie **Azure AD-Rollen** aus, und wählen Sie dann **Warnungen** aus.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Kümmern Sie sich um alle Warnungen mit hohem Schweregrad sofort. Über Warnungen mit mittlerem und geringem Schweregrad sollten Sie auf dem Laufenden bleiben und Änderungen vornehmen, wenn Sie glauben, dass ein Sicherheitsrisiko vorliegt.

Wenn bestimmte Warnungen nicht nützlich sind oder für Ihre Organisation nicht gelten, können Sie solche Warnungen jederzeit auf der Warnungsseite schließen. Diese Aktion kann später jederzeit auf der Seite „Azure AD-Einstellungen“ rückgängig gemacht werden.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Einrichten laufender Zugriffsüberprüfungen zur regelmäßigen Überprüfung der privilegierten Identitäten Ihrer Organisation

Zugriffsüberprüfungen sind die beste Methode, um Benutzer mit privilegierten Rollen oder bestimmte Prüfer zu fragen, ob sie die privilegierte Identität jeweils benötigen. Zugriffsüberprüfungen eignen sich hervorragend dazu, die Angriffsfläche zu reduzieren und konform zu bleiben. Weitere Informationen zum Starten einer Zugriffsüberprüfung finden Sie unter [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in Privileged Identity Management](pim-how-to-start-security-review.md) und [Zugriffsüberprüfungen für Azure-Rollen](pim-resource-roles-start-access-review.md). Für einige Organisationen ist das Durchführen regelmäßiger Zugriffsüberprüfungen erforderlich, um Gesetze und Vorschriften einzuhalten. Für andere wiederum sind Zugriffsüberprüfungen die beste Möglichkeit, das Prinzip der geringsten Rechte innerhalb der Organisation durchzusetzen.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie vierteljährliche Zugriffsüberprüfungen für alle Ihre Azure AD- und Azure-Rollen ein.

In den meisten Fällen ist der Prüfer für Azure AD-Rollen der Benutzer selbst. Für Azure-Rollen dagegen ist der Prüfer der Besitzer des Abonnements, in dem sich die Rolle befindet. Unternehmen haben jedoch häufig privilegierte Konten, die nicht mit der E-Mail-Adresse einer bestimmten Person verknüpft sind. In diesen Fällen liest niemand die E-Mails und überprüft den Zugriff.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Fügen Sie eine sekundäre E-Mail-Adresse für alle Konten mit Zuweisungen privilegierter Rollen hinzu, die nicht mit einer regelmäßig überprüften E-Mail-Adresse verknüpft sind.

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Optimale Nutzung des Überwachungsprotokolls zur Verbesserung der Sicherheit und Compliance

Mithilfe des Überwachungsprotokolls bleiben Sie auf dem Laufenden und halten Richtlinien ein. Privileged Identity Management speichert derzeit einen 30-tägigen Verlauf des gesamten Verlaufs Ihrer Organisation im Überwachungsprotokoll einschließlich der folgenden Informationen:

- Aktivierung/Deaktivierung berechtigter Rollen
- Rollenzuweisungsaktivitäten innerhalb und außerhalb von Privileged Identity Management
- Änderungen an Rolleneinstellungen
- Anforderungs-/Genehmigungs-/Verweigerungsaktivitäten für die Rollenaktivierung mit eingerichteter Genehmigung
- Warnungsaktualisierungen

Globale Administratoren und Administratoren für privilegierte Rollen können auf die Überwachungsprotokolle zugreifen. Weitere Informationen finden Sie unter [Anzeigen des Überwachungsverlaufs für Azure AD-Rollen](pim-how-to-use-audit-log.md) und [Anzeigen von Aktivitäten und des Überwachungsverlaufs für Azure-Ressourcenrollen in Privileged Identity Management](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Mindestens ein Administrator sollte wöchentlich alle Überwachungsereignisse lesen und monatlich alle Überwachungsereignisse exportieren.

Wenn Sie Ihre Überwachungsereignisse für einen längeren Zeitraum automatisch speichern möchten, wird das Privileged Identity Management-Überwachungsprotokoll automatisch mit den [Azure AD-Überwachungsprotokollen](../reports-monitoring/concept-audit-logs.md) synchronisiert.

> [!TIP]
> :heavy_check_mark: **Microsoft-Empfehlung**: Richten Sie die [Azure-Protokollüberwachung](../reports-monitoring/concept-activity-logs-azure-monitor.md) ein, um Überwachungsereignisse zwecks erhöhter Sicherheit und zu Compliancezwecken in einem Azure-Speicherkonto zu archivieren.
