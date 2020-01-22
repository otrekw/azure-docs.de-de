---
title: Anfordern einer Erhöhung der regionalen Azure vCPU-Kontingentgrenzen | Microsoft-Dokumentation
description: Anfordern einer regionalen Kontingenterhöhung
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: e262651a6e040c40dbe240ad3437eff1914aa3e5
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898690"
---
# <a name="standard-quota-increase-limits-by-region"></a>Standardkontingent: Erhöhen der Grenzwerte nach Region 

Azure Resource Manager unterstützt zwei Arten von vCPU-Kontingenten für virtuelle Computer:
* *VMs mit nutzungsbasierter Bezahlung* und *reservierte VM-Instanzen* verwenden *vCPU-Standardkontingente*.
* *Spot-VMs* unterliegen einem *Spot-vCPU-Kontingent*. 

Das standardmäßige vCPU-Kontingent für nutzungsbasierte Bezahlung und reservierte VM-Instanzen wird für jedes Abonnement in jeder Region auf zwei Ebenen erzwungen:
* Die erste Ebene ist der *Grenzwert regionaler vCPUs gesamt* (für alle VM-Serien).
* Die zweite Ebene ist der *vCPU-Grenzwert pro VM-Serie* (z. B. vCPUs der D-Serie).
 
Wenn Sie eine neue Spot-VM bereitstellen, darf die gesamte neue und vorhandene vCPU-Nutzung für diese VM-Serie das genehmigte vCPU-Kontingent für diese bestimmte VM-Serie nicht überschreiten. Darüber hinaus sollte die Gesamtzahl der neuen und vorhandenen vCPUs, die über alle VM-Serien hinweg bereitgestellt werden, das genehmigte regionale vCPU-Gesamtkontingent für das Abonnement nicht überschreiten. Wenn eines dieser Kontingente überschritten wird, ist die VM-Bereitstellung unzulässig. 

Über das Azure-Portal können Sie eine Erhöhung der vCPU-Kontingentgrenze für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert.

Wenn Sie ein neues Abonnement erstellen, entspricht der Standardwert für „Regionale vCPUs gesamt“ möglicherweise nicht der Gesamtsumme der vCPU-Standardkontingente für alle einzelnen VM-Serien. Diese Abweichung kann zu einem Abonnement mit ausreichendem Kontingent für jede einzelne VM-Serie führen, die Sie bereitstellen möchten. Möglicherweise ist jedoch nicht genügend Kontingent vorhanden, um die Gesamtzahl der regionalen vCPUs für alle Bereitstellungen zu unterstützen. In diesem Fall müssen Sie eine Anforderung zur expliziten Erhöhung des Grenzwerts für die Gesamtzahl der regionalen vCPUs übermitteln. Der Grenzwert „Regionale vCPUs gesamt“ darf die Gesamtsumme des genehmigten Kontingents für alle VM-Serien der Region nicht überschreiten.

Weitere Informationen zu standardmäßigen vCPU-Kontingenten finden Sie unter [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste](https://aka.ms/quotalimits).

Weitere Informationen zum Erhöhen der vCPU-Grenzwerte für Spot-VMs finden Sie unter [Spot-Kontingent: Erhöhen der Grenzwerte für alle VM-Serien](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Sie können auf zwei Arten eine Erhöhung der Grenzwerte des vCPU-Standardkontingents nach Region anfordern, wie in den folgenden Abschnitten beschrieben wird.

## <a name="request-a-quota-increase-by-region-from-the-help--support-pane"></a>Anfordern einer Kontingenterhöhung nach Region über den Bereich „Hilfe und Support“

Gehen Sie folgendermaßen vor, um eine Erhöhung des vCPU-Kontingents nach Region im Bereich **Hilfe und Support** anzufordern: 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich die Option **Hilfe und Support** aus.

   ![Der Link „Hilfe und Support“](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. Wählen Sie im Bereich **Hilfe und Support** die Option **Neue Supportanfrage** aus. 

    ![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. Wählen Sie in der Dropdownliste **Problemtyp** die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Die Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. Wählen Sie in der Dropdownliste **Abonnement** das Abonnement aus, dessen Kontingent erhöht werden soll.

   ![Die Dropdownliste „Abonnement“](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Andere Anforderungen** aus.

   ![Die Dropdownliste „Kontingenttyp“](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. Geben Sie im Bereich **Problemdetails** im Feld **Beschreibung** die folgenden zusätzlichen Informationen an: 

    a. **Bereitstellungsmodell**: Geben Sie **Resource Manager** an.  
    b. **Angeforderte Region**: Geben Sie die gewünschte Region an (z. B. **USA, Osten 2**).  
    c. **Neuer Grenzwert**: Geben Sie einen neuen vCPU-Grenzwert für die Region an. Dieser Wert sollte die Summe der genehmigten Kontingente für die einzelnen SKU-Serien für dieses Abonnement nicht überschreiten.

    ![Der Bereich „Problemdetails“](./media/resource-manager-core-quotas-request/regional-details.png)

1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.

## <a name="request-a-quota-increase-by-region-from-the-subscriptions-pane"></a>Anfordern einer Kontingenterhöhung nach Region über den Bereich „Abonnements“

Gehen Sie folgendermaßen vor, um eine Erhöhung des vCPU-Kontingents nach Region im Bereich **Abonnements** anzufordern: 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich die Option **Abonnements** aus.

   ![Der Link „Abonnements“](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Wählen Sie das Abonnement aus, dessen Kontingent Sie erhöhen möchten.

   ![Der Bereich „Abonnements“](./media/resource-manager-core-quotas-request/select-subscription.png)

1. Wählen Sie im linken Bereich der Seite **\<Abonnementname>** die Option **Nutzung und Kontingente** aus.

   ![Der Link „Nutzung und Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. Wählen Sie oben rechts **Erhöhung anfordern** aus.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

1. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Andere Anforderungen** aus.

   ![Die Dropdownliste „Kontingenttyp“](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. Geben Sie im Bereich **Problemdetails** im Feld **Beschreibung** die folgenden zusätzlichen Informationen an: 

    a. **Bereitstellungsmodell**: Geben Sie **Resource Manager** an.  
    b. **Angeforderte Region**: Geben Sie die gewünschte Region an (z. B. **USA, Osten 2**).  
    c. **Neuer Grenzwert**: Geben Sie einen neuen vCPU-Grenzwert für die Region an. Dieser Wert sollte die Summe der genehmigten Kontingente für die einzelnen SKU-Serien für dieses Abonnement nicht überschreiten.

    ![Der Bereich „Problemdetails“](./media/resource-manager-core-quotas-request/regional-details.png)

1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.

