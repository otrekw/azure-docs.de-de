---
title: Erstellen eines Datenbeschriftungsprojekts
titleSuffix: Azure Machine Learning
description: In diesem Artikel wird gezeigt, wie Sie Beschriftungsprojekte zum Kennzeichnen von Daten für das maschinelle Lernen erstellen und ausführen.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 864cccc4629140754a326823cbaebd7ad8933d3d
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75765068"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Erstellen eines Datenbeschriftungsprojekts und Exportieren der Beschriftungen 

Das Beschriften großer Datenmengen in Projekten für maschinelles Lernen bereitet häufig Kopfzerbrechen. Projekte mit einer Komponente für maschinelles Sehen – z. B. zur Bildklassifizierung oder Objekterkennung – erfordern im Allgemeinen Beschriftungen für Tausende von Bildern.
 
[Azure Machine Learning](https://ml.azure.com/) ist Ihr zentraler Ort zum Erstellen, Verwalten und Überwachen von Beschriftungsprojekten. Verwenden Sie die Lösung zum Koordinieren von Daten, Beschriftungen und Teammitgliedern, um Beschriftungsaufgaben effizient zu verwalten. Machine Learning unterstützt Bildklassifizierung – mit mehreren Beschriftungen oder mehreren Klassen – und die Objektidentifikation mit Begrenzungsrahmen.

Machine Learning verfolgt den Fortschritt nach und verwaltet die Warteschlange mit unvollständigen Beschriftungsaufgaben. Beschriftungsersteller benötigen kein Azure-Konto für die Teilnahme. Nachdem sie mit Ihrem Microsoft-Konto oder über [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) authentifiziert sind, können sie sich Beschriftungsaufgaben widmen, so wie es ihre Zeit erlaubt.

In Machine Learning können Sie das Projekt starten und beenden, Personen und Teams hinzufügen und entfernen und den Fortschritt überwachen. Sie können beschriftete Daten im COCO-Format oder als Azure Machine Learning-Dataset exportieren.

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

* Die Daten, die beschriftet werden sollen und entweder in lokalen Dateien oder im Azure-Speicher vorhanden sind.
* Der Satz von Beschriftungen, die Sie anwenden möchten.
* Die Anweisungen für die Beschriftung.
* ein Azure-Abonnement Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen, bevor Sie beginnen.
* Ein Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Erstellen eines Beschriftungsprojekts

Beschriftungsprojekte werden in Azure Machine Learning verwaltet. Auf der Seite **Beschriftungsprojekte** verwalten Sie Projekte und Personen. Einem Projekt können ein oder mehrere Teams zugewiesen sein, und ein Team kann aus einem oder mehreren Personen bestehen.

Wenn sich Ihre Daten bereits in einem Azure-Blobspeicher befinden, sollten Sie diesen als Datenspeicher zur Verfügung stellen, bevor Sie das Beschriftungsprojekt erstellen. Ausführliche Informationen dazu finden Sie unter [Erstellen und Registrieren von Datenspeichern](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores).

Zum Erstellen eines Projekts wählen Sie **Projekt hinzufügen** aus. Geben Sie dem Projekt einen geeigneten Namen, und wählen Sie den **Beschriftungsaufgabentyp** aus.

![Assistent zum Erstellen von Beschriftungsprojekten](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Wählen Sie **Bildklassifizierung mit mehreren Beschriftungen** für Projekte aus, in denen *eine oder mehrere* Beschriftungen aus einer Reihe von Klassen auf ein Bild angewendet werden sollen. Ein Foto eines Hunds z. B. kann sowohl mit *Hund* als auch mit *Tag* beschriftet werden.
* Wählen Sie **Bildklassifizierung mit mehreren Klassen** für Projekte aus, in denen nur eine *einzige Klasse* aus einer Reihe von Klassen auf ein Bild angewendet werden soll.
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

Es ist wichtig, die Beschriftungsaufgabe genau zu erläutern. Auf der Seite **Beschriftungsanweisungen** können Sie einen Link zu einer externen Website für Beschriftungsanweisungen hinzufügen. Stellen Sie Anweisungen bereit, die aufgabenorientiert und für die Zielgruppe geeignet sind. Stellen Sie sich die folgenden Fragen:

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

>[!NOTE]
> Beachten Sie, dass die Beschriftungsersteller die ersten 9 Beschriftungen mit den Zifferntasten 1–9 auswählen können.

## <a name="initialize-the-labeling-project"></a>Initialisieren des Beschriftungsprojekts

Nach dem Initialisieren das Beschriftungsprojekts sind einige Aspekte des Projekts unveränderlich. Sie können den Aufgabentyp oder das Dataset nicht ändern. Beschriftungen sowie die URL für die Aufgabenbeschreibung *können* dagegen geändert werden. Überprüfen Sie die Einstellungen sorgfältig, bevor Sie das Projekt erstellen. Nachdem Sie das Projekt übermittelt haben, werden Sie wieder auf die Startseite für **Beschriftungen** geleitet, wo das Projekt als **Wird initialisiert** angezeigt wird. Diese Seite wird nicht automatisch aktualisiert. Aktualisieren Sie die Seite nach einer Pause manuell, um den Status des Projekts als **Erstellt** anzuzeigen.

## <a name="manage-teams-and-people"></a>Verwalten von Teams und Personen

Standardmäßig wird für jedes Beschriftungsprojekt, das Sie erstellen, ein neues Team mit Ihnen als Mitglied angezeigt. Teams können aber auch von Projekten gemeinsam genutzt werden. Und Projekte können über mehrere Teams verfügen. Zum Erstellen eines Teams wählen Sie auf der Seite **Teams** die Option **Team hinzufügen** aus.

Personen werden auf der Seite **Personen** verwaltet. Personen werden per E-Mail-Adresse hinzugefügt und entfernt. Jeder Beschriftungsersteller muss sich mit Ihrem Microsoft-Konto oder über Azure Active Directory authentifizieren, falls Sie es verwenden.  

Nachdem Sie eine Person hinzugefügt haben, können Sie diese einem oder mehreren Teams zuweisen: Wechseln Sie zur Seite **Teams**, wählen Sie das Team aus, und wählen Sie dann **Personen zuweisen** oder **Personen entfernen** aus.

Zum Senden einer E-Mail an das Team wählen Sie das Team aus, um die Seite **Details zum Team** anzuzeigen. Wählen Sie auf dieser Seite **E-Mail an Team** aus, um einen E-Mail-Entwurf mit den Adressen aller Personen im Team zu öffnen.

## <a name="run-and-monitor-the-project"></a>Ausführen und Überwachen des Projekts

Sobald das Projekt initialisiert wurde, startet Azure die Ausführung. Wählen Sie das Projekt auf der Hauptseite **Beschriftung**, um zu **Projektdetails** zu gelangen. Auf der Registerkarte **Dashboard** wird der Fortschritt der Beschriftungsaufgabe angezeigt.

Auf der Registerkarte**Daten** können Sie Ihr Dataset anzeigen und beschriftete Daten überprüfen. Wenn Sie falsch beschriftete Daten sehen, können Sie diese auswählen und auf **Ablehnen** klicken. Dadurch werden die Beschriftungen entfernt und die Daten wieder in die Warteschlange der unbeschrifteten Daten eingereiht.

Auf der Registerkarte **Team** können Sie dem Projekt Teams zuweisen oder eine Zuweisung aufheben.

Um das Projekt anzuhalten oder neu zu starten, wählen Sie die Schaltfläche **Anhalten**/**Starten** aus. Sie können Daten nur dann beschriften, wenn das Projekt ausgeführt wird.

Sie können Daten direkt über die Seite **Projektdetails** beschriften, indem Sie **Daten beschriften** auswählen.

## <a name="export-the-labels"></a>Exportieren der Beschriftungen

Sie können die Beschriftungsdaten für Machine Learning-Experimente jederzeit exportieren. Bildbeschriftungen können im [COCO-Format](http://cocodataset.org/#format-data) oder als Azure Machine Learning-Dataset exportiert werden. Verwenden Sie die Schaltfläche **Exportieren** auf der Seite **Projektdetails** Ihres Beschriftungsprojekts.

Die COCO-Datei wird im Standardblobspeicher des Azure Machine Learning-Arbeitsbereichs in einem Ordner unter *export/coco* erstellt. Sie können im Abschnitt **Datasets** von Machine Learning auf das exportierte Azure Machine Learning-Dataset zugreifen. Die Seite mit Datasetdetails bietet auch Beispielcode für den Zugriff auf Ihre Beschriftungen aus Python.

![Exportiertes Dataset](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Nächste Schritte

* Beschriften von Bildern für die [Bildklassifizierung oder Objekterkennung](how-to-label-images.md)
* Weitere Informationen zu [Azure Machine Learning und Machine Learning Studio (klassisch)](compare-azure-ml-to-studio-classic.md)
