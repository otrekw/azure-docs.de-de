---
title: Integration von Arbeitselementen (Vorschau) – Application Insights
description: Erfahren Sie, wie Sie Arbeitselemente in GitHub oder Azure DevOps mit darin eingebetteten Application Insights-Daten erstellen.
ms.topic: conceptual
ms.date: 02/9/2021
ms.openlocfilehash: ba0a67bad3ba47191414d6b406ab6cb4e6b7da78
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731917"
---
# <a name="work-item-integration-preview"></a>Integration von Arbeitselementen (Vorschau)

Die Funktion zur Integration von Arbeitselementen ermöglicht Ihnen das einfache Erstellen von Arbeitselementen in GitHub oder Azure DevOps, in die relevante Application Insights-Daten eingebettet sind.

> [!IMPORTANT]
> Die Integration von Arbeitselementen befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-and-configure-a-work-item-template"></a>Erstellen und Konfigurieren einer Arbeitselementvorlage

1. Zum Erstellen einer Arbeitselementvorlage navigieren Sie zu Ihrer Application Insights-Ressource, und wählen Sie auf der linken Seite unter *Konfigurieren* die Option **Arbeitselemente** und dann oben die Option **Neue Vorlage erstellen** aus.

    :::image type="content" source="./media/work-item-integration/create-work-item-template.png" alt-text=" Screenshot der Registerkarte für Arbeitselemente mit ausgewählter Option zum Erstellen einer neuen Vorlage" lightbox="./media/work-item-integration/create-work-item-template.png":::

    Sie können eine Arbeitselementvorlage auch über die Registerkarte mit End-to-End-Transaktionsdetails erstellen, falls derzeit keine Vorlage vorhanden ist. Wählen Sie ein Ereignis und dann auf der rechten Seite **Arbeitselement erstellen** aus. Anschließend wählen Sie **Mit Arbeitsmappenvorlage beginnen** aus.

    :::image type="content" source="./media/work-item-integration/create-template-from-transaction-details.png" alt-text=" Screenshot der Registerkarte mit End-to-End-Transaktionsdetails mit ausgewählten Optionen zum Erstellen eines Arbeitselements und zum Beginnen mit einer Arbeitsmappenvorlage" lightbox="./media/work-item-integration/create-template-from-transaction-details.png":::

2. Nachdem Sie **Neue Vorlage erstellen** ausgewählt haben, können Sie die Nachverfolgungssysteme auswählen, der Arbeitsmappe einen Namen geben, eine Verknüpfung mit dem ausgewählten Nachverfolgungssystem erstellen und eine Region zum Speichern der Verlage auswählen (dies ist standardmäßig die Region, in der sich Ihre Application Insights Ressource befindet). Die URL-Parameter sind die Standard-URL für Ihr Repository, z. B. `https://github.com/myusername/reponame` oder `https://mydevops.visualstudio.com/myproject`.

    :::image type="content" source="./media/work-item-integration/create-workbook.png" alt-text=" Screenshot zum Erstellen einer neuen Arbeitsmappenvorlage für Arbeitselemente":::

## <a name="create-a-work-item"></a>Erstellen eines Arbeitselements

 Sie können auf die neue Vorlage über alle End-to-End-Transaktionsdetails zugreifen, auf die Sie über „Leistung“, „Fehler“, „Verfügbarkeit“ oder andere Registerkarten zugreifen können.

1. Zum Erstellen eines Arbeitselements navigieren Sie zu den End-to-End-Transaktionsdetails, und wählen Sie ein Ereignis aus. Wählen Sie dann **Arbeitselement erstellen** und anschließend Ihre Arbeitselementvorlage aus.

    :::image type="content" source="./media/work-item-integration/create-work-item.png" alt-text=" Screenshot der Registerkarte mit End-to-End-Transaktionsdetails mit ausgewählter Option zum Erstellen eines Arbeitselements" lightbox="./media/work-item-integration/create-work-item.png":::

1. Im Browser wird eine neue Registerkarte für das von Ihnen ausgewählte Nachverfolgungssystem geöffnet. In Azure DevOps können Sie einen Fehler oder eine Aufgabe erstellen, und in GitHub können Sie ein neues Problem in Ihrem Repository erstellen. Es wird automatisch ein neues Arbeitselement mit Kontextinformationen erstellt, die von Application Insights bereitgestellt werden.

    :::image type="content" source="./media/work-item-integration/github-work-item.png" alt-text=" Screenshot eines automatisch erstellten GitHub-Problems" lightbox="./media/work-item-integration/github-work-item.png":::

    :::image type="content" source="./media/work-item-integration/azure-devops-work-item.png" alt-text=" Screenshot eines automatisch erstellten Fehlers in Azure DevOps" lightbox="./media/work-item-integration/azure-devops-work-item.png":::

## <a name="edit-a-template"></a>Bearbeiten einer Vorlage

Zum Bearbeiten der Vorlage navigieren Sie unter *Konfigurieren* zur Registerkarte **Arbeitselemente**, und wählen Sie das Stiftsymbol neben der Arbeitsmappe aus, die Sie aktualisieren möchten.

:::image type="content" source="./media/work-item-integration/edit-template.png" alt-text=" Screenshot der Registerkarte für Arbeitselemente mit ausgewähltem Stiftsymbol für die Bearbeitung":::

Wählen Sie oben das Bearbeitungssymbol ![Bearbeitungssymbol](./media/work-item-integration/edit-icon.png) aus, um mit dem Bearbeiten der Vorlage zu beginnen. Arbeitselementvorlagen basieren auf [Azure Monitor-Arbeitsmappen](../visualize/workbooks-overview.md). Die Arbeitselementinformationen werden mithilfe der Sprache KQL (Keyword Query Language) generiert. Sie können die Abfragen ändern, um weiteren wichtigen Kontext für Ihr Team hinzuzufügen. Wenn Sie die Bearbeitung abgeschlossen haben, speichern Sie die Arbeitsmappe, indem Sie auf der oberen Symbolleiste das Symbol zum Speichern ![Symbol zum Speichern](./media/work-item-integration/save-icon.png) auswählen.

:::image type="content" source="./media/work-item-integration/edit-workbook.png" alt-text=" Screenshot der Arbeitsmappe mit der Arbeitselementvorlage im Bearbeitungsmodus" lightbox="./media/work-item-integration/edit-workbook.png":::

Sie können mehrere Arbeitselementkonfigurationen erstellen und für jedes Szenario über eine benutzerdefinierte Arbeitsmappe verfügen. Die Arbeitsmappen können auch von Azure Resource Manager bereitgestellt werden, um Standardimplementierungen in allen Umgebungen zu gewährleisten.