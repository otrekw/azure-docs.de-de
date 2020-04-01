---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 266cd52117f36b282fdd4bc8615a15e451cc203f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132565"
---
Mit Functions ist es einfach, die Application Insights-Integration über das [Azure-Portal] einer Funktionen-App hinzuzufügen.

1. Geben Sie im [Portal][Azure-Portal] im oberen Bereich der Seite den Suchbegriff `Function Apps` in die Suchleiste ein, wählen Sie Ihre Funktions-App aus, und wählen Sie anschließend im oberen Bereich des Fensters das Banner mit dem Hinweis **Application Insights ist nicht konfiguriert.** aus. Sollte dieses Banner nicht angezeigt werden, ist Application Insights bereits für Ihre App aktiviert.

    ![Aktivieren von Application Insights über das Portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Erstellen Sie eine Application Insights-Ressource. Verwenden Sie dazu die Einstellungen, die in der Tabelle unterhalb der Abbildung angegeben sind:

   ![Erstellen einer Application Insights-Ressource](media/functions-connect-new-app-insights/ai-general.png)

    | Einstellung      | Vorgeschlagener Wert  | Beschreibung                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Name** | Eindeutiger App-Name | Es ist am einfachsten, den gleichen Namen wie für Ihre Funktionen-App zu verwenden, der in Ihrem Abonnement eindeutig sein muss. | 
    | **Location** | Europa, Westen | Verwenden Sie nach Möglichkeit dieselbe [Region](https://azure.microsoft.com/regions/) wie für Ihre Funktions-App (oder eine Region in der Nähe). |

1. Klicken Sie auf **OK**. Die Application Insights-Ressource wird in derselben Ressourcengruppe und unter demselben Abonnement wie Ihre Funktionen-App erstellt. Schließen Sie nach der Erstellung der Ressource das Application Insights-Fenster.

1. Wählen Sie in Ihrer Funktions-App die Option **Anwendungseinstellungen** aus, und scrollen Sie dann nach unten zu **Anwendungseinstellungen**. Wenn die Einstellung `APPINSIGHTS_INSTRUMENTATIONKEY` angezeigt wird, bedeutet dies, dass die Application Insights-Integration für Ihre unter Azure ausgeführte Funktions-App aktiviert ist.

[Azure-Portal]: https://portal.azure.com
