---
title: Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault
description: Hier erfahren Sie mehr über das Planen, Generieren und anschließende Übertragen Ihrer eigenen durch HSM geschützten Schlüssel für die Nutzung mit Azure Key Vault. Wird auch als Bring Your Own Key (BYOK) bezeichnet.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: tutorial
ms.date: 02/24/2021
ms.author: ambapat
ms.openlocfilehash: 171e0c03dc6f246d0f56d11f793ca711b0082f49
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588291"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Importieren von HSM-geschützten Schlüsseln in Key Vault

Zur Steigerung der Sicherheit können Sie bei Verwendung des Azure-Schlüsseltresors Schlüssel in HSMs (Hardwaresicherheitsmodule) importieren oder darin generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Azure Key Vault verwendet die nCipher nShield-HSM-Produktfamilie (validiert für FIPS 140-2 Level 2) zum Schützen der Schlüssel.

Diese Funktion steht für Azure China 21ViaNet nicht zur Verfügung.

> [!NOTE]
> Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../general/overview.md)  
> Ein Tutorial zu den ersten Schritten, z.B. zum Erstellen eines Schlüsseltresors für HSM-geschützte Schlüssel, finden Sie unter [Was ist Azure Key Vault?](../general/overview.md).

## <a name="supported-hsms"></a>Unterstützte HSMs

HSM-geschützte Schlüssel können abhängig vom verwendeten HSM auf zwei Arten an Key Vault übertragen werden. In der folgenden Tabelle erfahren Sie, welche Methode für Ihre HSMs verwendet werden muss, um Ihre eigenen HSM-geschützten Schlüssel zu generieren und anschließend für die Verwendung mit Azure Key Vault zu übertragen: 

|Herstellername|Herstellertyp|Unterstützte HSM-Modelle|Unterstützte Übertragungsmethode für HSM-Schlüssel|
|---|---|---|---|
|[nCipher](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|Hersteller,<br/>HSM als Dienst (aaS)|<ul><li>HSM-Produktfamilie „nShield“</li><li>nShield als Dienst</ul>|**Methode 1:** [nCipher BYOK](hsm-protected-keys-ncipher.md) (veraltet). Diese Methode wird nach dem <strong>30. Juni 2021</strong> nicht mehr unterstützt.<br/>**Methode 2:** [Verwenden der neuen BYOK-Methode](hsm-protected-keys-byok.md) (empfohlen)|
|Thales|Hersteller|<ul><li>Produktfamilie „Luna HSM 7“ mit Firmwareversion 7.3 oder neuer</li></ul>| [Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Fortanix|Hersteller,<br/>HSM als Dienst (aaS)|<ul><li>Self-Defending Key Management Service (SDKMS, selbstverteidigender Schlüsselverwaltungsdienst)</li><li>Equinix SmartKey</li></ul>|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Marvell|Hersteller|Alle Liquid Security-HSMs mit<ul><li>Firmwareversion 2.0.4 oder höher</li><li>Firmwareversion 3.2 oder höher</li></ul>|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Cryptomathic|ISV (Enterprise Key Management System)|Mehrere HSM-Marken und -Modelle, einschließlich<ul><li>nCipher</li><li>Thales</li><li>Utimaco</li></ul>Weitere Informationen finden Sie auf der [Cryptomathic-Website](https://www.cryptomathic.com/azurebyok).|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Securosys SA|Hersteller,<br/>HSM als Dienst|Primus-HSM-Familie, Securosys Clouds HSM|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|StorMagic|ISV (Enterprise Key Management System)|Mehrere HSM-Marken und -Modelle, einschließlich<ul><li>Utimaco</li><li>Thales</li><li>nCipher</li></ul>Weitere Informationen finden Sie auf der [StorMagic-Website](https://stormagic.com/doc/svkms/Content/Integrations/Azure_KeyVault_BYOK.htm).|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|IBM|Hersteller|IBM 476x, CryptoExpress|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|Utimaco|Hersteller,<br/>HSM als Dienst|u.trust Anchor, CryptoServer|[Verwenden einer neuen BYOK-Methode](hsm-protected-keys-byok.md)|
|||||

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Key Vault-Sicherheitsübersicht](../general/security-overview.md) an, um die Sicherheit, Dauerhaftigkeit und Überwachung Ihrer Schlüssel zu gewährleisten.
* Eine vollständige Beschreibung der neuen BYOK-Methode finden Sie unter [BYOK-Spezifikation](./byok-specification.md).