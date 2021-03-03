---
title: Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop (Vorschau)
description: Hier erfahren Sie mehr über das Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1453d43707a3680380e553fe69d738b27fe5ae3a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100571345"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop-preview"></a>Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop (Vorschau)

>[!IMPORTANT]
>Azure Monitor für Windows Virtual Desktop befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Artikel werden bekannte Probleme und Lösungen für häufig auftretende Probleme in Azure Monitor für Windows Virtual Desktop (Vorschau) beschrieben.

## <a name="issues-with-configuration-and-setup"></a>Probleme mit der Konfiguration und dem Setup

Wenn die Konfigurationsarbeitsmappe nicht richtig funktioniert, um das Setup zu automatisieren, können Sie diese Ressourcen verwenden, um Ihre Umgebung manuell einzurichten:

- Informationen zum manuellen Aktivieren der Diagnose oder zum Zugreifen auf den Log Analytics-Arbeitsbereich finden Sie unter [Senden der Windows Virtual Desktop-Diagnose an Log Analytics](diagnostics-log-analytics.md).
- Informationen zum manuellen Installieren der Log Analytics-Erweiterung auf einem Host finden Sie unter [Log Analytics-VM-Erweiterung für Windows](../virtual-machines/extensions/oms-windows.md).
- Eine Anleitung zum Einrichten eines neuen Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/logs/quick-create-workspace.md).
- Informationen zum Hinzufügen oder Entfernen von Leistungsindikatoren finden Sie unter [Konfigurieren von Leistungsindikatoren](../azure-monitor/agents/data-sources-performance-counters.md).
- Informationen zum Konfigurieren von Ereignissen für einen Log Analytics-Arbeitsbereich finden Sie unter [Datenquellen für das Sammeln von Windows-Ereignisprotokolldaten mit dem Log Analytics-Agent](../azure-monitor/agents/data-sources-windows-events.md).

## <a name="my-data-isnt-displaying-properly"></a>Meine Daten werden nicht ordnungsgemäß angezeigt

Wenn Ihre Daten nicht ordnungsgemäß angezeigt werden, überprüfen Sie die Konfiguration, die Berechtigungen und überprüfen Sie, dass die erforderlichen IP-Adressen nicht blockiert werden. 

- Stellen Sie zunächst sicher, dass Sie alle Felder in der Konfigurationsarbeitsmappe ausgefüllt haben, wie es unter [Verwenden von Azure Monitor für Windows Virtual Desktop zum Überwachen der Bereitstellung](azure-monitor.md) beschrieben ist. Wenn Leistungsindikatoren oder Ereignisse fehlen, werden die zugehörigen Daten nicht im Azure-Portal angezeigt.

- Überprüfen Sie Ihre Zugriffsrechte und wenden Sie sich an die Ressourcenbesitzer, um fehlende Berechtigungen anzufordern. Jeder, der Windows Virtual Desktop überwacht, benötigt die folgenden Berechtigungen:

    - Lesezugriff auf die Azure-Abonnements, die Ihre Windows Virtual Desktop-Ressourcen enthalten
    - Lesezugriff auf die Ressourcengruppen des Abonnements, die Ihre Windows Virtual Desktop-Sitzungshosts enthalten 
    - Lesezugriff auf den Log Analytics-Arbeitsbereich

- Möglicherweise müssen Sie ausgehende Ports in der Firewall Ihres Servers öffnen, damit Azure Monitor Daten an das Portal senden kann, siehe [Ausgehende Ports](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses). 

- Es werden keine Daten der letzten Aktivität angezeigt? Sie sollten vielleicht 15 Minuten warten und den Feed aktualisieren. Azure Monitor weist eine Wartezeit von 15 Minuten beim Auffüllen von Protokolldaten auf. Weitere Informationen finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

Wenn keine Informationen fehlen, die Daten aber dennoch nicht ordnungsgemäß angezeigt werden, liegt möglicherweise ein Problem in der Abfrage oder den Datenquellen vor. Sehen Sie sich unsere bekannten Probleme und Einschränkungen an. 

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Ich möchte Azure Monitor für Windows Virtual Desktop anpassen

Azure Monitor für Windows Virtual Desktop verwendet Azure Monitor-Arbeitsmappen. Mit Arbeitsmappen können Sie eine Kopie der Windows Virtual Desktop-Arbeitsmappenvorlage speichern und eigene Anpassungen vornehmen.

Entwurfsbedingt übernehmen benutzerdefinierte Arbeitsmappenvorlagen nicht automatisch Updates aus der Produktgruppe. Weitere Informationen finden Sie unter [Problembehandlung für arbeitsmappenbasierte Erkenntnisse](../azure-monitor/insights/troubleshoot-workbooks.md) und in der [Übersicht über Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>Ich kann die Daten nicht interpretieren

Weitere Informationen zu den Datenbegriffen finden Sie im [Glossar für Azure Monitor für Window Virtual Desktop](azure-monitor-glossary.md).

## <a name="the-data-i-need-isnt-available"></a>Die benötigten Daten sind nicht verfügbar

Wenn Sie mehr Leistungsindikatoren oder Ereignisse überwachen möchten, können Sie diese zum Senden an Ihren Log Analytics-Arbeitsbereich aktivieren und sie in der Hostdiagnose überwachen: Hostbrowser 

- Informationen zum Hinzufügen von Leistungsindikatoren finden Sie unter [Konfigurieren von Leistungsindikatoren](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#configuring-performance-counters).
- Informationen zum Hinzufügen von Windows-Ereignissen finden Sie unter [Konfigurieren der Windows-Ereignisprotokolle](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events#configuring-windows-event-logs).

Sie können keinen Datenpunkt finden, der bei der Diagnose eines Problems hilft? Senden Sie uns Feedback!

- Informationen zum Bereitstellen von Feedback finden Sie unter [Problembehandlung von Übersicht, Feedback und Support für Windows Virtual Desktop](troubleshoot-set-up-overview.md).
- Sie können Feedback zu Windows Virtual Desktop auch auf dem [Windows Virtual Desktop-Feedback-Hub](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) oder im [UserVoice-Forum](https://windowsvirtualdesktop.uservoice.com/forums/921118-general) hinterlassen.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Dies sind Probleme und Einschränkungen, die uns derzeit bekannt sind und an deren Behebung wir arbeiten:

- Sie können immer nur einen Hostpool gleichzeitig überwachen. 

- Um Favoriteneinstellungen zu speichern, müssen Sie eine benutzerdefinierte Vorlage der Arbeitsmappe speichern. Benutzerdefinierte Vorlagen übernehmen nicht automatisch Updates aus der Produktgruppe.

- Einige Fehlermeldungen sind nicht benutzerfreundlich formuliert, und nicht alle Fehlermeldungen sind in der Dokumentation beschrieben.

- Der Leistungsindikator für die Gesamtzahl der Sitzungen kann die Anzahl der Sitzungen um einen geringen Wert übersteigen, sodass Ihre Gesamtzahl der Sitzungen scheinbar den Grenzwert für „Max. Sitzungen“ übersteigt.

- Die Anzahl der verfügbaren Sitzungen spiegelt nicht die Richtlinien zur Skalierung des Hostpools wider. 
    
- In seltenen Fällen kann das Abschlussereignis einer Verbindung verloren gehen, was sich auf einige visuelle Elemente wie Verbindungen im Zeitverlauf und den Verbindungsstatus des Benutzers auswirken kann.  
    
- Die Konfigurationsarbeitsmappe unterstützt nur die Konfiguration von Hosts innerhalb derselben Region wie ihre Ressourcengruppe. 

- Die Verbindungszeit umfasst die Zeit, die Benutzer für die Eingabe ihrer Anmeldeinformationen benötigen. Dies korreliert mit der Erfahrung, kann aber in einigen Fällen falsche Spitzenwerte anzeigen. 
    

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie nicht sicher sind, wie die Daten zu interpretieren sind, oder weitere Informationen zu häufig verwendeten Begriffen benötigen, sehen Sie sich das [Glossar für Azure Monitor für Windows Virtual Desktop](azure-monitor-glossary.md) an. Informationen zum Einrichten und Verwenden von Azure Monitor für Window Virtual Desktop finden Sie unter [Verwenden von Azure Monitor für Windows Virtual Desktop zum Überwachen der Bereitstellung](azure-monitor.md).