---
title: Beispiele für Azure Resource Manager-Vorlagen – Event Grid | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Liste mit Azure Resource Manager-Vorlagenbeispielen für Azure Event Grid auf GitHub.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 38d8db0bcc504760595fe51b63072f63e785577a
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720621"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-Vorlagen für Event Grid

Informationen zur JSON-Syntax und zu den Eigenschaften, die in einer Vorlage verwendet werden sollen, finden Sie unter [Microsoft.EventGrid resource types](/azure/templates/microsoft.eventgrid/allversions) (Microsoft.EventGrid-Ressourcentypen). Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Event Grid:

| | |
|-|-|
|**Event Grid-Abonnements**||
| [Benutzerdefiniertes Thema und Abonnement mit WebHook-Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Stellt ein benutzerdefiniertes Event Grid-Thema bereit. Erstellt ein Abonnement für dieses benutzerdefinierte Thema, das einen WebHook-Endpunkt verwendet. |
| [Benutzerdefiniertes Themenabonnement mit EventHub-Endpunkt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Erstellt ein Event Grid-Abonnement für ein benutzerdefiniertes Thema. Das Abonnement verwendet eine Event Hub-Instanz für den Endpunkt. |
| [Azure- oder Ressourcengruppenabonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook)| Abonniert Ereignisse für eine Ressourcengruppe oder ein Azure-Abonnement. Die Ressourcengruppe, die Sie im Rahmen der Bereitstellung als Ziel angeben, ist die Quelle der Ereignisse. Das Abonnement verwendet einen Webhook für den Endpunkt. |
| [Blob Storage-Konto und Abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage)| Stellt ein Azure Blob Storage-Konto bereit und abonniert Ereignisse für dieses Speicherkonto. |
| | |
