---
author: laujan
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: lajanuar
ms.openlocfilehash: 85f5a40ef569ff6ec462bfdeca5fa966b918473b
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2021
ms.locfileid: "122326869"
---
* Navigieren Sie zu Ihrer Speicherressource im Azure-Portal, und wählen Sie die Registerkarte **Speicher-Explorer** aus, um die SAS-URL für Ihre benutzerdefinierten Modelltrainingdaten abzurufen. Wechseln Sie zum Container, klicken Sie mit der rechten Maustaste auf diesen, und wählen Sie **Shared Access Signature abrufen** aus. Achten Sie darauf, die SAS für den Container abzurufen, nicht für das Speicherkonto. Stellen Sie sicher, dass die Berechtigungen **Lesen**, **Schreiben**, **Löschen** und **Auflisten** aktiviert sind, und klicken Sie anschließend auf **Erstellen**. Kopieren Sie den Wert im Abschnitt **URL** dann an einen temporären Speicherort. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
