---
title: Anzeigen und Konfigurieren der DDoS Protection-Telemetrie
description: Erfahren Sie, wie Sie Telemetriedaten zum DDoS-Schutz anzeigen und konfigurieren.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 834339a20e369b3835faf05d069f8d4f77385e18
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2020
ms.locfileid: "96744858"
---
# <a name="view-and-configure-ddos-protection-telemetry"></a>Anzeigen und Konfigurieren der DDoS Protection-Telemetrie

Die Standardversion von Azure DDoS Protection liefert per Analyse von DDoS-Angriffen ausführliche Erkenntnisse zu Angriffen und ermöglicht eine Visualisierung. Kunden, die ihre virtuellen Netzwerke vor DDoS-Angriffen schützen, verfügen über detaillierte Einblicke in den Datenverkehr von Angriffen und die Aktionen, die zur Eindämmung des Angriffs durchgeführt werden. Hierfür werden Berichte zur Angriffsentschärfung und Protokolle zum Verlauf der Entschärfung genutzt. Über Azure Monitor werden umfangreiche Telemetriedaten bereitgestellt. Diese umfassen u. a. detaillierte Metriken während der Dauer eines DDoS-Angriffs. Warnungen können für beliebige durch den DDoS-Schutz verfügbar gemachte Azure Monitor-Metriken konfiguriert werden. Die Protokollierung kann zudem mit [Azure Sentinel](../sentinel/connect-azure-ddos-protection.md), Splunk (Azure Event Hubs), OMS Log Analytics und Azure Storage für die erweiterte Analyse über die Schnittstelle für die Azure Monitor-Diagnose integriert werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren von Warnungen für DDoS-Schutzmetriken
> * Verwenden der DDoS Protection-Telemetrie
> * Anzeigen von DDoS-Entschärfungsrichtlinien
> * Anzeigen der DDoS-Schutzwarnungen im Azure Security Center

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Um die Schritte in diesem Tutorial auszuführen, müssen Sie zunächst einen [Azure DDoS Standard-Schutzplan](manage-ddos-protection.md) erstellen.

## <a name="configure-alerts-for-ddos-protection-metrics"></a>Konfigurieren von Warnungen für DDoS-Schutzmetriken

Mithilfe der Warnungskonfiguration von Azure Monitor können Sie jede der verfügbaren DDoS -Schutzmetriken zum Warnen auswählen, wenn während eines Angriffs eine aktive Entschärfung vorhanden ist. Wenn die Bedingungen erfüllt sind, wird eine E-Mail mit einer Warnung an die angegebene Adresse gesendet:

1. Wählen Sie oben im Portal auf der linken Seite **Alle Dienste** aus.
2. Geben Sie *Monitor* in das Feld **Filter** ein. Wenn **Monitor** in den Ergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Wählen Sie unter **GEMEINSAME DIENSTE** die Option **Metriken** aus.
4. Geben Sie Ihre eigenen Werte bzw. die folgenden Beispielwerte ein, übernehmen Sie die restlichen Standardwerte, und klicken Sie dann auf **OK**:

    |Einstellung                  |Wert                                                                                               |
    |---------                |---------                                                                                           |
    |Name                     | Geben Sie _MyDdosAlert_ ein.                                                                                |
    |Subscription             | Wählen Sie das Abonnement aus, das die öffentliche IP-Adresse enthält, für die Sie Warnungen erhalten möchten.        |
    |Resource group           | Wählen Sie die Ressourcengruppe aus, die die öffentliche IP-Adresse enthält, für die Sie Warnungen erhalten möchten.      |
    |Resource                 | Wählen Sie die öffentliche IP-Adresse aus, die die öffentliche IP-Adresse enthält, für die Sie Warnungen erhalten möchten. DDoS überwacht öffentliche IP-Adressen, die Ressourcen in einem virtuellen Netzwerk zugewiesen sind. Wenn Sie keine Ressourcen mit öffentlichen IP-Adressen im virtuellen Netzwerk besitzen, müssen Sie zunächst eine Ressource mit einer öffentlichen IP-Adresse erstellen. Sie können die öffentliche IP-Adresse aller Ressourcen (einschließlich Azure Load Balancer-Instanzen, bei denen sich die virtuellen Back-End-Computer im virtuellen Netzwerk befinden) überwachen, die über Resource Manager (nicht klassisch) bereitgestellt werden und unter [virtuelles Netzwerk für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network) aufgeführt sind, mit Ausnahme der Ressourcen für Azure App Service-Umgebungen und Azure VPN Gateway. Um mit diesem Tutorial fortzufahren, können Sie schnell einen virtuellen [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-Computer erstellen.                   |
    |Metrik                   | Wählen Sie **Unter DDoS-Angriff oder nicht** aus.                                                                |
    |Schwellenwert                | 1 – **1** bedeutet, dass Sie angegriffen werden. **0** bedeutet, dass Sie nicht angegriffen werden.                         |
    |Zeitraum                   | Wählen Sie einen beliebigen Wert aus.                                                                   |
    |Per E-Mail benachrichtigen         | Aktivieren Sie das Kontrollkästchen.                                                                                 |
    |Weiterer Administrator | Geben Sie Ihre E-Mail-Adresse an, wenn Sie nicht der Besitzer, Mitwirkende oder Leser einer E-Mail für das Abonnement sind. |

    Innerhalb weniger Minuten nach der Erkennung eines Angriffs erhalten Sie eine E-Mail von Azure Monitor-Metriken, die etwa der folgenden Abbildung ähnelt:

    ![Angriffswarnung](./media/manage-ddos-protection/ddos-alert.png)


Um zur Überprüfung Ihrer Warnung einen DDoS-Angriff zu simulieren, lesen Sie [Überprüfen der DDoS-Erkennung](test-through-simulations.md).

Sie können auch mehr über das [Konfigurieren von Webhooks](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und über [Logik-Apps](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für die Erstellung von Warnungen erfahren.

## <a name="use-ddos-protection-telemetry"></a>Verwenden der DDoS Protection-Telemetrie

Die Telemetrie für einen Angriff wird in Echtzeit durch Azure Monitor bereitgestellt. Die Telemetrie ist nur so lange verfügbar, wie eine öffentliche IP-Adresse der Entschärfung unterliegt. Vor oder nach der Entschärfung eines Angriffs werden Ihnen keine Telemetriedaten angezeigt.

1. Wählen Sie oben im Portal auf der linken Seite **Alle Dienste** aus.
2. Geben Sie *Monitor* in das Feld **Filter** ein. Wenn **Monitor** in den Ergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Wählen Sie unter **GEMEINSAME DIENSTE** die Option **Metriken** aus.
4. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, die die öffentliche IP-Adresse enthält, für die Telemetriedaten angezeigt werden sollen.
5. Wählen Sie **Öffentliche IP-Adresse** für **Ressourcentyp** aus, wählen Sie dann die jeweilige öffentliche IP-Adresse aus, für die Telemetriedaten angezeigt werden sollen.
6. Eine Reihe von **verfügbaren Metriken** wird auf der linken Seite des Bildschirms angezeigt. Diese Metriken werden, wenn sie ausgewählt sind, im **Metrikdiagramm von Azure Monitor** auf dem Übersichtsbildschirm grafisch dargestellt.
7. Wählen Sie den **Aggregation** styp als **Max** aus.

Die Metriknamen stellen verschiedene Pakettypen und Bytes im Vergleich zu Paketen mit einem grundlegenden Konstrukt von Tagnamen für jede Metrik dar:

- **Gelöschter Tagname** (Beispiel: **Als DDoS eingehende gelöschte Pakete**): Die Anzahl der durch das DDoS-Schutzsystem gelöschten/bereinigten Pakete.
- **Weitergeleiteter Tagname** (Beispiel: **Weitergeleitete als DDoS eingehende Pakete**): Die Anzahl der durch das DDoS-System an das Ziel-VIP weitergeleiteten Pakete – Datenverkehr, der nicht gefiltert wurde.
- **Kein Tagname** (Beispiel: **Als DDoS eingehende Pakete**): Die gesamte Anzahl von Paketen, die in das Bereinigungssystem gelangt sind, welche die Summe der gelöschten und bereinigten Pakete darstellt.

Diese [Azure Monitor-Warnregel](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20Monitor%20Alert%20-%20DDoS%20Mitigation%20Started) führt eine einfache Abfrage aus, um festzustellen, wann eine aktive DDoS-Entschärfung auftritt. Um zur Überprüfung der Telemetriedaten einen DDoS-Angriff zu simulieren, lesen Sie [Überprüfen der DDoS-Erkennung](test-through-simulations.md). 

## <a name="view-ddos-mitigation-policies"></a>Anzeigen von DDoS-Entschärfungsrichtlinien

Der DDoS-Schutzstandard wendet drei automatisch optimierte Entschärfungsrichtlinien (TCP-SYN, TCP und UDP) für jede öffentliche IP-Adresse der geschützten Ressource in dem virtuellen Netzwerk an, für das DDoS aktiviert ist. Sie können die Richtlinienschwellenwerte anzeigen, indem Sie die Metriken **Eingehende TCP-Pakete zum Auslösen der DDoS-Entschärfung** und **Eingehende UDP-Pakete zum Auslösen der DDoS-Entschärfung** mit dem **Aggregation** styp als „Max“ auswählen, wie in der folgenden Abbildung gezeigt wird:

![Anzeigen von Entschärfungsrichtlinien](./media/manage-ddos-protection/view-mitigation-policies.png)

Die Schwellenwerte der Richtlinien werden automatisch über unsere auf Machine Learning basierende Netzwerkdatenverkehrs-Profilerstellung von Azure konfiguriert. Nur, wenn der Richtlinienschwellenwert überschritten wird, wird die DDoS-Entschärfung für die IP-Adresse durchgeführt, die einem Angriff ausgesetzt ist.

## <a name="view-ddos-protection-alerts-in-azure-security-center"></a>Anzeigen der DDoS-Schutzwarnungen im Azure Security Center

Im Azure Security Center sind zahlreiche [Sicherheitswarnungen](../security-center/security-center-managing-and-responding-alerts.md) vorhanden, die Informationen zum Untersuchen und Beheben von Problemen enthalten. Dieses Feature bietet eine einheitliche Ansicht für die Warnungen, einschließlich Warnungen für DDoS-Angriffe und Aktionen zum zeitnahen Abwehren dieser Angriffe.
Für die Erkennung und Abwehr von DDoS-Angriffen werden insbesondere zwei Warnungen ausgelöst:

- **DDoS Attack detected for Public IP** (DDoS-Angriff für öffentliche IP-Adresse erkannt): Diese Warnung wird generiert, wenn der DDoS-Schutzdienst einen DDoS-Angriff auf eine Ihrer öffentlichen IP-Adressen erkennt.
- **DDoS Attack mitigated for Public IP** (DDoS-Angriff für öffentliche IP-Adresse abgewehrt): Diese Warnung wird generiert, wenn ein Angriff auf eine öffentliche IP-Adresse abgewehrt wurde.
Öffnen Sie **Security Center** im Azure-Portal, um die Warnungen anzuzeigen. Klicken Sie unter **Bedrohungsschutz** auf **Sicherheitswarnungen**. Auf dem folgenden Screenshot sehen Sie ein Beispiel für eine Warnung zu einem DDoS-Angriff.

![DDoS-Warnung im Azure Security Center](./media/manage-ddos-protection/ddos-alert-asc.png)

Die Warnungen enthalten allgemeine Informationen zur angegriffenen öffentlichen IP-Adresse, zum geografischen Raum und zur Bedrohung sowie Schritte zur Behebung.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

- Konfigurieren von Warnungen für DDoS-Schutzmetriken
- Verwenden der DDoS Protection-Telemetrie
- Anzeigen von DDoS-Entschärfungsrichtlinien
- Anzeigen der DDoS-Schutzwarnungen im Azure Security Center

Weitere Informationen zur Konfiguration von Risikominderungsberichten und Datenflussprotokollen finden Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Konfigurieren der Berichte und Datenflussprotokolle zur Entschärfung von DDoS-Angriffen](reports-and-flow-logs.md)