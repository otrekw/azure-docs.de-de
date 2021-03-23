---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: lajanuar
ms.openlocfilehash: 8bf78d4c2f703ee10b26b1936620f7cf23ed7c14
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467203"
---
Navigieren Sie zu Ihrer Speicherressource im Azure-Portal, und wählen Sie die Registerkarte **Speicher-Explorer** aus, um die SAS-URL für Ihre benutzerdefinierten Modelltrainingdaten abzurufen. Wechseln Sie zum Container, klicken Sie mit der rechten Maustaste auf diesen, und wählen Sie **Shared Access Signature abrufen** aus. Achten Sie darauf, die SAS für den Container abzurufen, nicht für das Speicherkonto. Stellen Sie sicher, dass die Berechtigungen **Lesen**, **Schreiben**, **Löschen** und **Auflisten** aktiviert sind, und klicken Sie anschließend auf **Erstellen**. Kopieren Sie den Wert im Abschnitt **URL** dann an einen temporären Speicherort. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
