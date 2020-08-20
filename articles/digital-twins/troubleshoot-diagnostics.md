---
title: Einrichten der Diagnose
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie die Protokollierung mit Diagnoseeinstellungen aktivieren.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: d47bb7cc868c5733c6e36290f097fec783764cd3
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88003580"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Problembehandlung von Azure Digital Twins: Diagnoseprotokollierung

Azure Digital Twins sammelt [Metriken](troubleshoot-metrics.md) für Ihre Dienstinstanz, die Informationen zum Zustand Ihrer Ressourcen bereitstellen. Mit diesen Metriken können Sie die allgemeine Integrität des Azure Digital Twins-Diensts und der damit verbundenen Ressourcen bewerten. Anhand dieser benutzerorientierten Statistiken können Sie Vorgänge in Azure Digital Twins erkennen und die Ursache von Problemen analysieren, ohne sich an den Azure-Support wenden zu müssen.

In diesem Artikel erfahren Sie, wie Sie die **Diagnoseprotokollierung** für Ihre Metrikdaten von Ihrer Azure Digital Twins-Instanz aus aktivieren. Sie können diese Protokolle verwenden, um Probleme mit dem Dienst zu beheben, und Diagnoseeinstellungen konfigurieren, um Metriken von Azure Digital Twins an verschiedene Ziele zu senden. Weitere Informationen zu diesen Einstellungen finden Sie unter [*Erstellen von Diagnoseeinstellungen zum Senden von Plattformprotokollen und Metriken an verschiedene Ziele*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Aktivieren der Diagnoseeinstellungen über das Azure-Portal

Im Folgenden wird beschrieben, wie Sie Diagnoseeinstellungen für Ihre Azure Digital Twins-Instanz aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer Azure Digital Twins-Instanz. Sie finden sie, indem Sie ihren Namen auf der Suchleiste des Portals eingeben. 

2. Wählen Sie im Menü die Option **Diagnoseeinstellungen** und dann **Diagnoseeinstellung hinzufügen** aus.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

3. Geben Sie auf der daraufhin geöffneten Seite die folgenden Werte ein:
     * **Name der Diagnoseeinstellung:** Benennen Sie die Diagnoseeinstellungen.
     * **Kategoriedetails:** Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Kontrollkästchen für die Diagnose dieser Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
        
        Weitere Informationen zu diesen Optionen finden Sie im Abschnitt [*Kategoriedetails*](#category-details) weiter unten.
     * **Zieldetails:** Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:
        - An Log Analytics senden
        - In einem Speicherkonto archivieren
        - An einen Event Hub streamen

        Möglicherweise werden Sie aufgefordert, weitere Details einzugeben, wenn diese für Ihr ausgewähltes Ziel erforderlich sind.  
    
4. Speichern Sie die neuen Einstellungen. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Screenshot der Seite „Diagnoseeinstellungen“ und der Schaltfläche zum Hinzufügen":::

Neue Einstellungen werden in etwa zehn Minuten wirksam. Anschließend werden die Protokolle im konfigurierten Ziel auf der Seite **Diagnoseeinstellungen** für Ihre Instanz angezeigt. 

## <a name="category-details"></a>Kategoriedetails

Hier finden Sie weitere Informationen zu den Protokollkategorien, die beim Einrichten der Diagnoseeinstellungen unter **Kategoriedetails** ausgewählt werden können.

| Protokollkategorie | BESCHREIBUNG |
| --- | --- |
| ADTModelsOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Modellen. |
| ADTQueryOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Abfragen. |
| ADTEventRoutesOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Ereignisrouten sowie ausgehende Ereignisse von Azure Digital Twins an einen Endpunktdienst wie Event Grid, Event Hubs und Service Bus. |
| ADTDigitalTwinsOperation | Protokolliert alle API-Aufrufe im Zusammenhang mit Azure Digital Twins. |

Jede Protokollkategorie besteht aus Schreib-, Lese-, Lösch- und Aktionsvorgängen.  Diese lassen sich REST-API-Aufrufen wie folgt zuordnen:

| Ereignistyp | REST-API-Vorgänge |
| --- | --- |
| Schreiben | PUT und PATCH |
| Lesen | GET |
| Löschen | Delete |
| Aktion | POST |

Im Folgenden finden Sie eine umfassende Liste der Vorgänge und entsprechenden [REST-API-Aufrufe für Azure Digital Twins](https://docs.microsoft.com/rest/api/azure-digitaltwins/), die in jeder Kategorie protokolliert werden. 

>[!NOTE]
> Jede Protokollkategorie enthält mehrere Vorgänge/REST-API-Aufrufe. In der folgenden Tabelle ist jede Protokollkategorie allen darunter aufgeführten Vorgänge/REST-API-Aufrufen zugeordnet, anschließend folgt die nächste Kategorie. 

| Protokollkategorie | Vorgang | REST-API-Aufrufe und andere Ereignisse |
| --- | --- | --- |
| ADTModelsOperation | Microsoft.DigitalTwins/models/write | API zum Aktualisieren von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/read | API zum Abrufen nach ID und Auflisten von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/delete | API zum Löschen von Digital Twin-Modellen |
|  | Microsoft.DigitalTwins/models/action | API zum Hinzufügen von Digital Twin-Modellen |
| ADTQueryOperation | Microsoft.DigitalTwins/query/action | API zum Abfragen von Digital Twin-Instanzen |
| ADTEventRoutesOperation | Microsoft.DigitalTwins/eventroutes/write | API zum Hinzufügen von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/read | API zum Abrufen nach ID und Auflisten von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/delete | API zum Löschen von Ereignisrouten |
|  | Microsoft.DigitalTwins/eventroutes/action | Ausgehende Daten eines Ereignisses an einen Endpunktdienst (kein API-Aufruf) |
| ADTDigitalTwinsOperation | Microsoft.DigitalTwins/digitaltwins/write | Vorgänge zum Hinzufügen, Hinzufügen von Beziehungen, Aktualisieren und Aktualisieren von Komponenten von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/read | Vorgänge zum Abrufen nach ID, Abrufen von Komponenten, Abrufen der Beziehung nach ID, Auflisten eingehender Beziehungen und Auflisten von Beziehungen von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/delete | Vorgänge zum Löschen und Löschen von Beziehungen von Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/action | Senden von Komponententelemetrie und Senden von Telemetrie von Digital Twins |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Konfigurieren von Diagnosen finden Sie unter [*Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen*](../azure-monitor/platform/platform-logs-overview.md).
* Informationen zu den Azure Digital Twins-Metriken finden Sie unter [*Problembehandlung: Anzeigen von Metriken mit Azure Monitor*](troubleshoot-metrics.md).
* Informationen zur Aktivierung von Warnungen für Ihre Metriken finden Sie unter [*Problembehandlung: Einrichten von Warnungen*](troubleshoot-alerts.md).
