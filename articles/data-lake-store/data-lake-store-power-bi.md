---
title: Analysieren von Daten in Azure Data Lake Storage Gen1 – Power BI
description: Erfahren Sie, wie Sie in Azure Data Lake Storage Gen1 gespeicherte Daten mithilfe von Power BI Desktop analysieren und visualisieren.
author: twooley
ms.service: data-lake-store
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 282ac6f87d5ecff75e77b075ee1092f79b4923ad
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192865"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen1-by-using-power-bi"></a>Analysieren von Daten in Azure Data Lake Storage Gen1 mithilfe von Power BI
In diesem Artikel erfahren Sie, wie Sie in Azure Data Lake Storage Gen1 gespeicherte Daten mithilfe von Power BI Desktop analysieren und visualisieren.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Ein Data Lake Storage Gen1-Konto.** Befolgen Sie die Anweisungen unter [Erste Schritte mit Azure Data Lake Storage Gen1 über das Azure-Portal](data-lake-store-get-started-portal.md). In diesem Artikel wird davon ausgegangen, dass Sie bereits ein Data Lake Storage Gen1-Konto erstellt, **myadlsg1** aufgerufen und eine Beispieldatendatei (**Drivers.txt**) hochgeladen haben. Diese Beispieldatei kann aus dem [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)heruntergeladen werden.
* **Power BI Desktop**. Sie können das Tool aus dem [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331) herunterladen. 

## <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop
1. Starten Sie Power BI Desktop auf Ihrem Computer.
2. Klicken Sie auf dem Menüband **Start** auf **Daten abrufen** und dann auf „Mehr“. Klicken Sie im Dialogfeld **Daten abrufen** auf **Azure**. Klicken Sie auf **Azure Data Lake Store** und dann auf **Verbinden**.
   
    ![Herstellen der Verbindung mit Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Herstellen der Verbindung mit Data Lake Storage Gen1")
3. Wenn ein Dialogfeld anzeigt, dass der Connector sich in der Entwicklungsphase befindet, klicken Sie auf die Option zum Fortfahren.
4. Geben Sie im Dialogfeld **Azure Data Lake Store** die URL für Ihr Data Lake Storage Gen1-Konto an, und klicken Sie dann auf **OK**.
   
    ![URL für Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL für Data Lake Storage Gen1")
5. Klicken Sie im nächsten Dialogfeld auf **Anmelden**, um sich beim Data Lake Storage Gen1-Konto anzumelden. Sie werden zur Anmeldeseite Ihrer Organisation umgeleitet. Folgen Sie den Anweisungen, um sich beim Konto anzumelden.
   
    ![Anmelden bei Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Anmelden bei Data Lake Storage Gen1")
6. Wenn Sie sich erfolgreich angemeldet haben, klicken Sie auf **Verbinden**.
   
    ![Herstellen der Verbindung mit Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Herstellen der Verbindung mit Data Lake Storage Gen1")
7. Das nächste Dialogfeld zeigt die Datei an, die Sie in Ihr Data Lake Storage Gen1-Konto hochgeladen haben. Überprüfen Sie die Informationen, und klicken Sie dann auf **Laden**.
   
    ![Herunterladen von Daten aus Data Lake Storage Gen1](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Herunterladen von Daten aus Data Lake Storage Gen1")
8. Nachdem die Daten erfolgreich in Power BI geladen wurden, werden auf der Registerkarte **Felder** folgende Felder angezeigt.
   
    ![Importierte Felder](./media/data-lake-store-power-bi/imported-fields.png "Importierte Felder")
   
    Um die Daten visualisieren und analysieren zu können, sollten sie jedoch gemäß der folgenden Felder verfügbar sein:
   
    ![Gewünschte Felder](./media/data-lake-store-power-bi/desired-fields.png "Gewünschte Felder")
   
    In den nächsten Schritten aktualisieren wir die Abfrage, sodass die importierten Daten in das gewünschte Format konvertiert werden.
9. Klicken Sie auf dem Menüband **Start** auf **Abfragen bearbeiten**.
   
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/edit-queries.png "Abfragen bearbeiten")
10. Klicken Sie im Abfrage-Editor unter der Spalte **Inhalt** auf **Binär**.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query1.png "Abfragen bearbeiten")
11. Es wird ein Dateisymbol angezeigt, das die von Ihnen hochgeladene Datei **Drivers.txt** darstellt. Klicken Sie mit der rechten Maustaste auf die Datei, und klicken Sie auf **CSV**.    
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query2.png "Abfragen bearbeiten")
12. Es sollte eine Ausgabe wie unten angezeigt werden. Ihre Daten stehen jetzt in einem Format zur Verfügung, das Sie zum Erstellen von Visualisierungen verwenden können.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/convert-query3.png "Abfragen bearbeiten")
13. Klicken Sie auf dem Menüband **Start** auf **Schließen und übernehmen** und dann auf **Schließen & übernehmen**.
    
    ![Bearbeiten von Abfragen](./media/data-lake-store-power-bi/load-edited-query.png "Abfragen bearbeiten")
14. Sobald die Abfrage aktualisiert wurde, zeigt die Registerkarte **Felder** die neuen Felder, die zur Visualisierung verfügbar sind.
    
    ![Aktualisierte Felder](./media/data-lake-store-power-bi/updated-query-fields.png "Aktualisierte Felder")
15. Erstellen wir nun ein Kreisdiagramm, um für ein bestimmtes Land bzw. eine bestimmte Region die Fahrer in jeder Stadt darzustellen. Wählen Sie dazu folgende Optionen aus.
    
    1. Klicken Sie auf der Registerkarte „Visualisierungen“ auf das Symbol für ein Kreisdiagramm.
       
        ![Erstellen eines Kreisdiagramms](./media/data-lake-store-power-bi/create-pie-chart.png "Kreisdiagramm erstellen")
    2. Wir verwenden folgende Spalten: **Spalte 4** (Name der Stadt) und **Spalte 7** (Name des Landes bzw. der Region). Ziehen Sie diese Spalten aus der Registerkarte **Felder** auf die Registerkarte **Visualisierungen**, wie unten gezeigt.
       
        ![Erstellen von Visualisierungen](./media/data-lake-store-power-bi/create-visualizations.png "Erstellen von Visualisierungen")
    3. Das Kreisdiagramm sollte nun wie das unten gezeigte aussehen.
       
        ![Kreisdiagramm](./media/data-lake-store-power-bi/pie-chart.png "Erstellen von Visualisierungen")
16. Indem Sie aus den Filtern auf Seitenebene ein bestimmtes Land bzw. eine bestimmte Region auswählen, können Sie die Anzahl von Fahrern in jeder Stadt des ausgewählten Landes bzw. der ausgewählten Region anzeigen. Wählen Sie z.B. auf der Registerkarte **Visualisierungen** unter **Filter auf Seitenebene** als Land **Brasilien** aus.
    
    ![Auswählen eines Lands/einer Region](./media/data-lake-store-power-bi/select-country.png "Auswählen eines Lands/einer Region")
17. Das Kreisdiagramm wird automatisch aktualisiert und zeigt die Fahrer in den brasilianischen Städten an.
    
    ![Fahrer in einem Land/einer Region](./media/data-lake-store-power-bi/driver-per-country.png "Fahrer pro Land/Region")
18. Klicken Sie im Menü **Datei** auf **Speichern**, um die Visualisierung als Power BI Desktop-Datei zu speichern.

## <a name="publish-report-to-power-bi-service"></a>Veröffentlichen des Berichts im Power BI-Dienst
Nachdem Sie die Visualisierungen in Power BI Desktop erstellt haben, können Sie diese für andere Personen freigeben, indem Sie sie im Power BI-Dienst veröffentlichen. Ausführliche Anweisungen hierzu finden Sie unter [Veröffentlichen aus Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Weitere Informationen
* [Analysieren von Daten in Data Lake Storage Gen1 mit Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

