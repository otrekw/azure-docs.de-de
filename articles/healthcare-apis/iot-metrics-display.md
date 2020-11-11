---
title: Anzeigen und Konfigurieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschauversion)
description: Dieser Artikel enthält Informationen zum Anzeigen und Konfigurieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschauversion).
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/29/2020
ms.author: jasteppe
ms.openlocfilehash: 76166fc5c525d36474a585179472e93b22dad647
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133398"
---
# <a name="view-and-configure-azure-iot-connector-for-fhir-preview-metrics"></a>Anzeigen und Konfigurieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschauversion) 

In diesem Artikel erfahren Sie, wie Sie Metriken von Azure IoT-Konnektor für FHIR* anzeigen und konfigurieren. 

> [!TIP]
> Informationen zum Einrichten des Exports von Metrikdaten finden Sie unter [Exportieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschau) über die Diagnoseeinstellungen](https://docs.microsoft.com/azure/healthcare-apis/iot-metrics-diagnostics-export).

## <a name="view-metrics-for-azure-iot-connector-for-fhir-preview"></a>Anzeigen von Metriken für Azure IoT-Konnektor für FHIR (Vorschauversion)
1. Wenn Sie Metriken für IoT-Konnektor-Instanzen anzeigen möchten, wählen Sie im Azure-Portal Ihren Azure API for FHIR-Dienst aus. 

2. Navigieren Sie zu **Metriken**. 

3. Wählen Sie die Registerkarte **IoT-Konnektor** aus.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-main.png" alt-text="IoT-Konnektor1" lightbox="media/iot-metrics-display/iot-metrics-main.png"::: 

4. Wählen Sie eine IoT-Konnektor-Instanz aus, um die zugehörigen Metriken anzuzeigen. (Diesem Azure API for FHIR-Dienst sind beispielsweise vier IoT-Konnektor-Instanzen zugeordnet.)

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-connector.png" alt-text="IoT-Konnektor2" lightbox="media/iot-metrics-display/iot-metrics-select-connector.png"::: 

> [!NOTE]
> Auf der Registerkarte **Benutzerdefiniert** können spezifische Uhrzeit-/Datumskombinationen zum Anzeigen von IoT-Konnektor-Metriken erstellt werden.

5. Wählen Sie den anzuzeigenden Zeitraum für IoT-Konnektor-Metriken aus (beispielsweise „1 Stunde“, „24 Stunden“, „7 Tage“ oder „Benutzerdefiniert“).

   :::image type="content" source="media/iot-metrics-display/iot-metrics-select-time.png" alt-text="IoT-Konnektor3" lightbox="media/iot-metrics-display/iot-metrics-select-time.png"::: 
 
## <a name="metrics-types-for-azure-iot-connector-for-fhir-preview"></a>Metriktypen für Azure IoT-Konnektor für FHIR (Vorschauversion) 
Folgende IoT-Konnektor-Metriken werden angezeigt:

|Metriktyp|Zweck der Metrik| 
|-----------|--------------|
|Anzahl eingehender Nachrichten|Die Anzahl unformatierter eingehender Nachrichten (etwa die Geräteereignisse).|
|Anzahl normalisierter Nachrichten|Die Anzahl normalisierter Nachrichten.|
|Anzahl von Nachrichtengruppen|Die Anzahl von Gruppen, in denen innerhalb des angegebenen Zeitfensters Nachrichten aggregiert wurden.|
|Durchschnittliche Wartezeit der Normalisierungsphase|Die durchschnittliche Wartezeit der Normalisierungsphase. In der Normalisierungsphase werden eingehende unformatierte Nachrichten normalisiert.|
|Durchschnittliche Wartezeit der Gruppierungsphase|Die durchschnittliche Wartezeit der Gruppierungsphase. In der Gruppierungsphase werden Puffer-, Aggregations- und Gruppierungsvorgänge für normalisierte Nachrichten durchgeführt.| 
|Gesamtfehlerzahl|Die Gesamtanzahl von Fehlern.| 

## <a name="focusing-and-configuring-azure-iot-connector-for-fhir-preview-metrics"></a>Fokussieren und Konfigurieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschauversion)
In diesem Beispiel steht Metriken für die **Anzahl eingehender Nachrichten** im Mittelpunkt.

1. Wählen Sie einen Zeitpunkt aus, auf den Sie sich konzentrieren möchten.

   :::image type="content" source="media/iot-metrics-display/iot-metrics-focus.png" alt-text="IoT-Konnektor4" lightbox="media/iot-metrics-display/iot-metrics-focus.png"::: 

2. Zur weiteren Anpassung stehen auf diesem Bildschirm folgende Optionen zur Verfügung: **Metrik hinzufügen** , **Filter hinzufügen** und **Teilung anwenden**. 

   :::image type="content" source="media/iot-metrics-display/iot-metrics-add-options.png" alt-text="IoT-Konnektor5" lightbox="media/iot-metrics-display/iot-metrics-add-options.png"::: 

## <a name="conclusion"></a>Zusammenfassung 
Der Zugriff auf Metriken der Datenebene ist für die Überwachung und Problembehandlung von entscheidender Bedeutung.  Azure IoT-Konnektor für FHIR unterstützt Sie bei diesen Aufgaben durch Metriken. 

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie häufig gestellte Fragen zu Azure IoT-Konnektor für FHIR.

>[!div class="nextstepaction"]
>[Azure IoT-Konnektor für FHIR – Häufig gestellte Fragen](fhir-faq.md)

*Im Azure-Portal wird Azure IoT-Konnektor für FHIR als IoT-Konnektor (Vorschauversion) bezeichnet.

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.