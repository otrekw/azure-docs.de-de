---
title: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Operator mithilfe von Gerätegruppen Telemetriedaten aus Geräten in Ihrer Azure IoT Central-Anwendung analysieren.
author: dominicbetts
ms.author: dobett
ms.date: 10/29/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 281806999b08c3babbb753459835850ad9d733eb
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73894354"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry-preview-features"></a>Tutorial: Verwenden von Gerätegruppen zum Analysieren von Gerätetelemetriedaten (Vorschaufeatures)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen zum Analysieren von Gerätetelemetriedaten in Ihrer Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste der Geräte, die gruppiert werden, weil sie einigen der angegebenen Kriterien entsprechen. Gerätegruppen unterstützen Sie bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem Geräte in kleineren, logischen Gruppen gruppiert werden. Sie können beispielsweise eine Gerätegruppe zum Auflisten aller Klimageräte in Seattle erstellen, damit ein Techniker alle Geräte finden kann, für die er zuständig ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Gerätegruppe
> * Verwenden einer Gerätegruppe zum Analysieren von Gerätetelemetriedaten

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie zunächst mithilfe der Schnellstartanleitungen [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) und [Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung](./quick-create-pnp-device.md) die Gerätevorlage **Environment Sensor** (Umgebungssensor), um sie hier verwenden zu können.

## <a name="create-simulated-devices"></a>Erstellen von simulierten Geräten

Fügen Sie vor dem Erstellen einer Gerätegruppe mindestens fünf simulierte Geräte aus der Gerätevorlage **Environment Sensor** hinzu, um sie in diesem Tutorial zu verwenden:

![Fünf simulierte Umgebungssensorgeräte](./media/tutorial-use-device-groups/simulated-devices.png)

Verwenden Sie für vier der Umgebungssensorgeräte die Ansicht **Environmental Sensor properties** (Eigenschaften des Umgebungssensors), um den Kundennamen auf **Contoso** festzulegen:

![Festlegen des Kundennamens auf „Contoso“](./media/tutorial-use-device-groups/customer-name.png)

## <a name="create-a-device-group"></a>Erstellen einer Gerätegruppe

Gehen Sie wie folgt vor, um eine Gerätegruppe zu erstellen:

1. Wählen Sie im linken Bereich **Gerätegruppen** aus.

1. Wählen Sie **+ Neu** aus.

    ![Neue Gerätegruppe](media/tutorial-use-device-groups/image1.png)

1. Geben Sie Ihrer Gerätegruppe einen Namen wie **Contoso-Geräte**. Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie die Gerätevorlage **Umgebungssensor** aus, die für diese Gruppe verwendet werden soll.

1. Erstellen Sie die Abfrage zum Identifizieren der Geräte für die Gerätegruppe, die zu **Contoso** gehören, indem Sie die Eigenschaft **Kundenname**, den Vergleichsoperator **Gleich** und **Contoso** als Wert auswählen. Sie können mehrere Abfragen hinzufügen, und Geräte, die **alle** Kriterien erfüllen, werden in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann.

    ![Gerätegruppenabfrage](media/tutorial-use-device-groups/image2.png)

    > [!NOTE]
    > Die Gerätegruppe ist eine dynamische Abfrage. Jedes Mal, wenn Sie die Liste der Geräte anzeigen, werden möglicherweise andere Geräte in der Liste angezeigt. Die Liste hängt davon ab, welche Geräte derzeit die Kriterien der Abfrage erfüllen.

1. Wählen Sie **Speichern** aus.

> [!NOTE]
> Wählen Sie für „Azure IoT Edge-Geräte“ die Option „Azure IoT Edge-Vorlagen“ aus, um eine Gerätegruppe zu erstellen.

## <a name="analytics"></a>Analytics

Sie können **Analytics** bei einer Gerätegruppe verwenden, um die Telemetriedaten aus den Geräten in der Gruppe zu analysieren. Beispielsweise können Sie die durchschnittliche Temperatur darstellen, die von allen Contoso-Umgebungssensoren gemeldet wird.

So analysieren Sie die Telemetriedaten für eine Gerätegruppe:

1. Wählen Sie im linken Bereich **Analytics** aus.

1. Wählen Sie in der von Ihnen erstellten Gerätegruppe **Contoso-Geräte** aus. Fügen Sie dann die Telemetrietypen **Temperatur** und **Luftfeuchtigkeit** hinzu:

    ![Erstellen von Analysen](./media/tutorial-use-device-groups/create-analysis.png)

    Verwenden Sie die Zahnradsymbole neben den Telemetrietypen, um einen Aggregationstyp auszuwählen. Der Standardwert ist **Durchschnitt**. Verwenden Sie **Aufteilen nach**, um die Anzeigeart für die Aggregatdaten zu ändern. Sollten Sie z. B. nach „Geräte-ID“ aufteilen, wird für jedes Gerät ein Plot angezeigt, wenn Sie **Analysieren** auswählen.

1. Wählen Sie **Analysieren** aus, um die durchschnittlichen Telemetriewerte anzuzeigen:

    ![Anzeigen der Analyse](./media/tutorial-use-device-groups/view-analysis.png)

    Sie können die Ansicht anpassen, den angezeigten Zeitraum ändern und die Daten exportieren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](tutorial-create-telemetry-rules.md)
