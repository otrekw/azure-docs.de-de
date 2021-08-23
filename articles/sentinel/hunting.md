---
title: Bedrohungssuchfunktionen in Azure Sentinel | Microsoft-Dokumentation
description: Verwenden Sie die integrierten Suchabfragen von Azure Sentinel, um die richtigen Fragen zu stellen, mit denen Sie Probleme in Ihren Daten finden können.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2021
ms.author: yelevin
ms.openlocfilehash: a8bf6ec24a67327d715b4a5d09b1d0d633b980c5
ms.sourcegitcommit: ce9178647b9668bd7e7a6b8d3aeffa827f854151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109810370"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>Suchen nach Bedrohungen mit Azure Sentinel

> [!IMPORTANT]
>
> - Upgrades für das **Bedrohungssuche-Dashboard** befinden sich derzeit in der **VORSCHAU**. Die folgenden Elemente im Zusammenhang mit diesem Upgrade werden als „(Vorschau)“ markiert. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

Als Sicherheitsanalysten und Prüfer möchten Sie proaktiv nach Sicherheitsbedrohungen suchen, aber Ihre verschiedenen Systeme und Sicherheits-Appliances generieren eine Menge von Daten, deren Analyse und Filterung in aussagekräftige Ereignisse schwierig sein können. Azure Sentinel verfügt über leistungsstarke Such- und Abfragetools zum Aufspüren von Sicherheitsbedrohungen in den Datenquellen Ihrer Organisation. Um Sicherheitsanalysten dabei zu unterstützen, proaktiv nach neuen Anomalien zu suchen, die von Ihren Sicherheits-Apps oder auch Ohren Regeln zur geplanten Analyse nicht erkannt wurden, werden Sie von den in Azure Sentinel integrierten Bedrohungssuchabfragen dazu geführt, die richtigen Fragen zu stellen, um Probleme in den Daten zu finden, die bereits in Ihrem Netzwerk vorhanden sind. 

Zum Beispiel liefert eine integrierte Abfrage Daten über die ungewöhnlichsten Prozesse, die in Ihrer Infrastruktur ausgeführt werden. Sie möchten nicht, dass bei jeder Ausführung eines solchen Prozesse eine Benachrichtigung erfolgt, denn er könnte völlig unproblematisch sein, aber Sie möchten die Abfrage gelegentlich ausführen, um zu sehen, ob es etwas Ungewöhnliches gibt. 

Mit Azure Sentinel-Bedrohungssuche (Ermittlung) können Sie die folgenden Funktionen nutzen:

- **Integrierte Abfragen**: Die Hauptseite für die Bedrohungssuche, auf die über das Azure Sentinel-Navigationsmenü zugegriffen werden kann, enthält vorgefertigte Abfragebeispiele, die Ihnen den Einstieg erleichtern und Sie mit den Tabellen und der Abfragesprache vertraut machen. Diese integrierten Bedrohungssuchabfragen werden von Microsoft-Sicherheitsexperten kontinuierlich weiterentwickelt. Dies geschieht durch Hinzufügen neuer Abfragen und Optimieren vorhandener Abfragen, um Ihnen einen Einstiegspunkt für die Suche nach neuen Erkennungen bereitzustellen und Ihnen zu ermöglichen zu bestimmen, wo Sie nach den Anfängen neuer Angriffe suchen sollten. 

- **Leistungsstarke Abfragesprache mit IntelliSense**: Bedrohungssuchabfragen sind in die [Kusto-Abfragesprache (KQL)](/azure/data-explorer/kusto/query/) integriert, eine Abfragesprache, die Ihnen die Leistungsfähigkeit und Flexibilität bietet, die Sie für die nächste Stufe der Bedrohungssuche benötigen. Es ist dieselbe Sprache, die von den Abfragen in Ihren Analyseregeln und an anderer Stelle in Azure Sentinel verwendet wird.

- **Bedrohungssuche-Dashboard (Vorschau)** : Mit diesem Upgrade der Hauptseite können Sie alle Ihre Abfragen oder eine ausgewählte Teilmenge mit einem einzigen Klick ausführen. Ermitteln Sie, wo mit der Suche begonnen werden soll, indem Sie sich die Ergebnisanzahl, Spitzen oder die Änderung der Ergebnisanzahl über einen Zeitraum von 24 Stunden ansehen. Sie können auch nach Favoriten, Datenquelle, MITRE ATT&CK-Taktik und --Verfahren, Ergebnissen oder Ergebnisdelta sortieren und filtern. Zeigen Sie die Abfragen an, mit denen die erforderlichen Datenquellen noch nicht verbunden sind, und erhalten Sie Empfehlungen zum Aktivieren dieser Abfragen.

- **Erstellen Sie Ihre eigenen Lesezeichen**: Während des Bedrohungssuchprozesses stoßen Sie möglicherweise auf Abfrageergebnisse, die ungewöhnlich oder verdächtig aussehen. Sie können diese Elemente mit einem „Lesezeichen“ versehen und sie speichern und beiseitelegen, um in Zukunft darauf zurückgreifen zu können. Sie können Ihre mit Lesezeichen gekennzeichneten Elemente verwenden, um einen Incident zur Untersuchung zu erstellen oder anzureichern. Weitere Informationen zu Lesezeichen finden Sie unter [Verwenden von Lesezeichen bei der Bedrohungssuche](bookmarks.md).

- **Verwenden Sie Notebooks, um die Untersuchung zu ermöglichen**: Notebooks bieten Ihnen eine Art virtuelle Sandboxumgebung mit eigenem Kernel. Sie können Notebooks verwenden, um Ihre Bedrohungssuche und Untersuchungen durch maschinelles Lernen, Visualisierung und Datenanalyse zu verbessern. Sie können eine vollständige Untersuchung in einem Notebook durchführen, die Rohdaten, den Code, den Sie darauf ausführen, die Ergebnisse und ihre Visualisierungen kapseln und das Ganze speichern, damit es für andere Benutzer in Ihrer Organisation freigegeben und wiederverwendet werden kann. 

- **Fragen Sie die gespeicherten Daten ab**: Die Daten sind in Tabellen zugänglich, die Sie abfragen können. Beispielsweise können Sie Prozesserstellung, DNS-Ereignisse und viele andere Ereignistypen abfragen.

- **Links zur Community**: Nutzen Sie die Leistungsfähigkeit der größeren Community, um weitere Abfragen und Datenquellen zu finden.
 
## <a name="get-started-hunting"></a>Beginnen mit der Bedrohungssuche (Hunting)

Klicken Sie im Azure Sentinel-Portal auf **Suche** (Hunting).

   :::image type="content" source="media/hunting/hunting-start.png" alt-text="Azure Sentinel beginnt mit der Bedrohungssuche" lightbox="media/hunting/hunting-start.png":::

- Wenn Sie die Seite **Suche** (Hunting) öffnen, werden alle Bedrohungssuchabfragen in einer einzelnen Tabelle angezeigt. In der Tabelle werden alle Abfragen, die von Microsofts Team der Sicherheitsanalysten geschrieben wurden, sowie alle weiteren Abfragen aufgelistet, die Sie erstellt oder geändert haben. Jede Abfrage enthält eine Beschreibung dazu, nach welcher Bedrohung mit ihr gesucht wird und für welche Art von Daten sie ausgeführt wird. Diese Vorlagen sind nach ihren verschiedene Taktiken gruppiert: Die Symbole auf der rechten Seite kategorisieren den Typ der Bedrohung, etwa Erstzugriff, Persistenz und Exfiltration.

- (Vorschau) Um zu sehen, wie die Abfragen für Ihre Umgebung gelten, klicken Sie auf die Schaltfläche **Alle Abfragen ausführen (Vorschau)** , oder wählen Sie mithilfe der Kontrollkästchen links neben den einzelnen Zeilen eine Teilmenge von Abfragen und dann die Schaltfläche **Ausgewählte Abfragen ausführen (Vorschau)** aus. Die Ausführung der Abfragen kann von wenigen Sekunden bis zu mehreren Minuten dauern. Dies hängt von der Anzahl der ausgewählten Abfragen, des Zeitraums und der abgefragten Datenmenge ab.

- (Vorschau) Sobald Ihre Abfragen ausgeführt wurden, können Sie mit dem Filter **Ergebnisse** anzeigen, welche Abfragen Ergebnisse zurückgegeben haben. Anschließend können Sie diese sortieren, um zu sehen, welche Abfragen die meisten oder die wenigsten Ergebnisse lieferten. Sie können auch sehen, welche Abfragen in Ihrer Umgebung nicht aktiv sind, indem Sie im Filter **Ergebnisse** die Option *Nicht zutreffend* auswählen. Bewegen Sie den Mauszeiger über das Infosymbol (i) neben *Nicht zutreffend*, um zu sehen, welche Datenquellen erforderlich sind, um diese Abfrage zu aktivieren.

- (Vorschau) Sie können Datenspitzen identifizieren, indem Sie nach **Ergebnisse – Delta** sortieren oder filtern. Dadurch werden die Ergebnisse der letzten 24 Stunden mit den Ergebnissen der vorherigen 24 bis 48 Stunden verglichen, um große Volumenunterschiede besser zu erkennen.

- (Vorschau) Die **MITRE ATT&CK-Taktikleiste** oben in der Tabelle zeigt an, wie viele Abfragen jeder MITRE ATT&CK-Taktik zugeordnet sind. Die Taktikleiste wird basierend auf dem aktuellen Satz angewendeter Filter dynamisch aktualisiert. Dies ist eine einfache Möglichkeit zu sehen, welche MITRE ATT&CK-Taktiken angezeigt werden, wenn Sie nach einer bestimmten Ergebnisanzahl, einem hohen Ergebnisdelta, *Nicht zutreffend*-Ergebnissen oder einem anderen Satz von Filtern filtern.

- (Vorschau) Die Abfragen können auch MITRE ATT&CK-Techniken zugeordnet werden. Sie können mithilfe des Filters **Technik** nach MITRE ATT&CK-Techniken filtern oder sortieren. Wenn Sie eine Abfrage öffnen, können Sie auf die Technik klicken, um die MITRE ATT&CK-Beschreibung der Technik anzuzeigen.

- Sie können jede Abfrage in Ihren Favoriten speichern. In Ihren Favoriten gespeicherte Abfragen werden jedes Mal automatisch ausgeführt, wenn auf die **Bedrohungssuche-Seite** zugegriffen wird. Sie können eine eigene Bedrohungssuchabfrage erstellen oder eine vorhandene Vorlage für Bedrohungssuchabfragen klonen oder anpassen.
 
- Indem Sie auf der Seite mit den Details der Bedrohungssuchabfrage auf **Abfrage ausführen** klicken, können Sie eine Abfrage ausführen, ohne die Bedrohungssuchseite zu verlassen. Die Anzahl der Übereinstimmungen wird in der Tabelle in der Spalte **Ergebnisse** angezeigt. Überprüfen Sie die Liste der Bedrohungssuchabfragen und deren Übereinstimmungen.

- Sie können eine schnelle Überprüfung der zugrunde liegenden Abfrage im Bereich mit den Abfragedetails durchführen. Sie können die Ergebnisse anzeigen, indem Sie auf den Link **Abfrageergebnisse anzeigen** (unterhalb des Abfragefensters) oder auf die Schaltfläche **Ergebnisse anzeigen** (unten im Bereich) klicken. Die Abfrage wird auf dem Blatt **Protokolle** (Log Analytics) geöffnet, und unterhalb der Abfrage können Sie die Übereinstimmungen für die Abfrage überprüfen.

- Um verdächtige oder interessante Ergebnisse aus einer Abfrage in Log Analytics aufzubewahren, markieren Sie die Kontrollkästchen der Zeilen, die Sie beibehalten möchten, und wählen Sie **Lesezeichen hinzufügen** aus. Dadurch wird für jede markierte Zeile ein Datensatz (ein Lesezeichen) erstellt, das die Zeilenergebnisse, die Abfrage, die die Ergebnisse erstellt hat, sowie Entitätszuordnungen zum Extrahieren von Benutzern, Hosts und IP-Adressen enthält. Sie können jedem Lesezeichen Ihre eigenen Tags (siehe unten) und Notizen hinzufügen.

- Sie können alle mit Lesezeichen markierten Ergebnisse anzeigen, indem Sie auf der Hauptseite der **Bedrohungssuche** auf die Registerkarte **Lesezeichen** klicken. Sie können Tags zu Lesezeichen hinzufügen, um sie für die Filterung zu klassifizieren. Wenn Sie beispielsweise eine Angriffskampagne untersuchen, können Sie ein Tag für die Kampagne erstellen, das Tag allen relevanten Lesezeichen hinzufügen und dann alle Lesezeichen auf Basis der Kampagne filtern.

- Sie können ein einzelnes, mit Lesezeichen markiertes Ergebnis untersuchen, indem Sie das Lesezeichen auswählen und dann im Detailbereich auf **Untersuchen** klicken, um die Untersuchungserfahrung zu öffnen. Sie können einen Incident auch aus einem oder mehreren Lesezeichen erstellen oder einem vorhandenen Incident ein oder mehrere Lesezeichen hinzufügen, indem Sie die Kontrollkästchen links neben den gewünschten Lesezeichen aktivieren und dann im Dropdownmenü **Incidentaktionen** am oberen Rand des Bildschirms entweder **Neuen Incident erstellen** oder **Zu vorhandenem Incident hinzufügen** auswählen. Anschließend können Sie den Incident wie jeden anderen selektieren und untersuchen.

- Nachdem Sie eine Bedrohungssuchabfrage ermittelt oder erstellt haben, die wertvolle Erkenntnisse über mögliche Angriffe liefert, können Sie basierend auf der Abfrage benutzerdefinierte Erkennungsregeln erstellen und diese Erkenntnisse als Warnungen in Ihren Antwortdiensten für Sicherheitsvorfälle anzeigen. Zeigen Sie die Ergebnisse der Abfrage in Log Analytics an (siehe oben), klicken Sie anschließend oben im Bereich auf die Schaltfläche **Neue Warnungsregel**, und wählen Sie dann **Azure Sentinel-Warnung erstellen** aus. Der **Assistent für Analyseregel** wird geöffnet. Führen Sie die erforderlichen Schritte aus, wie unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](tutorial-detect-threats-custom.md) erläutert.

## <a name="query-language"></a>Abfragesprache 

Hunting (Bedrohungssuche) in Azure Sentinel basiert auf der Kusto-Abfragesprache. Weitere Informationen zu der Abfragesprache und den unterstützten Operatoren finden Sie unter [Erste Schritte mit Azure Monitor-Protokollabfragen](../azure-monitor/logs/get-started-queries.md).

## <a name="public-hunting-query-github-repository"></a>Öffentliches GitHub-Repository für Bedrohungssuchabfragen (Hunting-Abfragen)

Sehen Sie sich das [Repository für Bedrohungssuchabfragen](https://github.com/Azure/Azure-Sentinel/tree/master/Hunting%20Queries) an. Tragen Sie eigene Abfragen bei, und verwenden Sie Beispielabfragen, die von unseren Kunden geteilt wurden.

 ## <a name="sample-query"></a>Beispielabfrage

Eine typische Abfrage beginnt mit einem Tabellennamen, gefolgt von einer Reihe von Operatoren, die durch \| getrennt sind.

Beginnen Sie im obigen Beispiel mit dem Tabellennamen „SecurityEvent“, und fügen Sie nach Bedarf über Pipes verkettete Elemente hinzu.

1. Definieren Sie einen Zeitfilter, um nur Datensätze aus den letzten sieben Tagen zu überprüfen.

1. Fügen Sie der Abfrage einen Filter hinzu, um nur Ereignisse mit der ID 4688 anzuzeigen.

1. Fügen Sie der Abfrage einen Filter für die Befehlszeile (CommandLine) hinzu, sodass nur Instanzen von „cscript.exe“ enthalten sind.

1. Wählen Sie über „project“ nur die Spalten aus, Sie untersuchen möchten, begrenzen Sie die Ergebnisse auf 1000, und klicken Sie auf **Abfrage ausführen**.

1. Klicken Sie auf das grüne Dreieck, um die Abfrage auszuführen. Sie können die Abfrage testen, und sie ausführen, um nach anormalem Verhalten zu suchen.

## <a name="useful-operators"></a>Hilfreiche Operatoren

Die Abfragesprache ist leistungsstark und hat zahlreiche Operatoren, von denen einige besonders nützliche hier aufgeführt sind:

**where**: Filtert eine Tabelle auf die Teilmenge von Zeilen, die ein Prädikat erfüllen.

**summarize**: Erzeugt eine Tabelle, in der der Inhalt der Eingabetabelle aggregiert ist.

**join**: Führt die Zeilen zweier Tabellen zusammen, um eine neue Tabelle zu erzeugen, indem Werte aus den angegebenen Spalten aus beiden Tabellen zugeordnet werden.

**count**: Gibt die Anzahl von Datensätzen in der Eingabedatensatzgruppe zurück.

**top**: Gibt die ersten N Datensätze zurück, wobei diese nach den angegebenen Spalten sortiert sind.

**limit**: Gibt maximal die angegebene Anzahl von Zeilen zurück.

**project**: Wählt die Spalten aus, die einbezogen, umbenannt oder gelöscht werden sollen, und fügt neue berechnete Spalten ein.

**extend**: Erstellt berechnete Spalten und fügt diese an das Resultset an.

**makeset**: Gibt ein dynamisches(JSON) Array der Menge unterschiedlicher Werte zurück, die ein Ausdruck (Expr) in der Gruppe annimmt.

**find**: Sucht nach Zeilen, die einem Prädikat in einer Gruppe von Tabellen entsprechen.

## <a name="save-a-query"></a>Speichern einer Abfrage

Sie können eine Abfrage erstellen oder ändern, und Sie können sie als Ihre eigene Abfrage speichern oder für Benutzer freigeben, die sich im selben Mandanten befinden.

:::image type="content" source="./media/hunting/save-query.png" alt-text="Speichern der Abfrage" lightbox="./media/hunting/save-query.png":::

### <a name="create-a-new-hunting-query"></a>Erstellen einer neuen Bedrohungssuchabfrage

1. Klicken Sie auf **Neue Abfrage**.

1. Füllen Sie alle leeren Felder aus, und wählen Sie **Erstellen** aus.

    :::image type="content" source="./media/hunting/new-query.png" alt-text="Neue Abfrage" lightbox="./media/hunting/new-query.png":::

### <a name="clone-and-modify-an-existing-hunting-query"></a>Klonen und Ändern einer vorhandenen Bedrohungssuchabfrage

1. Wählen Sie in der Tabelle die Bedrohungssuchabfrage aus, die Sie ändern möchten.

1. Wählen Sie in der Zeile der Abfrage, die Sie ändern möchten, die Auslassungspunkte (...) aus, und wählen Sie **Abfrage klonen**.

    :::image type="content" source="./media/hunting/clone-query.png" alt-text="Abfrage klonen" lightbox="./media/hunting/clone-query.png":::

1. Ändern Sie die Abfrage, und wählen Sie **Erstellen** aus.

    :::image type="content" source="./media/hunting/custom-query.png" alt-text="Benutzerdefinierte Abfrage" lightbox="./media/hunting/custom-query.png":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine Bedrohungsuntersuchung mit Azure Sentinel ausführen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- [Verwenden von Notebooks zur Ermittlung von Anomalien](notebooks.md)
- [Behalten des Überblicks über Daten bei einer Bedrohungssuche](bookmarks.md)
