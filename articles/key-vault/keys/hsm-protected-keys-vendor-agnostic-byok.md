---
title: 'Gewusst wie: Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault – Azure Key Vault| Microsoft-Dokumentation'
description: Verwenden Sie diesen Artikel zum Planen, Generieren und Übertragen Ihrer eigenen durch HSM geschützten Schlüssel für die Nutzung mit Azure Key Vault. Dieser Ansatz wird auch als Bring Your Own Key (BYOK) bezeichnet.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat
ms.openlocfilehash: 77568928e50fb4398aa786dbbd4a44b9277a8d5b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426978"
---
# <a name="import-hsm-protected-keys-to-key-vault-preview"></a>Importieren von HSM-geschützten Schlüsseln in Key Vault (Vorschauversion)

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase und ist nur in den Azure-Regionen *USA, Osten 2 (EUAP)* und *USA, Mitte (EUAP)* verfügbar. 

Zur Steigerung der Sicherheit können Sie bei Verwendung von Azure Key Vault Schlüssel in ein HSM (Hardwaresicherheitsmodul) importieren oder darin generieren. Dieser Schlüssel verbleibt immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Key Vault verwendet die nCipher nShield-HSM-Produktfamilie (validiert für FIPS 140-2 Level 2) zum Schützen der Schlüssel.

Verwenden Sie die Informationen in diesem Artikel zum Planen, Generieren und Übertragen Ihrer eigenen durch HSM geschützten Schlüssel für die Nutzung mit Azure Key Vault.

> [!NOTE]
> Diese Funktion steht für Azure China 21ViaNet nicht zur Verfügung. 
> 
> Diese Importmethode ist nur für [unterstützte HSMs](#supported-hsms) verfügbar. 

Weitere Informationen und ein Tutorial mit den ersten Schritten mit Key Vault (einschließlich der Erstellung eines Schlüsseltresors für durch HSM geschützte Schlüssel) finden Sie unter [Was ist Azure Key Vault?](../general/overview.md).

## <a name="overview"></a>Übersicht

Im Folgenden finden Sie eine Übersicht über den Prozess. Einige der Schritte, die Sie ausführen müssen, werden weiter unten in diesem Artikel beschrieben.

* Generieren Sie in Key Vault einen Schlüssel (als Schlüsselaustauschschlüssel (*Key Exchange Key*, KEK) bezeichnet). Der KEK muss ein RSA-HSM-Schlüssel sein, der nur den Schlüsselvorgang `import` umfasst. Nur die Premium-SKU von Key Vault unterstützt RSA-HSM-Schlüssel.
* Laden Sie den öffentlichen Schlüssel des KEK als PEM-Datei herunter.
* Übertragen Sie den öffentlichen Schlüssel des KEK auf einen Offlinecomputer, der mit einem lokalen HSM verbunden ist.
* Verwenden Sie auf dem Offlinecomputer das von Ihrem HSM-Anbieter bereitgestellte BYOK-Tool, um eine BYOK-Datei zu erstellen. 
* Der Zielschlüssel wird mit einem KEK verschlüsselt. Die Verschlüsselung bleibt bis zur Übertragung an die HSMs in Key Vault bestehen. Nur die verschlüsselte Version Ihres Schlüssels verlässt das lokale HSM.
* Ein KEK, der innerhalb eines Key Vault-HSM generiert wurde, kann nicht exportiert werden. HSMs erzwingen die Regel, dass keine unverschlüsselte Version eines KEK außerhalb eines Key Vault-HSM vorhanden sein darf.
* Der KEK muss sich in derselben Key Vault-Instanz befinden, in die der Zielschlüssel importiert werden soll.
* Wenn die BYOK-Datei in Key Vault hochgeladen wird, verwendet das Key Vault-HSM den privaten Schlüssel des KEK, um das Zielschlüsselmaterial zu entschlüsseln und als HSM-Schlüssel zu importieren. Dieser Vorgang erfolgt vollständig innerhalb eines Key Vault-HSM. Der Zielschlüssel verbleibt stets in den Schutzgrenzen des HSM.

## <a name="prerequisites"></a>Voraussetzungen

In der folgenden Tabelle sind die Voraussetzungen für die Verwendung von BYOK in Azure Key Vault aufgeführt:

| Anforderung | Weitere Informationen |
| --- | --- |
| Ein Azure-Abonnement |Um in Azure Key Vault einen Schlüsseltresor erstellen zu können, benötigen Sie ein Azure-Abonnement. [Registrieren Sie sich für eine kostenlose Testversion.](https://azure.microsoft.com/pricing/free-trial/) |
| Eine Key Vault-Instanz mit Premium-SKU zum Importieren von durch HSM geschützten Schlüsseln |Weitere Informationen zu den Dienstebenen und Funktionen in Azure Key Vault finden Sie unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/). |
| Ein HSM aus der Liste der unterstützten HSMs sowie ein BYOK-Tool mit den zugehörigen Anweisungen Ihres HSM-Anbieters | Sie müssen über die Berechtigungen für ein HSM und grundlegende Kenntnisse in der Verwendung Ihres HSM verfügen. Weitere Informationen finden Sie unter [Unterstützte HSMs](#supported-hsms). |
| Azure-Befehlszeilenschnittstelle Version 2.1.0 oder höher | Weitere Informationen finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).|

## <a name="supported-hsms"></a>Unterstützte HSMs

|Herstellername|Herstellertyp|Unterstützte HSM-Modelle|Weitere Informationen|
|---|---|---|---|
|Thales|Hersteller|Produktfamilie SafeNet Luna HSM 7 mit Firmwareversion 7.3 oder höher| [SafeNet Luna – BYOK-Tool und -Dokumentation](https://supportportal.thalesgroup.com/csm?id=kb_article_view&sys_kb_id=3892db6ddb8fc45005c9143b0b961987&sysparm_article=KB0021016)|
|Fortanix|HSM als Dienst (aaS)|Self-Defending Key Management Service (SDKMS, selbstverteidigender Schlüsselverwaltungsdienst)|[Exportieren von SDKMS-Schlüsseln nach Cloudanbietern für BYOK – Azure Key Vault](https://support.fortanix.com/hc/en-us/articles/360040071192-Exporting-SDKMS-keys-to-Cloud-Providers-for-BYOK-Azure-Key-Vault)|


> [!NOTE]
> Verwenden Sie zum Importieren von durch HSM geschützten Schlüsseln aus der HSM-Produktfamilie nCipher nShield das [BYOK-Legacyverfahren](hsm-protected-keys-legacy.md).

## <a name="supported-key-types"></a>Unterstützte Schlüsseltypen

|Schlüsselname|Schlüsseltyp|Schlüsselgröße|Origin|BESCHREIBUNG|
|---|---|---|---|---|
|Schlüsselaustauschschlüssel (Key Exchange Key, KEK)|RSA| 2\.048 Bit<br />3\.072 Bit<br />4\.096 Bit|Azure Key Vault-HSM|Ein durch HSM gestütztes RSA-Schlüsselpaar, das in Azure Key Vault generiert wurde|
|Zielschlüssel|RSA|2\.048 Bit<br />3\.072 Bit<br />4\.096 Bit|Anbieter-HSM|Der Schlüssel, der an das Azure Key Vault-HSM übertragen werden soll|

## <a name="generate-and-transfer-your-key-to-the-key-vault-hsm"></a>Generieren und Übertragen des Schlüssels an das Key Vault-HSM

So generieren Sie Ihren Schlüssel und übertragen ihn an das Key Vault-HSM

* [Schritt 1: Generieren eines KEK](#step-1-generate-a-kek)
* [Schritt 2: Herunterladen des öffentlichen Schlüssels des KEK](#step-2-download-the-kek-public-key)
* [Schritt 3: Generieren des Schlüssels und Vorbereiten für die Übertragung](#step-3-generate-and-prepare-your-key-for-transfer)
* [Schritt 4: Übertragen des Schlüssels an Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

### <a name="step-1-generate-a-kek"></a>Schritt 1: Generieren eines KEK

Ein KEK ist ein RSA-Schlüssel, der in einem Key Vault-HSM generiert wird. Der KEK wird zum Verschlüsseln des Schlüssels verwendet, den Sie importieren möchten (*Zielschlüssel*).

Für den KEK gilt Folgendes:
- Es muss sich um einen RSA-HSM-Schlüssel handeln (2.048 Bit, 3.072 Bit oder 4.096 Bit).
- Er muss im selben Schlüsseltresor generiert werden, in den Sie den Zielschlüssel importieren möchten.
- Die zulässigen Schlüsselvorgänge müssen auf `import` festgelegt sein.

> [!NOTE]
> Der KEK muss als einzigen zulässigen Schlüsselvorgang „import“ aufweisen. „import“ schließt sich mit allen anderen wichtigen Vorgängen gegenseitig aus.

Verwenden Sie den Befehl [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create), um den KEK mit auf `import` festgelegten Schlüsselvorgängen zu erstellen. Notieren Sie die Schlüssel-ID (`kid`), die vom folgenden Befehl zurückgegeben wird. (Sie verwenden den `kid`-Wert in [Schritt 3](#step-3-generate-and-prepare-your-key-for-transfer).)

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

### <a name="step-2-download-the-kek-public-key"></a>Schritt 2: Herunterladen des öffentlichen Schlüssels des KEK

Verwenden Sie den Befehl [az keyvault key download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download), um den öffentlichen Schlüssel des KEK in eine PEM-Datei herunterzuladen. Der importierte Zielschlüssel ist mit dem öffentlichen Schlüssel des KEK verschlüsselt.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

Übertragen Sie die Datei „KEKforBYOK.publickey.pem“ an Ihren Offlinecomputer. Sie benötigen diese Datei im nächsten Schritt.

### <a name="step-3-generate-and-prepare-your-key-for-transfer"></a>Schritt 3: Generieren des Schlüssels und Vorbereiten für die Übertragung

Informieren Sie sich in der Dokumentation Ihres HSM-Anbieters darüber, wie Sie das BYOK-Tool herunterladen und installieren. Befolgen Sie die Anweisungen des HSM-Anbieters, um einen Zielschlüssel zu generieren und dann eine Schlüsselübertragungsdatei (BYOK-Datei) zu erstellen. Das BYOK-Tool verwendet den `kid` aus [Schritt 1](#step-1-generate-a-kek) und die Datei „KEKforBYOK.publickey.pem“, die Sie in [Schritt 2](#step-2-download-the-kek-public-key) heruntergeladen haben, um einen verschlüsselten Zielschlüssel in einer BYOK-Datei zu generieren.

Übertragen Sie die BYOK-Datei an Ihren verbundenen Computer.

> [!NOTE] 
> Das Importieren von RSA-Schlüsseln mit 1.024 Bit wird nicht unterstützt. Derzeit wird das Importieren eines EC-Schlüssels (Elliptic Curve, elliptische Kurve) nicht unterstützt.
> 
> **Bekanntes Problem:** Das Importieren eines RSA 4K-Zielschlüssels von SafeNet Luna HSMs wird nur mit Firmware 7.4.0 oder höher unterstützt.

### <a name="step-4-transfer-your-key-to-azure-key-vault"></a>Schritt 4: Übertragen des Schlüssels an Azure Key Vault

Um den Schlüsselimport abzuschließen, übertragen Sie das Schlüsselübertragungspaket (BYOK-Datei) von Ihrem nicht verbundenen Computer an den mit dem Internet verbundenen Computer. Verwenden Sie den Befehl [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import), um die BYOK-Datei in das Key Vault-HSM hochzuladen.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

Wenn der Upload erfolgreich ist, werden die Eigenschaften des importierten Schlüssels in der Azure-Befehlszeilenschnittstelle angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Sie können diesen HSM-geschützten Schlüssel jetzt in Ihrem Schlüsseltresor verwenden. Weitere Informationen finden Sie in dieser [Gegenüberstellung von Preisen und Funktionen](https://azure.microsoft.com/pricing/details/key-vault/).



