---
title: Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Im Rahmen von Azure IoT Central-Aufträgen können Funktionen zur Verwaltung von mehreren Geräten gleichzeitig ausgeführt werden, z. B. das Aktualisieren von Eigenschaften oder das Ausführen eines Befehls.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/03/2020
ms.topic: how-to
manager: peterpr
ms.openlocfilehash: c9d5a0daa364b09e45699e898511c28d4b4d92ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157754"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung

Sie können Microsoft Azure IoT Central verwenden, um Ihre angeschlossenen Geräte bedarfsgerecht mithilfe von Aufträgen zu überwachen. Mit Aufträgen können Sie Massenaktualisierungen an Geräteeigenschaften vornehmen und Befehle ausführen. In diesem Artikel wird Ihnen der Einstieg in die Verwendung von Aufträgen in Ihrer eigenen Anwendung gezeigt.

## <a name="create-and-run-a-job"></a>Erstellen und Ausführen eines Auftrags

In diesem Abschnitt erfahren Sie, wie Sie einen Auftrag erstellen und ausführen. Es wird gezeigt, wie Sie den leichten Schwellenwert für eine Gruppe von logistischen Gatewaygeräten festlegen.

1. Navigieren Sie im linken Bereich zu **Aufträge**.

2. Wählen Sie **+ Neu** aus, um einen neuen Auftrag zu erstellen:

    ![Neuen Auftrag erstellen](./media/howto-run-a-job/createnewjob.png)

3. Geben Sie einen Namen und eine Beschreibung ein, mit denen Sie Ihren Auftrag wiedererkennen können.

4. Wählen Sie die Zielgerätegruppe aus, auf die der Auftrag angewandt werden soll. Im Abschnitt **Zusammenfassung** können Sie sehen, auf wie viele Geräte Ihre Auftragskonfiguration angewendet wird.

5. Wählen Sie als Nächstes entweder **Cloudeigenschaft**, **Eigenschaft** oder **Befehl** als Typ des zu konfigurierenden Auftrags aus. Zum Einrichten einer Auftragskonfiguration des Typs **Eigenschaft** wählen Sie die gewünschte Eigenschaft aus, und legen Sie deren neuen Wert fest. Zum Einrichten eines **Befehls** wählen Sie den auszuführenden Befehl aus. Mit einem Eigenschaftenauftrag können mehrere Eigenschaften festgelegt werden:

    ![Auftrag konfigurieren](./media/howto-run-a-job/configurejob.png)

6. Nachdem Sie Ihren Auftrag erstellt haben, wählen Sie **Ausführen** oder **Speichern**. Der Auftrag wird nun auf der Hauptseite **Aufträge** angezeigt. Auf dieser Seite können Sie Ihren derzeit ausgeführten Auftrag sowie den Verlauf aller zuvor ausgeführten oder gespeicherten Aufträge sehen. Sie können Ihren gespeicherten Auftrag jederzeit erneut öffnen, um ihn weiter zu bearbeiten oder auszuführen:

    ![Auftrag anzeigen](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Sie können den Verlauf von bis zu 30 Tagen für Ihre zuvor ausgeführten Aufträge anzeigen.

7. Eine Übersicht zu Ihrem Auftrag erhalten Sie, indem Sie den anzuzeigenden Auftrag aus der Liste auswählen. Diese Übersicht enthält die Auftragsdetails, Geräte und Gerätestatuswerte. In dieser Übersicht können Sie auch **Auftragsdetails herunterladen** auswählen, um eine CSV-Datei mit Ihren Auftragsdetails, einschließlich der Geräte und ihrer Statuswerte, herunterzuladen. Diese Informationen können bei der Problembehandlung hilfreich sein:

    ![Anzeigen des Gerätestatus](./media/howto-run-a-job/downloaddetails.png)

### <a name="manage-a-job"></a>Verwalten eines Auftrags

Wenn Sie einen Ihrer gerade ausgeführten Aufträge anhalten möchten, öffnen Sie ihn, und wählen Sie **Beenden**  aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag angehalten wurde. Im Abschnitt **Zusammenfassung** wird angezeigt, welche Geräte abgeschlossen wurden, fehlgeschlagen sind oder noch ausstehen.

Wenn Sie einen derzeit angehaltenen Auftrag ausführen möchten, wählen Sie ihn und dann **Ausführen** aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag wieder ausgeführt wird. Der Abschnitt **Zusammenfassung** wird weiterhin mit dem neuesten Status aktualisiert.

![Auftrag verwalten](./media/howto-run-a-job/managejob.png)

## <a name="copy-a-job"></a>Kopieren eines Auftrags

Wenn Sie einen Ihrer vorhandenen Aufträge kopieren möchten, wählen Sie ihn auf der Seite **Aufträge** und dann **Kopieren** aus. Daraufhin wird eine Kopie der Auftragskonfiguration geöffnet, die Sie bearbeiten können; an den Auftragsnamen ist **Kopie** angefügt. Sie können den neuen Auftrag speichern oder ausführen:

![Auftrag kopieren](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Anzeigen des Auftragsstatus

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

### <a name="view-the-device-status"></a>Anzeigen des Gerätestatus

Zum Anzeigen des Status eines Auftrags und aller betroffenen Geräte öffnen Sie den Auftrag. Wählen Sie **Auftragsdetails herunterladen** aus, um eine CSV-Datei herunterzuladen, die die Auftragsdetails sowie eine Liste der Geräte mit ihren Statuswerten enthält. Neben den einzelnen Gerätenamen werden folgende Statusmeldungen angezeigt:

| Statusmeldung       | Bedeutung der Statusmeldung                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Abgeschlossen            | Der Auftrag wurde auf diesem Gerät ausgeführt.                                     |
| Fehler               | Beim Ausführen des Auftrags auf diesem Gerät ist ein Fehler aufgetreten. In der Fehlermeldung sind weitere Informationen enthalten.  |
| Ausstehend              | Der Auftrag wurde auf diesem Gerät noch nicht ausgeführt.                                   |

> [!NOTE]
> Wenn ein Gerät gelöscht wurde, können Sie es nicht mehr auswählen. Es wird als „gelöscht“ mit der Geräte-ID angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Aufträge in Ihrer Azure IoT Central-Anwendung erstellt werden, können Sie die nächsten Schritte ausführen:

- [Verwalten von Geräten](howto-manage-devices.md)
- [Verwalten der Versionen Ihrer Gerätevorlage](howto-version-device-template.md)
