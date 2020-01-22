---
title: Anfordern einer Erhöhung des vCPU-Kontingentgrenzwerts pro Azure-VM-Serie | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Kontingentgrenzwerte pro VM-vCPU angefordert werden.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: fc59ffda245834c716067bc63e6c3745fa5d23b3
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898846"
---
# <a name="standard-quota-increase-limits-by-vm-series"></a>Standardkontingent: Erhöhen der Grenzwerte nach VM-Serie

Azure Resource Manager unterstützt zwei Arten von vCPU-Kontingenten für virtuelle Computer:
* *VMs mit nutzungsbasierter Bezahlung* und *reservierte VM-Instanzen* verwenden *vCPU-Standardkontingente*.
* *Spot-VMs* unterliegen einem *Spot-vCPU-Kontingent*. 

Das standardmäßige vCPU-Kontingent für nutzungsbasierte Bezahlung und reservierte VM-Instanzen wird für jedes Abonnement in jeder Region auf zwei Ebenen erzwungen:
* Die erste Ebene ist der *Grenzwert regionaler vCPUs gesamt* (für alle VM-Serien).
* Die zweite Ebene ist der *vCPU-Grenzwert pro VM-Serie* (z. B. vCPUs der Dv3-Serie). 

Wenn Sie eine neue Spot-VM bereitstellen, darf die gesamte neue und vorhandene vCPU-Nutzung für diese VM-Serie das genehmigte vCPU-Kontingent für diese bestimmte VM-Serie nicht überschreiten. Darüber hinaus sollte die Gesamtzahl der neuen und vorhandenen vCPUs, die über alle VM-Serien hinweg bereitgestellt werden, das genehmigte regionale vCPU-Gesamtkontingent für das Abonnement nicht überschreiten. Wenn eines dieser Kontingente überschritten wird, ist die VM-Bereitstellung unzulässig.

Über das Azure-Portal können Sie eine Erhöhung der vCPU-Kontingentgrenze für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert. 

Weitere Informationen zu vCPU-Standardkontingenten finden Sie unter [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests). 

Weitere Informationen zum Erhöhen des vCPU-Grenzwerts nach Region für das Standardkontingent finden Sie unter [Standardkontingent: Erhöhen der Grenzwerte nach Region](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests). 

Weitere Informationen zum Erhöhen der vCPU-Grenzwerte für Spot-VMs finden Sie unter [Spot-Kontingent: Erhöhen der Grenzwerte für alle VM-Serien](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Sie können auf zwei Arten eine Erhöhung der Grenzwerte des vCPU-Standardkontingents pro VM-Serie anfordern, wie in den folgenden Abschnitten beschrieben wird.

## <a name="request-a-standard-quota-increase-from-the-help--support-pane"></a>Anfordern einer Erhöhung des Standardkontingents über den Bereich „Hilfe und Support“

Gehen Sie folgendermaßen vor, um eine Erhöhung des Standardkontingents pro VM-Serie über den Bereich **Hilfe und Support** anzufordern: 

> [!NOTE]
> Sie können auch eine Erhöhung der Kontingentgrenzwerte für mehrere Regionen über eine einzige Supportanfrage anfordern. Einzelheiten dazu finden Sie in Schritt 8.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich die Option **Hilfe und Support** aus.

   ![Der Link „Hilfe und Support“](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. Wählen Sie im Bereich **Hilfe und Support** die Option **Neue Supportanfrage** aus. 

    ![Neue Supportanfrage](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. Wählen Sie in der Dropdownliste **Problemtyp** die Option **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Die Dropdownliste „Problemtyp“](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. Wählen Sie in der Dropdownliste **Abonnement** das Abonnement aus, dessen Kontingent erhöht werden soll.

   ![Die Dropdownliste „Abonnement“](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)** aus. 

   ![Die Dropdownliste „Kontingenttyp“](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Wählen Sie die Registerkarte **Details** aus, und klicken Sie dann unter **Problemdetails** auf **Details angeben**, und geben Sie dann zusätzliche Informationen ein, um Ihre Anforderung zu verarbeiten.

   ![Der Link „Details angeben“](./media/resource-manager-core-quotas-request/provide-details.png)

1. Gehen Sie im Bereich **Kontingentdetails** oben rechts folgendermaßen vor:

   ![Der Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/1-7.png)

   a. Wählen Sie in der Dropdownliste **Bereitstellungsmodell** das entsprechende Modell aus.

   b. Wählen Sie in der Dropdownliste **Standorte** einen Standort aus. Geben Sie für den ausgewählten Standort unter **Typen** im Feld **Typ auswählen** die Angabe **Standard** ein.

   ![Der Bereich „Kontingentdetails“: Kontingenttypen](./media/resource-manager-core-quotas-request/1-8.png)

   Sie können sowohl Standardkontingenttypen als auch Spot-Kontingenttypen über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl unter **Typen** verwenden.
   
   Weitere Informationen zum Erhöhen des Spot-Kontingentgrenzwerts finden Sie unter [Azure Spot-VMs für Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   c. Wählen Sie unter der Dropdownliste **Standard** die SKU-Serie aus, deren Kontingente Sie erhöhen möchten.

   ![Der Bereich „Kontingentdetails“: SKU-Serie](./media/resource-manager-core-quotas-request/1-9.png)

   d. Geben Sie die neuen gewünschten Kontingentgrenzwerte für dieses Abonnement ein. Um eine SKU aus der Liste zu entfernen, deaktivieren Sie das Kontrollkästchen neben der SKU, oder wählen Sie das Symbol **Löschen** (X) aus. 

   ![Das Textfeld „Neuer vCPU-Grenzwert“](./media/resource-manager-core-quotas-request/1-10.png)

1. Um eine Kontingenterhöhung für mehrere Standorte anzufordern, wählen Sie einen weiteren Standort in der Dropdownliste aus, und wählen Sie dann einen geeigneten VM-Typ aus. Sie können dann einen Grenzwert eingeben, der für den zusätzlichen Standort gilt.

   ![Zusätzliche Standorte im Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/1-11.png)
   
1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.

## <a name="request-a-standard-quota-increase-from-the-subscriptions-pane"></a>Anfordern einer Erhöhung des Standardkontingents über den Bereich „Abonnements“

Gehen Sie folgendermaßen vor, um eine Erhöhung des Standardkontingents pro VM-Serie über den Bereich **Abonnements** anzufordern:

> [!NOTE]
> Sie können auch eine Erhöhung der Kontingentgrenzwerte für mehrere Regionen über eine einzige Supportanfrage anfordern. Einzelheiten dazu finden Sie in Schritt 7.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich die Option **Abonnements** aus.

   ![Der Link „Abonnements“](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Wählen Sie das Abonnement aus, dessen Kontingent Sie erhöhen möchten.

   ![Der Bereich „Abonnements“](./media/resource-manager-core-quotas-request/select-subscription.png)

1. Wählen Sie im linken Bereich der Seite **\<Abonnementname>** die Option **Nutzung und Kontingente** aus.

   ![Der Link „Nutzung und Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. Wählen Sie oben rechts **Erhöhung anfordern** aus.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

1. Wählen Sie in der Dropdownliste **Kontingenttyp** die Option **Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)** aus.

   ![Die Dropdownliste „Kontingenttyp“](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
1. Gehen Sie im Bereich **Kontingentdetails** oben rechts folgendermaßen vor:

   ![Der Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/1-1-6.png)

   a. Wählen Sie in der Dropdownliste **Bereitstellungsmodell** das entsprechende Modell aus.

   b. Wählen Sie in der Dropdownliste **Standorte** einen Standort aus. 
   
   c. Wählen Sie für den ausgewählten Standort unter **Typen** die Option **Typ auswählen** aus, und aktivieren Sie dann das Kontrollkästchen **Standard**.

   ![Das Kontrollkästchen „Standard“](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   Unter **Typen** können Sie sowohl Standardkontingenttypen als auch Kontingenttypen mit niedriger Priorität über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl verwenden.
   
   Weitere Informationen zum Erhöhen des Spot-Kontingentgrenzwerts finden Sie unter [Azure Spot-VMs für Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   d. Wählen Sie unter der Dropdownliste **Standard** die SKU-Serie aus, deren Kontingente Sie erhöhen möchten.

   ![Der Bereich „Kontingentdetails“: SKU-Serie](./media/resource-manager-core-quotas-request/1-1-8.png)

   e. Geben Sie die neuen gewünschten Kontingentgrenzwerte für dieses Abonnement ein. Um eine SKU aus der Liste zu entfernen, deaktivieren Sie das Kontrollkästchen neben der SKU, oder wählen Sie **Löschen** (X) aus. 

   ![Das Textfeld „Neuer vCPU-Grenzwert“](./media/resource-manager-core-quotas-request/1-1-9.png)
   
1. Um eine Kontingenterhöhung für mehrere Standorte anzufordern, wählen Sie einen weiteren Standort in der Dropdownliste aus, und wählen Sie dann einen geeigneten VM-Typ aus. 

   In diesem Schritt wird die SKU-Serie vorab geladen, die Sie für frühere Standorte ausgewählt haben. Geben Sie die Kontingentgrenzwerte ein, die Sie auf die zusätzliche Serie anwenden möchten.
   
   ![Zusätzliche Standorte im Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/1-1-10.png)
 
1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.
