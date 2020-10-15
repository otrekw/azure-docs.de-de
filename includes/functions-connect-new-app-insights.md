---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/10/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 87af50c5b5e5b69fd175ac4a570c4b6f659b97e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84731003"
---
Mit Azure Functions ist es einfach, die Application Insights-Integration über das [Azure-Portal] einer Funktionen-App hinzuzufügen.

1. Suchen Sie im [Azure-Portal][Azure-Portal] nach **Funktions-App**, und wählen Sie diese Option und dann Ihre Funktions-App aus. 

1. Wählen Sie oben im Fenster das Banner **Application Insights ist nicht konfiguriert** aus. Sollte dieses Banner nicht angezeigt werden, ist Application Insights möglicherweise bereits für Ihre App aktiviert.

    :::image type="content" source="media/functions-connect-new-app-insights/enable-application-insights.png" alt-text="Aktivieren von Application Insights über das Portal":::

1. Erweitern Sie **Ressource ändern**, und erstellen Sie eine Application Insights-Ressource. Verwenden Sie dazu die Einstellungen, die in der folgenden Tabelle angegeben sind.  

    | Einstellung      | Vorgeschlagener Wert  | BESCHREIBUNG                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name der neuen Ressource** | Eindeutiger App-Name | Es ist am einfachsten, den gleichen Namen wie für Ihre Funktionen-App zu verwenden, der in Ihrem Abonnement eindeutig sein muss. | 
    | **Location** | Europa, Westen | Verwenden Sie nach Möglichkeit dieselbe [Region](https://azure.microsoft.com/regions/) wie für Ihre Funktions-App (oder eine Region in der Nähe). |

    :::image type="content" source="media/functions-connect-new-app-insights/ai-general.png" alt-text="Aktivieren von Application Insights über das Portal":::.

1. Wählen Sie **Übernehmen**. 

   Die Application Insights-Ressource wird in derselben Ressourcengruppe und unter demselben Abonnement wie Ihre Funktionen-App erstellt. Schließen Sie nach der Erstellung der Ressource das Application Insights-Fenster.

1. Wählen Sie in Ihrer Funktions-App unter **Einstellungen** die Option **Konfiguration** und dann **Anwendungseinstellungen** aus. Wenn die Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY` angezeigt wird, bedeutet dies, dass die Application Insights-Integration für Ihre unter Azure ausgeführte Funktions-App aktiviert ist.

[Azure-Portal]: https://portal.azure.com
