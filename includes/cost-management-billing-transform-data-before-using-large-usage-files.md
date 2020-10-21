---
title: Includedatei
description: Includedatei
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: include
ms.date: 10/09/2020
ms.author: banders
ms.reviwer: ''
ms.custom: include file
ms.openlocfilehash: fb34b4ef1cf209a15c7e6ac9a59cc11dd4b0056e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026475"
---
## <a name="transform-data-before-using-large-usage-files"></a>Transformieren von Daten vor der Verwendung von großen Nutzungsdateien

Es kann vorkommen, dass Ihre Nutzungs- oder Abstimmungsdatei zu groß zum Öffnen ist. Es kann auch sein, dass Sie nur einen Teil der Informationen benötigen, um ein Problem zu beheben. Beispielsweise benötigen Sie nur Informationen zu einer bestimmten Ressource oder nur die Verbrauchsdaten für einige Dienste oder Ressourcengruppen. Sie können die Daten transformieren, um sie vor dem Erstellen von PivotTables zusammenzufassen.

1. Öffnen Sie in Excel eine leere Arbeitsmappe.
1. Wählen Sie im oberen Menü die Option **Daten** > **Aus Text/CSV**, Ihre Nutzungsdatei und dann **Importieren** aus.
1. Wählen Sie am unteren Rand des Fensters die Option **Daten transformieren** aus. In einem neuen Fenster wird eine Zusammenfassung der Daten angezeigt.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" alt-text="Beispiel: Zusammengefasste Daten" lightbox="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" :::
1. Gehen Sie wie folgt vor, falls Sie über eine Microsoft-Kundenvereinbarung (MCA) verfügen: Überspringen Sie diesen Schritt, und fahren Sie mit dem nächsten Schritt fort, weil die Spaltentitel in MCA-Nutzungsdateien normalerweise in den ersten Zeilen enthalten sind. Bereiten Sie die Daten auf, indem Sie die Tabelle erstellen. Entfernen Sie die obersten Zeilen, und behalten Sie nur die Titel bei. Wählen Sie **Zeilen entfernen** > **Erste Zeilen entfernen** aus.  
     :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/remove-top-rows.png" alt-text="Beispiel: Zusammengefasste Daten" :::
1. Geben Sie im Fenster „Erste Zeilen entfernen“ die Anzahl von Zeilen ein, die oben entfernt werden sollen. Für EA ist dies normalerweise „2“ und für CSP „1“. Klicken Sie auf **OK**.
1. Wählen Sie **Erste Zeile als Überschriften verwenden** aus.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/use-first-row-as-header.png" alt-text="Beispiel: Zusammengefasste Daten" :::
    
    In der Tabellenansicht werden oben die Spaltentitel angezeigt.
1. Fügen Sie als Nächstes einen Filter hinzu. Verwenden Sie jeweils den Auswahlpfeil rechts neben einem Spaltentitel, um zu filtern. Die vorgeschlagenen Filter sind „Abonnement-ID“, „Dienstname (Kategorie der Verbrauchseinheit)“, „Instanz-ID“ und „Ressourcengruppe“. Sie können in einem Dokument mehrere Filter verwenden. Wir empfehlen Ihnen, alle zutreffenden Filter anzuwenden, um die Dokumentgröße zu reduzieren und die späteren Arbeitsschritte zu vereinfachen.
1. Wählen Sie nach dem Anwenden Ihrer Filter die Option **Schließen und laden** aus.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/close-and-load.png" alt-text="Beispiel: Zusammengefasste Daten" :::

Die Datei wird geladen, und es wird eine Tabelle mit gefilterten Nutzungsdaten angezeigt. Sie können nun eine neue PivotTable zum Behandeln von Nutzungsproblemen erstellen.