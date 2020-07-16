---
title: Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Im Rahmen von Azure IoT Central-Aufträgen können Funktionen zur Verwaltung von mehreren Geräten gleichzeitig ausgeführt werden, z. B. das Aktualisieren von Eigenschaften oder das Ausführen eines Befehls.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 06/08/2020
ms.topic: how-to
ms.openlocfilehash: dec9abc38bc0354ef3d22994a7988bfb006f5769
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84609729"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung

Sie können Microsoft Azure IoT Central verwenden, um Ihre angeschlossenen Geräte bedarfsgerecht mithilfe von Aufträgen zu überwachen. Mit Aufträgen können Sie Massenaktualisierungen an Geräteeigenschaften vornehmen und Befehle ausführen. In diesem Artikel wird Ihnen der Einstieg in die Verwendung von Aufträgen in Ihrer eigenen Anwendung gezeigt.

## <a name="create-and-run-a-job"></a>Erstellen und Ausführen eines Auftrags

In diesem Abschnitt erfahren Sie, wie Sie einen Auftrag erstellen und ausführen. Es wird gezeigt, wie Sie den leichten Schwellenwert für eine Gruppe von logistischen Gatewaygeräten festlegen.

1. Navigieren Sie im linken Bereich zu **Aufträge**.

2. Wählen Sie **+ Neu** aus, um einen neuen Auftrag zu erstellen:

    ![Neuen Auftrag erstellen](./media/howto-run-a-job/create-new-job.png)

3. Geben Sie einen Namen und eine Beschreibung ein, mit denen Sie Ihren Auftrag wiedererkennen können.

4. Wählen Sie die Zielgerätegruppe aus, auf die der Auftrag angewandt werden soll. Im Abschnitt **Zusammenfassung** können Sie sehen, auf wie viele Geräte Ihre Auftragskonfiguration angewendet wird.

5. Wählen Sie als Nächstes entweder **Cloudeigenschaft**, **Eigenschaft** oder **Befehl** als Typ für den zu konfigurierenden Auftrag aus. Zum Einrichten einer Auftragskonfiguration des Typs **Eigenschaft** wählen Sie die gewünschte Eigenschaft aus, und legen Sie deren neuen Wert fest. Zum Einrichten eines **Befehls** wählen Sie den auszuführenden Befehl aus. Mit einem Eigenschaftenauftrag können mehrere Eigenschaften festgelegt werden:

    ![Auftrag konfigurieren](./media/howto-run-a-job/configure-job.png)

6. Nachdem Sie Ihren Auftrag erstellt haben, wählen Sie **Ausführen** oder **Speichern**. Der Auftrag wird nun auf der Hauptseite **Aufträge** angezeigt. Auf dieser Seite können Sie Ihren derzeit ausgeführten Auftrag sowie den Verlauf aller zuvor ausgeführten oder gespeicherten Aufträge sehen. Sie können Ihren gespeicherten Auftrag jederzeit erneut öffnen, um ihn weiter zu bearbeiten oder auszuführen:

    ![Auftrag anzeigen](./media/howto-run-a-job/view-job.png)

    > [!NOTE]
    > Sie können den Verlauf von bis zu 30 Tagen für Ihre zuvor ausgeführten Aufträge anzeigen.

7. Eine Übersicht zu Ihrem Auftrag erhalten Sie, indem Sie den anzuzeigenden Auftrag aus der Liste auswählen. Diese Übersicht enthält die Auftragsdetails, Geräte und Gerätestatuswerte. In dieser Übersicht können Sie auch **Auftragsdetails herunterladen** auswählen, um eine CSV-Datei mit Ihren Auftragsdetails, einschließlich der Geräte und ihrer Statuswerte, herunterzuladen. Diese Informationen können bei der Problembehandlung hilfreich sein:

    ![Anzeigen des Gerätestatus](./media/howto-run-a-job/download-details.png)

## <a name="manage-jobs"></a>Aufträge verwalten

Wenn Sie einen Ihrer gerade ausgeführten Aufträge anhalten möchten, öffnen Sie ihn, und wählen Sie **Beenden**  aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag angehalten wurde. Im Abschnitt **Zusammenfassung** wird angezeigt, welche Geräte abgeschlossen wurden, fehlgeschlagen sind oder noch ausstehen.

Wenn Sie einen derzeit angehaltenen Auftrag ausführen möchten, wählen Sie ihn und dann **Ausführen** aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag wieder ausgeführt wird. Der Abschnitt **Zusammenfassung** wird weiterhin mit dem neuesten Status aktualisiert.

![Auftrag verwalten](./media/howto-run-a-job/manage-job.png)

## <a name="copy-a-job"></a>Kopieren eines Auftrags

Wenn Sie einen Ihrer vorhandenen Aufträge kopieren möchten, wählen Sie ihn auf der Seite **Aufträge** und dann **Kopieren** aus. Daraufhin wird eine Kopie der Auftragskonfiguration geöffnet, die Sie bearbeiten können; an den Auftragsnamen ist **Kopie** angefügt. Sie können den neuen Auftrag speichern oder ausführen:

![Auftrag kopieren](./media/howto-run-a-job/copy-job.png)

## <a name="view-job-status"></a>Anzeigen des Auftragsstatus

Nachdem ein Auftrag erstellt wurde, wird die Spalte **Status** mit der neuesten Statusmeldung des Auftrags aktualisiert. In der folgenden Tabelle werden die möglichen Statuswerte aufgeführt:

| Statusmeldung       | Bedeutung der Statusmeldung                                          |
| -------------------- | ------------------------------------------------------- |
| Abgeschlossen            | Dieser Auftrag wurde auf allen Geräten ausgeführt.              |
| Fehler               | Bei diesem Auftrag ist ein Fehler aufgetreten. Er wurde auf den Geräten nicht vollständig ausgeführt.  |
| Ausstehend              | Mit der Ausführung dieses Auftrags wurde auf den Geräten noch nicht begonnen.         |
| Wird ausgeführt              | Dieser Auftrag wird derzeit auf Geräten ausgeführt.             |
| Beendet              | Dieser Auftrag wurde von einem Benutzer manuell beendet.           |

Der Statusmeldung folgt eine Übersicht über die Geräte im Auftrag. In der folgenden Tabelle werden die möglichen Gerätestatuswerte aufgeführt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Erfolgreich            | Die Anzahl der Geräte, auf denen der Auftrag erfolgreich ausgeführt wurde.       |
| Fehler               | Die Anzahl der Geräte, auf denen der Auftrag nicht erfolgreich ausgeführt wurde.       |

### <a name="view-the-device-status-values"></a>Anzeigen der Werte für den Gerätestatus

Zum Anzeigen des Status eines Auftrags und aller betroffenen Geräte öffnen Sie den Auftrag. Neben den einzelnen Gerätenamen werden folgende Statusmeldungen angezeigt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Abgeschlossen            | Der Auftrag wurde auf diesem Gerät ausgeführt.                                     |
| Fehler               | Beim Ausführen des Auftrags auf diesem Gerät ist ein Fehler aufgetreten. In der Fehlermeldung sind weitere Informationen enthalten.  |
| Ausstehend              | Der Auftrag wurde auf diesem Gerät noch nicht ausgeführt.                                   |

Klicken Sie auf **Herunterladen**, um eine CSV-Datei herunterzuladen, die die Auftragsdetails sowie eine Liste der Geräte mit den entsprechenden Statuswerten enthält.

### <a name="filter-the-list-of-devices"></a>Filtern der Geräteliste

Sie können die Geräteliste auf der Seite mit den Auftragsdetails filtern, indem Sie auf das Filtersymbol klicken. Außerdem können Sie die Felder **Geräte-ID** und **Status** zum Filtern verwenden:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Filtern der Geräteliste":::

### <a name="customize-columns-in-the-device-list"></a>Anpassen der Spalten der Geräteliste

Sie können zusätzliche Spalten auswählen, die in der Geräteliste angezeigt werden sollen, indem Sie auf das Symbol für die Spaltenoptionen klicken:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Spaltenoptionen":::

Es wird ein Dialogfeld angezeigt, in dem Sie die Spalten auswählen können, die in der Geräteliste angezeigt werden sollen. Wählen Sie die Spalten aus, die Sie anzeigen möchten, klicken Sie auf den Pfeil nach rechts und anschließend auf **OK**. Aktivieren Sie die Option **Alle auswählen**, um alle verfügbaren Spalten auszuwählen:

:::image type="content" source="media/howto-run-a-job/column-picker-popup.png" alt-text="Dialogfeld für die Spaltenauswahl":::

Die ausgewählten Spalten werden in der Geräteliste angezeigt:

:::image type="content" source="media/howto-run-a-job/column-picker-column-selected.png" alt-text="Ausgewählte Spalten":::

Die ausgewählten Spalten werden in einer oder mehreren Benutzersitzungen beibehalten, die Zugriff auf die Anwendung haben.

## <a name="rerun-jobs"></a>Erneutes Ausführen von Aufträgen

Sie können einen Auftrag, bei dem bei Geräten ein Fehler aufgetreten ist, erneut ausführen. Klicken Sie auf **Erneut ausführen**:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Erneutes Ausführen eines Auftrags":::

Geben Sie einen Auftragsnamen und eine Beschreibung an, und klicken Sie dann auf **Rerun job** (Auftrag erneut ausführen). Ein neuer Auftrag wird übermittelt, um die Aktion auf Geräten, bei denen ein Fehler aufgetreten ist, erneut auszuführen:

:::image type="content" source="media/howto-run-a-job/rerun-failed.png" alt-text="Erneutes Ausführen eines Auftrags auf Geräten, bei denen ein Fehler aufgetreten ist":::

> [!NOTE]
> Sie können nicht mehr als fünf Aufträge gleichzeitig aus einer IoT Central-Anwendung ausführen.

> [!NOTE]
> Wenn Sie nach Abschluss eines Auftrags ein Gerät löschen, das sich in der Geräteliste für diesen Auftrag befindet, wird es im Geräteeintrag beim Gerätenamen als gelöscht angezeigt, und der Link zu den Gerätedetails für das gelöschte Gerät ist nicht mehr verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Aufträge in Ihrer Azure IoT Central-Anwendung erstellt werden, können Sie die nächsten Schritte ausführen:

- [Verwalten von Geräten](howto-manage-devices.md)
- [Verwalten der Versionen Ihrer Gerätevorlage](howto-version-device-template.md)
