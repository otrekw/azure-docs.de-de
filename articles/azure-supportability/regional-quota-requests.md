---
title: Anforderungen zur Erhöhung von regionalen Azure-Kontingenten | Microsoft-Dokumentation
description: Anfordern einer regionalen Kontingenterhöhung
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 132cf6ccfec5af9951f5dc6d6a3c6d3c81363d81
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850005"
---
# <a name="standard-quota-regional-vcpu-limit-increase"></a>Standardkontingent: Erhöhung des Grenzwerts für regionale vCPUs 

Resource Manager unterstützt für virtuelle Computer zwei Arten von vCPU-Kontingenten. Für **VMs mit nutzungsbasierter Bezahlung** und **reservierte VM-Instanzen** wird das Standardkontingent verwendet. **Spot-VMs** nutzen Spotkontingent. 

vCPU-Standardkontingente für VMs mit nutzungsbasierter Bezahlung und reservierte VM-Instanzen werden für jedes Abonnement in jeder Region auf zwei Ebenen erzwungen.
 
Die erste Ebene ist der Grenzwert **Regionale vCPUs gesamt** (für alle VM-Serien). Die zweite Ebene ist der Grenzwert **vCPUs pro VM-Serie** (etwa die vCPUs der D-Serie). Bei jeder Bereitstellung eines neuen virtuellen Computers darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für diese VM-Serie das für diese spezielle VM-Serie genehmigte vCPU-Kontingent nicht überschreiten. Ferner darf die Gesamtzahl der neuen und vorhandenen vCPUs, die insgesamt für alle VM-Serien bereitgestellt werden, das für das Abonnement genehmigte Kontingent „Regionale vCPUs gesamt“ nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen. Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert.

Wenn ein neues Abonnement erstellt wird, entspricht der Standardwert für „Regionale vCPUs gesamt“ möglicherweise nicht der Summe der vCPU-Standardkontingente für alle einzelnen VM-Serien. Dadurch kann es passieren, dass ein Abonnement zwar über ein ausreichendes Kontingent für die einzelnen bereitzustellenden VM-Serien verfügt, das Kontingent „Regionale vCPUs gesamt“ jedoch nicht für alle Bereitstellungen ausreicht. In diesem Fall müssen Sie explizit eine Erhöhung des Grenzwerts „Regionale vCPUs gesamt“ anfordern. Der Grenzwert „Regionale vCPUs gesamt“ darf die Summe des genehmigten Kontingents für alle VM-Serien der Region nicht überschreiten.

Weitere Informationen zu vCPU-Standardkontingenten finden Sie auf den Seiten [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://aka.ms/quotalimits).

Weitere Informationen zum **Erhöhen der vCPU-Grenzwerte für Spot-VMs** finden Sie [hier](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Über das Blatt **Hilfe und Support** oder über das Blatt **Nutzung und Kontingente** können Sie im Portal eine Erhöhung des **Grenzwert der gesamten regionalen vCPUs für Standard-VMs** anfordern.

## <a name="request-standard-quota-regional-vcpu-limit-increase-at-subscription-level-using-the-help--support-blade"></a>Anfordern einer Erhöhung des Grenzwerts für das vCPU-Standardkontingent auf Abonnementebene mithilfe des Blatts „Hilfe und Support“

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Hilfe und Support“ eine Supportanfrage zu erstellen. 

1. Wählen Sie unter https://portal.azure.com die Option **Hilfe und Support** aus.

![Hilfe und Support](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Wählen Sie **Neue Supportanfrage** aus. 

![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. Wählen Sie in der Dropdownliste „Problemtyp“ die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

![Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

![Auswählen des Abonnements (neue Supportanfrage)](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Andere Anforderungen** aus.

![Kontingenttyp](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. Geben Sie im Bereich **Details** zusätzliche Informationen an (wie im folgenden Beispiel gezeigt), um die Verarbeitung Ihrer Anforderung zu unterstützen und mit der Erstellung des Falls fortzufahren. 
    1.  **Bereitstellungsmodell**: Geben Sie „Resource Manager“ an
    2.  **Angeforderte Region**: Geben Sie die gewünschte Region an (z. B. USA, Osten 2)
    3.  **Neuer Grenzwert**: Geben Sie den neuen Grenzwert für die Region an. Dieser Wert sollte die Summe der genehmigten Kontingente für die einzelnen SKU-Familien für dieses Abonnement nicht überschreiten.

![Kontingentdetails](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>Anfordern einer Kontingenterhöhung für „Regionale vCPUs gesamt“ auf Abonnementebene mithilfe des Blatts **Nutzung + Kontingente**

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Nutzung + Kontingente“ eine Supportanfrage zu erstellen. 

1. Klicken Sie unter https://portal.azure.com auf **Abonnements**.

![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Wählen Sie **Nutzung + Kontingente** aus.

![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Klicken Sie in der Ecke oben rechts auf **Erhöhung anfordern**.

![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Andere Anforderungen** aus.

![Kontingenttyp](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. Geben Sie im Bereich **Details** zusätzliche Informationen an (wie im folgenden Beispiel gezeigt), um die Verarbeitung Ihrer Anforderung zu unterstützen und mit der Erstellung des Falls fortzufahren. 
    1.  **Bereitstellungsmodell**: Geben Sie „Resource Manager“ an
    2.  **Angeforderte Region**: Geben Sie die gewünschte Region an (z. B. USA, Osten 2)
    3.  **Neuer Grenzwert**: Geben Sie den neuen Grenzwert für die Region an. Dieser Wert sollte die Summe der genehmigten Kontingente für die einzelnen SKU-Familien für dieses Abonnement nicht überschreiten.

![Kontingentdetails](./media/resource-manager-core-quotas-request/regional-details.png)



