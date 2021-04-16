---
title: Arbeiten mit Warnungen auf der lokalen Verwaltungskonsole
description: Mithilfe der lokalen Verwaltungskonsole können Sie eine Unternehmensübersicht über die aktuellen Bedrohungen in Ihrem Netzwerk erhalten und besser verstehen, wie diese von den Sensorbenutzern behandelt werden.
ms.date: 12/06/2020
ms.topic: how-to
ms.openlocfilehash: 604650f0cb08eac4a3ab1cfd3fdcbf2e7ff0d19e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105032139"
---
# <a name="work-with-alerts-on-the-on-premises-management-console"></a>Arbeiten mit Warnungen auf der lokalen Verwaltungskonsole 

Folgende Aktionen können Sie über das Fenster **Warnungen** in der Verwaltungskonsole ausführen:

- Arbeiten mit Warnungsfiltern

- Arbeiten mit Warnungszählern

- Anzeigen von Warnungsinformationen

- Verwalten von Warnungsereignissen

- Erstellen von Warnungsausschlussregeln

- Auslösen von Warnungsausschlussregeln von externen Systemen

- Beschleunigen des Workflows bei Vorfällen mit Warnungsgruppen

## <a name="view-alerts-in-the-on-premises-management-console"></a>Anzeigen von Warnungen an der lokalen Verwaltungskonsole

An der lokalen Verwaltungskonsole werden Warnungen von allen verbundenen Sensoren aggregiert. Dadurch erhalten Sie eine Unternehmensübersicht über die aktuellen Bedrohungen in Ihrem Netzwerk und können besser verstehen, wie diese von den Sensorbenutzern verwendet werden.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/alerts-with-samples.png" alt-text="Screenshot des Fensters „Warnungen“":::

### <a name="work-with-alert-filters"></a>Arbeiten mit Warnungsfiltern

Im Fenster **Warnungen** werden die Warnungen angezeigt, die von Sensoren generiert wurden, die mit Ihrer lokalen Verwaltungskonsole verbunden sind. Sie können alle Warnungen der verbundenen Sensoren oder die von einer bestimmten Quelle gesendeten Warnungen anzeigen:

- Website

- Zone

- Sicherungsmedium

- Sensor

Wählen Sie **Filter löschen** aus, um alle Warnungen anzuzeigen.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/clear-filters.png" alt-text="Löschen der Filter durch Auswählen der Schaltfläche „Filter löschen“":::

### <a name="work-with-alert-counters"></a>Arbeiten mit Warnungszählern

Warnungszähler bieten eine Aufschlüsselung der Warnungen nach dem Schweregrad und dem Bestätigungsstatus.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/number-of-alerts.png" alt-text="Warnungszähler mit der Anzahl Ihrer Warnungen":::

Die folgenden Schweregrade werden im Warnungszähler angezeigt:

- **Kritisch**: Gibt einen böswilligen Angriff an, der sofort behandelt werden muss.

- **Schwer:** Gibt eine Sicherheitsbedrohung an, deren Behandlung wichtig ist.

- **Minderschwer:** Gibt eine Abweichung vom normalen Verhalten an, die auf eine Sicherheitsbedrohung hinweisen könnte.

- **Warnung:** Gibt eine Abweichung vom normalen Verhalten ohne Sicherheitsbedrohung an.

Die Schweregrade sind vordefiniert.

Sie können den Zähler anpassen, um Zahlen basierend auf bestätigten und nicht bestätigten Warnungen bereitzustellen. Nicht bestätigte Warnungen wurden auf Defender für IoT-Sensoren ausgelöst, aber noch nicht von den Verantwortlichen für den Sensor überprüft.

Wenn die Option **Show Acknowledged Alerts** (Bestätigte Warnungen anzeigen) aktiviert ist, werden alle bestätigten Warnungen im Fenster **Warnungen** angezeigt.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/show-acknowledged-alerts.png" alt-text="Anzeigen der bestätigten Warnungen":::

### <a name="view-alert-information"></a>Anzeigen von Warnungsinformationen

Die Warnung enthält die folgenden Informationen:

- Eine Zusammenfassung des Warnungsereignisses

- Schweregrad der Warnung.

- Einen Link zur Warnung auf dem Sensor, von dem das Ereignis erkannt wurde

- Eine Warnungs-UUID. Die UUID besteht aus der Warnungs-ID, die dem auf dem Sensor erkannten Warnungsereignis zugeordnet ist, gefolgt von einem Bindestrich und dann einer eindeutigen System-ID.

**Warnungs-UUID an der lokalen Verwaltungskonsole**

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/unauthorized-internet-connectivity.png" alt-text="Ein Gerät ist verbunden, aber nicht autorisiert.":::

**Warnungs-ID des Sensors**

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/internet-connectivity-alert.png" alt-text="Warnungs-ID des Sensors":::

Durch die Verwendung von UUIDs wird sichergestellt, dass jede an der lokalen Verwaltungskonsole angezeigte Warnung durch eine eindeutige Nummer gesucht und identifiziert werden kann. Dies ist erforderlich, da Warnungen, die von unterschiedlichen Sensoren generiert werden, möglicherweise dieselbe Warnungs-ID enthalten können.

> [!NOTE]
> Standardmäßig werden UUIDs in den folgenden Partnersystemen angezeigt, wenn Weiterleitungsregeln definiert sind: ArcSight, syslog-Server, QRadar, Sentinel und NetWitness. Es ist kein Setup erforderlich.

So zeigen Sie Warnungsinformationen an

- Wählen Sie in der Warnungsliste eine Warnung aus.

  :::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/alert-information.png" alt-text="Screenshot der Warnungsinformationen":::

So zeigen Sie die Warnung auf dem Sensor an

- Wählen Sie in der Warnung **OPEN SENSOR** (Sensor öffnen) aus.

So zeigen Sie die Geräte in einer Zonenzuordnung an

- Wenn Sie in den Gerätezuordnungen nur Geräte mit Warnungen und alle damit verbundenen Geräte anzeigen möchten, wählen Sie **SHOW DEVICES** (Geräte anzeigen) aus.

## <a name="manage-alert-events"></a>Verwalten von Warnungsereignissen

Zum Verwalten von Warnungsereignissen über die lokale Verwaltungskonsole stehen mehrere Optionen zur Verfügung.

- Ermitteln oder bestätigen Sie Warnungsereignisse. Wählen Sie **LEARN & ACKNOWLEDGE ALL** (Alle ermitteln und bestätigen) aus, um alle Warnungsereignisse zu ermitteln, die autorisiert werden können, und um alle Warnungsereignisse zu bestätigen, die derzeit nicht bestätigt sind.

  :::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/learn-and-acknowledge.png" alt-text="Auswählen von „LEARN & ACKNOWLEDGE ALL“ (Alle ermitteln und bestätigen)":::

- Schalten Sie Warnungsereignisse stumm, oder heben Sie die Stummschaltung auf.

Weitere Informationen zum Erlernen, Bestätigen und Stummschalten von Warnungsereignissen finden Sie im Artikel [Verwalten von Warnungsereignissen](how-to-manage-the-alert-event.md).

## <a name="export-alert-information"></a>Exportieren von Warnungsinformationen

Exportieren Sie Warnungsinformationen in eine CSV-Datei. Sie können Informationen zu allen erkannten Warnungen oder Informationen basierend auf der gefilterten Ansicht exportieren. Die folgenden Informationen werden exportiert:

- Adresse (Quelle)
- Destination Address (Zieladresse)
- Warnungstitel
- Schweregrad der Warnung
- Warnmeldung
- Zusätzliche Informationen
- Status „Bestätigt“
- PCAP-Verfügbarkeit

So erfolgt das Exportieren

1. Wählen Sie im Seitenmenü die Option Warnungen aus.
1. Wählen Sie Exportieren aus.
1. Wählen Sie „Export Extended Alerts“ (Erweiterte Warnungen exportieren) aus, um für jede Warnung, die mehrere Geräte abdeckt, Warnungsinformationen in separaten Zeilen zu exportieren. Wenn „Export Extended Alerts“ (Erweiterte Warnungen exportieren) ausgewählt ist, wird in der CSV-Datei eine duplizierte Zeile der Warnung mit den eindeutigen Elementen in jeder Zeile erstellt. Die Verwendung dieser Option vereinfacht die Untersuchung exportierter Warnungsereignisse.  

## <a name="create-alert-exclusion-rules"></a>Erstellen von Warnungsausschlussregeln

Weisen Sie Defender für IoT an, Benachrichtigungstrigger basierend auf Folgendem zu ignorieren:

- Zeitzonen und Zeiträume

- Geräteadresse (IP-Adresse, MAC-Adresse, Subnetz)

- Warnungsnamen

- Spezifische Sensoren

Erstellen Sie Warnungsausschlussregeln, wenn Defender für IoT Aktivitäten ignorieren soll, die eine Warnung auslösen.

Wenn Sie z. B. wissen, dass alle von einem bestimmten Sensor überwachten OT-Geräte für zwei Tage gewartet werden, können Sie eine Ausschlussregel definieren, damit Defender für IoT die von diesem Sensor erkannten Warnungen während des festgelegten Zeitraums ignoriert.

### <a name="alert-exclusion-logic"></a>Logik für Warnungsausschlüsse

Die Warnungsregellogik ist `AND`-basiert. Dies bedeutet, dass nur dann eine Warnung ausgelöst wird, wenn alle Regelbedingungen erfüllt sind.

Wenn keine Regelbedingung definiert wird, schließt die Bedingung alle Optionen ein. Wenn Sie in die Regel etwa nicht den Namen eines Sensors einfügen, wird sie auf alle Sensoren angewandt.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/create-alert-exclusion-v2.png" alt-text="Screenshot der Ansicht „Ausschlussregel erstellen“":::

Regelzusammenfassungen werden im Fenster **Ausschlussregel** angezeigt.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/exclusion-summary-v2.png" alt-text="Screenshot der Zusammenfassungsansicht der Ausschlussregel":::

Außer mithilfe von Ausschlussregeln können Sie Warnungen auch unterdrücken, indem Sie sie stumm schalten.

### <a name="create-exclusion-rules"></a>Erstellen von Ausschlussregeln

So erstellen Sie Ausschlussregeln

1. Wählen Sie im linken Bereich der lokalen Verwaltungskonsole **Alert Exclusion** (Warnungsausschluss) aus. Sie definieren eine neue Ausschlussregel, indem Sie in der rechten oberen Ecke des daraufhin geöffneten Fensters das Symbol **Hinzufügen** (:::image type="icon" source="media/how-to-work-with-alerts-on-premises-management-console/add-icon.png" border="false":::) auswählen. Das Dialogfeld **Ausschlussregel erstellen** wird geöffnet.

   :::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/create-alert-exclusion-view.png" alt-text="Erstellen Sie einen Warnungsausschluss, indem Sie hier Informationen eingeben.":::

2. Geben Sie im Feld **Name** einen Namen für die Regel ein. Der Name darf keine Anführungszeichen (`"`) enthalten.

3. Geben Sie im Abschnitt **By Time Zone/Period** (Nach Zeitzone/Zeitraum) einen Zeitraum in einer bestimmten Zeitzone ein. Verwenden Sie dieses Feature, wenn für einen bestimmten Zeitraum in einer Zeitzone eine Ausschlussregel erstellt wurde, diese aber gleichzeitig in anderen Zeitzonen implementiert werden sollte. Möglicherweise müssen Sie eine Ausschlussregel zwischen 8:00 Uhr und 10:00 Uhr in drei unterschiedlichen Zeitzonen anwenden. Erstellen Sie in diesem Fall drei separate Ausschlussregeln für den gleichen Zeitraum in der entsprechenden Zeitzone.

4. Wählen Sie **ADD** (Hinzufügen) aus. Während des Ausschlusszeitraums werden auf den verbundenen Sensoren keine Warnungen erstellt.

   :::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/by-the-time-period.png" alt-text="Screenshot der Ansicht „By Time Period“ (Nach Zeitraum)":::

5. Legen Sie im Abschnitt **By Device Address** (Nach Geräteadresse) Folgendes fest:

  - Die IP-Adresse, MAC-Adresse oder Subnetzadresse des Geräts, das Sie ausschließen möchten

  - Die Richtung des Datenverkehrs für die ausgeschlossenen Geräte, die Quelle und das Ziel

6. Wählen Sie **ADD** (Hinzufügen) aus.

7. Beginnen Sie im Abschnitt **By Alert Title** (Nach Warnungstitel), den Titel der Warnung einzugeben. Wählen Sie in der Dropdownliste den oder die Titel der Warnungen aus, die ausgeschlossen werden sollen.

   :::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/alert-title.png" alt-text="Screenshot der Ansicht „By Alert Title“ (Nach Warnungstitel)":::

8. Wählen Sie **ADD** (Hinzufügen) aus.

9. Beginnen Sie im Abschnitt **By Sensor Name** (Nach Sensorname), den Namen des Sensors einzugeben. Wählen Sie in der Dropdownliste die Sensoren aus, die ausgeschlossen werden sollen.

10. Wählen Sie **ADD** (Hinzufügen) aus.

11. Wählen Sie **SAVE** (SPEICHERN) aus. Die neue Regel wird in der Liste der Regeln angezeigt.

Sie können Warnungen unterdrücken, indem Sie diese stumm schalten oder Warnungsausschlussregeln erstellen. In diesem Abschnitt werden potenzielle Anwendungsfälle für beide Features beschrieben.

- **Ausschlussregel:** Schreiben Sie eine Ausschlussregel, wenn Folgendes zutrifft:

  - Sie wissen im Voraus, dass Sie das Ereignis aus der Datenbank ausschließen möchten. Beispielsweise wissen Sie bereits, dass ein Szenario, das auf einem bestimmten Sensor erkannt wird, irrelevante Warnungen auslöst. Wenn Sie etwa Wartungsarbeiten an den SPS-Systemen der Organisation an einem bestimmten Standort ausführen, möchten Sie die SPS-Warnungen an diesem Standort unterdrücken.

  - Sie möchten, dass Defender für IoT Ereignisse für einen bestimmten Zeitraum (zur Systemwartung) ignoriert.

  - Sie möchten Ereignisse in einem bestimmten Subnetz ignorieren.

  - Sie möchten die Warnungsereignisse, die von mehreren Sensoren generiert werden, mit einer Regel steuern.

  - Sie möchten den Warnungsausschluss nicht als Ereignis im Ereignisprotokoll nachverfolgen.

- **Stumm schalten:** Schalten Sie eine Warnung stumm, wenn Folgendes zutrifft:

  - Elemente, die stumm geschaltet werden sollen, sind nicht geplant. Sie wissen nicht im Voraus, welche Ereignisse irrelevant sein werden.

  - Sie möchten die Warnung im Fenster **Warnungen** unterdrücken, aber trotzdem im Ereignisprotokoll nachverfolgen.

  - Sie möchten Ereignisse über einen bestimmten Kanal ignorieren.

### <a name="trigger-alert-exclusion-rules-from-external-systems"></a>Auslösen von Warnungsausschlussregeln von externen Systemen

Lösen Sie Warnungsausschlussregeln von externen Systemen aus. Sie können z. B. Ausschlussregeln von Ticketsystemen des Unternehmens oder von Systemen verwalten, mit denen Prozesse zur Netzwerkwartung verwaltet werden.

Definieren Sie die Sensoren, die Engines, die Startzeit und die Endzeit für die Regel. Weitere Informationen finden Sie unter [Sensor- und Verwaltungskonsolen-APIs für Defender für IoT](references-work-with-defender-for-iot-apis.md).

Regeln, die Sie mithilfe der API erstellen, sind im Fenster **Ausschlussregel** schreibgeschützt.

:::image type="content" source="media/how-to-work-with-alerts-on-premises-management-console/edit-exclusion-rule-screen.png" alt-text="Screenshot der Ansicht „Edit Exclusion Rule“ (Ausschlussregel bearbeiten)":::

## <a name="next-steps"></a>Nächste Schritte

[Arbeiten mit Warnungen auf dem Sensor](how-to-work-with-alerts-on-your-sensor.md).
Warnungen der [Defender für IoT-Engine](alert-engine-messages.md) ansehen.