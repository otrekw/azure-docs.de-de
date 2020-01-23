---
title: 'Azure-VPN Gateway: Konfigurieren von Warnungen für Diagnoseprotokollereignisse'
description: Schritte zum Konfigurieren von Warnungen für Diagnoseprotokollereignisse vom VPN Gateway
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: alzam
ms.openlocfilehash: eb8f0204771b204af740c4ddc8e359499520a012
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045933"
---
# <a name="set-up-alerts-on-diagnostic-log-events-from-vpn-gateway"></a>Einrichten von Warnungen für Diagnoseprotokollereignisse vom VPN Gateway

In diesem Artikel erfahren Sie, wie Sie mit Azure Log Analytics Warnungen einrichten, die auf Diagnoseprotokollereignissen von Azure VPN Gateway basieren. 

Die folgenden Protokolle sind in Azure verfügbar:

|***Name*** | ***Beschreibung*** |
|---        | ---               |
|GatewayDiagnosticLog | Enthält Diagnoseprotokolle für Konfigurationsereignisse, wichtige Änderungen und Wartungsereignisse, die das Gateway betreffen. |
|TunnelDiagnosticLog | Enthält Ereignisse, die die Änderung des Tunnelzustands betreffen. Ereignisse im Zusammenhang mit der Herstellung und Trennung der Tunnelverbindung beinhalten ggf. eine Zusammenfassung, in der die Ursache der Zustandsänderung beschrieben wird. |
|RouteDiagnosticLog | Enthält Änderungen an statische Routen und BGP-Ereignisse, die am Gateway auftreten. |
|IKEDiagnosticLog | Enthält IKE-Kontrollnachrichten und -ereignisse, die am Gateway erfasst werden. |
|P2SDiagnosticLog | Enthält Point-to-Site-Kontrollnachrichten und -ereignisse, die am Gateway erfasst werden. |

## <a name="setup"></a>Einrichten von Warnungen

Die folgenden Beispielschritte erstellen eine Warnung für ein Trennungsereignis, das einen Site-to-Site-VPN-Tunnel umfasst:


1. Suchen Sie im Azure-Portal unter **Alle Dienste** nach **Log Analytics**, und wählen Sie **Log Analytics-Arbeitsbereiche** aus.

   ![Auswahl für den Wechsel zu Log Analytics-Arbeitsbereichen](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert0.png "Erstellen")

2. Wählen Sie **Erstellen** auf der Seite **Log Analytics** aus.

   ![Log Analytics-Seite mit Schaltfläche „Erstellen“](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert1.png  "Select")

3. Wählen Sie **Neu erstellen** aus und geben Sie die Details ein.

   ![Details zum Erstellen eines Log Analytics-Arbeitsbereichs](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert2.png  "Select")

4. Suchen Sie Ihr VPN Gateway auf dem Blatt **Überwachen** > **Diagnoseeinstellungen**.

   ![Auswahl zum Suchen des VPN-Gateways in den Diagnoseeinstellungen](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert3.png  "Select")

5. Doppelklicken Sie zum Aktivieren der Diagnose auf das Gateway, und wählen Sie dann **Diagnose aktivieren** aus.

   ![Auswahl zum Aktivieren der Diagnose](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert4.png  "Select")

6. Geben Sie die Details ein und stellen Sie sicher, dass **An Log Analytics senden** und **TunnelDiagnosticLog** ausgewählt sind. Wählen Sie den Log Analytics-Arbeitsbereich aus, den Sie in Schritt 3 erstellt haben.

   ![Aktivierte Kontrollkästchen](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert5.png  "Select")

   > [!NOTE]
   > Es kann einige Stunden dauern, bis die Daten erstmalig angezeigt werden.

7. Wechseln Sie zur Übersicht für die virtuelle Netzwerkgatewayressource, und wählen Sie **Warnungen** auf der Registerkarte **Überwachung** aus. Erstellen Sie dann eine neue Warnungsregel, oder bearbeiten Sie eine bestehende Warnungsregel.

   ![Auswahl zum Erstellen einer neuen Warnungsregel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Select")

   ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert6.png  "Select")
8. Wählen Sie den Log Analytics-Arbeitsbereich und die Ressource aus.

   ![Auswahl für Arbeitsbereich und Ressource](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert7.png  "Select")

9. Wählen Sie **Benutzerdefinierte Protokollsuche** als Signallogik unter **Bedingung hinzufügen** aus.

   ![Auswahl für eine benutzerdefinierte Protokollsuche](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert8.png  "Select")

10. Geben Sie im Textfeld **Suchabfrage** die folgende Abfrage ein. Ersetzen Sie die Werte in „<>“ und „TimeGenerated“ entsprechend.

    ```
    AzureDiagnostics
    | where Category == "TunnelDiagnosticLog"
    | where _ResourceId == tolower("<RESOURCEID OF GATEWAY>")
    | where TimeGenerated > ago(5m) 
    | where remoteIP_s == "<REMOTE IP OF TUNNEL>"
    | where status_s == "Disconnected"
    | project TimeGenerated, OperationName, instance_s, Resource, ResourceGroup, _ResourceId 
    | sort by TimeGenerated asc
    ```

    Legen Sie den Schwellwert auf 0 fest und wählen Sie **Fertig** aus.

    ![Eingeben einer Abfrage und Auswählen eines Schwellenwerts](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert9.png  "Select")

11. Wählen Sie auf der Seite **Regel erstellen** **Neu erstellen** unter dem Abschnitt **AKTIONSGRUPPEN** aus. Geben Sie die Details ein und wählen Sie **OK** aus.

    ![Details für eine neue Aktionsgruppe](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert10.png  "Select")

12. Geben Sie auf der Seite **Regel erstellen** die Details für **Aktionen anpassen** ein und stellen Sie sicher, dass der richtige Name im Abschnitt **AKTIONSGRUPPENNAME** angezeigt wird. Wählen Sie **Warnungsregel erstellen** aus, um die Regel zu erstellen.

    ![Auswahl zum Erstellen einer Regel](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log/log-alert11.png  "Select")

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von Warnungen zu Tunnelmetriken finden Sie unter [Einrichten von Warnungen zu VPN Gateway-Metriken](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).
