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
ms.date: 10/23/2019
ms.custom: seodec18
ms.openlocfilehash: f66925c20f6767286eb98a7f5fab180845da4c33
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014800"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Bereitstellen und Verwalten der Vorschauversion von Azure Time Series Insights

In diesem Artikel wird beschrieben, wie Sie eine Azure Time Series Insights-Umgebung (Vorschauversion) mit dem [Azure-Portal](https://portal.azure.com/) erstellen.

## <a name="overview"></a>Übersicht

Azure Time Series Insights-Umgebungen (Vorschauversion) sind Umgebungen mit nutzungsbasierter Bezahlung.

Wenn Sie eine Azure Time Series Insights Preview-Umgebung bereitstellen, erstellen Sie diese Azure-Ressourcen:

* Eine Azure Time Series Insights Preview-Umgebung  
* Ein Azure Storage-Konto vom Typ „Allgemein v1“
* Einen optionalen warmen Speicher für schnellere und unbegrenzte Abfragen
  
Erfahren Sie, [wie Sie Ihre Umgebung planen](./time-series-insights-update-plan.md).

Ordnen Sie jede Azure Time Series Insights Preview-Umgebung einer Ereignisquelle zu. Weitere Informationen finden Sie unter [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md) und [Hinzufügen einer IoT Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md). Sie geben in diesem Schritt eine „Timestamp ID“-Eigenschaft sowie eine eindeutige Consumergruppe an. Auf diese Weise wird sichergestellt, dass die Umgebung Zugriff auf die entsprechenden Ereignisse hat.

> [!NOTE]
> Der vorherige Schritt ist im Bereitstellungsworkflow optional, während Sie die Time Series Preview-Umgebung erstellen. Sie müssen der Umgebung jedoch eine Ereignisquelle anfügen, damit die Daten in diese Umgebung fließen können.

Nach Abschluss der Bereitstellung können Sie Ihre Zugriffsrichtlinien und andere Umgebungsattribute gemäß Ihren geschäftlichen Anforderungen ändern.

## <a name="create-the-environment"></a>Erstellen der Umgebung

So erstellen Sie eine Azure Time Series Insights Preview-Umgebung

1. Wählen Sie im Menü **SKU** die Schaltfläche **PAYG** aus. Geben Sie einen Umgebungsnamen an, und wählen Sie die zu verwendende Abonnementgruppe und Ressourcengruppe aus. Wählen Sie dann einen unterstützten Ort zum Hosten der Umgebung aus.

   [![Erstellen Sie eine Azure Time Series Insights-Instanz.](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

1. Geben Sie eine Time Series-ID ein.

    >[!NOTE]
    > * Bei der Time Series-ID muss die Groß-/Kleinschreibung beachtet werden. Sie ist zudem unveränderlich. (Nach dem Festlegen kann sie nicht mehr geändert werden.)
    > * Time Series-IDs können aus bis zu drei Schlüsseln bestehen.
    > * Weitere Informationen zum Auswählen einer Time Series-ID finden Sie unter [Auswählen einer Time Series-ID](./time-series-insights-update-how-to-id.md).

1. Erstellen Sie ein Azure Storage-Konto, indem Sie einen Namen für das Konto auswählen und eine Replikationsauswahl treffen. Hierdurch wird automatisch ein Azure Storage-Konto vom Typ „Allgemein v1“ erstellt. Das Konto wird in derselben Region wie die Azure Time Series Insights Preview-Umgebung erstellt, die Sie zuvor ausgewählt haben.

    [![Erstellen eines Azure Storage-Kontos für Ihre Instanz](media/v2-update-manage/manage-five.png)](media/v2-update-manage/manage-five.png#lightbox)

1. **(Optional)** Aktivieren Sie den warmen Speicher für Ihre Umgebung, wenn Sie schnellere und unbegrenzte Abfragen über die neuesten Daten in Ihrer Umgebung wünschen. Sie können auch einen warmen Speicher über die Option **Speicherkonfiguration** im linken Navigationsbereich erstellen oder löschen, nachdem Sie eine Time Series Insights Preview-Umgebung erstellt haben.

1. **(Optional)** Sie können jetzt eine Ereignisquelle hinzufügen. Alternativ können Sie warten, bis die Instanz bereitgestellt wurde.

   * Time Series Insights unterstützt [Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) und [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) als Ereignisquellenoptionen. Zwar können Sie bei der Erstellung der Umgebung nur eine einzelne Ereignisquelle hinzufügen, doch können Sie später eine weitere Ereignisquelle hinzufügen. Sie können eine vorhandene Consumergruppe auswählen oder eine neue Consumergruppe erstellen, wenn Sie die Ereignisquelle hinzufügen. Es wird empfohlen, eine eindeutige Consumergruppe zu erstellen, um sicherzustellen, dass alle Ereignisse für Ihre Azure Time Series Insights Preview-Umgebung sichtbar sind.

   * Wählen Sie die entsprechende Timestamp-Eigenschaft aus. Standardmäßig verwendet Azure Time Series Insights die in der Warteschlange verbrachte Zeit einer Nachricht für jede Ereignisquelle.

     > [!TIP]
     > Die in der Warteschlange verbrachte Zeit einer Nachricht ist möglicherweise nicht die beste konfigurierte Einstellung, um sie in Batchereignisszenarien oder Uploadszenarien mit Verlaufsdaten zu verwenden. Stellen Sie in solchen Fällen sicher, dass Sie Ihre Entscheidung überprüfen, ob eine „Timestamp“-Eigenschaft verwendet werden soll oder nicht.

     [![Registerkarte „Ereignisquelle“](media/v2-update-manage/manage-two.png)](media/v2-update-manage/manage-two.png#lightbox)

1. Vergewissern Sie sich, dass Ihre Umgebung mit den gewünschten Einstellungen bereitgestellt wurde.

    [![Registerkarte „Bewerten + erstellen“](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

## <a name="manage-the-environment"></a>Verwalten der Umgebung

Sie können Ihre Azure Time Series Insights-Umgebung (Vorschauversion) über das Azure-Portal verwalten. Wenn Sie über das Azure-Portal verwalten, sehen Sie einige wichtige Unterschiede zwischen einer Azure Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung und den allgemein verfügbaren S1- oder S2-Umgebungen:

* Das Blatt **Übersicht** im Azure-Portal ist in Azure Time Series Insights unverändert, mit Ausnahme der folgenden Unterschiede:
  * Die Kapazität wird entfernt, da sie nicht für Umgebungen mit nutzungsbasierter Bezahlung gilt.
  * Die Eigenschaft „Time Series-ID“ wird hinzugefügt. Sie bestimmt, wie Ihre Daten partitioniert werden.
  * Verweisdatasets wurden entfernt.
  * Die angezeigte URL leitet Sie zum [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) weiter.
  * Der Name Ihres Azure Storage-Kontos wird aufgeführt.

* Das Blatt **Konfigurieren** im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, da Umgebungen mit nutzungsbasierter Bezahlung nicht konfigurierbar sind. Sie können jedoch **Speicherkonfiguration** verwenden, um den neu eingeführten warmen Speicher zu konfigurieren.

* Das Blatt **Verweisdaten** im Azure-Portal wurde in Azure Time Series Insights Preview entfernt, da Verweisdaten keine Komponente von Umgebungen mit nutzungsbasierter Bezahlung sind.

[![Time Series Insights-Umgebung (Vorschauversion) im Azure-Portal](media/v2-update-manage/manage-four.png)](media/v2-update-manage/manage-four.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über allgemein verfügbare Umgebungen sowie Preview-Umgebungen von Time Series Insights, indem Sie [Planen Ihrer Umgebung](./time-series-insights-update-plan.md) lesen.

- Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Konfigurieren [einer IoT-Hub-Quelle](./time-series-insights-how-to-add-an-event-source-iothub.md).
