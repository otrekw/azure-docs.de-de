---
title: Verwalten der Azure NetApp Files-Abrechnung mithilfe von Tags | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die Azure NetApp Files-Abrechnung mithilfe von Tags verwalten.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/06/2021
ms.author: b-juche
ms.openlocfilehash: b0efd8c580b0d4f1ae94d4dfc655bb351e084a67
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109480873"
---
# <a name="manage-billing-by-using-capacity-pool-billing-tags"></a>Verwalten der Abrechnung mithilfe von Abrechnungstags für Kapazitätspools

Tags sind Name-Wert-Paare, die Ihnen das Kategorisieren von Ressourcen und die Anzeige einer konsolidierten Abrechnung ermöglichen. Sie können dasselbe Tag auf mehrere Ressourcen und Ressourcengruppen anwenden.  Unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen und Verwaltungshierarchie](../azure-resource-manager/management/tag-resources.md) erhalten Sie ausführliche Informationen zu Tags.  

Mithilfe von Tags können Sie die Abrechnung und Ausgaben für Azure NetApp Files verwalten. Beispiel: Ihr Unternehmen besitzt nur ein Azure-Abonnement, aber mehrere Abteilungen, die Azure-Ressourcen verwenden und Kosten verursachen. Sie können die Ressourcen mit den Abteilungsnamen auf Kapazitätspoolebene markieren. Die entsprechenden Tags werden in der Rechnung angezeigt, damit Sie die Ausgaben der einzelnen Abteilungen anzeigen können.   

Abrechnungstags werden auf Kapazitätspoolebene und nicht auf Volumeebene zugewiesen.

## <a name="steps"></a>Schritte

1. Wenn Sie einem Kapazitätspool ein Tag hinzufügen oder das Tag eines Kapazitätspools ändern möchten, wechseln Sie zum **Kapazitätspool**, und wählen Sie **Tags** aus.   

2. Geben Sie das Paar aus **Name** und **Wert** ein.  Klicken Sie auf **Anwenden**.

    > [!IMPORTANT] 
    > Tagdaten werden global repliziert. Verwenden Sie daher keine Tagnamen oder -werte, die die Sicherheit Ihrer Ressourcen gefährden könnten. Verwenden Sie beispielsweise keine Tagnamen, die personenbezogene oder vertrauliche Informationen enthalten. 

      ![Momentaufnahme: Fenster „Tags“ eines Kapazitätspools](../media/azure-netapp-files/billing-tags-capacity-pool.png)

3. Sie können Informationen zu markierten Ressourcen über das Portal [Azure Cost Management](../cost-management-billing/cost-management-billing-overview.md) anzeigen und herunterladen: 
    1. Klicken Sie auf **Kostenanalyse**, und wählen Sie die Ansicht **Kosten nach Ressource** aus.    
      [ ![Screenshot: „Kostenanalyse“ in Azure Cost Management](../media/azure-netapp-files/cost-analysis.png) ](../media/azure-netapp-files/cost-analysis.png#lightbox)  

    2. Wählen Sie zum Herunterladen einer Rechnung die Option **Rechnungen** und dann die Schaltfläche **Herunterladen** aus.   
      [ ![Screenshot: „Rechnungen“ in Azure Cost Management](../media/azure-netapp-files/azure-cost-invoices.png) ](../media/azure-netapp-files/azure-cost-invoices.png#lightbox)  

    1. Laden Sie im daraufhin angezeigten Downloadfenster die Nutzungsdetails herunter. Die heruntergeladene `csv`-Datei enthält Abrechnungstags für Kapazitätspools für die entsprechenden Ressourcen.   
       ![Momentaufnahme: Downloadfenster von Azure Cost Management](../media/azure-netapp-files/invoice-download.png)   

       [ ![Screenshot: Heruntergeladenes Arbeitsblatt](../media/azure-netapp-files/spreadsheet-download.png) ](../media/azure-netapp-files/spreadsheet-download.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

[Cost model for Azure NetApp Files (Kostenmodell für Azure NetApp Files)](azure-netapp-files-cost-model.md) 