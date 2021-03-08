---
title: Planen einer Bereitstellung von Servern mit Azure Arc-Unterstützung im großen Umfang
description: Erfahren Sie, wie Sie eine große Anzahl von Computern für Server mit Azure Arc-Unterstützung aktivieren, um die Konfiguration wichtiger Sicherheits-, Verwaltungs- und Überwachungsfunktionen in Azure zu vereinfachen.
ms.date: 02/23/2021
ms.topic: conceptual
ms.openlocfilehash: fd02e7c0b4d65efde13fbc428a15d60adab174d4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692868"
---
# <a name="planing-for-an-at-scale-deployment-of-azure-arc-enabled-servers"></a>Planen einer Bereitstellung von Servern mit Azure Arc-Unterstützung im großen Umfang

Die Bereitstellung eines IT-Infrastrukturdiensts oder einer Geschäftsanwendung ist für jedes Unternehmen eine Herausforderung. Damit dies erfolgreich umgesetzt werden kann und dabei keine unangenehmen Überraschungen und unerwartete Kosten auftreten, müssen Sie gründlich planen, sodass Sie bestmöglich vorbereitet sind. Um die Bereitstellung von Servern mit Azure Arc-Unterstützung im großen Umfang zu planen, sollten Sie die Entwurfs- und Bereitstellungskriterien abdecken, die erfüllt sein müssen, damit die Aufgaben zur Unterstützung einer Bereitstellung im großen Umfang erfolgreich abgeschlossen werden können.

Damit die Bereitstellung problemlos fortgesetzt werden kann, sollte Ihr Plan folgende Punkte deutlich hervorheben:

* Rollen und Zuständigkeiten.
* Inventar physischer Server oder virtueller Computer, um zu überprüfen, ob sie die Netzwerk- und Systemanforderungen erfüllen.
* Die Fertigkeiten und Schulungen, die erforderlich sind, um eine erfolgreiche Bereitstellung und laufende Verwaltung zu ermöglichen.
* Akzeptanzkriterien und wie Sie den Erfolg verfolgen.
* Tools oder Methoden, die zur Automatisierung der Bereitstellungen verwendet werden sollen.
* Identifizierte Risiken und Entschärfungspläne, um Verzögerungen, Unterbrechungen usw. zu vermeiden.
* Vermeiden von Störungen während der Bereitstellung.
* Was ist der Eskalationspfad, wenn ein erhebliches Problem auftritt?

Dieser Artikel soll sicherstellen, dass Sie für eine erfolgreiche Bereitstellung von Servern mit Azure Arc-Unterstützung auf mehreren physischen Produktionsservern oder virtuellen Computern in Ihrer Umgebung vorbereitet sind.

## <a name="prerequisites"></a>Voraussetzungen

* Auf Ihren Computern wird ein [unterstütztes Betriebssystem](agent-overview.md#supported-operating-systems) für den Connected Machine-Agent ausgeführt.
* Ihre Computer sind von Ihrem lokalen Netzwerk oder einer anderen Cloudumgebung aus entweder direkt oder über einen Proxyserver mit Ressourcen in Azure verbunden.
* Zum Installieren und Konfigurieren des Connected Machine-Agents der Server mit Arc-Unterstützung ist ein Konto mit erhöhten Rechten (d. h. Administrator oder Root-Benutzer) auf den Computern vorhanden.
* Zum Durchführen des Onboardings von Computern müssen Sie der Rolle **Onboarding verbundener Azure-Computer** angehören.
* Zum Lesen, Ändern oder Löschen eines Computers müssen Sie der Rolle **Ressourcenadministrator für Azure Connected Machine** angehören.

## <a name="pilot"></a>Pilotphase

Vor der Bereitstellung auf allen Produktionscomputern sollten Sie diesen Bereitstellungsprozess zunächst auswerten, bevor Sie ihn in Ihrer Umgebung umfassend anwenden. Identifizieren Sie für eine Pilotphase eine repräsentative Stichprobe von Computern, von denen nicht die Fähigkeit Ihres Unternehmens abhängt, den Geschäftsbetrieb aufrechtzuerhalten. Sie sollten sicher sein, dass Sie genügend Zeit für die Durchführung der Pilotphase und die Bewertung seiner Auswirkungen haben: Wir empfehlen mindestens 30 Tage.

Richten Sie einen formalen Plan ein, der den Umfang und die Details der Pilotphase beschreibt. Im folgenden Beispiel erfahren Sie, was ein Plan enthalten sollte, um Ihnen den Einstieg zu erleichtern.

* Ziele: Beschreibt die geschäftlichen und technischen Anstöße, die zu der Entscheidung geführt haben, dass eine Pilotphase notwendig ist.
* Auswahlkriterien: Gibt die Kriterien zur Auswahl der Aspekte der Lösung an, die eine Pilotphase zeigen sollte.
* Umfang: Beschreibt den Umfang der Pilotphase, der die Lösungskomponenten, den erwarteten Zeitplan, die Dauer der Pilotphase und die Anzahl der Zielcomputer umfasst, aber nicht darauf beschränkt ist.
* Erfolgskriterien und Metriken: Definiert die Erfolgskriterien der Pilotphase und bestimmte Measures, die zum Ermitteln des Erfolgsniveaus verwendet werden.
* Schulungsplan: Beschreibt den Plan für die Schulung von System Engineers, Administratoren usw., die im Rahmen der Pilotphase neu bei Azure und Azure-Diensten sind.
* Übergangsplan: Beschreibt die Strategie und die Kriterien, die für den Übergang von der Pilotphase zur Produktion verwendet werden.
* Rollback: Beschreibt die Verfahren für das Rollback einer Pilotphase in den Zustand vor der Bereitstellung.
* Risiken: Listet alle identifizierten Risiken für die Durchführung der Pilotphase und in Verbindung mit der Produktionsbereitstellung auf.

## <a name="phase-1-build-a-foundation"></a>Phase 1: Erstellen eines Fundaments

In dieser Phase aktivieren System Engineers oder Administratoren die Kernfunktionen in den Azure-Abonnements ihrer Organisationen, um die Grundlage zu schaffen, bevor sie ihre Computer für die Verwaltung durch Server mit Arc-Unterstützung und andere Azure-Dienste aktivieren.

|Aufgabe |Detail |Duration |
|-----|-------|---------|
| [Erstellen einer Ressourcengruppe](../../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) | Eine dedizierte Ressourcengruppe, um nur Server mit Arc-Unterstützung einzubeziehen und die Verwaltung und Überwachung dieser Ressourcen zu zentralisieren. | Eine Stunde |
| Anwenden von [Tags](../../azure-resource-manager/management/tag-resources.md) zur Unterstützung der Organisation von Computern. | Bewerten und Entwickeln einer IT-ausgerichteten [Markierungsstrategie](/cloud-adoption-framework/decision-guides/resource-tagging/), mit der die Komplexität der Verwaltung Ihrer Server mit Arc-Unterstützung reduziert und Verwaltungsentscheidungen vereinfacht werden können. | Ein Tag |
| Entwerfen und Bereitstellen von [Azure Monitor-Protokollen](../../azure-monitor/logs/data-platform-logs.md) | Bewerten von [Entwurfs- und Bereitstellungsüberlegungen](../../azure-monitor/logs/design-logs-deployment.md), um zu bestimmen, ob Ihre Organisation einen vorhandenen Log Analytics-Arbeitsbereich verwenden oder einen anderen implementieren sollte, um gesammelte Protokolldaten von Hybridservern und -computern zu speichern.<sup>1</sup> | Ein Tag |
| [Entwickeln eines Azure Policy](../../governance/policy/overview.md)-Governanceplans | Bestimmen Sie, wie Sie die Governance von Hybridservern und -computern im Abonnement- oder Ressourcengruppenumfang mit Azure Policy implementieren. | Ein Tag |
| Konfigurieren der [rollenbasierten Zugriffssteuerung](../../role-based-access-control/overview.md) (Role Based Access Control, RBAC) | Entwickeln Sie einen Zugriffsplan, um zu steuern, wer Zugriff auf die Verwaltung von Servern mit Arc-Unterstützung hat, und wer Daten aus anderen Azure-Diensten und -Lösungen anzeigen kann. | Ein Tag |
| Identifizieren von Computern mit bereits installiertem Log Analytics-Agent | Führen Sie die folgende Protokollabfrage in [Log Analytics](../../azure-monitor/logs/log-analytics-overview.md) durch, um die Konvertierung vorhandener Log Analytics-Agent-Bereitstellungen in einem von der Erweiterung verwalteten Agent zu unterstützen:<br> Heartbeat <br> &#124;, wobei TimeGenerated > ago(30d) <br> &#124;, wobei ResourceType == "machines" und (ComputerEnvironment == "Non-Azure") <br> &#124; zusammenfassen nach Computer, ResourceProvider, ResourceType, Computer Environment | Eine Stunde |

<sup>1</sup> Ein wichtiger Aspekt bei der Bewertung des Entwurfs Ihres Log Analytics-Arbeitsbereichs ist die Integration in Azure Automation zur Unterstützung der Funktionen „Updateverwaltung“ und „Änderungsnachverfolgung und Bestand“ sowie Azure Security Center und Azure Sentinel. Wenn Ihre Organisation bereits über ein Automation-Konto verfügt und dessen Verwaltungsfeatures aktiviert hat, die mit einem Log Analytics-Arbeitsbereich verknüpft sind, bewerten Sie, ob Sie Verwaltungsvorgänge zentralisieren und optimieren und die Kosten minimieren können, indem Sie diese vorhandenen Ressourcen nutzen, um zum Vergleich ein doppeltes Konto, einen doppelten Arbeitsbereich usw. zu erstellen.

## <a name="phase-2-deploy-arc-enabled-servers"></a>Phase 2: Bereitstellen von Servern mit Arc-Unterstützung

Als Nächstes ergänzen wir die in Phase 1 festgelegte Grundlage, indem wir die Bereitstellung vorbereiten und die Installation des Agents durchführen.

|Aufgabe |Detail |Duration |
|-----|-------|---------|
| Herunterladen des vordefinierten Installationsskripts | Überprüfen Sie das vordefinierte Installationsskript für die Bereitstellung des Connected Machine-Agents im großen Umfang, um Ihre Anforderungen der automatischen Bereitstellung zu unterstützen, und passen Sie es an.<br><br> Beispiel für das Onboarding von Ressourcen im großen Umfang:<br><br> * [Grundlegendes Skript für die Bereitstellung im großen Umfang](servers/onboard-service-principal.md)<br><br> * [Onboarding von VMware-vSphere-Windows Server-VMs im großen Umfang](https://github.com/microsoft/azure_arc/blob/master/azure_arc_servers_jumpstart/docs/vmware_scaled_powercli_win.md)<br><br> * [Onboarding von VMware-vSphere-Linux-VMs im großen Umfang](https://github.com/microsoft/azure_arc/blob/master/azure_arc_servers_jumpstart/docs/vmware_scaled_powercli_linux.md)<br><br> * [Onboarding von AWS EC2-Instanzen mithilfe von Ansible im großen Umfang](https://github.com/microsoft/azure_arc/blob/master/azure_arc_servers_jumpstart/docs/aws_scale_ansible.md)<br><br> * [Bereitstellung im großen Umfang mit PowerShell-Remoting](https://docs.microsoft.com/azure/azure-arc/servers/onboard-powershell) (nur Windows)| Ein oder mehrere Tage, abhängig von den Anforderungen, Organisationsprozessen (z. B. Änderungs- und Release Management) und verwendeter Automatisierungsmethode. |
| [Erstellen eines Dienstprinzipals](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) |Erstellen Sie einen Dienstprinzipal, um Computer mit Azure PowerShell oder über das Portal nicht-interaktiv zu verbinden.| Eine Stunde |
| Bereitstellen des Connected Machine-Agents für Ihre Zielserver und -computer |Stellen Sie die Skripts mit dem Automatisierungstool auf Ihren Servern bereit, und stellen Sie deren Verbindung mit Azure her.| Ein oder mehrere Tage, abhängig von Ihrem Freigabeplan und je nachdem, ob ein Rollout in Phasen stattfindet. |

## <a name="phase-3-manage-and-operate"></a>Phase 3: Verwaltung und Betrieb

In Phase 3 aktivieren Administratoren oder System Engineers die Automatisierung manueller Aufgaben für Verwaltung und Betrieb des Connected Machine-Agents und des Computers während ihres Lebenszyklus.

|Aufgabe |Detail |Duration |
|-----|-------|---------|
|Erstellen einer Resource Health-Warnung |Wenn ein Server länger als 15 Minuten keine Heartbeats an Azure sendet, kann dies bedeuten, dass er offline ist, die Netzwerkverbindung blockiert wurde oder der Agent nicht ausgeführt wird. Entwickeln Sie einen Plan, wie Sie auf diese Vorfälle reagieren und sie untersuchen, und verwenden Sie [Resource Health-Warnungen](../..//service-health/resource-health-alert-monitor-guide.md), um benachrichtigt zu werden, wenn sie beginnen.<br><br> Geben Sie Folgendes an, wenn Sie die Warnung konfigurieren:<br> **Ressourcentyp** = **Server mit Azure Arc-Unterstützung**<br> **Aktueller Ressourcenstatus** = **Nicht verfügbar**<br> **Vorheriger Ressourcenstatus** = **Verfügbar** | Eine Stunde |
|Erstellen einer Azure Advisor-Warnung | Für einen optimalen Betrieb und um sicherzustellen, dass die aktuellsten Sicherheits- und Fehlerfixes wirksam sind, sollten Sie den Agent der Server mit Azure Arc-Unterstützung stets auf dem neuesten Stand zu halten. Veraltete Agents werden mit einer [Azure Advisor-Warnung](../../advisor/advisor-alerts-portal.md) identifiziert.<br><br> Geben Sie Folgendes an, wenn Sie die Warnung konfigurieren:<br> **Empfehlungstyp** = **Upgrade auf die aktuelle Version des Azure Connected Machine-Agents** | Eine Stunde |
|[Weisen Sie Azure-Richtlinien](../../governance/policy/assign-policy-portal.md) Ihrem Abonnement- oder Ressourcengruppenbereich zu. |Weisen Sie die Richtlinie zum **Aktivieren von Azure Monitor für VMs** und andere, die Ihren Anforderungen entsprechen, dem Abonnement- oder Ressourcengruppenbereich zu, um sicherzustellen, dass alle Ihre Server mit Arc-Unterstützung automatisch für die Überwachung mit Azure Monitor für VMs konfiguriert werden.| Varies |
|[Aktivieren der Updateverwaltung für Ihre Server mit Arc-Unterstützung](../../automation/update-management/enable-from-automation-account.md) |Konfigurieren Sie die Updateverwaltung in Azure Automation zur Verwaltung von Betriebssystemupdates für Ihre Windows- und Linux-VMs, die bei Servern mit Arc-Unterstützung registriert sind. | 15 Minuten |

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Problembehandlung finden Sie im [Handbuch zur Problembehandlung des Connected Machine-Agents](troubleshoot-agent-onboard.md).

* Erfahren Sie, wie Sie die Bereitstellung mit anderen Azure-Diensten wie Azure Automation [State Configuration](../../automation/automation-dsc-overview.md) und anderen unterstützten [Azure-VM-Erweiterungen](manage-vm-extensions.md) vereinfachen können.