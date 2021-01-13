---
title: Bereitstellen und Verwalten von Azure Container Instances
description: Automatisieren von Aufgaben und Workflows zum Erstellen und Verwalten von Containerbereitstellungen in Azure Container Instances mithilfe von Azure Logic Apps
services: logic-apps, container-instances
ms.service: logic-apps
ms.suite: integration
author: dlepow
ms.author: danlep
ms.manager: gwallace
ms.reviewer: estfan, macolso
ms.topic: article
tags: connectors
ms.date: 01/14/2020
ms.openlocfilehash: ecb1049d64197f2a60438df7eedfb244907f7327
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76046204"
---
# <a name="deploy-and-manage-azure-container-instances-by-using-azure-logic-apps"></a>Bereitstellen und Verwalten von Azure Container Instances mithilfe von Azure Logic Apps

Mit Azure Logic Apps und dem Azure Container Instances-Connector können Sie automatisierte Aufgaben und Workflows zum Bereitstellen und Verwalten von [Containergruppen](../container-instances/container-instances-container-groups.md) einrichten. Der Container Instances-Connector unterstützt die folgenden Aktionen:

* Erstellen oder Löschen einer Containergruppe
* Abrufen der Eigenschaften einer Containergruppe
* Abrufen einer Liste von Containergruppen
* Abrufen der Protokolle einer Containerinstanz

Verwenden Sie diese Aktionen in Ihren Logik-Apps für Aufgaben wie das Ausführen einer Containerworkload als Reaktion auf einen Logic Apps-Trigger. Sie können die Ausgaben von Containerinstanzaktionen auch in anderen Aktionen verwenden. 

Dieser Connector bietet nur Aktionen, daher müssen Sie zum Starten Ihrer Logik-App einen separaten Trigger wie etwa einen **Wiederholungstrigger** verwenden, um eine Containerworkload nach einem regelmäßigen Zeitplan auszuführen. Möglicherweise müssen Sie auch nach einem Ereignis wie dem Eintreffen einer Outlook-E-Mail eine Containergruppenbereitstellung auslösen. 

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* ein Azure-Abonnement Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Grundlegende Kenntnisse zum [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) und zum [Erstellen und Verwalten von Containerinstanzen](../container-instances/container-instances-quickstart.md)

* Die Logik-App, in der Sie auf Ihre Containerinstanzen zugreifen möchten. Um eine Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z. B. dem **Wiederholungstrigger**.

## <a name="add-a-container-instance-action"></a>Hinzufügen einer Containerinstanzaktion

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Auswählen eines Pfads: 

   * Wählen Sie im letzten Schritt zum Hinzufügen einer Aktion **Neuer Schritt** aus. 

     Oder

   * Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
   Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „Containerinstanz“ als Filter ein. Wählen Sie in der Liste der Aktionen die gewünschte Azure Container Instances-Connectoraktion aus.

1. Geben Sie einen Namen für die Verbindung an. 

1. Geben Sie die erforderlichen Informationen für die ausgewählte Aktion ein, und fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort.

  Wählen Sie beispielsweise **Create container group** (Containergruppe erstellen) aus, und geben Sie die Eigenschaften für eine Containergruppe und eine oder mehrere Containerinstanzen in der Gruppe ein, wie in der folgenden Abbildung dargestellt (Teildetails):

  ![Erstellen einer Containergruppe](./media/connectors-create-api-container-instances/logic-apps-aci-connector.png)

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/aci/) und in der [YAML-Referenz](../container-instances/container-instances-reference-yaml.md) zu Containergruppen.

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich eine [Beispiel-Logik-App](https://github.com/Azure-Samples/aci-logicapps-integration) an, die einen Container in Azure Container Instances ausführt, um die Stimmung von E-Mail- oder Twitter-Text zu analysieren.

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)