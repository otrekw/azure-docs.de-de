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
ms.openlocfilehash: a0a8f96fabdff3543222077f5113b0bd602997b7
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416547"
---
# <a name="browser-support-of-fido2-passwordless-authentication"></a>Browserunterstützung der kennwortlosen FIDO2-Authentifizierung

Azure Active Directory ermöglicht die Verwendung von [FIDO2-Sicherheitsschlüsseln](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless#fido2-security-keys) als kennwortloses Gerät. Die Verfügbarkeit der FIDO2-Authentifizierung für Microsoft-Konten wurde [2018 angekündigt](https://techcommunity.microsoft.com/t5/identity-standards-blog/all-about-fido2-ctap2-and-webauthn/ba-p/288910). Wie in der Ankündigung erläutert, müssen bestimmte optionale Features und Erweiterungen der FIDO2 CTAP-Spezifikation implementiert werden, um die sichere Authentifizierung mit Microsoft- und Azure Active Directory-Konten zu unterstützen. Im folgenden Diagramm wird gezeigt, welche Kombinationen aus Browsern und Betriebssystemen die kennwortlose Authentifizierung mithilfe von FIDO2-Authentifizierungsschlüsseln mit Azure Active Directory unterstützen.

## <a name="supported-browsers"></a>Unterstützte Browser

In der folgenden Tabelle wird die Unterstützung für das Authentifizieren von Azure Active Directory-Konten (Azure AD-Konten) und Microsoft-Konten (MSA) aufgeführt. Microsoft-Konten werden von Kunden für Dienste wie Xbox, Skype oder Outlook.com erstellt. Zu den unterstützten Gerätetypen gehören **USB**, Near-Field Communication (**NFC**) und Bluetooth Low Energy (**BLE**).

|  | Chrome |  |  | Edge |  |  | Firefox |  |  |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| | USB | NFC | BLE | USB | NFC | BLE | USB | NFC | BLE |
| **Windows**  | ![Chrome unterstützt USB unter Windows für AAD-Konten.][y] | ![Chrome unterstützt NFC unter Windows für AAD-Konten.][y] | ![Chrome unterstützt BLE unter Windows für AAD-Konten.][y] | ![Edge unterstützt USB unter Windows für AAD-Konten.][y] | ![Edge unterstützt NFC unter Windows für AAD-Konten.][y] | ![Edge unterstützt BLE unter Windows für AAD-Konten.][y] | ![Firefox unterstützt USB unter Windows für AAD-Konten.][y] | ![Firefox unterstützt NFC unter Windows für AAD-Konten.][y] | ![Firefox unterstützt BLE unter Windows für AAD-Konten.][y] |
| **macOS**  | ![Chrome unterstützt USB unter macOS für AAD-Konten.][y] | ![Chrome bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Chrome bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] | ![Edge unterstützt USB unter macOS für AAD-Konten.][y] | ![Edge bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Edge bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für USB für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für NFC für AAD-Konten.][n] | ![Firefox bietet unter macOS keine Unterstützung für BLE für AAD-Konten.][n] |
| **Linux**  | ![Chrome unterstützt USB unter Linux für AAD-Konten.][y] | ![Chrome bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Chrome bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] | ![Edge bietet unter Linux keine Unterstützung für USB für AAD-Konten.][n] | ![Edge bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Edge bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für USB für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für NFC für AAD-Konten.][n] | ![Firefox bietet unter Linux keine Unterstützung für BLE für AAD-Konten.][n] |

## <a name="operating-system-versions-tested"></a>Getestete Betriebssystemversionen

Die Angaben in der obigen Tabelle wurden für die folgenden Betriebssystemversionen getestet.

| Betriebssystem | Neueste getestete Version |
| --- | --- |
| Windows | Windows 10 20H2 1904 |
| macOS | OS X 11 Big Sur |
| Linux | Fedora 32 Workstation |

## <a name="next-steps"></a>Nächste Schritte
[Aktivieren der kennwortlosen Anmeldung mit Sicherheitsschlüsseln (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-passwordless-security-key)

<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png
