---
title: Ressourcen, Rollen und Access Control in Application Insights | Microsoft Docs
description: "\"Besitzer\", \"Mitwirkende\" und \"Leser\" für die gewonnenen Unternehmensinformationen."
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: 3d69ad7eeb7c8dd32c2e3cb286f9596bed7fe042
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87926500"
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Ressourcen, Rollen und Zugriffssteuerung in Application Insights

Anhand einer [rollenbasierten Zugriffssteuerung von Azure (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) können Sie steuern, wer in Azure [Application Insights][start] über Lese- und Aktualisierungszugriff auf Ihre Daten verfügt.

> [!IMPORTANT]
> Weisen Sie Benutzern den Zugriff in der **Ressourcengruppe oder dem Abonnement** zu, dem Ihre Anwendungsressource angehört – nicht in der Ressource selbst. Weisen Sie die Rolle **Mitwirkender der Application Insights-Komponente** zu. Dadurch wird eine einheitliche Steuerung des Zugriffs auf Webtests und Warnungen zusammen mit Ihrer Anwendungsressource sichergestellt. [Weitere Informationen](#access)


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="resources-groups-and-subscriptions"></a>Ressourcen, Gruppen und Abonnements

Zunächst einige Definitionen:

* **Ressource** – Eine Instanz eines Microsoft Azure-Diensts. Die Application Insights-Ressource erfasst, analysiert und zeigt die Telemetriedaten an, die von der Anwendung gesendet werden.  Andere Arten von Azure-Ressourcen sind Web-Apps, Datenbanken und virtuelle Computer.
  
    Um Ihre Ressourcen anzuzeigen, öffnen Sie das [Azure-Portal][portal], melden Sie sich an, und klicken Sie auf „Alle Ressourcen“. Um eine Ressource zu suchen, geben Sie einen Teil des Namens im Feld „Filter“ ein.
  
    ![Liste von Azure-Ressourcen](./media/resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Ressourcengruppe**][group]: Jede Ressource gehört zu einer Gruppe. Eine Gruppe ist eine bequeme Möglichkeit, verwandte Ressourcen insbesondere für die Zugriffssteuerung zu verwalten. Beispielsweise könnten Sie eine Web-App, eine Application Insights-Ressource zur Überwachung der App sowie eine Storage-Ressource zur Aufbewahrung exportierter Daten in eine Ressourcengruppe aufnehmen.

* [**Abonnement**](https://portal.azure.com): Um Application Insights oder sonstige Azure-Ressourcen zu verwenden, müssen Sie ein Azure-Abonnement abschließen. Jede Ressourcengruppe gehört zu einem Azure-Abonnement, in dem Sie Ihr Preispaket und, wenn es sich um ein Abonnement für die Organisation handelt, die Mitglieder und deren Zugriffsberechtigungen auswählen.
* [**Microsoft-Konto**][account]: Benutzername und Kennwort für Ihre Anmeldung bei Microsoft Azure-Abonnements, XBox Live, „Outlook.com“ und sonstigen Microsoft-Diensten.

## <a name="control-access-in-the-resource-group"></a><a name="access"></a> Steuern des Zugriffs in der Ressourcengruppe

Es ist wichtig zu wissen, dass es zusätzlich zu den Ressource, die Sie für Ihre Anwendung erstellt haben, auch separate, ausgeblendete Ressourcen für Warnungen und Webtests gibt. Sie sind derselben [Ressourcengruppe](#resource-group) zugeordnet wie Ihre Application Insights-Ressource. Möglicherweise haben Sie auch andere Azure-Dienste dort aufgenommen, z. B. Websites oder Speicher.

## <a name="to-provide-access-to-another-user"></a>So erteilen Sie einem anderen Benutzer Zugriffsberechtigungen

Sie benötigen Eigentümerrechte für das Abonnement oder die Ressourcengruppe.

Der Benutzer muss über ein [Microsoft-Konto][account]oder über Zugriff auf sein [Microsoft-Organisationskonto](../../active-directory/fundamentals/sign-up-organization.md) verfügen. Sie können den Zugriff an Einzelpersonen und auch an Benutzergruppen erteilen, die in Azure Active Directory definiert sind.

#### <a name="navigate-to-resource-group-or-directly-to-the-resource-itself"></a>Navigieren zur Ressourcengruppe oder direkt zur Ressource

Wählen Sie im linken Menü die Option **Zugriffssteuerung (IAM)** aus.

![Screenshot der Schaltfläche „Zugriffssteuerung“ im Azure-Portal](./media/resources-roles-access-control/0001-access-control.png)

Wählen Sie **Rollenzuweisung hinzufügen** aus.

![Screenshot des Menüs „Zugriffssteuerung“ mit rot hervorgehobener Schaltfläche „Hinzufügen“](./media/resources-roles-access-control/0002-add.png)

Die Ansicht **Berechtigungen hinzufügen** unten bezieht sich in erster Linie auf Application Insights-Ressourcen. Wenn Sie die Zugriffssteuerungsberechtigungen einer höheren Ebene wie Ressourcengruppen anzeigen, sehen Sie zusätzliche, nicht Application Insights-bezogene Rollen.

Wenn Sie Informationen zu allen integrierten Rollen für die rollenbasierte Zugriffssteuerung in Azure anzeigen möchten, nutzen Sie die [offiziellen Referenzinhalte](../../role-based-access-control/built-in-roles.md).

![Screenshot einer Liste der Benutzerrollen für die Zugriffssteuerung](./media/resources-roles-access-control/0003-user-roles.png)

#### <a name="select-a-role"></a>Auswählen einer Rolle

Wir haben Links zur zugehörigen offiziellen Referenzdokumentation angegeben (sofern zutreffend).

| Role | In der Ressourcengruppe |
| --- | --- |
| [Besitzer](../../role-based-access-control/built-in-roles.md#owner) |Kann alle Elemente ändern, einschließlich des Benutzerzugriffs. |
| [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) |Kann alle Elemente bearbeiten, einschließlich aller Ressourcen. |
| [Mitwirkender der Application Insights-Komponente](../../role-based-access-control/built-in-roles.md#application-insights-component-contributor) |Kann Application Insights-Ressourcen bearbeiten. |
| [Leser](../../role-based-access-control/built-in-roles.md#reader) |Kann Inhalte anzeigen, aber nicht ändern. |
| [Application Insights-Momentaufnahmedebugger](../../role-based-access-control/built-in-roles.md#application-insights-snapshot-debugger) | Erteilt dem Benutzer die Berechtigung zum Verwenden von Features des Application Insights-Momentaufnahmedebuggers. Beachten Sie, dass diese Rolle weder in der Rolle „Besitzer“ noch in der Rolle „Mitwirkender“ enthalten ist. |
| Mitwirkender der Releaseverwaltung für Azure App Service-Bereitstellung | Rolle „Mitwirkender“ für Dienste, die über Azure App Service-Bereitstellung bereitgestellt werden. |
| [Datenpurger](../../role-based-access-control/built-in-roles.md#data-purger) | Besondere Rolle für das Löschen von personenbezogenen Daten. Weitere Informationen finden Sie in unserem [Leitfaden für personenbezogene Daten](../platform/personal-data-mgmt.md).   |
| ExpressRoute-Administrator | Kann Expressrouten erstellen, löschen und verwalten.|
| [Log Analytics-Mitwirkender](../../role-based-access-control/built-in-roles.md#log-analytics-contributor) | Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Erstellen und Konfigurieren von Automation-Konten, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen.  |
| [Log Analytics-Leser](../../role-based-access-control/built-in-roles.md#log-analytics-reader) | Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. |
| masterreader | Ermöglicht einem Benutzer, alle Inhalte anzuzeigen, aber keine Änderungen vorzunehmen. |
| [Überwachungsmitwirkender](../../role-based-access-control/built-in-roles.md#monitoring-contributor) | Kann alle Überwachungsdaten lesen und Überwachungseinstellungen aktualisieren.|
| [Herausgeber von Überwachungsmetriken](../../role-based-access-control/built-in-roles.md#monitoring-metrics-publisher) | Ermöglicht die Veröffentlichung von Metriken für Azure-Ressourcen. |
| [Überwachungsleser](../../role-based-access-control/built-in-roles.md#monitoring-reader) | Kann alle Überwachungsdaten lesen. |
| Mitwirkender an Ressourcenrichtlinien (Vorschau) | Über EA abgeglichene Benutzer mit Rechten zum Erstellen/Ändern der Ressourcenrichtlinie, zum Erstellen eines Supporttickets und zum Lesen von Ressourcen/der Hierarchie.  |
| [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) | Ermöglicht einem Benutzer, den Zugriff für andere Benutzer auf Azure-Ressourcen zu verwalten.|
| [Mitwirkender von Website](../../role-based-access-control/built-in-roles.md#website-contributor) | Ermöglicht Ihnen das Verwalten von Websites (nicht der Webpläne), nicht aber den Zugriff darauf.|

Zum "Bearbeiten" gehört das Erstellen, Löschen und Aktualisieren von:

* Ressourcen
* Webtests
* Alerts
* Fortlaufendem Export

#### <a name="select-the-user"></a>Auswählen des Benutzers

Wenn der gewünschte Benutzer nicht im Verzeichnis enthalten ist, können Sie jeden Benutzer mit einem Microsoft-Konto einladen.
(Wenn Benutzer Dienste wie Outlook.com, OneDrive, Windows Phone oder XBox Live verwenden, besitzen sie ein Microsoft-Konto.)

## <a name="related-content"></a>Verwandte Inhalte

* [Rollenbasierte Zugriffssteuerung von Azure (Azure-RBAC)](../../role-based-access-control/role-assignments-portal.md)

## <a name="powershell-query-to-determine-role-membership"></a>PowerShell-Abfrage zum Bestimmen der Rollenmitgliedschaft

Da bestimmte Rollen mit Benachrichtigungen und E-Mail-Warnungen verknüpft sein können, kann es hilfreich sein, eine Liste von Benutzern zu generieren, die einer bestimmten Rolle angehören. Als Hilfe zum Generieren dieser Listen bieten wir die folgenden Beispielabfragen, die entsprechend Ihren speziellen Anforderungen angepasst werden können:

### <a name="query-entire-subscription-for-admin-roles--contributor-roles"></a>Abfrage des gesamten Abonnements nach den Rollen „Administrator“ und „Mitwirkender“

```powershell
(Get-AzRoleAssignment -IncludeClassicAdministrators | Where-Object {$_.RoleDefinitionName -in @('ServiceAdministrator', 'CoAdministrator', 'Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-application-insights-resource-for-owners-and-contributors"></a>Abfrage innerhalb des Kontexts einer bestimmten Application Insights-Ressource für Besitzer und Mitwirkende

```powershell
$resourceGroup = "RGNAME"
$resourceName = "AppInsightsName"
$resourceType = "microsoft.insights/components"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup -ResourceType $resourceType -ResourceName $resourceName | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-resource-group-for-owners-and-contributors"></a>Abfrage innerhalb des Kontexts einer bestimmten Ressourcengruppe für Besitzer und Mitwirkende

```powershell
$resourceGroup = "RGNAME"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../../azure-resource-manager/management/overview.md
[portal]: https://portal.azure.com/
[start]: ./app-insights-overview.md
