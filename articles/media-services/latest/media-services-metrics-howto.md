---
title: Anzeigen von Metriken mit Azure Monitor
description: In diesem Artikel wird das Überwachen von Metriken mit den Azure-Portal-Diagrammen und der Azure-Befehlszeilenschnittstelle beschrieben.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 09c306b85c910e8ec2021abf15013935c44b0df3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707709"
---
# <a name="monitor-media-services-metrics"></a>Überwachen von Media Services-Metriken

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[Azure Monitor](../../azure-monitor/overview.md) ermöglicht Ihnen die Überwachung von Metriken und Diagnoseprotokollen, die Ihnen zu verstehen helfen, wie sich Ihre Anwendungen verhalten. Eine ausführliche Beschreibung dieser Funktion und Argumente für die Verwendung der Azure Media Services-Metriken und -Diagnoseprotokolle finden Sie unter [Überwachen von Media Services-Metriken und -Diagnoseprotokollen](media-services-metrics-diagnostic-logs.md).

Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z. B. die Diagrammdarstellung im Portal, den Zugriff über die REST-API oder die Abfrage über die Azure CLI. In diesem Artikel wird das Überwachen von Metriken mit den Azure-Portal-Diagrammen und der Azure-Befehlszeilenschnittstelle beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen eines Media Services-Kontos](./create-account-howto.md)
- Siehe [Überwachen von Media Services-Metriken und -Diagnoseprotokollen](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>Anzeigen von Metriken im Azure-Portal

1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
1. Navigieren Sie zu Ihrem Azure Media Services-Konto, und wählen Sie **Metriken** aus.
1. Klicken Sie auf das Feld **RESSOURCE**, und wählen Sie die Ressource aus, deren Metriken Sie überwachen möchten.

    Auf der rechten Seite wird das Fenster **Ressource auswählen** mit der Liste der für Sie verfügbaren Ressourcen angezeigt. In diesem Fall wird Folgendes angezeigt:

    * &lt;Name des Media Services-Kontos&gt;
    * &lt;Name des Media Services-Kontos&gt;/&lt;Name des Streamingendpunkts&gt;
    * &lt;Speicherkontoname&gt;

    Wählen Sie die Ressource aus, und klicken Sie auf **Anwenden**. Weitere Informationen zu unterstützten Ressourcen und Metriken finden Sie unter [Überwachen von Media Services-Metriken](media-services-metrics-diagnostic-logs.md).

    ![Screenshot, der die ausgewählte Ressource und die hervorgehobene Schaltfläche „Übernehmen“ zeigt.](media/media-services-metrics/metrics02.png)

    > [!NOTE]
    > Um zwischen den Ressourcen zu wechseln, deren Metriken überwacht werden sollen, klicken Sie erneut auf das Feld **RESSOURCE**, und wiederholen Sie diesen Schritt.
1. (Optional) Benennen Sie das Diagramm (ändern Sie den Namen, indem Sie oben auf das Bleistiftsymbol klicken).
1. Fügen Sie Metriken hinzu, die Sie anzeigen möchten.

    ![Metriken](media/media-services-metrics/metrics03.png)
1. Sie können das Diagramm an das Dashboard anheften.

## <a name="view-metrics-with-azure-cli"></a>Anzeigen von Metriken mit der Azure-Befehlszeilenschnittstelle

Um mit der Azure CLI Metriken ausgehender Daten abzurufen, führen Sie den Befehl `az monitor metrics` wie folgt aus:

```azurecli-interactive
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Um andere Metriken abzurufen, ersetzen Sie „Egress“ durch den Namen der gewünschten Metrik.

## <a name="see-also"></a>Weitere Informationen

* [Azure Monitor-Metriken](../../azure-monitor/platform/data-platform.md)
* [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-metric.md)

## <a name="next-steps"></a>Nächste Schritte

[Diagnoseprotokolle](media-services-diagnostic-logs-howto.md)
