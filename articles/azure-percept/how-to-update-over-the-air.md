---
title: Aktualisieren Ihres Azure Percept DK-Geräts per Funk (Over-the-Air-Update)
description: Hier erfahren Sie, wie Sie Over-the-Air-Updates für Ihr Azure Percept DK-Gerät erhalten.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: aa8f696b6a26e812256be4e93975844f2c721b6e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660386"
---
# <a name="update-your-azure-percept-dk-over-the-air"></a>Aktualisieren Ihres Azure Percept DK-Geräts per Funk (Over-the-Air-Update)

In dieser Anleitung erfahren Sie, wie Sie die Trägerplatine Ihres Azure Percept DK-Geräts per Funk („over the air“) mit Device Update für IoT Hub aktualisieren.

## <a name="import-your-update-file-and-manifest-file"></a>Importieren Sie Ihre Update- und Ihre Manifestdatei.

> [!NOTE]
> Falls Sie das Update bereits importiert haben, können Sie direkt mit **Erstellen einer Geräteupdategruppe** fortfahren.

1. Navigieren Sie zu der Azure IoT Hub-Instanz, die Sie für Ihr Azure Percept-Gerät verwenden. Wählen Sie im Menübereich auf der linken Seite unter **Automatische Geräteverwaltung** die Option **Geräteupdates** aus.
 
1. Im oberen Bereich des Bildschirms werden mehrere Registerkarten angezeigt. Wählen Sie die Registerkarte **Updates** aus.
 
1. Wählen Sie unter der Überschrift **Bereitstellung kann durchgeführt werden** die Option **+ Neues Update importieren** aus.
 
1. Klicken Sie auf die Felder unter **Manifestdatei für Import auswählen** und **Select Update Files** (Updatedateien auswählen), um die passende Manifestdatei (.json) und eine einzelne Updatedatei (.swu) auszuwählen. Die Updatedateien für Ihr Azure Percept-Gerät finden Sie [hier](https://go.microsoft.com/fwlink/?linkid=2155625).
 
1. Wählen Sie unter **Speichercontainer auswählen** das Ordnersymbol oder das Textfeld und anschließend das passende Speicherkonto aus.
 
1. Falls Sie bereits einen Speichercontainer erstellt haben, können Sie ihn wiederverwenden. Wählen Sie andernfalls **+ Container** aus, um einen neuen Speichercontainer für OTA-Updates zu erstellen. Wählen Sie den gewünschten Container aus, und klicken Sie auf **Auswählen**.
 
    >[!Note]
    >Falls Sie über keinen Container verfügen, werden Sie aufgefordert, einen zu erstellen.
 
1. Wählen Sie **Übermitteln** aus, um den Importvorgang zu starten. Die Übermittlung dauert etwa vier Minuten.
 
    >[!Note]
    >Möglicherweise werden Sie aufgefordert, eine CORS-Regel (Cross-Origin Resource Sharing, Ressourcenfreigabe zwischen verschiedenen Ursprüngen) für den Zugriff auf den ausgewählten Speichercontainer hinzuzufügen. Wählen Sie **Add rule and retry** (Regel hinzufügen und wiederholen) aus, um den Vorgang fortzusetzen.
 
    >[!Note]
    >Aufgrund der Imagegröße wird ggf. die Seite **Wird übermittelt...** angezeigt. Es kann bis zu fünf Minuten dauern, bis der nächste Schritt angezeigt wird.
    
1. Der Importvorgang beginnt, und Sie werden zur Registerkarte **Importverlauf** der Seite **Geräteupdates** umgeleitet. Klicken Sie auf **Aktualisieren**, um den Fortschritt des Importvorgangs zu überwachen. Der Vorgang kann je nach Größe des Updates einige Minuten oder auch länger dauern. (Zu Spitzenzeiten muss mit einer Dauer von bis zu einer Stunde gerechnet werden.)

1. Wenn in der Statusspalte angezeigt wird, dass der Import erfolgreich war, wählen Sie die Registerkarte **Bereitstellung kann durchgeführt werden** aus, und klicken Sie auf **Aktualisieren**. Ihr importiertes Update sollte nun in der Liste angezeigt werden.
 
## <a name="create-a-device-update-group"></a>Erstellen einer Geräteupdategruppe
Mit Device Update für IoT Hub können Sie ein Update auf bestimmte Gruppen von Azure Percept DK-Geräten ausrichten. Um eine Gruppe zu erstellen, müssen Sie Ihrer Gerätezielgruppe in Azure IoT Hub ein Tag hinzufügen.

> [!NOTE]
> Falls Sie bereits eine Gruppe erstellt haben, können Sie direkt mit dem nächsten Schritt fortfahren.

Anforderungen für Gruppentags:
- Sie können Ihrem Tag einen beliebigen Wert hinzufügen (außer **Nicht kategorisiert**, da es sich dabei um einen reservierten Wert handelt).
- Der Tagwert darf maximal 255 Zeichen enthalten.
- Der Tagwert darf nur folgende Sonderzeichen enthalten: „.“, „-“, „_“, „~“.
- Bei Tag- und Gruppennamen wird die Groß-/Kleinschreibung beachtet.
- Ein Gerät kann immer nur über ein einzelnes Tag verfügen. Wenn bereits ein Tag vorhanden ist und ein weiteres Tag hinzugefügt wird, wird das zuvor vorhandene Tag überschrieben.
- Ein Gerät kann nur einer einzelnen Gruppe angehören.

1. Fügen Sie Ihrem Gerät bzw. Ihren Geräten ein Tag hinzu.
    1. Suchen Sie im linken Navigationsbereich unter **IoT Edge** nach Ihrem Azure Percept DK-Gerät, und navigieren Sie zum zugehörigen **Gerätezwilling**.
    1. Fügen Sie wie im Anschluss gezeigt einen neuen Tagwert vom Typ **Device Update für IoT Hub** hinzu. (Ändern Sie dabei ```<CustomTagValue>``` in einen eigenen Wert – beispielsweise „AzurePerceptGroup1“.) Weitere Informationen zu Tags im JSON-Dokument für Gerätezwillinge finden Sie [hier](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins#device-twins).

    ```
    "tags": {
    "ADUGroup": "<CustomTagValue>"
    },
    ```

 
1. Klicken Sie auf **Speichern**, und beheben Sie ggf. vorhandene Formatierungsprobleme.
 
1. Erstellen Sie eine Gruppe, indem Sie ein vorhandenes Azure IoT Hub-Tag auswählen.
    1. Kehren Sie zu Ihrer Azure IoT Hub-Seite zurück.
    1. Wählen Sie im Menübereich auf der linken Seite unter **Automatische Geräteverwaltung** die Option **Geräteupdates** aus.
    1. Wählen Sie die Registerkarte **Gruppen** . Auf dieser Seite wird die Anzahl nicht gruppierter Geräte angezeigt, die mit Device Update verbunden sind.
    1. Wählen Sie **+ Hinzufügen** aus, um eine neue Gruppe zu erstellen.
    1. Wählen Sie in der Liste ein IoT Hub-Tag aus, und klicken Sie auf **Übermitteln**.
    1. Wenn die Gruppe erstellt wurde, werden das Updatekonformitätsdiagramm und die Gruppenliste aktualisiert. Das Diagramm gibt Aufschluss über die Anzahl von Geräten mit verschiedenen Konformitätszuständen: **Mit neuestem Update**, **Neue Updates verfügbar**, **Updates werden ausgeführt** und **Not yet grouped** (Noch nicht gruppiert).
 

## <a name="deploy-an-update"></a>Bereitstellen eines Updates
1. Die neu erstellte Gruppe sollte mit einem neuen Update unter **Verfügbare Updates** aufgelistet werden. (Möglicherweise muss die Ansicht einmal aktualisiert werden.) Wählen Sie das Update aus.
 
1. Vergewissern Sie sich, dass als Zielgerätegruppe die richtige Gerätegruppe ausgewählt ist. Wählen Sie ein **Startdatum** und eine **Startzeit** für Ihre Bereitstellung aus, und klicken Sie anschließend auf **Bereitstellung erstellen**. 

    >[!CAUTION]
    >Wenn Sie die Startzeit auf einen Zeitpunkt in der Vergangenheit festlegen, wird die Bereitstellung sofort auslöst.
 
1. Überprüfen Sie das Konformitätsdiagramm. Dort sollte zu sehen sein, dass das Update jetzt ausgeführt wird.
 
1. Nach Abschluss des Updates wird das Konformitätsdiagramm mit dem neuen Status Ihres Updates aktualisiert.
 
1. Wählen Sie oben auf der Seite **Geräteupdates** die Registerkarte **Bereitstellungen** aus.
 
1. Wählen Sie Ihre Bereitstellung aus, um die Bereitstellungsdetails anzuzeigen. Klicken Sie ggf. auf **Aktualisieren**, bis sich der **Status** in **Erfolgreich** ändert.

## <a name="next-steps"></a>Nächste Schritte

Ihr Development Kit wurde erfolgreich aktualisiert. Sie können die Entwicklung und den Betrieb mit Ihrem Development Kit fortsetzen.