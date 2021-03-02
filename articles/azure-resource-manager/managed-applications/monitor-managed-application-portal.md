---
title: Verwenden des Azure-Portals zum Überwachung einer verwalteten App
description: Zeigt, wie das Azure-Portal verwendet wird, um die Verfügbarkeit und Warnungen einer verwalteten Anwendung zu überwachen.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: a02062edd1a940bcc6588ab53457e0af91fedd9a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100578274"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Überwachen einer bereitgestellten Instanz einer verwalteten Anwendung

Nachdem Sie eine verwaltete Anwendung in Ihrem Azure-Abonnement bereitgestellt haben, können Sie den Status der Anwendung überprüfen. Dieser Artikel beschreibt die Optionen im Azure-Portal zum Überprüfen des Status. Sie können die Verfügbarkeit der Ressourcen in einer verwalteten Anwendung überwachen. Sie können auch Warnungen einrichten und anzeigen.

## <a name="view-resource-health"></a>Anzeigen der Ressourcenintegrität

1. Wählen Sie Ihre verwaltete Anwendungsinstanz aus.

   ![Auswählen der verwalteten Anwendung](./media/monitor-managed-application-portal/select-managed-application.png)

1. Klicken Sie auf **Ressourcenintegrität**.

   ![Auswählen der Ressourcenintegrität](./media/monitor-managed-application-portal/select-resource-health.png)

1. Zeigen Sie die Verfügbarkeit der Ressourcen in einer verwalteten Anwendung an.

   ![Anzeigen der Ressourcenintegrität](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Anzeigen von Warnungen

1. Wählen Sie **Warnungen**.

   ![Auswählen von Warnungen](./media/monitor-managed-application-portal/select-alerts.png)

1. Wenn Sie die Warnungsregeln konfiguriert haben, werden Informationen zu den ausgelösten Warnungen angezeigt.

   ![Anzeigen von Warnungen](./media/monitor-managed-application-portal/view-alerts.png)

1. Wählen Sie zum Hinzufügen von Warnungsregeln **+ Neue Warnungsregel** aus.

   ![Erstellen einer Warnung](./media/monitor-managed-application-portal/create-new-alert.png)

Sie können Warnungen für Ihre verwaltete Anwendungsinstanz oder die Ressourcen in der verwalteten Anwendung erstellen. Weitere Informationen zum Erstellen von Warnungen finden Sie unter [Überblick über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* Beispiele zu verwalteten Anwendungen finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).
* Informationen zum Bereitstellen einer verwalteten Anwendung finden Sie unter [Bereitstellen einer Dienstkatalog-App über das Azure-Portal](deploy-service-catalog-quickstart.md).