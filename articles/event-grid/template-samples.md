---
title: Beispiele für Azure Resource Manager-Vorlagen – Event Grid | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Liste mit Azure Resource Manager-Vorlagenbeispielen für Azure Event Grid auf GitHub.
ms.topic: sample
ms.date: 07/07/2020
ms.openlocfilehash: 910012adf2dc930e6f1a26f1a7fc41f5ed0580c9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119054"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Azure Resource Manager-Vorlagen für Event Grid

Informationen zur JSON-Syntax und zu den Eigenschaften, die in einer Vorlage verwendet werden sollen, finden Sie unter [Microsoft.EventGrid resource types](/azure/templates/microsoft.eventgrid/allversions) (Microsoft.EventGrid-Ressourcentypen). Die folgende Tabelle enthält Links zu Azure Resource Manager-Vorlagen für Event Grid:

## <a name="event-grid-subscriptions"></a>Event Grid-Abonnements
- [Erstellen eines benutzerdefinierten Event Grid-Themas und eines Abonnements in Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid): Hier wird ein benutzerdefiniertes Event Grid-Thema bereitgestellt. Darüber hinaus wird ein Abonnement für dieses benutzerdefinierte Thema erstellt, von dem ein WebHook-Endpunkt verwendet wird. 
- [Erstellen eines benutzerdefinierten Event Grid-Themas und Senden von Ereignissen an Event Hub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler): Hier wird ein Event Grid-Abonnement für ein benutzerdefiniertes Thema erstellt. Das Abonnement verwendet eine Event Hub-Instanz für den Endpunkt. 
- [Erstellen eines Event Grid-Abonnements für Ressourcenereignisse](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook): Hier werden Ereignisse für eine Ressourcengruppe oder für ein Azure-Abonnement abonniert. Die Ressourcengruppe, die Sie im Rahmen der Bereitstellung als Ziel angeben, ist die Quelle der Ereignisse. Das Abonnement verwendet einen Webhook für den Endpunkt. 
- [Erstellen eines Azure Blob Storage-Kontos mit Event Grid-Abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-subscription-and-storage): Hier werden ein Azure Blob Storage-Konto bereitgestellt und Ereignisse für dieses Speicherkonto abonniert. 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die folgenden Beispiele an:

- [PowerShell-Beispiele](powershell-samples.md)
- [CLI-Beispiele](cli-samples.md)
