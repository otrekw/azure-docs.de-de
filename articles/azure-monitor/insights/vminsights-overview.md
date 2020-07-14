---
title: Was ist Azure Monitor for VMs?
description: Übersicht über die Azure Monitor für VMs-Lösung, mit der neben dem automatischen Ermitteln und Zuordnen von Anwendungskomponenten und ihren Abhängigkeiten auch die Integrität und Leistung der virtuellen Azure-Computer überwacht wird.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: f058ce1ae5b7328c6864684994a74f2fd118ca6e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85506989"
---
# <a name="what-is-azure-monitor-for-vms"></a>Was ist Azure Monitor for VMs?

Azure Monitor für VMs überwacht Ihre virtuellen Computer, VM-Skalierungsgruppen und Azure Arc-Computer bedarfsorientiert. Der Dienst analysiert die Leistung und Integrität Ihrer Windows- und Linux-VMs und überwacht deren Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen. Er beinhaltet Unterstützung für die Überwachung von Leistung und Anwendungsabhängigkeiten für virtuelle Computer, die lokal oder bei einem anderen Cloudanbieter gehostet werden. Die folgenden wichtigen Features bieten tief greifende Erkenntnisse:

- **Vordefinierte Diagramme zu Leistungstrends**: Zeigen Kernleistungsmetriken des Gast-VM-Betriebssystems an.

- **Abhängigkeitsdiagramm**: Zeigt die mit der VM verbundenen Komponenten aus verschiedenen Ressourcengruppen und Abonnements an.  

>[!NOTE]
>Wir haben kürzlich Änderungen am Integritätsfeature [angekündigt](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
). Diese Änderungen basieren auf dem Feedback, das wir von unseren Public Preview-Kunden erhalten haben. Aufgrund der Anzahl von Änderungen wird das Integritätsfeature für Neukunden nicht mehr angeboten. Bestandskunden können das Integritätsfeature dagegen weiterhin verwenden. Ausführlichere Informationen finden Sie in den [häufig gestellten Fragen zur allgemeinen Verfügbarkeit](vminsights-ga-release-faq.md).  

Die Integration in Azure Monitor-Protokolle ermöglicht die leistungsstarke Aggregation und Filterung und bietet Azure Monitor für VMs die Möglichkeit zum Analysieren von Datentrends im zeitlichen Verlauf. Sie können diese Daten direkt in einem einzelnen virtuellen Computer anzeigen, oder Sie können eine aggregierte Ansicht Ihrer virtuellen Computer mit Azure Monitor bereitstellen, wobei in der Ansicht der Modus im Arbeitsbereichskontext oder im Azure-Ressourcenkontext unterstützt wird. Weitere Informationen finden Sie in der [Übersicht über Zugriffsmodi](../platform/design-logs-deployment.md#access-mode).

![VM-Insights-Perspektive im Azure-Portal](media/vminsights-overview/vminsights-azmon-directvm.png)

Azure Monitor für VMs kann eine vorhersehbare Leistung und Verfügbarkeit wichtiger Anwendungen bieten. Die Lösung identifiziert Leistungsengpässe und Netzwerkprobleme und kann Ihnen außerdem helfen, zu verstehen, ob ein Problem mit anderen Abhängigkeiten zusammenhängt.  

## <a name="data-usage"></a>Datennutzung

Wenn Sie Azure Monitor für VMs bereitstellen, werden die von Ihren virtuellen Computern gesammelten Daten erfasst und in Azure Monitor gespeichert. Die gesammelten Leistungs- und Abhängigkeitsdaten werden in einem Log Analytics-Arbeitsbereich gespeichert. Basierend auf den Preisen, die auf der [Seite mit der Azure Monitor-Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) veröffentlicht sind, wird Azure Monitor für VMs für Folgendes abgerechnet:

- Die erfassten und gespeicherten Daten.
- Die erstellten Warnungsregeln.
- Die gesendeten Benachrichtigungen. 

Die Protokollgröße unterscheidet sich je nach Zeichenfolgenlänge der Leistungsindikatoren und kann mit der Anzahl der logischen Datenträger und Netzwerkadapter anwachsen, die der VM zugeordnet sind. Wenn Sie bereits über einen Arbeitsbereich verfügen und diese Indikatoren sammeln, erfolgt keine doppelte Belastung. Wenn Sie bereits die Dienstzuordnung verwenden, besteht die einzige für Sie sichtbare Veränderung in den zusätzlichen Verbindungsdaten, die an Azure Monitor gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den Anforderungen und Methoden, die Ihnen beim Überwachen Ihrer virtuellen Computer helfen, finden Sie unter [Bereitstellen von Azure Monitor für VMs](vminsights-enable-overview.md).
