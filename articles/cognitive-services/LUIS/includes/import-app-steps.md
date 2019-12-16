---
title: Schritte zum Importieren von Apps
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 12/03/2019
ms.author: diberry
ms.openlocfilehash: b0d1735df7a3ac36e20af771939aec6b8013f2df
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74840815"
---
1. Wählen Sie im [LUIS-Vorschauportal](https://preview.luis.ai) auf der Seite **Meine Apps** die Option **Importieren** und dann **Als JSON importieren** aus. Suchen Sie die gespeicherte JSON-Datei aus dem vorherigen Schritt. Sie müssen den Namen der App nicht ändern. Wählen Sie **Fertig**aus.

1. Wählen Sie die Version auf der Registerkarte **Versionen** im Abschnitt **Verwalten** und dann **Klonen** aus, um die Version zu klonen. Geben Sie ihr anschließend einen aus zehn Zeichen bestehenden Namen, und wählen Sie **Fertig** aus, um den Klonvorgang zu beenden. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

    > [!TIP]
    > Das Klonen in eine neue Version ist eine bewährte Methode vor dem Ändern einer App. Wenn Sie mit einer Version fertig sind, exportieren Sie die Version (als JSON- oder LU-Datei), und checken Sie diese in Ihre Quellcodeverwaltung ein.

1. Wählen Sie **Erstellen** und dann **Absichten** aus, um die wichtigsten Bausteine einer LUIS-App anzuzeigen, nämlich die Absichten.

    ![Wechseln Sie von der Seite „Versionen“ zur Seite „Absichten“.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)