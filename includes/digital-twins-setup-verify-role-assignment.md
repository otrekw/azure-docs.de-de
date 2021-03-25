---
author: baanders
description: Includedatei zum Überprüfen der Rollenzuweisung in der Azure Digital Twins-Einrichtung
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: b4dfd154ff3fb7af48ef43b0a1dca168c5a93481
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92489036"
---
Eine Möglichkeit, um zu überprüfen, ob die Rollenzuweisung erfolgreich eingerichtet wurde, besteht darin, die Rollenzuweisungen für die Instanz von Azure Digital Twins im [Azure-Portal](https://portal.azure.com) anzuzeigen. Wechseln Sie im Azure-Portal zu Ihrer Azure Digital Twins-Instanz. (Sie können sie auf der Seite der [Azure Digital Twins-Instanzen](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) suchen oder ihren Namen auf der Suchleiste des Portals eingeben.)

Sehen Sie sich dann unter *Zugriffssteuerung (IAM) > Rollenzuweisungen* alle zugewiesenen Rollen an. Der Benutzer sollte in der Liste mit der Rolle *Azure Digital Twins Data Owner* (Azure Digital Twins-Datenbesitzer) angezeigt werden. 

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Anzeigen der Rollenzuweisungen für eine Azure Digital Twins-Instanz im Azure-Portal":::