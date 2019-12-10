---
title: Verwenden des Datenbezeichnungstools von Azure Machine Learning
title.suffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie die Datentaggingtools in einem Azure Machine Learning-Bezeichnungsprojekt verwenden.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: bc7eaeefcef042417b29eb7c887cc19acf8c5c02
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2019
ms.locfileid: "74688233"
---
# <a name="tag-images-in-a-labeling-project"></a>Taggen von Bildern in einem Bezeichnungsprojekt

Nachdem Ihr Projektadministrator ein Bezeichnungsprojekt in Azure Machine Learning erstellt hat, können Sie mit dem Bezeichnungstool schnell Daten für ein Machine Learning-Projekt vorbereiten. Dieser Artikel beschreibt Folgendes:

> [!div class="checklist"]
> * Zugreifen auf Ihre Bezeichnungsprojekte
> * Die Bezeichnungstools
> * Verwenden der Tools für bestimmte Bezeichnungsaufgaben

## <a name="prerequisites"></a>Voraussetzungen

* Die Bezeichnungsportal-URL für ein laufendes Datenbeschriftungsprojekt
* Ein [Microsoft-Konto](https://account.microsoft.com/account) oder ein Azure Active Directory-Konto für die Organisation und das Projekt

> [!NOTE]
> Der Projektadministrator findet die URL des Bezeichnungsportals auf der Registerkarte **Details** der Seite **Projektdetails**.

## <a name="sign-in-to-the-projects-labeling-portal"></a>Anmelden beim Bezeichnungsportal des Projekts

Navigieren Sie zu der vom Projektadministrator bereitgestellten Bezeichnungsportal-URL. Melden Sie sich mit dem E-Mail-Konto an, das der Administrator verwendet hat, um Sie dem Team hinzuzufügen. Bei den meisten Benutzern handelt es sich dabei um ihr Microsoft-Konto. Sollte für das Bezeichnungsprojekt Azure Active Directory verwendet werden, melden Sie sich darüber an.

## <a name="understand-the-labeling-task"></a>Grundlegendes zur Bezeichnungsaufgabe

Nach der Anmeldung wird die Übersichtsseite des Projekts angezeigt.

Navigieren Sie zu **Detaillierte Anweisungen anzeigen**. Diese Anweisungen sind projektspezifisch. Sie enthalten relevante Informationen wie etwa die Art der vorliegenden Daten und Hinweise zum Treffen von Entscheidungen. Nachdem Sie die Informationen gelesen haben, kehren Sie zur Projektseite zurück, und wählen Sie **Start labeling** (Bezeichnen beginnen) aus.

## <a name="common-features-of-the-labeling-task"></a>Allgemeine Features der Bezeichnungsaufgabe

Bei Bildbezeichnungsaufgaben müssen jeweils geeignete Tags aus einer vom Projektadministrator angegebenen Gruppe ausgewählt werden. Die ersten neun Tags können über die Zifferntasten auf der Tastatur ausgewählt werden.  

Bei Bildklassifizierungsaufgaben können Sie auswählen, dass mehrere Bilder gleichzeitig angezeigt werden sollen. Verwenden Sie die Symbole über dem Bildbereich, um das Layout auszuwählen. Wenn Sie alle angezeigten Bilder gleichzeitig auswählen möchten, verwenden Sie **Alle auswählen**. Wenn Sie einzelne Bilder auswählen möchten, verwenden Sie die runde Auswahlschaltfläche in der rechten oberen Ecke des Bilds. Sie müssen mindestens ein Bild auswählen, um ein Tag anzuwenden. Wenn Sie mehrere Bilder auswählen, wird jedes Tag, das Sie auswählen, auf alle ausgewählten Bilder angewendet.

Hier haben wir ein 2 × 2-Layout ausgewählt und sind im Begriff, das Tag „Mammal“ (Säugetier) auf die Bilder mit dem Bären und dem Orca anzuwenden. Das Bild mit dem Hai wurde bereits als „Cartilaginous fish“ (Knorpelfisch) markiert, und der Leguan wurde noch nicht markiert.

![Layouts mit mehreren Bildern und Auswahl](media/how-to-label-images/layouts.png)

> [!Important] 
> Wechseln Sie das Layout nur, wenn Sie über eine neue Seite mit Daten ohne Bezeichnung verfügen. Durch das Wechseln von Layouts werden die bereits ausgeführten Bezeichnungen der Seite gelöscht.

Azure aktiviert die Schaltfläche **Senden**, wenn Sie alle Bilder auf der Seite markiert haben. Wählen Sie **Übermitteln** aus, um Ihre Arbeit zu speichern.

Nach der Übermittlung von Tags für die vorliegenden Daten aktualisiert Azure die Seite mit einer neuen Gruppe von Bildern aus der Arbeitswarteschlange.

## <a name="tag-images-for-multi-class-classification"></a>Taggen von Bildern für die Klassifizierung mit mehreren Klassen

Wenn Ihr Projekt vom Typ „Bildklassifizierung mit mehreren Klassen“ ist, weisen Sie dem gesamten Bild ein einzelnes Tag zu. Die Anweisungen stehen jederzeit auf der Seite **Anweisungen** unter **Detaillierte Anweisungen anzeigen** zur Verfügung.

Fehler bei der Tagzuweisung zu einem Bild können nachträglich korrigiert werden. Wählen Sie das Symbol **X** für die unter dem Bild angezeigte Bezeichnung aus, um das Tag zu löschen. Alternativ können Sie das Bild und anschließend eine andere Klasse auswählen. In diesem Fall wird das zuvor angewendete Tag durch den neu ausgewählten Wert ersetzt.

## <a name="tag-images-for-multi-label-classification"></a>Taggen von Bildern für die Klassifizierung mit mehreren Beschriftungen

Wenn Sie an einem Projekt vom Typ „Bildklassifizierung mit mehreren Beschriftungen“ arbeiten, wenden Sie *mindestens ein* Tag auf ein Bild an. Wenn Sie die projektspezifischen Anweisungen anzeigen möchten, wählen Sie **Anweisungen** aus, und navigieren Sie zu **Detaillierte Anweisungen anzeigen**.

Wählen Sie das Bild aus, auf das Sie eine Bezeichnung anwenden möchten, und wählen Sie anschließend das Tag aus. Das-Tag wird auf alle ausgewählten Bilder angewendet, und die Auswahl der Bilder wird aufgehoben. Wenn Sie weitere Tags anwenden möchten, müssen Sie die Bilder erneut auswählen. Die folgende Animation zeigt das Taggen mit mehreren Bezeichnungen:

1. **Alle auswählen** wird zum Anwenden des Tags „Ocean“ (Meer) verwendet.
1. Ein einzelnes Bild wird ausgewählt und als „Closeup“ (Nahaufnahme) markiert.
1. Drei Bilder werden ausgewählt und als „Wide Angle“ (Weitwinkel) markiert.

![Animation mit dem Flow für mehrere Bezeichnungen](media/how-to-label-images/multilabel.gif)

Wenn Sie einen Fehler korrigieren möchten, klicken Sie auf das Symbol **X**, um ein einzelnes Tag zu löschen, oder wählen Sie die Bilder und anschließend das Tag aus, um es aus allen ausgewählten Bildern zu löschen. Dieses Szenario wird hier gezeigt. Durch Klicken auf „Land“ wird dieses Tag aus den beiden ausgewählten Bildern gelöscht.

![Screenshot: Mehrere Markierungsaufhebungen](media/how-to-label-images/multiple-deselection.png)

Azure aktiviert die Schaltfläche **Senden** erst, nachdem Sie mindestens ein Tag auf jedes Bild angewandt haben. Wählen Sie **Übermitteln** aus, um Ihre Arbeit zu speichern.

## <a name="tag-images-and-specify-bounding-boxes-for-object-detection"></a>Taggen von Bildern und Angeben von Begrenzungsrahmen für die Objekterkennung

Bei einem Projekt vom Typ „Object Identification (Bounding Boxes)“ (Objektidentifizierung (Begrenzungsrahmen)) geben Sie Begrenzungsrahmen im Bild an und wenden jeweils ein Tag auf die einzelnen Rahmen an. Bilder können über mehrere Begrenzungsrahmen mit jeweils einem einzelnen Tag verfügen. Verwenden Sie **Detaillierte Anweisungen anzeigen**, um zu ermitteln, ob in Ihrem Projekt mehrere Begrenzungsrahmen verwendet werden.

1. Wählen Sie ein Tag für den Begrenzungsrahmen aus, den Sie erstellen möchten.
1. Wählen Sie das **Tool für rechteckige Felder** ![Tool für rechteckige Felder](media/how-to-label-images/rectangular-box-tool.png) aus, oder drücken Sie „R“.
3. Klicken Sie auf Ihr Ziel, und ziehen Sie diagonal, um einen ungefähren Begrenzungsrahmen zu erstellen. Ziehen Sie die Ränder oder Ecken, um das Begrenzungsfeld anzupassen.

![Screenshot: Erstellung eines einfachen Begrenzungsrahmens](media/how-to-label-images/bounding-box-sequence.png)

Wenn Sie einen Begrenzungsrahmen löschen möchten, klicken Sie auf das X-förmige Ziel, das nach der Erstellung neben dem Begrenzungsrahmen angezeigt wird.

Das Tag eines vorhandenen Begrenzungsrahmens kann nicht geändert werden. Sollte Ihnen bei der Tagzuweisung ein Fehler unterlaufen sein, müssen Sie den Begrenzungsrahmen löschen und einen neuen Begrenzungsrahmen mit dem richtigen Tag erstellen.

Vorhandene Begrenzungsrahmen können standardmäßig bearbeitet werden. Das Tool zum **Sperren/Entsperren von Bereichen** (![Tool zum Sperren/Entsperren von Bereichen](media/how-to-label-images/lock-bounding-boxes-tool.png)) oder „L“ schaltet dieses Verhalten um. Wenn Bereiche gesperrt sind, können Sie nur die Form oder Position eines neuen Begrenzungsrahmens ändern.

Verwenden Sie das Tool zum **Bearbeiten von Bereichen** (![Tool zum Bearbeiten von Bereichen](media/how-to-label-images/regions-tool.png)) oder „M“, um ein vorhandenes Begrenzungsfeld anzupassen. Ziehen Sie die Ränder oder Ecken, um die Form anzupassen. Klicken Sie auf den Innenbereich, um den gesamten Begrenzungsrahmen verschieben zu können. Wenn Sie einen Bereich nicht bearbeiten können, haben Sie diese Funktionalität wahrscheinlich mit dem Tool zum **Sperren/Entsperren von Bereichen** umgeschaltet.

Verwenden Sie das Tool für **vorlagenbasierte Felder** (![Tool für vorlagenbasierte Felder](media/how-to-label-images/template-box-tool.png)) oder „T“, um mehrere Begrenzungsrahmen derselben Größe zu erstellen. Wenn das Bild keinen Begrenzungsrahmen besitzt und Sie vorlagenbasierte Rahmen aktivieren, erzeugt das Tool Rahmen mit einer Größe von 50 × 50 Pixel. Wenn Sie einen Begrenzungsrahmen erstellt haben und anschließend vorlagenbasierte Rahmen aktivieren, haben neue Begrenzungsrahmen die Größe des zuletzt erstellten Rahmens. Die Größe von vorlagenbasierten Feldern kann nach der Platzierung angepasst werden. Beim Ändern der Größe eines vorlagenbasierten Rahmens wird nur die Größe dieses spezifischen Rahmens geändert.

Wenn Sie *alle* Begrenzungsrahmen im aktuellen Bild löschen möchten, wählen Sie das **Tool zum Löschen aller Regionen** ![Tool zum Löschen aller Regionen](media/how-to-label-images/delete-regions-tool.png) aus.

Wählen Sie nach Erstellung der Begrenzungsrahmen für ein Bild die Option **Übermitteln** aus, um Ihre Arbeit zu speichern. Andernfalls geht Ihre Arbeit verloren.

## <a name="finish-up"></a>Fertigstellen

Wenn Sie eine Seite mit markierten Daten übermitteln, weist Azure Ihnen neue Daten ohne Bezeichnung aus einer Arbeitswarteschlange zu. Sind keine Daten ohne Bezeichnung mehr vorhanden, erhalten Sie eine entsprechende Meldung mit einem Link zur Startseite des Portals.

Wenn Sie mit dem Anwenden von Bezeichnungen fertig sind, wählen Sie in der rechten oberen rechten Ecke des Bezeichnungsportals Ihren Namen und anschließend **Abmelden** aus. Wenn Sie sich nicht abmelden, wird in Azure zu einem späteren Zeitpunkt eine Zeitüberschreitung festgestellt, und die Daten werden einer anderen Person für die Bezeichnung zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Bildklassifizierungsmodelle in Azure trainieren](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml).
* Informieren Sie sich über die [Erkennung von Objekten mit Azure und der schnelleren R-CNN-Technik](https://www.microsoft.com/developerblog/2017/10/24/bird-detection-with-azure-ml-workbench/).
