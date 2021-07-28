---
title: Erstellen von Projekten und Hinzufügen von Videoclips mithilfe des Editors von Azure Video Analyzer for Media (früher Video Indexer)
titleSuffix: Azure Media Services
description: In diesem Thema wird veranschaulicht, mithilfe des Editors von Azure Video Analyzer for Media (früher Video Indexer) Projekte erstellt und Videoclips hinzugefügt werden.
services: media-services
author: Juliako
manager: femila
ms.topic: article
ms.date: 11/28/2020
ms.author: juliako
ms.openlocfilehash: a8e996465a35583db8584d41f6aeccd847ead0ff
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385610"
---
# <a name="add-video-clips-to-your-projects"></a>Hinzufügen von Videoclips zu Ihren Projekten

Die Website von [Azure Video Analyzer for Media (früher Video Indexer)](https://www.videoindexer.ai/) ermöglicht Ihnen, die umfassenden Erkenntnisse Ihres Videos für Folgendes zu nutzen: die richtigen Medieninhalte zu finden, die für Sie interessanten Teile zu finden und die Ergebnisse zum Erstellen eines völlig neuen Projekts zu nutzen. 

Nach der Erstellung kann das Projekt gerendert und von Video Analyzer for Media heruntergeladen sowie in eigenen Bearbeitungsanwendungen oder Downstream-Workflows verwendet werden.

Einige Szenarien, in denen Sie dieses Feature als hilfreich empfinden können, sind: 

* Erstellung von Filmhighlights für Trailer.
* Verwendung alter Clips von Videos in Nachrichtensendungen.
* Erstellung von kürzeren Inhalten für soziale Medien.

In diesem Artikel wird gezeigt, wie Sie ein Projekt erstellen und dem Projekt ausgewählte Clips aus den Videos hinzufügen können. 

## <a name="create-new-project-and-manage-videos"></a>Neues Projekt erstellen und Videos verwalten

1. Navigieren Sie zur [Video Analyzer for Media](https://www.videoindexer.ai/)-Website und melden Sie sich an.
1. Wählen Sie die Registerkarte **Projekte** aus. Wenn Sie bereits Projekte erstellt haben, werden hier alle Ihre anderen Projekte angezeigt.
1. Klicken Sie auf **Neues Projekt erstellen**.  

    :::image type="content" source="./media/video-indexer-view-edit/new-project.png" alt-text="Neues Projekt erstellen":::
1. Weisen Sie Ihrem Projekt einen Namen zu, indem Sie auf das Stiftsymbol klicken. Ersetzen Sie den Text „Unbenanntes Projekt“ durch Ihren Projektnamen, und klicken Sie auf das Häkchen.

    :::image type="content" source="./media/video-indexer-view-edit/new-project-edit-name.png" alt-text="Ein neues Projekt":::
    
### <a name="add-videos-to-the-project"></a>Videos zum Projekt hinzufügen

> [!NOTE]
> Derzeit dürfen Projekte nur Videos enthalten, die in derselben Sprache indiziert sind. </br>Sobald Sie ein Video in einer Sprache ausgewählt haben, können Sie die Videos in Ihrem Konto nicht mehr hinzufügen, die in einer anderen Sprache vorliegen. Die Videos in anderen Sprachen werden abgeblendet/deaktiviert.

1. Fügen Sie in diesem Projekt Videos hinzu, mit denen Sie arbeiten möchten, indem Sie **Videos hinzufügen** auswählen.

    Es werden alle Videos in Ihrem Konto und ein Suchfeld mit dem Text „Nach Text, Schlüsselwörtern oder sichtbarem Inhalt suchen“ angezeigt. Sie können Videos suchen, die eine bestimmte Person, eine bestimmtes Bezeichnung, eine bestimmte Marke, ein bestimmtes Schlüsselwort oder ein bestimmtes Vorkommen im Transkript und in der OCR enthalten.
    
    In der Abbildung unten suchten wir z. B. nach Videos, in denen „Custom Vision“ nur im Transkript erwähnt wird (verwenden Sie **Filter**, wenn Sie Ihre Suchergebnisse filtern möchten).
    
    :::image type="content" source="./media/video-indexer-view-edit/custom-vision.png" alt-text="Screenshot: Suche nach Videos, die „Custom Vision“ erwähnen":::
1. Klicken Sie auf **Hinzufügen**, um dem Projekt Videos hinzuzufügen.
1. Jetzt werden alle von Ihnen ausgewählten Videos angezeigt. Dies sind die Videos, aus denen Sie Clips für Ihr Projekt auswählen werden.

    Sie können die Reihenfolge der Videos durch Ziehen und Ablegen oder durch Auswählen der Listenmenüschaltfläche und anschließendem Auswählen von **Nach unten** oder **Nach oben** ändern. Über das Listenmenü können Sie auch das Video aus diesem Projekt entfernen. 
    
    Sie haben die Möglichkeit, jederzeit weitere Videos zu diesem Projekt hinzuzufügen, indem Sie **Videos hinzufügen** auswählen. Sie können auch mehrere Vorkommen desselben Videos zu Ihrem Projekt hinzufügen. Sie können diese Option wählen, wenn Sie einen Clip aus einem Video und dann einen Clip aus einem anderen Video sowie anschließend einen anderen Clip aus dem ersten Video anzeigen möchten. 

### <a name="select-clips-to-use-in-your-project"></a>Auswählen der in Ihrem Projekt zu verwendenden Clips

Wenn Sie auf der rechten Seite der einzelnen Videos auf den Pfeil nach unten klicken, öffnen Sie die Einblicke in das Video anhand von Zeitstempeln (Clips des Videos). 

1. Um Abfragen für bestimmte Clips zu erstellen, verwenden Sie das Suchfeld mit der Beschriftung „In Transkript, sichtbarem Text, Personen und Bezeichnungen suchen“.
1. Wählen Sie **Einblicke anzeigen** aus, um die anzuzeigenden Einblicke anzupassen. 

    :::image type="content" source="./media/video-indexer-view-edit/search-try-cognitive-services.png" alt-text="Screenshot: Suche nach Videos, die „Cognitive Services ausprobieren“ erwähnen":::
1. Fügen Sie Filter hinzu, um weitere Details zu den gesuchten Szenen festzulegen, indem Sie **Filteroptionen** auswählen.

    Sie können mehrere Filter hinzufügen. 
1. Wenn Sie mit Ihren Ergebnissen zufrieden sind, wählen Sie die Clips aus, die Sie zu diesem Projekt hinzufügen möchten, indem Sie das Segment auswählen, das Sie hinzufügen möchten. Sie können die Auswahl dieses Clips aufheben, indem Sie erneut auf das Segment klicken.
    
    Fügen Sie alle Segmente eines Videos hinzu (oder alle, die nach Ihrer Suche zurückgegeben wurden), indem Sie auf die Listenmenüoption neben dem Video klicken und **Alles auswählen** wählen. 

Während Sie Ihre Clips auswählen und sortieren, können Sie sich das Video im Player auf der rechten Seite der Seite ansehen. 

> [!IMPORTANT]
> Denken Sie daran, Ihr Projekt zu speichern, wenn Sie Änderungen vornehmen, indem Sie oben auf der Seite **Projekt speichern** auswählen. 

### <a name="render-and-download-the-project"></a>Rendern und Herunterladen des Projekts

> [!NOTE]
> Für kostenpflichtige Konten von Video Analyzer for Media verursacht das Rendering Ihres Projekts Kosten für die Codierung. Die Testkonten von Video Analyzer for Media sind auf fünf Stunden Rendering beschränkt.

1. Wenn Sie fertig sind, stellen Sie sicher, dass Ihr Projekt gespeichert wurde. Sie können dieses Projekt jetzt rendern. Wenn Sie auf **Rendern** klicken, wird ein Popupdialogfeld angezeigt, das Ihnen mitteilt, dass Video Analyzer for Media eine Datei rendern wird. Anschließend wird der Downloadlink an Ihre E-Mail-Adresse gesendet. Wählen Sie „Fortsetzen“ aus. 

    :::image type="content" source="./media/video-indexer-view-edit/render-download.png" alt-text="Screenshot: Video Analyzer for Media mit der Option zum Rendern und Herunterladen des Projekts":::
    
    Es wird auch oben auf der Seite eine Benachrichtigung angezeigt, dass das Projekt gerendert wird. Sobald das Rendering abgeschlossen ist, wird eine neue Benachrichtigung angezeigt, dass das Projekt erfolgreich gerendert wurde. Klicken Sie auf die Benachrichtigung, um das Projekt herunterzuladen. Dadurch wird das Projekt im MP4-Format heruntergeladen.
1. Sie können über die Registerkarte **Projekte** auf gespeicherte Projekte zugreifen. 

    Wenn Sie dieses Projekt auswählen, werden alle Einblicke und die Zeitachse dieses Projekts angezeigt. Wenn Sie **Video-Editor** auswählen, können Sie dieses Projekt weiter bearbeiten. Die Bearbeitung umfasst das Hinzufügen oder Entfernen von Videos und Clips oder das Umbenennen des Projekts.
    
## <a name="create-a-project-from-your-video"></a>Erstellen eines Projekts aus Ihrem Video

Sie können ein neues Projekt direkt aus einem Video in Ihrem Konto erstellen. 

1. Navigieren Sie auf der Video Analyzer for Media-Website zur Registerkarte **Bibliothek**.
1. Öffnen Sie das Video, das Sie zum Erstellen Ihres Projekts verwenden möchten. Wählen Sie auf der Seite mit Einblicken und der Zeitachse die Schaltfläche **Video-Editor** aus.

    Sie gelangen dadurch auf dieselbe Seite, über die Sie auch ein neues Projekt erstellt haben. Anders als beim neuen Projekt werden die mit Zeitstempeln versehenen Einblicksegmente des Videos angezeigt, mit deren Bearbeitung Sie zuvor begonnen haben.

## <a name="see-also"></a>Weitere Informationen

[Video Analyzer for Media: Übersicht](video-indexer-overview.md)

