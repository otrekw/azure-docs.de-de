---
title: 'Tutorial: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation'
description: 'Tutorial: Hier erfahren Sie, wie Sie mithilfe von Gerätegruppen Telemetriedaten von Geräten in Ihrer Azure IoT Central-Anwendung analysieren.'
author: dominicbetts
ms.author: dobett
ms.date: 11/16/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: da7c1c0268f04b183ba48491bd5f0d0b01e15b41
ms.sourcegitcommit: f4e04fe2dfc869b2553f557709afaf057dcccb0b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113224107"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry"></a>Tutorial: Verwenden von Gerätegruppen zum Analysieren von Gerätetelemetriedaten

In diesem Artikel wird beschrieben, wie Sie Gerätegruppen zum Analysieren von Gerätetelemetriedaten in Ihrer Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste der Geräte, die gruppiert werden, weil sie einigen der angegebenen Kriterien entsprechen. Gerätegruppen unterstützen Sie bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem Geräte in kleineren, logischen Gruppen gruppiert werden. Sie können beispielsweise eine Gerätegruppe zum Auflisten aller Klimageräte in Seattle erstellen, damit ein Techniker alle Geräte finden kann, für die er zuständig ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Gerätegruppe
> * Verwenden einer Gerätegruppe zum Analysieren von Gerätetelemetriedaten

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen der Schritte in diesem Tutorial benötigen Sie Folgendes:

[!INCLUDE [iot-central-prerequisites-basic](../../../includes/iot-central-prerequisites-basic.md)]

## <a name="add-and-customize-a-device-template"></a>Hinzufügen und Anpassen einer Gerätevorlage

Fügen Sie eine Gerätevorlage aus dem Gerätekatalog hinzu. In diesem Tutorial wird die Gerätevorlage **ESP32-Azure IoT Kit** verwendet:

1. Wählen Sie zum Hinzufügen einer neuen Gerätevorlage auf der Seite **Gerätevorlagen** die Option **+ Neu** aus.

1. Scrollen Sie auf der Seite **Typ auswählen** nach unten, bis im Abschnitt **Vorkonfigurierte Gerätevorlage verwenden** die Kachel **ESP32-Azure IoT-Kit** angezeigt wird.

1. Wählen Sie die Kachel **ESP32-Azure IoT-Kit** und dann die folgende Option aus: **Next: Review** (Weiter: Überprüfen).

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus.

Der Name der Vorlage, die Sie erstellt haben, lautet **Sensor Controller**. Das Modell enthält Komponenten, z. B. **Sensor Controller**, **SensorTemp** und **Schnittstelle „Geräteinformationen“** . Mit Komponenten werden die Funktionen eines ESP32-Geräts definiert. Zu den Funktionen gehören die Bereiche Telemetrie, Eigenschaften und Befehle.

Fügen Sie der Gerätevorlage **Sensor Controller** zwei Cloudeigenschaften hinzu:

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen** aus. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage zwei Cloudeigenschaften hinzuzufügen:

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Customer Name     | Keine          | String |

1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.

Fügen Sie der Gerätevorlage ein neues Formular hinzu, um das Gerät zu verwalten:

1. Wählen Sie den Knoten **Ansichten** und anschließend die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.

1. Ändern Sie den Formularnamen in **Manage device**.

1. Wählen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** sowie die Eigenschaft **Zieltemperatur** aus. Wählen Sie anschließend **Abschnitt hinzufügen** aus.

1. Wählen Sie **Speichern** aus, um Ihr neues Formular zu speichern.

Veröffentlichen Sie die Gerätevorlage jetzt.

## <a name="create-simulated-devices"></a>Erstellen von simulierten Geräten

Fügen Sie vor dem Erstellen einer Gerätegruppe mindestens fünf simulierte Geräte auf der Grundlage der Gerätevorlage **Sensor Controller** hinzu, um sie in diesem Tutorial zu verwenden:

:::image type="content" source="media/tutorial-use-device-groups/simulated-devices.png" alt-text="Screenshot: Fünf simulierte Geräte vom Typ „Sensor Controller“":::

Verwenden Sie bei vier der simulierten Sensorgeräte die Ansicht **Gerät verwalten**, um den Kundennamen auf *Contoso* festzulegen, und wählen Sie **Speichern** aus.

:::image type="content" source="media/tutorial-use-device-groups/customer-name.png" alt-text="Screenshot: Festlegen der Cloudeigenschaft „Kundenname“":::

## <a name="create-a-device-group"></a>Erstellen einer Gerätegruppe

1. Wählen Sie im linken Bereich **Gerätegruppen** aus, um zur Seite „Gerätegruppen“ zu navigieren.

1. Wählen Sie **+ Neu** aus.

1. Nennen Sie Ihre Gerätegruppe *Contoso devices* (Contoso-Geräte). Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie für diese Gruppe die Gerätevorlage **Sensor Controller** aus.

1. Wenn Sie die Gruppe so anpassen möchten, dass sie nur die zu **Contoso** gehörenden Geräte enthält, wählen Sie **+ Filter** aus. Wählen Sie die Eigenschaft **Kundenname**, den Vergleichsoperator **Ist gleich** und **Contoso** als Wert aus. Sie können mehrere Filter hinzufügen. Dann werden Geräte, die **alle** Filterkriterien erfüllen, in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann.

    > [!TIP]
    > Die Gerätegruppe ist eine dynamische Abfrage. Jedes Mal, wenn Sie die Liste der Geräte anzeigen, werden möglicherweise andere Geräte in der Liste angezeigt. Die Liste hängt davon ab, welche Geräte derzeit die Kriterien der Abfrage erfüllen.

1. Wählen Sie **Speichern** aus.

:::image type="content" source="media/tutorial-use-device-groups/device-group-query.png" alt-text="Screenshot: Konfiguration der Gerätegruppenabfrage":::

> [!NOTE]
> Wählen Sie für „Azure IoT Edge-Geräte“ die Option „Azure IoT Edge-Vorlagen“ aus, um eine Gerätegruppe zu erstellen.

## <a name="analytics"></a>Analytics

Sie können **Analytics** bei einer Gerätegruppe verwenden, um die Telemetriedaten aus den Geräten in der Gruppe zu analysieren. Beispielsweise können Sie die durchschnittliche Temperatur darstellen, die von allen Contoso-Umgebungssensoren gemeldet wird.

So analysieren Sie die Telemetriedaten für eine Gerätegruppe:

1. Wählen Sie im linken Bereich **Analytics** aus.

1. Wählen Sie in der von Ihnen erstellten Gerätegruppe **Contoso-Geräte** aus. Fügen Sie dann die Telemetrietypen **Temperatur** und **Luftfeuchtigkeit** hinzu:

    :::image type="content" source="media/tutorial-use-device-groups/create-analysis.png" alt-text="Screenshot: Für die Analyse ausgewählte Telemetrietypen":::

    Verwenden Sie das Symbol mit Auslassungspunkten (…) neben den Telemetrietypen, um einen Aggregationstyp auszuwählen. Der Standardwert ist **Durchschnitt**. Verwenden Sie **Gruppieren nach**, um die Darstellung der Aggregatdaten zu ändern. Sollten Sie z. B. nach „Geräte-ID“ aufteilen, wird für jedes Gerät ein Plot angezeigt, wenn Sie **Analysieren** auswählen.

1. Wählen Sie **Analysieren** aus, um die durchschnittlichen Telemetriewerte anzuzeigen:

    :::image type="content" source="media/tutorial-use-device-groups/view-analysis.png" alt-text="Screenshot: Durchschnittswerte für alle Contoso-Geräte":::

    Sie können die Ansicht anpassen, den angezeigten Zeitraum ändern und die Daten als CSV-Datei exportieren oder als Tabelle anzeigen.

    :::image type="content" source="media/tutorial-use-device-groups/export-data.png" alt-text="Screenshot: Exportieren der Daten von den Contoso-Geräten":::

Weitere Informationen finden Sie unter [Analysieren von Gerätedaten mithilfe von Analytics](howto-create-analytics.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](tutorial-create-telemetry-rules.md)
