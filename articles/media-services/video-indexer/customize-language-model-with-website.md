---
title: Anpassen eines Sprachmodells mit der Video Indexer-Website
titleSuffix: Azure Media Services
description: Erfahren Sie, wie Sie ein Sprachmodell mit der Video Indexer-Website anpassen können.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 08/10/2020
ms.author: kumud
ms.openlocfilehash: dd8b36340deb6c785989107461dd420e7fc0d985
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97722571"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Anpassen eines Sprachmodells mit der Video Indexer-Website

Mit Video Indexer können Sie benutzerdefinierte Sprachmodelle erstellen, um die Spracherkennung durch Hochladen von Anpassungstexten anzupassen, insbesondere von Texten aus dem Bereich, an dessen Vokabular sich die Engine anpassen soll. Sobald Sie Ihr Modell trainiert haben, werden neue Wörter, die im Anpassungstext vorkommen, erkannt.

Eine detaillierte Übersicht und bewährte Methoden für benutzerdefinierte Sprachmodelle finden Sie unter [Anpassen eines Sprachmodells mit Video Indexer](customize-language-model-overview.md).

Sie können die Video Indexer-Website verwenden, um benutzerdefinierte Sprachmodelle in Ihrem Konto zu erstellen und zu bearbeiten, wie in diesem Thema beschrieben. Sie können auch die API verwenden. Dies wird unter [Anpassen von Sprachmodellen mithilfe von APIs](customize-language-model-with-api.md) beschrieben.

## <a name="create-a-language-model"></a>Erstellen eines Sprachmodells

1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
1. Um ein Modell in Ihrem Konto anzupassen, wählen Sie links auf der Seite die Schaltfläche **Anpassung des Inhaltsmodells** aus.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/customize-language-model/model-customization.png" alt-text="Anpassen eines Inhaltsmodells in Video Indexer":::
1. Wählen Sie die Registerkarte **Sprache** aus.

    Eine Liste der unterstützten Sprachen wird angezeigt.
1. Nach dem Auswählen der gewünschten Sprache wählen Sie **Modell hinzufügen** aus.
1. Geben Sie den Namen für das Sprachmodell ein, und drücken Sie die Eingabetaste.

    Damit wird das Modell erstellt, und das Hochladen von Textdateien in das Modell wird möglich.
1. Wählen Sie **Datei hinzufügen** aus, um eine Textdatei hinzuzufügen. Ihr Datei-Explorer wird geöffnet.
1. Navigieren Sie zur Textdatei, und wählen Sie diese aus. Sie können mehrere Textdateien einem Sprachmodell hinzufügen.

    Sie können auch eine Textdatei hinzufügen, indem Sie die Schaltfläche **...** auf der rechten Seite des Sprachmodells und **Datei hinzufügen** auswählen.
1. Nachdem Sie die Textdateien hochgeladen haben, wählen Sie die grüne Option **Trainieren** aus.

Der Trainingsprozess kann einige Minuten dauern. Wenn der Trainingsprozess abgeschlossen ist, wird **Trained** (Trainiert) neben dem Modell angezeigt. Sie können für die Datei eine Vorschau anzeigen, sie herunterladen und aus dem Modell löschen.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/customize-language-model/customize-language-model.png" alt-text="Modelltraining":::

### <a name="using-a-language-model-on-a-new-video"></a>Verwenden eines Sprachmodells für ein neues Video

Um Ihr Sprachmodell für ein neues Video zu verwenden, führen Sie eine der folgenden Aktionen aus:

* Wählen Sie die Schaltfläche **Hochladen** oben auf der Seite aus.

    ![Schaltfläche „Hochladen“ im Video Indexer](./media/customize-language-model/upload.png)
* Legen Sie Ihre Audio- oder Videodatei ab, oder suchen Sie nach Ihrer Datei.

Sie können optional die **Ausgangssprache des Videos** auswählen. Wählen Sie die Dropdownliste und daraus ein von Ihnen erstelltes Sprachmodell aus. Ihnen sollten dann die Sprache Ihres Sprachmodells sowie der von Ihnen in Klammern festgelegte Name angezeigt werden. Beispiel:

![Auswählen der Ausgangssprache des Videos – Neuindizierung eines Videos mit Video Indexer](./media/customize-language-model/reindex.png)

Wählen Sie unten auf der Seite die Option **Hochladen** aus, und Ihr neues Video wird mithilfe des Sprachmodells indiziert.

### <a name="using-a-language-model-to-reindex"></a>Neuindizieren mithilfe eines Sprachmodells

Führen Sie die folgenden Schritte aus, um das Sprachmodell zum erneuten Indizieren eines Videos in Ihrer Sammlung zu verwenden:

1. Melden Sie sich bei der [Video Indexer](https://www.videoindexer.ai/)-Startseite an.
1. Klicken Sie im Video auf die **...** -Schaltfläche, und wählen Sie **Neu indizieren** aus.
1. Über die Option **Ausgangssprache des Videos** können Sie die Ausgangssprache des Videos auswählen, mit der Sie Ihr Video erneut indizieren möchten. Wählen Sie die Dropdownliste und daraus ein von Ihnen erstelltes Sprachmodell aus. Ihnen sollten dann die Sprache Ihres Sprachmodells sowie der von Ihnen in Klammern festgelegte Name angezeigt werden.
1. Wählen Sie die Schaltfläche **Neuindizieren** aus, und Ihr Video wird mithilfe Ihres Sprachmodells erneut indiziert.

## <a name="edit-a-language-model"></a>Bearbeiten eines Sprachmodells

Sie können ein Sprachmodell bearbeiten durch Ändern seines Namens oder durch Hinzufügen und Löschen von Dateien.

Wenn Sie dem Sprachmodell Dateien hinzufügen oder daraus löschen, müssen Sie das Modell neu trainieren, indem Sie die grüne Option **Trainieren** auswählen.

### <a name="rename-the-language-model"></a>Umbenennen eines Sprachmodells

Sie können den Namen des Sprachmodells ändern, indem Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) auf der rechten Seite des Sprachmodells und dann die Option **Umbenennen** auswählen.

Geben Sie den neuen Namen ein, und drücken Sie die Eingabetaste.

### <a name="add-files"></a>Dateien hinzufügen

Wählen Sie **Datei hinzufügen** aus, um eine Textdatei hinzuzufügen. Ihr Datei-Explorer wird geöffnet.

Navigieren Sie zur Textdatei, und wählen Sie diese aus. Sie können mehrere Textdateien einem Sprachmodell hinzufügen.

Sie können auch eine Textdatei hinzufügen, indem Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) auf der rechten Seite des Sprachmodells und **Datei hinzufügen** auswählen.

### <a name="delete-files"></a>Löschen von Dateien

Um eine Datei aus dem Sprachmodell zu löschen, wählen Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) auf der rechten Seite der Textdatei und **Löschen** aus. Ein neues Fenster mit der Mitteilung, dass die Löschung nicht rückgängig gemacht werden kann, wird geöffnet. Wählen Sie die Option **Löschen** in dem neuen Fenster aus.

Durch diese Aktion wird die Datei vollständig aus dem Sprachmodell entfernt.

## <a name="delete-a-language-model"></a>Löschen eines Sprachmodells

Um ein Sprachmodell aus Ihrem Konto zu löschen, wählen Sie die Schaltfläche mit den Auslassungspunkten ( **...** ) auf der rechten Seite des Sprachmodells und dann **Löschen** aus.

Dies öffnet ein neues Fenster mit der Mitteilung, dass die Löschung nicht rückgängig gemacht werden kann. Wählen Sie die Option **Löschen** in dem neuen Fenster aus.

Durch diese Aktion wird das Sprachmodell vollständig aus Ihrem Konto entfernt. Jedes Video, das das gelöschte Sprachmodell verwendet hat, behält den gleichen Index bei, bis Sie das Video erneut indizieren. Wenn Sie das Video erneut indizieren, können Sie dem Video ein neues Sprachmodell zuweisen. Andernfalls verwendet Video Indexer das Standardmodell, um das Video erneut zu indizieren.

## <a name="customize-language-models-by-correcting-transcripts"></a>Anpassen von Sprachmodellen durch Korrigieren von Transkripten

Video Indexer unterstützt die automatische Anpassung von Sprachmodellen basierend auf den tatsächlichen Korrekturen, die Benutzer an den Transkriptionen ihrer Videos vornehmen.

1. Um Korrekturen an einem Transkript vorzunehmen, öffnen Sie das Video, das Sie bearbeiten möchten, aus Ihren Kontovideos. Wählen Sie die Registerkarte **Zeitachse**.

    ![Anpassen der Registerkarte „Zeitachse“ eines Sprachmodells – Video Indexer](./media/customize-language-model/timeline.png)

1. Wählen Sie das Stiftsymbol aus, um das Transkript Ihrer Transkription zu bearbeiten.

    ![Anpassen der Transkriptbearbeitung des Sprachmodells – Video Indexer](./media/customize-language-model/edits.png)

    Video Indexer erfasst alle Zeilen, die von Ihnen in der Transkription Ihres Videos korrigiert werden, und fügt sie automatisch zu einer Textdatei namens „Aus Transkriptbearbeitungen“ hinzu. Diese Bearbeitungen werden verwendet, um das spezifische Sprachmodell, mit dem dieses Video indiziert wurde, neu zu trainieren. 
    
    Die Änderungen, die in der Zeitleiste des [Widgets](video-indexer-embed-widgets.md) vorgenommen wurden, sind ebenfalls enthalten.
    
    Wenn Sie bei der Indizierung dieses Videos kein Sprachmodell angegeben haben, werden alle Bearbeitungen für dieses Video in einem Standardsprachenmodell namens „Kontoanpassungen“ innerhalb der erkannten Sprache des Videos gespeichert.
    
    Falls mehrere Änderungen an derselben Zeile vorgenommen wurden, wird nur die letzte Version der korrigierten Zeile für die Aktualisierung des Sprachmodells verwendet.  
    
    > [!NOTE]
    > Für die Anpassung werden nur Textkorrekturen verwendet. Korrekturen, die keine eigentlichen Wörter beinhalten (z. B. Satzzeichen oder Leerzeichen), werden nicht berücksichtigt.
    
1. Transkriptkorrekturen werden auf der Registerkarte „Sprache“ auf der Seite „Anpassung des Inhaltsmodells“ angezeigt.

   Um die Datei „Aus Transkriptbearbeitungen“ für jedes Ihrer Sprachmodelle anzusehen, wählen Sie sie aus, um sie zu öffnen.

    ![Aus Transkriptbearbeitungen – Video Indexer](./media/customize-language-model/from-transcript-edits.png)

## <a name="next-steps"></a>Nächste Schritte

[Customize language model using APIs (Anpassen eines Sprachmodells mithilfe von APIs)](customize-language-model-with-api.md)
