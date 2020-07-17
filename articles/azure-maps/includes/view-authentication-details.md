---
title: Anzeigen von Authentifizierungsdetails von Azure Maps
description: Verwenden Sie das Azure-Portal zum Anzeigen von Authentifizierungsdetails von Azure Maps.
author: philmea
ms.author: philmea
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 9146844a6b83f78ad99ef7cd1aec4b028daf3ff6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988366"
---
Sie können die Azure Maps-Kontoauthentifizierungsdetails im Azure-Portal anzeigen. Wählen Sie dort in Ihrem Konto im Menü **Einstellungen** die Option **Authentifizierung** aus.

![Authentifizierungsdetails](../media/how-to-manage-authentication/how-to-view-auth.png)

Sobald ein Azure Maps-Konto erstellt wird, wird der `x-ms-client-id`-Wert von Azure Maps auf der Authentifizierungsdetailseite des Azure-Portals angezeigt. Dieser Wert stellt das Konto dar, das für REST-API-Anforderungen verwendet wird. Dieser Wert sollte in der Anwendungskonfiguration gespeichert und abgerufen werden, bevor HTTP-Anforderung ausgeführt werden, wenn Azure AD-Authentifizierung mit Azure Maps verwendet wird.
