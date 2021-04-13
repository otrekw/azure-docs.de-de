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
ms.openlocfilehash: 77fdaf297fff0e145b1dd53908887bc14f9d3f14
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491140"
---
<!-- All needs updating -->
Als Operator in Ihrer Azure IoT Central-Anwendung haben Sie folgende Möglichkeiten:

* Anzeigen der von den beiden Thermostatkomponenten gesendeten Telemetriedaten auf der Seite **Übersicht**:

    :::image type="content" source="media/iot-central-monitor-thermostat/view-telemetry.png" alt-text="Anzeigen der Gerätetelemetrie":::

* Anzeigen der Geräteeigenschaften auf der Seite **Info**. Auf dieser Seite werden die Eigenschaften der Geräteinformationskomponente und der beiden Thermostatkomponenten angezeigt:

    :::image type="content" source="media/iot-central-monitor-thermostat/about-properties.png" alt-text="Anzeigen von Geräteeigenschaften":::

## <a name="customize-the-device-template"></a>Anpassen der Gerätevorlage

Als Lösungsentwickler können Sie die Gerätevorlage anpassen, die von IoT Central automatisch erstellt wurde, als der Temperaturregler eine Verbindung hergestellt hat.

So fügen Sie eine Cloudeigenschaft hinzu, um den dem Gerät zugeordneten Kundennamen zu speichern:

1. Navigieren Sie in Ihrer IoT Central-Anwendung auf der Seite **Gerätevorlagen** zur Gerätevorlage **Temperaturregler**.

1. Wählen Sie in der Gerätevorlage **Temperaturregler** die Option **Cloudeigenschaften** aus.

1. Wählen Sie **Cloudeigenschaft hinzufügen** aus. Geben Sie unter **Anzeigename** den *Kundennamen* ein, und wählen Sie unter **Schema** die Option **Zeichenfolge** aus. Klicken Sie dann auf **Speichern**.

So passen Sie die Darstellung des Befehls **Get Max-Min report** (Bericht zu Maximal-/Mindesttemperatur abrufen) in Ihrer IoT Central-Anwendung an:

1. Wählen Sie in der Gerätevorlage die Option **Anpassen** aus.

1. Ersetzen Sie unter **getMaxMinReport (thermostat1)** die Option *Get Max-Min report* (Bericht zu Maximal-/Mindesttemperatur abrufen) durch *Get thermostat1 status report* (Statusbericht für "thermostat1" abrufen).

1. Ersetzen Sie unter **getMaxMinReport (thermostat2)** die Option *Get Max-Min report* (Bericht zu Maximal-/Mindesttemperatur abrufen) durch *Get thermostat2 status report* (Statusbericht für "thermostat2" abrufen).

1. Wählen Sie **Speichern** aus.

So passen Sie die Darstellung der schreibbaren Eigenschaften für die **Zieltemperatur** in der IoT Central-Anwendung an:

1. Wählen Sie in der Gerätevorlage die Option **Anpassen** aus.

1. Ersetzen Sie unter **targetTemperature (thermostat1)** die Option *Zieltemperatur* durch *Zieltemperatur (1)* .

1. Ersetzen Sie unter **targetTemperature (thermostat2)** die Option *Zieltemperatur* durch *Zieltemperatur (2)* .

1. Wählen Sie **Speichern** aus.

Die Thermostatkomponenten im Modell **Thermostat** enthalten die schreibbare Eigenschaft **Target Temperature** (Zieltemperatur), und die Gerätevorlage enthält die Cloudeigenschaft **Customer Name** (Kundenname). Erstellen Sie eine Ansicht, die von einem Operator zum Bearbeiten dieser Eigenschaften verwendet werden kann:

1. Wählen Sie **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus.

1. Geben Sie _Properties_ (Eigenschaften) als Formularname ein.

1. Wählen Sie die Eigenschaften **Target Temperature (1)** (Zieltemperatur (1)), **Target Temperature (2)** (Zieltemperatur (2)) und **Customer Name** (Kundenname) aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus.

1. Speichern Sie die Änderungen.

:::image type="content" source="media/iot-central-monitor-thermostat/properties-view.png" alt-text="Ansicht zum Aktualisieren von Eigenschaftswerten":::

## <a name="publish-the-device-template"></a>Veröffentlichen der Gerätevorlage

Damit die von Ihnen vorgenommenen Anpassungen von einem Operator angezeigt und verwendet werden können, müssen Sie die Gerätevorlage veröffentlichen.

Wählen Sie in der Gerätevorlage **Thermostat** die Option **Veröffentlichen** aus. Wählen Sie im Bereich **Diese Gerätevorlage für die Anwendung veröffentlichen** die Option **Veröffentlichen** aus.

Ein Operator kann nun die Ansicht **Eigenschaften** verwenden, um die Eigenschaftswerte zu aktualisieren, und auf der Seite mit Gerätebefehlen Befehle namens **Get thermostat1 status report** (Statusbericht für "thermostat1" abrufen) und **Get thermostat2 status report** (Statusbericht für "thermostat2" abrufen) aufrufen:

* Aktualisieren schreibbarer Eigenschaftswerte auf der Seite **Properties** (Eigenschaften):

    :::image type="content" source="media/iot-central-monitor-thermostat/update-properties.png" alt-text="Aktualisieren der Geräteeigenschaften":::

* Aufrufen der Befehle auf der Seite **Befehle**:

    :::image type="content" source="media/iot-central-monitor-thermostat/call-command.png" alt-text="Aufrufen des Befehls":::

    :::image type="content" source="media/iot-central-monitor-thermostat/command-response.png" alt-text="Anzeigen der Befehlsantwort":::
