---
title: Wiederherstellen eines gelöschten Azure Database for MariaDB-Servers
description: In diesem Artikel wird beschrieben, wie Sie einen gelöschten Server in Azure Database for MariaDB über das Azure-Portal wiederherstellen können.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 4/12/2021
ms.openlocfilehash: 7cf2ce61239c7f2320f7b789acbb8f340e6de471
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315235"
---
# <a name="restore-a-deleted-azure-database-for-mariadb-server"></a>Wiederherstellen eines gelöschten Azure Database for MariaDB-Servers

Wenn ein Server gelöscht wird, kann die Datenbankserversicherung bis zu fünf Tage im Dienst aufbewahrt werden. Auf die Datenbanksicherung kann nur über das Azure-Abonnement zugegriffen werden, zu dem der Server ursprünglich gehörte. Und nur über dieses Abonnement kann die Datenbanksicherung auch wiederhergestellt werden. Sie können die folgenden empfohlenen Schritte ausführen, um eine gelöschte MariaDB-Serverressource innerhalb von 5 Tagen ab dem Zeitpunkt wiederherzustellen, zu dem der Server gelöscht wurde. Die empfohlenen Schritte funktionieren nur, wenn die Sicherung für den Server weiterhin verfügbar ist und nicht aus dem System gelöscht wurde. 

## <a name="pre-requisites"></a>Voraussetzungen
Sie benötigen Folgendes zum Wiederherstellen eines gelöschten Azure Database for MariaDB-Servers:
- Name des Azure-Abonnements, das den ursprünglichen Server gehostet hat
- Speicherort, an dem der Server erstellt wurde

## <a name="steps-to-restore"></a>Schritte zum Wiederherstellen

1. Wechseln Sie im Azure-Portal auf dem Blatt „Überwachen“ zum [Aktivitätsprotokoll](https://ms.portal.azure.com/#blade/Microsoft_Azure_ActivityLog/ActivityLogBlade). 

2. Klicken Sie im „Aktivitätsprotokoll“ wie hier gezeigt auf **Filter hinzufügen**, und legen Sie die folgenden Filter fest für 

    - **Abonnement** = Ihr Abonnement, das den gelöschten Server hostet
    - **Ressourcentyp** = Azure Database for MariaDB-Server (Microsoft.DBForMariaDB/servers) 
    - **Vorgang** = MariaDB-Server löschen (Microsoft.DBForMariaDB/servers/delete) 
 
     [![Für Löschen des MariaDB-Servers gefiltertes Aktivitätsprotokoll](./media/howto-restore-dropped-server/activity-log.png)](./media/howto-restore-dropped-server/activity-log.png#lightbox)
   
 3. Doppelklicken Sie auf das „MariaDB-Server löschen“-Ereignis, klicken Sie auf die Registerkarte „JSON“, und notieren Sie sich die Attribute „resourceId“ und „submissionTimestamp“ in der JSON-Ausgabe. Die „resourceId“ weist das folgende Format auf: /subscriptions/ffffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/TargetResourceGroup/providers/Microsoft.DBForMariaDB/servers/deletedserver.
 
 4. Wechseln Sie zur Seite [Server – Erstellen](/rest/api/mariadb/servers/create), klicken Sie auf die grün hervorgehobene Registerkarte „Ausprobieren“, und melden Sie sich mit Ihrem Azure-Konto an.
 
 5. Geben Sie „resourceGroupName“, „serverName“ (gelöschter Servername) und „subscriptionId“ an, abgeleitet aus dem in Schritt 3 aufgezeichneten resourceId-Attribut, während „api-version“, wie im Bild gezeigt, bereits eingetragen ist.
 
     [![Erstellen eines Servers mit der REST-API](./media/howto-restore-dropped-server/create-server-from-rest-api.png)](./media/howto-restore-dropped-server/create-server-from-rest-api.png#lightbox)
  
 6. Scrollen Sie im Abschnitt „Anforderungstext“ nach unten, und fügen Sie Folgendes ein:
     
    ```json
    {
        "location": "Dropped Server Location",  
        "properties": 
            {
                "restorePointInTime": "submissionTimestamp - 15 minutes",
                "createMode": "PointInTimeRestore",
                "sourceServerId": "resourceId"
            }
    }
    ```

7. Ersetzen Sie im vorstehenden Anforderungstext die folgenden Werte:
   * „Dropped server Location“ (Speicherort des gelöschten Servers) durch die Azure-Region, in der der gelöschte Server ursprünglich erstellt wurde
   * „submissionTimestamp“ und „resourceId“ durch die in Schritt 3 erfassten Werte 
   * Geben Sie für „restorePointinTime“ den Wert „submissionTimestamp“ minus **15 Minuten** an, um sicherzustellen, dass der Befehl nicht fehlerhaft ist.

8. Wenn Sie „Antwortcode 201“ oder 202 sehen, wurde die Wiederherstellungsanforderung erfolgreich übermittelt. 

9. Die Servererstellung kann abhängig von der Datenbankgröße und den Computeressourcen, die auf dem ursprünglichen Server bereitgestellt werden, eine Weile dauern. Der Wiederherstellungsstatus kann über das Aktivitätsprotokoll überwacht werden durch filtern nach 
   - **Abonnement** = Ihr Abonnement
   - **Ressourcentyp** = Azure Database for MariaDB-Server (Microsoft.DBForMariaDB/servers) 
   - **Vorgang** = MariaDB-Servererstellung aktualisieren

## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie versuchen, einen Server innerhalb von fünf Tagen wiederherzustellen, und nach dem genauen Ausführen der zuvor beschriebenen Schritte immer noch eine Fehlermeldung angezeigt wird, öffnen Sie einen Supportfall, um Unterstützung zu erhalten. Wenn Sie versuchen, einen gelöschten Server nach fünf Tagen wiederherzustellen, wird ein Fehler erwartet, weil die Sicherungsdatei nicht gefunden werden kann. Eröffnen Sie in diesem Szenario kein Supportticket. Das Supportteam kann keine Unterstützung bieten, wenn die Sicherung aus dem System gelöscht worden ist. 
- Um das versehentliche Löschen von Servern zu verhindern, sollten Sie unbedingt [Ressourcensperren](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/preventing-the-disaster-of-accidental-deletion-for-your-mysql/ba-p/825222) verwenden.
