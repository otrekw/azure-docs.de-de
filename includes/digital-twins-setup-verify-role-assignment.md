---
author: baanders
description: Includedatei zum Überprüfen der Rollenzuweisung in der Azure Digital Twins-Einrichtung
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: 044342122c2bd1d2b59a7fc3abb4ed6ca7bbc05f
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110473831"
---
Eine Möglichkeit, um zu überprüfen, ob die Rollenzuweisung erfolgreich eingerichtet wurde, besteht darin, die Rollenzuweisungen für die Instanz von Azure Digital Twins im [Azure-Portal](https://portal.azure.com) anzuzeigen. Wechseln Sie im Azure-Portal zu Ihrer Azure Digital Twins-Instanz. (Sie können sie auf der Seite der [Azure Digital Twins-Instanzen](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) suchen oder ihren Namen auf der Suchleiste des Portals eingeben.)

Sehen Sie sich dann unter *Zugriffssteuerung (IAM) > Rollenzuweisungen* alle zugewiesenen Rollen an. Der Benutzer sollte in der Liste mit der Rolle *Azure Digital Twins Data Owner* (Azure Digital Twins-Datenbesitzer) angezeigt werden. 

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Screenshot der Rollenzuweisungen für eine Azure Digital Twins-Instanz im Azure-Portal":::