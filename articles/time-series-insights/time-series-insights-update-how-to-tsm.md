---
title: Datenmodellierung in Vorschauumgebungen – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Datenmodellierung in Azure Time Series Insights Preview.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/20/2019
ms.custom: seodec18
ms.openlocfilehash: 591233154ab79b9493d77030727462bae68ea7d5
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76152356"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Datenmodellierung in Azure Time Series Insights Preview

In diesem Artikel wird beschrieben, wie Sie mit dem Zeitreihenmodell in Insights Preview arbeiten. Es werden mehrere allgemeine Datenszenarien dargestellt.

> [!TIP]
> * Erfahren Sie mehr über das [Zeitreihenmodell](time-series-insights-update-tsm.md) (Vorschau).
> * Weitere Informationen zur Navigation auf der Vorschaubenutzeroberfläche finden Sie unter [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

## <a name="instances"></a>Instanzen

Der Azure Time Series Insights-Explorer unterstützt **ERSTELLEN**-, **LESEN**-, **AKTUALISIEREN**- und **LÖSCHEN**-Vorgänge für Instanzen im Browser. 

Wählen Sie zunächst die **Modell**-Ansicht aus der **Analyse**-Ansicht im Time Series Insights-Explorer aus.

### <a name="create-a-single-instance"></a>Erstellen einer einzelnen Instanz

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Instanzen** aus. Alle Instanzen, die der ausgewählten Time Series Insights-Umgebung zugeordnet sind, werden angezeigt.

    [![Erstellen Sie eine einzelne Instanz, indem Sie zuerst „Instanzen“ auswählen.](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png#lightbox)

1. Wählen Sie **+ Hinzufügen**.

    [![Fügen Sie durch Auswählen der Schaltfläche „+ Hinzufügen“ eine Instanz hinzu.](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png#lightbox)

1. Geben Sie die Instanzdetails ein, wählen Sie die Zuordnung von Typ und Hierarchie aus, und wählen Sie **Erstellen** aus.

### <a name="bulk-upload-one-or-more-instances"></a>Massenupload einer oder mehrerer Instanzen

> [!TIP]
> Sie können Ihre Instanzen auf dem Desktop in JSON speichern. Die heruntergeladene JSON-Datei kann anschließend mithilfe der folgenden Schritte hochgeladen werden.

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Instanzennutzlast enthält.

    [![Massenupload von Instanzen mithilfe von JSON.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png#lightbox)

1. Wählen Sie die Option **Hochladen**.

### <a name="edit-a-single-instance"></a>Bearbeiten einer einzelnen Instanz

1. Wählen Sie die Instanz aus, und wählen Sie das Symbol **Bearbeiten** oder das **Stiftsymbol** aus. 
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Bearbeiten einer einzelnen Instanz](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png#lightbox)

### <a name="delete-an-instance"></a>Löschen einer Instanz

1. Wählen Sie die Instanz aus, und wählen Sie das **Löschen**- oder das **Papierkorb**-Symbol aus.

   [![Löschen Sie eine Instanz, indem Sie „Löschen“ auswählen.](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png#lightbox)

1. Bestätigen Sie den Löschvorgang durch Auswahl von **Löschen**.

> [!NOTE]
> Eine Instanz, die gelöscht werden soll, muss erfolgreich eine Feldvalidierungsprüfung bestehen.

## <a name="hierarchies"></a>Hierarchien

Der Azure Time Series Insights-Explorer unterstützt **ERSTELLEN**-, **LESEN**-, **AKTUALISIEREN**- und **LÖSCHEN**-Vorgänge für Hierarchien im Browser. 

Wählen Sie zunächst die **Modell**-Ansicht aus der **Analyse**-Ansicht im Time Series Insights-Explorer aus.

### <a name="create-a-single-hierarchy"></a>Erstellen einer einzelnen Hierarchie

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Hierarchien** aus. Alle Hierarchien, die der ausgewählten Time Series Insights-Umgebung zugeordnet sind, werden angezeigt.

    [![Erstellen einer Hierarchie über den Bereich.](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png#lightbox)

1. Wählen Sie **+ Hinzufügen**.

    [![Schaltfläche „+ Hinzufügen“ für Hierarchien](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png#lightbox)

1. Wählen Sie im rechten Bereich **+ Ebene hinzufügen** aus.

    [![Fügen Sie der Hierarchie eine Ebene hinzu.](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png)](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png#lightbox)

1. Geben Sie die Hierarchiedetails ein, und wählen Sie **Speichern** aus.

    [![Geben Sie Hierarchiedetails an.](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png)](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Massenupload einer oder mehrerer Hierarchien

> [!TIP]
> Sie können Ihre Hierarchien auf dem Desktop in JSON speichern. Die heruntergeladene JSON-Datei kann anschließend mithilfe der folgenden Schritte hochgeladen werden.

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Hierarchienutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    [![Auswahl für den Massenupload von Hierarchien](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Bearbeiten einer einzelnen Hierarchie

1. Wählen Sie die Hierarchie aus, und wählen Sie das Symbol **Bearbeiten** oder das **Stiftsymbol** aus.
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Auswahl für die Bearbeitung einer einzelnen Hierarchie](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png#lightbox)

### <a name="delete-a-hierarchy"></a>Löschen einer Hierarchie

1. Wählen Sie die Hierarchie aus, und wählen Sie das **Löschen**- oder das **Papierkorb**-Symbol aus. 

    [![Löschen Sie eine Hierarchie, indem Sie die Schaltfläche „Löschen“ auswählen.](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png#lightbox)

1. Bestätigen Sie den Löschvorgang durch Auswahl von **Löschen**.

## <a name="types"></a>Typen

Der Azure Time Series Insights-Explorer unterstützt **ERSTELLEN**-, **LESEN**-, **AKTUALISIEREN**- und **LÖSCHEN**-Vorgänge für Typen im Browser. 

Wählen Sie zunächst die **Modell**-Ansicht aus der **Analyse**-Ansicht im Time Series Insights-Explorer aus.

### <a name="create-a-single-type"></a>Erstellen eines einzelnen Typs

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Typen** aus. Alle Typen, die der ausgewählten Time Series Insights-Umgebung zugeordnet sind, werden angezeigt.

    [![Bereich „Zeitreihenmodelltypen“](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png#lightbox)

1. Wählen Sie **+ Hinzufügen** aus, um das modale Popup **Neuen Typ hinzufügen** anzuzeigen.
1. Geben Sie Eigenschaften und Variablen für den Typ ein. Wählen Sie nach der Eingabe **Speichern** aus. 

    [![Konfigurationseinstellungen zum Hinzufügen eines Typs.](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Massenupload eines oder mehrerer Typen

> [!TIP]
> Sie können Ihre Typen auf dem Desktop in JSON speichern. Die heruntergeladene JSON-Datei kann anschließend mithilfe der folgenden Schritte hochgeladen werden.

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Typnutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    [![Optionen für den Massenupload von Typen.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png#lightbox)

### <a name="edit-a-single-type"></a>Bearbeiten eines einzelnen Typs

1. Wählen Sie den Typ aus, und wählen Sie das Symbol **Bearbeiten** oder das **Stiftsymbol** aus.
1. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

    [![Bearbeiten Sie einen Typ im Bereich.](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png#lightbox)

### <a name="delete-a-type"></a>Löschen eines Typs

1. Wählen Sie den Typ aus, und wählen Sie das **Löschen**- oder das **Papierkorb**-Symbol aus. erforderlich.

   [![Löschen Sie einen Typ, indem Sie „Löschen“ auswählen.](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png#lightbox)

1. Bestätigen Sie den Löschvorgang durch Auswahl von **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Datenmodellierung](./time-series-insights-update-tsm.md).

- Weitere Informationen zur Preview finden Sie unter [Visualisieren von Daten im Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

- Informationen zu unterstützten JSON-Formen finden Sie unter [Unterstützte JSON-Formen](./time-series-insights-send-events.md#supported-json-shapes).
