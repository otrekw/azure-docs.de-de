---
title: Markieren einer Liste ausgewählter Ressourcen mit mehreren Glossarbegriffen
description: Erfahren Sie mehr über die Massenbearbeitung von Ressourcen in Azure Purview.
author: nayenama
ms.author: nayenama
ms.service: data-catalog
ms.subservice: data-catalog-gen2
ms.topic: conceptual
ms.date: 11/24/2020
ms.openlocfilehash: c42a6894c33993dc9aee5a9fdd10b1c3a3627320
ms.sourcegitcommit: 287c20509c4cf21d20eea4619bbef0746a5cd46e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97372032"
---
# <a name="how-to-bulk-edit-assets-to-tag-glossary-terms"></a>Massenbearbeitung von Ressourcen zum Taggen mit Glossarbegriffen

In diesem Artikel wird beschrieben, wie Sie eine Liste ausgewählter Objekte in einer einzigen Aktion mit mehreren Glossarbegriffen markieren.

### <a name="add-assets-to-view-selected-list-using-search"></a>Hinzufügen von Ressourcen zur Liste „Ausgewählte anzeigen“ mithilfe der Suche

1. Suchen Sie nach der Datenressource, die Sie der Liste für die Massenbearbeitung hinzufügen möchten.

2. Zeigen Sie auf der Suchergebnisseite auf die Ressource, die Sie der Liste **Ausgewählte anzeigen** für die Massenbearbeitung hinzufügen möchten, um ein Kontrollkästchen anzuzeigen.

   :::image type="content" source="media/how-to-bulk-edit-assets/asset-checkbox.png" alt-text="Screenshot des Kontrollkästchens":::

3. Aktivieren Sie das Kontrollkästchen, um es der Liste **Ausgewählte anzeigen** für die Massenbearbeitung hinzuzufügen. Nach dem Hinzufügen wird unten auf der Seite das Symbol „Ausgewählte Elemente“ angezeigt.

   :::image type="content" source="media/how-to-bulk-edit-assets/selected-list.png" alt-text="Screenshot der Liste":::

4. Wiederholen Sie die Schritte oben, um der Liste alle Datenressourcen hinzuzufügen.

### <a name="add-assets-to-view-selected-list-from-asset-detail-page"></a>Hinzufügen von Ressourcen zur Liste „Ausgewählte anzeigen“ von der Detailseite dieser Ressourcen

1. Aktivieren Sie auf der Detailseite der Ressource das Kontrollkästchen in der rechten oberen Ecke, um die Ressource der Liste **Ausgewählte anzeigen** für die Massenbearbeitung hinzuzufügen.

   :::image type="content" source="media/how-to-bulk-edit-assets/asset-list.png" alt-text="Screenshot der Ressource":::

### <a name="bulk-edit-assets-in-the-view-selected-list-to-add-replace-or-remove-glossary-terms"></a>Massenbearbeitung von Ressourcen in der Liste „Ausgewählte anzeigen“ zum Hinzufügen, Ersetzen oder Entfernen von Glossarbegriffen

1. Nachdem Sie alle Datenressourcen für die Massenbearbeitung ermittelt haben, wählen Sie auf der Suchergebnisseite oder der Detailseite der Ressourcen die Liste **Ausgewählte anzeigen** aus.

:::image type="content" source="media/how-to-bulk-edit-assets/view-list.png" alt-text="Screenshot der Ansicht":::

2. Überprüfen Sie die Liste, und wählen Sie **Entfernen** aus, wenn Sie Begriffe entfernen möchten.

:::image type="content" source="media/how-to-bulk-edit-assets/remove-list.png" alt-text="Screenshot des Entfernens":::

3. Wählen Sie den gewünschten Massenbearbeitungsvorgang aus, um Glossarbegriffe für alle ausgewählten Ressourcen hinzuzufügen, zu entfernen oder zu ersetzen.

4. Zum Hinzufügen von Glossarbegriffen wählen Sie als Vorgang **Hinzufügen** aus. Wählen Sie alle Glossarbegriffe aus, die Sie als neuen Wert hinzufügen möchten. Wählen Sie „Anwenden“ aus, wenn Sie fertig sind.
    - Durch den Vorgang „Hinzufügen“ wird ein neuer Wert an die Liste der Glossarbegriffe angefügt, mit denen die Datenressourcen bereits gekennzeichnet sind.  
   
    :::image type="content" source="media/how-to-bulk-edit-assets/add-list.png" alt-text="Screenshot des Hinzufügens":::

5. Wenn Sie Glossarbegriffe ersetzen möchten, wählen Sie den Vorgang **Ersetzen durch** aus. Wählen Sie alle Glossarbegriffe aus, die Sie durch einen neuen Wert ersetzen möchten. Wählen Sie „Anwenden“ aus, wenn Sie fertig sind.
    - Durch den Ersetzungsvorgang werden alle Glossarbegriffe für die ausgewählte Datenressourcen durch die in „Neuer Wert“ angegebenen Begriffe ersetzt.
   
6. Zum Entfernen von Glossarbegriffen wählen Sie als Vorgang **Entfernen** aus. Wählen Sie „Anwenden“ aus, wenn Sie fertig sind.
    - Durch das Entfernen werden alle Glossarbegriffe aus den ausgewählten Datenressourcen entfernt.
   
    :::image type="content" source="media/how-to-bulk-edit-assets/replace-list.png" alt-text="Screenshot des Entfernens von Begriffen":::

7. Schließen Sie nach Abschluss des Vorgangs das Blatt für die Massenbearbeitung, indem Sie **Schließen** oder **Remove all and close** (Alle entfernen und schließen) auswählen. Durch „Schließen“ werden die ausgewählten Ressourcen nicht entfernt, mit „Remove all and close“ (Alle entfernen und schließen) hingegen schon.
    :::image type="content" source="media/how-to-bulk-edit-assets/close-list.png" alt-text="Screenshot des Schließens":::

   > [!Important]
   > Die empfohlene Anzahl von Ressourcen für die Massenbearbeitung beträgt 15. Wenn Sie mehr als 15 auswählen, kann dies die Leistung beeinträchtigen.
   > Das Feld **Ausgewählte anzeigen** wird nur angezeigt, wenn mindestens eine Ressource ausgewählt ist.


[Tutorial: Erstellen und Importieren von Glossarbegriffen in Azure Purview (Vorschau)](how-to-create-import-export-glossary.md). Hier finden Sie weitere Informationen.
