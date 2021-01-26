---
title: 'Tutorial: Erste Schritte mit Azure Synapse Analytics: Visualisieren von Arbeitsbereichsdaten mit Power BI'
description: In diesem Tutorial wird beschrieben, wie Sie einen Power BI-Arbeitsbereich erstellen, Ihren Azure Synapse-Arbeitsbereich verlinken und ein Power BI-Dataset erstellen, bei dem Daten im Azure Synapse-Arbeitsbereich genutzt werden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: business-intelligence
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 952d69cccff86d1a0119391c400a40908c62ed69
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208963"
---
# <a name="visualize-data-with-power-bi"></a>Visualisieren von Daten mit Power BI

In diesem Tutorial wird beschrieben, wie Sie einen Power BI-Arbeitsbereich erstellen, Ihren Azure Synapse-Arbeitsbereich verlinken und ein Power BI-Dataset erstellen, bei dem Daten in Ihrem Azure Synapse-Arbeitsbereich genutzt werden. 

> [!NOTE]
> Für dieses Tutorial müssen Sie [Power BI Desktop installieren](https://aka.ms/pbidesktopstore).

## <a name="overview"></a>Übersicht

Aus den NYC Taxi-Daten haben wir aggregierte Datasets in zwei Tabellen erstellt:
- **nyctaxi.passengercountstats**
- **SQLDB1.dbo.PassengerCountStats**

Sie können einen Power BI-Arbeitsbereich mit Ihrem Azure Synapse-Arbeitsbereich verknüpfen. Mit dieser Funktion können Sie Daten problemlos in Ihren Power BI-Arbeitsbereich einfügen. Sie können Ihre Power BI-Berichte direkt in Ihrem Azure Synapse-Arbeitsbereich bearbeiten. 

### <a name="create-a-power-bi-workspace"></a>Erstellen eines Power BI-Arbeitsbereichs

1. Melden Sie sich bei [powerbi.microsoft.com](https://powerbi.microsoft.com/) an.
1. Klicken Sie auf **Arbeitsbereiche**, und wählen Sie dann **Arbeitsbereich erstellen** aus. Erstellen Sie einen neuen Power BI-Arbeitsbereich mit dem Namen **NYCTaxiWorkspace1** oder einem ähnlichen Namen. Dieser Name muss eindeutig sein.

### <a name="link-your-azure-synapse-workspace-to-your-new-power-bi-workspace"></a>Verknüpfen Ihres Azure Synapse-Arbeitsbereichs mit Ihrem neuen Power BI-Arbeitsbereich

1. Wechseln Sie in Synapse Studio zu **Verwalten** > **Verknüpfte Dienste**.
1. Wählen Sie **Neu** > **Mit Power BI verbinden** aus.
1. Legen Sie **Name** auf **NYCTaxiWorkspace1** fest.
1. Legen Sie **Arbeitsbereichsname** auf den Power BI-Arbeitsbereich fest, den Sie oben erstellt haben (**NYCTaxiWorkspace1** oder ähnlicher Name).
1. Klicken Sie auf **Erstellen**.

### <a name="create-a-power-bi-dataset-that-uses-data-in-your-azure-synapse-workspace"></a>Erstellen eines Power BI-Datasets, das Daten in Ihrem Azure Synapse-Arbeitsbereich verwendet

1. Wechseln Sie in Synapse Studio zu **Entwickeln** > **Power BI**.
1. Navigieren Sie zu **NYCTaxiWorkspace1** > **Power BI-Datasets**, und wählen Sie **Neues Power BI-Dataset** aus. Klicken Sie auf **Start**.
1. Wählen Sie die Datenquelle **SQLPOOL1** aus, und klicken Sie auf **Weiter**.
1. Klicken Sie auf **Herunterladen**, um die PBIDS-Datei für Ihre Datei **NYCTaxiWorkspace1SQLPOOL1.pbids** herunterzuladen. Klicken Sie auf **Weiter**.
1. Öffnen Sie die heruntergeladene **.pbids**-Datei. Power BI Desktop wird geöffnet, und es wird automatisch eine Verbindung mit **SQLDB1** in Ihrem Azure Synapse-Arbeitsbereich hergestellt.
1. Gehen Sie wie folgt vor, wenn ein Dialogfeld mit dem Namen **SQL Server-Datenbank** angezeigt wird:
    1. Wählen Sie **Microsoft-Konto** aus.
    1. Wählen Sie **Anmelden** aus, und melden Sie sich bei Ihrem Konto an.
    1. Wählen Sie **Verbinden**.
1. Aktivieren Sie nach dem Öffnen des Dialogfelds **Navigator** die Tabelle **PassengerCountStats**, und wählen Sie **Laden** aus.
1. Wählen Sie nach Anzeige des Dialogfelds **Verbindungseinstellungen** die Option **DirectQuery** > **OK** aus.
1. Wählen Sie auf der linken Seite die Schaltfläche **Bericht** aus.
1. Klicken Sie unter **Visualisierungen** auf das Liniendiagrammsymbol, um Ihrem Bericht ein **Liniendiagramm** hinzuzufügen.
    1. Ziehen Sie unter **Felder** die Spalte **PassengerCount** auf **Visualisierungen** > **Achse**.
    1. Ziehen Sie die Spalten **SumTripDistance** und **AvgTripDistance** auf **Visualisierungen** > **Werte**.
1. Wählen Sie auf der Registerkarte **Start** die Option **Veröffentlichen** aus.
1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern.
1. Wählen Sie den Dateinamen **PassengerAnalysis.pbix** und dann die Option **Speichern** aus.
1. Wählen Sie im Fenster **In Power BI veröffentlichen** unter **Ziel auswählen** den Arbeitsbereich **NYCTaxiWorkspace1** aus, und klicken Sie dann auf **Auswählen**.
1. Warten Sie, bis die Veröffentlichung abgeschlossen ist. 

### <a name="configure-authentication-for-your-dataset"></a>Konfigurieren der Authentifizierung für Ihr Dataset

1. Öffnen Sie [powerbi.microsoft.com](https://powerbi.microsoft.com/), und **melden Sie sich an**.
1. Wählen Sie auf der linken Seite unter **Arbeitsbereiche** den Arbeitsbereich **NYCTaxiWorkspace1** aus.
1. Suchen Sie in diesem Arbeitsbereich nach dem Dataset **PassengerAnalysis** und dem Bericht **PassengerAnalysis**.
1. Zeigen Sie auf das Dataset **PassengerAnalysis**, und wählen Sie die Schaltfläche mit den Auslassungspunkten (...) und dann die Option **Einstellungen** aus.
1. Klicken Sie unter **Anmeldeinformationen für Datenquelle** auf **Bearbeiten**, legen Sie die  **Authentifizierungsmethode** auf **OAuth2** fest, und wählen Sie anschließend **Anmelden** aus.

### <a name="edit-a-report-in-synapse-studio"></a>Bearbeiten eines Berichts in Synapse Studio

1. Wechseln Sie zurück zu Synapse Studio, und wählen Sie **Schließen und aktualisieren** aus.
1. Navigieren Sie zum Hub **Entwickeln**.
1. Klicken Sie rechts neben der Ebene **Power BI** auf die Schaltfläche mit Auslassungspunkten (…) und anschließend auf **Aktualisieren**, um den Knoten **Power BI-Berichte** zu aktualisieren.
1. Unter **Power BI** sollte Folgendes angezeigt werden:
    * Unter **NYCTaxiWorkspace1** > **Power BI-Datasets** ein neues Dataset mit dem Namen **PassengerAnalysis**
    * Unter **NYCTaxiWorkspace1** > **Power BI-Berichte** ein neuer Bericht mit dem Namen **PassengerAnalysis**.
1. Wählen Sie den Bericht **PassengerAnalysis** aus. Der Bericht wird geöffnet, und Sie können ihn direkt in Synapse Studio bearbeiten.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überwachen](get-started-monitor.md)
                                 

