---
author: baanders
description: Includedatei zum Überprüfen der Rollenzuweisung in der Azure Digital Twins-Einrichtung
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: fdb3bd034d93e623037be9fa0721a805924af5c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88864630"
---
Eine Möglichkeit, um zu überprüfen, ob die Rollenzuweisung erfolgreich eingerichtet wurde, besteht darin, die Rollenzuweisungen für die Instanz von Azure Digital Twins im [Azure-Portal](https://portal.azure.com) anzuzeigen. Wechseln Sie im Azure-Portal zu Ihrer Azure Digital Twins-Instanz. (Sie können sie auf der Seite der [Azure Digital Twins-Instanzen](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) suchen oder ihren Namen auf der Suchleiste des Portals eingeben.)

Sehen Sie sich dann unter *Zugriffssteuerung (IAM) > Rollenzuweisungen* alle zugewiesenen Rollen an. Der Benutzer sollte in der Liste mit der Rolle *Azure Digital Twins-Besitzer (Vorschau)* angezeigt werden. 

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Anzeigen der Rollenzuweisungen für eine Azure Digital Twins-Instanz im Azure-Portal":::