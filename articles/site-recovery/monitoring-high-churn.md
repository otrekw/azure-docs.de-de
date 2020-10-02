---
title: Überwachen von Änderungsmustern auf virtuellen Computern
description: Erfahren Sie, wie Sie Änderungsmuster auf virtuellen Computern überwachen, die mit Azure Site Recovery geschützt werden.
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 09/09/2020
ms.author: sharrai
ms.openlocfilehash: 51f45b5c0b8a2729f64eb376e06875719474cbfc
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663241"
---
# <a name="monitoring-churn-patterns-on-virtual-machines"></a>Überwachen von Änderungsmustern auf virtuellen Computern

Dieser Artikel bietet eine Übersicht über verschiedene Tools, die zur Überwachung von Änderungsmustern auf einem virtuellen Computer verwendet werden können. Mit den richtigen Tools können Sie leicht feststellen, welche Anwendung eine hohe Änderungsrate bewirkt, und dann weitere Maßnahmen für diese Anwendung ergreifen.

## <a name="for-azure-virtual-machines-windows-or-linux"></a>Virtuelle Azure-Computer (Windows oder Linux)

Wenn Ihr Computer in Azure gehostet wird und für die Speicherung einen verwalteten oder nicht verwalteten Datenträger verwendet, können Sie die Leistung anhand von Datenträgermetriken leicht nachverfolgen. Dies ermöglicht es Ihnen, die richtige Datenträgerauswahl entsprechend dem Nutzungsmuster Ihrer Anwendungen zu überwachen und die richtige Auswahl zu treffen. Sie können hiermit auch Warnungen, Diagnosen und eine Buildautomatisierung erstellen. [Weitere Informationen](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/)

Nachdem Sie Ihre Computer mit Azure Site Recovery geschützt haben, können Sie sie mithilfe von Azure Monitor-Protokollen und Log Analytics überwachen. [Weitere Informationen](https://docs.microsoft.com/azure/site-recovery/monitor-log-analytics)

Es gibt auch einige betriebssystemspezifische Tools, die Sie verwenden können.

## <a name="for-windows-machines"></a>Windows-Computer

Wenn Sie über einen Computer verfügen (lokal oder nicht lokal), auf dem das Windows-Betriebssystem ausgeführt wird, stehen noch einige weitere Tools zur Verfügung.

Neben der Überprüfung der Datenträgerauslastung im Task-Manager können Sie jederzeit auf den **Ressourcenmonitor** und den **Systemmonitor** zugreifen. Diese Tools sind auf Windows-Computern bereits vorhanden.

### <a name="resource-monitor"></a>Ressourcenmonitor

Der **Ressourcenmonitor** zeigt Informationen zur Verwendung von Hardware- und Softwareressourcen in Echtzeit an. Zum Ausführen des Ressourcenmonitors auf einem Windows-Computer führen Sie die folgenden Schritte aus:

1. Drücken Sie WIN+R, und geben Sie _resmon_ ein.
1. Sobald das „resmon“-Fenster, d. h. der Ressourcenmonitor, geöffnet wird, wechseln Sie zur Registerkarte „Datenträger“. Die folgende Ansicht wird angezeigt:

    ![Registerkarte „Datenträger“ im Ressourcenmonitor](./media/monitoring-high-churn/resmon-disk-tab.png)

1. Diese Registerkarte muss eine Zeit lang durchgehend überwacht werden, um ein klares Bild zu erhalten. Im Beispiel oben ist zu sehen, dass _wmiprv.exe_ eine hohe Änderungsrate aufweist.

Nachdem Sie die Anwendungen identifiziert haben, die auf Ihrem Computer eine hohe Änderungsrate bewirken, können Sie die notwendigen Maßnahmen ergreifen, um die Änderungsrate im Zusammenhang mit diesen Anwendungen zu beheben.

### <a name="performance-monitor"></a>Systemmonitor

Der **Systemmonitor** überwacht verschiedene Aktivitäten auf einem Computer, z. B. die CPU- oder Speicherauslastung. Zum Ausführen des Systemmonitors auf einem Windows-Computer führen Sie die folgenden Schritte aus:

1. Drücken Sie WIN+R, und geben Sie _perfmon_ ein.
1. Sobald das „perfmon“-Fenster, d. h. der Systemmonitor, geöffnet wird, ist die folgende Ansicht zu sehen:

    ![Systemmonitor – Schritt 1](./media/monitoring-high-churn/perfmon-step1.png)

1. Erweitern Sie auf der rechten Seite den Ordner **Überwachungstools**, und klicken Sie auf „Systemmonitor“. Dadurch wird die folgende Ansicht geöffnet, die Ihnen Echtzeitinformationen zur aktuellen Leistung bietet:

    ![Systemmonitor – Schritt 2](./media/monitoring-high-churn/perfmon-step1.png)

1. In diesem Diagramm wird derzeit nur ein Indikator überwacht. Dies ist die „Prozessorzeit (%)“, wie es in der Tabelle direkt unterhalb des Diagramms angegeben ist. Sie können weitere Elemente zur Überwachung hinzufügen, indem Sie oben im Tool auf das **+** -Symbol klicken.
1. Nachfolgend sehen Sie eine Darstellung des Systemmonitors nach dem Hinzufügen weiterer Indikatoren:

    ![Systemmonitor – Schritt 3](./media/monitoring-high-churn/perfmon-step3.png)

Weitere Informationen zum Systemmonitor finden Sie [hier](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/administration/monitor-use-performance-monitor-collect-event-trace-data).

## <a name="for-linux-machines"></a>Linux-Computer

Wenn Sie über einen Computer verfügen (lokal oder nicht lokal), auf dem das Linux-Betriebssystem ausgeführt wird, stehen noch einige weitere Tools zur Überwachung der Änderungsmuster zur Verfügung.

### <a name="iotop"></a>Iotop

Eines der am häufigsten verwendeten Tools ist _iotop_. Es ist ein Hilfsprogramm zum Anzeigen von Datenträgeraktivitäten in Echtzeit. Es kann die Prozesse, die E/A-Vorgänge ausführen, zusammen mit der jeweils verwendeten Datenträgerbandbreite auflisten.

Öffnen Sie die Eingabeaufforderung, und führen Sie den Befehl `iotop` aus.

### <a name="iostat"></a>IoStat

IoStat ist ein einfaches Tool zum Erfassen und Anzeigen von Statistiken zu Speichergeräten für die Systemeingabe und -ausgabe. Dieses Tool wird häufig verwendet, um Leistungsprobleme von Speichergeräten nachzuverfolgen, einschließlich Geräten, lokalen Datenträgern und Remotedatenträger.

Öffnen Sie die Eingabeaufforderung, und führen Sie den Befehl `iostat` aus.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Überwachung mit [Azure Monitor](monitor-log-analytics.md).
