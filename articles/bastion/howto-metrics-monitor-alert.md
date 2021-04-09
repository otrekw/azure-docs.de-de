---
title: Konfigurieren der Überwachung und der Metriken mithilfe von Azure Monitor
titleSuffix: Azure Bastion
description: Hier erfahren Sie mehr über die Überwachung und die Metriken in Azure Bastion, die Azure Monitor verwenden, die Azure-Lösung für Metriken, Warnungen und Diagnoseprotokolle.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: how-to
ms.date: 03/12/2021
ms.author: mialdrid
ms.openlocfilehash: 3a5beba3938b5a845a378ede155f2f64e6baac7a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103417942"
---
# <a name="how-to-configure-monitoring-and-metrics-for-azure-bastion-using-azure-monitor"></a>Konfigurieren der Überwachung und der Metriken für Azure Bastion mithilfe von Azure Monitor

In diesem Artikel erfahren Sie, wie Sie mit Azure Monitor die Überwachung und die Metriken für Azure Bastion konfigurieren.

>[!NOTE]
>Die Verwendung **klassischer Metriken** wird nicht empfohlen.
>

## <a name="about-metrics"></a>Informationen zu Metriken

Azure Bastion bietet verschiedene Metriken. In der folgenden Tabelle sind die Kategorie und die Dimension der einzelnen verfügbaren Metriken dargestellt.

|**Metrik**|**Kategorie**|**Dimension(en)**|
| --- | --- | --- |
|Bastion-Kommunikationsstatus**|[Verfügbarkeit](#availability)|–|
|Gesamter Speicher|[Verfügbarkeit](#availability)|Instanz|
|Verwendete CPU|[Verkehr](#traffic)|Instanz
|Verwendeter Arbeitsspeicher|[Verkehr](#traffic)|Instanz
|Sitzungsanzahl|[Leistung](#performance)|Instanz|

** Der Bastion-Kommunikationsstatus ist nur auf Bastionhosts anwendbar, die nach November 2020 bereitgestellt wurden.

### <a name="availability-metrics"></a><a name="availability"></a>Verfügbarkeitsmetriken

#### <a name="bastion-communication-status"></a><a name="communication-status"></a>Bastion-Kommunikationsstatus

Sie können den Kommunikationsstatus von Azure Bastion für alle Instanzen zusammengefasst anzeigen, die den Bastionhost darstellen.

* Der Wert **1** zeigt an, dass der Bastionhost verfügbar ist.
* Der Wert **0** hingegen bedeutet, dass der Bastionhostdienst nicht verfügbar ist.

:::image type="content" source="./media/metrics-monitor-alert/communication-status.png" alt-text="Screenshot, der den Kommunikationsstatus zeigt":::

#### <a name="total-memory"></a><a name="total-memory"></a>Speicher insgesamt

Sie können die insgesamt für Azure Bastion bereitgestellte und auf die einzelnen Instanzen aufgeteilte Speichermenge anzeigen.

:::image type="content" source="./media/metrics-monitor-alert/total-memory.png" alt-text="Screenshot, der die Gesamtgröße des Speichers zeigt":::

### <a name="traffic-metrics"></a><a name="traffic"></a>Datenverkehrsmetriken

#### <a name="used-cpu"></a><a name="used-cpu"></a>Verwendete CPU

Sie können die CPU-Auslastung von Azure Bastion für die einzelnen Bastionhostinstanzen anzeigen. Anhand dieser Metrik können Sie die Verfügbarkeit und Kapazität der Azure Bastion-Instanzen beurteilen.

:::image type="content" source="./media/metrics-monitor-alert/used-cpu.png" alt-text="Screenshot der CPU-Auslastung":::

#### <a name="used-memory"></a><a name="used-memory"></a>Verwendeter Arbeitsspeicher

Sie können die Arbeitsspeicherauslastung für die einzelnen Bastionhostinstanzen anzeigen. Anhand dieser Metrik können Sie die Verfügbarkeit und Kapazität der Azure Bastion-Instanzen beurteilen.

:::image type="content" source="./media/metrics-monitor-alert/used-memory.png" alt-text="Screenshot der Arbeitsspeicherauslastung":::

### <a name="performance-metrics"></a><a name="performance"></a>Leistungsmetriken

#### <a name="session-count"></a>Sitzungsanzahl

Sie können die Anzahl aktiver Sitzungen pro Bastionhostinstanz und für die verschiedenen Sitzungstypen zusammengefasst (RDP und SSH) anzeigen. Jede Azure Bastion-Instanz kann verschiedene aktive RDP- und SSH-Sitzungen unterstützen. Diese Metrik hilft Ihnen zu beurteilen, ob Sie die Anzahl der Instanzen anpassen müssen, auf denen der Bastionhostdienst ausgeführt wird. Weitere Informationen zur Sitzungsanzahl, die Azure Bastion unterstützt, finden Sie in den [FAQ zu Azure Bastion](bastion-faq.md).

:::image type="content" source="./media/metrics-monitor-alert/session-count.png" alt-text="Screenshot der Sitzungsanzahl":::

## <a name="how-to-view-metrics"></a><a name="metrics"></a>Anzeigen von Metriken

1. Navigieren Sie zu Ihrem Bastionhost, um Metriken anzuzeigen.
1. Wählen Sie aus der Liste **Überwachung** die Option **Metrik** aus.
1. Wählen Sie die Parameter aus. Falls keine Metriken festgelegt sind, klicken Sie auf **Metrik hinzufügen**, und wählen Sie die Parameter aus.

   * **Umfang:** Der Bereich ist standardmäßig auf den Bastionhost festgelegt.
   * **Metriknamespace:** Standardmetriken
   * **Metrik:** Wählen Sie die Metrik aus, die Sie anzeigen möchten.

1. Nach dem Auswählen einer Metrik wird die Standardaggregation angewendet. Optional können Sie die Teilung anwenden, wonach die Metrik mit anderen Dimensionen angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
  
