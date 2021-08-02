---
title: Problembehandlung für Elastic – Azure-Partnerlösungen
description: Dieser Artikel enthält Informationen zur Problembehandlung bei der Elastic-Integration in Azure.
ms.service: partner-services
ms.topic: conceptual
ms.date: 05/20/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: f4bc4bc9d9bb5890ed95bbb446f570d25e43d813
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111952574"
---
# <a name="troubleshooting-elastic-integration-with-azure"></a>Problembehandlung bei der Elastic-Integration in Azure

Dieses Dokument enthält Informationen zur Problembehandlung für Lösungen, die Elastic verwenden.

## <a name="unable-to-create-an-elastic-resource"></a>Elastic-Ressource kann nicht erstellt werden

Eine Elastic-Integration in Azure kann nur von Benutzern eingerichtet werden, die über Zugriff vom Typ *Besitzer* auf das Azure-Abonnement verfügen. [Vergewissern Sie sich, dass Sie über den entsprechenden Zugriff verfügen](../../role-based-access-control/check-access.md).

## <a name="logs-not-being-emitted-to-elastic"></a>Protokolle werden nicht an Elastic ausgegeben

Nur Ressourcen, die in [Azure Monitor-Ressourcenprotokollkategorien](../../azure-monitor/essentials/resource-logs-categories.md) aufgelistet sind, geben Protokolle an Elastic aus. Um zu überprüfen, ob eine Ressource Protokolle an Elastic ausgibt, navigieren Sie zu den [Azure-Diagnoseeinstellungen](../../azure-monitor/essentials/diagnostic-settings.md) für die Ressource. Überprüfen Sie, ob eine Option für die Diagnoseeinstellung vorhanden ist.

:::image type="content" source="media/troubleshoot/check-diagnostic-setting.png" alt-text="Überprüfen der Diagnoseeinstellung":::

## <a name="purchase-errors"></a>Fehler beim Kauf

* Beim Kauf tritt ein Fehler auf, weil keine gültige Kreditkarte mit dem Azure-Abonnement verknüpft ist oder keine Zahlungsmethode für das Abonnement angegeben ist.

  Verwenden Sie ein anderes Azure-Abonnement. Alternativ können Sie die Kreditkarte oder Zahlungsmethode für das Abonnement hinzufügen oder aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen oder Aktualisieren einer Kreditkarte für Azure](../../cost-management-billing/manage/change-credit-card.md).

* Das EA-Abonnement lässt keine Marketplace-Käufe zu.

  Verwenden Sie ein anderes Abonnement, oder überprüfen Sie, ob Marketplace-Käufe für Ihr EA-Abonnement aktiviert sind. Weitere Informationen finden Sie unter [Aktivieren von Azure Marketplace-Einkäufen](../../cost-management-billing/manage/ea-azure-marketplace.md#enabling-azure-marketplace-purchases).

## <a name="get-support"></a>Support

Wenn Sie sich bezüglich der Elastic-Integration in Azure an den Support wenden möchten, wählen Sie im linken Bereich **Neue Supportanfrage** aus. Wählen Sie **Elastic-Supportticket öffnen** aus.

:::image type="content" source="media/troubleshoot/open-ticket.png" alt-text="Öffnen eines Supporttickets":::

Öffnen Sie auf der Elastic-Website eine Supportanfrage.

:::image type="content" source="media/troubleshoot/elastic-support.png" alt-text="Öffnen des Elastic-Supports":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Verwaltung Ihrer Elastic-Instanz](manage.md).