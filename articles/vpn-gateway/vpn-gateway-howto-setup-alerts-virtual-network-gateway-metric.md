---
title: Einrichten von Warnungen zu Azure VPN Gateway-Metriken
description: Hier erfahren Sie, wie Sie das Azure-Portal verwenden, um Azure Monitor-Warnungen basierend auf Metriken für VPN-Gateways virtueller Netzwerke einzurichten.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: alzam
ms.openlocfilehash: 05fbc5675d6ee3b6720d9db9e07e7010cf1d9172
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89435656"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>Einrichten von Warnungen zu VPN Gateway-Metriken

Dieser Artikel hilft Ihnen beim Einrichten von Warnungen zu den Azure VPN Gateway-Metriken. Azure Monitor bietet die Möglichkeit, Warnungen für Azure-Ressourcen einzurichten. Sie können Warnungen für virtuelle Netzwerkgateways vom Typ „VPN“ einrichten.


|**Metrik**   | **Einheit** | **Granularität** | **Beschreibung** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bytes/s  | 5 Minuten| Durchschnittliche kombinierte Bandbreitennutzung aller Site-to-Site-Verbindungen für das Gateway.     |
|**P2SBandwidth**| Bytes/s  | 1 Minute  | Durchschnittliche kombinierte Bandbreitennutzung aller Point-to-Site-Verbindungen für das Gateway.    |
|**P2SConnectionCount**| Anzahl  | 1 Minute  | Die Anzahl der Point-to-Site-Verbindungen für das Gateway.   |
|**TunnelAverageBandwidth** | Bytes/s    | 5 Minuten  | Durchschnittliche Bandbreitennutzung der für das Gateway erstellten Tunnel. |
|**TunnelEgressBytes** | Byte | 5 Minuten | Ausgehender Datenverkehr in Tunneln, die für das Gateway erstellt wurden.   |
|**TunnelEgressPackets** | Anzahl | 5 Minuten | Anzahl der ausgehenden Pakete in Tunneln, die für das Gateway erstellt wurden.   |
|**TunnelEgressPacketDropTSMismatch** | Anzahl | 5 Minuten | Anzahl der ausgehenden Pakete, die in Tunneln aufgrund eines Konflikts beim Datenverkehrsselektor verworfen werden. |
|**TunnelIngressBytes** | Byte | 5 Minuten | Eingehender Datenverkehr in Tunneln, die für das Gateway erstellt wurden.   |
|**TunnelIngressPackets** | Anzahl | 5 Minuten | Anzahl der eingehenden Pakete in Tunneln, die für das Gateway erstellt wurden.   |
|**TunnelIngressPacketDropTSMismatch** | Anzahl | 5 Minuten | Anzahl der eingehenden Pakete, die in Tunneln aufgrund eines Konflikts beim Datenverkehrsselektor verworfen werden. |


## <a name="set-up-azure-monitor-alerts-based-on-metrics-by-using-the-azure-portal"></a><a name="setup"></a>Einrichten von Azure Monitor-Warnungen auf Basis von Metriken mithilfe des Azure-Portals

Die folgenden Beispielschritte erstellen für Folgendes eine Warnung für ein Gateway:

- **Metrik:** TunnelAverageBandwidth
- **Bedingung:** Bandbreite > 10 Bytes/Sekunde
- **Fenster:** 5 Minuten
- **Warnungsaktion:** Email



1. Wechseln Sie zur virtuellen Netzwerkgatewayressource, und wählen Sie **Warnungen** auf der Registerkarte **Überwachung** aus. Erstellen Sie dann eine neue Warnungsregel, oder bearbeiten Sie eine bestehende Warnungsregel.

   ![Optionen für die Erstellung einer Warnungsregel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Erstellen")

2. Wählen Sie Ihr VPN-Gateway als Ressource aus.

   ![Die Schaltfläche „Auswählen“ und das VPN-Gateway in der Liste der Ressourcen](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "Select")

3. Wählen Sie eine Metrik aus, die für die Warnung konfiguriert werden soll.

   ![Ausgewählte Metrik in der Metrikliste](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "Select")
4. Konfigurieren Sie die Signallogik. Es gibt drei Komponenten:

    a. **Dimensions** (Dimensionen): Wenn die Metrik über Dimensionen verfügt, können Sie bestimmte Dimensionswerte auswählen, sodass die Warnung nur Daten dieser Dimension auswertet. Diese sind optional.

    b. **Bedingung:** Dies ist der Vorgang zum Auswerten des Metrikwerts.

    c. **Time**: Geben Sie die Granularität der metrischen Daten und den Zeitraum für die Auswertung der Warnung an.

   ![Details zur Konfiguration der Signallogik](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "Select")

5. Wählen Sie **Warnungsregeln verwalten** aus, um die konfigurierten Regeln anzuzeigen.

   ![Schaltfläche zum Verwalten von Warnungsregeln](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "Select")

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von Warnungen für Tunnelressourcenprotokolle finden Sie unter [Einrichten von Warnungen für Ressourcenprotokolle von VPN Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
