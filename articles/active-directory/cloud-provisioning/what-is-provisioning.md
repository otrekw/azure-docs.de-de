---
title: Was ist die Identitätsbereitstellung mit Azure AD? | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über die Identitätsbereitstellung.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5d5528865fc910751b8052dcc78a82131e17290
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91266240"
---
# <a name="what-is-identity-provisioning"></a>Was ist die Identitätsbereitstellung?

Heutzutage werden Unternehmen und Konzerne immer mehr zu einer Mischung aus lokalen Anwendungen und Cloudanwendungen.  Benutzer müssen sowohl auf lokale Anwendungen als auch auf Cloudanwendungen zugreifen können – idealerweise mit einer einzelnen Identität, die sowohl für lokale Anwendungen als auch für Cloudanwendungen verwendet werden kann.

Bei der Bereitstellung wird ein Objekt auf der Grundlage bestimmter Bedingungen erstellt, auf dem neuesten Stand gehalten und wieder gelöscht, wenn die Bedingungen nicht mehr erfüllt sind. Ein Beispiel: Wenn in Ihrer Organisation ein neuer Benutzer hinzukommt, wird er in das Personalsystem aufgenommen.  An diesem Punkt kann im Zuge der Bereitstellung ein entsprechendes Benutzerkonto in der Cloud, in Active Directory und in verschiedenen Anwendungen erstellt werden, auf die der Benutzer Zugriff benötigt.  Dadurch kann der Benutzer umgehend mit der Arbeit beginnen und auf die benötigten Anwendungen und Systeme zugreifen. 

![Diagramm: Cloudbereitstellung mit Azure Active Directory](media/what-is-provisioning/cloud1.png)

Im Kontext von Azure Active Directory lässt sich die Bereitstellung in folgende Schlüsselszenarien unterteilen:  

- **[Personalbasierte Bereitstellung](#hr-driven-provisioning)**  
- **[App-Bereitstellung](#app-provisioning)**  
- **[Verzeichnisbereitstellung](#directory-provisioning)** 

## <a name="hr-driven-provisioning"></a>Personalbasierte Bereitstellung

![Diagramm: Personalbasierte Bereitstellung mit cloudbasiertem und lokalem Personalsystem und Azure Active Directory](media/what-is-provisioning/cloud2.png)

Die Cloudbereitstellung aus dem Personalsystem umfasst die Erstellung von Objekten (Benutzer, Rollen, Gruppen usw.) auf der Grundlage der Informationen aus Ihrem Personalsystem.  

Gängigstes Szenario: Ein neuer Mitarbeiter wird eingestellt und in das Personalsystem aufgenommen.  Daraufhin wird er in der Cloud bereitgestellt –  in diesem Fall in Azure AD.  Die Bereitstellung aus dem Personalsystem kann folgende Szenarien umfassen: 

- **Einstellung neuer Mitarbeiter:** Wenn ein neuer Mitarbeiter dem cloudbasierten Personalsystem hinzugefügt wird, wird in Active Directory, in Azure Active Directory und optional in Microsoft 365 sowie in anderen, von Azure AD unterstützten SaaS-Anwendungen automatisch ein Benutzerkonto erstellt, und die E-Mail-Adresse wird in das cloudbasierte Personalsystem zurückgeschrieben.
- **Aktualisierungen von Mitarbeiterattributen und -profil:** Wenn ein Mitarbeiterdatensatz im cloudbasierten Personalsystem aktualisiert wird (etwa der Name, Titel oder Vorgesetzte), wird das entsprechende Benutzerkonto in Active Directory, in Azure Active Directory und optional in Microsoft 365 und in anderen von Azure AD unterstützten SaaS-Anwendungen automatisch aktualisiert.
- **Kündigung von Mitarbeitern:** Wenn einem Mitarbeiter im cloudbasierten Personalsystem gekündigt wird, wird das entsprechende Benutzerkonto in Active Directory, in Azure Active Directory und optional in Office 365 und in anderen, von Azure AD unterstützten SaaS-Anwendungen automatisch deaktiviert.
- **Erneute Einstellung von Mitarbeitern:** Wenn ein Mitarbeiter im cloudbasierten Personalsystem erneut eingestellt wird, kann sein altes Konto in Active Directory, in Azure Active Directory und optional in Microsoft 365 und in anderen von Azure AD unterstützten SaaS-Anwendungen automatisch reaktiviert oder erneut bereitgestellt werden (je nachdem, was Sie bevorzugen).


## <a name="app-provisioning"></a>App-Bereitstellung

![Diagramm: App-Bereitstellung mit lokalen Apps, nicht von Microsoft stammenden Cloud-Apps und Azure Active Directory](media/what-is-provisioning/cloud3.png)

In Azure Active Directory (Azure AD) bezieht sich der Ausdruck **[App-Bereitstellung](../app-provisioning/user-provisioning.md)** auf die automatische Erstellung von Benutzeridentitäten und Rollen in den Cloudanwendungen, auf die Benutzer Zugriff benötigen. Zusätzlich zur Erstellung von Benutzeridentitäten umfasst die automatische Bereitstellung auch die Wartung und Entfernung von Benutzeridentitäten, wenn sich der Status oder die Rollen ändern. Gängige Szenarien sind die Bereitstellung eines Azure AD-Benutzers in Anwendungen wie [Dropbox](../saas-apps/dropboxforbusiness-provisioning-tutorial.md), [Salesforce](../saas-apps/salesforce-provisioning-tutorial.md), [ServiceNow](../saas-apps/servicenow-provisioning-tutorial.md) und anderen.

## <a name="directory-provisioning"></a>Verzeichnisbereitstellung

![Cloudbereitstellung](media/what-is-provisioning/cloud4.png)

Die lokale Bereitstellung umfasst die Bereitstellung aus lokalen Quellen (etwa Active Directory) in Azure AD.  

Gängigstes Szenario: Ein Benutzer in Active Directory (AD) wird in Azure AD bereitgestellt.

Hierzu werden die Azure AD Connect-Synchronisierung, die Azure AD Connect-Cloudbereitstellung und Microsoft Identity Manager verwendet. 
 
## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
- [Installieren der Cloudbereitstellung](how-to-install.md)