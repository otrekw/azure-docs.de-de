---
title: Unterstützte Topologien und Szenarien für die Azure AD Connect-Cloudbereitstellung
description: In diesem Thema werden die Voraussetzungen und die Hardwareanforderungen für die Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 142974423816b07d754a5425017aedc3195e2f4e
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795377"
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



## <a name="multi-forest-single-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, einzelner Azure AD-Mandant
![Topologie für mehrere Gesamtstrukturen und einen einzelnen Mandanten](media/plan-cloud-provisioning-topologies/multi-forest.png)

Die am häufigsten verwendete Topologie umfasst mehrere AD-Gesamtstrukturen mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Vorhandene Gesamtstruktur mit Azure AD Connect, neue Gesamtstruktur mit Cloudbereitstellung
![Topologie für einzelne Gesamtstruktur und einzelnen Mandanten](media/plan-cloud-provisioning-topologies/existing-forest-new-forest.png)

Dieses Topologieszenario ist mit dem Szenario mit mehreren Gesamtstrukturen vergleichbar. Hier geht es jedoch um eine vorhandene Azure AD Connect-Umgebung und das Hinzufügen einer neuen Gesamtstruktur mithilfe der Azure AD Connect-Cloudbereitstellung.  Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Integrieren einer vorhandenen Gesamtstruktur in eine neue Gesamtstruktur mit einem einzelnen Azure AD-Mandanten](tutorial-existing-forest.md).

## <a name="piloting-azure-ad-connect-cloud-provisioning-in-an-existing-hybrid-ad-forest"></a>Testen der Azure AD Connect-Cloudbereitstellung in einer vorhandenen Hybrid-AD-Gesamtstruktur
![Topologie für eine einzelne Gesamtstruktur und einen einzelnen Mandanten](media/plan-cloud-provisioning-topologies/migrate.png) Im Pilottestszenario sind sowohl Azure AD Connect als auch die Azure AD Connect-Cloudbereitstellung in derselben Gesamtstruktur vorhanden, und die Benutzer und Gruppen werden entsprechend festgelegt. HINWEIS:  Ein Objekt darf sich nur in einem der Tools im Gültigkeitsbereich befinden. 

Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Azure AD Connect-Pilotcloudbereitstellung für eine vorhandene synchronisierte AD-Gesamtstruktur](tutorial-pilot-aadc-aadccp.md).

## <a name="single-forest-single-azure-ad-tenant"></a>Einzelne Gesamtstruktur, einzelner Azure AD-Mandant
![Topologie für einzelne Gesamtstruktur und einzelnen Mandanten](media/plan-cloud-provisioning-topologies/single-forest.png)

Die einfachste Topologie ist eine einzelne lokale Gesamtstruktur mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten.  Ein Beispiel für dieses Szenario finden Sie unter [Tutorial: Integrieren einer einzelnen Gesamtstruktur mit einem einzelnen Azure AD-Mandanten](tutorial-single-forest.md).

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)

