---
title: Unterstützte Topologien und Szenarien für die Azure AD Connect-Cloudbereitstellung
description: In diesem Artikel werden verschiedene lokale Topologien und Topologien mit Azure Active Directory (Azure AD) beschrieben, in denen die Azure AD Connect-Cloudbereitstellung verwendet wird.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 172e836a212f9ce097c2c4e392a7386a510c2c6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91266291"
---
# <a name="azure-ad-connect-cloud-provisioning-supported-topologies-and-scenarios"></a>Unterstützte Topologien und Szenarien für die Azure AD Connect-Cloudbereitstellung
In diesem Artikel werden verschiedene lokale Topologien und Topologien mit Azure Active Directory (Azure AD) beschrieben, in denen die Azure AD Connect-Cloudbereitstellung verwendet wird. In diesem Artikel werden nur unterstützte Konfigurationen und Szenarien erörtert.

> [!IMPORTANT]
> Eine Änderung oder Nutzung der Azure AD Connect-Cloudbereitstellung außerhalb der formal dokumentierten Konfigurationen oder Aktionen wird von Microsoft nicht unterstützt. Derartige Konfigurationen oder Aktionen können zu einem inkonsistenten oder nicht unterstützten Status der Azure AD Connect-Cloudbereitstellung führen. Folglich kann Microsoft auch keinen technischen Support für solche Bereitstellungen leisten.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Für alle Szenarien und Topologien zu berücksichtigende Aspekte
Im Folgenden finden Sie eine Liste der Informationen, die beim Auswählen einer Lösung zu beachten sind.

- Benutzer und Gruppen müssen in allen Gesamtstrukturen eindeutig identifiziert werden.
- Bei der Cloudbereitstellung erfolgt kein gesamtstrukturübergreifender Abgleich.
- Ein Benutzer oder eine Gruppe darf nur einmal in allen Gesamtstrukturen dargestellt werden.
- Der Quellanker für Objekte wird automatisch ausgewählt.  Er verwendet das „ms-DS-ConsistencyGuid“-Attribut (sofern vorhanden). Andernfalls wird „ObjectGUID“ verwendet.
- Sie können das für den Quellanker verwendete Attribut nicht ändern.

## <a name="single-forest-single-azure-ad-tenant"></a>Einzelne Gesamtstruktur, einzelner Azure AD-Mandant
![Diagramm der Topologie für eine einzelne Gesamtstruktur und einen einzelnen Mandanten.](media/plan-cloud-provisioning-topologies/single-forest.png)

Die einfachste Topologie ist eine einzelne lokale Gesamtstruktur mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten.  Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Eine einzelnen Gesamtstruktur mit einem einzelnen Azure AD-Mandanten](tutorial-single-forest.md).


## <a name="multi-forest-single-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, einzelner Azure AD-Mandant
![Topologie für mehrere Gesamtstrukturen und einen einzelnen Mandanten](media/plan-cloud-provisioning-topologies/multi-forest.png)

Eine häufig verwendete Topologie umfasst mehrere AD-Gesamtstrukturen mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Vorhandene Gesamtstruktur mit Azure AD Connect, neue Gesamtstruktur mit Cloudbereitstellung
![Diagramm der Topologie für eine vorhandene Gesamtstruktur und eine neue Gesamtstruktur.](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Dieses Topologieszenario ist mit dem Szenario mit mehreren Gesamtstrukturen vergleichbar. Hier geht es jedoch um eine vorhandene Azure AD Connect-Umgebung und das Hinzufügen einer neuen Gesamtstruktur mithilfe der Azure AD Connect-Cloudbereitstellung.  Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Eine vorhandene Gesamtstruktur mit einem einzelnen Azure AD-Mandanten](tutorial-existing-forest.md).

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Testen der Azure AD Connect-Cloudbereitstellung in einer vorhandenen Hybrid-AD-Gesamtstruktur
![Topologie für eine einzelne Gesamtstruktur und einen einzelnen Mandanten](media/plan-cloud-provisioning-topologies/migrate.png) Im Pilottestszenario sind sowohl Azure AD Connect als auch die Azure AD Connect-Cloudbereitstellung in derselben Gesamtstruktur vorhanden, und die Benutzer und Gruppen werden entsprechend festgelegt. HINWEIS:  Ein Objekt darf sich nur in einem der Tools im Gültigkeitsbereich befinden. 

Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Azure AD Connect-Pilotcloudbereitstellung für eine vorhandene synchronisierte AD-Gesamtstruktur](tutorial-pilot-aadc-aadccp.md).



## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)

