---
title: include file
description: include file
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 03/12/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9618dc1cef8d04cc5906579af0ef372694aeaaa9
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674692"
---
## <a name="create-a-device-template"></a>Erstellen einer Gerätevorlage

Erstellen Sie auf Ihrem lokalen Computer einen Ordner namens `environmental-sensor`.

Laden Sie die [JSON-Datei für das Umgebungssensor-Funktionsmodell](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json) herunter, und speichern Sie sie im Ordner `environmental-sensor`.

Öffnen Sie die heruntergeladene Datei `EnvironmentalSensorInline.capabilitymodel.json` in einem Text-Editor, und ersetzen Sie die beiden Instanzen von `{YOUR_COMPANY_NAME_HERE}` durch den Namen Ihres Unternehmens.

Erstellen Sie in Ihrer Azure IoT Central-Anwendung eine Gerätevorlage mit dem Namen *Environmental sensor* (Umgebungssensor), indem Sie die Gerätefunktionsmodell-Datei `EnvironmentalSensorInline.capabilitymodel.json` importieren:

![Gerätevorlage mit importiertem Gerätefunktionsmodell](./media/iot-central-add-environmental-sensor/device-template.png)

Das Gerätefunktionsmodell enthält zwei Schnittstellen: die Standardschnittstelle **Device Information** (Geräteinformationen) und die benutzerdefinierte Schnittstelle **Environmental Sensor** (Umgebungssensor). Die Schnittstelle **Environmental Sensor** (Umgebungssensor) definiert folgende Funktionen:

| type | Anzeigename | BESCHREIBUNG |
| ---- | ------------ | ----------- |
| Eigenschaft | Gerätestatus     | Der Zustand des Geräts. Mögliche Zustände: online und offline. |
| Eigenschaft (schreibbar) | Customer Name    | Der Name des Kunden, der das Gerät momentan verwendet. |
| Eigenschaft (schreibbar) | Brightness Level (Helligkeitsstufe) | Die Helligkeitsstufe für das Licht des Geräts. Mögliche Werte: 1 (hoch), 2 (mittel), 3 (niedrig). |
| Telemetrie | Temperatur | Die aktuelle vom Gerät ermittelte Temperatur. |
| Telemetrie | Luftfeuchtigkeit    | Die aktuelle vom Gerät ermittelte Luftfeuchtigkeit. |
| Get-Help | blink          | Lässt die LED des Geräts für das angegebene Zeitintervall blinken. |
| Get-Help | turnon         | Schaltet die LED des Geräts ein. |
| Get-Help | turnoff        | Schaltet die LED des Geräts aus. |
| Get-Help | rundiagnostics | Dieser asynchrone Befehl startet eine Diagnose auf dem Gerät. |

Wählen Sie in der Gerätevorlage die Option **Anpassen** aus, um die Darstellung der Eigenschaft **Device State** (Gerätezustand) in Ihrer IoT Central-Anwendung anzupassen. Erweitern Sie den Eintrag **Device State** (Gerätezustand), und geben Sie unter **Name: TRUE** den Wert _Online_ und unter **Name: FALSE** den Wert _Offline_ ein. Speichern Sie die Änderungen:

![Anpassen der Gerätevorlage](./media/iot-central-add-environmental-sensor/customize-template.png)

## <a name="create-views"></a>Erstellen von Ansichten

Über Ansichten können Sie mit Geräten interagieren, die mit Ihrer IoT Central-Anwendung verbunden sind. So können Sie beispielsweise Ansichten verwenden, die Telemetriedaten oder Eigenschaften anzeigen oder in denen Sie schreibbare und cloudbasierte Eigenschaften bearbeiten können. Ansichten sind Teil einer Gerätevorlage.

Gehen Sie wie folgt vor, um Ihrer Gerätevorlage **Environmental sensor** (Umgebungssensor) einige Standardansichten hinzuzufügen: Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** und anschließend die Kachel **Standardansichten generieren** aus. Vergewissern Sie sich, dass **Übersicht** und **Info** auf **Ein** festgelegt sind, und wählen Sie anschließend **Standarddashboardansichten generieren** aus. In Ihrer Vorlage sind nun zwei Standardansichten definiert.

Die Schnittstelle **Environmental Sensor** (Umgebungssensor) enthält zwei schreibbare Eigenschaften: **Customer Name** (Kundenname) und **Brightness Level** (Helligkeitsstufe). Diese Eigenschaften können bearbeitet werden, um eine Ansicht zu erstellen:

1. Wählen Sie **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus.

1. Geben Sie _Properties_ (Eigenschaften) als Formularname ein.

1. Wählen Sie die Eigenschaften **Brightness Level** (Helligkeitsstufe) und **Customer Name** (Kundenname) aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus.

1. Speichern Sie die Änderungen.

![Hinzufügen einer Ansicht, um die Bearbeitung von Eigenschaften zu ermöglichen](./media/iot-central-add-environmental-sensor/properties-view.png)

## <a name="publish-the-template"></a>Veröffentlichen der Vorlage

Bevor Sie ein Gerät hinzufügen können, das die Gerätevorlage **Umgebungssensor** verwendet, müssen Sie die Vorlage veröffentlichen.

Wählen Sie in der Gerätevorlage die Option **Veröffentlichen** aus. Wählen Sie im Bereich **Diese Gerätevorlage für die Anwendung veröffentlichen** die Option **Veröffentlichen** aus.

Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Geräte**, um zu überprüfen, ob die Vorlage verwendet werden kann. Der Abschnitt **Geräte** enthält eine Liste der veröffentlichten Geräte in der Anwendung:

![Veröffentlichte Vorlagen auf der Seite „Geräte“](./media/iot-central-add-environmental-sensor/published-templates.png)

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Fügen Sie der Gerätevorlage, die Sie im vorherigen Abschnitt erstellt haben, in Ihrer Azure IoT Central-Anwendung ein echtes Gerät hinzu:

1. Wählen Sie auf der Seite **Geräte** die Gerätevorlage **Environmental Sensor** (Umgebungssensor) aus.

    > [!TIP]
    > Die Vorlage muss ausgewählt werden, bevor Sie **+ Neu** auswählen. Anderenfalls wird ein nicht zugeordnetes Gerät erstellt.

1. Wählen Sie **+ Neu** aus.

1. Stellen Sie sicher, dass **Simuliert** auf **Aus** festgelegt ist. Klicken Sie anschließend auf **Erstellen**.

Klicken Sie auf den Gerätenamen, und wählen Sie anschließend die Option **Verbinden** aus. Notieren Sie sich die auf der Seite **Geräteverbindung** angegebenen Geräteverbindungsinformationen: **ID-Bereich**, **Geräte-ID** und **Primärschlüssel**. Diese Werte werden beim Erstellen des Gerätecodes benötigt:

![Geräteverbindungsinformationen](./media/iot-central-add-environmental-sensor/device-connection.png)
