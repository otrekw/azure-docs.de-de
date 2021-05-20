---
author: baanders
description: Includedatei für die Schrittübersicht beim Setup von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: 19d7c55a669a3975cb4bc1dce707e2d1165be0b3
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108759926"
---
Das vollständige Setup für eine neue Azure Digital Twins-Instanz umfasst zwei Teile:
1. **Erstellen der Instanz**
2. **Einrichten von Benutzerzugriffsberechtigungen:** Azure-Benutzer müssen über die Rolle **Azure Digital Twins Data Owner** (Azure Digital Twins-Datenbesitzer) auf der Azure Digital Twins-Instanz verfügen, um sie und ihre Daten verwalten zu können. In diesem Schritt weisen Sie als Besitzer/Administrator des Azure-Abonnements diese Rolle der Person zu, die Ihre Azure Digital Twins-Instanz verwalten soll. Dies können Sie selbst sein oder eine andere Person in Ihrer Organisation.
 
>[!IMPORTANT]
>Um diesen vollständigen Artikel abzuschließen und eine verwendbare Instanz vollständig einzurichten, benötigen Sie Berechtigungen zum Verwalten von Ressourcen und Benutzerzugriff für das Azure-Abonnement. Der erste Schritt kann von allen Personen ausgeführt werden, die Ressourcen für das Abonnement erstellen können, aber der zweite Schritt erfordert Berechtigungen für die Benutzerzugriffsverwaltung (oder die Zusammenarbeit eines Benutzers mit diesen Berechtigungen). Weitere Informationen hierzu finden Sie im Abschnitt [Voraussetzungen: Erforderliche Berechtigungen](#prerequisites-permission-requirements) für den Schritt „Benutzerzugriffsberechtigung“.
