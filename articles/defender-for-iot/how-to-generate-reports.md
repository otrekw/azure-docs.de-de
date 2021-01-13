---
title: Generieren von Berichten
description: Verschaffen Sie sich mithilfe der Berichterstellungstools von Defender für IoT Einblicke in Netzwerkaktivitäten, Gefahren, Angriffe und Trends.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/17/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: d0c3f531ede0a590a6ba7bb21c6edbd768c08069
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836190"
---
# <a name="generate-reports"></a>Generieren von Berichten

Verschaffen Sie sich mithilfe der Berichterstellungstools von Azure Defender für IoT Einblicke in Netzwerkaktivitäten, Gefahren, Angriffe und Trends. Es stehen Tools zum Generieren von Berichten auf diesen Grundlagen zur Verfügung:

- Basierend auf Aktivität, die von einzelnen Sensoren erkannt wird.
- Basierend auf Aktivität, die von allen Sensoren erkannt wird. 

Diese Berichte werden von der lokalen Verwaltungskonsole generiert.

## <a name="reports-for-sensor-risk-assessment"></a>Berichte zur Risikobewertung mithilfe von Sensoren

Mithilfe von Berichten zur Risikobewertung können Sie eine Sicherheitsbewertung für jedes Netzwerkgerät sowie eine Sicherheitsbewertung für das gesamte Netzwerk erstellen. Das Gesamtergebnis gibt den Prozentsatz bezogen auf 100 Prozent Sicherheit an.

Diese Bewertung basiert auf Ergebnissen aus der Paketüberprüfung, Engines zur Verhaltensmodellierung und einem SCADA-spezifischen Entwurf von Zustandsautomaten. Weitere Informationen stehen in großem Umfang zur Verfügung, darunter:

- Konfigurationsprobleme

- Sicherheitsrisiko von Geräten, nach Sicherheitsstufe priorisiert

- Netzwerksicherheitsprobleme

- Netzwerkbetriebsprobleme

- Angriffsvektoren

- Verbindungen mit ICS-Netzwerken

- Internetverbindungen

- Indikatoren für industrielle Schadsoftware

- Protokollprobleme

  - Sichere Geräte: Geräte mit einer Sicherheitsbewertung oberhalb von 90 %.

- **Sichere Geräte**: Geräte mit einer Sicherheitsbewertung oberhalb von 90 Prozent.

- **Sicherheitsanfällige Geräte**: Geräte mit einer Sicherheitsbewertung unterhalb von 70 Prozent.

- **Geräte, die der Verbesserung bedürfen**: Geräte mit einer Sicherheitsbewertung zwischen 70 Prozent und 89 Prozent.

So erstellen Sie einen Bericht

1. Wählen Sie im seitlichen Menü **Risk Assessment** (Risikobewertung) aus.
2. Wählen Sie in der Dropdownliste **Select Sensor** (Sensor auswählen) einen Sensor aus.
3. Wählen Sie **Generate Report** (Bericht generieren) aus.
4. Wählen Sie im Abschnitt „Archivierte Berichte“ **Download** (Herunterladen) aus.

:::image type="content" source="media/how-to-generate-reports/risk-assessment.png" alt-text="Ansicht der Risikobewertung.":::

So importieren Sie ein Firmenlogo:

So importieren Sie ein Firmenlogo:

- Wählen Sie **Logo importieren** aus.

## <a name="reports-for-sensor-attack-vector"></a>Berichte zu Angriffsvektoren mithilfe von Sensoren

Angriffsvektorberichte bieten eine grafische Darstellung einer Risikokette von Geräten mit Sicherheitslücken. Durch diese Sicherheitslücken kann ein Angreifer Zugriff auf wichtige Netzwerkgeräte erhalten. Der Angriffsvektorsimulator berechnet Angriffsvektoren in Echtzeit und analysiert alle Angriffsvektoren für ein bestimmtes Ziel.

Das Arbeiten mit dem Angriffsvektor ermöglicht es Ihnen, die Wirkung von Entschärfungsaktivitäten in der Angriffssequenz zu bewerten. Sie können dann beispielsweise bestimmen, ob ein Systemupgrade dem Angreifer durch Unterbrechen der Angriffskette den Weg abschneidet oder ob ein alternativer Angriffsweg bestehen bleibt. Diese Informationen helfen Ihnen bei der Priorisierung von Wartungs- und Entschärfungsaktivitäten.

:::image type="content" source="media/how-to-generate-reports/control-center.png" alt-text="Darstellung Ihrer Benachrichtigungen im Kontrollcenter.":::

> [!NOTE]
> Administratoren und Sicherheitsanalysten können die in diesem Abschnitt beschriebenen Verfahren ausführen.

So erstellen Sie eine Angriffsvektorsimulation:

1. Wählen Sie im seitlichen Menü das :::image type="content" source="media/how-to-generate-reports/plus.png" alt-text="Pluszeichen"::: aus, um eine Simulation hinzuzufügen.

 :::image type="content" source="media/how-to-generate-reports/vector.png" alt-text="Die Angriffsvektorsimulation.":::

2. Geben Sie Simulationseigenschaften ein:

   - **Name**: Name der Simulation.

   - **Maximum vectors** (Maximale Anzahl Vektoren): Die maximale Anzahl der Vektoren in einer einzelnen Simulation.

   - **Show in Device map** (In der Geräteübersicht anzeigen): Den Angriffsvektor als Vektor Filter in der Geräteübersicht darstellen.

   - **All Source devices** (Alle Quellgeräte): Der Angriffsvektor berücksichtigt alle Geräte als Angriffsquelle.

   - **Attack Source** (Angriffsquelle): Der Angriffsvektor berücksichtigt nur die angegebenen Geräte als Angriffsquelle.

   - **All Target devices** (Alle Zielgeräte): Der Angriffsvektor berücksichtigt alle Geräte als Angriffsziel.

   - **Attack Target** (Angriffsziel): Der Angriffsvektor berücksichtigt nur die angegebenen Geräte als Angriffsziel.

   - **Exclude devices** (Geräte ausschließen): Die angegebenen Geräte werden aus der Simulation des Angriffsvektors ausgeschlossen.

   - **Exclude Subnets** (Subnetze ausschließen): Die angegebenen Subnetze werden aus der Simulation des Angriffsvektors ausgeschlossen.

3. Wählen Sie **Add Simulation** (Simulation hinzufügen) aus. Die Simulation wird zur Liste der Simulationen hinzugefügt.

   :::image type="content" source="media/how-to-generate-reports/new-simulation.png" alt-text="Hinzufügen einer neuen Simulation":::

4. Wählen Sie :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: aus, wenn Sie die Simulation bearbeiten möchten.

   Wählen Sie :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: aus, wenn Sie die Simulation löschen möchten.

   Wählen Sie :::image type="icon" source="media/how-to-generate-reports/make-a-favorite-icon.png" border="false"::: aus, wenn Sie die Simulation als Favorit markieren möchten.

5. Eine Liste der Angriffsvektoren wird zusammen mit einer Vektorbewertung (bezogen auf 100) und dem Quellgerät sowie dem Zielgerät des Angriffs angezeigt. Wählen Sie einen bestimmten Angriff aus, um eine grafische Darstellung der Angriffsvektoren anzuzeigen.

   :::image type="content" source="media/how-to-generate-reports/sample-attack-vectors.png" alt-text="Angriffsvektoren.":::

## <a name="sensor-data-mining-queries"></a>Data Mining-Abfragen für Sensoren

Mithilfe von Data Mining-Tools können Sie umfassende und detaillierte Informationen über Ihre Netzwerkgeräte in verschiedenen Schichten erstellen. Beispielsweise können Sie eine Abfrage auf folgenden Grundlagen erstellen:

- Zeiträume

- Internetverbindungen

- Ports

- Protokolle

- Firmwareversionen

- Programmierbefehle

- Inaktivität des Geräts

Sie können den Bericht mithilfe von Filtern optimieren. Beispielsweise können Sie ein bestimmtes Subnetz abfragen, in dem Firmware aktualisiert wurde.

:::image type="content" source="media/how-to-generate-reports/active-device-list-v2.png" alt-text="Liste der aktiven Geräte.":::

Zum Verwalten von Abfragen stehen verschiedene Tools zur Verfügung. Beispielsweise können Sie exportieren und speichern.

> [!NOTE]
> Administratoren und Sicherheitsanalysten haben Zugriff auf Data Mining-Optionen.

### <a name="dynamic-updates"></a>Dynamische Updates

Die von Ihnen erstellten Data Mining-Abfragen werden bei jedem Öffnen dynamisch aktualisiert. Beispiel:

- Wenn Sie einen Bericht für Firmwareversionen auf Geräten am 1. Juni erstellen und den Bericht am 10. Juni wieder öffnen, wird der Bericht mit den am 10. Juni gültigen Informationen aktualisiert.

- Wenn Sie am 1. Juni einen Bericht erstellen, um neue Geräte zu erkennen, die im Lauf der letzten 30 Tage ermittelt wurden, und den Bericht am 30. Juni öffnen, werden die Ergebnisse für die letzten 30 Tage angezeigt.

### <a name="data-mining-use-cases"></a>Data Mining-Anwendungsfälle

Mithilfe von Abfragen können Sie einen großen Bereich an Sicherheitsanforderungen für verschiedene Sicherheitsteams abdecken:

- **SOC-Reaktion auf Vorfälle**: Generieren Sie einen Bericht in Echtzeit, um die sofortige Reaktion auf Vorfälle zu unterstützen. Beispielsweise können Sie einen Bericht für eine Liste von Geräten generieren, für die möglicherweise Patchen erforderlich ist.

- **Forensik**: Generieren Sie einen Bericht auf der Grundlage von Verlaufsdaten für Untersuchungsberichte.

- **IT-Netzwerkintegrität**: Generieren Sie einen Bericht, der zur Verbesserung der Netzwerk-Gesamtsicherheit beiträgt. Erstellen Sie beispielsweise einen Bericht mit Geräten, die mit schwachen Anmeldeinformationen geschützt sind.

- **Transparenz**: Generieren Sie einen Bericht, der alle Abfragepunkte abdeckt, um alle Baselineparameter Ihres Netzwerks darzustellen.

### <a name="data-mining-storage"></a>Data Mining-Archivierung

Data Mining-Informationen werden fortlaufend gespeichert und archiviert, außer wenn ein Gerät gelöscht wird. Data Mining-Ergebnisse können exportiert und extern auf einem sicheren Server archiviert werden. Außerdem führt der Sensor automatisch täglich Sicherungen durch, um die Systemkontinuität und den Erhalt von Daten sicherzustellen.

### <a name="data-mining-and-reports"></a>Data Mining und Berichte 

Reguläre Berichte, auf die Sie über die Option **Reports** (Berichte) zugreifen, sind vordefinierte Data Mining-Berichte. Es handelt sich dabei nicht um dynamische Abfragen, wie sie im Data-Mining verfügbar sind, sondern um eine statische Darstellung der Data Mining-Abfrageergebnisse.

Die Ergebnisse von Data Mining-Abfragen stehen RO-Benutzern nicht zur Verfügung. Administratoren und Sicherheitsanalysten, die auf Zugänglichkeit der von den Data Mining-Abfragen generierten Informationen für RO-Benutzer Wert legen, sollten die Informationen als Bericht speichern.

### <a name="predefined-queries"></a>Vordefinierte Abfragen

Die folgenden vordefinierten Abfragen sind verfügbar. Diese Abfragen werden in Echtzeit generiert.

- **CVEs**: Eine Liste der Geräte, für die in den letzten 24 Stunden bekannte Sicherheitsrisiken erkannt wurden.

- **Ausgeschlossene CVEs**: Eine Liste aller CVEs, die manuell ausgeschlossen wurden. Um genauere Ergebnisse in VA-Berichten und Angriffsvektoren zu erzielen, können Sie die CVE-Liste manuell anpassen, indem Sie CVEs ein- und ausschließen.

- **Internetaktivität**: Geräte, die mit dem Internet verbunden sind.

- **Nicht aktive Geräte**: Geräte, die in den letzten sieben Tagen nicht kommuniziert haben.

- **Aktive Geräte**: Aktive Netzwerkgeräte innerhalb der letzten 24 Stunden.

- **Remotezugriff**: Geräte, die über Remotesitzungsprotokolle kommunizieren.

- **Programmierbefehle**: Geräte, die die Industrieprogrammierung senden.

Auf diese Berichte kann automatisch über den Bildschirm **Reports** (Berichte) zugegriffen werden, auf dem sie von RO-Benutzern und anderen Benutzern angezeigt werden können. RO-Benutzer können nicht auf Data Mining-Berichte zugreifen.

:::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Der Data Mining-Bildschirm":::

### <a name="create-a-data-mining-report"></a>Erstellen eines Data Mining-Berichts

So erstellen Sie einen Data Mining-Bericht:

1. Wählen Sie im seitlichen Menü **Data Mining** aus. Vorschläge für vordefinierte Berichte werden automatisch angezeigt.

 :::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Auswählen von Data Mining in der Seitenleiste.":::

2. Wählen Sie :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: aus.

3. Wählen Sie **New Report** (Neuer Bericht) aus, und definieren Sie den Bericht.

   :::image type="content" source="media/how-to-generate-reports/create-new-report-screen.png" alt-text="Erstellen Sie einen neuen Bericht, indem Sie diesen Bildschirm ausfüllen.":::

   Die folgenden Parameter sind verfügbar:

   - Geben Sie einen Namen und eine Beschreibung für den Bericht ein.

   - Wählen Sie für die Kategorien eine der folgenden Optionen aus:

      - **Categories (All)** (Kategorien (Alle)): Zeigt alle Berichtsergebnisse zu allen Geräten in Ihrem Netzwerk an.

      - **Generic** (Allgemein): zur Auswahl von Standardkategorien.

   - Wählen Sie die spezifischen Berichtsparameter aus, die Sie interessieren.

   - Wählen Sie eine Sortierreihenfolge aus (**Order by** (Sortieren nach)). Sortieren Sie die Ergebnisse auf der Grundlage von Aktivität oder Kategorie.

   - Wählen Sie **Save to Report Pages** (Auf Berichtsseiten speichern) aus, um die Berichtsergebnisse als Bericht zu speichern, auf den von der Seite **Report** (Bericht) aus zugegriffen werden kann. Dies ermöglicht RO-Benutzern das Ausführen des von Ihnen erstellten Berichts.

   - Wählen Sie **Filters (Add)** (Filter (Hinzufügen)) aus, um weitere Filter hinzuzufügen. Anforderungen mit Platzhalterzeichen werden unterstützt.

   - Geben Sie eine Gerätegruppe an (die in der Gerätekarte definiert ist).

   - Geben Sie eine IP-Adresse an.

   - Geben Sie einen Port an.

   - Geben Sie eine MAC-Adresse an.

4. Wählen Sie **Speichern** aus. Die Berichtergebnisse werden auf der Seite **Data Mining** geöffnet.

:::image type="content" source="media/how-to-generate-reports/data-mining-page.png" alt-text="Berichtergebnisse in der Darstellung auf der Seite „Data Mining“.":::

### <a name="manage-data-mining-reports"></a>Verwalten von Data Mining-Berichten

In der folgenden Tabelle werden die Verwaltungsoptionen für Data Mining beschrieben:

| Symbolbild | BESCHREIBUNG |
|--|--|
| :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: | Bearbeiten der Berichtsparameter. |
| :::image type="icon" source="media/how-to-generate-reports/export-as-pdf-icon.png" border="false"::: | Als PDF exportieren. |
| :::image type="icon" source="media/how-to-generate-reports/csv-export-icon.png" border="false"::: |Als CSV exportieren. |
| :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: | Anzeigen zusätzlicher Informationen, z. B. des Datums der letzten Änderung. Verwenden Sie dieses Feature, um eine Momentaufnahme eines Abfrageergebnisses zu erstellen. Dies kann beispielsweise für die weitere Untersuchung mit Teamleitern oder SOC-Analysten erforderlich sein. |
| :::image type="icon" source="media/how-to-generate-reports/pin-icon.png" border="false"::: | Auf der Seite **Reports** (Berichte) einblenden oder auf der Seite **Reports** ausblenden. :::image type="content" source="media/how-to-generate-reports/hide-reports-page.png" alt-text="Verbergen oder Anzeigen Ihrer Berichte."::: |
| :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: | Löschen Sie den Bericht. |

#### <a name="create-customized-directories"></a>Erstellen von angepassten Verzeichnissen 

Sie können die umfangreichen Informationen für Data Mining-Abfragen ordnen, indem Sie Verzeichnisse für Kategorien erstellen. Beispielsweise können Sie Verzeichnisse für Protokolle oder Speicherorte erstellen.

So erstellen Sie ein neues Verzeichnis:

1. Wählen Sie :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: aus, um ein neues Verzeichnis hinzuzufügen.

2. Wählen Sie **New Directory** (Neues Verzeichnis) aus, um das Formular für neue Verzeichnisse anzuzeigen.

3. Benennen Sie das neue Verzeichnis.

4. Ziehen Sie die erforderlichen Berichte in das neue Verzeichnis. Sie können den Bericht jederzeit wieder zurück in die Hauptansicht ziehen.

5. Klicken Sie mit der rechten Maustaste auf das neue Verzeichnis, um es zu öffnen, zu bearbeiten oder zu löschen.

#### <a name="create-snapshots-of-report-results"></a>Erstellen von Momentaufnahmen von Berichtsergebnissen

Möglicherweise müssen Sie bestimme Abfrageergebnisse für die spätere Untersuchung speichern. Beispielsweise kann es erforderlich sein, die Ergebnisse mit verschiedenen Sicherheitsteams zu teilen.

Momentaufnahmen werden innerhalb der Berichtsergebnisse gespeichert und generieren keine dynamischen Abfragen.

:::image type="content" source="media/how-to-generate-reports/report-results-report.png" alt-text="Momentaufnahmen":::

So erstellen Sie eine Momentaufnahme:

1. Öffnen Sie den erforderlichen Bericht.

2. Wählen Sie das Informationssymbol :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: aus.

3. Wählen Sie **Take New** (Neu erstellen) aus.

4. Geben Sie einen Namen für die Momentaufnahme ein, und wählen Sie **Save** (Speichern) aus.

:::image type="content" source="media/how-to-generate-reports/take-a-snapshot.png" alt-text="Erstellen einer Momentaufnahme":::

#### <a name="customize-the-cve-list"></a>Anpassen der CVE-Liste

Sie können die CVE-Liste manuell wie folgt anpassen:

  - Einschließen/Ausschließen von CVEs

  - Ändern der CVE-Bewertung

So nehmen Sie manuelle Änderungen am CVE-Bericht vor:

1.  Wählen Sie im seitlichen Menü **Data Mining** aus.

2. Wählen Sie in der oberen rechten Ecke des **Data Mining**-Fensters :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: aus. Wählen Sie dann **New Report** (Neuer Bericht) aus.

   :::image type="content" source="media/how-to-generate-reports/create-a-new-report-screen.png" alt-text="Erstellen eines neuen Berichts.":::

3. Wählen Sie im linken Bereich eine der folgenden Optionen aus:

   - **Bekannte Sicherheitsrisiken**: Aktiviert beide Optionen und stellt die Ergebnisse in den beiden Tabellen des Berichts dar, eine mit CVEs und die andere mit ausgeschlossenen CVEs.

   - **CVEs**: Wählen Sie diese Option aus, um eine Liste aller CVEs anzuzeigen.

   - **Ausgeschlossene CVEs**: Wählen Sie diese Option aus, um eine Liste aller ausgeschlossenen CVEs anzuzeigen.

4. Setzen Sie die Informationen für **Name** und **Description** (Beschreibung) ein, und wählen Sie **Save** (Speichern) aus. Der neue Bericht wird im Fenster **Data Mining** angezeigt.

5. Zum Ausschließen von CVEs öffnen Sie den Data Mining-Bericht für CVEs. Die Liste aller CVEs wird angezeigt.

   :::image type="content" source="media/how-to-generate-reports/cves.png" alt-text="CVE-Bericht.":::

6. Wählen Sie :::image type="icon" source="media/how-to-generate-reports/enable-selecting-icon.png" border="false"::: aus, um das Auswählen von Listenelementen zu aktivieren, und wählen Sie die CVEs aus, die Sie anpassen möchten. Die Leiste **Operations** (Vorgänge) wird unten angezeigt.

   :::image type="content" source="media/how-to-generate-reports/operations-bar-appears.png" alt-text="Screenshot der Leiste „Operations“ (Vorgänge) in Data Mining.":::

7. Wählen Sie die CVEs aus, die Sie ausschließen möchten, und wählen Sie dann **Delete Records** (Datensätze löschen) aus. Die ausgewählten CVEs erscheinen nicht mehr in der Liste der CVEs und werden in der Liste der ausgeschlossenen CVEs aufgeführt, wenn Sie eine erstellen.

8. Zum Einschließen der ausgeschlossenen CVEs in der Liste der CVEs generieren Sie den Bericht für ausgeschlossene CVEs, und löschen Sie in dieser Liste die Elemente, die Sie wieder in die Liste der CVEs aufnehmen möchten.

9. Wählen Sie die CVEs aus, deren Bewertung Sie ändern möchten, und wählen Sie dann **Update CVE Score** (CVE-Bewertung aktualisieren) aus.

   :::image type="content" source="media/how-to-generate-reports/set-new-score-screen.png" alt-text="Aktualisieren der CVE-Bewertung":::

10. Geben Sie die neue Bewertung ein, und wählen Sie **OK** aus. Die aktualisierte Bewertung wird in den von Ihnen ausgewählten CVEs angezeigt.

### <a name="reports-based-on-data-mining"></a>Auf Data Mining basierende Berichte

Berichte geben die von Data Mining-Abfrageergebnissen generierten Informationen wieder. Dies schließt die Data Mining-Standardberichte ein, die in der Ansicht „Berichte“ verfügbar sind. Administratoren und Sicherheitsanalysten können außerdem benutzerdefinierte Data Mining-Abfragen generieren und als Berichte speichern. Diese Berichte sind auch für RO-Benutzer verfügbar.

So erstellen Sie einen Bericht

1. Wählen Sie im seitlichen Menü **Reports** (Berichte) aus.

2. Wählen Sie den anzuzeigenden Bericht aus. Sie können unter **Custom** (Benutzerdefinierten) oder **Auto-Generated** (automatisch generierten) Berichten wählen, z. B. **Programming Commands** (Programmierbefehle) und **Remote Access** (Remotezugriff).

3. Sie können den Bericht exportieren, indem Sie eins der Symbole oben rechts auf dem Bildschirm auswählen:

   :::image type="icon" source="media/how-to-generate-reports/export-to-pdf-icon.png" border="false"::: Exportieren in eine PDF-Datei.

   :::image type="icon" source="media/how-to-generate-reports/export-to-csv-icon.png" border="false"::: Exportieren in eine CSV-Datei.

> [!NOTE] 
> RO-Benutzer können lediglich die für sie erstellten Berichte sehen.

:::image type="content" source="media/how-to-generate-reports/select-a-report-screen.png" alt-text="Auswählen des zu generierenden Berichts.":::

:::image type="content" source="media/how-to-generate-reports/remote-access-report.png" alt-text="Generierter Remotezugriffsbericht.":::

## <a name="reports-for-sensor-trends-and-statistics"></a>Berichte zu Sensortrends und Statistiken

Sie können Widgetdiagramme und Kreisdiagramme erstellen, um Erkenntnisse über Netzwerktrends und Statistiken zu gewinnen. Widgets können unter benutzerdefinierten Dashboards gruppiert werden.

> [!NOTE]
> Nur Administratoren und Sicherheitsanalysten können die in diesem Abschnitt beschriebenen Verfahren ausführen.

Zum Anzeigen von Dashboards und Widgets wählen Sie **Trends & Statistics** (Trends und Statistiken) im Menü auf der Seite aus.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Screenshot einer Untersuchung.":::

Das Dashboard besteht aus Widgets, die die folgenden Informationstypen grafisch beschreiben:

- Datenverkehr nach Port
- Bandbreite des Kanals
- Bandbreite gesamt
- Aktive TCP-Verbindung
- Geräte:
  - Neue Geräte
  - Ausgelastete Geräte
  - Geräte nach Hersteller
  - Geräte nach Betriebssystem
  - Getrennte Geräte
- Verbindungsabbruch nach Stunden
- Warnungen zu Vorfällen nach Typ
- Zugriff auf Datenbanktabellen
- Widgets zur Protokollanalyse
- Ethernet- und IP-Adresse:
  - Ethernet- und IP-Adressdatenverkehr nach CIP-Dienst
  - Ethernet- und IP-Adressdatenverkehr nach CIP-Klasse
  - Ethernet- und IP-Adressdatenverkehr nach Befehl
- OPC:
  - Größte OPC-Verwaltungsvorgänge
  - Größte OPC-E/A-Vorgänge
- Siemens S7:
  - S7-Datenverkehr nach Steuerungsfunktion
  - S7-Datenverkehr nach Unterfunktion
- SRTP:
  - SRTP-Datenverkehr nach Dienstcode
  - SRTP-Fehler nach Tag
- SuiteLink:
  - Am meisten abgefragte SuiteLink-Tags
  - Verhalten von numerischen SuiteLink-Tags
- IEC-60870-Datenverkehr nach ASDU
- DNP3-Datenverkehr nach Funktion
- MMS-Datenverkehr nach Dienst
- Modbus-Datenverkehr nach Funktion
- OPC-UA-Datenverkehr nach Dienst

> [!NOTE]
>  Die Zeit in den Widgets wird entsprechend der Sensorzeit festgelegt.

## <a name="reports-for-the-on-premises-management-console"></a>Berichte für die lokale Verwaltungskonsole

Mithilfe der lokalen Verwaltungskonsole können Sie Berichte für jeden Sensor generieren, der mit ihr verbunden ist. Berichte basieren auf ausgeführten Data Mining-Abfragen für einen Sensor.

Die folgenden Berichte können generiert werden:

- **Aktive Geräte (letzte 24 Stunden)** : Zeigt eine Liste der Geräte an, die innerhalb eines 24-Stunden-Zeitraums Netzwerkaktivität aufwiesen.

- **Nicht aktive Geräte (Letzte 7 Tage)** : Zeigt eine Liste der Geräte an, die innerhalb der letzten 7 Tage keine Netzwerkaktivität aufwiesen.

- **Programmierbefehle**: Zeigt eine Liste von Geräten an, die innerhalb der letzten 24 Stunden Programmierbefehle gesendet haben.

- **Remotezugriff**: Zeigt eine Liste von Geräten an, auf die innerhalb der letzten 24 Stunden durch Remotequellen zugegriffen wurde.

:::image type="content" source="media/how-to-generate-reports/reports-view.png" alt-text="Screenshot der Berichtsansicht.":::

Wenn Sie den Sensor in der lokalen Verwaltungskonsole auswählen, werden alle für diesen Sensor konfigurierten benutzerdefinierten Berichte in der Liste der Berichte angezeigt. Für jeden Sensor können Sie einen für den Sensor konfigurierten Standardbericht oder einen benutzerdefinierten Bericht generieren.

So erstellen Sie einen Bericht

1. Wählen Sie im linken Bereich **Reports** (Berichte) aus. Das Fenster **Reports** (Berichte) wird angezeigt.

2. Wählen Sie in der Dropdownliste **Sensors** (Sensoren) den Sensor aus, für den Sie einen Bericht generieren möchten.

   :::image type="content" source="media/how-to-generate-reports/sensor-drop-down-list.png" alt-text="Screenshot der Sensoransicht.":::

3. Wählen Sie in der rechten Dropdownliste den Bericht aus, den Sie generieren möchten.

4. Zum Erstellen einer PDF-Datei mit den Berichtsergebnissen wählen Sie :::image type="icon" source="media/how-to-generate-reports/pdf-report-icon.png" border="false"::: aus.

## <a name="risk-assessment-reports-for-the-on-premises-management-console"></a>Berichte zur Risikobewertung für die lokale Verwaltungskonsole

Generieren Sie einen Bericht zur Risikobewertung für jeden Sensor, der mit Ihrer lokalen Verwaltungskonsole verbunden ist. Dieser Bericht gibt Einblick in jedes der Netzwerke, die von Ihren Sensoren überwacht werden.

Mithilfe von Berichten zur Risikobewertung können Sie eine Sicherheitsbewertung für jedes Netzwerkgerät sowie eine Sicherheitsbewertung für das gesamte Netzwerk erstellen. Die Gesamtbewertung basiert auf der tiefgreifenden Paketüberprüfung, Engines zur Verhaltensmodellierung und einem SCADA-spezifischen Entwurf von Zustandsautomaten. Weitere Informationen stehen in großem Umfang zur Verfügung. Beispiel:

- Konfigurationsprobleme

- Sicherheitsrisiko von Geräten, nach Sicherheitsstufe priorisiert

- Netzwerksicherheitsprobleme

- Netzwerkbetriebsprobleme

- Angriffsvektoren

- Verbindungen mit ICS-Netzwerken

- Internetverbindungen

- Indikatoren für industrielle Schadsoftware

- Protokollprobleme

Der Bericht enthält Empfehlungen zur Entschärfung, mit deren Hilfe Sie Ihre Sicherheitsbewertung verbessern können.

- Sichere Geräte: Geräte mit einer Sicherheitsbewertung oberhalb von 90 %.

- **Sicherheitsanfällige Geräte**: Geräte mit einer Sicherheitsbewertung unterhalb von 70 Prozent.

- **Geräte, die der Verbesserung bedürfen**: Geräte mit einer Sicherheitsbewertung zwischen 70 Prozent und 89 Prozent.

So erstellen Sie einen Bericht

1. Wählen Sie im seitlichen Menü **Risk Assessment** (Risikobewertung) aus.

2. Wählen Sie in der Dropdownliste **Select Sensor** (Sensor auswählen) einen Sensor aus.

3. Wählen Sie **Generate Report** (Bericht generieren) aus.

4. Wählen Sie im Abschnitt **Archived Reports** (Archivierte Berichte) **Download** (Herunterladen) aus.

So importieren Sie ein Firmenlogo:

- Wählen Sie **Logo importieren** aus.

:::image type="content" source="media/how-to-generate-reports/import-logo-screenshot.png" alt-text="Importieren Sie Ihr Logo über die Ansicht „Risikobewertung“.":::

## <a name="see-also"></a>Weitere Informationen
[Arbeiten mit Standortübersichtansichten](how-to-gain-insight-into-global-regional-and-local-threats.md#work-with-site-map-views)
