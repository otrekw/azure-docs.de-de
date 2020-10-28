---
title: Exportieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschau) über die Diagnoseeinstellungen
description: In diesem Artikel wird erläuter, wie Sie Metriken von Azure IoT-Connector für FHIR (Vorschau) über die Diagnoseeinstellungen exportieren.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 10/16/2020
ms.author: jasteppe
ms.openlocfilehash: c81dcdd2e79f5d89a0766415b47ad118874e5ad2
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209113"
---
# <a name="export-azure-iot-connector-for-fhir-preview-metrics-through-diagnostic-settings"></a>Exportieren der Metriken von Azure IoT-Konnektor für FHIR (Vorschau) über die Diagnoseeinstellungen

In diesem Artikel erfahren Sie, wie Sie Metrikprotokolle von Azure IoT-Connector für FHIR* exportieren. Die Funktion, die die Metrikprotokollierung ermöglicht, sind die [**Diagnoseeinstellungen**](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings) im Azure-Portal. 

> [!TIP]
> Befolgen Sie die Anweisungen in [Aktivieren der Diagnoseprotokollierung in Azure-API for FHIR und Azure IoT-Connector für FHIR](enable-diagnostic-logging.md#enable-diagnostic-logging-in-azure-api-for-fhir), um die Überwachungsprotokollierung einzurichten.

## <a name="enable-metric-logging-for-the-azure-iot-connector-for-fhir-preview"></a>Aktivieren der Metrikprotokollierung für den Azure IoT-Connector für FHIR (Vorschau)
1. Um die Metrikprotokollierung für den Azure IoT-Connector für FHIR zu aktivieren, wählen Sie Ihren Azure API for FHIR-Dienst im Azure-Portal aus. 

2. Navigieren zu den **Diagnoseeinstellungen** 

3. Wählen Sie **+ Diagnoseeinstellung hinzufügen**  aus.

   :::image type="content" source="media/iot-metrics-export/diagnostic-settings-main.png" alt-text="IoT-Konnektor1" lightbox="media/iot-metrics-export/diagnostic-settings-main.png"::: 

4. Geben Sie einen Namen in das Dialogfeld **Name der Diagnoseeinstellung** ein.

5. Wählen Sie die Methode aus, die Sie für den Zugriff auf Ihre Diagnoseprotokolle verwenden möchten:

    1. **Archivieren Sie Protokolle zur (manuellen) Überprüfung in einem Speicherkonto** . Das Speicherkonto, das Sie verwenden möchten, muss bereits erstellt worden sein.
    2. **Streamen Sie die Protokolle zu Event Hub** , damit sie von einem Dienst eines Drittanbieters oder einer benutzerdefinierten Analyselösung erfasst werden können. Sie müssen einen Event Hub-Namespace und eine Event Hub-Richtlinie erstellen, ehe Sie diesen Schritt konfigurieren können.
    3. **Streamen Sie die Protokolle zum Log Analytics-Arbeitsbereich** in Azure Monitor. Sie müssen zuvor Ihren Log Analytics-Arbeitsbereich erstellen, bevor Sie diese Option auswählen können.

6. Wählen Sie **Fehler, Datenverkehr und Latenz** für den Azure IoT-Connector für FHIR und alle zusätzlichen Metrikkategorien aus, die Sie für die Azure API for FHIR erfassen möchten.

7. Wählen Sie **Speichern** aus.

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="IoT-Konnektor1" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

> [!Note] 
> Es kann bis zu 15 Minuten dauern, bis die ersten Metrikprotokolle im Repository Ihrer Wahl angezeigt werden.  
 
Weitere Informationen zum Arbeiten mit Diagnoseprotokollen finden Sie in der [Dokumentation zum Azure-Ressourcenprotokoll](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview).

## <a name="conclusion"></a>Zusammenfassung 
Zugriff auf Metrikprotokolle zu haben, ist für die Überwachung und Problembehandlung von entscheidender Bedeutung.  Azure IoT-Connector für FHIR ermöglicht Ihnen, diese Aufgaben mithilfe von Metrikprotokollen durchzuführen. 

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie häufig gestellte Fragen zu Azure IoT-Konnektor für FHIR.

>[!div class="nextstepaction"]
>[Azure IoT-Konnektor für FHIR – Häufig gestellte Fragen](fhir-faq.md#azure-iot-connector-for-fhir-preview)

*Im Azure-Portal wird Azure IoT-Konnektor für FHIR als IoT-Konnektor (Vorschauversion) bezeichnet.

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.
