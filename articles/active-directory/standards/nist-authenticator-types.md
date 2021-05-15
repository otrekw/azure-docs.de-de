---
title: NIST-Authentifikatortypen und angeglichene Azure Active Directory-Methoden
description: Erläuterungen zur Angleichung von Azure Active Directory Authentifizierungsmethoden an NIST-Authentifikatortypen.
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: barbaraselden
ms.author: baselden
manager: mtillman
ms.reviewer: martinco
ms.date: 4/26/2021
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 113dddb371faab784e96acc2076180118af7cfd8
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108294044"
---
# <a name="nist-authenticator-types-and-aligned-azure-active-directory-methods"></a>NIST-Authentifikatortypen und angeglichene Azure Active Directory-Methoden

Der Authentifizierungsprozess beginnt, wenn ein Anforderer seine Kontrolle über einen von mehreren Authentifikatoren bestätigt, die einem Abonnenten zugeordnet sind. Bei dem Abonnenten kann es sich um eine Person oder eine andere Entität handeln.

| NIST-Authentifikatortyp| Azure AD-Authentifizierungsmethoden |
| - | - |
|  Gespeichertes Geheimnis <br> (etwas, das Sie wissen)|  Kennwort (Cloudkonten)  <br>Kennwort (Verbund)<br> Kennwort (Kennwort-Hashsynchronisierung)<br>Kennwort (Pass-Through-Authentifizierung) |
|Lookupgeheimnis <br> (etwas, das Sie haben)| Keine. Bei einem Lookupgeheimnis handelt es sich definitionsgemäß um Daten, die nicht in einem System gespeichert sind. |
|Out-of-band <br>(etwas, das Sie haben)| Telefon (SMS) – nicht empfohlen |
| Single-Factor-Einmalkennwort <br>‎(Etwas, das Sie haben)| Microsoft Authenticator-App (einmaliges Kennwort)  <br>Single-Factor-Einmalkennwort ‎(über OTP-Hersteller)<sup data-htmlnode="">1</sup> | 
| Multi-Factor-Einmalkennwort<br>(etwas, das Sie haben + etwas, das Sie wissen oder etwas, das Sie sind)| Multi-Factor-Einmalkennwort ‎(über OTP-Hersteller)<sup data-htmlnode="">1</sup>| 
|Single-Factor-Kryptografiesoftware<br>(etwas, das Sie haben)|Kompatibles mobiles Gerät <br> Microsoft Authenticator-App (Benachrichtigung) <br> In Azure AD Hybrid eingebundenes Gerät <sup data-htmlnode="">2</sup> *mit Software TPM*<br> In Azure AD eingebundenes Gerät <sup data-htmlnode="">2</sup> *mit Software TPM* |
| Multi-Factor-Kryptografiehardware <br>(etwas, das Sie haben) | In Azure AD eingebundenes Gerät <sup data-htmlnode="">2</sup> *mit Hardware TPM* <br> In Azure AD Hybrid eingebundenes Gerät <sup data-htmlnode="">2</sup> *mit Hardware TPM*|
|Multi-Factor-Kryptografiesoftware<br>(etwas, das Sie haben + etwas, das Sie wissen oder etwas, das Sie sind) | Microsoft Authenticator-App für iOS (kennwortlos)<br> Windows Hello for Business *mit Software-TPM* |
|Multi-Factor-Kryptografiehardware <br>(etwas, das Sie haben + etwas, das Sie wissen oder etwas, das Sie sind) |Microsoft Authenticator-App für Android (kennwortlos)<br> Windows Hello for Business *mit Hardware-TPM*<br> Smartcard (Verbundidentitätsanbieter) <br> FIDO 2 Sicherheitsschlüssel |


<sup data-htmlnode="">1</sup> OATH-TOTP SHA-1-Token der 30-Sekunden- oder 60-Sekunden-Variante.

<sup data-htmlnode="">2</sup> Weitere Informationen zu den Einbindungszuständen von Geräten finden Sie in der [Dokumentation zur Azure AD Geräteidentität](https://docs.microsoft.com/azure/active-directory/devices/). 

## <a name="why-sms-isnt-recommended"></a>Warum SMS nicht empfohlen wird 

SMS-Textnachrichten erfüllen zwar den NIST-Standard, werden von NIST aber nicht empfohlen. Die Risiken von Gerätetauschen, SIM-Wechseln, Nummernportierung und anderem Verhalten können Probleme verursachen. Wenn diese Aktionen böswillig ausgeführt werden, können sie zu unliebsamen Erfahrungen führen. Sie werden zwar nicht empfohlen, sind aber besser als die alleinige Verwendung eines Kennworts, da sie mehr Aufwand für Hacker erfordern. 

## <a name="next-steps"></a>Nächste Schritte 

[NIST-Übersicht](nist-overview.md)

[Weitere Informationen zu AALs](nist-about-authenticator-assurance-levels.md)

[Authentifizierungsszenarien für Azure AD](nist-authentication-basics.md)

[NIST-Authentifikatortypen](nist-authenticator-types.md)

[Erzielen von NIST AAL1 mit Azure AD](nist-authenticator-assurance-level-1.md)

[Erzielen von NIST AAL2 mit Azure AD](nist-authenticator-assurance-level-2.md)

[Erzielen von NIST AAL3 mit Azure AD](nist-authenticator-assurance-level-3.md) 
