---
title: Taggen von Bildern in einem Bezeichnungsprojekt
title.suffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie die Datentaggingtools in einem Azure Machine Learning-Bezeichnungsprojekt verwenden.
author: sdgilley
ms.author: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/09/2020
ms.openlocfilehash: 90ef75c704459ca726480e6950ba748162c66b8e
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147017"
---
# <a name="tag-images-in-a-labeling-project"></a>Taggen von Bildern in einem Bezeichnungsprojekt 

Nachdem Ihr Projektadministrator ein [Bezeichnungsprojekt](https://docs.microsoft.com/azure/machine-learning/how-to-create-labeling-projects#create-a-labeling-project) in Azure Machine Learning erstellt hat, können Sie mit dem Beschriftungstool (öffentliche Vorschau) schnell Daten für ein Machine Learning-Projekt vorbereiten. Dieser Artikel beschreibt Folgendes:

> [!div class="checklist"]
> * Zugreifen auf Ihre Bezeichnungsprojekte
> * Die Bezeichnungstools
> * Verwenden der Tools für bestimmte Bezeichnungsaufgaben

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Microsoft-Konto](https://account.microsoft.com/account) oder ein Azure Active Directory-Konto für die Organisation und das Projekt
* Zugriffsebene „Mitwirkender“ für den Arbeitsbereich, der das Bezeichnungsprojekt enthält

## <a name="sign-in-to-the-workspace"></a>Anmelden beim Arbeitsbereich

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. Wählen Sie das Abonnement und den Arbeitsbereich mit dem Bezeichnungsprojekt aus.  Diese Informationen erhalten Sie vom Projektadministrator.

1. Wählen Sie auf der linken Seite **Datenbeschriftung** aus, um nach dem Projekt zu suchen.  

1. Wählen Sie in der Liste den Projektnamen aus.

## <a name="understand-the-labeling-task"></a>Grundlegendes zur Bezeichnungsaufgabe

Nachdem Sie das Projekt ausgewählt haben, wählen Sie oben auf der Seite die Option **Daten beschriften** aus.

Es werden projektspezifische Anweisungen angezeigt. Sie enthalten relevante Informationen wie etwa die Art der vorliegenden Daten und Hinweise zum Treffen von Entscheidungen. Lesen Sie diese Informationen, und wählen Sie anschließend oben auf der Seite **Aufgaben** aus.  Stattdessen können Sie auch am unteren Rand der Seite auf **Beschriftung starten** klicken.

## <a name="common-features-of-the-labeling-task"></a>Allgemeine Features der Bezeichnungsaufgabe

Bei Bildbezeichnungsaufgaben müssen jeweils geeignete Tags aus einer vom Projektadministrator angegebenen Gruppe ausgewählt werden. Die ersten neun Tags können über die Zifferntasten auf der Tastatur ausgewählt werden.  

Bei Bildklassifizierungsaufgaben können Sie auswählen, dass mehrere Bilder gleichzeitig angezeigt werden sollen. Verwenden Sie die Symbole über dem Bildbereich, um das Layout auszuwählen. 

Wenn Sie alle angezeigten Bilder gleichzeitig auswählen möchten, verwenden Sie **Alle auswählen**. Wenn Sie einzelne Bilder auswählen möchten, verwenden Sie die runde Auswahlschaltfläche in der rechten oberen Ecke des Bilds. Sie müssen mindestens ein Bild auswählen, um ein Tag anzuwenden. Wenn Sie mehrere Bilder auswählen, wird jedes Tag, das Sie auswählen, auf alle ausgewählten Bilder angewendet.

Hier haben wir ein 2 × 2-Layout ausgewählt und sind im Begriff, das Tag „Mammal“ (Säugetier) auf die Bilder mit dem Bären und dem Orca anzuwenden. Das Bild mit dem Hai wurde bereits als „Cartilaginous fish“ (Knorpelfisch) markiert, und der Leguan wurde noch nicht markiert.

![Layouts mit mehreren Bildern und Auswahl](./media/how-to-label-images/layouts.png)

> [!Important] 
> Wechseln Sie das Layout nur, wenn Sie über eine neue Seite mit Daten ohne Bezeichnung verfügen. Durch das Wechseln von Layouts werden die bereits ausgeführten Bezeichnungen der Seite gelöscht.

Azure aktiviert die Schaltfläche **Senden**, wenn Sie alle Bilder auf der Seite markiert haben. Wählen Sie **Übermitteln** aus, um Ihre Arbeit zu speichern.

Nach der Übermittlung von Tags für die vorliegenden Daten aktualisiert Azure die Seite mit einer neuen Gruppe von Bildern aus der Arbeitswarteschlange.

### <a name="assisted-machine-learning-preview"></a>Unterstütztes maschinelles Lernen (Vorschauversion) 

Möglicherweise werden Machine Learning-Algorithmen ausgelöst. Wenn diese Algorithmen in Ihrem Projekt aktiviert sind, wird ggf. Folgendes angezeigt:

* Nach der Kennzeichnung einiger Bilder wird am oberen Bildschirmrand neben dem Projektnamen unter Umständen **Gruppierte Aufgaben** angezeigt.  Das bedeutet, dass Bilder gruppiert werden, um ähnliche Bilder auf der gleichen Seite darzustellen.  Wechseln Sie in diesem Fall zu einer der Ansichten mit mehreren Bildern, um von der Gruppierung zu profitieren.  

* Später wird neben dem Projektnamen unter Umständen **Vorab beschriftete Aufgaben** angezeigt.  Bilder werden dann mit einer von einem Machine Learning-Klassifizierungsmodell vorgeschlagenen Bezeichnung angezeigt. Machine Learning-Modelle sind niemals hundertprozentig genau. Es werden zwar nur Bilder verwendet, bei denen das Modell eine hohe Zuverlässigkeit ermittelt hat, trotzdem kann es vorkommen, dass diese Bilder nicht korrekt gekennzeichnet sind.  Wenn diese Bezeichnungen angezeigt werden, korrigieren Sie falsche Bezeichnungen, bevor Sie die Seite übermitteln.  

* Bei Objekterkennungsmodellen sind Begrenzungsrahmen und Bezeichnungen möglicherweise bereits vorhanden.  Korrigieren Sie alle Fehler, bevor Sie die Seite senden.

Insbesondere in der Anfangsphase eines Bezeichnungsprojekts ist die Genauigkeit des Machine Learning-Modells möglicherweise nur für die Kennzeichnung einer kleinen Teilmenge von Bildern ausreichend. Sobald diese Bilder gekennzeichnet wurden, kehrt das Bezeichnungsprojekt zur manuellen Kennzeichnung zurück, um mehr Daten für das nächste Modelltraining zu erfassen. Im Laufe der Zeit erhöht sich die Zuverlässigkeit des Modells für einen größeren Teil von Bildern, was im weiteren Projektverlauf zu mehr Vorabbeschriftungsaufgaben führt.

## <a name="tag-images-for-multi-class-classification"></a>Taggen von Bildern für die Klassifizierung mit mehreren Klassen

Wenn Ihr Projekt vom Typ „Bildklassifizierung mit mehreren Klassen“ ist, weisen Sie dem gesamten Bild ein einzelnes Tag zu. Die Anweisungen stehen jederzeit auf der Seite **Anweisungen** unter **Detaillierte Anweisungen anzeigen** zur Verfügung.

Fehler bei der Tagzuweisung zu einem Bild können nachträglich korrigiert werden. Wählen Sie das Symbol **X** für die unter dem Bild angezeigte Bezeichnung aus, um das Tag zu löschen. Alternativ können Sie das Bild und anschließend eine andere Klasse auswählen. In diesem Fall wird das zuvor angewendete Tag durch den neu ausgewählten Wert ersetzt.

## <a name="tag-images-for-multi-label-classification"></a>Taggen von Bildern für die Klassifizierung mit mehreren Beschriftungen

Wenn Sie an einem Projekt vom Typ „Bildklassifizierung mit mehreren Beschriftungen“ arbeiten, wenden Sie *mindestens ein* Tag auf ein Bild an. Wenn Sie die projektspezifischen Anweisungen anzeigen möchten, wählen Sie **Anweisungen** aus, und navigieren Sie zu **Detaillierte Anweisungen anzeigen**.

Wählen Sie das Bild aus, auf das Sie eine Bezeichnung anwenden möchten, und wählen Sie anschließend das Tag aus. Das-Tag wird auf alle ausgewählten Bilder angewendet, und die Auswahl der Bilder wird aufgehoben. Wenn Sie weitere Tags anwenden möchten, müssen Sie die Bilder erneut auswählen. Die folgende Animation zeigt das Taggen mit mehreren Bezeichnungen:

1. **Alle auswählen** wird zum Anwenden des Tags „Ocean“ (Meer) verwendet.
1. Ein einzelnes Bild wird ausgewählt und als „Closeup“ (Nahaufnahme) markiert.
1. Drei Bilder werden ausgewählt und als „Wide Angle“ (Weitwinkel) markiert.

![Animation mit dem Flow für mehrere Bezeichnungen](./media/how-to-label-images/multilabel.gif)

Wenn Sie einen Fehler korrigieren möchten, klicken Sie auf das Symbol **X**, um ein einzelnes Tag zu löschen, oder wählen Sie die Bilder und anschließend das Tag aus, um es aus allen ausgewählten Bildern zu löschen. Dieses Szenario wird hier gezeigt. Durch Klicken auf „Land/Region“ wird dieses Tag aus den beiden ausgewählten Bildern gelöscht.

![Screenshot: Mehrere Markierungsaufhebungen](./media/how-to-label-images/multiple-deselection.png)

Azure aktiviert die Schaltfläche **Senden** erst, nachdem Sie mindestens ein Tag auf jedes Bild angewandt haben. Wählen Sie **Übermitteln** aus, um Ihre Arbeit zu speichern.


## <a name="tag-images-and-specify-bounding-boxes-for-object-detection"></a>Taggen von Bildern und Angeben von Begrenzungsrahmen für die Objekterkennung

Bei einem Projekt vom Typ „Object Identification (Bounding Boxes)“ (Objektidentifizierung (Begrenzungsrahmen)) geben Sie Begrenzungsrahmen im Bild an und wenden jeweils ein Tag auf die einzelnen Rahmen an. Bilder können über mehrere Begrenzungsrahmen mit jeweils einem einzelnen Tag verfügen. Verwenden Sie **Detaillierte Anweisungen anzeigen**, um zu ermitteln, ob in Ihrem Projekt mehrere Begrenzungsrahmen verwendet werden.

1. Wählen Sie ein Tag für den Begrenzungsrahmen aus, den Sie erstellen möchten.
1. Wählen Sie das **Tool für rechteckige Felder**![Tool für rechteckige Felder](./media/how-to-label-images/rectangular-box-tool.png) aus, oder drücken Sie „R“.
3. Klicken Sie auf Ihr Ziel, und ziehen Sie diagonal, um einen ungefähren Begrenzungsrahmen zu erstellen. Ziehen Sie die Ränder oder Ecken, um das Begrenzungsfeld anzupassen.

![Screenshot: Erstellung eines einfachen Begrenzungsrahmens](./media/how-to-label-images/bounding-box-sequence.png)

Wenn Sie einen Begrenzungsrahmen löschen möchten, klicken Sie auf das X-förmige Ziel, das nach der Erstellung neben dem Begrenzungsrahmen angezeigt wird.

Das Tag eines vorhandenen Begrenzungsrahmens kann nicht geändert werden. Sollte Ihnen bei der Tagzuweisung ein Fehler unterlaufen sein, müssen Sie den Begrenzungsrahmen löschen und einen neuen Begrenzungsrahmen mit dem richtigen Tag erstellen.

Vorhandene Begrenzungsrahmen können standardmäßig bearbeitet werden. Das Tool zum **Sperren/Entsperren von Bereichen** (![Tool zum Sperren/Entsperren von Bereichen](./media/how-to-label-images/lock-bounding-boxes-tool.png)) oder „L“ schaltet dieses Verhalten um. Wenn Bereiche gesperrt sind, können Sie nur die Form oder Position eines neuen Begrenzungsrahmens ändern.

Verwenden Sie das Tool zum **Bearbeiten von Bereichen** (![Tool zum Bearbeiten von Bereichen](./media/how-to-label-images/regions-tool.png)) oder „M“, um ein vorhandenes Begrenzungsfeld anzupassen. Ziehen Sie die Ränder oder Ecken, um die Form anzupassen. Klicken Sie auf den Innenbereich, um den gesamten Begrenzungsrahmen verschieben zu können. Wenn Sie einen Bereich nicht bearbeiten können, haben Sie diese Funktionalität wahrscheinlich mit dem Tool zum **Sperren/Entsperren von Bereichen** umgeschaltet.

Verwenden Sie das Tool für **vorlagenbasierte Felder** (![Tool für vorlagenbasierte Felder](./media/how-to-label-images/template-box-tool.png)) oder „T“, um mehrere Begrenzungsrahmen derselben Größe zu erstellen. Wenn das Bild keinen Begrenzungsrahmen besitzt und Sie vorlagenbasierte Rahmen aktivieren, erzeugt das Tool Rahmen mit einer Größe von 50 × 50 Pixel. Wenn Sie einen Begrenzungsrahmen erstellt haben und anschließend vorlagenbasierte Rahmen aktivieren, haben neue Begrenzungsrahmen die Größe des zuletzt erstellten Rahmens. Die Größe von vorlagenbasierten Feldern kann nach der Platzierung angepasst werden. Beim Ändern der Größe eines vorlagenbasierten Rahmens wird nur die Größe dieses spezifischen Rahmens geändert.

Wenn Sie *alle* Begrenzungsrahmen im aktuellen Bild löschen möchten, wählen Sie das **Tool zum Löschen aller Regionen**![Tool zum Löschen aller Regionen](./media/how-to-label-images/delete-regions-tool.png) aus.

Wählen Sie nach Erstellung der Begrenzungsrahmen für ein Bild die Option **Übermitteln** aus, um Ihre Arbeit zu speichern. Andernfalls geht Ihre Arbeit verloren.

## <a name="finish-up"></a>Fertigstellen

Wenn Sie eine Seite mit markierten Daten übermitteln, weist Azure Ihnen neue Daten ohne Bezeichnung aus einer Arbeitswarteschlange zu. Sind keine Daten ohne Bezeichnung mehr vorhanden, erhalten Sie eine entsprechende Meldung mit einem Link zur Startseite des Portals.

Wenn Sie mit dem Anwenden von Bezeichnungen fertig sind, wählen Sie in der rechten oberen rechten Ecke des Bezeichnungsportals Ihren Namen und anschließend **Abmelden** aus. Wenn Sie sich nicht abmelden, wird in Azure zu einem späteren Zeitpunkt eine Zeitüberschreitung festgestellt, und die Daten werden einer anderen Person für die Bezeichnung zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Bildklassifizierungsmodelle in Azure trainieren](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml).

