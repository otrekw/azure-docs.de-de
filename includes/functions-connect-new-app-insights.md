---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/09/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3289ba03d0f613d004bc8bff4dbcf2bd434f3da3
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83121541"
---
Mit Functions ist es einfach, die Application Insights-Integration über das [Azure-Portal] einer Funktionen-App hinzuzufügen.

1. Suchen Sie im [Azure-Portal][Azure-Portal] nach **Funktions-App**, und wählen Sie diese Option und dann Ihre Funktions-App aus. 

1. Wählen Sie oben im Fenster das Banner **Application Insights ist nicht konfiguriert** aus. Sollte dieses Banner nicht angezeigt werden, ist Application Insights bereits für Ihre App aktiviert.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Aktivieren von Application Insights über das Portal":::

1. Erstellen Sie eine Application Insights-Ressource. Verwenden Sie dazu die Einstellungen, die in der Tabelle unterhalb der Abbildung angegeben sind:

   :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Erstellen Sie eine Application Insights-Ressource":::.

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Eindeutiger App-Name | Es ist am einfachsten, den gleichen Namen wie für Ihre Funktionen-App zu verwenden, der in Ihrem Abonnement eindeutig sein muss. | 
    | **Location** | Europa, Westen | Verwenden Sie nach Möglichkeit dieselbe [Region](https://azure.microsoft.com/regions/) wie für Ihre Funktions-App (oder eine Region in der Nähe). |

1. Wählen Sie **Übernehmen**. Die Application Insights-Ressource wird in derselben Ressourcengruppe und unter demselben Abonnement wie Ihre Funktionen-App erstellt. Schließen Sie nach der Erstellung der Ressource das Application Insights-Fenster.

1. Wählen Sie in Ihrer Funktions-App die Option **Einstellungen** > **Konfiguration** und dann **Anwendungseinstellungen** aus. Wenn die Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY` angezeigt wird, bedeutet dies, dass die Application Insights-Integration für Ihre unter Azure ausgeführte Funktions-App aktiviert ist.

[Azure-Portal]: https://portal.azure.com
