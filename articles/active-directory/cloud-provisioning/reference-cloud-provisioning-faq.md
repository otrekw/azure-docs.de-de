---
title: Häufig gestellte Fragen zur Azure AD Connect-Cloudbereitstellung
description: Dieses Dokument enthält häufig gestellte Fragen zur Cloudbereitstellung.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: efcf2df4e472d022fcdec0c9b7c69c73192c503f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518469"
---
# <a name="azure-active-directory-connect-cloud-provisioning-faq"></a>Häufig gestellte Fragen zur Azure Active Directory Connect-Cloudbereitstellung

Lesen Sie hier häufig gestellte Fragen zur Azure AD Connect-Cloudbereitstellung (Azure Active Directory).

## <a name="general-installation"></a>Allgemeine Installation

**F: Wie oft wird die Cloudbereitstellung ausgeführt?**

Für die Cloudbereitstellung ist eine Ausführung alle zwei Minuten geplant. Alle zwei Minuten werden alle Änderungen an Benutzern, Gruppen oder Kennworthashes in Azure AD bereitgestellt.

**F: Bei der ersten Ausführung werden Fehler bei der Kennworthashsynchronisierung angezeigt. Warum?**

Dies entspricht dem erwarteten Verhalten. Die Fehler sind darauf zurückzuführen, dass das Benutzerobjekt nicht in Azure AD vorhanden ist. Nach der Bereitstellung von Benutzern in Azure AD sollten Kennworthashes bei der nachfolgenden Ausführung bereitgestellt werden. Warten Sie einige Ausführungen, und vergewissern Sie sich, dass bei der Kennworthashsynchronisierung keine Fehler mehr auftreten.

**F: Was passiert, wenn die Active Directory-Instanz über Attribute verfügt, die von der Cloudbereitstellung nicht unterstützt werden (z. B. Verzeichniserweiterungen)?**

Die Cloudbereitstellung wird ausgeführt, und die unterstützten Attribute werden bereitgestellt. Die nicht unterstützten Attribute werden in Azure AD nicht bereitgestellt. Sehen Sie sich die Verzeichniserweiterungen in Active Directory an, und stellen Sie sicher, dass Sie die Bereitstellung dieser Attribute für Azure AD nicht benötigen. Falls ein oder mehrere Attribute erforderlich sind, können Sie erwägen, die Azure AD Connect-Synchronisierung zu verwenden oder die benötigten Informationen in eines der unterstützten Attribute zu verschieben (z. B. Erweiterungsattribute 1 - 15).

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

Wenn bei der Cloudbereitstellung die Kennworthashsynchronisierung aktiviert ist und der synchronisierte Benutzer bei der nächsten Anmeldung bei der lokalen AD-Instanz das Kennwort ändern muss, wird der „zu ändernde“ Kennworthash nicht in Azure AD bereitgestellt. Sobald der Benutzer das Kennwort ändert, wird der Benutzerkennworthash von AD in Azure AD bereitgestellt.

**F: Unterstützt die Cloudbereitstellung das Rückschreiben von „ms-ds-consistencyGUID“ für Objekte?**

Nein, die Cloudbereitstellung unterstützt das Rückschreiben von „ms-ds-consistencyGUID“ für Objekte (einschließlich Benutzerobjekte) nicht. 

**F: Ich stelle Benutzer mithilfe der Cloudbereitstellung bereit. Ich habe die Konfiguration gelöscht. Warum werden die alten synchronisierten Objekte weiterhin in Azure AD angezeigt?** 

Wenn Sie die Konfiguration löschen, werden die synchronisierten Objekte in Azure AD von der Cloudbereitstellung nicht automatisch entfernt. Wenn Sie sicherstellen möchten, dass keine alten Objekte vorhanden sind, ändern Sie den Bereich der Konfiguration in eine leere Gruppe oder in Organisationseinheiten. Wenn die Bereitstellung läuft und die Objekte bereinigt, deaktivieren und löschen Sie die Konfiguration. 

**F:  Was bedeutet es, dass das Exchange-Hybridbereitstellungsfeature nicht unterstützt wird?**

Das Exchange-Hybridbereitstellungsfeature ermöglicht die Koexistenz lokaler und Office 365-basierter Exchange-Postfächer. Azure AD Connect synchronisiert eine bestimmte Gruppe von Attributen aus Azure AD mit Ihrem lokalen Verzeichnis.  Der Agent für die Cloudbereitstellung synchronisiert diese Attribute aktuell nicht mit Ihrem lokalen Verzeichnis und wird daher nicht als Ersatz für Azure AD Connect unterstützt.

**F:  Kann ich den Agent für die Cloudbereitstellung unter Windows Server Core installieren?**

Nein. Die Installation des Agents unter Server Core wird nicht unterstützt.

**F:  Kann ich zusammen mit dem Cloudbereitstellungs-Agent einen Stagingserver verwenden?**

Nein, Stagingserver werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
