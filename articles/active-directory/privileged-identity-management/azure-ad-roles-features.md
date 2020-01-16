---
title: Verwalten von Azure AD-Rollen in Privileged Identity Management (PIM) | Microsoft-Dokumentation
description: Verwalten benutzerdefinierter Azure AD-Rollen für die Zuweisung in Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef2b27e1ca9c1f6f8dfec1bd2bce4ad09c599cae
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475264"
---
# <a name="management-capabilities-for-azure-ad-roles-in-privileged-identity-management"></a>Verwaltungsfunktionen für Azure AD-Rollen in Privileged Identity Management

Die Verwaltungsfunktionen für Azure AD Rollen in Privileged Identity Management wurden aktualisiert, um die Verwaltung von Azure AD-Rollen und Azure-Ressourcenrollen zu vereinheitlichen. Zuvor umfasste Privileged Identity Management für Azure-Ressourcenrollen einige wichtige Features, die für Azure AD-Rollen nicht verfügbar waren.

Mit dem aktuell eingeführten Update führen wir beide Funktionen in einer einzigen Verwaltungsumgebung zusammen, in der Sie die gleichen Funktionen für Azure AD-Rollen wie für Azure-Ressourcenrollen finden. In diesem Artikel erhalten Sie Informationen zu den aktualisierten Features und sämtlichen Anforderungen.

## <a name="time-bound-assignments"></a>Zeitgebundene Zuweisungen

Zuvor waren in Privileged Identity Management für Azure AD-Rollen Rollenzuweisungen mit zwei möglichen Zuständen bekannt: *berechtigt* und *permanent*. Nun können Sie eine Start- und Endzeit für jeden Zuweisungstyp festlegen. Diese Ergänzung erlaubt vier mögliche Zustände, die Sie einer Zuweisung zuordnen können:

- Permanent berechtigt
- Permanent aktiv
- Berechtigt, mit angegebenen Start-/Enddaten für die Zuweisung
- Aktiv, mit angegebenen Start-/Enddaten für die Zuweisung

In vielen Fällen können Sie auch dann, wenn Sie für Benutzer nicht jedes Mal Berechtigungen zuweisen und Rollen aktivieren möchten, Ihre Azure AD-Organisation schützen, indem Sie eine Ablaufzeit für Zuweisungen festlegen. Wenn Sie z. B. über einige temporäre Benutzer verfügen, die berechtigt sind, sollten Sie eine Ablaufzeit festlegen, damit diese automatisch aus der Rollenzuweisung entfernt werden, wenn ihre Arbeit erledigt ist.

## <a name="new-role-settings"></a>Neue Rolleneinstellungen

Wir fügen außerdem neue Einstellungen für Azure AD-Rollen hinzu. Zuvor konnten Sie nur Aktivierungseinstellungen für einzelne Rollen konfigurieren. Das heißt, dass Aktivierungseinstellungen wie Anforderungen für die mehrstufige Authentifizierung und Incident-/Anforderungstickets auf alle Benutzer angewandt wurden, die für eine bestimmte Rolle berechtigt waren. Nun können Sie konfigurieren, ob ein einzelner Benutzer eine mehrstufige Authentifizierung ausführen muss, bevor er eine Rolle aktivieren kann. Außerdem haben Sie mehr Kontrolle über Ihre Privileged Identity Management-E-Mails zu bestimmten Rollen.

## <a name="extend-and-renew-assignments"></a>Verlängern und Erneuern von Zuweisungen

Wenn Sie sich mit der zeitgebundenen Zuweisung vertraut machen, fragen Sie sich vielleicht, was passiert, wenn eine Rolle abgelaufen ist? In dieser neuen Version bieten wir zwei Optionen für dieses Szenario:

- Verlängern: Wenn eine Rollenzuweisung demnächst abläuft, kann der Benutzer über Privileged Identity Management eine Verlängerung für diese Rollenzuweisung anfordern.
- Erneuern: Wenn eine Rollenzuweisung abgelaufen ist, kann der Benutzer über Privileged Identity Management eine Erneuerung für diese Rollenzuweisung anfordern.

Diese beiden vom Benutzer initiierten Aktionen erfordern eine Genehmigung von einem globalen Administrator oder einem Administrator für privilegierte Rollen. Administratoren müssen sich also nicht mehr um die Verwaltung von ablaufenden Zuweisungen kümmern. Sie müssen nur auf die Anforderungen zur Verlängerung oder Erneuerung warten und sie genehmigen, wenn die Anforderung berechtigt ist.

## <a name="api-changes"></a>API-Änderungen

Wenn Kunden die aktualisierte Version in Ihrer Azure AD Organisation eingeführt haben, funktioniert die bestehende Graph-API nicht mehr. Sie müssen die Umstellung durchführen, um die [Graph-API für Azure-Ressourcenrollen](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-resources?view=graph-rest-beta) verwenden zu können. Um Azure AD-Rollen mit dieser API zu verwalten, ersetzen Sie `/azureResources` in der Signatur durch `/aadroles`, und verwenden Sie die Verzeichnis-ID als `resourceId`.

Wir haben versucht, mit allen Kunden in Kontakt zu treten, die die bisherige API verwenden, um sie über diese Änderung vorab zu informieren. Wenn Ihre Azure AD-Organisation auf die neue Version umgestellt wurde und Sie weiterhin von der alten API abhängig sind, wenden Sie sich unter pim_preview@microsoft.com an das Team.

## <a name="powershell-change"></a>PowerShell-Änderung

Für Kunden, die das Privileged Identity Management-PowerShell-Modul für Azure AD-Rollen verwenden, funktioniert PowerShell mit dem Update nicht mehr. Anstelle der vorherigen Cmdlets müssen Sie die Privileged Identity Management-Cmdlets in der Vorschauversion des PowerShell-Moduls für Azure AD verwenden. Installieren Sie das Azure AD PowerShell-Modul aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17). Die Cmdlets basieren auf der Graph-API.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-assign.md)
- [Entfernen oder Aktualisieren der Zuweisung einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-update-remove.md)
- [Konfigurieren der Zuweisung einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-configure.md)
- [Rollendefinitionen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md)
