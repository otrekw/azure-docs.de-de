---
title: Häufig gestellte Fragen zur Azure AD Connect-Cloudsynchronisierung
description: Dieses Dokument enthält häufig gestellte Fragen zur Cloudsynchronisierung.
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
ms.openlocfilehash: 39d1554fd1b6cac1a90a794cfd93def97e494bfe
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98612528"
---
# <a name="azure-active-directory-connect-cloud-sync-faq"></a>Häufig gestellte Fragen zur Azure Active Directory Connect-Cloudsynchronisierung

Lesen Sie hier häufig gestellte Fragen zur Azure Active Directory Connect-Cloudsynchronisierung (Azure AD).

## <a name="general-installation"></a>Allgemeine Installation

**F: Wie oft wird die Cloudsynchronisierung ausgeführt?**

Für die Cloudbereitstellung ist eine Ausführung alle zwei Minuten geplant. Alle zwei Minuten werden alle Änderungen an Benutzern, Gruppen oder Kennworthashes in Azure AD bereitgestellt.

**F: Bei der ersten Ausführung werden Fehler bei der Kennworthashsynchronisierung angezeigt. Warum?**

Dies entspricht dem erwarteten Verhalten. Die Fehler sind darauf zurückzuführen, dass das Benutzerobjekt nicht in Azure AD vorhanden ist. Nach der Bereitstellung von Benutzern in Azure AD sollten Kennworthashes bei der nachfolgenden Ausführung bereitgestellt werden. Warten Sie einige Ausführungen, und vergewissern Sie sich, dass bei der Kennworthashsynchronisierung keine Fehler mehr auftreten.

**F: Was passiert, wenn die Active Directory-Instanz über Attribute verfügt, die von der Cloudsynchronisierung nicht unterstützt werden (z. B. Verzeichniserweiterungen)?**

Die Cloudbereitstellung wird ausgeführt, und die unterstützten Attribute werden bereitgestellt. Die nicht unterstützten Attribute werden in Azure AD nicht bereitgestellt. Sehen Sie sich die Verzeichniserweiterungen in Active Directory an, und stellen Sie sicher, dass Sie die Bereitstellung dieser Attribute für Azure AD nicht benötigen. Falls ein oder mehrere Attribute erforderlich sind, können Sie erwägen, die Azure AD Connect-Synchronisierung zu verwenden oder die benötigten Informationen in eines der unterstützten Attribute zu verschieben (z. B. Erweiterungsattribute 1 - 15).

**F: Worin besteht der Unterschied zwischen Azure AD Connect-Synchronisierung und Cloudsynchronisierung?**

Bei der Azure AD Connect-Synchronisierung erfolgt die Bereitstellung auf dem lokalen Synchronisierungsserver. Die Konfiguration wird auf dem lokalen Synchronisierungsserver gespeichert. Bei der Azure AD Connect-Cloudsynchronisierung wird die Bereitstellungskonfiguration in der Cloud gespeichert und als Teil des Azure AD-Bereitstellungsdiensts in der Cloud ausgeführt. 

**F: Kann ich mit der Cloudsynchronisierung mehrere Active Directory-Gesamtstrukturen synchronisieren?**

Ja. Mit der Cloudbereitstellung können mehrere Active Directory-Gesamtstrukturen synchronisiert werden. In der Umgebung mit mehreren Gesamtstrukturen müssen sich alle Verweise (Beispiel, Manager) innerhalb der Domäne befinden.  

**F: Wie wird der Agent aktualisiert?**

Die Agents werden von Microsoft automatisch aktualisiert. Dadurch entfällt für das IT-Team das Testen und Überprüfen neuer Agent-Versionen. 

**F: Kann ich die automatische Aktualisierung deaktivieren?**

Es gibt keine unterstützte Methode zum Deaktivieren der automatischen Aktualisierung.

**F: Kann ich den Quellanker für die Cloudsynchronisierung ändern?**

Bei der Cloudsynchronisierung wird standardmäßig „ms-ds-consistency-GUID“ mit einem Fallback auf ObjectGUID als Quellanker verwendet. Es gibt keine unterstützte Methode zum Ändern des Quellankers.

**F: Bei der Verwendung der Cloudsynchronisierung werden neue Dienstprinzipale mit den AD-Domänennamen angezeigt. Ist das Absicht?**

Ja, bei der Cloudsynchronisierung wird ein Dienstprinzipal für die Bereitstellungskonfiguration mit dem Domänennamen als Dienstprinzipalname erstellt. Nehmen Sie keine Änderungen an der Dienstprinzipalkonfiguration vor.

**F: Was geschieht, wenn ein synchronisierter Benutzer das Kennwort bei der nächsten Anmeldung ändern muss?**

Wenn bei der Cloudsynchronisierung die Kennworthashsynchronisierung aktiviert ist und der synchronisierte Benutzer bei der nächsten Anmeldung bei der lokalen AD-Instanz das Kennwort ändern muss, wird der „zu ändernde“ Kennworthash nicht in Azure AD bereitgestellt. Sobald der Benutzer das Kennwort ändert, wird der Benutzerkennworthash von AD in Azure AD bereitgestellt.

**F: Unterstützt die Cloudsynchronisierung das Rückschreiben von ms-ds-consistencyGUID für Objekte?**

Nein, die Cloudsynchronisierung unterstützt das Rückschreiben von ms-ds-consistencyGUID für Objekte (einschließlich Benutzerobjekte) nicht. 

**F: Ich stelle Benutzer mithilfe der Cloudsynchronisierung bereit. Ich habe die Konfiguration gelöscht. Warum werden die alten synchronisierten Objekte weiterhin in Azure AD angezeigt?** 

Wenn Sie die Konfiguration löschen, werden die synchronisierten Objekte in Azure AD von der Cloudsynchronisierung nicht automatisch entfernt. Wenn Sie sicherstellen möchten, dass keine alten Objekte vorhanden sind, ändern Sie den Bereich der Konfiguration in eine leere Gruppe oder in Organisationseinheiten. Wenn die Bereitstellung läuft und die Objekte bereinigt, deaktivieren und löschen Sie die Konfiguration. 

**F:  Was bedeutet es, dass das Exchange-Hybridbereitstellungsfeature nicht unterstützt wird?**

Das Feature „Exchange-Hybridbereitstellung“ ermöglicht die Koexistenz lokaler und Microsoft 365-basierter Exchange-Postfächer. Azure AD Connect synchronisiert eine bestimmte Gruppe von Attributen aus Azure AD mit Ihrem lokalen Verzeichnis.  Der Agent für die Cloudbereitstellung synchronisiert diese Attribute aktuell nicht mit Ihrem lokalen Verzeichnis und wird daher nicht als Ersatz für Azure AD Connect unterstützt.

**F:  Kann ich den Agent für die Cloudbereitstellung unter Windows Server Core installieren?**

Nein. Die Installation des Agents unter Server Core wird nicht unterstützt.

**F:  Kann ich zusammen mit dem Cloudbereitstellungs-Agent einen Stagingserver verwenden?**

Nein, Stagingserver werden nicht unterstützt.

**F:  Können Gastbenutzerkonten synchronisiert werden?**

Nein, das Synchronisieren von Gastbenutzerkonten wird nicht unterstützt.

**F:  Was geschieht, wenn ich einen Benutzer aus einer Organisationseinheit, deren Gültigkeitsbereich die Cloudsynchronisierung ist, in eine Organisationseinheit verschiebe, deren Gültigkeitsbereich Azure AD Connect ist?**

Der Benutzer wird gelöscht und neu erstellt.  Das Verschieben eines Benutzers aus einer Organisationseinheit, deren Gültigkeitsbereich die Cloudsynchronisierung ist, wird als Löschvorgang betrachtet.  Wenn der Benutzer in eine Organisationseinheit verschoben wird, die von Azure AD Connect verwaltet wird, wird er wieder Azure AD bereitgestellt, und es wird ein neuer Benutzer erstellt.

**F:  Was geschieht mit dem Benutzer, der in Azure AD erstellt wurde, wenn ich die Organisationseinheit umbenenne oder verschiebe, deren Gültigkeitsbereich der Cloudsynchronisierungsfilter ist?**

Nichts.  Die Benutzer werden nicht gelöscht, wenn die Organisationseinheit umbenannt oder verschoben wird.

**F:  Unterstützt die Cloudsynchronisierung von Azure AD Connect große Gruppen?**

Ja. Wir unterstützen heute bis zu 50.000 Gruppenmitglieder, die mithilfe der OE-Bereichsfilterung synchronisiert werden. Wenn Sie gleichzeitig die Gruppenbereichsfilterung verwenden, sollten Sie die Gruppengröße auf weniger als 1.500 Mitglieder beschränken. Der Grund hierfür ist: Sie können zwar eine große Gruppe als Teil des Gruppenbereichsfilters synchronisieren, aber wenn Sie dieser Gruppe in Batches von mehr als 1.500 Mitglieder hinzufügen, tritt bei der Deltasynchronisierung ein Fehler auf. 

## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Worum handelt es sich bei der Azure AD Connect-Cloudsynchronisierung?](what-is-cloud-sync.md)
