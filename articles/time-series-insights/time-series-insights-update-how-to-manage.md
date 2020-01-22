---
title: Bereitstellen und Verwalten einer Previewumgebung – Azure Time Series | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure Time Series Insights Preview-Umgebung bereitstellen und verwalten.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/19/2019
ms.custom: seodec18
ms.openlocfilehash: b70604c62ae21f9c433b3cd7d9e59f4ccebb61bd
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861726"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Bereitstellen und Verwalten der Vorschauversion von Azure Time Series Insights

In diesem Artikel wird beschrieben, wie Sie eine Azure Time Series Insights-Umgebung (Vorschauversion) mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

## <a name="overview"></a>Übersicht

Azure Time Series Insights-Umgebungen (Vorschauversion) sind Umgebungen mit *nutzungsbasierter* Bezahlung.

Wenn Sie eine Azure Time Series Insights Preview-Umgebung bereitstellen, erstellen Sie diese Azure-Ressourcen:

* Eine Azure Time Series Insights Preview-Umgebung  
* Ein Azure Storage-Konto vom Typ „Allgemein v1“
* Einen optionalen warmen Speicher für schnellere und unbegrenzte Abfragen

> [!TIP]
> * Erfahren Sie, [wie Sie Ihre Umgebung planen](./time-series-insights-update-plan.md).
> * Hier finden Sie weitere Informationen zum [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md) und zum [Hinzufügen einer IoT Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md).

Sie lernen Folgendes:

1. **(Optional)** Ordnen Sie jede Azure Time Series Insights Preview-Umgebung einer Ereignisquelle zu. Außerdem geben Sie eine Timestamp-ID-Eigenschaft und eine eindeutige Consumergruppe an, um sicherzustellen, dass die Umgebung auf die entsprechenden Ereignisse zugreifen kann.

   > [!NOTE]
   > Der vorherige Schritt ist bei der Bereitstellung einer Umgebung optional. Wenn Sie diesen Schritt überspringen, müssen Sie der Umgebung später eine Ereignisquelle anfügen, damit auf die Daten in der Umgebung zugegriffen werden kann.

1. Nach Abschluss der Bereitstellung können Sie Ihre Zugriffsrichtlinien und andere Umgebungsattribute gemäß Ihren geschäftlichen Anforderungen ändern.

## <a name="create-the-environment"></a>Erstellen der Umgebung

So erstellen Sie eine Azure Time Series Insights Preview-Umgebung

1. Wählen Sie **Nutzungsbasierte Zahlung** als **Dienstebene** aus. Geben Sie einen Umgebungsnamen an, und wählen Sie die zu verwendende Abonnementgruppe und Ressourcengruppe aus. Wählen Sie dann einen unterstützten Ort zum Hosten der Umgebung aus.

   [![Erstellen Sie eine Azure Time Series Insights-Instanz.](media/v2-update-manage/create-and-manage-configuration.png)](media/v2-update-manage/create-and-manage-configuration.png#lightbox)

1. Geben Sie eine Time Series-ID ein.

    > [!NOTE]
    > * Bei der Time Series-ID muss die *Groß-/Kleinschreibung* beachtet werden. Sie ist zudem *unveränderlich*. (Nach dem Festlegen kann sie nicht mehr geändert werden.)
    > * Time Series-IDs können aus bis zu *drei* Schlüsseln bestehen.
    > * Hier finden Sie weitere Informationen zum [Auswählen einer Time Series-ID](time-series-insights-update-how-to-id.md)

1. Erstellen Sie ein Azure-Speicherkonto, indem Sie einen Namen für das Konto auswählen und eine Replikationsauswahl treffen. Hierdurch wird automatisch ein Azure Storage-Konto vom Typ „Allgemein v1“ erstellt. Das Konto wird in derselben Region wie die Azure Time Series Insights Preview-Umgebung erstellt, die Sie zuvor ausgewählt haben.

    [![Cold Storage-Konfiguration](media/v2-update-manage/create-and-manage-cold-store.png)](media/v2-update-manage/create-and-manage-cold-store.png#lightbox)

1. **(Optional)** Aktivieren Sie den warmen Speicher für Ihre Umgebung, wenn Sie schnellere und unbegrenzte Abfragen über die neuesten Daten in Ihrer Umgebung wünschen. Sie können auch einen warmen Speicher über die Option **Speicherkonfiguration** im linken Navigationsbereich erstellen oder löschen, nachdem Sie eine Time Series Insights Preview-Umgebung erstellt haben.

    [![Warm Storage-Konfiguration](media/v2-update-manage/create-and-manage-warm-storage.png)](media/v2-update-manage/create-and-manage-warm-storage.png#lightbox)

1. **(Optional)** Sie können jetzt eine Ereignisquelle hinzufügen. Sie können aber auch warten, bis die Instanz bereitgestellt wurde.

   * Time Series Insights unterstützt [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) und [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) als Ereignisquellenoptionen. Zwar können Sie bei der Erstellung der Umgebung nur eine einzelne Ereignisquelle hinzufügen, doch können Sie später eine weitere Ereignisquelle hinzufügen. 
   
     Sie können eine vorhandene Consumergruppe auswählen oder eine neue Consumergruppe erstellen, wenn Sie die Ereignisquelle hinzufügen. Es wird empfohlen, eine eindeutige Consumergruppe zu erstellen, um sicherzustellen, dass alle Ereignisse für Ihre Azure Time Series Insights Preview-Umgebung sichtbar sind.

   * Wählen Sie die entsprechende Timestamp-Eigenschaft aus. Standardmäßig verwendet Azure Time Series Insights die in der Warteschlange verbrachte Zeit einer Nachricht für jede Ereignisquelle.

     > [!TIP]
     > Die in der Warteschlange verbrachte Zeit einer Nachricht ist möglicherweise nicht die beste konfigurierte Einstellung, um sie in Batchereignisszenarien oder Uploadszenarien mit Verlaufsdaten zu verwenden. Stellen Sie in solchen Fällen sicher, dass Sie Ihre Entscheidung überprüfen, ob eine „Timestamp“-Eigenschaft verwendet werden soll oder nicht.

     [![Registerkarte „Event Source configuration“](media/v2-update-manage/create-and-manage-event-source.png)](media/v2-update-manage/create-and-manage-event-source.png#lightbox) (Ereignisquellen-Konfiguration)

1. Vergewissern Sie sich, dass Ihre Umgebung in der gewünschten Weise bereitgestellt und konfiguriert wurde.

    [![Registerkarte „Bewerten + erstellen“](media/v2-update-manage/create-and-manage-review-and-confirm.png)](media/v2-update-manage/create-and-manage-review-and-confirm.png#lightbox)

## <a name="manage-the-environment"></a>Verwalten der Umgebung

Sie können Ihre Azure Time Series Insights-Umgebung (Vorschauversion) über das Azure-Portal verwalten. Wenn Sie Ihre Umgebung über das Azure-Portal verwalten, müssen Sie einige wichtige Unterschiede zwischen einer Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung und den allgemein verfügbaren S1- oder S2-Umgebungen berücksichtigen:

* Das Blatt **Übersicht** der Vorschau im Azure-Portal weist die folgenden Änderungen auf:

  * Die Kapazität wird entfernt, da sie auf Umgebungen mit nutzungsbasierter Bezahlung nicht zutrifft.
  * Die Eigenschaft **Time Series-ID** wird hinzugefügt. Sie bestimmt, wie Ihre Daten partitioniert werden.
  * Verweisdatasets wurden entfernt.
  * Die angezeigte URL leitet Sie zum [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) weiter.
  * Der Name Ihres Azure-Speicherkontos wird aufgeführt.

* Das Blatt **Konfigurieren** im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, da Umgebungen mit nutzungsbasierter Bezahlung nicht konfigurierbar sind. Sie können jedoch **Speicherkonfiguration** verwenden, um den neu eingeführten warmen Speicher zu konfigurieren.

* Das Blatt **Verweisdaten** im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, da Verweisdaten keine Komponente von Umgebungen mit nutzungsbasierter Bezahlung sind.

[![Time Series Insights-Umgebung (Vorschauversion) im Azure-Portal](media/v2-update-manage/create-and-manage-overview-confirm.png)](media/v2-update-manage/create-and-manage-overview-confirm.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über allgemein verfügbare Umgebungen sowie Preview-Umgebungen von Time Series Insights, indem Sie [Planen Ihrer Umgebung](./time-series-insights-update-plan.md) lesen.

- Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Konfigurieren [einer IoT-Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md).
