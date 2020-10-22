---
title: 'Azure ExpressRoute: Überwachung, Metriken und Warnungen'
description: Hier erfahren Sie mehr über die Azure ExpressRoute-Überwachung, -Metriken und -Warnungen mithilfe von Azure Monitor, der zentralen Anlaufstelle für alle Metriken, Warnungen und Diagnoseprotokolle in Azure.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 08/25/2020
ms.author: duau
ms.openlocfilehash: d92b5685722b8a37de3945caa1305a76b3cabb8a
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92206236"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute-Überwachung, Metriken und Warnungen

Dieser Artikel macht Sie mit der ExpressRoute-Überwachung, Metriken und Warnungen in Azure Monitor vertraut. Azure Monitor ist die Zentrale für alle Metriken, Warnungen und Diagnoseprotokolle im gesamten Azure-System.
 
>[!NOTE]
>Die Verwendung **klassischer Metriken** wird nicht empfohlen.
>

## <a name="expressroute-metrics"></a>ExpressRoute-Metriken

Um **Metriken** anzuzeigen, navigieren Sie zur Seite *Azure Monitor*, und klicken Sie auf *Metriken*. Wenn Sie **ExpressRoute**-Metriken anzeigen möchten, filtern Sie nach dem Ressourcentyp *ExpressRoute-Leitungen*. Um **Global Reach**-Metriken anzuzeigen, filtern Sie nach dem Ressourcentyp *ExpressRoute-Leitungen*, und wählen Sie eine ExpressRoute-Leitungsressource aus, bei der Global Reach aktiviert ist. Um **ExpressRoute Direct**-Metriken anzuzeigen, filtern Sie nach dem Ressourcentyp *ExpressRoute-Ports*. 

Nach dem Auswählen einer Metrik wird die Standardaggregation angewendet. Optional können Sie die Teilung anwenden, wonach die Metrik mit anderen Dimensionen angezeigt wird.

### <a name="available-metrics"></a>Verfügbare Metriken

|**Metrik**|**Kategorie**|**Dimension(en)**|**Feature(s)**|
| --- | --- | --- | --- |
|ARP-Verfügbarkeit|Verfügbarkeit|<ui><li>Peer (Primärer/Sekundärer ExpressRoute-Router)</ui></li><ui><li> Peeringtyp (Privat/Öffentlich/Microsoft)</ui></li>|ExpressRoute|
|BGP-Verfügbarkeit|Verfügbarkeit|<ui><li> Peer (Primärer/Sekundärer ExpressRoute-Router)</ui></li><ui><li> Peeringtyp</ui></li>|ExpressRoute|
|BitsInPerSecond|Verkehr|<ui><li> Peeringtyp (ExpressRoute)</ui></li><ui><li>Link (ExpressRoute Direct)</ui></li>|<li>ExpressRoute</li><li>ExpressRoute Direct|
|BitsOutPerSecond|Verkehr| <ui><li>Peeringtyp (ExpressRoute)</ui></li><ui><li> Link (ExpressRoute Direct) |<ui><li>ExpressRoute<ui><li>ExpressRoute Direct</ui></li> |
|CPU-Auslastung|Leistung| <ui><li>Instanz</ui></li>|ExpressRoute-Gateway für virtuelle Netzwerke|
|Pakete pro Sekunde|Leistung| <ui><li>Instanz</ui></li>|ExpressRoute-Gateway für virtuelle Netzwerke|
|GlobalReachBitsInPerSecond|Verkehr|<ui><li>Dienstschlüssel der Peeringleitung</ui></li>|Global Reach|
|GlobalReachBitsOutPerSecond|Verkehr|<ui><li>Dienstschlüssel der Peeringleitung</ui></li>|Global Reach|
|AdminState|Physische Konnektivität|Link|ExpressRoute Direct|
|LineProtocol|Physische Konnektivität|Link|ExpressRoute Direct|
|RxLightLevel|Physische Konnektivität|<ui><li>Link</ui></li><ui><li>Bereich</ui></li>|ExpressRoute Direct|
|TxLightLevel|Physische Konnektivität|<ui><li>Link</ui></li><ui><li>Bereich</ui></li>|ExpressRoute Direct|
>[!NOTE]
>Die Verwendung von *GlobalGlobalReachBitsInPerSecond* und *GlobalGlobalReachBitsOutPerSecond* ist nur sichtbar, wenn mindestens eine Global Reach-Verbindung hergestellt wird.
>

## <a name="circuits-metrics"></a>Leitungsmetriken

### <a name="bits-in-and-out---metrics-across-all-peerings"></a>Ein- und ausgehende Bits – Metriken über alle Peerings

Sie können Metriken über alle Peerings hinweg für eine bestimmte ExpressRoute-Leitung anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg" alt-text="Metriken zu Leitungen":::

### <a name="bits-in-and-out---metrics-per-peering"></a>Ein- und ausgehende Bits – Metriken pro Peering

Sie können Metriken für privates, öffentliches und Microsoft-Peering in Bits pro Sekunde anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg" alt-text="Metriken zu Leitungen":::

### <a name="bgp-availability---split-by-peer"></a>BGP-Verfügbarkeit – Aufgeteilt nach Peer  

Sie können die Verfügbarkeit von BGP nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die primäre BGP-Sitzung aufwärts für privates Peering und die zweite BGP-Sitzung abwärts für privates Peering an. 

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg" alt-text="Metriken zu Leitungen":::

### <a name="arp-availability---split-by-peering"></a>ARP-Verfügbarkeit – Aufgeteilt nach Peering  

Sie können die Verfügbarkeit von [ARP](./expressroute-troubleshooting-arp-resource-manager.md) nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die ARP-Sitzung aufwärts für privates Peering auf beiden Peers an, aber vollständig abwärts für Microsoft-Peering über Peerings hinweg. Die Standardaggregation (Durchschnitt) wurde für beide Peers verwendet.  

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg" alt-text="Metriken zu Leitungen":::

## <a name="expressroute-direct-metrics"></a>ExpressRoute Direct-Metriken

### <a name="admin-state---split-by-link"></a>Administratorstatus nach Link

Sie können den Administratorstatus für jeden Link des ExpressRoute Direct-Portpaars anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/adminstate-per-link.jpg" alt-text="Metriken zu Leitungen":::

### <a name="bits-in-per-second---split-by-link"></a>Eingehende Bits pro Sekunde nach Link

Sie können die eingehenden Bits pro Sekunde über beide Links des ExpressRoute Direct-Portpaars anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-in-per-second-per-link.jpg" alt-text="Metriken zu Leitungen":::

### <a name="bits-out-per-second---split-by-link"></a>Ausgehende Bits pro Sekunde nach Link

Sie können auch die ausgehenden Bits pro Sekunde über beide Links des ExpressRoute Direct-Portpaars anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-out-per-second-per-link.jpg" alt-text="Metriken zu Leitungen":::

### <a name="line-protocol---split-by-link"></a>Zeilenprotokoll nach Link

Sie können das Zeilenprotokoll für jeden Link des ExpressRoute Direct-Portpaars anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/line-protocol-per-link.jpg" alt-text="Metriken zu Leitungen":::

### <a name="rx-light-level---split-by-link"></a>Rx-Lichtpegel nach Link

Sie können den Rx-Lichtpegel (vom ExpressRoute Direct-Port **empfangene** Lichtintensität) für jeden Port anzeigen. Fehlerfreie Rx-Lichtpegel liegen in der Regel innerhalb eines Bereichs von -10 bis 0 dBm.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/rxlight-level-per-link.jpg" alt-text="Metriken zu Leitungen":::

### <a name="tx-light-level---split-by-link"></a>Tx-Lichtpegel nach Link

Sie können den Tx-Lichtpegel (vom ExpressRoute Direct-Port **übertragene** Lichtintensität) für jeden Port anzeigen. Fehlerfreie Tx-Lichtpegel liegen in der Regel innerhalb eines Bereichs von -10 bis 0 DBM.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/txlight-level-per-link.jpg" alt-text="Metriken zu Leitungen":::

## <a name="expressroute-virtual-network-gateway-metrics"></a>Metriken zum ExpressRoute-Gateway für virtuelle Netzwerke

### <a name="cpu-utilization---split-instance"></a>CPU-Auslastung – geteilte Instanz

Sie können die CPU-Auslastung der Gatewayinstanzen anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/cpu-split.jpg" alt-text="Metriken zu Leitungen":::

### <a name="packets-per-second---split-by-instance"></a>Pakete pro Sekunde – Unterteilung nach Instanz

Sie können das Gateway durchlaufende Pakete pro Sekunde anzeigen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/pps-split.jpg" alt-text="Metriken zu Leitungen":::

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute-Gatewayverbindungen in Bits pro Sekunde

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erconnections.jpg" alt-text="Metriken zu Leitungen":::

## <a name="alerts-for-expressroute-gateway-connections"></a>Warnungen zu ExpressRoute-Gatewayverbindungen

1. Um Warnungen zu konfigurieren, navigieren Sie zu **Azure Monitor**, und wählen Sie dann **Warnungen** aus.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg" alt-text="Metriken zu Leitungen":::
2. Klicken Sie auf **+Ziel auswählen**, und wählen Sie die ExpressRoute-Gatewayverbindungsressource aus.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg" alt-text="Metriken zu Leitungen":::
3. Definieren Sie die Warnungsdetails.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg" alt-text="Metriken zu Leitungen":::
4. Definieren Sie die Aktionsgruppe, und fügen Sie sie hinzu.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/actiongroup.png" alt-text="Metriken zu Leitungen":::

## <a name="alerts-based-on-each-peering"></a>Warnungen basierend auf dem jeweiligen Peering

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg" alt-text="Metriken zu Leitungen":::

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Konfigurieren von Warnungen für Aktivitätsprotokolle zu Leitungen

In **Warnungskriterien** können Sie **Aktivitätsprotokoll** als Signaltyp und dann das Signal auswählen.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg" alt-text="Metriken zu Leitungen":::

## <a name="additional-metrics-in-log-analytics"></a>Zusätzliche Metriken in Log Analytics

Sie können die ExpressRoute-Metriken auch anzeigen, indem Sie zu Ihrer ExpressRoute-Leitung navigieren und die Registerkarte *Protokolle* auswählen. Für alle Metriken, die Sie abfragen, enthält die Ausgabe die nachfolgenden Spalten.

|**Spalte**|**Typ**|**Beschreibung**|
| --- | --- | --- |
|TimeGrain|Zeichenfolge|PT1M (Metrikwerte werden jede Minute gepusht)|
|Anzahl|real|Normalerweise gleich 2 (jedes MSEE pusht jede Minute einen einzelnen Metrikwert).|
|Minimum|real|Der Mindestwert der beiden Metrikwerte, die von den beiden MSEEs gepusht werden|
|Maximum|real|Der Höchstwert der beiden Metrikwerte, die von den beiden MSEEs gepusht werden|
|Average|real|Entspricht (Minimum + Maximum)/2|
|Gesamt|real|Summe der beiden Metrikwerte aus beiden MSEEs (der Hauptwert, auf den sich bei der abgefragten Metrik konzentriert werden sollte)|
  
## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
* [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-arm.md)
* [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-arm.md)
* [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)