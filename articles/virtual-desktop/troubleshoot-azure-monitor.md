---
title: Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop
description: Hier erfahren Sie mehr über das Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 03/29/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: a104f4d33e1bd38e130101b34d3fd2021de27cd2
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106445481"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop"></a>Behandeln von Problemen mit Azure Monitor für Windows Virtual Desktop

In diesem Artikel werden bekannte Probleme und Lösungen für häufig auftretende Probleme in Azure Monitor für Windows Virtual Desktop beschrieben.

## <a name="issues-with-configuration-and-setup"></a>Probleme mit der Konfiguration und dem Setup

Wenn die Konfigurationsarbeitsmappe nicht richtig funktioniert, um das Setup zu automatisieren, können Sie diese Ressourcen verwenden, um Ihre Umgebung manuell einzurichten:

- Informationen zum manuellen Aktivieren der Diagnose oder zum Zugreifen auf den Log Analytics-Arbeitsbereich finden Sie unter [Senden der Windows Virtual Desktop-Diagnose an Log Analytics](diagnostics-log-analytics.md).
- Informationen zum manuellen Installieren der Log Analytics-Erweiterung auf einem Sitzungshost finden Sie unter [Log Analytics-VM-Erweiterung für Windows](../virtual-machines/extensions/oms-windows.md).
- Eine Anleitung zum Einrichten eines neuen Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/logs/quick-create-workspace.md).
- Informationen zum Hinzufügen, Entfernen oder Bearbeiten von Leistungsindikatoren finden Sie unter [Konfigurieren von Leistungsindikatoren](../azure-monitor/agents/data-sources-performance-counters.md).
- Informationen zum Konfigurieren von Windows-Ereignisprotokollen für einen Log Analytics-Arbeitsbereich finden Sie unter [Datenquellen für das Sammeln von Windows-Ereignisprotokolldaten mit dem Log Analytics-Agent](../azure-monitor/agents/data-sources-windows-events.md).

## <a name="my-data-isnt-displaying-properly"></a>Meine Daten werden nicht ordnungsgemäß angezeigt

Wenn Ihre Daten nicht ordnungsgemäß angezeigt werden, überprüfen Sie die folgenden allgemeinen Lösungen:

- Stellen Sie zunächst sicher, dass Sie die Konfigurationsarbeitsmappe ordnungsgemäß eingerichtet haben, wie es unter [Verwenden von Azure Monitor für Windows Virtual Desktop zum Überwachen der Bereitstellung](azure-monitor.md) beschrieben ist. Wenn Leistungsindikatoren oder Ereignisse fehlen, werden die zugehörigen Daten nicht im Azure-Portal angezeigt.
- Überprüfen Sie Ihre Zugriffsrechte und wenden Sie sich an die Ressourcenbesitzer, um fehlende Berechtigungen anzufordern. Jeder, der Windows Virtual Desktop überwacht, benötigt die folgenden Berechtigungen:
    - Lesezugriff auf die Azure-Abonnements, die Ihre Windows Virtual Desktop-Ressourcen enthalten
    - Lesezugriff auf die Ressourcengruppen des Abonnements, die Ihre Windows Virtual Desktop-Sitzungshosts enthalten 
    - Lesezugriff auf die Log Analytics-Arbeitsbereiche, die Sie verwenden
- Möglicherweise müssen Sie ausgehende Ports in der Firewall Ihres Servers öffnen, damit Azure Monitor und Log Analytics Daten an das Portal senden können. Weitere Informationen hierzu finden Sie in den folgenden Artikeln:
      - [Von Azure Monitor verwendete IP-Adressen](../azure-monitor/app/ip-addresses.md)
      - [Firewallanforderungen](../azure-monitor/agents/log-analytics-agent.md#firewall-requirements). 
- Es werden keine Daten der letzten Aktivität angezeigt? Sie sollten vielleicht 15 Minuten warten und den Feed aktualisieren. Azure Monitor weist eine Wartezeit von 15 Minuten beim Auffüllen von Protokolldaten auf. Weitere Informationen finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](../azure-monitor/logs/data-ingestion-time.md).

Wenn keine Informationen fehlen, die Daten aber dennoch nicht ordnungsgemäß angezeigt werden, liegt möglicherweise ein Problem in der Abfrage oder den Datenquellen vor. Informieren Sie sich über [Einschränkungen und bekannte Probleme](#known-issues-and-limitations). 

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Ich möchte Azure Monitor für Windows Virtual Desktop anpassen

Azure Monitor für Windows Virtual Desktop verwendet Azure Monitor-Arbeitsmappen. Mit Arbeitsmappen können Sie eine Kopie der Windows Virtual Desktop-Arbeitsmappenvorlage speichern und eigene Anpassungen vornehmen.

Entwurfsbedingt übernehmen benutzerdefinierte Arbeitsmappenvorlagen nicht automatisch Updates aus der Produktgruppe. Weitere Informationen finden Sie unter [Problembehandlung für arbeitsmappenbasierte Erkenntnisse](../azure-monitor/insights/troubleshoot-workbooks.md) und in der [Übersicht über Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>Ich kann die Daten nicht interpretieren

Weitere Informationen zu den Datenbegriffen finden Sie im [Glossar für Azure Monitor für Window Virtual Desktop](azure-monitor-glossary.md).

## <a name="the-data-i-need-isnt-available"></a>Die benötigten Daten sind nicht verfügbar

Wenn Sie mehr Leistungsindikatoren oder Windows-Ereignisprotokolle überwachen möchten, können Sie diese dazu aktivieren, Diagnoseinformationen an Ihren Log Analytics-Arbeitsbereich zu senden und sie in der **Hostdiagnose: Hostbrowser** zu überwachen. 

- Informationen zum Hinzufügen von Leistungsindikatoren finden Sie unter [Konfigurieren von Leistungsindikatoren](../azure-monitor/agents/data-sources-performance-counters.md#configuring-performance-counters).
- Informationen zum Hinzufügen von Windows-Ereignissen finden Sie unter [Konfigurieren der Windows-Ereignisprotokolle](../azure-monitor/agents/data-sources-windows-events.md#configuring-windows-event-logs).

Sie können keinen Datenpunkt finden, der bei der Diagnose eines Problems hilft? Senden Sie uns Feedback!

- Informationen zum Bereitstellen von Feedback finden Sie unter [Problembehandlung von Übersicht, Feedback und Support für Windows Virtual Desktop](troubleshoot-set-up-overview.md).
- Sie können Ihr Feedback in Bezug auf Windows Virtual Desktop auch im [Windows Virtual Desktop-Feedback-Hub](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) hochladen.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Die folgenden Probleme und Einschränkungen sind uns bekannt, und wir arbeiten an deren Behebung:

- Sie können immer nur einen Hostpool gleichzeitig überwachen. 
- Um Favoriteneinstellungen zu speichern, müssen Sie eine benutzerdefinierte Vorlage der Arbeitsmappe speichern. Benutzerdefinierte Vorlagen übernehmen nicht automatisch Updates aus der Produktgruppe.
- Beim Laden Ihrer Auswahl in die Konfigurationsarbeitsmappe werden manchmal „Fehler bei Abfrage“-Fehlermeldungen angezeigt. Aktualisieren Sie die Abfrage, geben Sie bei Bedarf die Auswahl erneut ein, und der Fehler sollte nicht mehr auftreten. 
- Einige Fehlermeldungen sind nicht benutzerfreundlich formuliert, und nicht alle Fehlermeldungen werden in der Dokumentation beschrieben.
- Der Leistungsindikator für die Gesamtzahl der Sitzungen kann die Anzahl der Sitzungen um einen geringen Wert übersteigen, sodass Ihre Gesamtzahl der Sitzungen scheinbar den Grenzwert für „Max. Sitzungen“ übersteigt.
- Die Anzahl der verfügbaren Sitzungen spiegelt nicht die Richtlinien zur Skalierung des Hostpools wider.   
- Werden widersprüchliche oder unerwartete Verbindungszeiten angezeigt? Auch wenn es selten vorkommt, kann das Verbindungsabschlussereignis verloren gehen, was sich auf einige Visuals und Metriken auswirken kann.
- Die Verbindungszeit umfasst die Zeit, die Benutzer für die Eingabe ihrer Anmeldeinformationen benötigen. Dies korreliert mit der Erfahrung, kann aber in einigen Fällen falsche Spitzenwerte anzeigen. 
    

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie zum Einstieg [Verwenden von Azure Monitor für Windows Virtual Desktop zum Überwachen der Bereitstellung](azure-monitor.md).
- Informationen dazu, wie Sie Ihre Datenspeicherkosten schätzen, messen und verwalten können, finden Sie unter [Schätzen der Azure Monitor-Kosten](azure-monitor-costs.md).
- Sehen Sie sich unser [Glossar](azure-monitor-glossary.md) an, um mehr über Begriffe und Konzepte im Zusammenhang mit Azure Monitor für Windows Virtual Desktop zu erfahren.
