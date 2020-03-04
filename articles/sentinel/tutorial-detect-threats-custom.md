---
title: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von verdächtigen Bedrohungen mit Azure Sentinel | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Analyseregeln erstellen, um verdächtige Bedrohungen mit Azure Sentinel zu erkennen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: cea7429ecea105355b0afe306bfa334e55d5d9c4
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77585106"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von verdächtigen Bedrohungen

Nachdem Sie  [Ihre Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md)  haben, können Sie benutzerdefinierte Regeln erstellen, mit denen Sie in Ihrer Umgebung nach bestimmten Kriterien suchen und bei einer Übereinstimmung Vorfälle generieren. Diese können Sie anschließend untersuchen. In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Regeln zum Erkennen von Bedrohungen mit Azure Sentinel erstellen.

Dieses Tutorial unterstützt Sie bei der Gefahrenerkennung mit Azure Sentinel.
> [!div class="checklist"]
> * Erstellen von Analyseregeln
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="create-custom-analytic-rules"></a>Erstellen von benutzerdefinierten Analyseregeln

Für die Suche nach verdächtigen Typen von Bedrohungen und Anomalien in Ihrer Umgebung können Sie benutzerdefinierte Analyseregeln erstellen. Diese Regeln stellen sicher, dass Sie im Bedarfsfall sofort benachrichtigt werden, um die Bedrohungen einzugrenzen, zu überprüfen und zu beheben.

1. Wählen Sie im Azure-Portal unter „Azure Sentinel“ die Option **Analytics** aus.

1. Wählen Sie in der oberen Menüleiste die Option **+Erstellen** und anschließend **Geplante Abfrageregel** aus. Dadurch wird der **Assistent für Analyseregeln** geöffnet.

    ![Erstellen einer geplanten Abfrage](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. Geben Sie auf der Registerkarte **Allgemein** einen eindeutigen **Namen** und eine **Beschreibung** an. Im Feld **Taktiken** stehen verschiedene Angriffskategorien zur Auswahl, nach denen die Regel klassifiziert werden kann. Legen Sie den **Schweregrad** der Warnung nach Bedarf fest. Beim Erstellen der Regel wird der **Status** standardmäßig auf **Aktiviert** eingestellt. Dies bedeutet, dass die Regel nach der Erstellung sofort ausgeführt wird. Wenn Sie die Regel nicht sofort ausführen möchten, wählen Sie **Deaktiviert** aus. Die Regel wird der Registerkarte **Aktive Regeln** hinzugefügt, von wo Sie diese bei Bedarf aktivieren können.

    ![Erste Phase beim Erstellen einer benutzerdefinierten Analyseregel](media/tutorial-detect-threats-custom/general-tab.png)

1. Auf der Registerkarte **Regellogik festlegen** können Sie im Feld **Regelabfrage** eine Abfrage direkt eingeben. Alternativ können Sie auch in Log Analytics eine Abfrage erstellen, diese kopieren und in das Feld einfügen.
 
   ![Erstellen einer Abfrage in Azure Sentinel](media/tutorial-detect-threats-custom/settings-tab.png)

   - Beachten Sie auf der rechten Seite den Bereich **Ergebnisvorschau**, in dem Azure Sentinel die Anzahl der von der Abfrage generierten Ergebnisse (Protokollereignisse) anzeigt. Die Anzeige wird beim Schreiben und Konfigurieren der Abfrage dynamisch geändert. Das Diagramm zeigt die Anzahl der Ergebnisse im definierten Zeitraum, der durch die Einstellungen im Abschnitt **Abfrageplanung** bestimmt wird.
    - Wenn Sie feststellen, dass Ihre Abfrage zu häufig bzw. zu viele Warnungen auslöst, können Sie im Abschnitt **Warnungsschwellenwert** eine Baseline festlegen.

      Mit der folgenden Beispielabfrage werden Sie gewarnt, wenn in Azure Activity eine ungewöhnliche Anzahl von Ressourcen erstellt wird.

      `AzureActivity
     \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
     \| where ActivityStatus == "Succeeded"
     \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

      > [!NOTE]
      > Die Abfrage sollte zwischen 1 und 10.000 Zeichen lang sein und darf weder „search \*“ noch „union \*“ enthalten.

    1. Im Abschnitt **Entitäten zuordnen** können Sie Parameter aus den Abfrageergebnissen mit von Azure Sentinel erkannten Entitäten verknüpfen. Diese Entitäten bilden die Grundlage für weitere Analysen und auch für das Gruppieren von Warnungen in Incidents auf der Registerkarte **Incidenteinstellungen**.
    1. Legen Sie im Abschnitt **Abfrageplanung** die folgenden Parameter fest:

       1. Legen Sie unter **Abfrage ausführen alle** fest, wie häufig die Abfrage ausgeführt werden soll: häufig (alle fünf Minuten) oder selten (einmal am Tag).

       1. Legen Sie unter **Datensuche für letzte** den Zeitraum der von der Abfrage abgedeckten Daten fest: z. B. Daten der letzten 10 Minuten oder der letzten 6 Stunden.

       > [!NOTE]
       > Diese beiden Einstellungen sind bis zu einem Punkt voneinander unabhängig. Sie können in einem kurzen Intervall eine Abfrage für einen Zeitraum ausführen, der länger ist als das Intervall (was zu sich überschneidenden Abfragen führt). Sie können jedoch für eine Abfrage kein Intervall festlegen, das den Abdeckungszeitraum überschreitet, da dies zu Lücken in der Gesamtabdeckung der Abfrage führen würde.

    1. Definieren Sie im Abschnitt **Warnungsschwellenwert** eine Baseline. Legen Sie beispielsweise **Warnung generieren, wenn für die Anzahl der Abfrageergebnisse Folgendes gilt:** auf **Ist größer als** fest, und geben Sie die Zahl 1000 ein, wenn die Regel nur dann eine Warnung generieren soll, wenn die Abfrage bei jeder Ausführung mehr als 1000 Ergebnisse generiert. Da dies ein Pflichtfeld ist, müssen Sie, wenn Sie keine Baseline festlegen möchten (d. h., wenn bei jedem Ereignis eine Warnung registriert werden soll), im Zahlenfeld den Wert „0“ eingeben.

    1. Im Abschnitt **Unterdrückung** können Sie die Einstellung**Ausführung der Abfrage beenden, wenn eine Warnung generiert wurde** auf **Ein** festlegen, wenn Sie nach dem Eingang einer Warnung das Ausführen dieser Regel für einen Zeitraum unterbrechen möchten, der das Abfrageintervall überschreitet. Wenn Sie diese Option aktivieren, müssen Sie unter **Abfrageausführung beenden für:** den Zeitraum festlegen, in dem die Abfrage nicht ausgeführt werden soll (bis zu 24 Stunden).

1. Auf der Registerkarte **Incidenteinstellungen** können Sie auswählen, ob und wie Azure Sentinel Warnungen in verwertbare Incidents umwandelt. Wenn Sie diese Registerkarte unverändert lassen, erstellt Azure Sentinel einen eigenen separaten Incident von jeder einzelnen Warnung. Durch Ändern der Einstellungen auf dieser Registerkarte können Sie festlegen, dass keine Incidents erstellt oder mehrere Warnungen zu einem einzelnen Incident gruppiert werden.

    1. Im Abschnitt **Incidenteinstellungen** ist die Option **Incidents aus Warnungen erstellen, die von dieser Analyseregel ausgelöst werden** standardmäßig auf **Aktiviert** eingestellt. Dies bedeutet, dass Azure Sentinel einen eigenen separaten Incident von jeder einzelnen Warnung erstellt, die von der Regel ausgelöst wird.<br></br>Wenn durch diese Regel nicht alle Incidents erstellt werden sollen (z. B. wenn Sie mit dieser Regel nur Informationen für die nachfolgende Analyse sammeln möchten), legen Sie diese Einstellung auf **Deaktiviert** fest.

    1. Wenn aus einer Gruppe ähnlicher oder wiederkehrender Warnungen ein einzelner Incident erstellt werden soll, stellen Sie im Abschnitt **Warnungsgruppierung** die Option **Alle verwandten Warnungen, die durch diese Analyseregel ausgelöst werden, in Incidents gruppieren** auf **Aktiviert** ein, und legen Sie die folgenden Parameter fest.

    1. **Gruppe auf Warnungen beschränken, die innerhalb des ausgewählten Zeitraums erstellt werden**:<br></br> Bestimmen Sie den Zeitraum, in dem ähnliche oder wiederkehrende Warnungen gruppiert werden sollen. Alle entsprechenden Warnungen innerhalb dieses Zeitraums generieren zusammen einen Incident oder eine Gruppe von Incidents (abhängig von den unten aufgeführten Gruppierungseinstellungen). Bei Warnungen außerhalb dieses Zeitraums wird ein separater Incident oder eine Reihe von Incidents generiert.

    2. **Von dieser Analyseregel ausgelöste Warnungen in einem einzigen Incident zusammenfassen und gruppieren nach**: Wählen Sie die Grundlage für die Gruppierung der Warnungen aus:

        - **Gruppieren von Warnungen in einem einzigen Incident, wenn alle Entitäten übereinstimmen**: <br></br>Warnungen werden gruppiert, wenn sie identische Werte für jede der zugeordneten Entitäten aufweisen (weiter oben definiert auf der Registerkarte „Regellogik festlegen“). Dies ist die empfohlene Einstellung.

        - **Gruppieren aller von dieser Regel ausgelösten Warnungen in einem einzigen Incident**: <br></br>Alle von dieser Regel generierten Warnungen werden zusammengefasst, auch wenn sie keine identischen Werte haben.

        - **Gruppieren von Warnungen in einem einzigen Incident, wenn die ausgewählten Entitäten übereinstimmen**: <br></br>Warnungen werden gruppiert, wenn sie identische Werte für einige der zugeordneten Entitäten (Auswahl in Dropdownliste) aufweisen. Sie sollten diese Einstellung verwenden, wenn Sie z. B. separate Incidents auf Basis der Quell- oder Ziel-IP-Adressen erstellen möchten.

    3. **Geschlossene übereinstimmende Incidents erneut öffnen**: Wenn ein Incident geschlossen wurde (was bedeutet, dass das zugrunde liegende Problem behoben wurde) und anschließend eine weitere Warnung generiert wird, die in diese Incidentgruppe gehören würde, haben Sie folgende Möglichkeiten: Legen Sie diese Einstellung auf **Aktiviert** fest, wenn der geschlossene Incident erneut geöffnet werden soll, oder übernehmen Sie die Einstellung **Deaktiviert**, wenn durch die Warnung ein neuer Incident erzeugt werden soll.

1. Wählen Sie auf der Registerkarte **Automatische Reaktionen** alle Playbooks aus, die automatisch ausgeführt werden sollen, wenn durch die benutzerdefinierte Regel eine Warnung generiert wird. Weitere Informationen zum Erstellen und Automatisieren von Playbooks finden Sie unter  [Reaktionen auf Bedrohungen](tutorial-respond-threats-playbook.md).

1. Wählen Sie **Überprüfen und erstellen** aus, um alle Einstellungen für die neue Warnungsregel zu überprüfen, und wählen Sie dann **Erstellen** aus, um die Warnungsregel zu initialisieren.
  
1. Nachdem die Warnung erstellt wurde, wird der Tabelle unter **Aktive Regeln** eine benutzerdefinierte Regel hinzugefügt. Über diese Liste können Sie die einzelnen Regeln aktivieren, deaktivieren oder löschen.

1. Die Ergebnisse der von Ihnen erstellten Warnungsregeln können Sie auf der Seite **Vorfälle** anzeigen. Hier können Sie Bedrohungen eingrenzen,  [Vorfälle untersuchen](tutorial-investigate-cases.md) und die Bedrohungen beseitigen.


> [!NOTE]
> In Azure Sentinel generierte Warnungen sind über die  [Sicherheits-API von Microsoft Graph](https://aka.ms/securitygraphdocs) verfügbar. Weitere Informationen finden Sie in der Dokumentation zur  [Sicherheits-API von Microsoft Graph](https://aka.ms/graphsecurityreferencebetadocs).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden.

Informationen zum Automatisieren der Reaktionen auf Bedrohungen finden Sie im Tutorial  [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).

