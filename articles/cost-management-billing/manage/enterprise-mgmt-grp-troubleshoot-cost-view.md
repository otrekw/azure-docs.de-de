---
title: Problembehandlung bei Azure Enterprise-Kostenansichten
description: Erfahren Sie, wie Sie Probleme beheben, die bei Unternehmenskostenansichten im Azure-Portal auftreten könnten.
author: bandersmsft
manager: amberb
ms.service: cost-management-billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 3abfac5f915b5c1c6020701feaa356d7a043ef8d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75985606"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Problembehandlung bei Unternehmenskostenansichten

In Unternehmensregistrierungen gibt es mehrere Einstellungen, die dazu führen können, dass Benutzer in der Anmeldung keine Kosten sehen können.  Diese Einstellungen werden vom Registrierungsadministrator verwaltet. Wurde die Registrierung nicht direkt über Microsoft gekauft, werden die Einstellungen vom Partner verwaltet.  In diesem Artikel erfahren Sie, um welche Einstellungen es geht, und wie sie sich auf die Anmeldung auswirken. Diese Einstellungen sind unabhängig von den„Rollenbasierte Zugriffssteuerung in Azure“-Rollen.

## <a name="enable-access-to-costs"></a>Aktivieren des Zugriffs auf Kosten

Erhalten Sie die Meldung „Nicht autorisiert“ oder *Kostenansichten sind in Ihrer Registrierung deaktiviert*, wenn Sie nach Kosteninformationen suchen?
![Screenshot mit „Nicht autorisiert“ im Feld „Aktuelle Kosten“ für das Abonnement](./media/enterprise-mgmt-grp-troubleshoot-cost-view/unauthorized.png)

Hierfür kann es einen der folgenden Gründe geben:

1. Sie haben Azure über einen Enterprise-Partner gekauft, und der Partner hat die Preisinformationen noch nicht freigegeben. Bitten Sie den Partner, die Preiseinstellung im [Enterprise Portal](https://ea.azure.com) zu aktualisieren.
2. Wenn Sie ein EA-Direktkunde sind, haben Sie mehrere Möglichkeiten:
    * Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung **Gebühren anzeigen** für Kontobesitzer deaktiviert.  
    * Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung **Gebühren anzeigen** für Abteilungsadministratoren deaktiviert.
    * Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten. Der Registrierungsadministrator kann die Einstellungen im [Enterprise Portal](https://ea.azure.com/manage/enrollment) aktualisieren.

      ![Screenshot mit den Enterprise Portal-Einstellungen für das Anzeigen von Gebühren.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Asset ist nicht verfügbar

Wenn Sie bei dem Versuch, auf ein Abonnement oder eine Verwaltungsgruppe zuzugreifen, die Fehlermeldung **Dieses Asset ist nicht verfügbar** erhalten, verfügen Sie nicht über die richtige Rolle, um dieses Element anzeigen zu können.  

![Screenshot mit der Meldung „Asset ist nicht verfügbar“.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/asset-not-found.png)

Bitten Sie Ihren Azure-Abonnement- oder -Verwaltungsgruppenadministrator, Ihnen Zugriff zu gewähren. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mithilfe von RBAC und des Azure-Portals](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).
