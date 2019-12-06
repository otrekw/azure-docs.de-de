---
title: Anforderungen zur Erhöhung von Azure-vCPU-Kontingenten pro VM-Serie | Microsoft-Dokumentation
description: Anforderungen zur Erhöhung von vCPU-Kontingenten pro VM-Serie
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: ccd0c88c95ae9a752ef8ea2387bbde4f8559bc68
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74531698"
---
# <a name="standard-quota-per-vm-series-vcpu-limit-increase"></a>Standardkontingent: Erhöhung des vCPU-Grenzwerts pro VM-Serie

Der Ressourcen-Manager unterstützt zwei Arten von vCPU-Kontingenten für virtuelle Computer. **VMs mit nutzungsbasierter Bezahlung und reservierte VM-Instanzen** verwenden Standardkontingente. **VMs mit niedriger Priorität** verwenden Kontingente mit niedriger Priorität. Die standardmäßigen vCPU-Kontingente für nutzungsbasierte Bezahlung und reservierte VM-Instanzen werden für jedes Abonnement in jeder Region auf zwei Ebenen erzwungen.

Die erste Ebene ist der Grenzwert **Regionale vCPUs gesamt** (für alle VM-Serien). Die zweite Ebene ist der Grenzwert **vCPUs pro VM-Serie** (z.B. vCPUs der Dv3-Serie). Bei jeder Bereitstellung eines neuen virtuellen Computers darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für diese VM-Serie das für diese spezielle VM-Serie genehmigte vCPU-Kontingent nicht überschreiten. Ferner darf die Gesamtzahl der neuen und vorhandenen vCPUs, die insgesamt für alle VM-Serien bereitgestellt werden, das für das Abonnement genehmigte Kontingent „Regionale vCPUs gesamt“ nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen.
Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert. 

Weitere Informationen zu standardmäßigen vCPU-Kontingenten finden Sie auf den Seiten [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests). 

Weitere Informationen zum Erhöhen des regionalen vCPU-Grenzwerts für Standardkontingente finden Sie [hier](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests). 

Weitere Informationen zum **Erhöhen von vCPU-Grenzwerten für VMs mit niedriger Priorität** finden Sie [hier](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Sie können eine Erhöhung der **vCPU-Grenzwerte für Standardkontingente pro VM-Serie** über das Blatt **Hilfe und Support** oder das Blatt **Nutzung und Kontingente** im Portal anfordern.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-the-help--support-blade"></a>Anfordern einer Erhöhung des standardmäßigen vCPU-Kontingents pro VM-Serie auf Abonnementebene über das Blatt „Hilfe und Support“

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Hilfe und Support“ eine Supportanfrage zu erstellen. 

Sie können auch Kontingente für mehrere Regionen über eine einzige Supportanfrage anfordern. Weitere Informationen finden Sie weiter unten in Schritt 11.

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

7. Wählen Sie im Bereich **Kontingentdetails** das **Bereitstellungsmodell** und einen **Standort** aus.

   ![Kontingentdetails (Bereitstellungsmodell)](./media/resource-manager-core-quotas-request/1-7.png)

8. Wählen Sie für den ausgewählten Speicherort den Wert **Typ** als **Standard** aus. Sie können sowohl Standardkontingenttypen als auch Kontingenttypen mit niedriger Priorität über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl im Feld **Typ** verwenden. Weitere Informationen zum **Erhöhen von Kontingentgrenzwerten für VMs mit niedriger Priorität** finden Sie auf **Seite <>** .

   ![SKU-Familie](./media/resource-manager-core-quotas-request/1-8.png)

9. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist.

   ![SKU-Familie](./media/resource-manager-core-quotas-request/1-9.png)

10. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. 

   ![Neue Grenzwerte](./media/resource-manager-core-quotas-request/1-10.png)

11. Um ein Kontingent für mehr als einen Standort anzufordern, können Sie in der Dropdownliste einen anderen **Standort** und den entsprechenden VM-Typ auswählen. In diesem Schritt werden die für einen früheren **Standort** ausgewählten SKU-Familien vorab für den neuen Standort geladen, und Sie können einfach die neuen gewünschten Grenzwerte eingeben.

   ![Mehrere Standorte](./media/resource-manager-core-quotas-request/1-11.png)
   
12. Nachdem Sie die gewünschten Kontingente für die einzelnen SKU-Familien eingegeben haben, klicken Sie im Bereich mit den Kontingentdetails auf **Speichern und fortfahren**, um die Erstellung der Supportanfrage fortzusetzen.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-usages--quota-blade"></a>Anfordern einer standardmäßigen vCPU-Kontingenterhöhung pro VM-Serie auf Abonnementebene über das Blatt „Nutzung und Kontingente“

Führen Sie die folgenden Schritte aus, um im Azure-Portal über das Blatt „Nutzung + Kontingente“ eine Supportanfrage zu erstellen.

Sie können auch **Kontingente für mehrere Regionen** über eine einzige Supportanfrage anfordern. Weitere Informationen finden Sie weiter unten in Schritt 10.

1. Klicken Sie unter https://portal.azure.com auf **Abonnements**.

   ![Abonnements](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Wählen Sie das Abonnement aus, für das ein höheres Kontingent benötigt wird.

   ![Wählen Sie das Abonnement aus.](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Wählen Sie **Nutzung + Kontingente** aus.

   ![Auswählen von „Nutzung + Kontingente“](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. Klicken Sie in der Ecke oben rechts auf **Erhöhung anfordern**.

   ![Anfordern einer Erhöhung](./media/resource-manager-core-quotas-request/request-increase.png)

5. Wählen Sie als Kontingenttyp die Option **Compute-VM (cores/vCPUs) subscription limit increases** (Erhöhung der Grenzwerte für Compute-/VM-Abonnements (Kerne/vCPUs)) aus. 

   ![Ausfüllen des Formulars](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
6. Wählen Sie im Bereich **Kontingentdetails** das Bereitstellungsmodell und einen Standort aus.

   ![Blatt für das Kontingentproblem](./media/resource-manager-core-quotas-request/1-1-6.png)

7. Wählen Sie für den ausgewählten Speicherort den Wert **Typ** als **Standard** aus. Sie können sowohl Standardkontingenttypen als auch Kontingenttypen mit niedriger Priorität über eine einzige Supportanfrage anfordern, indem Sie die Mehrfachauswahl im Feld **Typ** verwenden. Weitere Informationen zum **Erhöhen von vCPU-Grenzwerten für VMs mit niedriger Priorität** finden Sie diese [Seite](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

   ![Ausgewählte SKU-Serie](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   
8. Wählen Sie die **SKU-Familien** aus, für die eine Erhöhung erforderlich ist.

   ![Ausgewählte SKU-Serie](./media/resource-manager-core-quotas-request/1-1-8.png)

9. Geben Sie die gewünschten Grenzwerte für das Abonnement ein. Wenn Sie eine Zeile entfernen möchten, deaktivieren Sie die SKU über die Dropdownliste für die SKU-Familie, oder klicken Sie auf das X-Symbol. 

   ![Neue SKU-Kontingentanforderung](./media/resource-manager-core-quotas-request/1-1-9.png)
   

10. Um ein Kontingent für mehr als einen Standort anzufordern, können Sie in der Dropdownliste einen anderen **Standort** und den entsprechenden VM-Typ auswählen. In diesem Schritt werden die für einen früheren **Standort** ausgewählten SKU-Familien vorab für den neuen Standort geladen, und Sie können einfach die neuen gewünschten Grenzwerte eingeben.
   
    ![Neue SKU-Kontingentanforderung](./media/resource-manager-core-quotas-request/1-1-10.png)
 
