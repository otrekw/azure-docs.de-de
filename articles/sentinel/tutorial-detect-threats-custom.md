---
title: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen mit Azure Sentinel | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Analyseregeln erstellen, um mit Azure Sentinel Sicherheitsbedrohungen zu erkennen. Nutzen Sie die Ereignis- und Warnungsgruppierung und die Warnungsanreicherung, und informieren Sie sich über „AUTO DISABLED“.
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
ms.date: 02/10/2021
ms.author: yelevin
ms.openlocfilehash: 70b56e70ec0e6f511142c48cc89720c054807a5c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105042797"
---
# <a name="tutorial-create-custom-analytics-rules-to-detect-threats"></a>Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen

Nachdem Sie [Ihre Datenquellen mit Azure Sentinel verbunden haben](quickstart-onboard.md), können Sie benutzerdefinierte Analyseregeln erstellen, mit denen Sie Bedrohungen und anomales Verhalten in Ihrer Umgebung erkennen können. Mit diesen Regeln werden bestimmte Ereignisse oder Ereignisgruppen in Ihrer Umgebung gesucht, beim Erreichen bestimmter Ereignisschwellenwerte oder -bedingungen Warnungen ausgegeben, Vorfälle generiert, die Ihr SOC selektieren und untersuchen kann, und mit automatisierten Nachverfolgungs- und Korrekturprozessen auf Bedrohungen reagiert. 

In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Regeln zum Erkennen von Bedrohungen mit Azure Sentinel erstellen.

Nach Abschluss dieses Tutorials können Sie folgende Vorgänge durchführen:
> [!div class="checklist"]
> * Erstellen von Analyseregeln
> * Definieren, wie Ereignisse und Warnungen verarbeitet werden
> * Definieren, wie Warnungen und Vorfälle generiert werden
> * Auswählen automatisierter Reaktionen auf Bedrohungen für Ihre Regeln

## <a name="create-a-custom-analytics-rule-with-a-scheduled-query"></a>Erstellen einer benutzerdefinierten Analyseregel mit einer geplanten Abfrage

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Analysen** aus.

1. Wählen Sie auf der Aktionsleiste die Option **+ Erstellen** und anschließend **Geplante Abfrageregel** aus. Dadurch wird der **Assistent für Analyseregeln** geöffnet.

    :::image type="content" source="media/tutorial-detect-threats-custom/create-scheduled-query-small.png" alt-text="Erstellen einer geplanten Abfrage" lightbox="media/tutorial-detect-threats-custom/create-scheduled-query-full.png":::

### <a name="analytics-rule-wizard---general-tab"></a>Analyseregel-Assistent: Registerkarte „Allgemein“

- Geben Sie einen eindeutigen **Namen** und eine **Beschreibung** an. 

- Im Feld **Taktiken** stehen verschiedene Angriffskategorien zur Auswahl, nach denen die Regel klassifiziert werden kann. Diese basieren auf den Taktiken des [MITRE ATT&CK](https://attack.mitre.org/)-Frameworks.

- Legen Sie den **Schweregrad** der Warnung nach Bedarf fest. 

- Beim Erstellen der Regel wird der **Status** standardmäßig auf **Aktiviert** eingestellt. Dies bedeutet, dass die Regel nach der Erstellung sofort ausgeführt wird. Wenn Sie die Regel nicht sofort ausführen möchten, wählen Sie **Deaktiviert** aus. Die Regel wird der Registerkarte **Aktive Regeln** hinzugefügt, von wo Sie diese bei Bedarf aktivieren können.

   :::image type="content" source="media/tutorial-detect-threats-custom/general-tab.png" alt-text="Beginnen mit dem Erstellen einer benutzerdefinierten Analyseregel":::

## <a name="define-the-rule-query-logic-and-configure-settings"></a>Definieren der Regelabfragelogik und Konfigurieren von Einstellungen

Auf der Registerkarte **Regellogik festlegen** können Sie im Feld **Regelabfrage** eine Abfrage direkt eingeben. Alternativ können Sie eine Abfrage in Log Analytics erstellen, diese kopieren und in das Feld einfügen.

- Abfragen werden in der Kusto-Abfragesprache (KQL) geschrieben. Weitere Informationen zu KQL-[Konzepten](/azure/data-explorer/kusto/concepts/) und -[Abfragen](/azure/data-explorer/kusto/query/) finden Sie in diesem praktischen [Kurzreferenzhandbuch](/azure/data-explorer/kql-quick-reference).

- Im Beispiel in diesem Screenshot wird die Tabelle *SecurityEvent* abgefragt, um einen Typ von [fehlerhaften Windows-Anmeldeereignissen](/windows/security/threat-protection/auditing/event-4625) anzuzeigen.

   :::image type="content" source="media/tutorial-detect-threats-custom/set-rule-logic-tab-1-new.png" alt-text="Konfigurieren von Abfrageregellogik und Einstellungen" lightbox="media/tutorial-detect-threats-custom/set-rule-logic-tab-all-1-new.png":::

- Mit der folgenden Beispielabfrage werden Sie gewarnt, wenn im [Azure-Aktivitätsprotokoll](../azure-monitor/essentials/activity-log.md) eine ungewöhnliche Anzahl von Ressourcen erstellt wird.

    ```kusto
    AzureActivity
    | where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
    | where ActivityStatus == "Succeeded"
    | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller
    ```

    > [!NOTE]
    > #### <a name="rule-query-best-practices"></a>Bewährte Methoden für Abfragen
    > - Die Abfrage sollte zwischen 1 und 10.000 Zeichen lang sein und darf weder `search *` noch `union *` enthalten.
    >
    > - Das Erstellen von Azure Data Explorer-Abfragen mit ADX-Funktionen innerhalb des Log Analytics-Abfragefensters **wird nicht unterstützt**.
    >
    > - Wenn Sie bei Verwendung der **`bag_unpack`** -Funktion die Spalten mithilfe von `project field1` als Felder projizieren und eine Spalte nicht vorhanden ist, treten bei der Abfrage Fehler auf. Um dies zu verhindern, müssen Sie die Spalte wie folgt projizieren:
    >   - `project field1 = column_ifexists("field1","")`

### <a name="alert-enrichment"></a>Warnungsanreicherung

> [!IMPORTANT]
> Die Funktionen zur Warnungsanreicherung befinden sich derzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
    
- Im Konfigurationsbereich **Entitätszuordnung** können Sie Parameter aus den Abfrageergebnissen den in Azure Sentinel erkannten Entitäten zuordnen. Durch Entitäten wird die Ausgabe der Regeln (Warnungen und Vorfälle) um wesentliche Informationen erweitert, die als Bausteine für alle nachfolgenden Untersuchungsprozesse und Korrekturmaßnahmen dienen. Außerdem können Sie auf der Registerkarte **Incidenteinstellungen** Warnungen nach bestimmten Kriterien in Vorfällen gruppieren.

    Erfahren Sie mehr über [Entitäten in Azure Sentinel](entities-in-azure-sentinel.md).

    Unter [Zuordnen von Datenfeldern zu Entitäten in Azure Sentinel](map-data-fields-to-entities.md) finden Sie umfassende Anweisungen zur Entitätszuordnung sowie wichtige Informationen zur [Abwärtskompatibilität](map-data-fields-to-entities.md#notes-on-the-new-version).

- Im Konfigurationsbereich **Benutzerdefinierten Details** können Sie Ereignisdatenelemente aus der Abfrage extrahieren und in den durch die jeweilige Regel generierten Warnungen anzeigen. Dadurch erhalten Sie direkten Einblick in die Ereignisinhalte der Warnungen und Vorfälle.

    Weitere Informationen zum Anzeigen benutzerdefinierter Details in Warnungen finden Sie in den [vollständigen Anweisungen](surface-custom-details-in-alerts.md).

### <a name="query-scheduling-and-alert-threshold"></a>Abfrageplanung und Warnungsschwellenwert

- Legen Sie im Abschnitt **Abfrageplanung** die folgenden Parameter fest:

   :::image type="content" source="media/tutorial-detect-threats-custom/set-rule-logic-tab-2.png" alt-text="Festlegen von Abfragezeitplan und Ereignisgruppierung" lightbox="media/tutorial-detect-threats-custom/set-rule-logic-tab-all-2-new.png":::

    - Legen Sie unter **Abfrage ausführen alle** fest, wie häufig die Abfrage ausgeführt werden soll: häufig (alle fünf Minuten) oder selten (einmal alle 14 Tage).

    - Legen Sie unter **Datensuche für letzte** den Zeitraum der von der Abfrage abgedeckten Daten fest: z. B. Daten der letzten 10 Minuten oder der letzten 6 Stunden. Der Höchstwert ist 14 Tage.

        > [!NOTE]
        > **Abfrageintervalle und Rückblickperiode**
        >
        >  Diese beiden Einstellungen sind bis zu einem Punkt voneinander unabhängig. Sie können in einem kurzen Intervall eine Abfrage für einen Zeitraum ausführen, der länger ist als das Intervall (was zu sich überschneidenden Abfragen führt). Sie können jedoch für eine Abfrage kein Intervall festlegen, das den Abdeckungszeitraum überschreitet, da dies zu Lücken in der Gesamtabdeckung der Abfrage führen würde.
        >
        > **Erfassungsverzögerung**
        >
        > Azure Sentinel führt geplante Analyseregeln mit einer **fünfminütigen Verzögerung** nach der geplanten Zeit aus, um die **Wartezeit** zu berücksichtigen, die zwischen der Erstellung eines Ereignisses in der Quelle und dessen Erfassung in Azure Sentinel auftreten kann, und um eine vollständige Abdeckung ohne Datenduplizierung zu gewährleisten.
        >
        > Eine ausführliche technische Erklärung, warum diese Verzögerung notwendig ist und wie damit dieses Problem gelöst wird, finden Sie im Blogbeitrag von Ron Marsiano zu diesem Thema: [Handling ingestion delay in Azure Sentinel scheduled alert rules](https://techcommunity.microsoft.com/t5/azure-sentinel/handling-ingestion-delay-in-azure-sentinel-scheduled-alert-rules/ba-p/2052851) (Verarbeiten der Erfassungsverzögerung in geplanten Azure Sentinel-Warnungsregeln).

- Im Bereich **Warnungsschwellenwert** können Sie die Vertraulichkeitsstufe der Regel definieren. Legen Sie beispielsweise **Warnung generieren, wenn für die Anzahl der Abfrageergebnisse Folgendes gilt:** auf **Ist größer als** fest, und geben Sie die Zahl 1000 ein, wenn die Regel nur dann eine Warnung generieren soll, wenn die Abfrage bei jeder Ausführung mehr als 1000 Ergebnisse zurückgibt. Da dies ein Pflichtfeld ist, müssen Sie, wenn Sie keinen Schwellenwert festlegen möchten (d. h., wenn bei jedem Ereignis eine Warnung registriert werden soll), im Zahlenfeld den Wert „0“ eingeben.
    
### <a name="results-simulation"></a>Ergebnissimulation

Wenn Sie im Bereich **Ergebnissimulation** rechts im Assistenten die Option **Mit aktuellen Daten testen** auswählen, wird in Azure Sentinel ein Diagramm der Ergebnisse (Protokollereignisse) angezeigt, die die Abfrage in den letzten 50 Ausführungen entsprechend dem aktuell definierten Zeitplan generiert hätte. Wenn Sie die Abfrage ändern, wählen Sie **Mit aktuellen Daten testen** erneut aus, um das Diagramm zu aktualisieren. Das Diagramm zeigt die Anzahl der Ergebnisse im definierten Zeitraum, der durch die Einstellungen im Abschnitt **Abfrageplanung** bestimmt wird.
  
Die Ergebnissimulation für die Abfrage im obigen Screenshot könnte wie folgt aussehen. Die linke Seite ist die Standardansicht, und die rechte Seite zeigt, was Sie sehen, wenn Sie im Diagramm auf einen bestimmten Zeitpunkt zeigen.

:::image type="content" source="media/tutorial-detect-threats-custom/results-simulation.png" alt-text="Screenshots der Ergebnissimulation":::

Wenn die Abfrage zu viele oder zu häufige Warnungen generiert hätte, können Sie die Einstellungen in den [Bereichen **Abfrageplanung** und **Warnungsschwellenwert**](#query-scheduling-and-alert-threshold) anpassen und dann erneut **Mit aktuellen Daten testen** auswählen.

### <a name="event-grouping-and-rule-suppression"></a>Ereignisgruppierung und Unterdrückung von Regeln

> [!IMPORTANT]
> Die Ereignisgruppierung befindet sich derzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
    
- Wählen Sie unter **Ereignisgruppierung** eine von zwei Methoden aus, um **Ereignisse** in **Warnungen** zu gruppieren: 

    - **Group all events into a single alert** (Alle Ereignisse in einer einzelnen Warnung gruppieren) (Standardeinstellung). Die Regel generiert bei jeder Ausführung eine einzelne Warnung, solange die Anzahl der von der Abfrage zurückgegebenen Ergebnisse den oben angegebenen **Warnungsschwellenwert** überschreitet. Die Warnung enthält eine Zusammenfassung aller in den Ergebnissen zurückgegebenen Ereignisse. 

    - **Warnung für jedes Ereignis auslösen**. Die Regel generiert eine eindeutige Warnung für jedes Ereignis, das von der Abfrage zurückgegeben wird. Dies ist hilfreich, wenn die Ereignisse einzeln angezeigt oder nach bestimmten Parametern – etwa Benutzer oder Hostname – gruppiert werden sollen. Sie können diese Parameter in der Abfrage definieren.
    
        Derzeit ist die Anzahl von Warnungen, die eine Regel generieren kann, auf 20 begrenzt. Wenn in einer bestimmten Regel **Ereignisgruppierung** auf **Warnung für jedes Ereignis auslösen** festgelegt ist und die Abfrage der Regel mehr als 20 Ereignisse zurückgibt, generiert jedes der ersten 19 Ereignisse eine eindeutige Warnung, und die 20. Warnung fasst den gesamten Satz zurückgegebener Ereignisse zusammen. Mit anderen Worten: Die 20. Warnung ist die Warnung, die auch mit der Option **Alle Ereignisse in einer einzigen Warnung gruppieren** generiert wird.

    > [!NOTE]
    > Was ist der Unterschied zwischen **Ereignissen** und **Warnungen**?
    >
    > - Ein **Ereignis** ist eine Beschreibung eines einzelnen Vorkommens einer Aktion. Beispielsweise kann ein einzelner Eintrag in einer Protokolldatei als Ereignis gezählt werden. In diesem Kontext bezeichnet ein Ereignis ein einzelnes Ergebnis, das von einer Abfrage in einer Analyseregel zurückgegeben wird.
    >
    > - Eine **Warnung** ist eine Sammlung von Ereignissen, die gemeinsam im Hinblick auf die Sicherheit von Bedeutung sind. Eine Warnung kann ein einzelnes Ereignis beinhalten, wenn das Ereignis bedeutende Auswirkungen auf die Sicherheit hat, z. B. eine Administratoranmeldung aus dem Ausland außerhalb der Geschäftszeiten.
    >
    > - Und was sind **Incidents**? Die interne Logik von Azure Sentinel erstellt **Incidents** aus **Warnungen** oder Gruppen von Warnungen. Die Arbeit der SOC-Analysten – Selektierung, Untersuchung und Abhilfe – konzentriert sich auf die Vorfallwarteschlange.
    > 
    > Azure Sentinel erfasst unformatierte Ereignisse aus Datenquellen und bereits verarbeitete Warnungen von anderen. Es muss jederzeit unterschieden werden, ob es sich um Ereignisse oder Warnungen handelt.

- Im Abschnitt **Unterdrückung** können Sie die Einstellung **Ausführung der Abfrage beenden, wenn eine Warnung generiert wurde** auf **Ein** festlegen, wenn Sie nach dem Eingang einer Warnung das Ausführen dieser Regel für einen Zeitraum unterbrechen möchten, der das Abfrageintervall überschreitet. Wenn Sie diese Option aktivieren, müssen Sie unter **Abfrageausführung beenden für:** den Zeitraum festlegen, in dem die Abfrage nicht ausgeführt werden soll (bis zu 24 Stunden).

## <a name="configure-the-incident-creation-settings"></a>Konfigurieren der Einstellungen für die Incidenterstellung

Auf der Registerkarte **Incidenteinstellungen** können Sie auswählen, ob und wie Azure Sentinel Warnungen in verwertbare Incidents umwandelt. Wenn Sie diese Registerkarte unverändert lassen, erstellt Azure Sentinel einen eigenen separaten Incident von jeder einzelnen Warnung. Durch Ändern der Einstellungen auf dieser Registerkarte können Sie festlegen, dass keine Incidents erstellt oder mehrere Warnungen zu einem einzelnen Incident gruppiert werden.

> [!IMPORTANT]
> Die Registerkarte „Incidenteinstellungen“ befindet sich derzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

:::image type="content" source="media/tutorial-detect-threats-custom/incident-settings-tab.png" alt-text="Definieren der Einstellungen für Incidenterstellung und Warnungsgruppierung":::

### <a name="incident-settings"></a>Incidenteinstellungen

Im Bereich **Incidenteinstellungen** ist die Option **Incidents aus Warnungen erstellen, die von dieser Analyseregel ausgelöst werden** standardmäßig auf **Aktiviert** festgelegt. Dies bedeutet, dass in Azure Sentinel ein separater Vorfall von jeder einzelnen Warnung erstellt wird, die von der Regel ausgelöst wird.
    
- Wenn durch diese Regel nicht alle Incidents erstellt werden sollen (z. B. wenn Sie mit dieser Regel nur Informationen für die nachfolgende Analyse sammeln möchten), legen Sie diese Einstellung auf **Deaktiviert** fest.

- Informationen dazu, wie statt einem Vorfall für jede einzelne Warnung ein einzelner Vorfall aus einer Gruppe von Warnungen erstellt wird, finden Sie im nächsten Abschnitt.

### <a name="alert-grouping"></a>Warnungsgruppierung

Wenn aus einer Gruppe von bis zu 150 ähnlichen oder wiederkehrenden Warnungen (siehe Hinweis) ein einzelner Incident erstellt werden soll, legen Sie im Abschnitt **Warnungsgruppierung** die Option **Alle verwandten Warnungen, die durch diese Analyseregel ausgelöst werden, in Incidents gruppieren** auf **Aktiviert** fest, und legen Sie die folgenden Parameter fest.

- **Gruppe auf Warnungen beschränken, die innerhalb des ausgewählten Zeitraums erstellt werden**: Bestimmen Sie den Zeitraum, in dem ähnliche oder wiederkehrende Warnungen gruppiert werden sollen. Alle entsprechenden Warnungen innerhalb dieses Zeitraums generieren zusammen einen Incident oder eine Gruppe von Incidents (abhängig von den unten aufgeführten Gruppierungseinstellungen). Bei Warnungen außerhalb dieses Zeitraums wird ein separater Incident oder eine Reihe von Incidents generiert.

- **Von dieser Analyseregel ausgelöste Warnungen in einem einzigen Incident zusammenfassen und gruppieren nach**: Wählen Sie die Grundlage für die Gruppierung der Warnungen aus:

    - **Gruppieren von Warnungen in einem einzigen Incident, wenn alle Entitäten übereinstimmen**: Warnungen werden gruppiert, wenn sie identische Werte für jede der zugeordneten Entitäten aufweisen (weiter oben definiert auf der Registerkarte „Regellogik festlegen“). Dies ist die empfohlene Einstellung.

    - **Gruppieren aller von dieser Regel ausgelösten Warnungen in einem einzigen Incident**: Alle von dieser Regel generierten Warnungen werden zusammengefasst, auch wenn sie keine identischen Werte haben.

    - **Gruppieren von Warnungen in einem einzigen Incident, wenn die ausgewählten Entitäten übereinstimmen**: Warnungen werden gruppiert, wenn sie identische Werte für einige der zugeordneten Entitäten (Auswahl in Dropdownliste) aufweisen. Sie sollten diese Einstellung verwenden, wenn Sie z. B. separate Incidents auf Basis der Quell- oder Ziel-IP-Adressen erstellen möchten.

- **Geschlossene übereinstimmende Incidents erneut öffnen**: Wenn ein Incident gelöst oder geschlossen wurde und später eine weitere Warnung generiert wird, die zu diesem Incident gehören würde, haben Sie folgende Möglichkeiten: Legen Sie diese Einstellung auf **Aktiviert** fest, wenn der geschlossene Incident erneut geöffnet werden soll, oder übernehmen Sie die Einstellung **Deaktiviert**, wenn durch die Warnung ein neuer Incident erzeugt werden soll.
    
    > [!NOTE]
    > **Bis zu 150 Warnungen** können in einem einzelnen Vorfall gruppiert werden. Wenn mehr als 150 Warnungen von einer Regel generiert werden, die sie zu einem einzelnen Incident gruppiert, wird ein neuer Incident mit denselben Incidentinformationen wie der ursprüngliche Incident generiert, und die überzähligen Warnungen werden in dem neuen Incident gruppiert.

## <a name="set-automated-responses-and-create-the-rule"></a>Festlegen automatisierter Antworten und Erstellen der Regel

1. Wählen Sie auf der Registerkarte **Automatische Reaktionen** alle Playbooks aus, die automatisch ausgeführt werden sollen, wenn durch die benutzerdefinierte Regel eine Warnung generiert wird. Weitere Informationen zum Erstellen und Automatisieren von Playbooks finden Sie unter [Reaktionen auf Bedrohungen](tutorial-respond-threats-playbook.md).

    :::image type="content" source="media/tutorial-detect-threats-custom/automated-response-tab.png" alt-text="Definieren der Einstellungen für automatisierte Antworten":::

1. Wählen Sie **Überprüfen und erstellen** aus, um alle Einstellungen für die neue Warnungsregel zu überprüfen. Nachdem die Meldung „Überprüfung erfolgreich“ angezeigt wurde, wählen Sie **Erstellen** aus, um die Warnungsregel zu initialisieren.

    :::image type="content" source="media/tutorial-detect-threats-custom/review-and-create-tab.png" alt-text="Überprüfen aller Einstellungen und Erstellen der Regel":::

## <a name="view-the-rule-and-its-output"></a>Anzeigen der Regel und ihrer Ausgabe
  
- Die neu erstellte benutzerdefinierte Regel (vom Typ „Geplant“) finden Sie in der Tabelle auf der Registerkarte **Aktive Regeln** auf dem Hauptbildschirm von **Analysen**. Über diese Liste können Sie die einzelnen Regeln aktivieren, deaktivieren oder löschen.

- Die Ergebnisse der von Ihnen erstellten Warnungsregeln können Sie auf der Seite **Vorfälle** anzeigen. Hier können Sie Bedrohungen eingrenzen, [Vorfälle untersuchen](tutorial-investigate-cases.md) und die Bedrohungen beseitigen.

> [!NOTE]
> In Azure Sentinel generierte Warnungen stehen über [Microsoft Graph Security](/graph/security-concept-overview) zur Verfügung. Weitere Informationen finden Sie unter [Verwenden der Sicherheits-API von Microsoft Graph](/graph/api/resources/security-api-overview).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="issue-no-events-appear-in-query-results"></a>Problem: Keine Ereignisse in den Abfrageergebnissen

Wenn **Ereignisgruppierung** auf **Warnung für jedes Ereignis auslösen** festgelegt ist, kann es in bestimmten Szenarien vorkommen, dass bei einem späteren Anzeigen der Abfrageergebnisse (z. B. bei der Pivotierung von Warnungen eines Incidents) keine Abfrageergebnisse angezeigt werden. Dies liegt daran, dass die Verknüpfung zwischen Ereignis und Warnung durch Hashing der Informationen des jeweiligen Ereignisses und das Einfügen des Hash in die Abfrage erreicht wird. Wenn die Abfrageergebnisse seit der Generierung der Warnung geändert wurden, ist der Hash nicht mehr gültig, und es werden keine Ergebnisse angezeigt. 

Um die Ereignisse anzuzeigen, entfernen Sie die Zeile mit dem Hash aus der Abfrage der Regel und führen die Abfrage aus.

### <a name="issue-a-scheduled-rule-failed-to-execute-or-appears-with-auto-disabled-added-to-the-name"></a>Problem: Fehler bei der Ausführung einer geplanten Regel oder beim Hinzufügen von „AUTO DISABLED“ zum Namen

Ein Fehler bei der Ausführung einer geplanten Abfrage ist zwar selten, aber nicht ausgeschlossen. Azure Sentinel klassifiziert Fehler auf der Grundlage des spezifischen Fehlertyps und der Umstände, die dazu geführt haben, als vorübergehende oder dauerhafte Fehler.

#### <a name="transient-failure"></a>Vorübergehender Fehler

Ein vorübergehender Fehler tritt aufgrund einer temporären Situation auf, die sich bald wieder normalisiert. Zu diesem Zeitpunkt wird die Regelausführung erfolgreich angewendet. Dies sind einige Beispiele für Fehler, die von Azure Sentinel als vorübergehend klassifiziert werden:

- Die Ausführung einer Regelabfrage dauert zu lang und führt zu einem Timeout.
- Es bestehen Konnektivitätsprobleme zwischen Datenquellen und Log Analytics bzw. zwischen Log Analytics und Azure Sentinel.
- Alle anderen neuen und unbekannten Fehler werden als vorübergehende Fehler eingestuft.

Wenn ein vorübergehender Fehler auftritt, versucht Azure Sentinel weiterhin, die Regel gemäß den vordefinierten und immer kürzeren Intervallen bis zu einem bestimmten Punkt noch mal auszuführen. Danach wird die Regel nur zum nächsten geplanten Zeitpunkt wieder ausgeführt. Eine Regel wird aufgrund eines vorübergehenden Fehlers nie automatisch deaktiviert.

#### <a name="permanent-failure---rule-auto-disabled"></a>Dauerhafter Fehler: Regel automatisch deaktiviert

Ein dauerhafter Fehler tritt aufgrund einer Änderung der Bedingungen auf, die normalerweise die Ausführung der Regel ermöglichen und ohne menschliches Eingreifen nicht wiederhergestellt werden können. Im Folgenden finden Sie einige Beispiele für Fehler, die als permanente Fehler klassifiziert werden:

- Der Zielarbeitsbereich (für den die Regelabfrage ausgeführt wurde) wurde gelöscht.
- Die Zieltabelle (für die die Regelabfrage ausgeführt wurde) wurde gelöscht.
- Azure Sentinel wurde aus dem Zielarbeitsbereich entfernt.
- Eine von der Regelabfrage verwendete Funktion ist nicht mehr gültig. Sie wurde entweder geändert oder entfernt.
- Die Berechtigungen für eine der Datenquellen der Regelabfrage wurden geändert.
- Eine der Datenquellen der Regelabfrage wurde gelöscht oder getrennt.

**Im Fall einer vordefinierten Anzahl aufeinanderfolgender dauerhafter Fehler mit demselben Typ und derselben Regel** versucht Azure Sentinel nicht mehr, die Regel auszuführen und führt stattdessen die folgenden Schritte aus:

- Die Regel wird deaktiviert.
- Die Wörter **AUTO DISABLED** werden am Anfang des Regelnamens hinzugefügt.
- Der Grund für den Fehler (und die Deaktivierung) wird der Regelbeschreibung hinzugefügt.

Sie können mühelos erkennen, ob automatisch deaktivierte Regeln vorhanden sind, indem Sie die Regelliste nach Namen sortieren. Die automatisch deaktivierten Regeln werden am Anfang der Liste angezeigt.

SOC-Manager sollten sicherstellen, dass die Regelliste regelmäßig auf automatisch deaktivierte Regeln überprüft wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden.

- Informationen zum [Untersuchen von Vorfällen in Azure Sentinel](tutorial-investigate-cases.md)
- Informationen zu [Entitäten in Azure Sentinel](entities-in-azure-sentinel.md)
- Informationen zum [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md)