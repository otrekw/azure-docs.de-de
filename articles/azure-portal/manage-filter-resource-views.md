---
title: Anzeigen und Filtern von Ressourceninformationen in Azure
description: In diesem Artikel erfahren Sie, wie Sie Informationen filtern und verschiedene Ansichten verwenden, um Ihre Azure-Ressourcen besser zu verstehen.
ms.topic: how-to
ms.date: 03/16/2021
ms.openlocfilehash: bb48d0b0a7bf6017fbf407a95c33ef17729e34e3
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104771633"
---
# <a name="view-and-filter-azure-resource-information"></a>Anzeigen und Filtern von Ressourceninformationen in Azure

Mithilfe des Azure-Portals können Sie sich detaillierte Informationen zu den Ressourcen in Ihren Azure-Abonnements ansehen. In diesem Artikel erfahren Sie, wie Sie Informationen filtern und verschiedene Ansichten verwenden, um Ihre Ressourcen besser zu verstehen.

Der Fokus dieses Artikels liegt auf der Anzeige **Alle Ressourcen**, die Sie auf folgendem Screenshot sehen können. Auf Anzeigen für einzelne Ressourcentypen, z. B. für virtuelle Computer, gibt es andere Optionen, z. B. zum Starten und Stoppen einer VM.

:::image type="content" source="media/manage-filter-resource-views/all-resources.png" alt-text="Ansicht aller Ressourcen im Azure-Portal":::

## <a name="filter-resources"></a>Filtern von Ressourcen

Beginnen Sie damit, sich die Anzeige **Alle Ressourcen** näher anzusehen, indem Sie Filter verwenden, um den Fokus auf eine Teilmenge Ihrer Ressourcen zu legen. Auf dem folgenden Screenshot sehen Sie die Filterung für Ressourcengruppen. Dabei werden zwei der sechs Ressourcengruppen in einem Abonnement ausgewählt.

:::image type="content" source="media/manage-filter-resource-views/filter-resource-group.png" alt-text="Filteransicht basierend auf Ressourcengruppen":::

Sie können Filter kombinieren, einschließlich auf Textsuchen basierenden Filtern, wie Sie im folgenden Screenshot sehen können. In diesem Fall wird der Bereich der Ergebnisse auf Ressourcen festgelegt, die in einer der beiden bereits ausgewählten Ressourcengruppen „SimpleWinVM“ enthalten.

:::image type="content" source="media/manage-filter-resource-views/filter-simplewinvm.png" alt-text="Filteransicht basierend auf Texteingabe":::

Wenn Sie ändern möchten, welche Spalten in einer Ansicht eingeschlossen werden, klicken Sie auf **Ansicht verwalten** und dann auf **Spalten bearbeiten**.

:::image type="content" source="media/manage-filter-resource-views/edit-columns.png" alt-text="Bearbeiten der in einer Ansicht angezeigten Spalten":::

## <a name="save-use-and-delete-views"></a>Speichern, Verwenden und Löschen von Ansichten

Sie können Ansichten speichern, die von Ihnen ausgewählte Filter und Spalten enthalten. Speichern und Verwenden einer Ansicht:

1. Klicken Sie auf **Ansicht verwalten** und dann auf **Ansicht speichern**.

1. Geben Sie einen Namen für die Ansicht ein, und klicken Sie dann auf **OK**. Die gespeicherte Ansicht wird nun im Menü **Ansicht verwalten** angezeigt.

    :::image type="content" source="media/manage-filter-resource-views/simple-view.png" alt-text="Gespeicherte Ansicht":::

1. Zum Verwenden einer Ansicht können Sie zwischen der **Standardeinstellung** und einer Ihrer Ansichten wechseln, um zu sehen, wie dies die Liste der angezeigten Ressourcen beeinflusst.

Löschen einer Ansicht:

1. Klicken Sie auf **Ansicht verwalten** und dann auf **Alle Ansichten durchsuchen**.

1. Wählen Sie im Bereich **Gespeicherte Ansichten** die Ansicht und dann das Symbol zum **Löschen** ![Symbol „Ansicht löschen“](media/manage-filter-resource-views/icon-delete.png) aus.

## <a name="export-information-from-a-view"></a>Exportieren von Informationen aus einer Ansicht

Sie können die Ressourceninformationen aus einer Ansicht exportieren. So exportieren Sie Informationen im CSV-Format:

1. Wählen Sie **In CSV exportieren** aus.

    :::image type="content" source="media/manage-filter-resource-views/export-csv.png" alt-text="Screenshot vom Exportvorgang in das CSV-Format":::

1. Speichern Sie die Datei lokal, und öffnen Sie sie dann in Excel oder einer anderen Anwendung, die das CSV-Format unterstützt. 

Wenn Sie das Portal verschieben, sehen Sie weitere Bereiche, in denen Sie Informationen exportieren können, z. B. eine einzelne Ressourcengruppe.

## <a name="summarize-resources-with-visuals"></a>Zusammenfassen von Ressourcen mithilfe von Visuals

Die Ansichten, die Sie sich bisher angesehen haben, waren _Listenansichten_. Es gibt jedoch auch _Zusammenfassungsansichten_, zu denen Visuals gehören. Sie können diese Ansichten genauso wie Listenansichten speichern und verwenden. Filter bleiben für diese beiden Ansichtsarten bestehen. Es gibt sowohl Standardansichten, wie die unten gezeigte Ansicht **Standort**, als auch Ansichten, die für bestimmte Dienste relevant sind, wie die Ansicht **Status** für Azure Storage.

:::image type="content" source="media/manage-filter-resource-views/summary-map.png" alt-text="Zusammenfassung der Ressourcen in einer Kartenansicht":::

Speichern und Verwenden einer Zusammenfassungsansicht:

1. Klicken Sie im Menü „Ansicht“ auf die Option **Zusammenfassungsansicht**.

    :::image type="content" source="media/manage-filter-resource-views/menu-summary-view.png" alt-text="Menü „Zusammenfassungsansicht“":::

1. Die Zusammenfassungsansicht ermöglicht Ihnen Zusammenfassungen nach verschiedenen Attributen, z. B. **Standort** und **Typ**. Wählen Sie eine **Zusammenfassen nach**-Option sowie ein geeignetes Visual aus. Der folgende Screenshot zeigt die **Zusammenfassung nach Typ** mit einem **Balkendiagramm**-Visual.

    :::image type="content" source="media/manage-filter-resource-views/type-summary-bar-chart.png" alt-text="Zusammenfassung nach Typ in Form eines Balkendiagramms":::

1. Klicken Sie auf **Ansicht verwalten** und dann auf **Speichern**, um diese Ansicht zu speichern, wie Sie es auch mit der Listenansicht gemacht haben.

1. Wählen Sie in der Zusammenfassungsansicht unter **Zusammenfassung nach Typ** einen Balken des Diagramms aus. Wenn Sie den Balken auswählen, wird eine Liste angezeigt, die nach einem Typ der Ressource gefiltert ist.

    :::image type="content" source="media/manage-filter-resource-views/all-resources-filtered-type.png" alt-text="Alle Ressourcen gefiltert nach Typ":::

## <a name="run-queries-in-azure-resource-graph"></a>Ausführen von Abfragen in Azure Resource Graph

Azure Resource Graph bietet effiziente und leistungsstarke Möglichkeiten der Ressourcenuntersuchung und ermöglicht es, mehrere Abonnements bedarfsgerecht abzufragen. Auf der Anzeige **Alle Ressourcen** im Azure-Portal findet sich ein Link, mit dem eine Resource Graph-Abfrage geöffnet werden kann, deren Bereich auf die aktuelle gefilterte Ansicht festgelegt ist.

Ausführen einer Resource Graph-Abfrage:

1. Klicken Sie auf **Abfrage öffnen**.

    :::image type="content" source="media/manage-filter-resource-views/open-query.png" alt-text="Öffnen der Azure Resource Graph-Abfrage":::

1. Klicken Sie im **Azure Resource Graph-Explorer** auf **Abfrage ausführen**, um die Ergebnisse anzuzeigen.

    :::image type="content" source="media/manage-filter-resource-views/run-query.png" alt-text="Ausführen der Azure Resource Graph-Abfrage":::

    Weitere Informationen finden Sie unter [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md).

## <a name="next-steps"></a>Nächste Schritte

[Azure-Portal – Übersicht](azure-portal-overview.md)

[Erstellen und Freigeben von Dashboards im Azure-Portal](azure-portal-dashboards.md).
