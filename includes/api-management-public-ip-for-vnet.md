---
author: dlepow
ms.service: api-management
ms.topic: include
ms.date: 04/12/2021
ms.author: danlep
ms.openlocfilehash: 6ed30ed2333393e9d8c0222500aaf3f17da6d33d
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107531655"
---
* **Eine standardmäßige öffentliche [SKU-IPv4-Adresse](../articles/virtual-network/public-ip-addresses.md#standard)** , wenn Ihr Client die API-Version 2021-01-01-preview oder höher verwendet. Die öffentliche IP-Adressressource ist erforderlich, wenn Sie das virtuelle Netzwerk für den externen oder internen Zugriff einrichten. Bei einem internen virtuellen Netzwerk wird die öffentliche IP-Adresse nur für Verwaltungsvorgänge verwendet. Erfahren Sie mehr über [IP-Adressen von API Management](../articles/api-management/api-management-howto-ip-addresses.md).

  * Die IP-Adresse muss sich in derselben Region und im gleichen Abonnement wie die API Management-Instanz und das virtuelle Netzwerk befinden.

  * Der Wert der IP-Adresse wird als virtuelle öffentliche IPv4-Adresse der API Management Instanz in dieser Region zugewiesen. 

  * Wenn Sie von einem externen zu einem internen virtuellen Netzwerk wechseln (oder umgekehrt), Subnetze im Netzwerk ändern oder Verfügbarkeitszonen für die API Management Instanz aktualisieren, müssen Sie eine andere öffentliche IP-Adresse konfigurieren. 

  > [!IMPORTANT]
  > Derzeit verwendet der Azure-Portal die API-Version 2021-01-01 preview beim Erstellen oder Aktualisieren einer API Management Instanz. Sie können diese API-Version mithilfe einer Azure Resource Manager-Vorlage oder der API Management REST-API angeben. Die Azure CLI und Azure PowerShell unterstützen derzeit die API-Version 2020-12-01.
