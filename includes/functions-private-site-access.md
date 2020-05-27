---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 5e0cff7bde6e80a776d694820ca7b69dafa7c0d9
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648816"
---
Privater Websitezugriff bedeutet, dass der Zugriff auf Ihre App nur über ein privates Netzwerk zugelassen wird, z. B. über ein virtuelles Azure-Netzwerk.

* Zugriff auf private Sites ist im [Premium-Tarif](../articles/azure-functions/functions-premium-plan.md), [Verbrauchstarif](../articles/azure-functions/functions-scale.md#consumption-plan) und [App Service-Plan](../articles/azure-functions/functions-scale.md#app-service-plan) verfügbar, wenn Dienstendpunkte konfiguriert sind.
    * Dienstendpunkte können unter **Plattformfeatures** > **Netzwerk** > **Zugriffseinschränkungen konfigurieren** > **Regel hinzufügen** pro App konfiguriert werden. Virtuelle Netzwerke können nun als Regeltyp ausgewählt werden.
    * Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../articles/virtual-network/virtual-network-service-endpoints-overview.md).
    * Bedenken Sie, dass Ihre Funktion auch mit Dienstendpunkten immer noch vollständigen ausgehenden Zugriff auf das Internet besitzt, selbst wenn die virtuelle Netzwerkintegration konfiguriert ist.
* Der private Websitezugriff ist auch verfügbar, wenn eine App Service-Umgebung mit internen Lastenausgleich (ILB) konfiguriert ist. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../articles/app-service/environment/create-ilb-ase.md).

Informationen zum Einrichten des privaten Websitezugriffs finden Sie unter [Tutorial: Einrichten von privatem Websitezugriff für Azure Functions](../articles/azure-functions/functions-create-private-site-access.md).