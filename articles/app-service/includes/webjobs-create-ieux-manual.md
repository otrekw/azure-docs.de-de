---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5687fb99c27b8b2141e0a2a817327cfbb124951a
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102109325"
---
## <a name="create-a-manually-triggered-webjob"></a><a name="CreateOnDemand"></a> Erstellen eines manuell ausgelösten WebJobs

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zur Seite **App Service** bei Ihrer <abbr title="Bei Ihrer App-Ressource kann es sich um eine Web-App, API-App oder mobile App handeln.">App-Ressource</abbr>.
1. Wählen Sie **WebJobs** aus.

    ![Auswählen von WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

2. Wählen Sie auf der Seite **WebJobs** die Option **Hinzufügen**.

   ![Seite „WebJob“](../media/web-sites-create-web-jobs/wjblade.png)

3. Verwenden Sie die in der Tabelle angegebenen Einstellungen **WebJob hinzufügen**.

    ![Screenshot, der die Einstellungen zeigt, die für das Erstellen eines manuell ausgelösten WebJobs konfiguriert werden müssen.](../media/web-sites-create-web-jobs/addwjtriggered.png)
    
    | Einstellung      | Beispielwert   | 
    | ------------ | ----------------- | 
   | <abbr title="Ein Name, der innerhalb einer App Service-App eindeutig ist. Der Name muss mit einem Buchstaben oder einer Ziffer beginnen und darf nur die Sonderzeichen `-` und `_` enthalten.">Name</abbr> | myTriggeredWebJob | 
    | <abbr title="Eine *ZIP*-Datei, die die ausführbare Datei oder Skriptdatei sowie für die Ausführung des Programms oder Skripts erforderliche Hilfsdateien enthält.">Dateiupload</abbr> | ConsoleApp.zip |
    | <abbr title="Als Typ ist „continuous“ (fortlaufend) oder „triggered“ (ausgelöst) verfügbar.">type</abbr> | Ausgelöst | 
    | <abbr title="Als Typ ist „scheduled“ (geplant) oder „manual“ (manuell) verfügbar.">Triggers</a> (Trigger) | Manuell | |

4. Klicken Sie auf **OK**. 

   Der neue WebJob wird auf der Seite **WebJobs** angezeigt.

   ![Liste von WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)

7. Für eine **manuelle WebJob-Ausführung** klicken Sie mit der rechten Maustaste auf den WebJob-Namen in der Liste. Klicken Sie dann auf **Ausführen**.
   
    ![Webauftrag ausführen](../media/web-sites-create-web-jobs/runondemand.png)

