---
title: Aktivieren einer Azure VMware Solution-Ressource
description: Erfahren Sie, wie Sie eine Supportanfrage einreichen, um Ihre Azure VMware Solution-Ressource zu aktivieren. Sie können auch weitere Knoten für Ihre vorhandene private Azure VMware Solution-Cloud anfordern.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: c9c31d8ffbf62a511f4f2ab19f995b6bc0ee0ca4
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695145"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Aktivieren einer Azure VMware Solution-Ressource
Hier erfahren Sie, wie Sie eine Supportanfrage einreichen, um Ihre Ressource vom Typ [Azure VMware Solution](introduction.md) zu aktivieren. Sie können auch weitere Knoten für Ihre vorhandene private Azure VMware Solution-Cloud anfordern.

## <a name="eligibility-criteria"></a>Wer kann Azure Government erwerben?

Sie benötigen ein Azure-Konto in einem Azure-Abonnement. Das Azure-Abonnement muss eines der folgenden Kriterien erfüllen:

* Abonnement unter einem [Azure Konzernvertrag (Enterprise Agreement, EA)](../cost-management-billing/manage/ea-portal-agreements.md) mit Microsoft
* Von einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) verwaltetes Abonnement unter einem Azure-Plan


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>Aktivieren einer Azure VMware Solution-Ressource für EA-Kunden
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

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>Aktivieren einer Azure VMware Solution-Ressource für CSP-Kunden 

CSPs müssen [Microsoft Partner Center](https://partner.microsoft.com) verwenden, um Azure VMware Solution für ihre Kunden zu aktivieren. 

   >[!IMPORTANT] 
   >Da der Azure VMware Solution-Dienst keine mehrinstanzenfähige Umgebung bietet, werden Hostingpartner noch nicht unterstützt. 

1. Wählen Sie in **Partner Center** die Option **CSP** aus, um auf den Bereich **Kunden** zuzugreifen.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Kundenbereich in Microsoft Partner Center" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. Wählen Sie Ihren Kunden und anschließend **Produkte hinzufügen** aus.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft Partner Center" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. Wählen Sie **Azure-Plan** und anschließend **Zu Warenkorb hinzufügen** aus. 

1. Überprüfen Sie die allgemeine Einrichtung des Azure-Planabonnements für Ihren Kunden, und schließen Sie sie ab. Weitere Informationen finden Sie in der [Dokumentation zu Microsoft Partner Center](https://docs.microsoft.com/partner-center/azure-plan-manage).

Nachdem Sie den Azure-Plan konfiguriert und die erforderlichen RBAC-Berechtigungen als CSP eingerichtet haben, wenden Sie sich an Microsoft, um das Kontingent für ein Azure-Planabonnement zu aktivieren. Greifen Sie unter Verwendung der Prozedur **Administrator im Namen von** (Admin on Behalf Of, AOBO) von Partner Center aus auf das Azure-Portal zu.

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com) an.

1. Wählen Sie **CSP** aus, um auf den Bereich **Kunden** zuzugreifen.

1. Erweitern Sie die Kundendetails, und wählen Sie **Microsoft Azure-Verwaltungsportal** aus.

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
   - Sollen mehrere Kunden gehostet werden?

   >[!NOTE]
   >Für Azure VMware Solution werden mindestens drei Knoten empfohlen, um Ihre private Cloud zu starten, und für Redundanz werden N + 1 Knoten empfohlen. 

1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.

   Es dauert bis zu fünf Werktage, bis ein Supportmitarbeiter Ihre Anforderung bestätigt.

   >[!IMPORTANT] 
   >Wenn Sie bereits eine Azure VMware Solution haben und zusätzliche Knoten anfordern möchten, beachten Sie bitte, dass wir fünf Werktage für die Zuordnung der Knoten benötigen. 

1. Nach dem Hinzufügen zum Azure-Plan und dem Aktivieren des Kontingents kann der Kunde oder der Partneradministrator über das Azure-Portal eine private Azure VMware Solution-Cloud bereitstellen. Stellen Sie sicher, dass Sie den Ressourcenanbieter **Microsoft.AVS** im Azure-Portal registriert haben, damit Sie Ihre Knoten bereitstellen können.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Informationen zu weiteren Möglichkeiten für die Ressourcenanbieterregistrierung finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="next-steps"></a>Nächste Schritte

Nach der Aktivierung Ihrer Azure VMware Solution-Ressource und der ordnungsgemäßen Einrichtung des Netzwerks können Sie [eine private Cloud erstellen](tutorial-create-private-cloud.md).