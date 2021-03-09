---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 33dc766643355a5f5ebb6138e000595fd1bfe6fc
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743431"
---
## <a name="create-a-continuous-webjob"></a><a name="CreateContinuous"></a> Erstellen eines fortlaufenden WebJobs

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zur Seite **App Service** Ihrer <abbr title="Bei Ihrer App-Ressource kann es sich um eine Web-App, API-App oder mobile App handeln.">App-Ressource</abbr>.
1. Wählen Sie **WebJobs** aus.

   ![Auswählen von WebJobs](../media/web-sites-create-web-jobs/select-webjobs.png)

1. Wählen Sie auf der Seite **WebJobs** die Option **Hinzufügen**.

    ![Seite „WebJob“](../media/web-sites-create-web-jobs/wjblade.png)

1. Verwenden Sie die in der Tabelle angegebenen Einstellungen **WebJob hinzufügen**.

   ![Screenshot, der die Einstellungen von „WebJob hinzufügen“ zeigt, die Sie konfigurieren müssen.](../media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Einstellung      | Beispielwert   | 
   | ------------ | ----------------- | 
   | <abbr title="Ein Name, der innerhalb einer App Service-App eindeutig ist. Der Name muss mit einem Buchstaben oder einer Ziffer beginnen und darf nur die Sonderzeichen `-` und `_` enthalten.">Name</abbr> | myContinuousWebJob | 
   | <abbr title=" Eine *ZIP*-Datei, die die ausführbare Datei oder Skriptdatei sowie für die Ausführung des Programms oder Skripts erforderliche Hilfsdateien enthält.">Dateiupload</abbr> | ConsoleApp.zip |
   | <abbr title="Als Typ ist „continuous“ (fortlaufend) oder „triggered“ (ausgelöst) verfügbar.">type</abbr> | Fortlaufend | 
   | <abbr title="Diese Option ist nur für fortlaufende WebJobs verfügbar. Legt fest, ob das Programm oder Skript auf allen Instanzen oder nur einer Instanz ausgeführt wird. Die Option zum Ausführen auf mehreren Instanzen gilt nicht für die Tarife „Free“ oder „Shared“.">Skalieren</abbr> | Mehrere Instanzen | 

1. Klicken Sie auf **OK**.

    Der neue WebJob wird auf der Seite **WebJobs** angezeigt.

    ![Liste von WebJobs](../media/web-sites-create-web-jobs/listallwebjobs.png)

1. Klicken Sie zum **Starten oder Beenden** eines fortlaufenden WebJobs in der Liste mit der rechten Maustaste auf den WebJob, und klicken Sie dann auf **Beenden** oder **Starten**.

   ![Beenden eines fortlaufenden WebJobs](../media/web-sites-create-web-jobs/continuousstop.png)
