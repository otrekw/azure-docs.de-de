---
title: Herstellen einer Verbindung mit Box
description: Erstellen und Verwalten von Dateien mit Box-REST-APIs und Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 38e061e918d445de07961af1789891f44c59090a
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789862"
---
# <a name="create-and-manage-files-in-box-with-azure-logic-apps"></a>Erstellen und Verwalten von Dateien in Box mit Azure Logic Apps

In diesem Artikel wird gezeigt, wie Sie Ihre Dateien in Box innerhalb einer Logik-App mit dem Box-Connector erstellen und verwalten. Auf diese Weise können Sie Logik-Apps erstellen, mit denen Aufgaben und Workflows für das Verwalten Ihrer Dateien und anderer Aktionen automatisiert werden, wie z.B.:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Box abgerufen werden.

* Auslösen automatisierter Aufgaben und Workflows beim Erstellen oder Aktualisieren einer Datei.

* Führen Sie eine Aktion aus, die eine Datei kopiert oder löscht.

  Wenn diese Aktionen eine Antwort erhalten, stellen sie anschließend die Ausgabe anderen Aktionen zur Verfügung. 
  Wenn z. B. in Box eine Datei geändert wird, können Sie diese Datei mithilfe von Office 365 per E-Mail senden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Box-Konto](https://www.box.com/home).

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Die Logik-App, in der Sie auf Ihr Box-Konto zugreifen möchten. Um Ihre Logik-App mit einem Box-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Wenn Sie mit Logik-Apps noch nicht vertraut sind, lesen Sie [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="connector-reference"></a>Connector-Referenz

Technische Details wie Trigger, Aktionen und Limits, wie sie in der OpenAPI-Datei (ehemals Swagger) des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/box/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)