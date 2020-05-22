---
title: Schritte zum Importieren von Apps
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/07/2020
ms.author: diberry
ms.openlocfilehash: 4c87ea9ebb2dd7aec36025e4b45f74e186ca1cd4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588779"
---
1. Wählen Sie im [LUIS-Portal](https://www.luis.ai) auf der Seite **Meine Apps** die Option **+ New app for conversation** (+ Neue App für die Konversation) und dann **Als JSON importieren** aus. Suchen Sie die gespeicherte JSON-Datei aus dem vorherigen Schritt. Sie müssen den Namen der App nicht ändern. Wählen Sie **Fertig**aus.

1. Wählen Sie auf der Registerkarte **Versionen** im Abschnitt **Verwalten** die Version `0.1` aus. Wählen Sie dann **Klonen** aus, um die Version zu klonen. Geben Sie anschließend einen Namen (`ml-entity`) ein, und wählen Sie **Fertig** aus, um den Klonvorgang abzuschließen. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

    > [!TIP]
    > Das Klonen in eine neue Version ist eine bewährte Methode vor dem Ändern einer App. Wenn Sie den Vorgang mit einer geänderten Version abschließen, exportieren Sie die Version (als JSON- oder LU-Datei), und checken Sie diese in Ihre Quellcodeverwaltung ein.

1. Wählen Sie **Erstellen** und dann **Absichten** aus, um die wichtigsten Bausteine einer LUIS-App anzuzeigen, nämlich die Absichten.

    ![Wechseln Sie von der Seite „Versionen“ zur Seite „Absichten“.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)