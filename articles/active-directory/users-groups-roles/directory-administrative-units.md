---
title: Verwalten von Verwaltungseinheiten (Vorschau) – Azure AD | Microsoft-Dokumentation
description: Verwenden von Verwaltungseinheiten zum präziseren Delegieren von Berechtigungen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 04/16/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44a01bb7d2f4aa3d31204d6235e955e82e471d5d
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84729046"
---
# <a name="administrative-units-management-in-azure-active-directory-preview"></a>Verwalten von Verwaltungseinheiten in Azure Active Directory (Vorschau)

In diesem Artikel werden Verwaltungseinheiten in Azure Active Directory (Azure AD) beschrieben. Eine Verwaltungseinheit ist eine Azure AD Ressource, bei der es sich um einen Container für andere Azure AD Ressourcen handeln kann. In dieser Vorschauversion kann eine Verwaltungseinheit nur Benutzer und Gruppen enthalten.

Verwaltungseinheiten ermöglichen es Ihnen, Administratorberechtigungen zu erteilen, die auf eine von Ihnen definierte Abteilung, Region oder ein anderes Segment Ihrer Organisation beschränkt sind. Mithilfe von Verwaltungseinheiten können Sie Berechtigungen an regionale Administratoren delegieren oder Richtlinien auf granularer Ebene festlegen. Beispielsweise kann ein Benutzerkontoadministrator nur in seiner Verwaltungseinheit Profilinformationen aktualisieren, Kennwörter zurücksetzen und Benutzern Lizenzen zuweisen.

 Beispielsweise ist die [Helpdeskadministrator](directory-assign-admin-roles.md#helpdesk-administrator)-Rolle beim Delegieren an regionale Supportspezialisten auf die Verwaltung nur der Benutzer beschränkt, die sich in der von ihnen unterstützten Region befinden.

## <a name="deployment-scenario"></a>Bereitstellungsszenario

Das Einschränken des Verwaltungsbereichs mithilfe von Verwaltungseinheiten kann in Organisationen nützlich sein, die aus unabhängigen Abteilungen jeglicher Art bestehen. Nehmen Sie als Beispiel eine große Universität, die aus vielen autonomen Fakultäten (Wirtschaftswissenschaften, Ingenieurwissenschaften usw.) besteht, die jeweils über ein Team von IT-Administratoren verfügen, die den Zugriff steuern, Benutzer verwalten und Richtlinien für ihre Fakultät festlegen. Ein zentraler Administrator könnte folgende Aktionen ausführen:

- Erstellen einer Rolle mit Administratorberechtigungen nur für Azure AD-Benutzer in der Verwaltungseinheit „Wirtschaftswissenschaften“
- Erstellen einer Verwaltungseinheit für die Fakultät „Wirtschaftswissenschaften“
- Auffüllen der Verwaltungseinheit nur mit den Studenten und Mitarbeitern der Fakultät „Wirtschaftswissenschaften“
- Hinzufügen des IT-Teams der Fakultät „Wirtschaftswissenschaften“ zur Rolle mit dem entsprechenden Bereich

## <a name="license-requirements"></a>Lizenzanforderungen

Bei der Verwendung von Verwaltungseinheiten ist für jeden Administrator einer Verwaltungseinheit eine Azure Active Directory Premium-Lizenz erforderlich, und alle Mitglieder der Verwaltungseinheit benötigen Azure Active Directory Free-Lizenzen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="manage-administrative-units"></a>Verwalten von Verwaltungseinheiten

In dieser Vorschauversion können Sie Verwaltungseinheiten im Azure-Portal, mithilfe von PowerShell-Cmdlets oder mit Microsoft Graph verwalten. Ausführliche Informationen finden Sie in unserer Dokumentation:

- [Erstellen, Entfernen, Auffüllen und Hinzufügen von Rollen zu Verwaltungseinheiten](roles-admin-units-manage.md): Umfassende schrittweise Anleitungen
- [Arbeiten mit Verwaltungseinheiten](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0): Arbeiten mit Verwaltungseinheiten mithilfe von PowerShell
- [Unterstützung von Verwaltungseinheiten in Microsoft Graph](https://docs.microsoft.com/graph/api/resources/administrativeunit?view=graph-rest-beta): Ausführliche Dokumentation zu Verwaltungseinheiten in Microsoft Graph.

### <a name="planning-your-administrative-units"></a>Planen Ihrer Verwaltungseinheiten

Verwaltungseinheiten können zum logischen Gruppieren von Azure AD-Ressourcen verwendet werden. Beispielsweise kann es für eine Organisation mit einer global verteilten IT-Abteilung sinnvoll sein, Verwaltungseinheiten zu erstellen, die diese geografischen Begrenzungen definieren. In einem anderen Szenario, in dem eine multinationale Organisation über verschiedene „Unterorganisationen“ verfügt, die in ihrem Betrieb halbautonom sind, kann jede Unterorganisation durch eine Verwaltungseinheit dargestellt werden.

Die Kriterien für die Erstellung von Verwaltungseinheiten richten sich nach den besonderen Anforderungen eines Unternehmens. Verwaltungseinheiten sind eine gängige Methode, um die Struktur übergreifend über M365-Dienste hinweg zu definieren. Wir empfehlen, bei der Vorbereitung der Verwaltungseinheiten deren Verwendung über M365-Dienste hinweg im Blick zu haben. Um Verwaltungseinheiten optimal zu nutzen, könnten Sie übergreifend über M365-Dienste gemeinsame Ressourcen in einer Verwaltungseinheit verknüpfen.

Sie können davon ausgehen, dass die Erstellung von Verwaltungseinheiten in der Organisation die folgenden Phasen durchläuft:

1. Anfängliche Einführung: Ihre Organisation beginnt anhand anfänglicher Kriterien mit der Erstellung von Verwaltungseinheiten. Nach der Verfeinerung der Kriterien nimmt die Anzahl der Verwaltungseinheiten zu.
1. Bereinigung: Wenn die Kriterien optimal definiert sind, werden nicht mehr benötigte Verwaltungseinheiten gelöscht.
1. Stabilisierung: Die Struktur Ihrer Organisation ist optimal definiert, und die Anzahl der Verwaltungseinheiten wird sich über kürzere Zeiträume nicht erheblich ändern.

## <a name="currently-supported-scenarios"></a>Derzeit unterstützte Szenarien

Globale Administratoren oder Administratoren für privilegierte Rollen können das Azure AD-Portal zum Erstellen von Verwaltungseinheiten, zum Hinzufügen von Benutzern als Mitglieder von Verwaltungseinheiten und zum anschließenden Zuweisen von IT-Mitarbeitern zu auf die Verwaltungseinheit bezogenen Administratorrollen verwenden. Die bereichsbezogenen Administratoren können dann das Office 365-Portal für die grundlegende Verwaltung von Benutzern in ihren Verwaltungseinheiten verwenden.

Auch Gruppen können der Verwaltungseinheit als Mitglieder hinzugefügt werden. Ein auf den Bereich der Verwaltungseinheit bezogener Gruppenadministrator kann diese mithilfe von PowerShell und Microsoft Graph oder im Azure AD-Portal verwalten.

In der folgenden Tabelle wird die aktuelle Unterstützung für Szenarien mit Verwaltungseinheiten beschrieben.

### <a name="administrative-unit-management"></a>Verwalten von Verwaltungseinheiten

Berechtigungen |   MS Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center
----------- | ----------------------- | --------------- | -----------------
Erstellen und Löschen von Verwaltungseinheiten   |    Unterstützt    |   Unterstützt   |    Nicht unterstützt
Hinzufügen und Entfernen einzelner Mitglieder zu Verwaltungseinheiten    |   Unterstützt    |   Unterstützt   |    Nicht unterstützt
Massenvorgang zum Hinzufügen und Entfernen von Mitgliedern zu/von Verwaltungseinheiten mithilfe einer CSV-Datei   |    Nicht unterstützt     |  Unterstützt   |    Keine Unterstützung geplant
Zuweisen von Administratoren, die auf den Bereich der Verwaltungseinheit bezogen sind  |     Unterstützt    |   Unterstützt    |   Nicht unterstützt
Dynamisches Hinzufügen und Entfernen von Mitgliedern zu/von Verwaltungseinheiten anhand von Attributen | Nicht unterstützt | Nicht unterstützt | Nicht unterstützt

### <a name="user-management"></a>Benutzerverwaltung

Berechtigungen |   MS Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center
----------- | ----------------------- | --------------- | -----------------
Bereichs der Verwaltungseinheit bezogene Verwaltung von Benutzereigenschaften, Kennwörtern und Lizenzen   |    Unterstützt     |  Unterstützt   |   Unterstützt
Auf den Bereich der Verwaltungseinheit bezogene Blockierung und Aufhebung der Blockierung von Benutzeranmeldungen    |   Unterstützt   |    Unterstützt   |    Unterstützt
Auf den Bereich der Verwaltungseinheit bezogene Verwaltung von MFA-Benutzeranmeldeinformationen   |    Unterstützt   |   Unterstützt   |   Nicht unterstützt

### <a name="group-management"></a>Gruppenverwaltung

Berechtigungen |   MS Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center
----------- | ----------------------- | --------------- | -----------------
Auf den Bereich der Verwaltungseinheit bezogene Verwaltung von Gruppeneigenschaften und Gruppenmitgliedern     |  Unterstützt   |    Unterstützt    |  Nicht unterstützt
Auf den Bereich der Verwaltungseinheit bezogene Verwaltung der Gruppenlizenzierung   |    Unterstützt  |    Unterstützt   |   Nicht unterstützt

> [!NOTE]
>
> Auf den Bereich der Verwaltungseinheit bezogene Administratoren können keine Regeln für dynamische Gruppenmitgliedschaften verwalten.

Der mithilfe von Verwaltungseinheiten angewendete Bereich gilt nur für Verwaltungsberechtigungen. Verwaltungseinheiten verhindern nicht, dass Mitglieder oder Administratoren ihre [Standardbenutzerberechtigungen](../fundamentals/users-default-permissions.md) verwenden, um zu anderen Benutzern, Gruppen oder Ressourcen außerhalb der Verwaltungseinheit zu navigieren. Im Office 365-Portal werden Benutzer außerhalb der Verwaltungseinheiten eines bereichsbezogenen Administrators zwar herausgefiltert, können aber zu anderen Benutzern im Azure AD-Portal, in PowerShell und in anderen Microsoft-Diensten navigieren.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Verwaltungseinheiten](roles-admin-units-manage.md)
- [Verwalten von Benutzern in Verwaltungseinheiten](roles-admin-units-add-manage-users.md)
- [Verwalten von Gruppen in Verwaltungseinheiten](roles-admin-units-add-manage-groups.md)
- [Zuweisen von bereichsbezogenen Rollen zu einer Verwaltungseinheit](roles-admin-units-assign-roles.md)