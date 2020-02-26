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
ms.openlocfilehash: 2f5269587d222be9a1628b72c1f3f0dc1b105f3c
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461742"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>Importieren von HSM-geschützten Schlüsseln in Key Vault (Vorschauversion)

> [!NOTE]
> Dieses Feature befindet sich in der Vorschau und ist nur in den Regionen **USA, Osten 2 (EUAP)** und **USA, Mitte (EUAP)** verfügbar. 

Um bei Verwendung von Azure Key Vault die Sicherheit zu erhöhen, können Sie Schlüssel in HSMs (Hardwaresicherheitsmodule) importieren oder darin generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Azure Key Vault verwendet die nCipher nShield-HSM-Produktfamilie (validiert für FIPS 140-2 Level 2) zum Schützen der Schlüssel.

Verwenden Sie die Informationen in diesem Thema zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit Azure Key Vault.

> [!NOTE]
> Diese Funktion steht für Azure China 21ViaNet nicht zur Verfügung. 
> 
> Diese Importmethode ist nur für [unterstützte HSMs](#supported-hsms) verfügbar. 

Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)  Ein Tutorial zu den ersten Schritten, z.B. zum Erstellen eines Schlüsseltresors für HSM-geschützte Schlüssel, finden Sie unter [Was ist Azure Key Vault?](key-vault-overview.md).

## <a name="overview"></a>Übersicht

* Generieren Sie in Key Vault einen Schlüsselaustauschschlüssel (Key Exchange Key, KEK). Hierbei muss es sich um einen RSA-HSM-Schlüssel handeln, für den „import“ als einziger Schlüsselvorgang festgelegt ist. Nur die Premium-SKU von Key Vault unterstützt RSA-HSM-Schlüssel.
* Laden Sie den öffentlichen Schlüssel des KEK als PEM-Datei herunter.
* Übertragen Sie den öffentlichen Schlüssel des KEK an Ihre Offline-Arbeitsstation, die mit dem lokalen HSM verbunden ist.
* Verwenden Sie auf Ihrer Offline-Arbeitsstation das von Ihrem HSM-Anbieter bereitgestellte BYOK-Tool, um eine BYOK-Datei zu erstellen. 
* Der Zielschlüssel wird mit einem KEK verschlüsselt. Die Verschlüsselung bleibt bis zur Übertragung an die HSMs in Azure Key Vault bestehen. Nur die verschlüsselte Version Ihres Schlüssels verlässt das lokale HSM.
* Der KEK, der innerhalb der HSMs in Azure Key Vault generiert wird, kann nicht exportiert werden. Die HSMs erzwingen, dass außerhalb der Key Vault-HSMs keine unverschlüsselte Version des KEK vorhanden sein darf.
* Der KEK muss sich in derselben Key Vault-Instanz befinden, in die der Zielschlüssel importiert werden soll.
* Wenn die BYOK-Datei in Key Vault hochgeladen wird, verwenden die Key Vault-HSMs den privaten Schlüssel des KEK, um das Zielschlüsselmaterial zu entschlüsseln und als HSM-Schlüssel zu importieren. Dieser Vorgang erfolgt vollständig innerhalb der Key Vault-HSMs, und der Zielschlüssel verbleibt immer innerhalb der HSM-Schutzgrenzen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgende Tabelle enthält eine Liste mit Voraussetzungen, die beim Azure-Schlüsseltresor für Bring Your Own Key (BYOK) erfüllt sein müssen.

| Anforderung | Weitere Informationen |
| --- | --- |
| Azure-Abonnement |Um eine Azure Key Vault-Instanz erstellen zu können, benötigen Sie ein Azure-Abonnement: [Für kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/) |
| Eine Key Vault-Instanz (Premium-SKU) zum Importieren von HSM-geschützten Schlüsseln |Weitere Informationen zu den Dienstebenen und Funktionen für Azure Key Vault finden Sie auf der Website [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/). |
| Ein HSM aus der Liste der unterstützten HSMs sowie ein BYOK-Tool und zugehörige Anweisungen von Ihrem HSM-Anbieter | Sie benötigen Zugriff auf ein Hardwaresicherheitsmodul sowie grundlegende Kenntnisse zum Betrieb Ihrer HSMs. Weitere Informationen finden Sie unter [Unterstützte HSMs](#supported-hsms). |
| Azure CLI, Version 2.1.0 oder höher | Weitere Informationen finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).|

## <a name="supported-hsms"></a>Unterstützte HSMs

|Name des HSM-Anbieters|Unterstützte HSM-Modelle|Zusätzliche Details|
|---|---|---|
|Thales|Produktfamilie SafeNet Luna HSM 7 mit Firmwareversion 7.3 oder neuer| [SafeNet Luna – BYOK-Tool und -Dokumentation](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|


> [!NOTE]
> [Verwenden von BYOK-Legacyverfahren](hsm-protected-keys-legacy.md) zum Importieren von HSM-geschützten Schlüsseln aus der nCipher nShield-HSM-Produktfamilie


## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generieren und Übertragen des Schlüssels an das HSM des Azure-Schlüsseltresors

Führen Sie die folgenden fünf Schritte aus, um Ihren Schlüssel zu generieren und an das Azure Key Vault-HSM zu übertragen:

* [Schritt 1: Generieren eines KEK](#step-1-generate-a-kek)
* [Schritt 2: Herunterladen des öffentlichen KEK-Schlüssels](#step-2-download-kek-public-key)
* [Schritt 3: Generieren des Schlüssels und Vorbereiten für die Übertragung](#step-3-generate-and-prepare-your-key-for-transfer)
* [Schritt 4: Übertragen des Schlüssels an Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>Schritt 1: Generieren eines KEK

Der KEK (Key Exchange Key, Schlüsselaustauschschlüssel) ist ein RSA-Schlüssel, der im HSM von Key Vault generiert wird. Mit diesem Schlüssel wird der zu importierende Schlüssel (Zielschlüssel) verschlüsselt.

Für den KEK gilt Folgendes:
1. Es muss sich um einen **RSA-HSM**-Schlüssel handeln (2048 Bit, 3072 Bit oder 4096 Bit).
2. Er muss in derselben Key Vault-Instanz generiert werden, in die Sie den Zielschlüssel importieren möchten.
3. Die zulässigen Schlüsselvorgängen müssen auf **import** festgelegt sein.

Verwenden Sie den Befehl [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create), um den KEK mit auf „import“ festgelegten Schlüsselvorgängen zu erstellen. Notieren Sie sich den kid-Wert des Schlüsselbezeichners, der vom folgenden Befehl zurückgegeben wird. Sie benötigen ihn in [Schritt 3](#step-3-generate-and-prepare-your-key-for-transfer).


```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-kek-public-key"></a>Schritt 2: Herunterladen des öffentlichen KEK-Schlüssels

Verwenden Sie den Befehl [az keyvault key download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download), um den öffentlichen KEK-Schlüssel in eine PEM-Datei herunterzuladen. Der importierte Zielschlüssel ist mit dem öffentlichen KEK-Schlüssel verschlüsselt.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

Übertragen Sie die Datei „KEKforBYOK.publickey.pem“ an Ihre Offline-Arbeitsstation. Sie benötigen diese Datei im nächsten Schritt.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Schritt 3: Generieren des Schlüssels und Vorbereiten für die Übertragung

Informieren Sie sich in der Dokumentation Ihres HSM-Anbieters darüber, wie Sie das BYOK-Tool herunterladen und installieren. Befolgen Sie die Anweisungen des HSM-Anbieters, um einen Zielschlüssel zu generieren und dann eine Schlüsselübertragungsdatei (BYOK-Datei) zu erstellen. Das BYOK-Tool verwendet den Schlüsselbezeichner aus [Schritt 1](#step-1-generate-a-kek) und die Datei „KEKforBYOK.publickey.pem“, die Sie in [Schritt 2](#step-2-download-kek-public-key) heruntergeladen haben, um einen verschlüsselten Zielschlüssel in einer BYOK-Datei zu generieren.

Übertragen Sie die BYOK-Datei an Ihre verbundene Arbeitsstation.

> [!NOTE] 
> Der Zielschlüssel muss ein RSA-Schlüssel mit 2048 Bit, 3072 Bit oder 4096 Bit sein. Das Importieren von auf elliptischen Kurven basierenden Schlüsseln wird zurzeit nicht unterstützt.
> <br/><strong>Bekanntes Problem:</strong> Beim Importieren eines RSA-Zielschlüssel mit 4K aus SafeNet Luna-HSMs tritt ein Fehler auf. Wenn dieses Problem gelöst ist, wird das vorliegende Dokument aktualisiert.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>Schritt 4: Übertragen des Schlüssels an Azure Key Vault

Übertragen Sie in diesem letzten Schritt das Schlüsselübertragungspaket (eine BYOK-Datei) von Ihrer nicht verbundenen Arbeitsstation auf die mit dem Internet verbundene Arbeitsstation, und verwenden Sie dann den Befehl [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import), um die BYOK-Datei in das Azure Key Vault-HSM hochzuladen und den Schlüsselimport damit abzuschließen.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Wenn der Upload erfolgreich ist, werden die Eigenschaften des gerade importierten Schlüssels angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Sie können diesen HSM-geschützten Schlüssel jetzt in Ihrem Schlüsseltresor verwenden. Weitere Informationen finden Sie in dieser [Gegenüberstellung](https://azure.microsoft.com/pricing/details/key-vault/) von Preisen und Funktionen.
