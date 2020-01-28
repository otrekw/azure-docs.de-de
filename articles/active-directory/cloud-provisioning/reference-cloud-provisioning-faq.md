---
title: Häufig gestellte Fragen zur Azure AD Connect-Cloudbereitstellung
description: Dieses Dokument enthält häufig gestellte Fragen zur Cloudbereitstellung.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc3c4a943f24ba1f987aa1daf513b9e05ada65a7
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309998"
---
# <a name="azure-active-directory-connect-faq"></a>Häufig gestellte Fragen zu Azure Active Directory Connect

Lesen Sie hier häufig gestellte Fragen zur Azure AD Connect-Cloudbereitstellung (Azure Active Directory).

## <a name="general-installation"></a>Allgemeine Installation

**F: Wie oft wird die Cloudbereitstellung ausgeführt?**

Für die Cloudbereitstellung ist eine Ausführung alle zwei Minuten geplant. Alle zwei Minuten werden alle Änderungen an Benutzern, Gruppen oder Kennworthashes in Azure AD bereitgestellt.

**F: Bei der ersten Ausführung werden Fehler bei der Kennworthashsynchronisierung angezeigt. Warum?**

Dies entspricht dem erwarteten Verhalten. Die Fehler sind darauf zurückzuführen, dass das Benutzerobjekt nicht in Azure AD vorhanden ist. Nach der Bereitstellung von Benutzern in Azure AD sollten Kennworthashes bei der nachfolgenden Ausführung bereitgestellt werden. Warten Sie einige Ausführungen, und vergewissern Sie sich, dass bei der Kennworthashsynchronisierung keine Fehler mehr auftreten.

**F: Worin besteht der Unterschied zwischen Azure AD Connect-Synchronisierung und Cloudbereitstellung?**

Bei der Azure AD Connect-Synchronisierung erfolgt die Bereitstellung auf dem lokalen Synchronisierungsserver. Die Konfiguration wird auf dem lokalen Synchronisierungsserver gespeichert. Bei der Azure AD Connect-Cloudbereitstellung wird die Bereitstellungskonfiguration in der Cloud gespeichert und als Teil des Azure AD-Bereitstellungsdiensts in der Cloud ausgeführt. 

**F: Kann ich mit der Cloudbereitstellung mehrere Active Directory-Gesamtstrukturen synchronisieren?**

Ja. Mit der Cloudbereitstellung können mehrere Active Directory-Gesamtstrukturen synchronisiert werden. In der Umgebung mit mehreren Gesamtstrukturen müssen sich alle Verweise (Beispiel, Manager) innerhalb der Domäne befinden.  

**F: Wie wird der Agent aktualisiert?**

Die Agents werden von Microsoft automatisch aktualisiert. Dadurch entfällt für das IT-Team das Testen und Überprüfen neuer Agent-Versionen. 

**F: Kann ich die automatische Aktualisierung deaktivieren?**

Es gibt keine unterstützte Methode zum Deaktivieren der automatischen Aktualisierung.

**F: Kann ich den Quellanker für die Cloudbereitstellung ändern?**

Bei der Cloudbereitstellung wird standardmäßig „ms-ds-consistency-GUID“ mit einem Fallback auf „ObjectGUID“ als Quellanker verwendet. Es gibt keine unterstützte Methode zum Ändern des Quellankers.

**F: Bei der Verwendung der Cloudbereitstellung werden neue Dienstprinzipale mit den AD-Domänennamen angezeigt. Ist das Absicht?**

Ja, bei der Cloudbereitstellung wird ein Dienstprinzipal für die Bereitstellungskonfiguration mit dem Domänennamen als Dienstprinzipalname erstellt. Nehmen Sie keine Änderungen an der Dienstprinzipalkonfiguration vor.

**F: Was geschieht, wenn ein synchronisierter Benutzer das Kennwort bei der nächsten Anmeldung ändern muss?**

Wenn bei der Cloudbereitstellung die Kennworthashsynchronisierung aktiviert ist und der synchronisierte Benutzer bei der nächsten Anmeldung bei der lokalen AD-Instanz das Kennwort ändern muss, wird der zu ändernde Kennworthash nicht in Azure AD bereitgestellt. Sobald der Benutzer das Kennwort ändert, wird der Benutzerkennworthash von AD in Azure AD bereitgestellt.

**F: Unterstützt die Cloudbereitstellung das Rückschreiben von „ms-ds-consistencyGUID“ für Objekte?**

Nein, die Cloudbereitstellung unterstützt das Rückschreiben von „ms-ds-consistencyGUID“ für Objekte (einschließlich Benutzerobjekte) nicht. 

**F: Ich stelle Benutzer mithilfe der Cloudbereitstellung bereit. Ich habe die Konfiguration gelöscht. Warum werden die alten synchronisierten Objekte weiterhin in Azure AD angezeigt?** 

Wenn Sie die Konfiguration löschen, werden die synchronisierten Objekte in Azure AD bei der Cloudbereitstellung nicht bereinigt. Wenn Sie sicherstellen möchten, dass keine alten Objekte vorhanden sind, ändern Sie den Bereich der Konfiguration in eine leere Gruppe oder in Organisationseinheiten. Wenn die Bereitstellung läuft und die Objekte bereinigt, deaktivieren und löschen Sie die Konfiguration. 

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
