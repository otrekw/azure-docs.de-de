---
author: baanders
description: 'Include-Datei für Azure Digital Twins: Voraussetzung zum Einrichten einer App-Registrierung'
ms.service: digital-twins
ms.topic: include
ms.date: 10/29/2020
ms.author: baanders
ms.openlocfilehash: a700f45e90334033cfc4463043017166d5f7ffd9
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111993297"
---
Für die Authentifizierung aller Ressourcen, die in diesem Artikel verwendet werden, müssen Sie eine **[App-Registrierung für Azure Active Directory (Azure AD)](../articles/active-directory/fundamentals/active-directory-whatis.md) einrichten**. Führen Sie die Anweisungen unter  [Erstellen einer App-Registrierung](../articles/digital-twins/how-to-create-app-registration-portal.md) aus, um diese einzurichten. 

Wenn Sie über eine App-Registrierung verfügen, benötigen Sie die **Anwendungs-ID (Client)** , **Verzeichnis-ID (Mandant)** und den **geheimen Clientschlüssel** ([Ermittlung über das Azure-Portal](../articles/digital-twins/how-to-create-app-registration-portal.md#collect-important-values)) der Registrierung. Notieren Sie sich diese Werte, damit Sie sie später zum Gewähren des Zugriffs auf die Azure Digital Twins-APIs verwenden können.