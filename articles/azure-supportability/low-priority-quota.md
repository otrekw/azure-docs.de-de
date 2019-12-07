---
title: Kontingent mit niedriger Priorität | Microsoft-Dokumentation
description: Anforderungen von Kontingenten mit niedriger Priorität
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 49ac478330cf73dff050a3edcc15933692fa6448
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534726"
---
# <a name="low-priority-quota-limit-increase-for-all-vm-series"></a>Kontingent mit niedriger Priorität: Erhöhung des Limits für alle VM-Serien

Virtuelle Computer mit niedriger Priorität bieten ein anderes Modell der Azure-Nutzung, das geringere Kosten mit sich bringt, dafür aber Azure gestattet, bei Bereitstellungen reservierter VM-Instanzen oder von VMs mit nutzungsbasierter Bezahlung eine VM nach Bedarf zu entfernen. Weitere Informationen zu VMs mit niedriger Priorität finden Sie [hier](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority).

Resource Manager unterstützt zwei Arten von vCPU-Kontingenten für virtuelle Computer. **VMs mit nutzungsbasierter Bezahlung und reservierte VM-Instanzen** verwenden Standardkontingente. **VMs mit niedriger Priorität** verwenden Kontingente mit niedriger Priorität. 

Beim **Kontingenttyp mit niedriger Priorität** werden vCPU-Kontingente von Resource Manager für alle verfügbaren VM-Serien als ein einzelnes regionales Limit erzwungen.

Bei jeder Bereitstellung eines neuen virtuellen Computers mit niedriger Priorität darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für alle VM-Instanzen mit niedriger Priorität das genehmigte vCPU-Kontingentlimit mit niedriger Priorität nicht überschreiten. Wenn das Kontingent mit niedriger Priorität überschritten wird, wird die Bereitstellung der VM mit niedriger Priorität nicht zugelassen. Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits mit niedriger Priorität anfordern. 

Weitere Informationen zu standardmäßigen vCPU-Kontingenten finden Sie auf den Seiten „vCPU-Kontingente für virtuelle Computer“ und „Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen“. Weitere Informationen zum Erhöhen des regionalen vCPU-Limits für Standardkontingente finden Sie auf dieser [Seite](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

Sie können jetzt eine Erhöhung der **Kontingentlimits mit niedriger Priorität für alle VM-Serien** über das Blatt **Hilfe und Support** oder das Blatt **Nutzung und Kontingente** im Portal anfordern.

## <a name="request-low-priority-quota-limit-increase-for-all-vm-series-per-subscription-using-the-help--support-blade"></a>Anfordern einer Erhöhung des Kontingentlimits mit niedriger Priorität für alle VM-Serien pro Abonnement über das Blatt „Hilfe und Support“

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Hilfe und Support“ eine Supportanfrage zu erstellen.

Sie können auch **Kontingente für mehrere Regionen** über eine einzige Supportanfrage anfordern. Weitere Informationen finden Sie weiter unten in Schritt 10. 


1. Wählen Sie unter https://portal.azure.com die Option **Hilfe und Support** aus.

   ![Hilfe und Support](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Wählen Sie **Neue Supportanfrage** aus. 

     ![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Wählen Sie in der Dropdownliste „Problemtyp“ die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

   ![Auswählen des Abonnements (neue Supportanfrage)](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Compute-VM (cores/vCPUs) subscription limit increases** (Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)) aus. 

   ![Auswählen des Kontingenttyps](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. Klicken Sie unter **Problemdetails** auf **Details angeben**, und geben Sie weitere Informationen an, die bei der Verarbeitung Ihrer Anfrage hilfreich sein können.

   ![Angeben von Details](./media/resource-manager-core-quotas-request/provide-details.png)
   
7.  Wählen Sie im Bereich **Kontingentdetails*** das **Bereitstellungsmodell** und einen **Standort** aus.

![Angeben von Details](./media/resource-manager-core-quotas-request/3-7.png)

8. Wählen Sie für den ausgewählten Speicherort den Wert **Typ** als **‚Niedrige Priorität‘** aus. Sie können sowohl Standardkontingenttypen als auch Kontingenttypen mit niedriger Priorität über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl im Feld **Typ** verwenden. Weitere Informationen zum **Erhöhen des Standardkontingents für VM-Serien** finden Sie auf dieser [Seite](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

![Angeben von Details](./media/resource-manager-core-quotas-request/3-8.png)

9.  Geben Sie das gewünschte neue Limit für das Abonnement ein. 
 
 ![Angeben von Details](./media/resource-manager-core-quotas-request/3-9.png)

10. Um ein Kontingent für mehr als einen Standort anzufordern, können Sie in der Dropdownliste einen anderen Standort und den entsprechenden VM-Typ auswählen. Sie können dann die gewünschten Limits eingeben.

![Angeben von Details](./media/resource-manager-core-quotas-request/3-10.png)

11. Nachdem Sie das gewünschte Kontingent eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

## <a name="request-low-priority-quota-limit-increase-for-all-vm-series-per-subscription-using-usages--quota-blade"></a>Anfordern einer Erhöhung des Kontingentlimits mit niedriger Priorität für alle VM-Serien pro Abonnement über das Blatt „Nutzung + Kontingente“

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Nutzung + Kontingente“ eine Supportanfrage zu erstellen.

Sie können auch **Kontingente für mehrere Regionen** über eine einzige Supportanfrage anfordern. Weitere Informationen finden Sie weiter unten in Schritt 9. 

1.  Klicken Sie unter https://portal.azure.com auf **Abonnements**.

 ![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2.  Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

 ![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3.  Wählen Sie **Nutzung + Kontingente** aus.

 ![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4.  Wählen Sie oben rechts die Option **Erhöhung anfordern** aus.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5.  Wählen Sie als Kontingenttyp die Option **Compute-VM (cores/vCPUs) subscription limit increases** (Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)) aus.

  ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/select-quota-type.png)

6.  Wählen Sie im Bereich **Kontingentdetails** das Bereitstellungsmodell und einen Standort aus.

  ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/3-2-6.png)
 
7.  Wählen Sie für den ausgewählten Speicherort den Wert **Typ** als **‚Niedrige Priorität‘** aus. Sie können sowohl Standardkontingenttypen als auch Kontingenttypen mit niedriger Priorität über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl im Feld **Typ** verwenden. Weitere Informationen zum **Erhöhen des Standardkontingents für VM-Serien** finden Sie auf dieser [Seite](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

  ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/3-2-7.png)
 
8.  Geben Sie das gewünschte neue Limit für das Abonnement ein.

  ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/3-2-8.png)
 
9.  Um ein Kontingent für mehr als einen Standort anzufordern, können Sie in der Dropdownliste einen anderen **Standort** und den entsprechenden VM-Typ auswählen. Sie können dann die gewünschten Limits eingeben.

  ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/3-2-9.png)
 
10. Nachdem Sie das gewünschte Kontingent eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

