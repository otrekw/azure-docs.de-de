---
title: Metriken und Warnungen – Azure DNS
description: Mit diesem Lernpfad können Sie mit den Metriken und Warnungen von Azure DNS beginnen.
services: dns
documentationcenter: na
author: rohinkoul
manager: kumudD
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2021
ms.author: rohink
ms.openlocfilehash: f2eaac432673682b075763c6ce9fe9426ed77a70
ms.sourcegitcommit: 2f322df43fb3854d07a69bcdf56c6b1f7e6f3333
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108017563"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS-Metriken und -Warnungen

Azure DNS ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Microsoft Azure-Infrastruktur ausführt. Dieser Artikel beschreibt die Metriken und Warnungen für den Azure DNS-Dienst.

## <a name="azure-dns-metrics"></a>Azure DNS-Metriken

Mit Azure DNS stehen Ihnen Metriken zur Verfügung, mit denen Sie bestimmte Aspekte Ihrer DNS-Zonen überwachen können. Mithilfe der Metriken in Azure DNS können Sie die Benachrichtigung basierend auf erfüllten Bedingungen konfigurieren. Die zur Verfügung gestellten Metriken verwenden den [Azure Monitor-Dienst](../azure-monitor/index.yml) zur Anzeige der Daten. Weitere Informationen zur Benutzeroberfläche des Metrik-Explorers und zur Diagrammerstellung finden Sie unter [Metrik-Explorer von Azure Monitor](../azure-monitor/essentials/metrics-charts.md). 
 
Azure DNS stellt Azure Monitor die folgenden Metriken für Ihre DNS-Zonen zur Verfügung:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

Weitere Informationen finden Sie unter [Metrikdefinition](../azure-monitor/essentials/metrics-supported.md#microsoftnetworkdnszones).

>[!NOTE]
> Zurzeit sind diese Metriken nur für öffentliche DNS-Zonen verfügbar, die in Azure DNS gehostet werden. Wenn Sie private Zonen verwenden, die in Azure DNS gehostet werden, stellen diese Metriken keine Daten für diese Zonen bereit. Die Metriken und das Warnungsfeature werden darüber hinaus nur in der Azure Public Cloud unterstützt. Unterstützung für unabhängige Clouds wird zu einem späteren Zeitpunkt bereitgestellt. 

Das detaillierteste Element, für das Metriken angezeigt werden können, ist eine DNS-Zone. Derzeit können Sie keine Metriken für einzelne Ressourcendatensätze innerhalb einer Zone anzeigen.

### <a name="query-volume"></a>Abfragevolumen

Die Metrik *Abfragevolumen* zeigt die Anzahl der von Azure DNS empfangenen DNS-Abfragen für Ihre DNS-Zone. Die Maßeinheit ist `Count` und die Aggregation ist die `Sum` sämtlicher Abfragen, die über einen bestimmten Zeitraum eingegangen sind.

Wählen Sie zum Anzeigen dieser Metrik **Metriken (Vorschau)** in der Explorer-Benutzeroberfläche auf der Registerkarte **Überwachen** im Azure-Portal aus. Navigieren Sie zu Ihrer DNS-Zone und wählen Sie dann **Anwenden**. Wählen Sie in der Dropdownliste für *Metriken* die Option `Query Volume` und dann in der Dropdownliste für *Aggregation* die Option `Sum` aus.

![Abfragevolumen](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Abbildung: Metriken zum Azure DNS-Abfragevolumen*

### <a name="record-set-count"></a>Anzahl von Datensatzgruppen

Die Metrik *Anzahl der Recordsets* zeigt die Anzahl der Recordsets in Azure DNS für Ihre DNS-Zone. Alle Recordsets, die in der Zone definiert sind, werden dabei gezählt. Die Maßeinheit ist `Count` und die Aggregation ist die `Maximum` sämtlicher Recordsets.

Wählen Sie zum Anzeigen dieser Metrik **Metriken** in der Explorer-Benutzeroberfläche auf der Registerkarte **Überwachen** im Azure-Portal aus. Navigieren Sie zu Ihrer DNS-Zone und wählen Sie dann **Anwenden**. Wählen Sie in der Dropdownliste für *Metriken* die Option `Query Volume` und dann in der Dropdownliste für *Aggregation* die Option `Sum` aus.

Wählen Sie Ihre DNS-Zone aus der Dropdownliste **Ressource** aus, wählen Sie die Metrik **Anzahl der Recordsets** aus, und wählen Sie dann **Max** als **Aggregation** aus. 

![Anzahl von Datensatzgruppen](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Abbildung: Metriken zur Azure DNS-Recordsetanzahl*

### <a name="record-set-capacity-utilization"></a>Kapazitätsauslastung von Datensatzgruppen

In der Metrik *Recordset-Kapazitätsauslastung* wird die prozentuale Auslastung Ihrer Recordset-Kapazität für eine DNS-Zone angezeigt. Für jede Azure-DNS-Zone gibt es ein Recordset-Limit, das die maximale Anzahl der für die Zone zulässigen Recordsets definiert. Weitere Informationen finden Sie im Abschnitt [DNS-Limits](dns-zones-records.md#limits). Die Maßeinheit ist ein `Percentage` und der Aggregationstyp ist `Maximum`.

Wenn Sie beispielsweise 500 Recordsets in Ihrer DNS-Zone konfiguriert haben und die Zone das Standard-Recordset-Limit von 5000 hat. Die Metrik RecordSetCapacityUtilization" zeigt den Wert von 10 % an, der sich aus der Division von 500 durch 5000 ergibt. 

Wählen Sie zum Anzeigen dieser Metrik **Metriken** in der Explorer-Benutzeroberfläche auf der Registerkarte **Überwachen** im Azure-Portal aus. Navigieren Sie zu Ihrer DNS-Zone und wählen Sie dann **Anwenden**. Wählen Sie in der Dropdownliste für *Metriken* die Option `Record Set Capacity Utilization` und dann in der Dropdownliste für *Aggregation* die Option `Sum` aus. 

![Screenshot mit einer Beispielanzeige von Metriken.](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Abbildung: Metriken zur Azure DNS-Kapazitätsauslastung der Recordsets*

## <a name="alerts-in-azure-dns"></a>Warnungen in Azure DNS

Azure Monitor bietet Warnmeldungen, die Sie für jeden verfügbaren Metrikwert konfigurieren können. Weitere Informationen finden Sie unter [Azure Monitor Warnungen](../azure-monitor/alerts/alerts-metric.md).

1. Zur Konfiguration der Warnmeldungen für Azure DNS-Zonen wählen Sie **Warnungen** auf der Seite *Überwachung* im Azure-Portal. Klicken Sie dann auf **+ Neue Warnungsregel**.

    :::image type="content" source="./media/dns-alerts-metrics/alert-metrics.png" alt-text="Screenshot der Schaltfläche Warnung auf der Seite Monitor.":::


1. Klicken Sie auf die Verknüpfung **Ressource wählen** im Bereich, um die Seite *Ressource auswählen* zu öffnen. Filtern Sie nach **DNS-Zonen** und wählen Sie dann die Azure-DNS-Zone, die Sie als Zielressource verwenden möchten. Wählen Sie **Fertig**, nachdem Sie die Zone gewählt haben.

    :::image type="content" source="./media/dns-alerts-metrics/select-resource.png" alt-text="Screenshot der Seite &quot;Ressource auswählen&quot; in der Konfiguration von Warnmeldungen.":::

1. Anschließend klicken Sie auf die Verknüpfung **Bedingung hinzufügen** im Abschnitt Bedingungen, damit sich die Seite *Wählen Sie ein Signal* öffnet. Wählen Sie eine der drei *Metrischen* Signalarten, mit denen Sie die Warnmeldung konfigurieren möchten.

    :::image type="content" source="./media/dns-alerts-metrics/select-signal.png" alt-text="Screenshot der verfügbaren Metriken auf der Signalauswahlseite.":::

1. Auf der Seite *Signallogik konfigurieren* können Sie den Schwellenwert und die Häufigkeit der Auswertung für die ausgewählte Metrik konfigurieren.

    :::image type="content" source="./media/dns-alerts-metrics/configure-signal-logic.png" alt-text="Screenshot der Konfigurationsseite der Signallogik.":::

1. Um eine Benachrichtigung zu senden oder eine durch die Warnmeldung ausgelöste Aktion aufzurufen, klicken Sie auf die Schaltfläche **Aktionsgruppen hinzufügen**. Auf der Seite *Aktionsgruppen hinzufügen*, wählen Sie **+ Aktionsgruppe erstellen**. Weitere Informationen finden Sie unter [Aktionsgruppen](../azure-monitor/alerts/action-groups.md).

1. Geben Sie einen Namen für eine *Warnungsregel* ein und wählen Sie dann **Warnungsregel erstellen**, um Ihre Konfiguration zu speichern.

    :::image type="content" source="./media/dns-alerts-metrics/create-alert-rule.png" alt-text="Screenshot der Seite &quot;Warnregel erstellen&quot;.":::

Weitere Informationen zum Konfigurieren von Warnungen für Azure Monitor-Metriken finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../azure-monitor/alerts/alerts-metric.md). 

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Azure DNS](dns-overview.md).
