---
title: 'Schnellstart: Hinzufügen eines simulierten Geräts zu Azure IoT Central'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Gerätevorlage erstellen und Ihrer IoT Central-Anwendung ein simuliertes Gerät hinzufügen.
author: dominicbetts
ms.author: dobett
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 2eb0e3ce2ac20c89d9c0176ca3e7b33dc839c923
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107718825"
---
# <a name="quickstart-add-a-simulated-device-to-your-iot-central-application"></a>Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung

Eine Gerätevorlage definiert die Funktionen eines Geräts, das eine Verbindung mit Ihrer IoT Central-Anwendung herstellt. Zu den Funktionen zählen die vom Gerät gesendeten Telemetriedaten, Geräteeigenschaften und die Befehle, auf die ein Gerät reagiert. Mithilfe einer Gerätevorlage können Sie sowohl echte als auch simulierte Geräte zu einer Anwendung hinzufügen. Mithilfe von simulierten Geräten können Sie das Verhalten Ihrer IoT Central-Anwendung testen, bevor Sie eine Verbindung mit echten Geräten herstellen.

In dieser Schnellstartanleitung fügen Sie eine Gerätevorlage für ein ESP32-Azure IoT-Kit-Entwicklungsboard hinzu und erstellen ein simuliertes Gerät. Für diese Schnellstartanleitung wird kein echtes Gerät benötigt. Stattdessen wird mit einer Simulation des Geräts gearbeitet. Ein ESP32-Gerät ermöglicht Folgendes:

* Senden von Telemetriedaten, z. B. Temperatur
* Melden von gerätespezifischen Eigenschaften, z. B. die Maximaltemperatur seit dem Neustart des Geräts
* Reagieren auf Befehle, z. B. „reboot“
* Melden von generischen Geräteeigenschaften, z. B. Firmwareversion und Seriennummer

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) aus, um mit der Vorlage **Benutzerdefinierte App > Benutzerdefinierte Anwendung** eine IoT Central-Anwendung zu erstellen.

## <a name="create-a-device-template"></a>Erstellen einer Gerätevorlage

Wählen Sie im linken Bereich die Registerkarte **Gerätevorlagen** aus, um Ihrer Anwendung eine neue Gerätevorlage hinzuzufügen.

:::image type="content" source="media/quick-create-simulated-device/device-definitions.png" alt-text="Screenshot: Leere Liste mit Gerätevorlagen":::

Eine Gerätevorlage enthält ein Gerätemodell, mit dem Folgendes definiert wird:

* Die vom Gerät gesendeten Telemetriedaten.
* Geräteeigenschaften.
* Die Befehle, auf die das Gerät reagiert.

### <a name="add-a-device-template"></a>Hinzufügen einer Gerätevorlage

Ein Gerätemodell kann Ihrer IoT Central-Anwendung auf unterschiedliche Weise hinzugefügt werden. Sie können ein von Grund auf neues Modell erstellen, ein Modell aus einer Datei importieren oder ein Gerät aus dem Gerätekatalog auswählen. IoT Central unterstützt auch einen Ansatz mit *Gerätepriorisierung*. Hierbei wird von der Anwendung automatisch ein Modell aus einem Repository importiert, wenn ein echtes Gerät erstmals eine Verbindung herstellt.

In dieser Schnellstartanleitung wählen Sie ein Gerät aus dem Gerätekatalog aus, um eine Gerätevorlage zu erstellen.

In den folgenden Schritten wird veranschaulicht, wie Sie den Gerätekatalog zum Importieren des Modells für ein **ESP32**-Gerät verwenden. Diese Geräte senden Telemetriedaten, z. B. die Temperatur, an Ihre Anwendung:

1. Wählen Sie zum Hinzufügen einer neuen Gerätevorlage auf der Seite **Gerätevorlagen** die Option **+ Neu** aus.

1. Scrollen Sie auf der Seite **Typ auswählen** nach unten, bis im Abschnitt **Vorkonfigurierte Gerätevorlage verwenden** die Kachel **ESP32-Azure IoT-Kit** angezeigt wird.

1. Wählen Sie die Kachel **ESP32-Azure IoT-Kit** und dann die folgende Option aus: **Next: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

1. Nach einigen Sekunden wird Ihre neue Gerätevorlage angezeigt:

    :::image type="content" source="media/quick-create-simulated-device/devkit-template.png" alt-text="Screenshot: Gerätevorlage für ESP32-Gerät":::

    Der Name der Vorlage lautet **Sensor Controller**. Das Modell enthält Komponenten, z. B. **Sensor Controller**, **SensorTemp** und **Schnittstelle „Geräteinformationen“** . Mit Komponenten werden die Funktionen eines ESP32-Geräts definiert. Zu den Funktionen gehören die Bereiche Telemetrie, Eigenschaften und Befehle.

### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet bzw. von einem Gerät empfangen. Fügen Sie wie folgt zwei Cloudeigenschaften hinzu:

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen** aus. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage zwei Cloudeigenschaften hinzuzufügen:

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Customer Name     | Keine          | String |

1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern:

    :::image type="content" source="media/quick-create-simulated-device/cloud-properties.png" alt-text="Screenshot: Zwei Cloudeigenschaften":::

## <a name="views"></a>Sichten

Sie können die Anwendung so anpassen, dass relevante Informationen zum Gerät angezeigt werden. Anpassungen ermöglichen anderen Benutzern die Verwaltung der mit der Anwendung verbundenen Geräte. Sie können zwei Arten von Ansichten erstellen, um mit Geräten zu interagieren:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten und der von ihnen gesendeten Telemetriedaten

### <a name="default-views"></a>Standardansichten

Standardansichten ermöglichen einen schnellen Einstieg in die Visualisierung wichtiger Geräteinformationen. Sie können für Ihre Gerätevorlage bis zu drei Standardansichten generieren:

* Über die Ansicht **Befehle** können Sie Befehle an Ihr Gerät senden.
* In der Ansicht **Übersicht** werden Diagramme und Metriken verwendet, um Gerätetelemetriedaten anzuzeigen.
* In der Ansicht **Info** werden Geräteeigenschaften angezeigt.

Wählen Sie in der Gerätevorlage den Knoten **Ansichten** aus. Wie Sie sehen, hat IoT Central beim Hinzufügen der Vorlage die Ansichten **Übersicht**, **Info** und **Rohdaten** generiert.

So fügen Sie ein neues Formular zum Verwalten des Geräts hinzu:

1. Wählen Sie den Knoten **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.

1. Ändern Sie den Formularnamen in **Manage device**.

1. Wählen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** sowie die Eigenschaft **Zieltemperatur** aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus:

    :::image type="content" source="media/quick-create-simulated-device/new-form.png" alt-text="Screenshot: Neu hinzugefügtes Formular für Gerätevorlage":::

1. Wählen Sie **Speichern** aus, um Ihr neues Formular zu speichern.

## <a name="publish-device-template"></a>Veröffentlichen der Gerätevorlage

Um ein simuliertes Gerät erstellen oder eine Verbindung für ein echtes Gerät herstellen zu können, müssen Sie zunächst Ihre Gerätevorlage veröffentlichen. Die Vorlage wurde zwar im Zuge der ursprünglichen Erstellung bereits von IoT Central veröffentlicht, nun muss allerdings die aktualisierte Version veröffentlicht werden.

Veröffentlichen Sie wie folgt eine Gerätevorlage:

1. Navigieren Sie über die Seite **Gerätevorlagen** zur Gerätevorlage **Sensor Controller**.

1. Wählen Sie am oberen Rand des Blatts auf der Befehlsleiste die Option **Veröffentlichen** aus.

1. Wählen Sie im daraufhin angezeigten Dialogfeld **Veröffentlichen** aus.

Veröffentlichte Gerätevorlagen werden auf der Seite **Geräte** angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätemodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können aber Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage ohne Versionsverwaltung ändern. Wählen Sie nach dem Vornehmen von Änderungen die Option **Veröffentlichen** aus, um diese Änderungen zur Verwendung an echte und simulierte Geräte zu pushen.

## <a name="add-a-simulated-device"></a>Hinzufügen eines simulierten Geräts

Verwenden Sie die von Ihnen erstellte Gerätevorlage **ESP32**, um Ihrer Anwendung ein simuliertes Gerät hinzuzufügen.

1. Wählen Sie im linken Bereich die Option **Geräte** aus, um ein neues Gerät hinzuzufügen. Auf der Registerkarte **Geräte** werden die Option **Alle Geräte** und die Gerätevorlage **Sensor Controller** für das ESP32-Gerät angezeigt. Wählen Sie **Sensor Controller** aus.

1. Wählen Sie **+ Neu** aus, um ein simuliertes DevKit-Gerät hinzuzufügen. Verwenden Sie die vorgeschlagene **Geräte-ID**, oder geben Sie Ihre eigene ein. Eine Geräte-ID kann Buchstaben, Ziffern und das Zeichen `-` enthalten. Sie können auch einen Namen für Ihr neues Gerät eingeben. Stellen Sie sicher, dass die Option **Dieses Gerät simulieren?** auf **Ja** festgelegt ist, und wählen Sie dann **Erstellen** aus.

    :::image type="content" source="media/quick-create-simulated-device/simulated-device.png" alt-text="Screenshot: Simuliertes Gerät „Sensor Controller“":::

Sie können nun mit den Ansichten interagieren, die Sie zuvor mithilfe von simulierten Daten erstellt haben:

1. Wählen Sie auf der Seite **Geräte** Ihr simuliertes Gerät aus.

    * Die **Übersicht** enthält einen Plot der simulierten Telemetriedaten:

        :::image type="content" source="media/quick-create-simulated-device/simulated-telemetry.png" alt-text="Screenshot: Übersichtsseite für simuliertes Gerät":::

    * In der Ansicht **Info** werden Eigenschaftswerte angezeigt.

    * In der Ansicht **Befehle** können Sie Befehle für das Gerät ausführen (z. B. **reboot**).

    * Bei der Ansicht **Geräte verwalten** handelt es sich um das Formular, das Sie zum Verwalten des Geräts erstellt haben.

    * In der Ansicht **Rohdaten** können Sie die vom Gerät gesendeten unformatierten Telemetriedaten und Eigenschaftswerte anzeigen. Diese Ansicht ist nützlich für das Debuggen von Geräten.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie eine Gerätevorlage vom Typ **Sensor Controller** für ein ESP32-Gerät erstellen und Ihrer Anwendung ein simuliertes Gerät hinzufügen.

Weitere Informationen zur Überwachung von Geräten, die mit Ihrer Anwendung verbunden sind, finden Sie in der folgenden Schnellstartanleitung:

> [!div class="nextstepaction"]
> [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)](./quick-configure-rules.md)
