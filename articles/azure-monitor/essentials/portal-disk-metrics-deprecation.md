---
title: Veraltete Datenträgermetriken im Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Datenträgermetriken veraltet sind und wie Sie Ihre Metrikwarnungen mit neuen Metriken aktualisieren.
services: azure-monitor
ms.subservice: metrics
ms.topic: conceptual
author: albecker1
ms.author: albecker
ms.date: 03/12/2020
ms.openlocfilehash: 623d9385b9ae6b13c8964f655fb973fe67a0918a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601349"
---
# <a name="disk-metrics-deprecation-in-the-azure-portal"></a>Veraltete Datenträgermetriken im Azure-Portal

Veraltete datenträgerbezogene Metriken werden demnächst aus dem Azure-Portal entfernt. Für die veralteten Metriken steht jeweils eine neue Version zur Verfügung. In diesem Artikel erfahren Sie, welche Metriken neu sind und wie Sie Ihre Metrikwarnungen aktualisieren, um sie zu verwenden.

## <a name="list-of-new-metrics"></a>Liste neuer Metriken

Die folgende Tabelle gibt Aufschluss über die veralteten Metriken und die jeweils entsprechenden neuen Metriken. 

|Veraltete Metrik|Neue Metrik (Ersatzmetrik)|
|----|----|
|Warteschlangentiefe für Datenträger (veraltet)|Warteschlangentiefe für Datenträger (Vorschauversion)|
|Vom Datenträger gelesene Bytes/Sek. (veraltet)|Vom Datenträger gelesene Bytes/Sek. (Vorschauversion)|
|Datenträgerlesevorgänge/Sek. (veraltet)|Datenträgerlesevorgänge/Sek. (Vorschauversion)|
|Auf den Datenträger geschriebene Bytes/Sek. (veraltet)|Auf den Datenträger geschriebene Bytes/Sek. (Vorschauversion)|
|Datenträgerschreibvorgänge/Sek. (veraltet)|Datenträgerschreibvorgänge/Sek. (Vorschauversion)|
|Warteschlangentiefe für Betriebssystem (veraltet)|Warteschlangentiefe für Betriebssystem (Vorschau)|
|Vom Betriebssystem gelesene Bytes/s (veraltet)|Vom Betriebssystem gelesene Bytes/s (Vorschau)|
|Lesevorgänge des Betriebssystems/s (veraltet)|Lesevorgänge des Betriebssystems/s (Vorschau)|
|Vom Betriebssystem geschriebene Bytes/s (veraltet)|Vom Betriebssystem geschriebene Bytes/s (Vorschau)|
|Schreibvorgänge des Betriebssystems/s (veraltet)|Schreibvorgänge des Betriebssystems/s (Vorschau)|

<a id="update-metrics" />

## <a name="migrate-metrics-in-your-metric-alerts"></a>Migrieren von Metriken in Ihren Metrikwarnungen

Aktualisieren Sie Ihre Metrikwarnungen, um neue Metriken zu verwenden.

1. Suchen Sie im Azure-Portal nach **Warnungen**. Wählen Sie im Abschnitt **Dienste** die Option **Warnungen** aus.

   > [!div class="mx-imgBorder"]
   > ![Benachrichtigungsdienst](./media/portal-disk-metrics-deprecation/alert-service-azure-portal.png)

2. Wählen Sie auf der Seite **Warnungen** die Schaltfläche **Warnungsregeln verwalten** aus. 

   > [!div class="mx-imgBorder"]
   > ![Verwalten von Warnungsregeln](./media/portal-disk-metrics-deprecation/manage-alert-rules-button.png)

3. Aktivieren Sie in der Dropdownliste **Ressourcengruppe** das Kontrollkästchen **Virtuelle Computer** und in der Dropdownliste **Signaltyp** das Kontrollkästchen **Metriken**. 

   > [!div class="mx-imgBorder"]
   > ![Warnungen filtern](./media/portal-disk-metrics-deprecation/filter-alerts.png)

4. Suchen Sie in der Metrikliste nach datenträgerbezogenen Bedingungen. Klicken Sie auf den Namen der Regel. 

   Der Name wird in der Spalte **Name** als Link dargestellt.

   > [!div class="mx-imgBorder"]
   > ![Suchen von Datenträgerbedingungen](./media/portal-disk-metrics-deprecation/find-disk-conditions.png)

5. Klicken Sie im Abschnitt **Bedingungen** der Seite **Regelverwaltung** auf die Bedingung der Warnung. 

   Die Bedingung wird als Link dargestellt.  

   > [!div class="mx-imgBorder"]
   > ![Anpassungen von Bedingungen](./media/portal-disk-metrics-deprecation/adjust-condition.png)

   Die Seite **Signallogik konfigurieren** wird angezeigt. Die Einstellungen der Bedingung befinden sich auf dieser Seite im Bereich **Warnungslogik**.

6. Notieren Sie sich diese Einstellungen, da sie nach dem Entfernen der veralteten Metrik nicht mehr verfügbar sind.

   > [!div class="mx-imgBorder"]
   > ![Bedingungsregeln](./media/portal-disk-metrics-deprecation/condition-rules.png)

   > [!TIP] 
   > Erstellen Sie ggf. einen Screenshot, oder dokumentieren Sie die Einstellungen in einer Textdatei. 

7. Klicken Sie auf den Link **Zurück zur Signalauswahl**.

   > [!div class="mx-imgBorder"]
   > ![Zurück zur Signalauswahl](./media/portal-disk-metrics-deprecation/back-to-signal-selection.png)

8. Wählen Sie auf der Seite **Signallogik konfigurieren** die entsprechende Ersatzmetrik (neue Metrik) aus. Verwenden Sie die [Tabelle](#update-metrics) weiter oben in diesem Artikel, um den Namen der neuen Metrik zu ermitteln.

   > [!TIP] 
   > Beginnen Sie damit, den Namen in die Suchleiste einzugeben, um die Liste der Metriknamen einzugrenzen. 

   > [!div class="mx-imgBorder"]
   > ![Auswählen einer neuen Metrik](./media/portal-disk-metrics-deprecation/choose-new-metric.png)

9. Wählen Sie die Schaltfläche **Fertig** aus. 

   > [!div class="mx-imgBorder"]
   > ![Festlegen einer neuen Metrik](./media/portal-disk-metrics-deprecation/set-new-metric.png)

10. Wählen Sie die Schaltfläche **Speichern** aus, um Ihre Änderungen zu committen. 

    > [!div class="mx-imgBorder"]
    > ![Speichern der neuen Metrik](./media/portal-disk-metrics-deprecation/save-new-metric.png)






