---
title: Erstellen eines Datenbeschriftungsprojekts
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie Beschriftungsprojekte zum Kennzeichnen von Daten für das maschinelle Lernen erstellen und ausführen.  Zur Unterstützung bei dieser Aufgabe stehen die ML-gestützte Beschriftung sowie die Human-in-the-Loop-Beschriftung zur Verfügung.
author: sdgilley
ms.author: sgilley
ms.service: machine-learning
ms.topic: tutorial
ms.date: 03/01/2020
ms.openlocfilehash: 9974b42f582a3b5f26df0b6e77b42a03f23c47dd
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898716"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Erstellen eines Datenbeschriftungsprojekts und Exportieren der Beschriftungen 

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Das Beschriften großer Datenmengen in Projekten für maschinelles Lernen bereitet häufig Kopfzerbrechen. Projekte mit einer Komponente für maschinelles Sehen – z. B. zur Bildklassifizierung oder Objekterkennung – erfordern im Allgemeinen Beschriftungen für Tausende von Bildern.
 
[Azure Machine Learning](https://ml.azure.com/) ist Ihr zentraler Ort zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten. Verwenden Sie die Lösung zum Koordinieren von Daten, Beschriftungen und Teammitgliedern, um Beschriftungsaufgaben effizient zu verwalten. Machine Learning unterstützt Bildklassifizierung (entweder mit mehreren Beschriftungen oder mit mehreren Klassen) und die Objektidentifikation mit Begrenzungsrahmen.

Machine Learning verfolgt den Fortschritt nach und verwaltet die Warteschlange mit unvollständigen Beschriftungsaufgaben. Beschriftungsersteller benötigen kein Azure-Konto für die Teilnahme. Nachdem sie mit Ihrem Microsoft-Konto oder über [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) authentifiziert sind, können sie sich Beschriftungsaufgaben widmen, so wie es ihre Zeit erlaubt.

Sie können das Projekt starten und beenden, Beschriftungsersteller und Teams hinzufügen und entfernen sowie den Beschriftungsstatus überwachen. Sie können beschriftete Daten im COCO-Format oder als Azure Machine Learning-Dataset exportieren.

> [!Important]
> Derzeit werden nur Beschriftungsprojekte für die Bildklassifizierung und Objektidentifizierung unterstützt. Außerdem müssen die Datenbilder in einem Azure-Blobdatenspeicher verfügbar sein. (Wenn kein Datenspeicher vorhanden ist, können Sie Bilder während der Projekterstellung hochladen.) 

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Projekts
> * Angeben der Daten und Struktur des Projekts
> * Verwalten der Teams und Personen, die am Projekt arbeiten
> * Ausführen und Überwachen des Projekts
> * Exportieren der Beschriftungen


## <a name="prerequisites"></a>Voraussetzungen

* Die zu beschriftenden Daten (entweder in lokalen Dateien oder in Azure Blob Storage).
* Der Satz von Beschriftungen, die Sie anwenden möchten.
* Die Anweisungen für die Beschriftung.
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Erstellen eines Beschriftungsprojekts

Beschriftungsprojekte werden in Azure Machine Learning verwaltet. Auf der Seite **Beschriftungsprojekte** verwalten Sie Projekte und Personen. Einem Projekt können ein oder mehrere Teams zugewiesen sein, und ein Team kann aus einem oder mehreren Personen bestehen.

Wenn sich Ihre Daten bereits in einem Azure-Blobspeicher befinden, sollten Sie diesen als Datenspeicher zur Verfügung stellen, bevor Sie das Beschriftungsprojekt erstellen. Ausführliche Informationen dazu finden Sie unter [Erstellen und Registrieren von Datenspeichern](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores).

Zum Erstellen eines Projekts wählen Sie **Projekt hinzufügen** aus. Geben Sie dem Projekt einen geeigneten Namen, und wählen Sie den **Beschriftungsaufgabentyp** aus.

![Assistent zum Erstellen von Beschriftungsprojekten](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Wählen Sie **Bildklassifizierung mit mehreren Klassen** für Projekte aus, in denen nur eine *einzige Klasse* aus einer Reihe von Klassen auf ein Bild angewendet werden soll.
* Wählen Sie **Bildklassifizierung mit mehreren Beschriftungen** für Projekte aus, in denen *eine oder mehrere* Beschriftungen aus einer Reihe von Klassen auf ein Bild angewendet werden sollen. Ein Foto eines Hunds z. B. kann sowohl mit *Hund* als auch mit *Tag* beschriftet werden.
* Wählen Sie **Objektidentifikation (umgebendes Rechteck)** für Projekte aus, in denen jedem Objekt innerhalb eines Bilds eine Klasse und ein Begrenzungsrahmen zugewiesen werden soll.

Wählen Sie **Weiter**, wenn Sie bereit sind, fortzufahren.

## <a name="specify-the-data-to-label"></a>Festlegen der zu beschriftenden Daten

Wenn Sie bereits ein Dataset mit Ihren Daten erstellt haben, wählen Sie dieses aus der Dropdownliste **Vorhandenes Dataset auswählen** aus. Oder wählen Sie **Dataset erstellen** aus, um einen vorhandenen Azure-Datenspeicher auszuwählen oder lokale Dateien hochzuladen.


### <a name="create-a-dataset-from-an-azure-datastore"></a>Erstellen eines Datasets aus einem Azure-Datenspeicher

In vielen Fällen ist es in Ordnung, einfach lokale Dateien hochzuladen. [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) bietet jedoch eine schnellere und stabilere Methode zum Übertragen großer Datenmengen. Es wird empfohlen, Storage-Explorer als Standardmethode zum Verschieben von Dateien zu verwenden.

Erstellen eines Datasets aus Dateien, die Sie bereits in einem Azure-Blobspeicher gespeichert haben:

1. Wählen Sie **Dataset erstellen** > **Aus Datenspeicher**.
1. Weisen Sie Ihrem Dataset einen **Namen** zu.
1. Wählen Sie **Datei** als **Dataset-Typ** aus.  
1. Wählen Sie den Datenspeicher aus.
1. Wenn Ihre Daten sich in einem Unterordner des Blobspeichers befinden, klicken Sie auf **Durchsuchen**, um den Ordnerpfad auszuwählen.
    * Fügen Sie „/**“ an den Pfad an, um alle Dateien in den Unterordnern des ausgewählten Pfads einzubeziehen.
    * Fügen Sie „* */* .*“ an, um alle Daten im aktuellen Container und den zugehörigen Unterordnern einzubeziehen.
1. Geben Sie eine Beschreibung für das Dataset ein.
1. Wählen Sie **Weiter** aus.
1. Bestätigen Sie die Informationen. Wählen Sie **Zurück**, um Einstellungen zu ändern, oder **Erstellen**, um das Dataset zu erstellen.

> [!NOTE]
> Die von Ihnen ausgewählten Daten werden in Ihr Projekt geladen.  Daten, die dem Datenspeicher nach der Projekterstellung hinzugefügt werden, werden in diesem Projekt nicht angezeigt.  

### <a name="create-a-dataset-from-uploaded-data"></a>Erstellen eines Datasets aus hochgeladenen Daten

Direktes Hochladen Ihrer Daten:

1. Wählen Sie **Dataset erstellen** > **Aus lokalen Dateien** aus.
1. Weisen Sie Ihrem Dataset einen **Namen** zu.
1. Wählen Sie „Datei“ als **Dataset-Typ** aus.
1. *Optional:* Wählen Sie **Erweiterte Einstellungen** aus, um den Datenspeicher, den Container und den Pfad zu Ihren Daten anzupassen.
1. Wählen Sie **Durchsuchen** aus, um die lokalen Dateien zum Hochladen auszuwählen.
1. Geben Sie eine Beschreibung des Datasets ein.
1. Wählen Sie **Weiter** aus.
1. Bestätigen Sie die Informationen. Wählen Sie **Zurück**, um Einstellungen zu ändern, oder **Erstellen**, um das Dataset zu erstellen.

Die Daten werden in den Standardblobspeicher („workspaceblobstore“) Ihres Machine Learning-Arbeitsbereichs hochgeladen.

## <a name="specify-label-classes"></a>Angeben von Beschriftungsklassen

Auf der Seite **Beschriftungsklassen** geben Sie den Satz von Klassen zum Kategorisieren Ihrer Daten an. Gehen Sie dabei vorsichtig vor, denn die Genauigkeit und Geschwindigkeit Ihrer Beschriftungsersteller wird durch die Möglichkeit der Auswahl zwischen den Klassen beeinflusst. Beispiel: Anstatt Gattung und Art von Pflanzen oder Tieren vollständig anzugeben, wird empfohlen, einen Feldcode zu verwenden oder die Gattung abzukürzen.

Geben Sie eine Beschriftung pro Zeile ein. Verwenden Sie die Schaltfläche **+** , um eine neue Zeile hinzuzufügen. Wenn Sie mehr als 3 oder 4, aber weniger als 10 Beschriftungen haben, können Sie den Namen Zahlen („1: “, „2: “) voranstellen, damit die Beschriftungsersteller die Zifferntasten verwenden können, um ihre Arbeit zu beschleunigen.

## <a name="describe-the-labeling-task"></a>Beschreiben der Beschriftungsaufgabe

Es ist wichtig, die Beschriftungsaufgabe genau zu erläutern. Auf der Seite **Beschriftungsanweisungen** können Sie einen Link zu einer externen Website für Beschriftungsanweisungen hinzufügen. Sie können aber auch Anweisungen im Bearbeitungsfeld auf der Seite angeben. Stellen Sie Anweisungen bereit, die aufgabenorientiert und für die Zielgruppe geeignet sind. Stellen Sie sich die folgenden Fragen:

* Welche Beschriftungen sehen die Ersteller, und wie treffen sie ihre Auswahl? Gibt es einen Referenztext, auf den man sich beziehen kann?
* Was sollen sie tun, wenn keine Beschriftung geeignet erscheint?
* Was sollen sie tun, wenn mehrere Beschriftungen geeignet erscheinen?
* Welchen Konfidenzschwellenwert sollen sie einer Beschriftung zuweisen? Sollen sie „nach bestem Wissen und Gewissen“ beschriften, wenn sie nicht sicher sind?
* Was sollen sie tun, wenn interessante Objekte teilweise verdeckt sind oder sich überlappen?
* Was sollen sie tun, wenn ein interessantes Objekt am Bildrand abgeschnitten ist?
* Was sollen sie tun, nachdem sie eine Beschriftung eingereicht haben und glauben, dass sie einen Fehler gemacht haben?

Für Begrenzungsrahmen stellen sich folgende wichtige Fragen:

* Wie wird der Begrenzungsrahmen für diese Aufgabe definiert? Soll er vollständig im Inneren des Objekts befinden, oder soll er sich auf der Außenseite befinden? Soll er so weit wie möglich zugeschnitten werden, oder ist ein gewisser Spielraum akzeptabel?
* Welches Maß an Sorgfalt und Konsistenz erwarten Sie von den Beschriftungserstellern beim Definieren von Begrenzungsrahmen?
* Wie sollen Objekte beschriftet werden, die nur teilweise auf dem Bild zu sehen sind? 
* Wie sollen Objekte beschriftet werden, die teilweise von einem anderen Objekt verdeckt werden?

>[!NOTE]
> Beachten Sie, dass die Beschriftungsersteller die ersten 9 Beschriftungen mit den Zifferntasten 1–9 auswählen können.

## <a name="use-ml-assisted-labeling"></a>Verwenden der ML-gestützten Beschriftung

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Auf der Seite **Durch ML unterstützte Beschriftung** können Sie automatische Machine Learning-Modelle auslösen, um die Beschriftung zu beschleunigen. Zu Beginn Ihres Beschriftungsprojekts werden die Bilder in eine zufällige Reihenfolge gebracht, um potenzielle Verzerrungen zu verringern. Im Dataset enthaltene Verzerrungen fließen jedoch in das trainierte Modell ein. Wenn es sich also beispielsweise bei 80 Prozent der Bilder um eine einzelne Klasse handelt, gehören ungefähr 80 Prozent der Daten, die zum Trainieren des Modells verwendet werden, zu dieser Klasse. Dieses Training beinhaltet kein aktives Lernen.

Dieses Feature steht für die Bildklassifizierung (mehrere Klassen oder mehrere Beschriftungen) zur Verfügung.  

Wählen Sie *ML-gestützte Beschriftung aktivieren* aus, und geben Sie eine GPU an, um die Beschriftungsunterstützung zu aktivieren. Diese umfasst zwei Phasen:
* Clustering
* Vorabbeschriftung

Die genaue Anzahl beschrifteter Bilder, die zum Starten Beschriftungsunterstützung erforderlich sind, ist kein fester Wert.  Er kann zwischen Beschriftungsprojekten stark variieren. Bei manchen Projekten werden Clustering- oder Vorabbeschriftungsaufgaben nach der manuellen Beschriftung von 300 Bildern angezeigt. Bei der ML-gestützten Beschriftung kommt ein Verfahren namens *Transferlernen* zum Einsatz. Hierbei wird ein vorab trainiertes Modell verwendet, um den Trainingsprozess zu beschleunigen. Wenn die Klassen Ihres Datasets den Klassen des vorab trainierten Modells ähneln, stehen Vorabbeschriftungen ggf. bereits nach wenigen hundert manuell beschrifteten Bildern zur Verfügung. Wenn sich Ihr Dataset erheblich von den Daten unterscheidet, mit denen das Modell vorab trainiert wurde, dauert es unter Umständen deutlich länger.

Da die abschließenden Beschriftungen weiterhin von den Eingaben des Beschriftungserstellers abhängig sind, wird diese Technologie manchmal auch als *Human-in-the-Loop*-Beschriftung bezeichnet.

### <a name="clustering"></a>Clustering

Nachdem eine gewisse Anzahl von Beschriftungen übermittelt wurde, beginnt das Machine Learning-Modell damit, ähnliche Bilder zu gruppieren.  Diese ähnlichen Bilder werden den Beschriftungserstellern im gleichen Bildschirm angezeigt, um das manuelle Taggen zu beschleunigen. Das Clustering ist besonders hilfreich, wenn der Beschriftungsersteller ein Raster mit vier, sechs oder neun Bildern verwendet. 

Nachdem ein Machine Learning-Modell mit Ihren manuell beschrifteten Daten trainiert wurde, wird das Modell auf die letzte vollständig verbundene Ebene gekürzt. Nicht beschriftete Bilder durchlaufen dann das gekürzte Modell in einem Prozess, der in der Regel als Einbettung oder Merkmalserstellung bezeichnet wird. Dadurch werden die einzelnen Bilder jeweils in einen hochdimensionalen, durch diese Modellebene definierten Raum eingebettet. Bilder mit Pixelwiederholung in dem Raum werden für Clusteringaufgaben verwendet. 

### <a name="prelabeling"></a>Vorabbeschriftung

Nach der Übermittlung weiterer Bildbeschriftungen wird ein Klassifizierungsmodell verwendet, um Bildtags vorherzusagen.  Dem Beschriftungsersteller werden nun Seiten angezeigt, auf denen bereits vorhergesagte Beschriftungen für die einzelnen Bilder vorhanden sind.  Diese Bezeichnungen müssen überprüft und falsch beschriftete Bilder korrigiert werden, bevor die Seite übermittelt wird.  

Nachdem ein Machine Learning-Modell mit Ihren manuell beschrifteten Daten trainiert wurde, wird es anhand eines Testsatzes manuell beschrifteter Bilder ausgewertet, um seine Genauigkeit mit verschiedensten Zuverlässigkeitsschwellenwerten zu bestimmen. Diese Auswertung dient zur Ermittlung eines Zuverlässigkeitsschwellenwerts, über dem das Modell genau genug ist, um Vorabbeschriftungen anzuzeigen. Anschließend wird das Modell anhand von nicht beschrifteten Daten ausgewertet. Bilder, bei denen die Vorhersagezuverlässigkeit über dem Schwellenwert liegt, werden für die Vorabbeschriftung verwendet.

> [!NOTE]
> Die ML-gestützte Beschriftung steht **ausschließlich** in Arbeitsbereichen der Enterprise Edition zur Verfügung.

## <a name="initialize-the-labeling-project"></a>Initialisieren des Beschriftungsprojekts

Nach dem Initialisieren das Beschriftungsprojekts sind einige Aspekte des Projekts unveränderlich. Sie können den Aufgabentyp oder das Dataset nicht ändern. Beschriftungen sowie die URL für die Aufgabenbeschreibung *können* dagegen geändert werden. Überprüfen Sie die Einstellungen sorgfältig, bevor Sie das Projekt erstellen. Nachdem Sie das Projekt übermittelt haben, gelangen Sie wieder zur Startseite von **Datenbeschriftung**, wo das Projekt mit dem Status **Initialisierung** angezeigt wird. Diese Seite wird nicht automatisch aktualisiert. Aktualisieren Sie die Seite nach einer Pause manuell, um den Status des Projekts als **Erstellt** anzuzeigen.

## <a name="manage-teams-and-people"></a>Verwalten von Teams und Personen

Standardmäßig wird für jedes Beschriftungsprojekt, das Sie erstellen, ein neues Team mit Ihnen als Mitglied angezeigt. Teams können aber auch von Projekten gemeinsam genutzt werden. Und Projekte können über mehrere Teams verfügen. Zum Erstellen eines Teams wählen Sie auf der Seite **Teams** die Option **Team hinzufügen** aus.

Personen werden auf der Seite **Personen** verwaltet. Personen werden per E-Mail-Adresse hinzugefügt und entfernt. Jeder Beschriftungsersteller muss sich mit Ihrem Microsoft-Konto oder über Azure Active Directory authentifizieren, falls Sie es verwenden.  

Nachdem Sie eine Person hinzugefügt haben, können Sie diese einem oder mehreren Teams zuweisen: Wechseln Sie zur Seite **Teams**, wählen Sie das Team aus, und wählen Sie dann **Personen zuweisen** oder **Personen entfernen** aus.

Zum Senden einer E-Mail an das Team wählen Sie das Team aus, um die Seite **Details zum Team** anzuzeigen. Wählen Sie auf dieser Seite **E-Mail an Team** aus, um einen E-Mail-Entwurf mit den Adressen aller Personen im Team zu öffnen.

## <a name="run-and-monitor-the-project"></a>Ausführen und Überwachen des Projekts

Sobald das Projekt initialisiert wurde, startet Azure die Ausführung. Wählen Sie das Projekt auf der Hauptseite von **Datenbeschriftung** aus, um zu **Projektdetails** zu gelangen. Auf der Registerkarte **Dashboard** wird der Fortschritt der Beschriftungsaufgabe angezeigt.

Auf der Registerkarte**Daten** können Sie Ihr Dataset anzeigen und beschriftete Daten überprüfen. Wenn Sie falsch beschriftete Daten sehen, können Sie diese auswählen und auf **Ablehnen** klicken. Dadurch werden die Beschriftungen entfernt und die Daten wieder in die Warteschlange der unbeschrifteten Daten eingereiht.

Auf der Registerkarte **Team** können Sie dem Projekt Teams zuweisen oder eine Zuweisung aufheben.

Um das Projekt anzuhalten oder neu zu starten, wählen Sie die Schaltfläche **Anhalten**/**Starten** aus. Sie können Daten nur dann beschriften, wenn das Projekt ausgeführt wird.

Sie können Daten direkt über die Seite **Projektdetails** beschriften, indem Sie **Daten beschriften** auswählen.

## <a name="add-new-label-class-to-a-project"></a>Hinzufügen einer neuen Beschriftungsklasse zu einem Projekt

Im Rahmen des Beschriftungsprozesses stellen Sie möglicherweise fest, dass Sie weitere Beschriftungen benötigen, um Ihre Bilder zu klassifizieren.  So kann es beispielsweise sein, dass Sie für verwirrende Bilder Beschriftungen wie „Unbekannt“ oder „Sonstiges“ hinzufügen möchten.

Gehen Sie wie folgt vor, um einem Projekt Beschriftungen hinzuzufügen:

1. Wählen Sie das Projekt auf der Hauptseite von **Datenbeschriftung** aus.
1. Wählen Sie oben auf der Seite die Option **Anhalten** aus, um die Aktivität der Beschriftungsersteller zu beenden.
1. Wählen Sie die Registerkarte **Details** aus.
1. Wählen Sie in der Liste auf der linken Seite die Option **Beschriftungsklassen** aus.
1. Wählen Sie im oberen Bereich der Liste **+ Beschriftungen hinzufügen** aus. ![Hinzufügen einer Beschriftung](media/how-to-create-labeling-projects/add-label.png)
1. Fügen Sie im Formular Ihre neue Beschriftung hinzu, und wählen Sie aus, wie Sie fortfahren möchten.  Da Sie die verfügbaren Beschriftungen für ein Bild geändert haben, wählen Sie aus, wie mit den bereits beschrifteten Daten verfahren werden soll:
    * „Von vorn beginnen und alle vorhandenen Beschriftungen entfernen.“  Wählen Sie diese Option aus, wenn Sie mit der Beschriftung von vorn beginnen und dabei den neuen vollständigen Beschriftungssatz verwenden möchten. 
    * „Von vorn beginnen und alle vorhandenen Beschriftungen beibehalten.“  Wählen Sie diese Option aus, um alle Daten als nicht beschriftet zu markieren, die vorhandenen Beschriftungen aber als Standardtag für Bilder beizubehalten, die bereits beschriftet wurden.
    * „Fortfahren und alle vorhandenen Beschriftungen beibehalten.“ Wählen Sie diese Option aus, um die bereits beschrifteten Daten unverändert zu lassen und von nun an die neue Beschriftung für noch nicht beschriftete Daten verwenden zu können.
1. Passen Sie Ihre Anweisungsseite nach Bedarf für die neuen Beschriftungen an.
1. Wählen Sie nach dem Hinzufügen aller neuen Beschriftungen oben auf der Seite die Option **Starten** aus, um das Projekt wieder zu starten.  

## <a name="export-the-labels"></a>Exportieren der Beschriftungen

Sie können die Beschriftungsdaten für Machine Learning-Experimente jederzeit exportieren. Bildbeschriftungen können im [COCO-Format](http://cocodataset.org/#format-data) oder als Azure Machine Learning-Dataset exportiert werden. Verwenden Sie die Schaltfläche **Exportieren** auf der Seite **Projektdetails** Ihres Beschriftungsprojekts.

Die COCO-Datei wird im Standardblobspeicher des Azure Machine Learning-Arbeitsbereichs in einem Ordner unter *export/coco* erstellt. Sie können im Abschnitt **Datasets** von Machine Learning auf das exportierte Azure Machine Learning-Dataset zugreifen. Die Seite mit Datasetdetails bietet auch Beispielcode für den Zugriff auf Ihre Beschriftungen aus Python.

![Exportiertes Dataset](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Nächste Schritte

* Beschriften von Bildern für die [Bildklassifizierung oder Objekterkennung](how-to-label-images.md)
* Weitere Informationen zu [Azure Machine Learning und Machine Learning Studio (klassisch)](compare-azure-ml-to-studio-classic.md)
