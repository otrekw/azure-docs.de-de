---
title: Erstellen von Klassifizierungsmodellen mit automatisiertem maschinellem Lernen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Klassifizierungsmodelle über die Oberfläche für automatisiertes maschinelles Lernen (automatisiertes ML) von Azure Machine Learning trainieren und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 02/04/2020
ms.openlocfilehash: 96af942ab68d4ae738df56bf94d8410ee5d8cc34
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129661"
---
# <a name="tutorial-create-a-classification-model-with-automated-ml-in-azure-machine-learning"></a>Tutorial: Erstellen eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen in Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

In diesem Tutorial erfahren Sie, wie Sie unter Verwendung der Oberfläche für automatisiertes maschinelles Lernen von Azure Machine Learning ein einfaches Klassifizierungsmodell erstellen, ohne eine einzige Codezeile zu schreiben. Dieses Klassifizierungsmodell sagt vorher, ob ein Kunde Festgeld bei einer Bank anlegt.

Mit automatisiertem maschinellen Lernen können Sie zeitintensive Aufgaben automatisieren. Beim automatisierten maschinellen Lernen werden viele Kombinationen von Algorithmen und Hyperparametern schnell durchlaufen, um basierend auf einer von Ihnen ausgewählten Erfolgsmetrik das beste Modell zu ermitteln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie einen Azure Machine Learning-Arbeitsbereich.
> * Ausführen eines automatisierten Machine Learning-Experiments
> * Anzeigen von Details zum Experiment
> * Bereitstellen des Modells.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.

* Laden Sie die Datendatei [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) herunter. In der Spalte **y** ist angegeben, ob ein Kunde Festgeld angelegt hat. Sie wird später als Zielspalte für Vorhersagen in diesem Tutorial festgelegt. 

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Azure Machine Learning-Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im Dienst. 

Sie erstellen einen Arbeitsbereich über das Azure-Portal, einer webbasierten Konsole zum Verwalten Ihrer Azure-Ressourcen.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal-enterprise.md)]

>[!IMPORTANT] 
> Notieren Sie sich Ihren **Arbeitsbereich** und Ihr **Abonnement**. Sie benötigen diese Informationen, um sicherzustellen, dass Sie Ihr Experiment an der richtigen Stelle erstellen. 

## <a name="create-and-run-the-experiment"></a>Erstellen und Ausführen des Experiments

Sie schließen die folgende Experimenteinrichtung ab und führen Schritte über Azure Machine Learning unter https://ml.azure.com aus. Hierbei handelt es sich um eine konsolidierte Weboberfläche mit Tools für maschinelles Lernen zur Durchführung von Data Science-Szenarien für Datenwissenschaftler jeglicher Qualifikation. Diese Oberfläche wird in Internet Explorer-Browsern nicht unterstützt.

1. Melden Sie sich unter https://ml.azure.com bei Azure Machine Learning an.

1. Wählen Sie Ihr Abonnement und den erstellten Arbeitsbereich aus.

1. Wählen Sie **Erste Schritte** aus.

1. Wählen Sie im linken Bereich im Abschnitt **Ersteller** die Option **Automatisiertes maschinelles Lernen** aus.

   Wenn Sie zum ersten Mal ein Experiment für automatisiertes ML ausführen, werden eine leere Liste und Links zur Dokumentation angezeigt.

   ![Seite für die ersten Schritte](./media/tutorial-first-experiment-automated-ml/get-started.png)

1. Wählen Sie **Neue automatisierte ML-Ausführung** aus. 

1. Erstellen Sie ein neues Dataset, indem Sie im Dropdown **+ Dataset erstellen** die Option **Aus lokalen Dateien** auswählen. 

    1. Geben Sie Ihrem Dataset im Formular **Grundlegende Informationen** einen Namen, und geben Sie optional eine Beschreibung an. Die Oberfläche für automatisiertes maschinelles Lernen unterstützt derzeit nur TabularDatasets. Der Datasettyp muss daher standardmäßig auf *Tabellarisch* festgelegt sein.

    1. Wählen Sie links unten die Option **Weiter** aus.

    1. Wählen Sie im Formular **Datenspeicher- und Dateiauswahl** den Standarddatenspeicher aus, der im Zuge der Erstellung Ihres Arbeitsbereichs automatisch eingerichtet wurde: **workspaceblobstore (Azure Blob Storage)** . Hier laden Sie Ihre Datendatei hoch, um sie für Ihren Arbeitsbereich verfügbar zu machen.

    1. Wählen Sie **Durchsuchen** aus.
    
    1. Wählen Sie auf dem lokalen Computer die Datei **bankmarketing_train.csv** aus. Dies ist die Datei, die Sie als [Voraussetzung](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) heruntergeladen haben.

    1. Weisen Sie Ihrem Dataset einen eindeutigen Namen zu, und geben Sie eine optionale Beschreibung ein. 

    1. Wählen Sie unten links **Weiter** aus, um das Dataset in den Standardcontainer hochzuladen, der bei der Erstellung Ihres Arbeitsbereichs automatisch eingerichtet wurde.  
    
       Wenn der Upload abgeschlossen ist, wird das Formular „Einstellungen und Vorschau“ basierend auf dem Dateityp vorab aufgefüllt. 
       
    1. Überprüfen Sie, ob das Formular **Einstellungen und Vorschau** wie folgt ausgefüllt ist, und klicken Sie auf **Weiter**.
        
        Feld|BESCHREIBUNG| Wert für das Tutorial
        ---|---|---
        Dateiformat|Definiert das Layout und den Typ der in einer Datei gespeicherten Daten.| Durch Trennzeichen getrennt
        Trennzeichen|Mindestens ein&nbsp;Zeichen zum Angeben der Grenze zwischen separaten, unabhängigen Regionen in Nur-Text-Datenströmen oder anderen Datenströmen. |Komma
        Codieren|Gibt an, welche Bit-zu-Zeichen-Schematabelle verwendet werden soll, um Ihr Dataset zu lesen.| UTF-8
        Spaltenüberschriften| Gibt an, wie die Header des Datasets, sofern vorhanden, behandelt werden.| Alle Dateien weisen dieselben Header auf.
        Zeilen überspringen | Gibt an, wie viele Zeilen im Dataset übersprungen werden.| Keine

    1. Das Formular **Schema** ermöglicht eine weitere Konfiguration der Daten für dieses Experiment. Wählen Sie für dieses Beispiel den Umschalter für das Feature **day_of_week** aus, um es für dieses Experiment nicht einzuschließen. Wählen Sie **Weiter** aus.

        ![Konfiguration der Registerkarte „Preview“ (Vorschau)](./media/tutorial-first-experiment-automated-ml/schema-tab-config.gif)

    1. Überprüfen Sie im Formular **Details bestätigen** die zuvor in die Formulare **Grundlegende Infos** und **Einstellungen und Vorschau** eingetragenen Informationen.
    1. Wählen Sie **Erstellen** aus, um die Erstellung Ihres Datasets abzuschließen.
    1. Wählen Sie Ihr Dataset aus, sobald es in der Liste angezeigt wird.
    1. Überprüfen Sie die **Datenvorschau**, um sicherzustellen, dass Sie nicht **day_of_week** einbezogen haben. Klicken Sie dann auf **OK**.

    1. Klicken Sie auf **Weiter**.

1. Füllen Sie das Formular **Ausführung konfigurieren** folgendermaßen aus:
    1. Geben Sie den folgenden Experimentnamen ein: `my-1st-automl-experiment`

    1. Wählen Sie **y** als Zielspalte aus, in der Sie Vorhersagen ausführen möchten. Diese Spalte gibt an, ob der Kunde eine Termineinlage bei der Bank gezeichnet hat.
    1. Wählen Sie **Create a new compute** (Neuen Compute erstellen) aus, und konfigurieren Sie Ihr Computeziel. Ein Computeziel ist eine lokale oder cloudbasierte Ressourcenumgebung, in der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Für dieses Experiment verwenden wir eine cloudbasierte Computeumgebung. 

        Feld | BESCHREIBUNG | Wert für das Tutorial
        ----|---|---
        Computename |Ein eindeutiger Name, der Ihren Computekontext identifiziert.|automl-compute
        Größe des virtuellen&nbsp;Computers&nbsp;| Wählen Sie die Größe für Ihren Computes aus.|Standard_DS12_V2
        Min/Max nodes (Min./Max. Knoten) (unter „Erweiterte Einstellungen“)| Um ein Datenprofil zu erstellen, müssen Sie mindestens einen Knoten angeben.|Min. Knoten: 1<br>Max. Knoten: 6
  
        1. Wählen Sie **Erstellen** aus, um das Computeziel abzurufen. 

            **Dieser Vorgang nimmt einige Minuten in Anspruch.** 

        1. Wählen Sie nach der Erstellung in der Dropdownliste Ihr neues Computeziel aus.

    1. Wählen Sie **Weiter** aus.

1. Wählen Sie im Formular **Aufgabentyp und -einstellungen** als ML-Aufgabentyp **Klassifizierung** aus.

    1. Klicken Sie auf **Zusätzliche Konfigurationseinstellungen anzeigen**, und füllen Sie die Felder wie folgt aus. Mit diesen Einstellungen können Sie den Trainingsauftrag besser steuern. Andernfalls werden die Standardwerte auf Basis der Experimentauswahl und -daten angewendet.

        >[!NOTE]
        > Für dieses Tutorial legen Sie keine Metrikbewertung oder maximale Anzahl von Kernen pro Iteration fest. Sie verhindern auch nicht, dass Algorithmen getestet werden.
   
        Zusätzliche&nbsp;Konfigurationen|BESCHREIBUNG|Wert&nbsp;für&nbsp;das Tutorial
        ------|---------|---
        Primary metric (Primäre Metrik)| Auswertungsmetrik, die zur Messung des Machine Learning-Algorithmus verwendet wird.|AUC_weighted
        Automatische Featurebereitstellung| Ermöglicht eine Vorabaufbereitung der Daten. Dies umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren.| Aktivieren
        Blockierte Algorithmen | Algorithmen, die Sie aus den Trainingsauftrag ausschließen möchten.| Keine
        Beendigungskriterium| Wenn ein Kriterium erfüllt ist, wird der Trainingsauftrag angehalten. |Dauer&nbsp;des&nbsp;Trainingsauftrags (Stunden): 1 <br> Metrikschwellenwert&nbsp;&nbsp;: Keine
        Überprüfen | Wählen Sie einen Kreuzvalidierungstyp und die Anzahl von Tests aus.|Überprüfungstyp:<br>&nbsp;k-fold&nbsp;cross-validation <br> <br> Anzahl von Überprüfungen: 2
        Parallelität| Die maximale Anzahl paralleler Iterationen pro Iteration| Max.&nbsp;parallele&nbsp;Iterationen: 5
        
        Wählen Sie **Speichern** aus.

1. Wählen Sie **Fertig stellen** aus, um das Experiment auszuführen. Wenn die Vorbereitung des Experiments beginnt, wird der Bildschirm **Ausführungsdetails** geöffnet, auf dem am oberen Rand der **Ausführungsstatus** angezeigt wird.

>[!IMPORTANT]
> Die Vorbereitung des Experiments nimmt **10 –15 Minuten** in Anspruch.
> Sobald es ausgeführt wird, dauert **jede Iteration mindestens zwei bis drei Minuten**.  
> Klicken Sie von Zeit zu Zeit auf **Aktualisieren**, um den Status der Ausführung während der Experimentausführung anzuzeigen.
>
> In einer Produktionsumgebung würden Sie in dieser Zeit wahrscheinlich eine kurze Pause machen. Für dieses Tutorial empfehlen wir jedoch, schon während der Ausführung der weiteren Iterationen mit der Untersuchung der getesteten Algorithmen auf der Registerkarte **Modelle** zu beginnen. 

##  <a name="explore-models"></a>Untersuchen von Modellen

Navigieren Sie zur Registerkarte **Modelle**, um die getesteten Algorithmen (Modelle) anzuzeigen. Standardmäßig werden die Modelle nach ihrem Abschluss nach der Metrikbewertung sortiert. In diesem Tutorial steht das Modell, das für die ausgewählte **AUC_weighted**-Metrik die höchste Bewertung erhält, ganz oben in der Liste.

Während Sie auf den Abschluss aller Experimentmodelle warten, können Sie den **Algorithmusnamen** eines abgeschlossenen Modells auswählen und sich die zugehörigen Leistungsdetails ansehen. 

Nachfolgend werden die Registerkarten **Modelldetails** und **Visualisierungen** durchlaufen, um die Eigenschaften, Metriken und Leistungsdiagramme des ausgewählten Modells anzuzeigen. 

![Details zur Iterationsausführung](./media/tutorial-first-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-best-model"></a>Bereitstellen des besten Modells

Über die Oberfläche für automatisiertes maschinelles Lernen können Sie in wenigen Schritten das beste Modell als Webdienst bereitstellen. Bei der Bereitstellung handelt es sich um die Integration des Modells, sodass neue Daten vorhergesagt und potenzielle Verkaufschancen identifiziert werden können. 

In diesem Experiment bedeutet Bereitstellung in einem Webdienst, dass das Finanzinstitut nun über eine iterative und skalierbare Weblösung zur Identifizierung potenzieller Festgeldkunden verfügt. 

Nach Abschluss der Ausführung navigieren Sie zurück zur Seite **Ausführungsdetails** und wählen die Registerkarte **Modelle** aus.

Aus diesem Experimentkontext geht **VotingEnsemble** basierend auf der **AUC_weighted**-Metrik als bestes Modell hervor.  Wir stellen dieses Modell bereit. Die Bereitstellung dauert jedoch etwa 20 Minuten. Der Bereitstellungsprozess umfasst mehrere Schritte, einschließlich der Registrierung des Modells, der Erstellung von Ressourcen und der Konfiguration dieser Ressourcen für den Webdienst.

1. Wählen Sie rechts unten die Schaltfläche **Deploy Best Model** (Bestes Modell bereitstellen) aus.

1. Füllen Sie den Bereich **Modell bereitstellen** wie folgt aus:

    Feld| Wert
    ----|----
    „Deployment name“ (Bereitstellungsname)| my-automl-deploy
    „Deployment description“ (Bereitstellungsbeschreibung)| „Meine erste Bereitstellung eines automatisierten Machine Learning-Experiments“
    Computetyp | Azure-Compute-Instanz (ACI) auswählen
    Authentifizierung aktivieren| Deaktivieren Sie diese Option. 
    Use custom deployments (Benutzerdefinierte Bereitstellungen verwenden)| Deaktivieren Sie diese Option. Dadurch wird die automatische Erstellung der Standardtreiberdatei (Bewertungsskript) und der Umgebungsdatei ermöglicht. 
    
    In diesem Beispiel werden die im Menü *Erweitert* angegebenen Standardwerte verwendet. 

1. Klicken Sie auf **Bereitstellen**.  

    Oben auf dem Bildschirm **Ausführen** wird eine grüne Erfolgsmeldung und im Bereich **Empfohlenes Modell** unter **Bereitstellungsstatus** eine Statusmeldung angezeigt. Wählen Sie von Zeit zu Zeit die Option **Aktualisieren**, um den Status der Bereitstellung zu überprüfen.
    
Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können. 

Fahren Sie mit [**Nächste Schritte**](#next-steps) fort, um weitere Informationen zur Nutzung Ihres neuen Webdiensts zu erhalten, und testen Sie Ihre Vorhersagen mithilfe der integrierten Azure Machine Learning-Unterstützung von Power BI.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereitstellungsdateien sind größer als Daten- und Experimentdateien, sodass ihre Speicherung teurer ist. Löschen Sie nur die Bereitstellungsdateien, um die Kosten für Ihr Konto zu minimieren, oder wenn Sie den Arbeitsbereich und die Experimentdateien beibehalten möchten. Löschen Sie andernfalls die gesamte Ressourcengruppe, wenn Sie keine der Dateien verwenden möchten.  

### <a name="delete-the-deployment-instance"></a>Löschen der Bereitstellungsinstanz

Löschen Sie nur die Bereitstellungsinstanz aus Azure Machine Learning unter https://ml.azure.com/, wenn Sie die Ressourcengruppe und den Arbeitsbereich für andere Tutorials und Untersuchungen behalten möchten. 

1. Navigieren Sie zu Azure Machine Learning unter https://ml.azure.com/. Navigieren Sie zu Ihrem Arbeitsbereich, und wählen Sie links unter dem Bereich **Ressourcen** die Option **Endpunkte** aus. 

1. Wählen Sie die zu löschende Bereitstellung aus, und klicken Sie auf **Delete** (Löschen). 

1. Wählen Sie **Proceed** (Fortfahren) aus.

### <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial zum automatisierten maschinellen Lernen haben Sie über die Oberfläche für automatisiertes maschinelles Lernen von Azure Machine Learning ein Klassifizierungsmodell erstellt und bereitgestellt. Weitere Informationen und nächste Schritte finden Sie in diesen Artikeln:

> [!div class="nextstepaction"]
> [Verwenden eines Webdiensts](how-to-consume-web-service.md#consume-the-service-from-power-bi)

+ Weitere Informationen zu [automatisiertem Machine Learning](concept-automated-ml.md).
+ Weitere Informationen zu Klassifizierungsmetriken und -diagrammen finden Sie im Artikel [Grundlegendes zu den Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md#classification). Weitere Informationen zur Merkmalserstellung finden Sie [hier](how-to-use-automated-ml-for-ml-models.md#featurization).
+ Weitere Informationen zur [Datenprofilerstellung](how-to-use-automated-ml-for-ml-models.md#profile).


>[!NOTE]
> Dieses Bank Marketing-Dataset wird unter der [Creative Commons (CCO: Public Domain)-Lizenz](https://creativecommons.org/publicdomain/zero/1.0/) zur Verfügung gestellt. Alle Rechte in den einzelnen Inhalten der Datenbank sind gemäß der [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) lizenziert und auf [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset) verfügbar. Dieses Dataset war ursprünglich in der [UCI Machine Learning Database](https://archive.ics.uci.edu/ml/datasets/bank+marketing) verfügbar.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez und P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. (Ein datengesteuerter Ansatz zur Prognose des Erfolgs im Bank-Telemarketing.) Decision Support Systems, Elsevier, 62:22-31, Juni 2014.
