---
title: Behandeln von Fehlern und Ausnahmen in MSAL für Python
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen, Anspruchsaufforderungen für den bedingten Zugriff und Wiederholversuche in MSAL für Python-Anwendungen verarbeiten.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2020
ms.author: marsma
ms.reviewer: saeeda, rayluo
ms.custom: aaddev
ms.openlocfilehash: 3046787393d38ed60b54236f33acc065db90321d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98761085"
---
# <a name="handle-errors-and-exceptions-in-msal-for-python"></a>Behandeln von Fehlern und Ausnahmen in MSAL für Python

[!INCLUDE [Active directory error handling introduction](../../../includes/active-directory-develop-error-handling-introduction.md)]

## <a name="error-handling-in-msal-for-python"></a>Fehlerbehandlung in MSAL für Python

In MSAL für Python werden die meisten Fehler als Rückgabewert des API-Aufrufs ausgegeben. Der Fehler wird als Wörterbuch dargestellt, das die JSON-Antwort von der Microsoft Identity Platform enthält.

* Eine erfolgreiche Antwort enthält den Schlüssel `"access_token"`. Das Format der Antwort wird durch das OAuth2-Protokoll definiert. Weitere Informationen finden Sie unter [5.1 Erfolgreiche Antwort](https://tools.ietf.org/html/rfc6749#section-5.1).
* Eine Fehlerantwort enthält `"error"` und in der Regel `"error_description"`. Das Format der Antwort wird durch das OAuth2-Protokoll definiert. Weitere Informationen finden Sie unter [5.2 Fehlerantwort](https://tools.ietf.org/html/rfc6749#section-5.2).

Wenn ein Fehler zurückgegeben wird, enthält der Schlüssel `"error_description"` eine lesbare Nachricht. Diese wiederum enthält in der Regel einen Fehlercode der Microsoft Identity Platform. Einzelheiten zu den verschiedenen Fehlercodes finden Sie unter [Authentifizierungs- und Autorisierungsfehlercodes](./reference-aadsts-error-codes.md).

In MSAL für Python sind Ausnahmen selten, da die meisten Fehler durch die Rückgabe eines Fehlerwerts behandelt werden. Die `ValueError`-Ausnahme wird nur ausgelöst, wenn ein Problem mit Ihrer Verwendung der Bibliothek vorliegt, z. B. wenn API-Parameter falsch formatiert sind.

[!INCLUDE [Active directory error handling claims challenges](../../../includes/active-directory-develop-error-handling-claims-challenges.md)]

[!INCLUDE [Active directory error handling retries](../../../includes/active-directory-develop-error-handling-retries.md)]

## <a name="next-steps"></a>Nächste Schritte

Erwägen Sie die Aktivierung der [Protokollierung in MSAL für Python](msal-logging-python.md), um Probleme besser diagnostizieren und debuggen zu können.
