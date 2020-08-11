---
title: Einrichten der Diagnose
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie die Protokollierung mit Diagnoseeinstellungen aktivieren.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 402eda92d5cc8e5c2566345d31c9ae625b82695c
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87809396"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Problembehandlung von Azure Digital Twins: Diagnoseprotokollierung

Azure Digital Twins sammelt [Metriken](troubleshoot-metrics.md) für Ihre Dienstinstanz, die Informationen zum Zustand Ihrer Ressourcen bereitstellen. Mit diesen Metriken können Sie die allgemeine Integrität des Azure Digital Twins-Diensts und der damit verbundenen Ressourcen bewerten. Anhand dieser benutzerorientierten Statistiken können Sie Vorgänge in Azure Digital Twins erkennen und die Ursache von Problemen analysieren, ohne sich an den Azure-Support wenden zu müssen.

In diesem Artikel erfahren Sie, wie Sie die **Diagnoseprotokollierung** für Ihre Metrikdaten von Ihrer Azure Digital Twins-Instanz aus aktivieren. Sie können diese Protokolle verwenden, um Dienstprobleme zu beheben.

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Aktivieren der Diagnoseeinstellungen über das Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen auf der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü die Option **Diagnoseeinstellungen** und dann **Diagnoseeinstellung hinzufügen** aus.

    :::image type="content" source="media/troubleshoot-metrics/diagnostic-settings.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

3. Geben Sie auf der daraufhin geöffneten Seite die folgenden Werte ein:
     * **Name der Diagnoseeinstellung:** Benennen Sie die Diagnoseeinstellungen.
     * **Kategoriedetails:** Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Kontrollkästchen für die Diagnose dieser Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
     * **Zieldetails:** Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:
        - An Log Analytics senden
        - In einem Speicherkonto archivieren
        - An einen Event Hub streamen

        Möglicherweise werden Sie aufgefordert, weitere Details einzugeben, wenn diese für Ihr ausgewähltes Ziel erforderlich sind.  
    
4. Speichern Sie die neuen Einstellungen. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

Neue Einstellungen werden in etwa zehn Minuten wirksam. Anschließend werden die Protokolle im konfigurierten Ziel auf der Seite **Diagnoseeinstellungen** für Ihre Instanz angezeigt. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Konfigurieren von Diagnosen finden Sie unter [*Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen*](../azure-monitor/platform/platform-logs-overview.md).
* Informationen zu den Azure Digital Twins-Metriken finden Sie unter [*Problembehandlung: Anzeigen von Metriken mit Azure Monitor*](troubleshoot-metrics.md).
