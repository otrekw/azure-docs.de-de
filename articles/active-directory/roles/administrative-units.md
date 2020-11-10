---
title: Verwaltungseinheiten in Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Berechtigungen in Azure Active Directory mithilfe von Verwaltungseinheiten präziser delegieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 09/22/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0fb24d0cd2714969b5a888b1036f524c4c062d76
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2020
ms.locfileid: "93027617"
---
# <a name="administrative-units-in-azure-active-directory"></a>Verwaltungseinheiten in Azure Active Directory

In diesem Artikel werden Verwaltungseinheiten in Azure Active Directory (Azure AD) beschrieben. Eine Verwaltungseinheit ist eine Azure AD Ressource, bei der es sich um einen Container für andere Azure AD Ressourcen handeln kann. Eine Verwaltungseinheit kann nur Benutzer und Gruppen enthalten.

Sie können Verwaltungseinheiten verwenden, um Administratorberechtigungen zu erteilen, die auf eine von Ihnen definierte Abteilung, Region oder ein anderes Segment Ihrer Organisation beschränkt sind. Mithilfe von Verwaltungseinheiten können Sie Berechtigungen an regionale Administratoren delegieren oder Richtlinien auf granularer Ebene festlegen. So können Sie beispielsweise sicherstellen, dass ein Benutzerkontoadministrator nur in seiner Verwaltungseinheit Profilinformationen aktualisieren, Kennwörter zurücksetzen und Benutzern Lizenzen zuweisen kann.

Ein weiteres Beispiel ist das Delegieren der Rolle [Helpdeskadministrator](permissions-reference.md#helpdesk-administrator) an regionale Supportspezialisten, wobei die Rolle auf die Verwaltung von Benutzer beschränkt ist, die sich in der von ihnen unterstützten Region befinden.

## <a name="deployment-scenario"></a>Bereitstellungsszenario

Das Einschränken des Verwaltungsbereichs mithilfe von Verwaltungseinheiten kann in Organisationen hilfreich sein, die aus beliebigen unabhängigen Abteilungen bestehen. Stellen Sie sich beispielsweise eine große Universität vor, die sich aus vielen eigenständigen Fakultäten (Wirtschaftswissenschaften, Technik usw.) zusammensetzt. Die einzelnen Fakultäten verfügen jeweils über ein Team von IT-Administratoren, die den Zugriff steuern, Benutzer verwalten und Richtlinien für ihre Fakultät festlegen. 

Ein zentraler Administrator könnte folgende Aktionen ausführen:

- Erstellen einer Rolle mit Administratorberechtigungen nur für Azure AD-Benutzer in der Verwaltungseinheit „Wirtschaftswissenschaften“
- Erstellen einer Verwaltungseinheit für die Fakultät „Wirtschaftswissenschaften“
- Auffüllen der Verwaltungseinheit nur mit den Studenten und Mitarbeitern der Fakultät „Wirtschaftswissenschaften“
- Hinzufügen des IT-Teams der Fakultät „Wirtschaftswissenschaften“ zu der Rolle, zusammen mit dem zugehörigen Bereich

## <a name="license-requirements"></a>Lizenzanforderungen

Wenn Sie Verwaltungseinheiten verwenden möchten, ist für jeden Administrator einer Verwaltungseinheit eine Azure Active Directory Premium-Lizenz erforderlich, und alle Mitglieder der Verwaltungseinheit benötigen Azure Active Directory Free-Lizenzen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="manage-administrative-units"></a>Verwalten von Verwaltungseinheiten

Sie können Verwaltungseinheiten im Azure-Portal, mithilfe von PowerShell-Cmdlets und -Skripts oder mit Microsoft Graph verwalten. Weitere Informationen finden Sie unter

- [Erstellen, Entfernen, Auffüllen und Hinzufügen von Rollen zu Verwaltungseinheiten](admin-units-manage.md): Enthält umfassende schrittweise Anleitungen.
- [Verwenden von Verwaltungseinheiten:](/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0&preserve-view=true) Enthält Informationen zur Verwendung von Verwaltungseinheiten über PowerShell.
- [Ressourcentyp „administrativeUnit“:](/graph/api/resources/administrativeunit?view=graph-rest-1.0&preserve-view=true) Enthält eine ausführliche Dokumentation zu Microsoft Graph für Verwaltungseinheiten.

### <a name="plan-your-administrative-units"></a>Planen Ihrer Verwaltungseinheiten

Verwaltungseinheiten können zur logischen Gruppierung von Azure AD-Ressourcen verwendet werden. Beispielsweise kann es für eine Organisation mit einer global verteilten IT-Abteilung sinnvoll sein, Verwaltungseinheiten zu erstellen, die diese geografischen Begrenzungen definieren. In einem anderen Szenario, in dem eine multinationale Organisation über verschiedene *Unterorganisationen* verfügt, die in ihrem Betrieb halbautonom sind, kann eine Verwaltungseinheit die einzelnen Unterorganisationen darstellen.

Die Kriterien für die Erstellung von Verwaltungseinheiten richten sich nach den besonderen Anforderungen eines Unternehmens. Verwaltungseinheiten sind eine gängige Methode, um die Struktur in Microsoft 365-Diensten zu definieren. Wir empfehlen, bei der Vorbereitung der Verwaltungseinheiten deren Verwendung über Microsoft 365-Dienste hinweg im Blick zu haben. Um Verwaltungseinheiten optimal zu nutzen, könnten Sie in Microsoft 365-Diensten gemeinsame Ressourcen in einer Verwaltungseinheit verknüpfen.

Sie können davon ausgehen, dass die Erstellung von Verwaltungseinheiten in der Organisation die folgenden Phasen durchläuft:

1. **Anfängliche Einführung:** Ihre Organisation beginnt anhand anfänglicher Kriterien mit der Erstellung von Verwaltungseinheiten. Mit zunehmender Verfeinerung der Kriterien nimmt dann auch die Anzahl von Verwaltungseinheiten zu.
1. **Bereinigung:** Nachdem die Kriterien optimal definiert wurden, werden nicht mehr benötigte Verwaltungseinheiten gelöscht.
1. **Stabilisierung:** Die Struktur Ihrer Organisation ist optimal definiert, und die Anzahl von Verwaltungseinheiten wird sich auf kurze Sicht nicht stark verändern.

## <a name="currently-supported-scenarios"></a>Derzeit unterstützte Szenarien

Globale Administratoren oder Administratoren für privilegierte Rollen können zum Erstellen von Verwaltungseinheiten, zum Hinzufügen von Benutzern als Mitglieder von Verwaltungseinheiten und zum anschließenden Zuweisen von IT-Mitarbeitern zu Administratorrollen, die auf die Verwaltungseinheit ausgerichtet sind, das Azure AD-Portal verwenden. Administratoren für Verwaltungseinheiten können dann Microsoft 365 Admin Center für die grundlegende Verwaltung von Benutzern in ihren Verwaltungseinheiten verwenden.

Darüber hinaus können Sie Gruppen als Mitglieder einer Verwaltungseinheit hinzufügen. Ein auf die Verwaltungseinheit ausgerichteter Gruppenadministrator kann diese mithilfe von PowerShell und Microsoft Graph oder über das Azure AD-Portal verwalten.

In den folgenden Tabellen wird die aktuelle Unterstützung von Szenarien mit Verwaltungseinheiten beschrieben:

### <a name="administrative-unit-management"></a>Verwalten von Verwaltungseinheiten

| Berechtigungen |   Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center | 
| -- | -- | -- | -- |
| Erstellen und Löschen von Verwaltungseinheiten   |    Unterstützt    |   Unterstützt   |    Nicht unterstützt | 
| Hinzufügen und Entfernen einzelner Mitglieder zu Verwaltungseinheiten    |   Unterstützt    |   Unterstützt   |    Nicht unterstützt | 
| Hinzufügen und Entfernen von Verwaltungseinheitsmitgliedern per Massenvorgang unter Verwendung von CSV-Dateien   |    Nicht unterstützt     |  Unterstützt   |    Keine Unterstützung geplant | 
| Zuweisen von Administratoren, die auf den Bereich der Verwaltungseinheit bezogen sind  |     Unterstützt    |   Unterstützt    |   Nicht unterstützt | 
| Dynamisches Hinzufügen und Entfernen von Verwaltungseinheitsmitgliedern auf der Grundlage von Attributen | Nicht unterstützt | Nicht unterstützt | Nicht unterstützt 

### <a name="user-management"></a>Benutzerverwaltung

| Berechtigungen |   Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center |
| -- | -- | -- | -- |
| Auf die Verwaltungseinheit ausgerichtete Verwaltung von Benutzereigenschaften, Kennwörtern und Lizenzen   |    Unterstützt     |  Unterstützt   |   Unterstützt |
| Auf die Verwaltungseinheit ausgerichtete Blockierung von Benutzeranmeldungen sowie Aufhebung der Blockierung    |   Unterstützt   |    Unterstützt   |    Unterstützt |
| Auf die Verwaltungseinheit ausgerichtete Verwaltung von Anmeldeinformationen für die mehrstufige Authentifizierung   |    Unterstützt   |   Unterstützt   |   Nicht unterstützt |

### <a name="group-management"></a>Gruppenverwaltung

| Berechtigungen |   Graph/PowerShell   | Azure AD-Portal | Microsoft 365 Admin Center |
| -- | -- | -- | -- |
| Auf die Verwaltungseinheit ausgerichtete Verwaltung von Gruppeneigenschaften und Gruppenmitgliedern     |  Unterstützt   |    Unterstützt    |  Nicht unterstützt |
| Auf die Verwaltungseinheit ausgerichtete Verwaltung der Gruppenlizenzierung   |    Unterstützt  |    Unterstützt   |   Nicht unterstützt |


Der mithilfe von Verwaltungseinheiten angewendete Bereich gilt nur für Verwaltungsberechtigungen. Verwaltungseinheiten verhindern nicht, dass Mitglieder oder Administratoren ihre [Standardbenutzerberechtigungen](../fundamentals/users-default-permissions.md) verwenden, um zu anderen Benutzern, Gruppen oder Ressourcen außerhalb der Verwaltungseinheit zu navigieren. Im Microsoft 365 Admin Center werden Benutzer außerhalb der Verwaltungseinheiten eines bereichsbezogenen Administrators herausgefiltert. Sie können aber zu anderen Benutzern im Azure AD-Portal, in PowerShell und in anderen Microsoft-Diensten navigieren.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Verwaltungseinheiten](admin-units-manage.md)
- [Hinzufügen und Verwalten von Benutzern in einer Verwaltungseinheit in Azure Active Directory](admin-units-add-manage-users.md)
- [Hinzufügen und Verwalten von Gruppen in Verwaltungseinheiten in Azure Active Directory](admin-units-add-manage-groups.md)
- [Zuweisen von bereichsbezogenen Rollen zu einer Verwaltungseinheit](admin-units-assign-roles.md)
