---
title: Spot-VM-Kontingent | Microsoft-Dokumentation
description: Erhöhen der Kontingentgrenzwerte durch Anfordern von Spot-Kontingenten
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9567df95e4e66d9c6b82b29cb9bba9adaf39349d
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898862"
---
# <a name="spot-quota-increase-limits-for-all-vm-series"></a>Spot-Kontingent: Erhöhen der Grenzwerte für alle VM-Serien

Spot-VMs bieten ein anderes Modell der Azure-Nutzung. Sie können zu niedrigeren Kosten führen. Dafür entfernt Azure jedoch nach Bedarf VMs für Bereitstellungen mit nutzungsbasierter Bezahlung oder reservierten VM-Instanzen. Weitere Informationen zu Spot-VMs finden Sie unter [Azure Spot-VMs für Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

Azure Resource Manager unterstützt zwei Arten von vCPU-Kontingenten für virtuelle Computer:
* *VMs mit nutzungsbasierter Bezahlung* und *reservierte VM-Instanzen* verwenden *vCPU-Standardkontingente*.
* *Spot-VMs* unterliegen einem *Spot-vCPU-Kontingent*. 

Beim *Spot-Kontingenttyp* werden vCPU-Kontingente von Resource Manager für alle verfügbaren VM-Serien als ein einzelner regionaler Grenzwert erzwungen.

Wenn Sie eine neue Spot-VM bereitstellen, darf die gesamte neue und vorhandene vCPU-Nutzung für alle Spot-VM-Instanzen die genehmigte Spot-vCPU-Kontingentgrenze nicht überschreiten. Wenn das Spot-Kontingent überschritten wird, ist die Bereitstellung der Spot-VM unzulässig. 

In diesem Artikel wird erläutert, wie Sie mithilfe des Azure-Portals eine Erhöhung des vCPU-Kontingentgrenzwerts anfordern. 

Weitere Informationen zu vCPU-Standardkontingenten finden Sie unter [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste](https://aka.ms/quotalimits). 

Weitere Informationen zum Erhöhen des vCPU-Grenzwerts nach Region finden Sie unter [Standardkontingent: Erhöhung des regionalen vCPU-Grenzwerts](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

## <a name="request-a-quota-limit-increase-from-the-help--support-pane"></a>Anfordern einer Erhöhung des Kontingentgrenzwerts über den Bereich „Hilfe und Support“ 

Gehen Sie folgendermaßen vor, um eine Erhöhung des Spot-Kontingentgrenzwerts für alle VM-Serien über den Bereich **Hilfe und Support** anzufordern:

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

   ![Der Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/3-7.png)

   a. Wählen Sie in der Dropdownliste **Bereitstellungsmodell** das entsprechende Modell aus.

   b. Wählen Sie in der Dropdownliste **Standorte** einen Standort aus. Geben Sie für den ausgewählten Standort unter **Typen** im Feld **Typ auswählen** die Angabe **Spot** ein. 
   
   ![Die Detailregisterkarte „Neue Supportanfrage“](./media/resource-manager-core-quotas-request/3-8.png)

    Sie können sowohl Standardkontingenttypen als auch Spot-Kontingenttypen über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl unter **Typen** verwenden. 
    
    Weitere Informationen finden Sie unter [Standardkontingent: Erhöhen des vCPU-Grenzwerts pro VM-Serie](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   c. Geben Sie den neuen gewünschten Kontingentgrenzwert für dieses Abonnement ein. 
 
   ![Das Textfeld „Neuer vCPU-Grenzwert“](./media/resource-manager-core-quotas-request/3-9.png)

1. Um eine Kontingenterhöhung für mehrere Standorte anzufordern, wählen Sie einen weiteren Standort in der Dropdownliste aus, und wählen Sie dann einen geeigneten VM-Typ aus. Sie können dann einen Grenzwert eingeben, der für den zusätzlichen Standort gilt.

   ![Zusätzliche Standorte im Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/3-10.png)

1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.

## <a name="request-a-quota-limit-increase-from-the-subscriptions-pane"></a>Anfordern einer Erhöhung des Kontingentgrenzwerts über den Bereich „Abonnements“

Gehen Sie folgendermaßen vor, um eine Erhöhung des Spot-Kontingentgrenzwerts für alle VM-Serien über den Bereich **Abonnements** anzufordern:

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

   ![Der Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/3-2-6.png)
 
   a. Wählen Sie in der Dropdownliste **Bereitstellungsmodell** das entsprechende Modell aus.

   b. Wählen Sie in der Dropdownliste **Standorte** einen Standort aus. 
   
   c. Geben Sie für den ausgewählten Standort unter **Typen** im Feld **Typ auswählen** die Angabe **Spot** ein.

   ![Der Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/3-2-7.png)

   Weitere Informationen finden Sie unter [Standardkontingent: Erhöhen des vCPU-Grenzwerts pro VM-Serie](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   d. Geben Sie den neuen gewünschten Kontingentgrenzwert für dieses Abonnement ein.

   ![Das Textfeld „Neuer vCPU-Grenzwert“](./media/resource-manager-core-quotas-request/3-2-8.png)
 
1. Um eine Kontingenterhöhung für mehrere Standorte anzufordern, wählen Sie einen weiteren Standort in der Dropdownliste aus, und wählen Sie dann einen geeigneten VM-Typ aus. Sie können dann einen Grenzwert eingeben, der für den zusätzlichen Standort gilt.

   ![Zusätzliche Standorte im Bereich „Kontingentdetails“](./media/resource-manager-core-quotas-request/3-2-9.png)
 
1. Wählen Sie **Speichern und fortfahren** aus, um die Erstellung der Supportanfrage fortzusetzen.


