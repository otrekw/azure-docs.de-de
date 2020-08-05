---
author: baanders
description: Includedatei zum Überprüfen der Rollenzuweisung in der Azure Digital Twins-Einrichtung
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: e651b02bf72ced58b6cba637a68ace3258514176
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405580"
---
Eine Möglichkeit, um zu überprüfen, ob die Rollenzuweisung erfolgreich eingerichtet wurde, besteht darin, die Rollenzuweisungen für die Instanz von Azure Digital Twins im [Azure-Portal](https://portal.azure.com) anzuzeigen. Wechseln Sie zur Portalseite für [Azure Digital Twins-Instanzen](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) (Sie können dazu diesen Link oder die Suchleiste des Portals verwenden), und wählen Sie den Namen der Instanz aus, die Sie überprüfen möchten. 

Sehen Sie sich dann unter *Zugriffssteuerung (IAM) > Rollenzuweisungen* alle zugewiesenen Rollen an. Der Benutzer sollte in der Liste mit der Rolle *Azure Digital Twins-Besitzer (Vorschau)* angezeigt werden. 

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Anzeigen der Rollenzuweisungen für eine Azure Digital Twins-Instanz im Azure-Portal":::