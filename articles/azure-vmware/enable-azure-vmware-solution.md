---
title: Anfordern des Hostkontingents und Aktivieren von Azure VMware Solution
description: Erfahren Sie, wie Sie das Hostkontingent bzw. die Hostkapazität anfordern und den Azure VMware Solution-Ressourcenanbieter aktivieren. Sie können auch weitere Hosts in einer vorhandenen privaten Azure VMware Solution-Cloud anfordern.
ms.topic: how-to
ms.custom: contperf-fy21q3
ms.date: 02/17/2021
ms.openlocfilehash: 5d154f5c63ffccdbf1729e641133b54be478d884
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100653164"
---
# <a name="request-host-quota-and-enable-azure-vmware-solution"></a>Anfordern des Hostkontingents und Aktivieren von Azure VMware Solution

In dieser Anleitung erfahren Sie, wie Sie das Hostkontingent bzw. die Hostkapazität anfordern und den [Azure VMware Solution](introduction.md)-Ressourcenanbieter aktivieren, um den Dienst zu aktivieren. Bevor Sie Azure VMware Solution aktivieren können, müssen Sie ein Supportticket übermitteln, damit Ihre Hosts zugewiesen werden. Wenn Sie über eine vorhandene private Azure VMware Solution-Cloud verfügen und weitere Hosts zugeordnet werden sollen, müssen Sie auf die gleiche Weise vorgehen.

>[!IMPORTANT]
>Das Zuordnen der Hosts kann abhängig von der angeforderten Anzahl einige Tage dauern.  Fordern Sie also an, was für die Bereitstellung erforderlich ist, damit weniger häufig eine Kontingenterhöhung angefordert werden muss.


Der gesamte Prozess ist einfach und umfasst zwei Schritte:
- Anfordern eines zusätzlichen Hostkontingents / einer zusätzlichen Hostkapazität für [EA-Kunden](#request-host-quota-for-ea-customers) oder [CSP-Kunden](#request-host-quota-for-csp-customers) 
- Aktivieren des Microsoft.AVS-Ressourcenanbieters

## <a name="eligibility-criteria"></a>Wer kann Azure Government erwerben?

Sie benötigen ein Azure-Konto in einem Azure-Abonnement. Das Azure-Abonnement muss eines der folgenden Kriterien erfüllen:

- Abonnement unter einem [Azure Konzernvertrag (Enterprise Agreement, EA)](../cost-management-billing/manage/ea-portal-agreements.md) mit Microsoft
- Von einem Cloudlösungsanbieter (Cloud Solution Provider, CSP) verwaltetes Abonnement unter einem CSP-Angebotsvertrag in Azure oder einem Azure-Plan

## <a name="request-host-quota-for-ea-customers"></a>Anfordern des Hostkontingents für EA-Kunden

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
   - Anzahl von Hosts
   - Weitere Details

   >[!NOTE]
   >Für Azure VMware Solution werden mindestens drei Hosts empfohlen, um Ihre private Cloud aufzubauen. Für Redundanzzwecke werden N+1 Hosts empfohlen. 

1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.


## <a name="request-host-quota-for-csp-customers"></a>Anfordern des Hostkontingents für CSP-Kunden 

CSPs müssen [Microsoft Partner Center](https://partner.microsoft.com) verwenden, um Azure VMware Solution für ihre Kunden zu aktivieren. In diesem Artikel wird der [CSP-Azure-Plan](/partner-center/azure-plan-lp) als Beispiel verwendet, um die Kaufabwicklung für Partner zu veranschaulichen.

Greifen Sie unter Verwendung der Prozedur **Administrator im Namen von** (Admin on Behalf Of, AOBO) von Partner Center aus auf das Azure-Portal zu.

>[!IMPORTANT] 
>Der Azure VMware Solution-Dienst bietet keine Mehrinstanzenfähigkeit. Hostingpartner, die dies erfordern, werden nicht unterstützt. 

1. Konfigurieren Sie den Azure CSP-Plan:

   1. Wählen Sie in **Partner Center** die Option **CSP** aus, um auf den Bereich **Kunden** zuzugreifen.
   
      :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Kundenbereich in Microsoft Partner Center" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::
   
   1. Wählen Sie Ihren Kunden und anschließend **Produkte hinzufügen** aus.
   
      :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft Partner Center" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::
   
   1. Wählen Sie **Azure-Plan** und anschließend **Zu Warenkorb hinzufügen** aus. 
   
   1. Überprüfen Sie die allgemeine Einrichtung des Abonnements mit Azure-Plan für Ihren Kunden, und schließen Sie sie ab. Weitere Informationen finden Sie in der [Dokumentation zu Microsoft Partner Center](/partner-center/azure-plan-manage).

1. Nachdem Sie den Azure-Plan konfiguriert und die erforderlichen [Azure RBAC-Berechtigungen](/partner-center/azure-plan-manage) für das Abonnement eingerichtet haben, fordern Sie das Kontingent für Ihr Abonnement mit Azure-Plan an. 

   1. Greifen Sie unter Verwendung der Prozedur **Administrator im Namen von** (Admin on Behalf Of, AOBO) von [Microsoft Partner Center](https://partner.microsoft.com) aus auf das Azure-Portal zu.
   
   1. Wählen Sie **CSP** aus, um auf den **Kundenbereich** zuzugreifen.
   
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
      - Anzahl von Hosts
      - Weitere Details
      - Sollen mehrere Kunden gehostet werden?
   
      >[!NOTE]
      >Für Azure VMware Solution werden mindestens drei Hosts empfohlen, um Ihre private Cloud aufzubauen. Für Redundanzzwecke werden N+1 Hosts empfohlen. 
   
   1. Wählen Sie **Überprüfen + erstellen** aus, um die Anforderung zu übermitteln.

## <a name="register-the-microsoftavs-resource-provider"></a>Registrieren des **Microsoft.AVS**-Ressourcenanbieters

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="next-steps"></a>Nächste Schritte

Nach dem Aktivieren der Ressource und der ordnungsgemäßen Einrichtung des Netzwerks können Sie [eine private Cloud](tutorial-create-private-cloud.md) erstellen.
