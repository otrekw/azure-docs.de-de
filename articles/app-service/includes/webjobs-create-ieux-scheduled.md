---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5105df5793d37b166b017585a62c962933a0b019
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081128"
---
## <a name="create-a-scheduled-webjob"></a><a name="CreateScheduledCRON"></a> Erstellen eines geplanten WebJobs


1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zur Seite **App Service** Ihrer <abbr title="Bei Ihrer App-Ressource kann es sich um eine Web-App, API-App oder mobile App handeln.">App-Ressource</abbr>.
1. Wählen Sie **WebJobs** aus.

   ![Auswählen von WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

1. Wählen Sie auf der Seite **WebJobs** die Option **Hinzufügen**.

    ![Seite „WebJob“](../media/web-sites-create-web-jobs/wjblade.png)

1. Verwenden Sie die in der Tabelle angegebenen Einstellungen **WebJob hinzufügen**.

    ![Seite „WebJob hinzufügen“](../media/web-sites-create-web-jobs/addwjscheduled.png)
    
    | Einstellung      | Beispielwert   |
    | ------------ | ----------------- | 
    | <abbr title="Ein Name, der innerhalb einer App Service-App eindeutig ist. Der Name muss mit einem Buchstaben oder einer Ziffer beginnen und darf nur die Sonderzeichen `-` und `_` enthalten.">Name</a> | myScheduledWebJob | 
    | <abbr title="Eine *ZIP*-Datei, die die ausführbare Datei oder Skriptdatei sowie für die Ausführung des Programms oder Skripts erforderliche Hilfsdateien enthält.">Dateiupload</abbr> | ConsoleApp.zip |
    | <abbr title="Als Typ ist „continuous“ (fortlaufend) oder „triggered“ (ausgelöst) verfügbar.">type</abbr> | Ausgelöst |
    | <abbr title="Damit die Planung zuverlässig funktioniert, aktivieren Sie das Feature „Always On“. Dieses Feature steht nur in den Tarifen „Basic“, „Standard“ und „Premium“ zur Verfügung.">Trigger</a> | Geplant |
    | CRON-Ausdruck</a> | 0 0/20 * * * * | 
    
    <br>
    
    <details>
     <summary>Weitere Informationen zu CRON-Ausdrücken</summary>
     <a name="#ncrontab-expressions"></a>
    
     Sie können einen [NCRONTAB-Ausdruck](../../azure-functions/functions-bindings-timer.md#ncrontab-expressions) im Portal eingeben oder eine `settings.job`-Datei dem Stammverzeichnis Ihrer WebJob-*ZIP*-Datei hinzufügen, wie im folgenden Beispiel gezeigt:
     
     ```json
     {
         "schedule": "0 */15 * * * *"
     }
     ```
     
     Weitere Informationen finden Sie unter [Planen eines ausgelösten WebJobs](../webjobs-dotnet-deploy-vs.md#scheduling-a-triggered-webjob).
     
     [!INCLUDE [webjobs-cron-timezone-note](../../../includes/webjobs-cron-timezone-note.md)]
     </details>
     <br>

1. Klicken Sie auf **OK**.

    Der neue WebJob wird auf der Seite **WebJobs** angezeigt.
    
    ![Liste von WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)
