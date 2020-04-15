---
title: Schritte zum Importieren von Apps
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/30/2020
ms.author: diberry
ms.openlocfilehash: b7b8befa0f5871b65b9b5621bfb99c659bf07235
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422750"
---
1. Wählen Sie im [LUIS-Vorschauportal](https://preview.luis.ai) auf der Seite **Meine Apps** die Option **+ Neue App für Unterhaltung** und dann **Als JSON importieren** aus. Suchen Sie die gespeicherte JSON-Datei aus dem vorherigen Schritt. Sie müssen den Namen der App nicht ändern. Wählen Sie **Fertig**aus.

1. Wählen Sie auf der Registerkarte **Versionen** im Abschnitt **Verwalten** die Version `0.1` aus. Wählen Sie dann **Klonen** aus, um die Version zu klonen. Weisen Sie anschließend einen aus zehn Zeichen bestehenden Namen (`ml-entity`) zu, und wählen Sie **Fertig**, um den Klonvorgang abzuschließen. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

    > [!TIP]
    > Das Klonen in eine neue Version ist eine bewährte Methode vor dem Ändern einer App. Wenn Sie den Vorgang mit einer geänderten Version abschließen, exportieren Sie die Version (als JSON- oder LU-Datei), und checken Sie diese in Ihre Quellcodeverwaltung ein.

1. Wählen Sie **Erstellen** und dann **Absichten** aus, um die wichtigsten Bausteine einer LUIS-App anzuzeigen, nämlich die Absichten.

    ![Wechseln Sie von der Seite „Versionen“ zur Seite „Absichten“.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)