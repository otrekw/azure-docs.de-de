---
title: Herstellen einer Verbindung mit Office 365 Video
description: Automatisieren von Aufgaben und Workflows, die Videos in Office 365 Video mithilfe von Azure Logic Apps verwalten
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/18/2016
tags: connectors
ms.openlocfilehash: 8ac6b7b411e7f42dd076c5b16e7b500a819c617f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665784"
---
# <a name="manage-videos-in-office365-video-by-using-azure-logic-apps"></a>Verwalten von Videos in Office 365 Video mithilfe von Azure Logic Apps

Stellen Sie eine Verbindung mit Office 365 Video her, um Informationen über ein Office 365-Video oder eine Liste von Videos abzurufen und vieles mehr. Mit Office 365 Video können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Office 365 Video abgerufen werden. 

* Verwenden Sie Aktionen zum Überprüfen des Videoportalstatus, zum Abrufen einer Liste mit allen Videos in einem Kanal und vieles mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. 

Sie können z.B. mit dem Bing-Suche-Connector nach Office 365-Videos suchen und anschließend mithilfe des Office 365-Video-Connectors Informationen zu diesem Video abrufen. Wenn das Video Ihre Anforderungen erfüllt, können Sie es auf Facebook veröffentlichen.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="connect-to-office365-video"></a>Herstellen einer Verbindung mit Office365 Video

Wenn Sie Ihren Logik-Apps diesen Connector hinzufügen, müssen Sie sich bei Ihrem Office 365-Video-Konto anmelden und den Logik-Apps das Herstellen einer Verbindung mit Ihrem Konto erlauben.

> [!INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Office 365 Video-Eigenschaften ein, z. B. den Mandantennamen oder die Kanal-ID. 

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/office365videoconnector/) an.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)