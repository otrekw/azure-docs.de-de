---
title: Einrichten der Diagnose
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie die Protokollierung mit Diagnoseeinstellungen aktivieren.
author: baanders
ms.author: baanders
ms.date: 7/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 60ee2102a2b725c68e29afec9a44de03e52e9467
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87379591"
---
# <a name="enable-logging-with-diagnostics-settings"></a>Aktivieren der Protokollierung mit Diagnoseeinstellungen

Sie können wählen, ob Sie Ihre Metrikdaten an [Log Analytics](../azure-monitor/log-query/get-started-portal.md), einen [Event Hubs](../event-hubs/event-hubs-about.md)-Endpunkt oder [Azure Storage](../storage/blobs/storage-blobs-overview.md) senden möchten, indem Sie die Protokollierung mit Diagnoseeinstellungen für Ihre Instanz festlegen.

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Aktivieren der Diagnoseeinstellungen über das Azure-Portal

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen in der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü die Option **Diagnoseeinstellungen** und dann **Diagnoseeinstellung hinzufügen** aus.

    :::image type="content" source="media/how-to-view-metrics/diagnostic-settings.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

3. Geben Sie auf der daraufhin geöffneten Seite die folgenden Werte ein:
 * **Name der Diagnoseeinstellung**: Benennen Sie die Diagnoseeinstellungen.
 * **Kategoriedetails**: Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Kontrollkästchen für die Diagnose dieser Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:
    - DigitalTwinsOperation
    - EventRoutesOperation
    - ModelsOperation
    - QueryOperation
    - AllMetrics
 * **Zieldetails**: Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:
    - An Log Analytics senden
    - In einem Speicherkonto archivieren
    - An einen Event Hub streamen

    Möglicherweise werden Sie aufgefordert, weitere Details einzugeben, wenn diese für Ihr ausgewähltes Ziel erforderlich sind.  
    
4. Speichern Sie die neuen Einstellungen. 

    :::image type="content" source="media/how-to-set-up-diagnostics/diagnostic-settings-details.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

Neue Einstellungen werden in etwa zehn Minuten wirksam. Anschließend werden die Protokolle im konfigurierten Ziel auf der Seite **Diagnoseeinstellungen** für Ihre Instanz angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Konfigurieren von Diagnosen finden Sie unter [*Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen*](../azure-monitor/platform/platform-logs-overview.md).
* Informationen zu den Azure Digital Twins-Metriken finden Sie unter [*Vorgehensweise: Anzeigen von Metriken mit Azure Monitor*](how-to-view-metrics.md)