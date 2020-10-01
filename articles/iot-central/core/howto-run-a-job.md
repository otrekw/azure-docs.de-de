---
title: Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Im Rahmen von Azure IoT Central-Aufträgen können Funktionen zur Verwaltung von mehreren Geräten gleichzeitig ausgeführt werden, z. B. das Aktualisieren von Eigenschaften oder das Ausführen eines Befehls.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/10/2020
ms.topic: how-to
ms.openlocfilehash: ae8b830469a9b52ae68310dde2e65dcffdf4e3be
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2020
ms.locfileid: "90060814"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Erstellen und Ausführen von Aufträgen in Ihrer Azure IoT Central-Anwendung

Sie können mithilfe von Azure IoT Central Ihre angeschlossenen Geräte anhand von Aufträgen bedarfsgerecht überwachen. Mit Aufträgen können Sie Massenaktualisierungen an Geräte- und Cloudeigenschaften vornehmen sowie Befehle ausführen. In diesem Artikel wird Ihnen der Einstieg in die Verwendung von Aufträgen in Ihrer eigenen Anwendung gezeigt.

## <a name="create-and-run-a-job"></a>Erstellen und Ausführen eines Auftrags

Im folgenden Beispiel wird gezeigt, wie Sie einen Auftrag erstellen und ausführen, um den leichten Schwellenwert für eine Gruppe von logistischen Gatewaygeräten festzulegen. Sie verwenden den Auftrags-Assistenten zum Erstellen und Ausführen von Aufträgen. Sie können einen Auftrag zur späteren Ausführung speichern.

1. Wählen Sie im linken Bereich **Aufträge** aus.

1. Wählen Sie **+ Neuer Auftrag** aus.

1. Geben Sie auf der Seite **Konfigurieren des Auftrags** einen Namen und eine Beschreibung zur Identifizierung des Auftrags ein, den Sie gerade erstellen.

1. Wählen Sie die Zielgerätegruppe aus, auf die der Auftrag angewandt werden soll. Unter Ihrer Auswahl **Gerätegruppe** können Sie sehen, für wie viele Geräte Ihre Auftragskonfiguration gilt.

1. Wählen Sie **Cloud-Eigenschaft**, **Eigenschaft** oder **Befehl** als **Auftragstyp** aus:

    Zum Einrichten einer Auftragskonfiguration des Typs **Eigenschaft** wählen Sie die gewünschte Eigenschaft aus, und legen Sie deren neuen Wert fest. Wählen Sie den auszuführenden Befehl aus, um eine **Befehl**-Auftragskonfiguration einzurichten. Mit einem Eigenschaftenauftrag können mehrere Eigenschaften festgelegt werden.

    :::image type="content" source="media/howto-run-a-job/configure-job.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

    Wählen Sie **Speichern und beenden** aus, um den Auftrag zur Liste von gespeicherten Aufträgen auf der Seite **Aufträge** hinzuzufügen. Sie können später zu einem Auftrag aus der Liste von gespeicherten Aufträgen zurückkehren.

    Wählen Sie **Weiter** aus, um zur Registerkarte **Überprüfen** zu wechseln. Auf der Seite **Überprüfung** werden die Details der Auftragskonfiguration angezeigt. Wählen Sie **Ausführen** aus, um den Auftrag zu übermitteln.

    :::image type="content" source="media/howto-run-a-job/job-wizard-review.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

1. Ein Auftrag durchläuft die Phasen *Ausstehend*, *Wird ausgeführt* und *Abgeschlossen*. Die Auftragsausführungsdetails enthalten Metriken des Typs „Ergebnis“, Details zu „Dauer“ und das Raster der Geräteliste.

    Wenn der Auftrag abgeschlossen wurde, können Sie **Ergebnisprotokoll** auswählen, um eine CSV-Datei mit Ihren Auftragsdetails, einschließlich der Geräte und deren Statuswerten, herunterzuladen. Diese Informationen können bei der Problembehandlung hilfreich sein.

    :::image type="content" source="media/howto-run-a-job/download-details.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

1. Der Auftrag wird jetzt auf der Seite **Aufträge** in der Liste **Letzte 30 Tage** angezeigt. Auf dieser Seite werden Ihre zurzeit ausgeführten Aufträge sowie der Verlauf aller zuvor ausgeführten oder gespeicherten Aufträge gezeigt.

    > [!NOTE]
    > Sie können den Verlauf von bis zu 30 Tagen für Ihre zuvor ausgeführten Aufträge anzeigen.

## <a name="manage-jobs"></a>Aufträge verwalten

Um einen aktiven Auftrag zu beenden, öffnen Sie ihn und wählen Sie **Anhalten** aus. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag angehalten wurde. Im Abschnitt **Zusammenfassung** wird angezeigt, welche Geräte abgeschlossen wurden, fehlgeschlagen sind oder noch ausstehen.

:::image type="content" source="media/howto-run-a-job/manage-job.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

Wenn sich ein Auftrag in einem angehaltenen Zustand befindet, können Sie **Fortsetzen** auswählen, um seine Ausführung fortzusetzen. Der Auftragsstatus wird geändert, um anzugeben, dass der Auftrag wieder ausgeführt wird. Der Abschnitt **Zusammenfassung** wird weiterhin mit dem neuesten Status aktualisiert.

:::image type="content" source="media/howto-run-a-job/stopped-job.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

## <a name="copy-a-job"></a>Kopieren eines Auftrags

Wählen Sie zum Kopieren eines vorhandenen Auftrags einen bereits ausgeführten Auftrag aus. Wählen Sie auf der Seite „Auftragsergebnisse“ oder der Seite mit den Auftragsdetails **Kopieren** aus:

:::image type="content" source="media/howto-run-a-job/job-details-copy.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

Daraufhin wird eine Kopie der Auftragskonfiguration geöffnet, die Sie bearbeiten können; an den Auftragsnamen ist **Kopie** angefügt.

## <a name="view-job-status"></a>Anzeigen des Auftragsstatus

Nachdem ein Auftrag erstellt wurde, wird die Spalte **Status** mit der neuesten Auftragsstatusmeldung aktualisiert. In der folgenden Tabelle werden die möglichen Werte für den *Auftragsstatus* aufgeführt:

| Statusmeldung       | Bedeutung der Statusmeldung                                          |
| -------------------- | ------------------------------------------------------- |
| Abgeschlossen            | Dieser Auftrag wurde auf allen Geräten ausgeführt.              |
| Fehler               | Dieser Auftrag ist fehlgeschlagen und konnte auf Geräten nicht vollständig ausgeführt werden.  |
| Ausstehend              | Mit der Ausführung dieses Auftrags wurde auf den Geräten noch nicht begonnen.         |
| Wird ausgeführt              | Dieser Auftrag wird derzeit auf Geräten ausgeführt.             |
| Beendet              | Ein Benutzer hat diesen Auftrag manuell angehalten.           |

Der Statusmeldung folgt eine Übersicht über die Geräte im Auftrag. In der folgenden Tabelle werden die möglichen Werte für den *Gerätestatus* aufgeführt:

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

Wählen Sie **Ergebnisprotokoll** aus, um eine CSV-Datei mit den Auftragsdetails und der Liste der Geräte mit den entsprechenden Statuswerten herunterzuladen.

## <a name="filter-the-device-list"></a>Filtern der Geräteliste

Sie können die Geräteliste auf der Seite mit den **Auftragsdetails** filtern, indem Sie auf das Filtersymbol klicken. Außerdem können Sie zum Filtern die Felder **Geräte-ID** und **Status** verwenden:

:::image type="content" source="media/howto-run-a-job/filter.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

## <a name="customize-columns-in-the-device-list"></a>Anpassen der Spalten der Geräteliste

Sie können der Geräteliste Spalten hinzufügen, indem Sie das Symbol „Spaltenoptionen“ auswählen:

:::image type="content" source="media/howto-run-a-job/column-options.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

Über das Dialogfeld **Spaltenoptionen** können Sie die Spalten der Geräteliste auswählen. Wählen Sie die Spalten aus, die Sie anzeigen möchten. Wählen Sie den Pfeil nach rechts und anschließend **OK** aus. Wählen Sie **Alle auswählen**, um alle verfügbaren Spalten auszuwählen. Die ausgewählten Spalten werden in der Geräteliste angezeigt.

Ausgewählte Spalten werden in einer oder mehreren Benutzersitzungen beibehalten, die Zugriff auf die Anwendung haben.

## <a name="rerun-jobs"></a>Erneutes Ausführen von Aufträgen

Sie können einen Auftrag, bei dem bei Geräten ein Fehler aufgetreten ist, erneut ausführen. Wählen Sie **Rerun on failed** (Bei Fehler erneut ausführen) aus:

:::image type="content" source="media/howto-run-a-job/rerun.png" alt-text="Screenshot, der die Auswahl zum Erstellen des Eigenschaftenauftrags „Leichten Schwellenwert festlegen“ zeigt":::

Geben Sie einen Auftragsnamen und eine Beschreibung an, und wählen Sie dann **Rerun job** (Auftrag erneut ausführen) aus. Ein neuer Auftrag wird übermittelt, um die Aktion auf Geräten, bei denen ein Fehler aufgetreten ist, erneut auszuführen.

> [!NOTE]
> Sie können nicht mehr als fünf Aufträge gleichzeitig aus einer Azure IoT Central-Anwendung ausführen.
>
> Wenn ein Auftrag abgeschlossen ist und Sie ein Gerät löschen, das sich in der Geräteliste des Auftrags befindet, wird der Geräteeintrag im Gerätenamen als gelöscht angezeigt. Der Link „Details“ ist für das gelöschte Gerät nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Aufträge in Ihrer Azure IoT Central-Anwendung erstellt werden, können Sie die nächsten Schritte ausführen:

- [Verwalten von Geräten](howto-manage-devices.md)
- [Verwalten der Versionen Ihrer Gerätevorlage](howto-version-device-template.md)
