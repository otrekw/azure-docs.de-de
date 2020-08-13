---
title: Visualisieren der Azure IoT Central-Daten in einem Power BI-Dashboard | Microsoft-Dokumentation
description: Verwenden Sie die Power BI-Lösung für Azure IoT Central zum Visualisieren und Analysieren Ihrer IoT Central-Daten.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/4/2019
ms.topic: conceptual
ms.openlocfilehash: f996bb2d5126ef038ca872aee1f1893979a5229b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "87080997"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualisieren und Analysieren der Azure IoT Central-Daten in einem Power BI-Dashboard

*Dieses Thema richtet sich an Administratoren und Lösungsentwickler.*

:::image type="content" source="media/howto-connect-powerbi/iot-continuous-data-export.png" alt-text="Power BI-Lösungspipeline":::

Verwenden Sie die Power BI-Lösung für Azure IoT Central V3, um ein leistungsfähiges Power BI-Dashboard zur Überwachung der Leistung Ihrer IoT-Geräte zu erstellen. In Ihrem Power BI-Dashboard können Sie folgende Aktionen ausführen:

- Nachverfolgen, wie viele Daten im Laufe der Zeit von Ihren Geräten gesendet werden
- Vergleichen von Datenvolumes zwischen verschiedenen Telemetriedatenströmen
- Filtern nach Daten, die von bestimmten Geräten gesendet wurden
- Anzeigen der neuesten Telemetriedaten in einer Tabelle

Durch diese Lösung wird eine Pipeline eingerichtet, mit der Daten aus Ihrem Azure Blob Storage-Konto mit [kontinuierlichem Datenexport](howto-export-data-blob-storage.md) gelesen werden. Die Daten werden von der Pipeline unter Verwendung von Azure Functions, Azure Data Factory und Azure SQL-Datenbank verarbeitet und transformiert. Sie können die Daten in einem als PBIX-Datei herunterladbaren Power BI-Bericht visualisieren und analysieren. Die Ressourcen werden jeweils in Ihrem Azure-Abonnement erstellt, sodass Sie die einzelnen Komponenten gemäß Ihren Anforderungen anpassen können.

## <a name="prerequisites"></a>Voraussetzungen

Um die in dieser Anleitung aufgeführten Schritte ausführen zu können, benötigen Sie ein aktives Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Zum Einrichten der Lösung benötigen Sie Folgendes:

- Eine IoT Central-Anwendung der Version 3. Informationen zum Überprüfen Ihrer Anwendungsversion finden Sie unter [Über Ihre Anwendung](./howto-get-app-info.md). Informationen zum Erstellen einer IoT Central--Anwendung finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md).
- Kontinuierlicher Datenexport, der zum Exportieren von Telemetriedaten, Geräten und Gerätevorlagen in Azure Blob Storage konfiguriert ist. Weitere Informationen finden Sie unter [Exportieren von IoT-Daten zu Zielen in Azure](howto-export-data.md).
  - Vergewissern Sie sich, dass nur Ihre IoT Central-Anwendung Daten in den Blobcontainer exportiert.
  - Ihre [Geräte müssen JSON-codierte Nachrichten senden](../../iot-hub/iot-hub-devguide-messages-d2c.md). Von Geräten müssen in den Nachrichtensystemeigenschaften `contentType:application/JSON` und `contentEncoding:utf-8` oder `contentEncoding:utf-16` oder `contentEncoding:utf-32` angegeben werden.
- Power BI Desktop (neueste Version). Siehe [Power BI-Tools und -Apps herunterladen](https://powerbi.microsoft.com/downloads/).
- Power BI Pro (falls Sie das Dashboard für andere Benutzer freigeben möchten).

> [!NOTE]
> Sollten Sie eine IoT Central-Anwendung der Version 2 verwenden, lesen Sie [Visualisieren und Analysieren der Azure IoT Central-Daten in einem Power BI-Dashboard](https://docs.microsoft.com/previous-versions/azure/iot-central/core/howto-connect-powerbi) auf der Dokumentationsseite für ältere Versionen.

## <a name="install"></a>Installieren

Navigieren Sie zum Einrichten der Pipeline zur Seite [Power BI-Lösung für Azure IoT Central V3](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) von **Microsoft AppSource**. Wählen Sie **Jetzt holen** aus, und befolgen Sie die Anweisungen.

Lesen und befolgen Sie beim Öffnen der PBIX-Datei die Anweisungen auf der Startseite. Dort wird beschrieben, wie Sie Ihren Bericht mit Ihrer SQL-Datenbank verknüpfen.

## <a name="report"></a>Bericht

Die PBIX-Datei enthält den Bericht **Devices and Telemetry** (Geräte und Telemetriedaten) mit einer Verlaufsansicht der von Geräten gesendeten Telemetriedaten. Sie bietet eine Aufschlüsselung der verschiedenen Arten von Telemetriedaten und zeigt die neuesten Telemetriedaten, die von Geräten gesendet wurden.

:::image type="content" source="media/howto-connect-powerbi/report.png" alt-text="Power BI-Bericht für Geräte und Telemetriedaten":::

## <a name="pipeline-resources"></a>Pipelineressourcen

Über das Azure-Portal kann auf alle Azure-Ressourcen zugegriffen werden, aus denen sich die Pipeline zusammensetzt. Alle Ressourcen befinden sich in der Ressourcengruppe, die Sie beim Einrichten der Pipeline erstellt haben.

:::image type="content" source="media/howto-connect-powerbi/azure-deployment.png" alt-text="Ressourcengruppenansicht im Azure-Portal":::

In der folgenden Liste werden die Rollen der jeweiligen Ressourcen in der Pipeline beschrieben:

### <a name="azure-functions"></a>Azure-Funktionen

Die Azure-Funktions-App wird jedes Mal ausgelöst, wenn von IoT Central eine neue Datei in den Blobspeicher geschrieben wird. Durch die Funktionen werden Daten aus den Blobs für Telemetriedaten, Geräte und Gerätevorlagen extrahiert, um die von Azure Data Factory verwendeten SQL-Zwischentabellen aufzufüllen.

### <a name="azure-data-factory"></a>Azure Data Factory

Von Azure Data Factory wird eine Verbindung mit SQL-Datenbank als verknüpfter Dienst hergestellt. Die Daten werden mithilfe von gespeicherten Prozeduren verarbeitet und in den Analysetabellen gespeichert.

Azure Data Factory wird alle 15 Minuten ausgeführt, um den aktuellen Datenbatch zu transformieren, damit er in die SQL-Tabellen geladen werden kann. (Hierbei handelt es sich um den derzeit kleinstmöglichen Wert für den Trigger vom Typ **Rollierendes Fenster**.)

### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Von Azure Data Factory wird eine Gruppe von Analysetabellen für Power BI generiert. Diese Schemas können in Power BI erkundet und zur Erstellung eigener Visualisierungen verwendet werden.

## <a name="estimated-costs"></a>Geschätzte Kosten

Auf der Seite [Power BI-Lösung für Azure IoT Central V3](https://appsource.microsoft.com/product/web-apps/iot-central.power-bi-solution-iot-central) von Microsoft AppSource befindet sich ein Kostenschätzungslink für die Ressourcen, die Sie bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie Ihre Daten in Power BI visualisieren, können Sie sich als Nächstes mit der [Verwaltung von Geräten](howto-manage-devices.md) vertraut machen.
