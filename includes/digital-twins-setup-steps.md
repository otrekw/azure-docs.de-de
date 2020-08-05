---
author: baanders
description: Includedatei für die Schrittübersicht beim Setup von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/22/2020
ms.author: baanders
ms.openlocfilehash: b8751685cfd273171cb8a5b500e4bed41ce2f59e
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407521"
---
>[!NOTE]
>Diese Vorgänge sollen von einem Benutzer mit der Rolle *Besitzer* für das Azure-Abonnement abgeschlossen werden. Obwohl einige Teile ohne diese erhöhte Berechtigung durchgeführt werden können, kann eine verwendbare Instanz nur in Zusammenarbeit mit einem Besitzer vollständig eingerichtet werden. Weitere Informationen hierzu finden Sie im Abschnitt [*Voraussetzungen: Erforderliche Berechtigungen*](#prerequisites-permission-requirements) weiter unten.

Das vollständige Setup für eine neue Azure Digital Twins-Instanz umfasst drei Teile:
1. **Erstellen der Instanz**
2. **Einrichten von Benutzerzugriffsberechtigungen:** Azure-Benutzer müssen über die Rolle *Azure Digital Twins-Besitzer (Vorschau)* auf der Azure Digital Twins-Instanz verfügen, um sie und ihre Daten verwalten zu können. In diesem Schritt weisen Sie als Besitzer im Azure-Abonnement diese Rolle der Person zu, die Ihre Azure Digital Twins-Instanz verwalten soll. Dies können Sie selbst sein oder eine andere Person in Ihrer Organisation.
3. **Einrichten von Zugriffsberechtigungen für Clientanwendungen:** Es ist üblich, eine Clientanwendung zu schreiben, die für die Interaktion mit Ihrer Instanz verwendet werden soll. Damit diese Client-App auf Ihre Azure Digital Twins-Instanz zugreifen kann, müssen Sie eine *App-Registrierung* in [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md) einrichten, mit der sich die Clientanwendung bei der Instanz authentifiziert.

Zum Fortfahren benötigen Sie ein Azure-Abonnement. Sie können [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ein kostenloses einrichten.
