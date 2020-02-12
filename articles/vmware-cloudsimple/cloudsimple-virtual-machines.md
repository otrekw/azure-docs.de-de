---
title: 'Azure VMware Solutions (AVS): Übersicht über virtuelle Computer'
description: Hier finden Sie Informationen zu virtuellen AVS-Computern und ihren Vorteilen.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0e6cc6d51e37cea98cc553a236295311d21f595a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024907"
---
# <a name="avs-virtual-machines-overview"></a>Übersicht über virtuelle AVS-Computer

AVS ermöglicht Ihnen die Verwaltung von virtuellen VMware-Computern über das Azure-Portal. Ein Cluster oder ein Ressourcenpool aus Ihrem vSphere-Cluster wird über Azure verwaltet, indem er Ihrem Abonnement zugeordnet wird.

Um einen virtuellen AVS-Computer in Azure zu erstellen, muss eine VM-Vorlage in der vCenter-Instanz Ihrer privaten AVS-Cloud vorhanden sein. Die Vorlage wird verwendet, um das Betriebssystem und die Anwendungen anzupassen. Die VM-Vorlage kann abgesichert werden, um die Sicherheitsrichtlinien für ein Unternehmen zu erfüllen. Sie können mit der Vorlage virtuelle Computer erstellen und diese in einem Self-Service-Modell über das Azure-Portal verwenden.

## <a name="benefits"></a>Vorteile

Virtuelle AVS-Computer im Azure-Portal bieten einen Self-Service-Mechanismus, über den Benutzer virtuelle VMware-Computer erstellen und verwalten können.

* Erstellen eines virtuellen AVS-Computers in der vCenter-Instanz Ihrer privaten AVS-Cloud
* Verwalten von VM-Eigenschaften
  * Hinzufügen/Entfernen von Datenträgern
  * Hinzufügen/Entfernen von Netzwerkadaptern (NICs)
* Ein-/Ausschaltvorgänge Ihres virtuellen AVS-Computers
  * Ein- und ausschalten
  * Zurücksetzen eines virtuellen Computers
* Löschen eines virtuellen Computers

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [VMware-VMs in Azure nutzen](quickstart-create-vmware-virtual-machine.md).
* Erfahren Sie, wie Sie [Ihre Azure-Abonnements zuordnen](azure-subscription-mapping.md).
