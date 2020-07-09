---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/20/2020
ms.author: cephalin
ms.openlocfilehash: c3fa57dd162fbbfbf0d46f73bffc78f279ef2968
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83649136"
---
* **Anmeldeinformationen auf Benutzerebene**: ein Satz von Anmeldeinformationen für das gesamte Azure-Konto. Hiermit können Sie App Service für alle Apps in allen Abonnements bereitstellen, für die das Azure-Konto über Zugriffsberechtigungen verfügt. Dies ist der Standardsatz, der auf der Benutzeroberfläche des Portals angezeigt wird (z.B. **Übersicht** und **Eigenschaften** auf der [Ressourcenseite](../articles/azure-resource-manager/management/manage-resources-portal.md#manage-resources) der App). Wenn ein Benutzer App-Zugriff über rollenbasierte Zugriffssteuerung (RBAC) oder Co-Administrator-Berechtigungen erhält, kann er die eigenen Anmeldeinformationen auf Benutzerebene verwenden, bis der Zugriff widerrufen wird. Teilen Sie diese Anmeldeinformationen nicht mit anderen Azure-Benutzern.

* **Anmeldeinformationen auf App-Ebene**: ein Satz von Anmeldeinformationen für jede App. Er kann nur verwendet werden, um diese App bereitzustellen. Die Anmeldeinformationen für eine App werden bei der Erstellung jeder App automatisch generiert. Sie können nicht manuell konfiguriert, jedoch jederzeit zurückgesetzt werden. Damit ein Benutzer Zugriff auf Anmeldeinformationen auf App-Ebene über die rollenbasierte Zugriffssteuerung (RBAC) erhalten kann, muss er in der App mindestens ein Mitwirkender sein (einschließlich der integrierten Rolle „Websitemitwirkender“). Benutzer mit Leseberechtigung dürfen nicht veröffentlichen und haben keinen Zugriff auf diese Anmeldeinformationen.