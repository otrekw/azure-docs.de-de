---
title: Anzeigen von Authentifizierungsdetails von Azure Maps
description: Verwenden Sie das Azure-Portal zum Anzeigen von Authentifizierungsdetails von Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a6ffcbf5a8c36958dd3ea74de4d826fe25a1139c
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126453"
---
Sie können die Azure Maps-Kontoauthentifizierungsdetails im Azure-Portal anzeigen. Wählen Sie dort in Ihrem Konto im Menü **Einstellungen** die Option **Authentifizierung** aus.

![Authentifizierungsdetails](../media/how-to-manage-authentication/how-to-view-auth.png)

Sobald ein Azure Maps-Konto erstellt wird, wird der `x-ms-client-id`-Wert von Azure Maps auf der Authentifizierungsdetailseite des Azure-Portals angezeigt. Dieser Wert stellt das Konto dar, das für REST-API-Anforderungen verwendet wird. Dieser Wert sollte in der Anwendungskonfiguration gespeichert und abgerufen werden, bevor HTTP-Anforderung ausgeführt werden, wenn Azure AD-Authentifizierung mit Azure Maps verwendet wird.
