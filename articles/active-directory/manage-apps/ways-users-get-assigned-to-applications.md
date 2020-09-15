---
title: Informationen dazu, wie Benutzer Apps in Azure Active Directory zugewiesen werden
description: Hier erfahren Sie, wie Benutzer einer App zugewiesen werden, die Azure Active Directory für die Identitätsverwaltung verwendet.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: kenwith
ms.openlocfilehash: 243e5ece0d0a14cb7e3ade409ee68510cef64a9c
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89397099"
---
# <a name="understand-how-users-are-assigned-to-apps-in-azure-active-directory"></a>Informationen dazu, wie Benutzer Apps in Azure Active Directory zugewiesen werden
In diesem Artikel erhalten Sie Informationen zum Zuweisen von Benutzern zu einer Anwendung in Ihrem Mandanten.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Wie werden Benutzer einer Anwendung in Azure AD zugewiesen?
Damit Benutzer Zugriff auf eine Anwendung haben, müssen sie dieser zunächst zugewiesen werden. Die Zuweisung kann durch einen Administrator, einen Vertreter des Unternehmens oder manchmal auch durch den Benutzer selbst erfolgen. Nachfolgend sind die verschiedenen Möglichkeiten aufgeführt, wie Benutzer Anwendungen zugewiesen werden können:

*  Ein Administrator [weist einen Benutzer](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) direkt der Anwendung zu.
*  Ein Administrator [weist eine Gruppe](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal), der der Benutzer als Mitglied angehört, der Anwendung zu. Dies kann die folgenden Gruppen umfassen:
    * Eine Gruppe, die lokal synchronisiert wurde
    * Eine in der Cloud erstellte statische Sicherheitsgruppe
    * Eine in der Cloud erstellte [dynamische Sicherheitsgruppe](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal)
    * Eine in der Cloud erstellte Office 365-Gruppe
    * Die Gruppe [Alle Benutzer](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups)
*  Ein Administrator aktiviert den [Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access), damit ein Benutzer eine Anwendung mithilfe der Funktion **App hinzufügen** in [Meine Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **ohne Genehmigung des Unternehmens** hinzufügen kann.
*  Ein Administrator aktiviert den [Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access), damit ein Benutzer eine Anwendung mithilfe der Funktion **App hinzufügen** in [Meine Apps](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) hinzufügen kann, jedoch nur **mit vorheriger Genehmigung durch festgelegte genehmigende Personen des Unternehmens**.
*  Ein Administrator aktiviert die [Self-Service-Gruppenverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), damit ein Benutzer einer Gruppe, der eine Anwendung zugewiesen ist, **ohne Genehmigung des Unternehmens** beitreten kann.
*  Ein Administrator aktiviert die [Self-Service-Gruppenverwaltung](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), damit ein Benutzer einer Gruppe, der eine Anwendung zugewiesen ist, beitreten kann, jedoch nur **mit vorheriger Genehmigung durch festgelegte genehmigende Personen des Unternehmens**.
*  Ein Administrator weist einem Benutzer direkt eine Lizenz für eine Erstanbieteranwendung zu, z.B. für [Microsoft Office 365](https://products.office.com/).
*  Ein Administrator weist einer Gruppe, der der Benutzer als Mitglied angehört, eine Lizenz für eine Erstanbieteranwendung zu, z.B. für [Microsoft Office 365](https://products.office.com/).
*  Ein [Administrator gibt seine Zustimmung für eine Anwendung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview), sodass diese von allen Benutzern verwendet werden kann. Ein Benutzer meldet sich dann bei der Anwendung an.
* Ein Benutzer [gibt selbst seine Zustimmung zu einer Anwendung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview), indem er sich bei der Anwendung anmeldet.

## <a name="next-steps"></a>Nächste Schritte
* [Schnellstartserie zur Anwendungsverwaltung](view-applications-portal.md)
* [Worum handelt es sich bei der Anwendungsverwaltung?](what-is-application-management.md)
* [Worum handelt es sich beim einmaligen Anmelden?](what-is-single-sign-on.md)
