---
title: Herstellen einer Verbindung mit GitHub
description: Überwachen von GitHub-Ereignissen mit GitHub-REST-APIs und Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: 7dc865f520b6f4667ace720e656a210e0252d1a1
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789760"
---
# <a name="connect-to-github-from-azure-logic-apps"></a>Herstellen einer Verbindung mit GitHub aus Azure Logic Apps

GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Darin finden Sie die gesamte verteilte Versionskontroll- und SCM-Funktionalität (Source Code Management, Quellcodeverwaltung) in Git sowie weitere Features.

Um mit der Verwendung des GitHub-Connectors loszulegen, [erstellen Sie zunächst eine Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Herstellen einer Verbindung mit GitHub

Um den GitHub-Connector in einer Logik-App verwenden zu können, müssen Sie zuerst eine *Verbindung* erstellen, und dann Details für diese Eigenschaften angeben: 

| Eigenschaft | Erforderlich | BESCHREIBUNG | 
| -------- | -------- | ----------- | 
| Tokenverschlüsselung | Ja | Geben Sie Ihre Anmeldeinformationen für GitHub an. |

Nachdem Sie die Verbindung erstellt haben, können Sie die Aktionen ausführen und auf die in diesem Artikel beschriebenen Trigger lauschen.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/github/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)