---
title: Anzeigen des Überwachungsberichts für Zuweisungen von Gruppen mit privilegiertem Zugriff in Privileged Identity Management (PIM) – Azure AD | Microsoft-Dokumentation
description: Zeigen Sie Aktivitäten und den Überwachungsverlauf für Zuweisungen von Gruppen mit privilegiertem Zugriff in Azure AD Privileged Identity Management (PIM) an.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: pim
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94dd5c2579eb7ce96dea70c6354df8ec84125bd9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88141668"
---
# <a name="audit-activity-history-for-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Aktivitäten und Überwachungsverlauf für Zuweisungen von Gruppen mit privilegiertem Zugriff (Vorschau) in Privileged Identity Management

Mit Privileged Identity Management (PIM) können Sie Aktivitäten, Aktivierungen und den Überwachungsverlauf für Mitglieder und Besitzer von Azure-Gruppen mit privilegiertem Zugriff innerhalb Ihrer Azure Active Directory (Azure AD)-Organisation anzeigen.

> [!NOTE]
> Wenn Ihre Organisation Verwaltungsfunktionen an einen Dienstanbieter ausgelagert hat, der die [delegierte Azure-Ressourcenverwaltung](../../lighthouse/concepts/azure-delegated-resource-management.md) verwendet, werden die von diesem Dienstanbieter autorisierten Rollenzuweisungen hier nicht angezeigt.

Führen Sie die folgenden Schritte aus, um den Überwachungsverlauf für Gruppen mit privilegiertem Zugriff anzuzeigen.

## <a name="view-resource-audit-history"></a>Anzeigen des Ressourcenüberwachungsverlaufs

Mit der **Ressourcenüberwachung** erhalten Sie eine Übersicht über alle Aktivitäten, die Ihren Gruppen mit privilegiertem Zugriff zugeordnet sind.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Gruppen mit privilegiertem Zugriff (Vorschau)** aus.

1. Wählen Sie die Gruppe mit privilegiertem Zugriff aus, für die Sie den Überwachungsverlauf anzeigen möchten.

1. Wählen Sie unter **Aktivität** die Option **Ressourcenüberwachung** aus.

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Ressourcenüberwachungsliste mit Filtern](media/groups-audit/groups-resource-audit.png)

## <a name="view-my-audit"></a>Anzeigen der eigenen Überwachung

Unter **Meine Überwachung** können Sie Ihre persönliche Rollenaktivität für eine Gruppe mit privilegiertem Zugriff anzeigen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Gruppen mit privilegiertem Zugriff (Vorschau)** aus.

1. Wählen Sie die Gruppe mit privilegiertem Zugriff aus, für die Sie den Überwachungsverlauf anzeigen möchten.

1. Wählen Sie unter **Aktivität** die Option **Meine Überwachung** aus.

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Überwachungsliste für den aktuellen Benutzer](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Gruppenmitgliedschaft und -besitz in Privileged Identity Management](groups-assign-member-owner.md)
- [Genehmigen oder Ablehnen von Anforderungen für Gruppen mit privilegiertem Zugriff in PIM](groups-approval-workflow.md)
- [Anzeigen des Überwachungsverlaufs für Azure AD-Rollen in PIM](groups-audit.md)
