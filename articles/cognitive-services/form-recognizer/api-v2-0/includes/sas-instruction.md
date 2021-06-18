---
author: laujan
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/25/2021
ms.author: lajanuar
ms.openlocfilehash: bb1a40b4f8b52181b34d89d40ae58505356e93bf
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111891032"
---
Navigieren Sie zu Ihrer Speicherressource im Azure-Portal, und wählen Sie die Registerkarte **Speicher-Explorer** aus, um die SAS-URL für Ihre benutzerdefinierten Modelltrainingdaten abzurufen. Wechseln Sie zum Container, klicken Sie mit der rechten Maustaste auf diesen, und wählen Sie **Shared Access Signature abrufen** aus. Achten Sie darauf, die SAS für den Container abzurufen, nicht für das Speicherkonto. Stellen Sie sicher, dass die Berechtigungen **Lesen**, **Schreiben**, **Löschen** und **Auflisten** aktiviert sind, und klicken Sie anschließend auf **Erstellen**. Kopieren Sie den Wert im Abschnitt **URL** dann an einen temporären Speicherort. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
