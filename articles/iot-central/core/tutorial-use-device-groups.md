---
title: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Operator mithilfe von Gerätegruppen Telemetriedaten aus Geräten in Ihrer Azure IoT Central-Anwendung analysieren.
author: dominicbetts
ms.author: dobett
ms.date: 02/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 58fc71ab05c34e8acd252e7a1984c55996d1b3a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80999038"
---
# <a name="tutorial-use-device-groups-to-analyze-device-telemetry"></a>Tutorial: Verwenden von Gerätegruppen zum Analysieren von Gerätetelemetriedaten

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen zum Analysieren von Gerätetelemetriedaten in Ihrer Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste der Geräte, die gruppiert werden, weil sie einigen der angegebenen Kriterien entsprechen. Gerätegruppen unterstützen Sie bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem Geräte in kleineren, logischen Gruppen gruppiert werden. Sie können beispielsweise eine Gerätegruppe zum Auflisten aller Klimageräte in Seattle erstellen, damit ein Techniker alle Geräte finden kann, für die er zuständig ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Gerätegruppe
> * Verwenden einer Gerätegruppe zum Analysieren von Gerätetelemetriedaten

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie zunächst anhand der Schnellstartanleitungen [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md) und [Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung](./quick-create-simulated-device.md) die Gerätevorlage **MXChip IoT DevKit**, um sie hier verwenden zu können.

## <a name="create-simulated-devices"></a>Erstellen von simulierten Geräten

Fügen Sie vor dem Erstellen einer Gerätegruppe mindestens fünf simulierte Geräte aus der Gerätevorlage **MXChip IoT DevKit** hinzu, um sie in diesem Tutorial zu verwenden:

![Fünf simulierte Sensorgeräte](./media/tutorial-use-device-groups/simulated-devices.png)

Verwenden Sie bei vier der simulierten Sensorgeräte die Ansicht **Gerät verwalten**, um den Kundennamen auf *Contoso* festzulegen:

![Festlegen des Kundennamens auf „Contoso“](./media/tutorial-use-device-groups/customer-name.png)

## <a name="create-a-device-group"></a>Erstellen einer Gerätegruppe

Gehen Sie wie folgt vor, um eine Gerätegruppe zu erstellen:

1. Wählen Sie im linken Bereich **Gerätegruppen** aus.

1. Wählen Sie **+** aus:

    ![Neue Gerätegruppe](media/tutorial-use-device-groups/image1.png)

1. Nennen Sie Ihre Gerätegruppe *Contoso-Geräte*. Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie die Gerätevorlage **MXChip IoT DevKit** aus, die für diese Gruppe verwendet werden soll.

1. Wenn Sie die Gruppe so anpassen möchten, dass sie nur die zu **Contoso** gehörenden Geräte enthält, wählen Sie **+ Filter** aus. Wählen Sie die Eigenschaft **Kundenname**, den Vergleichsoperator **Ist gleich** und **Contoso** als Wert aus. Sie können mehrere Filter hinzufügen. Dann werden Geräte, die **alle** Filterkriterien erfüllen, in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann:

    ![Gerätegruppenabfrage](media/tutorial-use-device-groups/image2.png)

    > [!TIP]
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
