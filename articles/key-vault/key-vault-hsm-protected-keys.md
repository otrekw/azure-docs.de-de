---
title: 'Gewusst wie: Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault – Azure Key Vault| Microsoft-Dokumentation'
description: Verwenden Sie diesen Artikel zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit dem Azure-Schlüsseltresor. Wird auch als Bring Your Own Key (BYOK) bezeichnet.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 00d2d38801929454110b41be88d133e3af232af7
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425747"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Importieren von HSM-geschützten Schlüsseln in Key Vault

Zur Steigerung der Sicherheit können Sie bei Verwendung des Azure-Schlüsseltresors Schlüssel in HSMs (Hardwaresicherheitsmodule) importieren oder darin generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Azure Key Vault verwendet die nCipher nShield-HSM-Produktfamilie (validiert für FIPS 140-2 Level 2) zum Schützen der Schlüssel.

Diese Funktion steht für Azure China 21ViaNet nicht zur Verfügung.

> [!NOTE]
> Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)  
> Ein Tutorial zu den ersten Schritten, z.B. zum Erstellen eines Schlüsseltresors für HSM-geschützte Schlüssel, finden Sie unter [Was ist Azure Key Vault?](key-vault-overview.md).

## <a name="supported-hsms"></a>Unterstützte HSMs

HSM-geschützte Schlüssel können abhängig vom verwendeten HSM auf zwei Arten an Key Vault übertragen werden. In der folgenden Tabelle erfahren Sie, welche Methode für Ihre HSMs verwendet werden muss, um Ihre eigenen HSM-geschützten Schlüssel zu generieren und anschließend für die Verwendung mit Azure Key Vault zu übertragen: 

|Name des HSM-Anbieters|Unterstützte HSM-Modelle|Unterstützte Übertragungsmethode für HSM-Schlüssel|
|---|---|---|
|Thales|<ul><li>Produktfamilie „SafeNet Luna HSM 7“ mit Firmwareversion 7.3 oder neuer</li></ul>| [Verwenden Sie die neue BYOK-Methode (Vorschau).](hsm-protected-keys-vendor-agnostic-byok.md)|
|nCipher|<ul><li>HSM-Produktfamilie „nShield“</li></ul>|[Verwenden Sie die alte BYOK-Methode.](hsm-protected-keys-legacy.md)|


## <a name="next-steps"></a>Nächste Schritte

Halten Sie sich an die [bewährten Methoden zum Verwenden von Key Vault](key-vault-best-practices.md), um die Sicherheit, Dauerhaftigkeit und Überwachung Ihrer Schlüssel zu gewährleisten.
