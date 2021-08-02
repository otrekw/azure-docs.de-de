---
title: Integrieren und Bereitstellen nativer Azure-Dienste
description: Hier erfahren Sie, wie Sie native Microsoft Azure-Tools integrieren und bereitstellen, um Ihre Azure VMware Solution-Workloads zu überwachen und zu verwalten.
ms.topic: how-to
ms.date: 06/14/2021
ms.openlocfilehash: e7af5617ee62f451f57daf806f70c7c433e40612
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111758217"
---
# <a name="integrate-and-deploy-azure-native-services"></a>Integrieren und Bereitstellen nativer Azure-Dienste

Mithilfe von nativen Microsoft Azure-Diensten können Sie Ihre virtuellen Computer (VMs) in einer Hybridumgebung (Azure, Azure VMware Solution und lokal) überwachen, verwalten und schützen. Weitere Informationen finden Sie unter [Unterstützte Features für VMs](../security-center/security-center-services.md).

Zu den nativen Azure-Diensten, die Sie in Azure VMware Solution integrieren können, gehören:

- **Log Analytics-Arbeitsbereich:** Jeder Arbeitsbereich verfügt über ein eigenes Datenrepository und eine eigene Konfiguration zum Speichern von Protokolldaten. Datenquellen und Lösungen werden so konfiguriert, dass sie ihre Daten in einem bestimmten Arbeitsbereich speichern. Stellen Sie den Log Analytics-Agent mithilfe der Unterstützung der VM-Erweiterung für Server mit Azure Arc-Unterstützung für neue und vorhandene virtuelle Computer einfach bereit. 
- **Azure Security Center:** Vereinheitlichtes Sicherheitsverwaltungssystem für Infrastrukturen, mit dem der Sicherheitsstatus von Rechenzentren gestärkt wird und ein erweiterter Schutz vor Bedrohungen für Hybridworkloads in der Cloud oder lokal bereitgestellt wird. 
- **Azure Sentinel** ist eine cloudnative SIEM-Lösung (Security Information & Event Management). Es bietet Sicherheitsanalysen, Warnungserkennung und automatisierte Reaktionen auf Bedrohungen in einer Umgebung. Azure Sentinel baut auf einem Log Analytics-Arbeitsbereich auf.
- **Azure Arc** erweitert die Azure-Verwaltung auf jede Infrastruktur, einschließlich Azure VMware Solution, der lokalen Infrastruktur oder anderer Cloudplattformen. 
- Die **Azure-Updateverwaltung** verwaltet Betriebssystemupdates für Ihre Windows- und Linux-Computer in einer Hybridumgebung.
- **Azure Monitor** ist eine umfassende Lösung für das Erfassen von, Analysieren von und Reagieren auf Telemetriedaten aus Ihren cloudbasierten und lokalen Umgebungen. Es ist keine Bereitstellung erforderlich. 

In diesem Artikel integrieren Sie native Azure-Dienste in Ihre private Azure VMware Solution-Cloud. Außerdem erfahren Sie, wie Sie die Tools verwenden, um Ihre VMs während ihres gesamten Lebenszyklus zu verwalten. 


## <a name="enable-azure-update-management"></a>Aktivieren der Azure-Updateverwaltung

Die [Azure-Updateverwaltung](../automation/update-management/overview.md) in Azure Automation verwaltet Betriebssystemupdates für Ihre Windows- und Linux-Computer in einer Hybridumgebung. Sie überwacht die Konformität von Patches und leitet Warnungen zu Abweichungen bei Patches zur Korrektur an Azure Monitor weiter. Die Azure-Updateverwaltung muss eine Verbindung mit Ihrem Log Analytics-Arbeitsbereich herstellen, um gespeicherte Daten zum Bewerten des Status von Updates auf Ihren VMs verwenden zu können.

1. [Erstellen Sie ein Azure Automation-Konto.](../automation/automation-create-standalone-account.md) 

   >[!TIP]
   >Sie können [mit einer ARM-Vorlage (Azure Resource Manager) ein Azure Automation-Konto erstellen](../automation/quickstart-create-automation-account-template.md). Bei Verwendung einer ARM-Vorlage werden im Vergleich zu anderen Bereitstellungsmethoden weniger Schritte benötigt.

1. [Aktivieren Sie die Updateverwaltung über ein Automation-Konto.](../automation/update-management/enable-from-automation-account.md)  Dadurch wird Ihr Log Analytics-Arbeitsbereich mit Ihrem Automation-Konto verknüpft. Außerdem werden Azure- und Nicht-Azure-VMs in der Updateverwaltung aktiviert.

   - Wenn Sie über einen Arbeitsbereich verfügen, wählen Sie **Updateverwaltung** aus. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto und dann **Aktivieren** aus. Die Einrichtung dauert bis zu 15 Minuten.

   - Wenn Sie einen neuen Log Analytics-Arbeitsbereich erstellen möchten, finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/logs/quick-create-workspace.md) eine entsprechende Anleitung. Sie können einen Arbeitsbereich auch über die [CLI](../azure-monitor/logs/quick-create-workspace-cli.md), [PowerShell](../azure-monitor/logs/powershell-workspace-configuration.md) oder eine [Azure Resource Manager-Vorlage](../azure-monitor/logs/resource-manager-workspace.md) erstellen.
 
1. Nachdem Sie die Updateverwaltung aktiviert haben, können Sie [Updates auf virtuellen Computern bereitstellen und die Ergebnisse überprüfen](../automation/update-management/deploy-updates.md). 


## <a name="enable-azure-security-center"></a>Aktivieren von Azure Security Center

Azure Security Center bietet erweiterten Bedrohungsschutz für virtuelle Azure VMware Solution-Computer und lokale virtuelle Computer. Die Lösung bewertet das Sicherheitsrisiko von virtuellen Azure VMware Solution-Computern und gibt bei Bedarf Warnungen aus. Diese Sicherheitswarnungen können zur Behebung an Azure Monitor weitergeleitet werden. 

Azure Security Center bietet viele Features. Dazu gehören:
- Überwachung der Dateiintegrität
- Erkennung dateiloser Angriffe
- Bewertung von Patches für Betriebssysteme 
- Bewertung von Sicherheitsfehlkonfigurationen
- Bewertung von Endpoint Protection

>[!NOTE]
>Azure Security Center ist ein vorkonfiguriertes Tool, für das keine Bereitstellung erforderlich ist, das aber im Azure-Portal aktiviert werden muss. 

Informationen zum Aktivieren von Azure Security Center finden Sie unter [Integrieren von Azure Security Center in Azure VMware Solution](azure-security-integration.md).

## <a name="onboard-vms-to-azure-arc-enabled-servers"></a>Durchführen des Onboardings für virtuelle Computer auf Servern mit Azure Arc-Unterstützung

Azure Arc erweitert die Azure-Verwaltung auf jede Infrastruktur, einschließlich Azure VMware Solution und der lokalen Infrastruktur.  *Server mit Azure Arc-Unterstützung* bieten die Möglichkeit, physische Server und virtuelle Computer unter Windows und Linux zu verwalten, die [außerhalb](../azure-arc/servers/overview.md) von Azure, in Ihrem Unternehmensnetzwerk oder bei einem anderen Cloudanbieter gehostet werden.

Informationen zum Aktivieren von Servern mit Azure Arc-Unterstützung für mehrere virtuelle Windows- oder Linux-Computer finden Sie unter [Verbinden von Hybridcomputern mit Azure im großen Stil](../azure-arc/servers/onboard-service-principal.md).

## <a name="onboard-hybrid-kubernetes-clusters-with-arc-enabled-kubernetes"></a>Durchführen des Onboardings für Kubernetes-Hybridcluster mit Kubernetes mit Arc-Aktivierung

Mit [Kubernetes mit Azure Arc-Unterstützung](../azure-arc/kubernetes/overview.md) können Sie einen Kubernetes-Cluster anfügen, der in Ihrer Azure VMware Solution-Umgebung gehostet wird. 

Weitere Informationen finden Sie unter [Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals (Vorschauversion)](../azure-arc/kubernetes/create-onboarding-service-principal.md).

## <a name="deploy-the-log-analytics-agent"></a>Bereitstellen des Log Analytics-Agents

Sie können die virtuellen Azure VMware Solution-Computer über den Log Analytics-Agent überwachen. Computer, die mit dem Log Analytics-Arbeitsbereich verbunden sind, verwenden den [Log Analytics-Agent](../azure-monitor/agents/log-analytics-agent.md), um Daten zu Änderungen an installierter Software, Microsoft-Diensten, Windows-Registrierung und -Dateien sowie Linux-Daemons auf überwachten Servern zu sammeln. Wenn Daten verfügbar sind, sendet der Agent diese zur Verarbeitung an Azure Monitor-Protokolle. Von Azure Monitor-Protokollen wird Logik auf die empfangenen Daten angewendet, und die Daten werden aufgezeichnet und zu Analysezwecken verfügbar gemacht.

Stellen Sie den Log Analytics-Agent mithilfe von [Servern mit Azure Arc-Unterstützung und Unterstützung der VM-Erweiterung](../azure-arc/servers/manage-vm-extensions.md) bereit.

## <a name="enable-azure-monitor"></a>Aktivieren von Azure Monitor

[Azure Monitor](../azure-monitor/overview.md) ist eine umfassende Lösung für das Sammeln und Analysieren von Telemetriedaten aus Ihren Cloud- und lokalen Umgebungen und das Reagieren auf diese. Vorteile von Azure Monitor: 

   - Nahtlose Überwachung 

   - Bessere Sichtbarkeit der Infrastruktur 

   - Sofortige Benachrichtigungen 

   - Automatische Behebung 

   - Kosteneffizienz 

Sie können Daten aus verschiedenen Quellen erfassen, um sie zu überwachen und zu analysieren. Weitere Informationen finden Sie unter [Quellen von Überwachungsdaten für Azure Monitor](../azure-monitor/agents/data-sources.md).  Sie können auch verschiedene Arten von Daten für Analysen, Visualisierungen und Warnungen sammeln. Weitere Informationen finden Sie unter [Azure Monitor-Datenplattform](../azure-monitor/data-platform.md). 

Sie können die Leistung des Gastbetriebssystems überwachen und Anwendungsabhängigkeiten für virtuelle Azure VMware Solution-Computer oder lokale virtuelle Computer erkennen und zuordnen. Sie können außerdem Warnungsregeln erstellen, um Probleme in Ihrer Umgebung wie hohe Ressourcennutzung, fehlende Patches, wenig Speicherplatz auf dem Datenträger und problematischer Takt Ihrer virtuellen Computer zu ermitteln. Sie können eine automatisierte Reaktion auf erkannte Ereignisse festlegen, indem Sie eine Warnung an die ITSM-Tools (IT-Service-Management) senden. Benachrichtigungen zu erkannten Warnungen können auch per E-Mail gesendet werden.


1. [Entwerfen Ihrer Azure Monitor-Protokollbereitstellung](../azure-monitor/logs/design-logs-deployment.md)

1. [Aktivieren von Azure Monitor für VMs: Übersicht](../azure-monitor/vm/vminsights-enable-overview.md)

1. [Konfigurieren eines Log Analytics-Arbeitsbereichs für Azure Monitor für VMs](../azure-monitor/vm/vminsights-configure-workspace.md)

1. Erstellen Sie Warnungsregeln, um Probleme in Ihrer Umgebung zu identifizieren:
   - [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../azure-monitor/alerts/alerts-metric.md)
   - [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../azure-monitor/alerts/alerts-log.md)
   - [Aktionsregeln](../azure-monitor/alerts/alerts-action-rules.md) zum Festlegen automatisierter Aktionen und Benachrichtigungen
   - [Verbinden von Azure mit ITSM-Tools mithilfe des ITSM-Connectors](../azure-monitor/alerts/itsmc-overview.md)