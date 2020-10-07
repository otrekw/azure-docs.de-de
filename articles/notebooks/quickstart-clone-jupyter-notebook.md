---
title: Klonen eines Jupyter-Notebooks von GitHub mithilfe von Azure Notebooks (Vorschauversion)
description: Klonen Sie schnell ein Jupyter-Notebook aus einem GitHub-Repository, und führen Sie es in Ihrem Azure Notebooks-Konto aus.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 267e79e7d4bf108ac3b2c72d64cee5a07ba638be
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87424476"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Schnellstart: Klonen eines Notebooks in Azure Notebooks (Vorschauversion)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

In dieser Schnellstartanleitung kopieren Sie eine in GitHub gespeicherte Jupyter Notebook-Instanz in ein Azure Notebooks-Konto. 

GitHub-Repositorys bieten Speicherplatz und Versionskontrolle für Jupyter Notebook-Instanzen. Projektmitarbeiter verwalten lokale Kopien der Repositorys und führen die Notebooks über diese Kopien aus. Beim Klonen einer Jupyter Notebook-Instanz von GitHub in Ihrem Azure Notebooks-Konto wird eine unabhängige Kopie des Notebooks erstellt. Änderungen werden nur in Ihrem Azure Notebooks-Konto gespeichert und wirken sich nicht auf das ursprüngliche GitHub-Repository aus. 

Da Ihr Azure Notebooks-Klon in der Cloud gespeichert wird, können Sie ihn mit anderen Projektmitarbeitern teilen, die keine lokalen Kopien erstellen oder keine Jupyter-Installation auf den eigenen Computern besitzen müssen. Sie können ein Notebook auch einfach als Ausgangspunkt für ein eigenes Projekt oder zum Abrufen von Datendateien klonen. 

## <a name="prerequisites"></a>Voraussetzungen
Keine.

## <a name="clone-azure-cognitive-services-notebooks"></a>Klonen von Azure Cognitive Services-Notebooks

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com), und melden Sie sich an Ausführliche Informationen finden Sie unter [Schnellstart: Anmelden bei Azure Notebooks](quickstart-sign-in-azure-notebooks.md).

1. Wählen Sie oben auf Ihrer öffentlichen Profilseite **Meine Projekte** aus:

    ![Link „Meine Projekte“ am oberen Rand des Browserfensters](media/quickstarts/my-projects-link.png)

1. Wählen Sie auf der Seite **Meine Projekte** den Pfeil nach oben aus (Tastenkombination: U; die Schaltfläche wird als **Upload GitHub Repo** (GitHub-Repository hochladen) angezeigt, wenn das Browserfenster breit genug ist):

    ![Befehl „Upload GitHub Repo“ (GitHub-Repository hochladen) auf der Seite „Meine Projekte“](media/quickstarts/upload-github-repo-command.png)

1. Geben Sie im Dialogfeld **Upload GitHub Repository** (GitHub-Repository hochladen), das nun angezeigt wird, die folgenden Details ein, und wählen Sie dann **Import** (Importieren) aus:

   - **GitHub-Repository**: Microsoft/cognitive-services-notebooks (mit diesem Namen werden die Jupyter-Notebooks für Azure Cognitive Services unter [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks) geklont).
   - **Clone recursively** (Rekursiv klonen): (nicht aktiviert)
   - **Projektname**: Cognitive Services Clone (Cognitive Services-Klon)
   - **Project ID** (Projekt-ID): cognitive-services-clone (cognitive-services-klon)
   - **Public** (Öffentlich): (nicht aktiviert)

     ![Popupfenster „GitHub-Repository hochladen“ zur Erfassung von Repositoryinformationen](media/quickstarts/upload-github-repo-popup.png)

1. Haben Sie ein bisschen Geduld, bis der Prozess abgeschlossen ist; das Klonen eines Repositorys kann einige Minuten in Anspruch nehmen.

1. Nach dem Abschluss des Klonens führt Azure Notebooks Sie zum neuen Projekt, in dem Sie die Kopien aller Dateien sehen können.

    :::image type="content" source="media/quickstarts/completed-clone.png" alt-text="Ansicht eines fertig gestellten Klons" lightbox="media/quickstarts/completed-clone.png":::

## <a name="share-a-notebook"></a>Teilen eines Notebooks

1. Um Ihre Kopie des geklonten Projekts zu teilen, verwenden Sie das **Freigabe**-Steuerelement, rufen Sie einen Link oder HTML- oder Markupcode ab, der den Link enthält, oder erstellen Sie eine E-Mail-Nachricht mit dem Link:

    ![Projektbefehl „Freigabe“](media/quickstarts/share-project-command.png)

1. Da Sie beim Klonen des Projekts die Option **Public** (Öffentlich) deaktiviert haben, ist der Klon privat. Um Ihre Kopie öffentlich zu machen, wählen Sie **Project Settings** (Projekteinstellungen) aus, legen Sie die Option **Public project** (Öffentliches Projekt) im Popup fest, und wählen Sie dann **Save** (Speichern) aus.

1. Wählen Sie ein Notebook im Projekt aus, um es auszuführen. Beispielsweise stellt jedes Notebook im Azure Cognitive Services-Repository seinen eigenen, eigenständigen Schnellstart dar. Die Abbildung unten zeigt das Ergebnis der Verwendung des BingImageSearchAPI-Notebooks nach dem Hinzufügen eines Cognitive Services API-Abonnementschlüssels und dem Ändern des Suchbegriffs „puppies“ (Welpen) in „bunnies“ (Häschen) dar:

    ![Ausführen eines von GitHub geklonten Jupyter-Notebooks](media/quickstarts/clone-notebook-result.png)

1. Wenn Sie die Ausführung des Notebooks abgeschlossen haben, wählen Sie **File** (Datei)  > **Close and halt** (Schließen und beenden) aus, um das Notebook und sein Browserfenster zu schließen.

1. Um ein einzelnes Notebook im Projekt zu teilen, klicken Sie mit der rechten Maustaste auf das Notebook, und wählen Sie **Copy link** (Link kopieren; Tastenkombination: y) aus:

    ![Kontextmenübefehl zum Kopieren eines Links zu einem einzelnen Notebook](media/quickstarts/copy-link-to-individual-notebook.png)

1. Um andere Dateien als Notebooks zu bearbeiten, klicken Sie mit der rechten Maustaste auf die Datei im Projekt, und wählen Sie **Edit file** (Datei bearbeiten; Tastenkombination: i) aus. Mit der Standardaktion **Run** (Ausführen; Tastenkombination: r) wird nur der Dateiinhalt angezeigt, Bearbeiten ist nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Ausführen einer Jupyter Notebook-Instanz für lineare Regression](tutorial-create-run-jupyter-notebook.md)
