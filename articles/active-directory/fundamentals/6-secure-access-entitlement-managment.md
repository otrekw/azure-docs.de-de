---
title: Verwalten des externen Zugriffs mit der Azure Active Directory-Berechtigungsverwaltung
description: Verwenden der Azure Active Directory-Berechtigungsverwaltung im Rahmen des allgemeinen externen Zugriffssicherheitsplans
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f06a54f59405d9833194b2e7d4488bc93d2437ae
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98725177"
---
# <a name="manage-external-access-with-entitlement-management"></a>Verwalten des Zugriffs mit der Berechtigungsverwaltung 


Die [Berechtigungsverwaltung](../governance/entitlement-management-overview.md) ist eine Identity Governance-Funktion, mit der Organisationen Identitäten und Zugriffszyklus skaliert verwalten können, indem sie Zugriffsanforderungsworkflows, Zugriffszuweisungen, Überprüfungen und Ablaufzeiten automatisieren. Die Berechtigungsverwaltung ermöglicht delegierten Nicht-Administratoren das Erstellen von [Zugriffspaketen](../governance/entitlement-management-overview.md), auf die externe Benutzer anderer Organisationen Zugriff anfordern können. Ein- und mehrstufige Genehmigungsworkflows können so konfiguriert werden, dass Anforderungen ausgewertet und wiederkehrende Bewertungen mit zeitlich begrenztem Zugriff für Benutzer [bereitgestellt](../governance/what-is-provisioning.md) werden. Die Berechtigungsverwaltung ermöglicht die richtlinienbasierte Bereitstellung und das Aufheben der Bereitstellung externer Konten.

## <a name="key-concepts-for-enabling-entitlement-management"></a>Wichtige Begriffe für die Aktivierung der Berechtigungsverwaltung

Die folgenden Begriffe sind wichtig, um die Berechtigungsverwaltung zu verstehen.

### <a name="access-packages"></a>Zugriffspakete

Ein [Zugriffspaket](../governance/entitlement-management-overview.md) ist die Grundlage für die Berechtigungsverwaltung. Zugriffspakete sind Gruppierungen von richtliniengesteuerten Ressourcen, die ein Benutzer benötigt, um an einem Projekt mitzuarbeiten oder andere Aufgaben auszuführen. Ein Zugriffspaket kann z. B. Folgendes umfassen:

* Zugriff auf bestimmte SharePoint-Websites

* Unternehmensanwendungen, einschließlich Ihrer benutzerdefinierten internen und Saas-Apps wie Salesforce

* Microsoft Teams-Kanäle

* Microsoft 365-Gruppen 

### <a name="catalogs"></a>Kataloge

Zugriffspakete befinden sich in [Katalogen](../governance/entitlement-management-catalog-create.md). Sie erstellen einen Katalog, wenn Sie zugehörige Ressourcen und Zugriffspakete gruppieren und deren Verwaltung delegieren möchten. Zuerst fügen Sie Ressourcen zu einem Katalog hinzu. Dann können Sie diese Ressourcen zu Zugriffspaketen hinzufügen. Beispielsweise können Sie einen Katalog „Finanzen“ erstellen [und dessen Verwaltung an ein Mitglied der Finanzabteilung delegieren](../governance/entitlement-management-delegate.md). Diese Person kann dann [Ressourcen hinzufügen](../governance/entitlement-management-catalog-create.md), Zugriffspakete erstellen und die Zugriffsgenehmigung für diese Pakete verwalten.

Das folgende Diagramm zeigt einen typischen Governancelebenszyklus für einen externen Benutzer, der Zugriffsberechtigungen für ein Zugriffspaket erhält, das eine Ablaufzeit hat.

![Ein Diagramm vom Governancezyklus des externen Benutzers.](media/secure-external-access/6-governance-lifecycle.png)

### <a name="self-service-external-access"></a>Externer Self-Service-Zugriff

Sie können Zugriffspakete über [„Mein Zugriff“ des Azure AD-Portals](../governance/entitlement-management-request-access.md) bereitstellen, um externen Benutzern das Anfordern von Zugriff zu ermöglichen. Über Richtlinien ist festgelegt, wer ein Zugriffspaket anfordern kann. Sie legen die Berechtigungen für das Zugriffspaket fest:

* Bestimmte [verbundene Organisationen](../governance/entitlement-management-organization.md)

* Alle konfigurierten verbundenen Organisationen

* Alle Benutzer aus beliebigen Organisationen

* Bereits in Ihrem Mandanten vorhandene Mitglieder oder Gastbenutzer

### <a name="approvals"></a>Genehmigungen   
Zugriffspakete können eine obligatorische Genehmigung für den Zugriff enthalten. **Implementieren Sie für externe Benutzer immer Genehmigungsprozesse**. Eine Genehmigung kann ein- oder mehrstufig sein. Genehmigungen werden durch Richtlinien festgelegt. Wenn sowohl interne als auch externe Benutzer auf dasselbe Paket zugreifen müssen, richten Sie wahrscheinlich unterschiedliche Zugriffsrichtlinien für verschiedene Kategorien verbundener Organisationen und für interne Benutzer ein.

### <a name="expiration"></a>Ablauf  
Zugriffspakete können ein Ablaufdatum aufweisen. Die Ablauffrist kann auf einen bestimmten Tag festgelegt werden oder es wird dem Benutzer eine bestimmte Anzahl von Tagen für den Zugriff gewährt. Wenn das Zugriffspaket abläuft und der Benutzer kein andere Zugriffsmöglichkeit hat, kann das B2B-Gastbenutzerobjekt, das den Benutzer darstellt, gelöscht oder bei der Anmeldung blockiert werden. Es wird empfohlen, den Ablauf von Zugriffspaketen für externe Benutzer zu erzwingen. Nicht alle Zugriffspakete verfügen über Ablauffristen. Für die, die über keine Frist verfügen, stellen Sie sicher, dass Sie Zugriffsüberprüfungen durchführen.

### <a name="access-reviews"></a>Zugriffsüberprüfungen

Zugriffspakete können regelmäßige [Zugriffsüberprüfungen](../governance/manage-guest-access-with-access-reviews.md) erfordern, bei denen der Besitzer des Pakets oder ein Beauftragter die fortgesetzte Notwendigkeit für den Zugriff der Benutzer bestätigen muss. 

Stellen Sie vor dem Einrichten der Überprüfung Folgendes fest:

* Wer

   * Welche Kriterien gelten für den fortgesetzten Zugriff?

   * Wer sind die angegebenen Prüfer?

* Wie oft sollen geplante Überprüfungen erfolgen?

   * Zu den integrierten Optionen gehören monatlich, vierteljährlich, zweimal jährlich oder jährlich. 

   * Für Pakete, die den externen Zugriff unterstützen, wird vierteljährlich oder häufiger empfohlen. 

 

> [!IMPORTANT]
> Bei Zugriffsüberprüfungen von Zugriffspaketen wird nur der durch die Berechtigungsverwaltung gewährte Zugriff geprüft. Sie müssen daher andere Prozesse einführen, um den Zugriff externer Benutzer außerhalb der Berechtigungsverwaltung zu überprüfen.

Weitere Informationen zu Zugriffsüberprüfungen finden Sie unter [Planen der Bereitstellung von Azure Active Directory-Zugriffsüberprüfungen](../governance/deploy-access-reviews.md).

## <a name="using-automation-in-entitlement-management"></a>Verwenden von Automatisierungen in der Berechtigungsverwaltung

Sie können [Berechtigungsverwaltungsfunktionen ausführen, indem Sie Microsoft Graph](/graph/tutorial-access-package-api) verwenden, einschließlich

* [Verwalten von Zugriffspaketen](/graph/api/resources/accesspackage?view=graph-rest-beta)

* [Verwalten von Zugriffsüberprüfungen](/graph/api/resources/accessreviewsv2-root?view=graph-rest-beta)

* [Verwalten von verbundenen Organisationen](/graph/api/resources/connectedorganization?view=graph-rest-beta)

* [Verwalten der Einstellungen für die Berechtigungsverwaltung](/graph/api/resources/entitlementmanagementsettings?view=graph-rest-beta)

## <a name="recommendations"></a>Empfehlungen 

Es wird empfohlen, den externen Zugriff über die Berechtigungsverwaltung zu steuern.

**Für Projekte mit einem oder mehreren Geschäftspartnern [erstellen und verwenden Sie Zugriffspakete](../governance/entitlement-management-access-package-create.md), um den Benutzern dieses Partners den Zugriff auf Ressourcen** zu ermöglichen und bereitzustellen. 

* Wenn in Ihrem Verzeichnis bereits B2B-Benutzer vorhanden sind, können Sie diese auch direkt den entsprechenden Zugriffspaketen zuweisen.

* Sie können den Zugriff im [Azure-Portal](../governance/entitlement-management-access-package-assignments.md) oder über [Microsoft Graph](/graph/api/resources/accesspackageassignmentrequest?view=graph-rest-beta) zuweisen.

**Verwenden Sie Ihre Identity Governance-Einstellungen, um Benutzer aus Ihrem Verzeichnis zu entfernen, wenn deren Zugriffspakete ablaufen**.

![Screenshot: Konfigurieren der Verwaltung des Lebenszyklus von externen Benutzern](media/secure-external-access/6-manage-external-lifecycle.png)

Diese Einstellungen gelten nur für Benutzer, die über die Berechtigungsverwaltung integriert wurden.

**[Delegieren Sie die Verwaltung von Katalogen und Zugriffspaketen](../governance/entitlement-management-delegate.md) an Geschäftsinhaber, die mehr Informationen darüber haben, wer zugriffsberechtigt sein soll**.

![Screenshot: Konfigurieren eines Katalogs](media/secure-external-access/6-catalog-management.png)

**‎[Erzwingen einer Ablauffrist für Zugriffspakete](../governance/entitlement-management-access-package-lifecycle-policy.md) auf die externe Benutzer Zugriff haben.**


![Screenshot: Konfigurieren der Ablauffrist für ein Zugriffspaket](media/secure-external-access/6-access-package-expiration.png)

* Wenn Sie das Enddatum eines projektbasierten Zugriffspakets kennen, verwenden Sie dieses, um das entsprechende Datum festzulegen. 

* Andernfalls wird empfohlen, dass die Ablauffrist nicht mehr 365 Tage beträgt, es sei denn, es handelt sich um einen mehrjährigen Auftrag.

* Lassen Sie zu, dass Benutzer die Zugriffsberechtigung verlängern.

* Fordern Sie eine Genehmigung zum Erteilen der Verlängerung an.

**[Erzwingen Sie Zugriffsüberprüfungen von Paketen](../governance/manage-guest-access-with-access-reviews.md), um einen nicht zulässigen Zugriff durch Gäste zu vermeiden.**

![Screenshot: Erstellen eines neuen Zugriffspakets](media/secure-external-access/6-new-access-package.png)

* Erzwingen Sie Überprüfungen vierteljährlich.

* Geben Sie für compliancerelevante Projekte die Prüfer als festgelegte Prüfer an, damit sich externe Benutzer nicht selbst überprüfen. Die Benutzer, die Zugriffspakete verwalten, sind als Prüfer gut geeignet. 

* Bei weniger sensiblen Projekten reduziert die Selbstüberprüfung durch die Benutzer den Aufwand für die Organisation, Benutzern den Zugriff zu entziehen, die nicht mehr in ihrer ursprünglichen Organisation tätig sind.

Weitere Informationen finden Sie unter [Steuern des Zugriffs für externe Benutzer in der Azure AD-Berechtigungsverwaltung](../governance/entitlement-management-external-users.md) 

### <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie Informationen zum Sichern des externen Zugriffs auf Ressourcen. Es wird empfohlen, die Aktionen in der angegebenen Reihenfolge auszuführen.

1. [Ermitteln des Sicherheitsstatus für externen Zugriff](1-secure-access-posture.md)

2. [Ermitteln des aktuellen Status](2-secure-access-current-state.md)

3. [Erstellen eines Governanceplans](3-secure-access-plan.md)

4. [Verwenden von Gruppen für die Sicherheit](4-secure-access-groups.md)

5. [Übergang zu Azure AD B2B](5-secure-access-b2b.md)

6. [Schützen des Zugriffs mit der Berechtigungsverwaltung](6-secure-access-entitlement-managment.md) (Hier befinden Sie sich.)

7. [Schützen des Zugriffs mit Richtlinien für bedingten Zugriff](7-secure-access-conditional-access.md)

8. [Schützen des Zugriffs mit Vertraulichkeitsbezeichnungen](8-secure-access-sensitivity-labels.md)

9. [Schützen des Zugriffs auf Microsoft Teams, OneDrive und SharePoint](9-secure-access-teams-sharepoint.md)

 

