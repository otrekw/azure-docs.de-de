---
title: Herstellen einer Verbindung mit Box
description: Automatisieren von Aufgaben und Workflows, die Dateien in Box mithilfe von Azure Logic Apps erstellen und verwalten
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: c7f97ff33742eb545cbfbd7521ba135584851e5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666770"
---
# <a name="create-and-manage-files-in-box-by-using-azure-logic-apps"></a>Erstellen und Verwalten von Dateien in Box mithilfe von Azure Logic Apps

In diesem Artikel wird gezeigt, wie Sie Ihre Dateien in Box innerhalb einer Logik-App mit dem Box-Connector erstellen und verwalten. Auf diese Weise können Sie Logik-Apps erstellen, mit denen Aufgaben und Workflows für das Verwalten Ihrer Dateien und anderer Aktionen automatisiert werden, wie z.B.:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Box abgerufen werden.

* Auslösen automatisierter Aufgaben und Workflows beim Erstellen oder Aktualisieren einer Datei.

* Führen Sie eine Aktion aus, die eine Datei kopiert oder löscht.

  Wenn diese Aktionen eine Antwort erhalten, stellen sie anschließend die Ausgabe anderen Aktionen zur Verfügung. 
  Wenn z. B. in Box eine Datei geändert wird, können Sie diese Datei mithilfe von Office 365 per E-Mail senden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Box-Konto](https://www.box.com/home).

* ein Azure-Abonnement Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Die Logik-App, in der Sie auf Ihr Box-Konto zugreifen möchten. Um Ihre Logik-App mit einem Box-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Wenn Sie mit Logik-Apps noch nicht vertraut sind, lesen Sie [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="connector-reference"></a>Connector-Referenz

Technische Details wie Trigger, Aktionen und Limits, wie sie in der OpenAPI-Datei (ehemals Swagger) des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/box/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)