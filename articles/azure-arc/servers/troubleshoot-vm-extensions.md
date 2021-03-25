---
title: Behandeln von Problemen mit VM-Erweiterungen für Server mit Azure Arc-Unterstützung
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit Azure-VM-Erweiterungen beheben, die bei Servern mit Azure Arc-Unterstützung auftreten.
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 799e5c8ec00a894c6a54c64494edd8f259faf2dc
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "100584675"
---
# <a name="troubleshoot-arc-enabled-servers-vm-extension-issues"></a>Behandeln von Problemen mit VM-Erweiterungen für Server mit Azure Arc-Unterstützung

Dieser Artikel enthält Informationen zur Behandlung und Behebung von Problemen, die bei dem Versuch auftreten können, Azure-VM-Erweiterungen auf Servern mit Azure Arc-Unterstützung bereitzustellen oder zu entfernen. Allgemeine Informationen finden Sie unter [Verwaltung von Erweiterungen für virtuelle Computer mit Azure Arc-fähigen Servern](./manage-vm-extensions.md).

## <a name="general-troubleshooting"></a>Allgemeine Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal abgerufen werden.

Die folgenden Schritte zur Problembehandlung gelten für alle VM-Erweiterungen.

1. Um die Protokolldatei des Gast-Agents zu überprüfen, sollten Sie die Aktivität bei der Bereitstellung der Erweiterung in `%SystemDrive%\ProgramData\GuestConfig\ext_mgr_logs` für Windows und in `/var/lib/GuestConfig/ext_mgr_logs` für Linux ansehen.

2. Überprüfen Sie die Erweiterungsprotokolle für die spezifische Erweiterung auf weitere Details in `%SystemDrive%\ProgramData\GuestConfig\extension_logs\<Extension>` für Windows. Die Erweiterungsausgabe wird für jede unter Linux installierte Erweiterung in einer Datei unter `/var/lib/GuestConfig/extension_logs` protokolliert.

3. Lesen Sie die Abschnitte zur Problembehandlung in der Dokumentation zu Erweiterungen für Fehlercodes, bekannten Problemen usw. Informationen zur weiteren Problembehandlung für jede Erweiterung finden Sie im Abschnitt **Problembehandlung und Support** in der Übersicht für die jeweilige Erweiterung. Dies schließt die Beschreibung der Fehlercodes ein, die in das Protokoll geschrieben werden. Die Artikel zur Erweiterung sind in der [Tabelle der Erweiterungen](manage-vm-extensions.md#extensions) verlinkt.

4. Sehen Sie sich die Systemprotokolle an. Überprüfen Sie, ob es andere Vorgänge gab, die möglicherweise die Erweiterung beeinträchtigt haben, z.B. eine lange Installation einer anderen Anwendung, für die exklusiver Zugriff auf den Paket-Manager notwendig war.

## <a name="troubleshooting-specific-extension-scenarios"></a>Problembehandlung für bestimmte Erweiterungsszenarien

### <a name="vm-insights"></a>VM Insights

- Wenn Sie VM Insights für einen Server mit Azure Arc-Unterstützung aktivieren, werden der Dependency- und der Log Analytics-Agent installiert. Auf einem langsamen Computer oder einem Computer mit einer langsamen Netzwerkverbindung können während des Installationsvorgangs Timeouts auftreten. Microsoft ergreift derzeit Maßnahmen im Connected Machine-Agent, um dieses Problem zu beheben. In der Zwischenzeit ist eine erneute Installation unter Umständen erfolgreich.

### <a name="log-analytics-agent-for-linux"></a>Log Analytics-Agent für Linux

- In der Log Analytics-Agent-Version 1.13.9 (die entsprechende Version der Erweiterung lautet 1.13.15) werden hochgeladene Daten nicht ordnungsgemäß mit der Ressourcen-ID des Servers mit Azure Arc-Unterstützung markiert. Protokolle werden zwar an den Dienst gesendet, beim Versuch, die Daten vom ausgewählten aktivierten Server nach Auswählen von **Protokolle** oder **Erkenntnisse** anzuzeigen, werden aber keine Daten zurückgegeben. Sie können die Daten anzeigen, indem Sie Abfragen aus Azure Monitor-Protokollen oder über Azure Monitor für VMs ausführen, die auf den Arbeitsbereich begrenzt sind.

- Einige Distributionen werden zurzeit vom Log Analytics-Agent für Linux nicht unterstützt. Für den Agent müssen zusätzliche Abhängigkeiten installiert werden, u. a. Python 2. Informationen zur Supportmatrix zu den Voraussetzungen finden Sie [hier](../../azure-monitor/agents/agents-overview.md#supported-operating-systems).

- Fehlercode 52 in der Statusmeldung weist auf eine fehlende Abhängigkeit hin. Überprüfen Sie die Ausgabe und die Protokolle, um weitere Informationen zur fehlenden Abhängigkeit zu erhalten.

- Wenn bei der Installation ein Fehler auftritt, lesen Sie den Abschnitt **Problembehandlung und Support** in der Übersicht für die Erweiterung. In den meisten Fällen ist in der Statusmeldung ein Fehlercode enthalten. [Hier](../../virtual-machines/extensions/oms-linux.md#troubleshoot-and-support) finden Sie Informationen zu den Statusmeldungen für den Log Analytics-Agent für Linux sowie allgemeine Informationen zur Problembehandlung für diese VM-Erweiterung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem hier nicht aufgeführt wird, oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Azure-Experten über [Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-arc.html).

- Stellen Sie eine Verbindung mit [@AzureSupport](https://twitter.com/azuresupport) her, dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Der Azure-Support verbindet die Azure-Community mit Antworten, Support und Experten.

- Erstellen Sie einen Azure-Supportfall. Wechseln Sie zur [Azure-Supportwebsite](https://azure.microsoft.com/support/options/), und wählen Sie **Support erhalten** aus.
