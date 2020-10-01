---
title: Aktivieren einer Azure VMware Solution-Ressource
description: Erfahren Sie, wie Sie eine Supportanfrage einreichen, um Ihre Azure VMware Solution-Ressource zu aktivieren. Sie können auch weitere Knoten für Ihre vorhandene private Azure VMware Solution-Cloud anfordern.
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 72a7aca97067ce4e9ed0e901e4016c82b3549eb1
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817882"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Aktivieren einer Azure VMware Solution-Ressource
Erfahren Sie, wie Sie eine Supportanfrage einreichen, um Ihre Azure VMware Solution-Ressource zu aktivieren. Sie können auch weitere Knoten für Ihre vorhandene private Azure VMware Solution-Cloud anfordern.

## <a name="eligibility-criteria"></a>Wer kann Azure Government erwerben?

* Sie benötigen einen [Azure Konzernvertrag (Enterprise Agreement, EA)](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements) mit Microsoft.
* Sie benötigen ein Azure-Konto in einem Azure-Abonnement.


## <a name="enable-azure-vmware-solution-resource"></a>Aktivieren einer Azure VMware Solution-Ressource
Bevor Sie Ihre Azure VMware Solution-Ressource erstellen, müssen Sie ein Supportticket einreichen, damit ihre Knoten zugewiesen werden. Sobald das Supportteam Ihre Anforderung erhalten hat, dauert es bis zu fünf Werktage, bis die Anforderung bestätigt wird und ihre Knoten zugewiesen werden. Wenn Sie eine vorhandene private Azure VMware Solution-Cloud haben und möchten, dass weitere Knoten zugeordnet werden, durchlaufen Sie denselben Prozess.


1. Erstellen Sie im Azure-Portal unter **Hilfe und Support** eine **[Neue Supportanfrage](https://rc.portal.azure.com/#create/Microsoft.Support)** , und geben Sie die folgenden Informationen für das Ticket an:
   - **Problemtyp:** Technisch
   - **Abonnement:** Wählen Sie Ihr Abonnement aus.
   - **Dienst:** Alle Dienste > Azure VMware Solution
   - **Ressource:** Allgemeine Frage 
   - **Zusammenfassung:** Kapazität erforderlich
   - **Problemtyp:** Capacity Management Issues (Kapazitätsverwaltungsprobleme)
   - **Problemuntertyp:** Customer Request for Additional Host Quota/Capacity (Kundenanfrage für zusätzliches Hostkontingent/zusätzliche Hostkapazität)

1. Geben Sie in der **Beschreibung** des Supporttickets auf der Registerkarte **Details** Folgendes an:

   - Proof of Concept oder Produktion 
   - Name der Region
   - Anzahl von Knoten
   - Weitere Details

   >[!NOTE]
   >Für Azure VMware Solution werden mindestens drei Knoten empfohlen, um Ihre private Cloud zu starten, und für Redundanz werden N + 1 Knoten empfohlen. 

1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.

   Es dauert bis zu fünf Werktage, bis ein Supportmitarbeiter Ihre Anforderung bestätigt.

   >[!IMPORTANT] 
   >Wenn Sie bereits eine Azure VMware Solution haben und zusätzliche Knoten anfordern möchten, beachten Sie bitte, dass wir fünf Werktage für die Zuordnung der Knoten benötigen. 

1. Stellen Sie sicher, dass Sie den Ressourcenanbieter **Microsoft.AVS** im Azure-Portal registriert haben, damit Sie Ihre Knoten bereitstellen können.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Informationen zu weiteren Möglichkeiten für die Ressourcenanbieterregistrierung finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).