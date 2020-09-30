---
title: Schritte zum Importieren von Apps
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/22/2020
ms.openlocfilehash: aa0ba1de63cbcf1d9485c71b6bec7a29afb5140f
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91545167"
---
1. Wählen Sie im [LUIS-Portal](https://www.luis.ai) auf der Seite **Meine Apps** die Option **+ New app for conversation** (+ Neue App für die Konversation) und dann **Als JSON importieren** aus. Suchen Sie die gespeicherte JSON-Datei aus dem vorherigen Schritt. Sie müssen den Namen der App nicht ändern. Wählen Sie **Fertig**aus.

1. Wählen Sie auf der Registerkarte **Versionen** im Abschnitt **Verwalten** die Version `0.1` aus. Wählen Sie dann **Klonen** aus, um die Version zu klonen. Geben Sie anschließend einen Namen (`ml-entity`) ein, und wählen Sie **Fertig** aus, um den Klonvorgang abzuschließen. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

    > [!TIP]
    > Das Klonen in eine neue Version ist eine bewährte Methode vor dem Ändern einer App. Wenn Sie den Vorgang mit einer geänderten Version abschließen, exportieren Sie die Version (als JSON- oder LU-Datei), und checken Sie diese in Ihre Quellcodeverwaltung ein.

1. Wählen Sie **Erstellen** und dann **Absichten** aus, um die wichtigsten Bausteine einer LUIS-App anzuzeigen, nämlich die Absichten.

    ![Wechseln Sie von der Seite „Versionen“ zur Seite „Absichten“.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)
