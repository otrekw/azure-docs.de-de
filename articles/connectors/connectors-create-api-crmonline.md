---
title: Herstellen einer Verbindung mit Dynamics 365
description: Erstellen und Verwalten von Dynamics 365-Datensätzen mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 05/09/2020
tags: connectors
ms.openlocfilehash: 00bf8ea2b783e09711a95f203bdfcce0e6b90b2c
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82994295"
---
# <a name="create-and-manage-records-in-dynamics-365-by-using-azure-logic-apps"></a>Erstellen und Verwalten von Dynamics 365-Datensätzen mithilfe von Azure Logic Apps

Dynamics 365 verwendet [Common Data Service](https://docs.microsoft.com/powerapps/maker/common-data-service/data-platform-intro). Verwenden Sie für Verbindungen mit Dynamics 365 den [Common Data Service-Connector](https://docs.microsoft.com/connectors/commondataservice/).

> [!IMPORTANT]
> Der [Dynamics 365-Connector](https://docs.microsoft.com/connectors/dynamicscrmonline/) ist veraltet, funktioniert jedoch weiterhin, bis er entfernt wird. Verwenden Sie den Dynamics 365-Connector nicht für neue Logik-Apps. Es wurde noch kein Zeitplan zum Entfernen des Dynamics 365-Connectors angekündigt. Sie müssen vorhandene Logik-Apps nicht in den Common Data Service-Connector oder einen anderen geplanten neuen Connector konvertieren, aber Sie müssen ihre Logik-Apps konvertieren, wenn der Connector entfernt wird. Weitere Informationen finden Sie unter [Dynamics 365-Connector ist veraltet](https://docs.microsoft.com/power-platform/important-changes-coming).
>
> Der [Common Data Service-Connector](https://docs.microsoft.com/connectors/commondataservice/) bietet die gleichen Funktionen wie der veraltete Dynamics 365-Connector, enthält jedoch Verbesserungen, die die Zuverlässigkeit steigern. Informationen zur Verwendung des Common Data Service-Connectors in Logik-Apps finden Sie unter [Erstellen und Verwalten von Common Data Service-Datensätzen mit Azure Logic Apps](../connectors/connect-common-data-service.md).

Weitere Informationen zu Common Data Service finden Sie in diesen Themen:

* [Learn: Erste Schritte mit Common Data Service](https://docs.microsoft.com/learn/modules/get-started-with-powerapps-common-data-service/)
* [Learn: Verbinden und Analysieren von Dynamics 365-Daten mithilfe von Power Platform und Common Data Service](https://docs.microsoft.com/learn/wwl/connect-analyze-dynamics-365-data/)