---
title: Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Im Rahmen von Azure IoT Central-Aufträgen können Funktionen zur Verwaltung von mehreren Geräten gleichzeitig ausgeführt werden, z. B. das Aktualisieren von Eigenschaften oder das Ausführen eines Befehls.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 06/08/2020
ms.topic: how-to
ms.openlocfilehash: 406881a9131aae35b91dcab248745bb426cecf0e
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88797835"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung

Sie können Microsoft Azure IoT Central verwenden, um Ihre angeschlossenen Geräte bedarfsgerecht mithilfe von Aufträgen zu überwachen. Mit Aufträgen können Sie Massenaktualisierungen an Geräteeigenschaften vornehmen und Befehle ausführen. In diesem Artikel wird Ihnen der Einstieg in die Verwendung von Aufträgen in Ihrer eigenen Anwendung gezeigt.

## <a name="create-and-run-a-job"></a>Erstellen und Ausführen eines Auftrags

In diesem Abschnitt wird gezeigt, wie Sie einen Auftrag erstellen und ausführen, indem Sie den leichten Schwellenwert für eine Gruppe von logistischen Gatewaygeräten festlegen.

1. Wählen Sie im linken Bereich **Aufträge** aus.

2. Wählen Sie **+ Neu** aus.

   ![Screenshot: Auswahl für die Erstellung eines Auftrags](./media/howto-run-a-job/create-new-job.png)

3. Geben Sie einen Namen und eine Beschreibung ein, mit denen Sie Ihren zu erstellenden Auftrag wiedererkennen können.

4. Wählen Sie die Zielgerätegruppe aus, auf die der Auftrag angewandt werden soll. Im Abschnitt **Zusammenfassung** können Sie sehen, auf wie viele Geräte Ihre Auftragskonfiguration angewendet wird.

5. Wählen Sie **Cloudeigenschaft**, **Eigenschaft** oder **Befehl** als Typ für den zu konfigurierenden Auftrag aus. 

   Zum Einrichten einer Auftragskonfiguration des Typs **Eigenschaft** wählen Sie die gewünschte Eigenschaft aus, und legen Sie deren neuen Wert fest. Wählen Sie den auszuführenden Befehl aus, um eine **Befehl**-Auftragskonfiguration einzurichten. Mit einem Eigenschaftenauftrag können mehrere Eigenschaften festgelegt werden.

   ![Screenshot: Auswahl für das Erstellen eines Eigenschaftsauftrags zum Festlegen des leichten Schwellenwerts](./media/howto-run-a-job/configure-job.png)

6. Wählen Sie **Ausführen** oder **Speichern** aus. Der Auftrag wird nun auf der Hauptseite **Aufträge** angezeigt. Auf dieser Seite können Sie Ihren derzeit ausgeführten Auftrag sowie den Verlauf aller zuvor ausgeführten oder gespeicherten Aufträge sehen. Sie können Ihren gespeicherten Auftrag jederzeit wieder öffnen, um ihn weiter zu bearbeiten oder auszuführen.

   ![Screenshot, der den Namen, den Status und die Beschreibung eines erstellten Auftrags anzeigt.](./media/howto-run-a-job/view-job.png)

   > [!NOTE]
   > Sie können den Verlauf von bis zu 30 Tagen für Ihre zuvor ausgeführten Aufträge anzeigen.

7. Wählen Sie den gespeicherten Auftrag aus, und führen Sie ihn dann über die Schaltfläche **Ausführen** aus. 

   Das Dialogfeld **Auftrag ausführen?** wird angezeigt. Bestätigen Sie den Vorgang durch Auswählen der Schaltfläche **Auftrag ausführen**. 

   ![Screenshot des Dialogfelds, das bestätigt, dass Sie einen Auftrag ausführen möchten.](./media/howto-run-a-job/run-job.png)

8. Ein Auftrag durchläuft die Phasen „Ausstehend“, „Wird ausgeführt“ und „Abgeschlossen“. Die Auftragsausführungsdetails enthalten Metriken des Typs „Ergebnis“, Details zu „Dauer“ und das Raster der Geräteliste. 

   In dieser Übersicht können Sie auch **Ergebnisprotokoll** auswählen, um eine CSV-Datei mit Ihren Auftragsdetails, einschließlich der Geräte und ihrer Statuswerte, herunterzuladen. Diese Informationen können bei der Problembehandlung hilfreich sein.

   ![Screenshot zum Gerätestatus](./media/howto-run-a-job/download-details.png)

## <a name="manage-jobs"></a>Aufträge verwalten

Um einen aktiven Auftrag zu beenden, öffnen Sie ihn und wählen Sie **Anhalten** aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag angehalten wurde. Im Abschnitt **Zusammenfassung** wird angezeigt, welche Geräte abgeschlossen wurden, fehlgeschlagen sind oder noch ausstehen.

![Screenshot, der einen aktiven Auftrag und die Schaltfläche zum Anhalten eines Auftrags zeigt](./media/howto-run-a-job/manage-job.png)

Nachdem sich ein Auftrag in einem angehaltenen Zustand befindet, können Sie **Fortsetzen** auswählen, um die Ausführung des Auftrags fortzusetzen. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag wieder ausgeführt wird. Der Abschnitt **Zusammenfassung** wird weiterhin mit dem neuesten Status aktualisiert.

![Screenshot, der einen angehaltenen Auftrag und die Schaltfläche zum Fortsetzen eines Auftrags anzeigt](./media/howto-run-a-job/stopped-job.png)

## <a name="copy-a-job"></a>Kopieren eines Auftrags

Wenn Sie einen Ihrer vorhandenen Aufträge kopieren möchten, wählen Sie ihn auf der Seite **Aufträge** und dann **Auftragsdetails** aus. Die Detailseite für den **Auftrag** wird angezeigt. 

![Screenshot, der die Seite für Auftragsdetails anzeigt](./media/howto-run-a-job/job-details.png)

Wählen Sie **Kopieren** aus.

![Screenshot, der die Schaltfläche zum Kopieren anzeigt](./media/howto-run-a-job/job-details-copy.png)

Daraufhin wird eine Kopie der Auftragskonfiguration geöffnet, die Sie bearbeiten können; an den Auftragsnamen ist **Kopie** angefügt. Sie können den neuen Auftrag speichern oder ausführen.

![Screenshot, der eine Kopie der Auftragskonfiguration anzeigt](./media/howto-run-a-job/copy-job.png)

## <a name="view-job-status"></a>Anzeigen des Auftragsstatus

Nachdem ein Auftrag erstellt wurde, wird die Spalte **Status** mit der neuesten Statusmeldung des Auftrags aktualisiert. In der folgenden Tabelle werden die möglichen Auftragsstatuswerte aufgeführt:

| Statusmeldung       | Bedeutung der Statusmeldung                                          |
| -------------------- | ------------------------------------------------------- |
| Abgeschlossen            | Dieser Auftrag wurde auf allen Geräten ausgeführt.              |
| Fehler               | Dieser Auftrag war fehlerhaft und konnte nicht vollständig auf Geräten ausgeführt werden.  |
| Ausstehend              | Mit der Ausführung dieses Auftrags wurde auf den Geräten noch nicht begonnen.         |
| Wird ausgeführt              | Dieser Auftrag wird derzeit auf Geräten ausgeführt.             |
| Beendet              | Ein Benutzer hat diesen Auftrag manuell angehalten.           |

Der Statusmeldung folgt eine Übersicht über die Geräte im Auftrag. In der folgenden Tabelle werden die möglichen Gerätestatuswerte aufgeführt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Erfolgreich            | Die Anzahl der Geräte, auf denen der Auftrag erfolgreich ausgeführt wurde.       |
| Fehler               | Die Anzahl der Geräte, auf denen der Auftrag nicht erfolgreich ausgeführt wurde.       |

Zum Anzeigen des Status eines Auftrags und aller betroffenen Geräte öffnen Sie den Auftrag. Neben den einzelnen Gerätenamen werden folgende Statusmeldungen angezeigt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Abgeschlossen            | Der Auftrag wurde auf diesem Gerät ausgeführt.                                     |
| Fehler               | Beim Ausführen des Auftrags auf diesem Gerät ist ein Fehler aufgetreten. In der Fehlermeldung sind weitere Informationen enthalten.  |
| Ausstehend              | Der Auftrag wurde auf diesem Gerät noch nicht ausgeführt.                                   |

Klicken Sie auf **Herunterladen**, um eine CSV-Datei herunterzuladen, die die Auftragsdetails sowie eine Liste der Geräte mit den entsprechenden Statuswerten enthält.

## <a name="filter-the-device-list"></a>Filtern der Geräteliste

Sie können die Geräteliste auf der Seite mit den **Auftragsdetails** filtern, indem Sie auf das Filtersymbol klicken. Außerdem können Sie die Felder **Geräte-ID** und **Status** zum Filtern verwenden.

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Screenshot, der die Auswahl für das Filtern einer Geräteliste anzeigt":::

## <a name="customize-columns-in-the-device-list"></a>Anpassen der Spalten der Geräteliste

Sie können zusätzliche Spalten auswählen, die in der Geräteliste angezeigt werden sollen, indem Sie auf das Symbol für die Spaltenoptionen klicken.

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Screenshot, der das Symbol für Spaltenoptionen anzeigt":::

In einem Dialogfeld können Sie die Spalten auswählen, die in der Geräteliste angezeigt werden sollen. Wählen Sie die Spalten aus, die Sie anzeigen möchten. Wählen Sie den Pfeil nach rechts und anschließend **OK** aus. Aktivieren Sie die Option **Alle auswählen**, um alle verfügbaren Spalten auszuwählen.

:::image type="content" source="media/howto-run-a-job/column-picker-popup.png" alt-text="Screenshot, der das Dialogfeld zum Auswählen der anzuzeigenden Spalten anzeigt":::

Die ausgewählten Spalten werden in der Geräteliste angezeigt.

:::image type="content" source="media/howto-run-a-job/column-picker-column-selected.png" alt-text="Screenshot, in dem ausgewählte Spalten in der Geräteliste angezeigt werden":::

Die ausgewählten Spalten werden in einer oder mehreren Benutzersitzungen beibehalten, die Zugriff auf die Anwendung haben.

## <a name="rerun-jobs"></a>Erneutes Ausführen von Aufträgen

Sie können einen Auftrag, bei dem bei Geräten ein Fehler aufgetreten ist, erneut ausführen. Wählen Sie **Rerun on failed** (Bei Fehler erneut ausführen) aus.

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Screenshot, der die Schaltfläche zum erneuten Ausführen eines Auftrags auf Geräten mit Fehlern anzeigt":::

Geben Sie einen Auftragsnamen und eine Beschreibung an, und wählen Sie dann **Rerun job** (Auftrag erneut ausführen) aus. Ein neuer Auftrag wird übermittelt, um die Aktion auf Geräten, bei denen ein Fehler aufgetreten ist, erneut auszuführen.

:::image type="content" source="media/howto-run-a-job/rerun-failed.png" alt-text="Screenshot, der das Dialogfeld zum erneuten Ausführen von Geräten anzeigt, bei denen ein Fehler aufgetreten ist":::

> [!NOTE]
> Sie können nicht mehr als fünf Aufträge gleichzeitig aus einer Azure IoT Central-Anwendung ausführen.
>
> Wenn ein Auftrag abgeschlossen ist und Sie ein Gerät löschen, das sich in der Geräteliste des Auftrags befindet, wird der Geräteeintrag im Gerätenamen als gelöscht angezeigt. Der Link „Details“ ist für das gelöschte Gerät nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Aufträge in Ihrer Azure IoT Central-Anwendung erstellt werden, können Sie die nächsten Schritte ausführen:

- [Verwalten von Geräten](howto-manage-devices.md)
- [Verwalten der Versionen Ihrer Gerätevorlage](howto-version-device-template.md)
