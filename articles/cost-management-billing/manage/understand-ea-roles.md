---
title: Informationen zu den Administratorrollen für Enterprise in Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über Enterprise-Administratorrollen in Azure.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: banders
ms.openlocfilehash: 9fab4d8ba0cf2e6f684a1b9de177084f8ce31604
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462189"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure

Azure-Kunden mit einem Enterprise Agreement (EA) können zur Verwaltung ihrer Organisation und ihrer Ausgaben fünf verschiedene Administratorrollen zuweisen:

- Unternehmensadministrator
- Unternehmensadministrator (nur Leseberechtigung)<sup>1</sup>
- Abteilungsadministrator
- Abteilungsadministrator (nur Leseberechtigung)
- Kontobesitzer

<sup>1</sup> Der Rechnungsempfänger des Konzernvertrags hat diese Rolle.

Diese Rollen sind spezifisch für die Verwaltung von Azure Enterprise Agreements und ergänzend zu den integrierten Rollen in Azure zum Steuern des Zugriffs auf Ressourcen. Weitere Informationen finden Sie unter [Integrierte Rollen für Azure-Ressourcen](../../role-based-access-control/built-in-roles.md).

Die folgenden Abschnitte beschreiben die Einschränkungen und Funktionen der einzelnen Rollen.

## <a name="user-limit-for-admin-roles"></a>Benutzerlimit für Administratorrollen

|Role| Benutzerlimit|
|---|---|
|Unternehmensadministrator|Unbegrenzt|
|Unternehmensadministrator (nur Leseberechtigung)|Unbegrenzt|
|Abteilungsadministrator|Unbegrenzt|
|Abteilungsadministrator (nur Leseberechtigung)|Unbegrenzt|
|Kontobesitzer|1 pro Konto<sup>2</sup>|

<sup>2</sup> Für jedes Konto ist ein eindeutiges Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto erforderlich.

## <a name="organization-structure-and-permissions-by-role"></a>Organisationsstruktur und Berechtigungen nach Rolle

|Aufgaben| Unternehmensadministrator|Unternehmensadministrator (nur Leseberechtigung)|Abteilungsadministrator|Abteilungsadministrator (nur Leseberechtigung)|Kontobesitzer|
|---|---|---|---|---|---|
|Anzeigen von Unternehmensadministratoren|✔|✔|✘|✘|✘|
|Hinzufügen und Entfernen von Unternehmensadministratoren|✔|✘|✘|✘|✘|
|Anzeigen von Benachrichtigungskontakten<sup>3</sup> |✔|✔|✘|✘|✘|
|Hinzufügen und Entfernen von Benachrichtigungskontakten<sup>3</sup> |✔|✘|✘|✘|✘|
|Erstellen und Verwalten von Abteilungen |✔|✘|✘|✘|✘|
|Anzeigen von Abteilungsadministratoren|✔|✔|✔|✔|✘|
|Hinzufügen und Entfernen von Abteilungsadministratoren|✔|✘|✔|✘|✘|
|Anzeigen von Konten in der Registrierung |✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✘|
|Hinzufügen von zur Registrierung und Ändern des Kontobesitzers|✔|✘|✔<sup>4</sup>|✘|✘|
|Erstellen und Verwalten von Abonnements und Abonnementberechtigungen|✘|✘|✘|✘|✔|

- <sup>3</sup> Benachrichtigungskontakte erhalten E-Mail-Benachrichtigungen über das Azure Enterprise Agreement.
- <sup>4</sup> Die Aufgabe ist auf Konten in Ihrer Abteilung beschränkt.


## <a name="usage-and-costs-access-by-role"></a>Zugriff auf Nutzung und Kosten nach Rolle

|Aufgaben| Unternehmensadministrator|Unternehmensadministrator (nur Leseberechtigung)|Abteilungsadministrator|Abteilungsadministrator (nur Leseberechtigung) |Kontobesitzer|
|---|---|---|---|---|---|
|Anzeigen des Guthabens einschließlich finanzieller Verpflichtung|✔|✔|✘|✘|✘|
|Anzeigen von Ausgabenkontingenten der Abteilungen|✔|✔|✘|✘|✘|
|Festlegen von Ausgabenkontingenten der Abteilungen|✔|✘|✘|✘|✘|
|Anzeigen der EA-Preisliste der Organisation|✔|✔|✘|✘|✘|
|Anzeigen von Nutzungs- und Kostendetails|✔|✔|✔<sup>5</sup>|✔<sup>5</sup>|✔<sup>6</sup>|
|Verwalten von Ressourcen im Azure-Portal|✘|✘|✘|✘|✔|

- <sup>5</sup> Erfordert das Aktivieren der Richtlinie **DA view charges** (Gebühren anzeigen für Abteilungsadministrator) im Enterprise Portal durch den Unternehmensadministrator. Der Abteilungsadministrator kann dann Kostendetails für die Abteilung einsehen.
- <sup>6</sup> Erfordert das Aktivieren der Richtlinie **AO view charges** (Gebühren anzeigen für Kontobesitzer) im Enterprise Portal durch den Unternehmensadministrator. Der Kontobesitzer kann dann Kostendetails für das Konto einsehen.


## <a name="pricing-in-azure-portal"></a>Preise im Azure-Portal

Möglicherweise werden Ihnen im Azure-Portal abhängig von Ihrer administrativen Rolle und der Festlegung der Richtlinien zum Anzeigen von Gebühren durch den Unternehmensadministrator unterschiedliche Gebühren angezeigt. Die beiden folgenden Richtlinien im Enterprise-Portal haben Auswirkungen auf die im Azure-Portal angezeigten Preise:

- DA view charges (Gebühren anzeigen für Abteilungsadministratoren)
- AO view charges (Gebühren anzeigen für Kontobesitzer)

Informationen zum Festlegen dieser Richtlinien finden Sie unter [Verwalten des Zugriffs auf Abrechnungsinformationen für Azure](manage-billing-access.md).

Die folgende Tabelle zeigt die Beziehungen zwischen den Enterprise Agreement-Administratorrollen, den Richtlinien zum Anzeigen von Gebühren, der Rolle für rollenbasierte Zugriffssteuerung (RBAC) im Azure-Portal und den im Azure-Portal angezeigten Preisen. Dem Unternehmensadministrator werden Nutzungsdetails immer basierend auf den EA-Preisen der Organisation angezeigt. Dem Abteilungsadministrator und dem Kontobesitzer werden jedoch unterschiedliche Preise angezeigt, entsprechend der Richtlinie zum Anzeigen von Gebühren und ihrer RBAC-Rolle. Die in der folgenden Tabelle aufgeführte Rolle des Abteilungsadministrators bezieht sich sowohl auf die Rolle „Abteilungsadministrator“ als auch die auf die Rolle „Abteilungsadministrator (nur Leseberechtigung)“.

|Enterprise Agreement-Administratorrolle|Richtlinie zum Anzeigen von Gebühren für die Rolle|RBAC-Rolle|Preisansicht|
|---|---|---|---|
|Kontobesitzer ODER Abteilungsadministrator|✔ Aktiviert|Besitzer|EA-Preise der Organisation|
|Kontobesitzer ODER Abteilungsadministrator|✘ Deaktiviert|Besitzer|Einzelhandelspreise|
|Kontobesitzer ODER Abteilungsadministrator|✔ Aktiviert |none|Keine Preise|
|Kontobesitzer ODER Abteilungsadministrator|✘ Deaktiviert |none|Keine Preise|
|Keine|Nicht verfügbar |Besitzer|Einzelhandelspreise|

Sie legen die Enterprise-Administratorrolle und die Richtlinien zum Anzeigen von Gebühren im Enterprise-Portal fest. Die RBAC-Rolle kann im Azure-Portal aktualisiert werden. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mithilfe von RBAC und des Azure-Portals](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten des Zugriffs auf Abrechnungsinformationen für Azure](manage-billing-access.md)
- [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../../role-based-access-control/role-assignments-portal.md)
- [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/built-in-roles.md)
