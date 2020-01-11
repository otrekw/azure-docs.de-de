---
title: Herstellen einer Verbindung mit Google Drive
description: Automatisieren von Workflows, die Dateien für Google Drive mithilfe von Azure Logic Apps erstellen und verwalten
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 155a579438747ca7de67eba6449b8a0191e86e2c
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666838"
---
# <a name="create-and-manage-files-for-google-drive-by-using-azure-logic-apps"></a>Erstellen und Verwalten von Dateien für Google Drive mithilfe von Azure Logic Apps

Verbinden Sie sich mit Google Drive, um Dateien zu erstellen, Zeilen abzurufen usw. Google Drive ermöglicht Folgendes: 

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus einer Suche abgerufen werden. 
* Verwenden von Aktionen, um Bilder, Nachrichten und mehr zu suchen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z. B. ein Video suchen und dann Twitter verwenden, um das Video in einem Twitter-Feed zu posten.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-the-connection-to-google-drive"></a>Herstellen der Verbindung mit Google Drive

Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Google Drive erlauben.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Google Drive-Eigenschaften ein, z. B. Ordnerpfad oder Dateiname. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Technische Details zu Triggern, Aktionen und Beschränkungen aus der Swagger-Beschreibung des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/googledrive/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](apis-list.md)
