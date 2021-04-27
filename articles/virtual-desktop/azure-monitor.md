---
title: Verwenden Sie Monitor Windows Virtual Desktop Monitor - Azure
description: Erfahren Sie mehr über der Verwendung von Azure Monitor für Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 03/31/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 7da35f77dd232e5f523e2bdc3f125c68015ab871
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106448184"
---
# <a name="use-azure-monitor-for-windows-virtual-desktop-to-monitor-your-deployment"></a>Verwenden von Azure Monitor für Windows Virtual Desktop zum Überwachen der Bereitstellung

Azure Monitor für Windows Virtual Desktop ist ein Dashboard, das auf Azure Monitor Workbooks basiert und IT-Experten dabei unterstützt, ihre Windows Virtual Desktop-Umgebungen zu verstehen. In diesem Thema wird erläutert, wie Sie Azure Monitor für Windows Virtual Desktop zur Überwachung Ihrer Windows Virtual Desktop-Umgebungen einrichten.

## <a name="requirements"></a>Anforderungen

Bevor Sie Azure Monitor für Windows Virtual Desktop verwenden, müssen folgende Einrichtungsaufgaben erfüllt sein:

- Alle überwachten Windows Virtual Desktop-Umgebungen müssen auf der neuesten Version von Windows Virtual Desktop basieren, die mit Azure Resource Manager kompatibel ist.
- Mindestens ein Log Analytics-Arbeitsbereich muss konfiguriert sein. Verwenden Sie einen bestimmten Log Analytics-Arbeitsbereich für Ihre Windows Virtual Desktop-Sitzungshosts, um sicherzustellen, dass Leistungszähler und Ereignisse nur von Sitzungshosts in Ihrer Windows Virtual Desktop-Bereitstellung erfasst werden.
- Aktivieren Sie die Datensammlung für die folgenden Elemente in Ihrem Log Analytics-Arbeitsbereich:
    - Diagnose aus Ihrer Windows Virtual Desktop-Umgebung
    - Empfohlene Leistungsindikatoren von Ihren Windows Virtual Desktop-Sitzungs-Hosts
    - Empfohlene Windows-Ereignisprotokolle von Ihren Windows Virtual Desktop-Sitzungs-Hosts

 Der in diesem Artikel beschriebene Daten Einrichtungsprozess ist der Einzige, den Sie zum Überwachen von Windows Virtual Desktop benötigen. Sie können alle anderen Elemente, die Daten an Ihren Log Analytics Arbeitsbereich senden, deaktivieren, um Kosten zu sparen.

Jeder Benutzer, der Azure Monitor für Windows Virtual Desktop für Ihre Umgebung überwacht, benötigt außerdem die folgenden Lesezugriffsberechtigungen:

- Lesezugriff auf die Azure-Abonnements, die Ihre Windows Virtual Desktop-Ressourcen enthalten
- Lesezugriff auf die Ressourcengruppen des Abonnements, die Ihre Windows Virtual Desktop-Sitzungshosts enthalten
- Lesezugriff auf den Log Analytics-Arbeitsbereich

>[!NOTE]
> Der Lesezugriff ermöglicht Administratoren nur das Anzeigen von Daten. Zum Verwalten von Ressourcen im Windows Virtual Desktop-Portal sind andere Berechtigungen erforderlich.

## <a name="open-azure-monitor-for-windows-virtual-desktop"></a>Öffnen von Azure Monitor für Windows Virtual Desktop

Sie können Azure Monitor für Windows Virtual Desktop mit einer der folgenden Methoden öffnen:

- Wechseln Sie zu [aka.ms/azmonwvdi](https://aka.ms/azmonwvdi).
- Suchen Sie im Azure-Portal nach **Windows Virtual Desktop**, und wählen Sie diese Option aus. Dann wählen Sie **Insights** aus.
- Suchen Sie im Azure-Portal nach **Azure Monitor**, und wählen Sie diese Option. Wählen Sie unter **Insights** den **Insights Hub** aus und wählen Sie dann **Windows Virtual Desktop** aus.
Wenn die Seite geöffnet ist, geben Sie das **Abonnement**, die **Ressourcengruppe**, den **Host Pool** und den **Zeitbereich** der Umgebung ein, die Sie überwachen möchten.

>[!NOTE]
>Windows Virtual Desktop unterstützt derzeit nur das Überwachen eines Abonnements, einer Ressourcengruppe und eines Hostpools. Wenn Sie die zu überwachende Umgebung nicht finden können, suchen Sie in der [Dokumentation zur Problembehandlung](troubleshoot-azure-monitor.md) nach bekannten Funktionsanforderungen und Problemen.

## <a name="log-analytics-settings"></a>Log-Analyseeinstellungen

Um Azure Monitor für Windows Virtual Desktop verwenden zu können, benötigen Sie mindestens einen Log Analytics-Arbeitsbereich. Verwenden Sie einen bestimmten Log Analytics-Arbeitsbereich für Ihre Windows Virtual Desktop-Sitzungshosts, um sicherzustellen, dass Leistungszähler und Ereignisse nur von Sitzungshosts in Ihrer Windows Virtual Desktop-Bereitstellung erfasst werden. Wenn Sie bereits einen Arbeitsbereich eingerichtet haben, fahren Sie [mit Einrichten unter Verwendung der Konfigurations-Arbeitsmappe](#set-up-using-the-configuration-workbook)fort. Zum Einrichten, siehe [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../azure-monitor/logs/quick-create-workspace.md).

>[!NOTE]
>Es gelten die Standardgebühren für Datenspeicher für Log Analytics. Für den Einstieg empfiehlt es sich, das Modell mit nutzungsbasierter Bezahlung auszuwählen und dann beim Skalieren der Bereitstellung und der Aufnahme von mehr Daten anzupassen. Weitere Informationen finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/).

## <a name="set-up-using-the-configuration-workbook"></a>Einrichten mit Hilfe der Konfigurations-Arbeitsmappe

Wenn Sie Azure Monitor für Windows Virtual Desktop zum ersten Mal öffnen, müssen Sie Azure Monitor für Ihre Windows Virtual Desktop-Umgebung einrichten. So konfigurieren Sie die Ressourcen:

1. Öffnen Sie Azure Monitor for Windows Virtual Desktop im Azure-Portal unter [aka.ms/azmonwvdi](https://aka.ms/azmonwvdi), wählen Sie dann die **Konfigurations-Arbeitsmappe** aus.
2. Wählen Sie unter **Abonnement**, **Ressourcengruppe** und **Host-Pool**, eine zu konfigurierende Umgebung aus.

Die Konfigurationsarbeitsmappe richtet Ihre Überwachungsumgebung ein und ermöglicht Ihnen eine Überprüfung der Konfiguration, nachdem Sie den Einrichtungsvorgang abgeschlossen haben. Es ist wichtig, Ihre Konfiguration zu überprüfen, wenn Elemente im Dashboard nicht korrekt angezeigt werden oder wenn die Produktgruppe Aktualisierungen veröffentlicht, die neue Einstellungen erfordern.

### <a name="resource-diagnostic-settings"></a>Diagnoseeinstellungen für Ressourcen

Zum Erfassen von Informationen über die virtuelle Windows-Desktop Infrastruktur müssen Sie mehrere Diagnose-Einstellungen auf Ihren Windows-Hosts für virtuelle Desktops und Arbeitsbereiche aktivieren (Dies ist Ihr virtueller Windows-Desktop Arbeitsbereich, nicht Ihr Log Analytics Arbeitsbereich). Weitere Informationen zu Host Pools, Arbeitsbereichen und anderen virtuellen Windows-Desktop Ressourcen Objekten finden Sie in unserem [Umwelt-Handbuch](environment-setup.md).

Weitere Informationen zur Windows Virtual Desktop-Diagnose und den unterstützten Diagnose Tabellen finden Sie unter [Senden von Windows Virtual Desktop Diagnostics an Log Analytics](diagnostics-log-analytics.md).

So legen Sie Ihre Einstellungen für die Ressourcendiagnose in der Konfigurationsarbeitsmappe fest: 

1. Wählen Sie die Registerkarte **Ressourcen-Diagnose-Einstellungen** in der Konfigurations-Arbeitsmappe aus. 
2. Wählen Sie **Log Analytics Arbeitsbereich** aus, um die Windows-Desktop Diagnose zu senden. 

#### <a name="host-pool-diagnostic-settings"></a>Diagnoseeinstellungen für den Hostpool

So richten Sie die Host Pool Diagnose mithilfe des Abschnitts Diagnose Einstellungen für Ressourcen in der Konfigurations-Arbeitsmappe ein:

1. Prüfen Sie unter **Host-Pool**, ob die Diagnose von Windows Virtual Desktop aktiviert ist. Wenn dies nicht der Fall ist, wird eine Fehlermeldung angezeigt, die besagt: "Es wurde keine vorhandene Diagnosekonfiguration für den ausgewählten Hostpool gefunden." Sie müssen die folgenden unterstützten Diagnosetabellen aktivieren:

    - Prüfpunkt
    - Fehler
    - Verwaltung
    - Verbindung
    - HostRegistration
    - AgentHealthStatus
    
    >[!NOTE]
    > Wenn die Fehlermeldung nicht angezeigt wird, müssen Sie die Schritte 2 bis 4 nicht ausführen.

2. Wählen Sie **Hostpool konfigurieren** aus.
3. Klicken Sie auf **Bereitstellen**.
4. Aktualisieren Sie die Konfigurationsarbeitsmappe.

#### <a name="workspace-diagnostic-settings"></a>Arbeitsbereich-Diagnoseeinstellungen 

So richten Sie die Arbeitsbereichsdiagnose mithilfe des Abschnitts für die Ressourcendiagnoseeinstellungen in der Konfigurationsarbeitsmappe ein:

 1. Überprüfen Sie unter **Arbeitsbereich**, ob die Windows Virtual Desktop-Diagnose für den virtuellen Windows Desktop-Arbeitsbereich aktiviert ist. Wenn dies nicht der Fall ist, wird eine Fehlermeldung angezeigt, die besagt: "Es wurde keine vorhandene Diagnosekonfiguration für den ausgewählten Hostpool gefunden." Sie müssen die folgenden unterstützten Diagnosetabellen aktivieren:
 
    - Prüfpunkt
    - Fehler
    - Verwaltung
    - Feed
    
    >[!NOTE]
    > Wenn die Fehlermeldung nicht angezeigt wird, müssen Sie die Schritt 2-4 nicht ausführen.

2. Wählen Sie **Konfigurieren eines Arbeitsbereichs**.
3. Klicken Sie auf **Bereitstellen**.
4. Aktualisieren Sie die Konfigurationsarbeitsmappe.

### <a name="session-host-data-settings"></a>Einstellungen für Sitzungshostdaten

Um Informationen über Ihre Windows Virtual Desktop-Sitzungshosts zu sammeln, müssen Sie den Log Analytics-Agenten auf allen Sitzungshosts im Host-Pool installieren, sicherstellen, dass die Sitzungshosts an einen Log Analytics-Arbeitsbereich senden, und die Einstellungen des Log Analytics-Agenten so konfigurieren, dass Leistungsdaten und Windows-Ereignisprotokolle gesammelt werden.

Der Arbeitsbereich Log Analytics, an den Sie Sitzungshost-Daten senden, muss nicht identisch sein, an den Sie Diagnosedaten senden. Wenn Sie über Azure-Sitzungs-Hosts außerhalb Ihrer Windows-Umgebung für virtuelle Desktops verfügen, empfiehlt es sich, einen bestimmten Log Analytics Arbeitsbereich für die Windows Virtual Desktop-Sitzungs-Hosts zu haben. 

So legen Sie den Log Analytics Arbeitsbereich fest, in dem Sie Sitzungs-Hostdaten sammeln möchten: 

1. Wählen Sie die Registerkarte **Sitzungs-Host Dateneinstellungen** in der Konfigurations Arbeitsmappe aus. 
2. Hiermit wählen Sie den **Log Analytics-Arbeitsbereich** aus, an den Sitzungs-Hostdaten gesendet werden sollen. 

#### <a name="session-hosts"></a>Sitzungshosts

Im letzten Schritt müssen Sie den Log Analytics-Agent auf allen Hosts im Host-Pool installieren, um Daten von den Hosts an den ausgewählten Arbeitsbereich zu senden. Wenn Log Analytics nicht für alle Sitzungshosts im Hostpool konfiguriert ist, wird oben in den Einstellungen für **Sitzungshostdaten** ein Abschnitt **Sitzungshosts** mit der Meldung „Einige Hosts im Hostpool senden keine Daten an den ausgewählten Log Analytics-Arbeitsbereich."

>[!NOTE]
> Wenn Sie den Abschnitt **Sitzungshosts** oder eine Fehlermeldung nicht sehen, sind alle Sitzungshosts korrekt eingerichtet. Fahren Sie mit dem Einrichten der Anweisungen für [Arbeitsbereichs-Leistungsindikatoren](#workspace-performance-counters)fort.

So richten Sie die restlichen Sitzungs-Hosts mithilfe der Konfigurations-Arbeitsmappe ein:

1. Wählen Sie **Hosts dem Arbeitsbereich** hinzufügen. 
2. Aktualisieren Sie die Konfigurationsarbeitsmappe.

>[!NOTE]
>Der Hostcomputer muss aktiv sein, um die Log Analytics-Erweiterung zu installieren. Wenn bei der automatischen Bereitstellung auf einem Host ein Fehler auftritt, können Sie die Erweiterung immer auch manuell auf einem Host installieren. Informationen zum manuellen Installieren der Erweiterung finden Sie unter [Log Analytics-VM-Erweiterung für Windows](../virtual-machines/extensions/oms-windows.md).

#### <a name="workspace-performance-counters"></a>Leistungsindikatoren für den Arbeitsbereich

Sie müssen bestimmte Leistungsindikatoren aktivieren, um Leistungsinformationen von ihren Sitzungs-Hosts zu sammeln und an den Arbeitsbereich Log Analytics zu senden.

Wenn Sie bereits Leistungsindikatoren aktiviert haben und diese entfernen möchten, folgen Sie den Anweisungen unter [Konfigurieren von Leistungsindikatoren](../azure-monitor/agents/data-sources-performance-counters.md). Sie können Leistungszähler an der gleichen Stelle hinzufügen und entfernen.

So richten Sie Leistungsindikatoren mithilfe der Konfigurations-Arbeitsmappe ein: 

1. Aktivieren Sie unter Arbeitsbereichs- **Leistungsindikatoren** in der Konfigurations Arbeitsmappe die Option **konfigurierte** Indikatoren, um die Indikatoren anzuzeigen, die Sie bereits für das Senden an den Arbeitsbereich Log Analytics aktiviert haben. Überprüfen Sie **fehlende Zähler**, um sicherzustellen, dass Sie alle erforderlichen Zähler aktiviert haben.
2. Wenn Sie fehlende Zähler haben, wählen Sie **Leistungsindikatoren konfigurieren** aus.
3. Wählen Sie **Konfiguration anwenden** aus.
4. Aktualisieren Sie die Konfigurationsarbeitsmappe.
5. Stellen Sie sicher, dass alle erforderlichen Indikatoren aktiviert sind, indem Sie die Liste **fehlende** Leistungsindikatoren überprüfen. 

#### <a name="configure-windows-event-logs"></a>Konfigurieren der Windows-Ereignisprotokolle

Außerdem müssen Sie bestimmte Windows-Ereignisprotokolle aktivieren, um Fehler, Warnungen und Informationen von den Sitzungs-Hosts zu erfassen und diese an den Arbeitsbereich Log Analytics zu senden.

Wenn Sie Windows-Ereignisprotokolle bereits aktiviert haben und diese entfernen möchten, befolgen Sie die Anweisungen unter [Konfigurieren von Windows-Ereignisprotokollen](../azure-monitor/agents/data-sources-windows-events.md#configuring-windows-event-logs).  Sie können Windows-Ereignisprotokolle am gleichen Standort hinzufügen und entfernen.

So richten Sie Windows-Ereignisprotokolle anhand der Konfigurations-Arbeitsmappe ein:

1. Überprüfen Sie unter **Konfiguration von Windows-Ereignisprotokollen** die Option **konfigurierte Ereignisprotokolle**, um die Ereignisprotokolle anzuzeigen, die Sie bereits für das Senden an den Arbeitsbereich Log Analytics freigegeben haben. Überprüfen Sie **fehlende Ereignisprotokolle**, um sicherzustellen, dass Sie alle Windows-Ereignisprotokolle aktiviert haben.
2. Wenn Sie über fehlende Windows-Ereignisprotokolle verfügen, wählen Sie **Ereignisse konfigurieren** aus.
3. Klicken Sie auf **Bereitstellen**.
4. Aktualisieren Sie die Konfigurationsarbeitsmappe.
5. Stellen Sie sicher, dass alle erforderlichen Windows-Ereignisprotokolle aktiviert sind, indem Sie die Liste der **fehlenden Ereignisprotokolle** überprüfen. 

>[!NOTE]
>Wenn die automatische Ereignisbereitstellung fehlschlägt, wählen Sie **Agentenkonfiguration öffnen** in der Konfigurationsarbeitsmappe, um fehlende Windows-Ereignisprotokolle manuell hinzuzufügen. 

## <a name="optional-configure-alerts"></a>Optional: Konfigurieren von Warnungen

Azure Monitor für Windows Virtual Desktop ermöglicht Ihnen die Überwachung von Azure Monitor-Warnungen, die innerhalb Ihres ausgewählten Abonnements im Kontext Ihrer Windows Virtual Desktop-Daten auftreten. Bei Azure Monitor Warnungen handelt es sich um eine fakultative Funktion in ihren Azure-Abonnements, und Sie müssen Sie separat von Azure Monitor für den virtuellen Windows-Desktop einrichten. Sie können das Azure Monitor Alerts-Framework verwenden, um benutzerdefinierte Warnungen für virtuelle Windows-Desktop Ereignisse, Diagnosen und Ressourcen festzulegen. Weitere Informationen zu Warnungen finden Sie unter [Reagieren auf Ereignisse mit Azure Monitor-Warnungen](../azure-monitor/alerts/tutorial-response.md).

## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten

Wenn Sie den Azure Monitor-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Diensts zu verbessern.

Um genaue und effiziente Funktionen zur Problembehandlung bereitzustellen, enthalten die gesammelten Daten die Portalsitzungs-ID, die Azure Active Directory-Benutzer-ID und den Namen der Portalregisterkarte, auf der das Ereignis aufgetreten ist. Microsoft erfasst weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://privacy.microsoft.com/privacystatement).

>[!NOTE]
>Informationen zum Anzeigen oder Löschen Ihrer personenbezogenen Daten, die vom Dienst gesammelt werden, finden Sie unter [Anträge betroffener Personen für Azure im Rahmen der DSGVO](/microsoft-365/compliance/gdpr-dsr-azure). Allgemeine Informationen zur DSGVO finden Sie im [Abschnitt über DSGVO für Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Azure Monitor für Ihre Windows Virtual Desktop-Umgebung konfiguriert haben, finden Sie hier einige Ressourcen, die Ihnen bei der Überwachung Ihrer Umgebung helfen könnten:

- Sehen Sie sich unser [Glossar](azure-monitor-glossary.md) an, um mehr über Begriffe und Konzepte im Zusammenhang mit Azure Monitor für Windows Virtual Desktop zu erfahren.
- Informationen dazu, wie Sie Ihre Datenspeicher Kosten schätzen, Messen und verwalten können, finden Sie unter [Schätzen der Azure Monitor Kosten](azure-monitor-costs.md).
- Wenn ein Problem auftritt, finden Sie weitere Informationen im [Leitfaden zur Problembehandlung](troubleshoot-azure-monitor.md).
