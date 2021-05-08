---
title: Einrichten einer Gen2-Umgebung über das Azure-Portal – Azure Time Series Insights Gen2 | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Umgebung in Azure Time Series Insights Gen2 über das Azure-Portal einrichten.
author: riserrad
ms.author: riserrad
manager: edett
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: how-to
ms.date: 03/15/2021
ms.custom: seodec18
ms.openlocfilehash: e10685b1b64e8e3cd636245625c13df12f177fbb
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107951822"
---
# <a name="create-an-azure-time-series-insights-gen2-environment-using-the-azure-portal"></a>Erstellen einer Azure Time Series Insights Gen2-Umgebung im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie eine Azure Time Series Insights Gen2-Umgebung über das [Azure-Portal](https://portal.azure.com/) erstellen.

Das Tutorial zur Umgebungsbereitstellung führt Sie durch diesen Vorgang. Sie erfahren, wie Sie die richtige Zeitreihen-ID auswählen und Beispiele aus zwei JSON-Nutzlasten anzeigen.</br>

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWzk3P]

## <a name="overview"></a>Übersicht

Wenn Sie eine Azure Time Series Insights Gen2-Umgebung bereitstellen, erstellen Sie diese Azure-Ressourcen:

* Eine Azure Time Series Insights Gen2-Umgebung, die das nutzungsbasierte Bezahlmodell verwendet
* Ein Azure-Speicherkonto
* Einen optionalen warmen Speicher für schnellere und unbegrenzte Abfragen

> [!TIP]
>
> * Erfahren Sie, [wie Sie Ihre Umgebung planen](./how-to-plan-your-environment.md).
> * Hier finden Sie weitere Informationen zum [Hinzufügen einer Event Hub-Quelle](./how-to-ingest-data-event-hub.md) und zum [Hinzufügen einer IoT Hub-Quelle](./how-to-ingest-data-iot-hub.md).

Sie lernen Folgendes:

1. Ordnen Sie jede Azure Time Series Insights Gen2-Umgebung einer Ereignisquelle zu. Außerdem geben Sie eine Timestamp-ID-Eigenschaft und eine eindeutige Consumergruppe an, um sicherzustellen, dass die Umgebung auf die entsprechenden Ereignisse zugreifen kann.

1. Nach Abschluss der Bereitstellung können Sie Ihre Zugriffsrichtlinien und andere Umgebungsattribute gemäß Ihren geschäftlichen Anforderungen ändern.

   > [!NOTE]
   > Der erste Schritt ist bei der Bereitstellung einer Umgebung optional. Wenn Sie diesen Schritt überspringen, müssen Sie der Umgebung später eine Ereignisquelle anfügen, damit der Datenfluss in Ihre Umgebung beginnen und mittels Abfragen auf diese Daten zugegriffen werden kann.

## <a name="create-the-environment"></a>Erstellen der Umgebung

So erstellen Sie eine Azure Time Series Insights Gen2-Umgebung

1. Erstellen Sie eine Azure Time Series Insights-Ressource unter *Internet der Dinge* im [Azure-Portal](https://portal.azure.com/).

1. Wählen Sie **Gen2(L1)** als **Ebene** aus. Geben Sie einen Umgebungsnamen an, und wählen Sie die zu verwendende Abonnementgruppe und Ressourcengruppe aus. Wählen Sie dann einen unterstützten Ort zum Hosten der Umgebung aus.

   :::image type="content" source="media/how-to-create-environment-using-portal/environment-configuration.png" alt-text="Erstellen Sie eine Azure Time Series Insights-Instanz." lightbox="media/how-to-create-environment-using-portal/environment-configuration.png":::

1. Geben Sie eine Time Series-ID ein.

   :::image type="content" source="media/how-to-create-environment-using-portal/environment-configuration-2.png" alt-text="Erstellen einer Azure Time Series Insights-Umgebungskonfiguration (Fortsetzung)." lightbox="media/how-to-create-environment-using-portal/environment-configuration-2.png":::

   > [!NOTE]
   >
   > * Bei der Time Series-ID muss die *Groß-/Kleinschreibung* beachtet werden. Sie ist zudem *unveränderlich*. (Nach dem Festlegen kann sie nicht mehr geändert werden.)
   > * Time Series-IDs können aus bis zu *drei* Schlüsseln bestehen. Stellen Sie sich dies als einen Primärschlüssel in einer Datenbank vor, der eindeutig jeden Gerätesensor darstellt, der Daten an Ihre Umgebung senden würde. Dabei könnte es sich um eine Eigenschaft oder eine Kombination aus bis zu drei Eigenschaften handeln.
   > * Hier finden Sie weitere Informationen zum [Auswählen einer Time Series-ID](./how-to-select-tsid.md)

1. Erstellen Sie ein Azure Storage-Konto, indem Sie einen Speicherkontonamen und Kontotyp auswählen und eine Auswahl für die [Replikation](../storage/common/redundancy-migration.md?tabs=portal) treffen. Hierdurch wird automatisch ein Azure Storage-Konto erstellt. Standardmäßig wird ein Konto vom Typ [Universell v2](../storage/common/storage-account-overview.md) erstellt. Das Konto wird in derselben Region wie die Azure Time Series Insights Gen2-Umgebung erstellt, die Sie zuvor ausgewählt haben.
Alternativ können Sie auch Ihren eigenen Speicher (BYOS) über eine [Azure Resource Manager-Vorlage](./time-series-insights-manage-resources-using-azure-resource-manager-template.md) verwenden, wenn Sie eine neue Azure Time Series Gen2-Umgebung erstellen.

1. **(Optional)** Aktivieren Sie den warmen Speicher für Ihre Umgebung, wenn Sie schnellere und unbegrenzte Abfragen über die neuesten Daten in Ihrer Umgebung wünschen. Sie können auch einen warmen Speicher über die Option **Speicherkonfiguration** im linken Navigationsbereich erstellen oder löschen, nachdem Sie eine Azure Time Series Insights Gen2-Umgebung erstellt haben.

1. **(Optional)** Sie können jetzt eine Ereignisquelle hinzufügen. Sie können aber auch warten, bis die Instanz bereitgestellt wurde.

   * Azure Time Series Insights unterstützt [Azure IoT Hub](./how-to-ingest-data-iot-hub.md) und [Azure Event Hubs](./how-to-ingest-data-event-hub.md) als Ereignisquellenoptionen. Zwar können Sie bei der Erstellung der Umgebung nur eine einzelne Ereignisquelle hinzufügen, doch können Sie später eine weitere Ereignisquelle hinzufügen.

     Sie können eine vorhandene Consumergruppe auswählen oder eine neue Consumergruppe erstellen, wenn Sie die Ereignisquelle hinzufügen. Stellen Sie sicher, dass die Ereignisquelle immer eine eindeutige Consumergruppe verwendet, damit Ihre Umgebung Daten in diese einlesen kann.

   * Wählen Sie aus, wann mit dem Sammeln von Daten aus der Ereignisquelle begonnen werden soll. Die Standardeinstellung sieht eine Erfassung ab dem Zeitpunkt vor, zu dem die Ereignisquelle erstellt wird.

     > [!TIP]
     > Wenn Sie die Option **Alle meine Daten** auswählen, um bereits vorhandene Daten aus Ihrer Ereignisquelle zu sammeln, kann es anfänglich zu einer hohen Latenz kommen, weil die Azure Time Series Insights Gen2-Umgebung alle Daten verarbeitet. Die Latenz sollte mit zunehmender Indizierung der Daten abnehmen.

   * Wählen Sie die entsprechende Timestamp-Eigenschaft aus. Standardmäßig verwendet Azure Time Series Insights die in der Warteschlange verbrachte Zeit einer Nachricht für jede Ereignisquelle.

     > [!TIP]
     > Die in der Warteschlange verbrachte Zeit einer Nachricht ist möglicherweise nicht die beste konfigurierte Einstellung, um sie in Batchereignisszenarien oder Uploadszenarien mit Verlaufsdaten zu verwenden. Stellen Sie in solchen Fällen sicher, dass Sie Ihre Entscheidung überprüfen, ob eine „Timestamp“-Eigenschaft verwendet werden soll oder nicht.

   :::image type="content" source="media/how-to-create-environment-using-portal/configure-event-source.png" alt-text="Registerkarte „Event Source configuration“" lightbox="media/how-to-create-environment-using-portal/configure-event-source.png"::: (Ereignisquellen-Konfiguration)

1. Wählen Sie **Überprüfen und erstellen** aus, um sich zu vergewissern, dass Ihre Umgebung in der gewünschten Weise bereitgestellt und konfiguriert wurde.

    :::image type="content" source="media/how-to-create-environment-using-portal/environment-confirmation.png" alt-text="Registerkarte „Überprüfen und erstellen“" lightbox="media/how-to-create-environment-using-portal/environment-confirmation.png":::

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über allgemein verfügbare Umgebungen sowie Gen2-Umgebungen von Azure Time Series Insights, indem Sie [Planen Ihrer Umgebung](./how-to-plan-your-environment.md) lesen.
* Erfahren Sie mehr über [Ereignisquellen der Streamingerfassung](./concepts-streaming-ingestion-event-sources.md) für Ihre Azure Time Series Insights Gen2-Umgebung.
* Erfahren Sie mehr über das [Verwalten Ihrer Umgebung](./how-to-provision-manage.md).
