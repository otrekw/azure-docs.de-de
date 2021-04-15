---
title: Problembehandlung bei Azure VPN Gateway mithilfe von Diagnoseprotokollen
description: Problembehandlung bei Azure VPN Gateway mithilfe von Diagnoseprotokollen
services: vpn-gateway
author: stegag
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 03/15/2021
ms.author: stegag
ms.openlocfilehash: 232e084e44696c6aa88a9dd33092c48a96e35f85
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104771997"
---
# <a name="troubleshoot-azure-vpn-gateway-using-diagnostic-logs"></a>Problembehandlung bei Azure VPN Gateway mithilfe von Diagnoseprotokollen

In diesem Artikel werden die für die VPN Gateway-Diagnose verfügbaren Protokolle beschrieben, und es wird erläutert, wie diese zur effizienten Problembehandlung von VPN Gateway-Problemen verwendet werden.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Die folgenden Protokolle sind in Azure verfügbar:

|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**GatewayDiagnosticLog** | Enthält Diagnoseprotokolle für Konfigurationsereignisse, wichtige Änderungen und Wartungsereignisse, die das Gateway betreffen. |
|**TunnelDiagnosticLog** | Enthält Ereignisse, die die Änderung des Tunnelzustands betreffen. Ereignisse im Zusammenhang mit der Herstellung und Trennung der Tunnelverbindung beinhalten ggf. eine Zusammenfassung, in der die Ursache der Zustandsänderung beschrieben wird. |
|**RouteDiagnosticLog** | Enthält Änderungen an statische Routen und BGP-Ereignisse, die am Gateway auftreten. |
|**IKEDiagnosticLog** | Enthält IKE-Kontrollnachrichten und -ereignisse, die am Gateway erfasst werden. |
|**P2SDiagnosticLog** | Enthält Point-to-Site-Kontrollnachrichten und -ereignisse, die am Gateway erfasst werden. |

Beachten Sie, dass in diesen Tabellen mehrere Spalten verfügbar sind. In diesem Artikel werden jedoch nur die wichtigsten dargestellt, um die Protokolliernutzung zu erleichtern.

## <a name="set-up-logging"></a><a name="setup"></a>Einrichten der Protokollierung

Informationen zum Einrichten von Diagnoseprotokollereignissen über Azure VPN Gateway mithilfe von Azure Log Analytics finden Sie unter [Einrichten von Warnungen für Diagnoseprotokollereignisse vom VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log).

## <a name="gatewaydiagnosticlog"></a><a name="GatewayDiagnosticLog"></a>GatewayDiagnosticLog

Konfigurationsänderungen werden in der Tabelle **GatewayDiagnosticLog** überwacht. Es kann einige Minuten dauern, bis die von Ihnen vorgenommen Änderungen in den Protokollen angezeigt werden.

Im Folgenden ist eine Beispielabfrage als Referenz dargestellt.

```
AzureDiagnostics  
| where Category == "GatewayDiagnosticLog"  
| project TimeGenerated, OperationName, Message, Resource, ResourceGroup  
| sort by TimeGenerated asc
```

Diese Abfrage für **GatewayDiagnosticLog** enthält mehrere Spalten.

|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**TimeGenerated** | Zeitstempel der einzelnen Ereignisse in UTC-Zeit.|
|**OperationName** |Das Ereignis, das aufgetreten ist. Dabei kann es sich um *SetGatewayConfiguration, SetConnectionConfiguration, HostMaintenanceEvent, GatewayTenantPrimaryChanged, MigrateCustomerSubscription, GatewayResourceMove, ValidateGatewayConfiguration* handeln.|
|**Meldung** | Details zum Vorgang und Liste mit Ergebnissen (Erfolg/Fehler).|

Das folgende Beispiel zeigt die Aktivität, die beim Anwenden einer neuen Konfiguration aufgezeichnet wurde:

:::image type="content" source="./media/troubleshoot-vpn-with-azure-diagnostics/image-26-set-gateway.png" alt-text="Beispiel für einen SetGateway-Vorgang in GatewayDiagnosticLog":::


Ein SetGatewayConfiguration-Vorgang wird jedes Mal aufgezeichnet, wenn eine Konfiguration auf einem VPN Gateway oder auf einem lokalen Netzwerkgateway geändert wird.
Mithilfe von Querverweisen auf die Ergebnisse aus der Tabelle **GatewayDiagnosticLog** und aus der Tabelle **TunnelDiagnosticLog** können wir ermitteln, ob ein Tunnelverbindungsfehler zur der Zeit aufgetreten ist, als eine Konfiguration geändert oder eine Wartung durchgeführt wurde. Wenn dies der Fall ist, haben wir einen guten Anhaltspunkt für die mögliche Grundursache.

## <a name="tunneldiagnosticlog"></a><a name="TunnelDiagnosticLog"></a>TunnelDiagnosticLog

Die Tabelle **TunnelDiagnosticLog** ist sehr nützlich, um die historischen Konnektivitätsstatus des Tunnels zu überprüfen.

Im Folgenden ist eine Beispielabfrage als Referenz dargestellt.

```
AzureDiagnostics
| where Category == "TunnelDiagnosticLog"
| project TimeGenerated, OperationName, instance_s, Resource, ResourceGroup
| sort by TimeGenerated asc
```

Diese Abfrage für **TunnelDiagnosticLog** enthält mehrere Spalten.


|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**TimeGenerated** | Zeitstempel der einzelnen Ereignisse in UTC-Zeit.|
|**OperationName** | Das Ereignis, das aufgetreten ist. Dabei kann es sich um *TunnelConnected* oder *TunnelDisconnected* handeln.|
| **Instance\_s** | Die Rolleninstanz des Gateways, die das Ereignis ausgelöst hat. Dabei kann es sich entweder um „GatewayTenantWorker\_IN\_0“ oder um „GatewayTenantWorker\_IN\_1“ handeln, also die Namen der beiden Instanzen des Gateways.|
| **Ressource** | Gibt den Namen des VPN-Gateways an. |
| **ResourceGroup** | Gibt die Ressourcengruppe an, in der sich das Gateway befindet.|


Beispielausgabe:

:::image type="content" source="./media/troubleshoot-vpn-with-azure-diagnostics/image-16-tunnel-connected.png" alt-text="Beispiel für das Ereignis „Tunnel verbunden“ in „TunnelDiagnosticLog“":::


**TunnelDiagnosticLog** ist sehr nützlich für die Problembehandlung bei Ereignissen in der Vergangenheit, bei denen es um die unerwartete Trennung von VPN-Verbindungen geht. Als Leichtgewicht bietet diese Tabelle die Möglichkeit, große Zeitbereiche über mehrere Tage mit geringem Aufwand zu analysieren.
Erst nachdem Sie den Zeitstempel einer Trennung identifiziert haben, können Sie die Tabelle **IKEdiagnosticLog** genauer analysieren, um ausführliche Informationen zur Ursache der Trennung zu erhalten, sofern diese mit IPsec zusammenhängen.


Einige Tipps zur Problembehandlung:
- Wenn bei einer Gatewayinstanz ein Trennungsereignis und nach einigen Sekunden bei der **anderen** Gatewayinstanz ein Verbindungsereignis auftritt, liegt ein Gatewayfailover vor. Dieses Verhalten ist normalerweise bei der Wartung einer Gatewayinstanz zu erwarten. Weitere Informationen zu diesem Verhalten finden Sie unter [Informationen zur Redundanz von Azure-VPN-Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable#about-azure-vpn-gateway-redundancy).
- Das gleiche Verhalten lässt sich beobachten, wenn Sie absichtlich eine Gatewayzurücksetzung auf Azure-Seite ausführen, wodurch die aktive Gatewayinstanz neu gestartet wird. Weitere Informationen zu diesem Verhalten finden Sie unter [Zurücksetzen eines VPN-Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-resetgw-classic).
- Wenn bei einer Gatewayinstanz ein Trennungsereignis und nach einigen Sekunden bei **derselben** Gatewayinstanz ein Verbindungsereignis auftritt, liegt möglicherweise eine Netzwerkstörung vor, die ein DPD-Timeout verursacht, oder eine vom lokalen Gerät fälschlicherweise gesendete Trennung.

## <a name="routediagnosticlog"></a><a name="RouteDiagnosticLog"></a>RouteDiagnosticLog

Mit der Tabelle **RouteDiagnosticLog** wird die Aktivität für statisch geänderte Routen oder über BGP empfangene Routen nachverfolgt.

Im Folgenden ist eine Beispielabfrage als Referenz dargestellt.

```
AzureDiagnostics
| where Category == "RouteDiagnosticLog"
| project TimeGenerated, OperationName, Message, Resource, ResourceGroup
```

Diese Abfrage für **RouteDiagnosticLog** enthält mehrere Spalten.

|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**TimeGenerated** | Zeitstempel der einzelnen Ereignisse in UTC-Zeit.|
|**OperationName** | Das Ereignis, das aufgetreten ist. Hierbei kann es sich um *StaticRouteUpdate, BgpRouteUpdate, BgpConnectedEvent, BgpDisconnectedEvent* handeln.|
| **Meldung** | Details zum Vorgang.|

Die Ausgabe enthält nützliche Informationen zu verbundenen/nicht verbundenen BGP-Peers und ausgetauschten Routen.

Beispiel:


:::image type="content" source="./media/troubleshoot-vpn-with-azure-diagnostics/image-31-bgp-route.png" alt-text="Beispiel für BGP-Routenaustauschaktivität in „RouteDiagnosticLog“":::


## <a name="ikediagnosticlog"></a><a name="IKEDiagnosticLog"></a>IKEDiagnosticLog

Die Tabelle **IKEDiagnosticLog** enthält eine ausführliche Debugprotokollierung für IKE/IPsec. Es lohnt sich, sich diese Tabelle bei der Problembehandlung anzusehen, wenn Trennungsereignisse vorliegen oder in VPN-Szenarios Verbindungen nicht hergestellt werden.

Im Folgenden ist eine Beispielabfrage als Referenz dargestellt.

```
AzureDiagnostics  
| where Category == "IKEDiagnosticLog" 
| extend Message1=Message
| parse Message with * "Remote " RemoteIP ":" * "500: Local " LocalIP ":" * "500: " Message2
| extend Event = iif(Message has "SESSION_ID",Message2,Message1)
| project TimeGenerated, RemoteIP, LocalIP, Event, Level 
| sort by TimeGenerated asc
```

Diese Abfrage für **IKEDiagnosticLog** enthält mehrere Spalten.


|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**TimeGenerated** | Zeitstempel der einzelnen Ereignisse in UTC-Zeit.|
| **RemoteIP** | IP-Adresse des lokalen VPN-Geräts. In der Praxis empfiehlt es sich, nach der IP-Adresse des jeweiligen lokalen Geräts zu filtern, falls mehrere vorhanden sind. |
|**LocalIP** | IP-Adresse des VPN Gateways, bei dem die Problembehandlung durchgeführt wird. In der Praxis empfiehlt es sich, nach der IP-Adresse des jeweiligen VPN-Gateways zu filtern, falls im Abonnement mehrere enthalten sind. |
|**Event** | Enthält eine Diagnosemeldung, die für die Problembehandlung nützlich ist. Diese beginnt in der Regel mit einem Schlüsselwort und bezieht sich auf die vom Azure-Gateway ausgeführten Aktionen: **\[SEND\]** gibt ein Ereignis an, das durch ein vom Azure-Gateway gesendetes IPSec-Paket verursacht wurde.  **\[RECEIVED\]** gibt ein Ereignis an, das auf ein von einem lokalen Gerät empfangenes Paket verweist.  **\[LOCAL\]** gibt eine Aktion an, die vom Azure-Gateway lokal durchgeführt wird. |


Die Spalten „RemoteIP“, „LocalIP“ und „Event“ sind in der ursprünglichen Spaltenliste in der AzureDiagnostics-Datenbank übrigens nicht enthalten. Sie wurden der Abfrage durch Analysieren der Ausgabe der Spalte „Message“ hinzugefügt, um die Analyse zu vereinfachen.

Tipps zur Problembehandlung:

- Zum Starten einer IPsec-Aushandlung müssen Sie nach der anfänglichen SA\_INIT-Meldung suchen. Diese Meldung kann von beiden Seiten des Tunnels gesendet werden. Die Seite, von der das erste Paket gesendet wird, heißt in der IPsec-Terminologie „Initiator“, während die andere Seite als „Antwortdienst“ bezeichnet wird. Die erste SA\_INIT-Meldung ist immer eine Meldung, für die rCookie = 0 gilt.

- Wenn beim Einrichten des IPsec-Tunnels ein Fehler auftritt, wird der Vorgang von Azure einige Sekunden wiederholt. Daher ist die Problembehandlung bei einem ausgefallenen VPN in der Tabelle „IKEdiagnosticLog“ sehr bequem, da Sie nicht warten müssen, bis Sie das Problem reproduzieren können. Außerdem ist der Fehler theoretisch bei jeder Wiederholung derselbe, sodass Sie sich jederzeit eine Aushandlung, bei der ein Fehler aufgetreten ist, beispielhaft genauer ansehen können.

- Die SA\_INIT-Meldung enthält die IPSec-Parameter, die der Peer für diese IPsec-Aushandlung verwenden möchte. Im offiziellen Dokument   
[IPsec-/IKE-Standardparameter](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices#ipsec) sind die vom Azure-Gateway unterstützten IPsec-Parameter mit den Standardeinstellungen aufgeführt.


## <a name="p2sdiagnosticlog"></a><a name="P2SDiagnosticLog"></a>P2SDiagnosticLog

**P2SDiagnosticLog** ist die letzte verfügbare Tabelle für die VPN-Diagnose. Mit dieser Tabelle wird die Aktivität für P2S (Point-to-Site) nachverfolgt.

Im Folgenden ist eine Beispielabfrage als Referenz dargestellt.

```
AzureDiagnostics  
| where Category == "P2SDiagnosticLog"  
| project TimeGenerated, OperationName, Message, Resource, ResourceGroup
```

Diese Abfrage für **P2SDiagnosticLog** enthält mehrere Spalten.

|***Name** _ | _ *_Beschreibung_** |
|---        | ---               |
|**TimeGenerated** | Zeitstempel der einzelnen Ereignisse in UTC-Zeit.|
|**OperationName** | Das Ereignis, das aufgetreten ist. Das ist das Ereignis *P2SLogEvent*.|
| **Meldung** | Details zum Vorgang.|

In der Ausgabe werden alle vom Gateway angewendeten Punkt-zu-Standort-Einstellungen sowie die geltenden IPSec-Richtlinien angezeigt.

:::image type="content" source="./media/troubleshoot-vpn-with-azure-diagnostics/image-28-p2s-log-event.png" alt-text="Beispiel einer Punkt-zu-Standort-Verbindung in „P2SDiagnosticLog“":::

Wenn von einem Client eine Punkt-zu-Standort-Verbindung über IKEv2 oder OpenVPN hergestellt wird, werden in der Tabelle die Protokollpaketaktivität, EAP/RADIUS-Konversationen und Ergebnisse (Erfolg/Fehler) nach Benutzer aufgezeichnet.

:::image type="content" source="./media/troubleshoot-vpn-with-azure-diagnostics/image-29-eap.png" alt-text="Beispiel für eine EAP-Authentifizierung in „P2SDiagnosticLog“":::

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von Warnungen für Tunnelressourcenprotokolle finden Sie unter [Einrichten von Warnungen für Ressourcenprotokolle von VPN Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).

