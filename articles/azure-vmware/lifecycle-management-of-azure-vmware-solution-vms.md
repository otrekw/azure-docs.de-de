---
title: Lebenszyklusverwaltung von Azure VMware Solution-VMs
description: Erfahren Sie, wie Sie alle Aspekte des Lebenszyklus Ihrer Azure VMware Solution-VMs mit nativen Tools in Microsoft Azure verwalten.
ms.topic: conceptual
ms.date: 09/11/2020
ms.openlocfilehash: 20948ec088d11468b5750ca89979050965246b58
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89663252"
---
# <a name="lifecycle-management-of-azure-vmware-solution-vms"></a>Lebenszyklusverwaltung von Azure VMware Solution-VMs

Sie können die nativen Tools in Microsoft Azure verwenden, um Ihre virtuellen Computer (VMs) in der Azure-Umgebung zu überwachen und zu verwalten. Sie können damit jedoch auch Ihre virtuellen Computer in Azure VMware Solution und Ihre lokalen VMs überwachen und verwalten. In dieser Übersicht betrachten wir die integrierte Überwachungsarchitektur in Azure und die Möglichkeiten, die Ihnen mit den enthaltenen nativen Tools zum Verwalten Ihrer Azure VMware Solution-VMs über den gesamten Lebenszyklus zur Verfügung stehen.

## <a name="benefits"></a>Vorteile

- Native Azure-Dienste können zum Verwalten Ihrer VMs in einer Hybridumgebung (Azure, Azure VMware Solution und lokal) verwendet werden.
- Integrierte Überwachung und Sichtbarkeit von VMs in Azure, Azure VMware Solution und an Ihrem lokalen Standort.
- Mit der Azure-Updateverwaltung in Azure Automation können Sie Betriebssystemupdates für Ihre Windows- und Linux-Computer verwalten. 
- Azure Security Center ermöglicht einen erweiterten Schutz vor Bedrohungen, einschließlich:
    - Überwachung der Dateiintegrität
    - Dateilose Sicherheitswarnungen
    - Bewertung von Patches für Betriebssysteme
    - Bewertung von Sicherheitsfehlkonfigurationen
    - Bewertung von Endpoint Protection 
- Stellen Sie mithilfe von Azure Arc ganz einfach den Microsoft Monitoring Agent (MMA) für neue VMs bereit. 
- Ihr Log Analytics-Arbeitsbereich in Azure Monitor ermöglicht das Sammeln von Protokollen und Leistungsindikatoren über den MMA oder Erweiterungen. Sammeln Sie Daten und Protokolle an einem einzigen Punkt, und präsentieren Sie diese Daten in verschiedenen nativen Azure-Diensten. 
- Zu den Vorteilen Azure Monitor gehören: 
    - Nahtlose Überwachung 
    - Bessere Sichtbarkeit der Infrastruktur 
    - Sofortige Benachrichtigungen 
    - Automatische Behebung 
    - Kosteneffizienz 

## <a name="integrated-azure-monitoring-architecture"></a>Integrierte Azure-Überwachungsarchitektur

Das folgende Diagramm zeigt die integrierte Überwachungsarchitektur für Azure VMware Solution-VMs.

![Integrierte Azure-Überwachungsarchitektur](media/lifecycle-mgmt-avs-vms/integrated-azure-monitoring-architecture.png)

## <a name="integrating-and-deploying-azure-native-services"></a>Integrieren und Bereitstellen von nativen Azure-Diensten

**Azure Arc** erweitert die Azure-Verwaltung auf jede Infrastruktur, einschließlich Azure VMware Solution, der lokalen Infrastruktur oder anderer Cloudplattformen. Azure Arc kann durch Installieren eines Azure Kubernetes Service-Clusters (AKS) in der Azure VMware Solution-Umgebung bereitgestellt werden. Weitere Informationen finden Sie unter [Herstellen einer Verbindung für einen Azure Arc-fähigen Kubernetes-Cluster](../azure-arc/kubernetes/connect-cluster.md).

Azure VMware Solution-VMs können über den MMA (auch als Log Analytics-Agent oder OMS-Linux-Agent bezeichnet) überwacht werden. Der MMA kann bei der Bereitstellung von VMs über Arc-VM-Lebenszyklusworkflows automatisch installiert werden. Der MMA kann auch beim Bereitstellen von VMs aus einer Vorlage in vCenter installiert werden. Auch hierbei werden die VMs über Arc-Workflows bereitgestellt. Auf allen bereitgestellten virtuellen Azure VMware Solution-VMs kann anschließend der MMA installiert werden, um Protokolle an den Azure Log Analytics-Arbeitsbereich zu senden. Weitere Informationen finden Sie unter [Übersicht über Log Analytics-Agents](../azure-monitor/platform/log-analytics-agent.md).

Der **Log Analytics-Arbeitsbereich** ermöglicht das Sammeln von Protokollen und Leistungsindikatoren über den MMA oder Erweiterungen. Eine Anleitung zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/learn/quick-create-workspace.md).
- Informationen zum Hinzufügen von Windows-VMs zu Ihrem Log Analytics-Arbeitsbereich finden Sie unter [Installieren des Log Analytics-Agents auf Windows-Computern](../azure-monitor/platform/agent-windows.md).
- Informationen zum Hinzufügen von Linux-VMs zu Ihrem Log Analytics-Arbeitsbereich finden Sie unter [Installieren des Log Analytics-Agents auf Linux-Computern](../azure-monitor/platform/agent-linux.md).

Die **Azure-Updateverwaltung** in Azure Automation verwaltet Betriebssystemupdates für Ihre Windows- und Linux-Computer in einer Hybridumgebung. Sie überwacht die Konformität von Patches und leitet Warnungen zu Abweichungen bei Patches zur Korrektur an Azure Monitor weiter. Die Azure-Updateverwaltung muss eine Verbindung mit Ihrem Log Analytics-Arbeitsbereich herstellen, um gespeicherte Daten zum Bewerten des Status von Updates auf Ihren VMs verwenden zu können.
- Zum Hinzufügen von Log Analytics zur Azure-Updateverwaltung müssen Sie zuerst [ein Azure Automation-Konto erstellen](../automation/automation-create-standalone-account.md).
- Informationen zum Verknüpfen Ihres Log Analytics-Arbeitsbereichs mit Ihrem Automation-Konto finden Sie unter [Log Analytics-Arbeitsbereich und Automation-Konto](../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).
- Informationen zum Aktivieren der Azure-Updateverwaltung für Ihre VMs finden Sie unter [Aktivieren der Updateverwaltung über ein Automation-Konto](../automation/update-management/update-mgmt-enable-automation-account.md).
- Nachdem Sie der Azure-Updateverwaltung VMs hinzugefügt haben, können Sie [Updates auf VMs bereitstellen und die Ergebnisse überprüfen](../automation/update-management/update-mgmt-deploy-updates.md). 

**Azure Security Center** bietet lokal und in der Cloud erweiterten Bedrohungsschutz für alle Ihre Hybridworkloads. Die Lösung bewertet das Sicherheitsrisiko von Azure VMware Solution-VMs und gibt bei Bedarf Warnungen aus. Diese Sicherheitswarnungen können zur Behebung an Azure Monitor weitergeleitet werden.
- Azure Security Center muss nicht bereitgestellt werden. Weitere Informationen finden Sie in der Liste der [unterstützten Features für virtuelle Computer](../security-center/security-center-services.md).
- Informationen zum Hinzufügen von Azure VMware Solution-VMs und von Nicht-Azure-VMs in Azure Security Center finden Sie unter [Einbinden von Windows-Computern in Azure Security Center](../security-center/quick-onboard-windows-computer.md) und [Einbinden von Linux-Computern in Azure Security Center](../security-center/quick-onboard-linux-computer.md).
- Nach dem Hinzufügen von VMs analysiert Azure Security Center den Sicherheitsstatus der Ressourcen, um mögliche Sicherheitsrisiken zu identifizieren. Außerdem werden auf der Registerkarte „Übersicht“ Empfehlungen bereitgestellt. Weitere Informationen finden Sie unter [Sicherheitsempfehlungen in Azure Security Center anzeigen](../security-center/security-center-recommendations.md).
- Sie können in Azure Security Center Sicherheitsrichtlinien definieren. Informationen zum Konfigurieren Ihrer Sicherheitsrichtlinien finden Sie unter [Arbeiten mit Sicherheitsrichtlinien](../security-center/tutorial-security-policy.md).

**Azure Monitor** ist eine umfassende Lösung für das Sammeln und Analysieren von Telemetriedaten aus Ihren Cloud- und lokalen Umgebungen und das Reagieren auf diese. Es ist keine Bereitstellung erforderlich.
- Mit Azure Monitor können Sie Daten aus verschiedenen Quellen erfassen, um sie zu überwachen und zu analysieren. Weitere Informationen finden Sie unter [Quellen von Überwachungsdaten für Azure Monitor](../azure-monitor/platform/data-sources.md).
- Sie können auch verschiedene Arten von Daten für Analysen, Visualisierungen und Warnungen sammeln. Weitere Informationen finden Sie unter [Azure Monitor-Datenplattform](../azure-monitor/platform/data-platform.md).
- Informationen zum Konfigurieren von Azure Monitor mit Ihrem Log Analytics-Arbeitsbereich finden Sie unter [Konfigurieren eines Log Analytics-Arbeitsbereichs für Azure Monitor für VMs](../azure-monitor/insights/vminsights-configure-workspace.md).
- Sie können Warnungsregeln erstellen, um Probleme in Ihrer Umgebung wie hohe Ressourcennutzung, fehlende Patches, wenig Speicherplatz auf dem Datenträger und Takt Ihrer VMs zu ermitteln. Sie können auch eine automatisierte Antwort auf erkannte Ereignisse festlegen, indem Sie eine Warnung an die ITSM-Tools (IT-Service-Management) senden. Benachrichtigungen zu erkannten Warnungen können auch per E-Mail gesendet werden. Informationen zum Erstellen solcher Regeln finden Sie unter:
    - [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../azure-monitor/platform/alerts-metric.md)
    - [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../azure-monitor/platform/alerts-log.md)
    - [Aktionsregeln](../azure-monitor/platform/alerts-action-rules.md) zum Festlegen automatisierter Aktionen und Benachrichtigungen
    - [Verbinden von Azure mit ITSM-Tools mithilfe des ITSM-Connectors](../azure-monitor/platform/itsmc-overview.md)

**Azure-PaaS (Platform as a Service)** ist eine Entwicklungs- und Bereitstellungsumgebung in der Cloud mit Ressourcen für das Bereitstellen cloudbasierter Anwendungen. Beispielsweise können Sie Azure SQL-Datenbank in Ihre Azure VMware Solution-VMs integrieren. SQL-Warnungen können dann mit Azure VMware Solution-VM-Warnungen verknüpft werden. Nehmen Sie beispielsweise an, dass sich der SQL-Datenbank-Abschnitt Ihrer Anwendung in Azure-PaaS befindet und die Webanwendungsebene derselben Anwendung auf Ihren Azure VMware Solution-VMs gehostet wird. Datenbankwarnungen können dann mit Webanwendungswarnungen verknüpft werden. Die Problembehandlung wird durch eine einzige integrierte Sichtbarkeit für Azure, Azure VMware Solution und lokale VMs vereinfacht.
