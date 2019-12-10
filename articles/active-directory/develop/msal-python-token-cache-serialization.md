---
title: Benutzerdefinierte Tokencacheserialisierung in MSAL für Python | Azure
titleSuffix: Microsoft identity platform
description: In diesem Artikel erfahren Sie, wie Sie den Tokencache für MSAL für Python serialisieren.
services: active-directory
documentationcenter: dev-center-name
author: rayluo
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2019
ms.author: rayluo
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae6628e1fd4929723f11a2195136f94532a4ed76
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74152279"
---
# <a name="custom-token-cache-serialization-in-msal-for-python"></a>Benutzerdefinierte Tokencacheserialisierung in MSAL für Python

In MSAL Python wird standardmäßig für die Dauer der App-Sitzung ein speicherinterner Cache beibehalten, wenn Sie eine Instanz von [ClientApplication](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication) erstellen.

Die Serialisierung des Tokencaches, sodass verschiedene Sitzungen ihrer App darauf zugreifen können, wird nicht standardmäßig bereitgestellt. Dies liegt daran, dass MSAL Python in App-Typen verwendet werden kann, die über keinen Zugriff auf das Dateisystem verfügen, z. B. Web-Apps. Um einen persistenten Tokencache in einer MSAL Python-App zu erhalten, müssen Sie die benutzerdefinierte Tokencacheserialisierung bereitstellen.

Die Strategien für das Serialisieren des Tokencaches variieren in Abhängigkeit davon, ob Sie eine öffentliche Clientanwendung (Desktop) oder eine vertrauliche Clientanwendung (Web-App, Web-API oder Daemon-App) schreiben.

## <a name="token-cache-for-a-public-client-application"></a>Tokencache für eine öffentliche Clientanwendung

Öffentliche Clientanwendungen werden auf dem Gerät eines Benutzers ausgeführt und verwalten Token für einen einzelnen Benutzer. In diesem Fall können Sie den gesamten Cache in eine Datei serialisieren. Denken Sie daran, die Dateisperrung anzugeben, wenn Ihre App oder eine andere App gleichzeitig auf den Cache zugreifen können. Ein einfaches Beispiel für das Serialisieren eines Tokencaches in eine Datei ohne Sperren finden Sie im Beispiel in der Referenzdokumentation zur Klasse [SerializableTokenCache](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache).

## <a name="token-cache-for-a-web-app-confidential-client-application"></a>Tokencache für eine Web-App (vertrauliche Clientanwendung)

Für Web-Apps oder Web-APIs können Sie den Sitzungs- bzw. einen Redis-Cache oder eine Datenbank zum Speichern des Tokencaches verwenden. Es sollte ein Tokencache pro Benutzer (d. h. pro Konto) vorhanden sein. Stellen Sie daher sicher, dass Sie den Tokencache pro Konto serialisieren.

## <a name="next-steps"></a>Nächste Schritte

Ein Beispiel für die Verwendung des Tokencaches für eine Windows- oder Linux-Web-App oder Web-API finden Sie unter [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/app.py#L64-L72). Das Beispiel bezieht sich auf eine Web-API, die die Microsoft Graph-API aufruft.
