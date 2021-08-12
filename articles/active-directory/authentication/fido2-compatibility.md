---
title: Browserunterstützung der kennwortlosen FIDO2-Authentifizierung | Azure Active Directory
description: Kombinationen aus Browsern und Betriebssystemen unterstützen die kennwortlose FIDO2-Authentifizierung für Apps, die Azure Active Directory verwenden.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/02/2021
ms.author: nichola
author: knicholasa
manager: martinco
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2595acf25b63f89f6e0e29e996548b58767e9fde
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110786151"
---
# <a name="browser-support-of-fido2-passwordless-authentication"></a>Browserunterstützung der kennwortlosen FIDO2-Authentifizierung

Azure Active Directory ermöglicht die Verwendung von [FIDO2-Sicherheitsschlüsseln](./concept-authentication-passwordless.md#fido2-security-keys) als kennwortloses Gerät. Die Verfügbarkeit der FIDO2-Authentifizierung für Microsoft-Konten wurde [2018 bekannt gegeben](https://techcommunity.microsoft.com/t5/identity-standards-blog/all-about-fido2-ctap2-and-webauthn/ba-p/288910) und im März 2021 [allgemein verfügbar](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/passwordless-authentication-is-now-generally-available/ba-p/1994700) gemacht. Im folgenden Diagramm wird gezeigt, welche Kombinationen aus Browsern und Betriebssystemen die kennwortlose Authentifizierung mithilfe von FIDO2-Authentifizierungsschlüsseln mit Azure Active Directory unterstützen.

## <a name="supported-browsers"></a>Unterstützte Browser

In der folgenden Tabelle wird die Unterstützung für das Authentifizieren von Azure Active Directory-Konten (Azure AD-Konten) und Microsoft-Konten (MSA) aufgeführt. Microsoft-Konten werden von Kunden für Dienste wie Xbox, Skype oder Outlook.com erstellt. Zu den unterstützten Gerätetypen gehören **USB**, Near-Field Communication (**NFC**) und Bluetooth Low Energy (**BLE**).

| OS | Chrome | Chrome  | Chrome | Edge | Edge | Edge | Firefox | Firefox | Firefox |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| | USB | NFC | BLE | USB | NFC | BLE | USB | NFC | BLE |
| **Windows**  | ![Chrome unterstützt USB unter Windows für AAD-Konten.][y] | ![Chrome unterstützt NFC unter Windows für AAD-Konten.][y] | ![Chrome unterstützt BLE unter Windows für AAD-Konten.][y] | ![Microsoft Edge unterstützt USB unter Windows für AAD-Konten.][y] | ![Microsoft Edge unterstützt NFC unter Windows für AAD-Konten.][y] | ![Microsoft Edge unterstützt BLE unter Windows für AAD-Konten.][y] | ![Firefox unterstützt USB unter Windows für AAD-Konten.][y] | ![Firefox unterstützt NFC unter Windows für AAD-Konten.][y] | ![Firefox unterstützt BLE unter Windows für AAD-Konten.][y] |
| **macOS**  | ![Chrome unterstützt USB unter macOS für AAD-Konten.][y] | ![Chrome bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Chrome bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] | ![Microsoft Edge unterstützt USB unter macOS für AAD-Konten.][y] | ![Microsoft Edge bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Microsoft Edge bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für USB für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] |
| **Linux**  | ![Chrome unterstützt USB unter Linux für AAD-Konten.][y] | ![Chrome bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Chrome bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] | ![Microsoft Edge bietet unter Linux keine Unterstützung für USB für AAD-Konten.][n] | ![Microsoft Edge bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Microsoft Edge bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für USB für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] |



## <a name="unsupported-browsers"></a>Nicht unterstützte Browser

Die folgenden Kombinationen aus Betriebssystem und Browser werden nicht unterstützt, aber zukünftige Unterstützung und Tests werden untersucht. Wenn Sie weitere Unterstützung für Betriebssystem und Browser wünschen, senden Sie uns bitte Ihr Feedback über das Produktfeedback-Tool unten auf der Seite.

| Betriebssystem | Browser |
| ---- | ---- |
| iOS | Safari, Brave |
| macOS | Safari |
| Android | Chrome |
| ChromeOS | Chrome |

## <a name="minimum-browser-version"></a>Mindestversion des Browsers

Nachfolgend sind die Mindestanforderungen für die Browserversion aufgeführt. 

| Browser | Mindestversion |
| ---- | ---- |
| Chrome | 76 |
| Edge | Windows 10 Version 1903<sup>1</sup> |
| Firefox | 66 |

<sup>1</sup>Alle Versionen der neuen Microsoft Edge-Unterstützung Fido2 auf Chromium-Basis. Unterstützung für Microsoft Edge-Legacy wurde in 1903 hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln](./howto-authentication-passwordless-security-key.md)

<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png
