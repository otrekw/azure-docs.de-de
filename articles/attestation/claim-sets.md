---
title: Azure Attestation-Anspruchsätze
description: Die Anspruchsätze von Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: afe2cf288cd4a15091e8278309b3ecf74a2d35a4
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98572747"
---
# <a name="claim-sets"></a>Anspruchsätze

Ansprüche, die während der Nachweisgenerierung für Enclaves mit Microsoft Azure Attestation generiert werden, können in die folgenden Kategorien unterteilt werden:

- **Eingehende Ansprüche**: Dies sind Ansprüche, die von Microsoft Azure Attestation nach dem Analysieren der Nachweisdaten generiert werden. Sie können von Richtlinienerstellern genutzt werden, um in einer benutzerdefinierten Richtlinie Autorisierungsregeln zu definieren.

- **Ausgehende Ansprüche**: Dies sind von Azure Attestation generierte Ansprüche. Es handelt sich um alle Ansprüche, die im Nachweistoken enthalten sind.

- **Eigenschaftsansprüche**: Ansprüche, die von Azure Attestation als Ausgabe erstellt werden. Dies enthält alle Ansprüche, die Eigenschaften des Nachweistokens darstellen, wie z. B. die Codierung des Berichts, die Gültigkeitsdauer des Berichts usw.

### <a name="common-incoming-claims-across-all-attestation-types"></a>Häufige eingehende Ansprüche für alle Nachweistypen

Die folgenden Ansprüche werden von Azure Attestation generiert und können zum Definieren von Autorisierungsregeln in einer benutzerdefinierten Richtlinie verwendet werden:
- **x-ms-ver**: JWT-Schemaversion („1.0“ wird erwartet)
- **x-ms-attestation-type**: Zeichenfolgenwert für Nachweistyp 
- **x-ms-policy-hash**: Hash der Azure Attestation-Evaluierungsrichtlinie, Berechnung als BASE64URL(SHA256(UTF8(BASE64URL(UTF8(Richtlinientext)))))
- **x-ms-policy-signer**: JSON-Objekt mit Member „jwk“ als Schlüssel, der von einem Kunden zum Signieren seiner Richtlinie für den Upload genutzt wird

Die unten angegebenen Ansprüche werden als veraltet eingestuft, aber sie werden vollständig unterstützt. Wir empfehlen Ihnen, die nicht als veraltet eingestuften Anspruchsnamen zu verwenden.

Veralteter Anspruch | Empfohlener Anspruch 
--- | --- 
ver | x-ms-ver
tee | x-ms-attestation-type
maa-policyHash | x-ms-policy-hash
policy_hash | x-ms-policy-hash
policy_signer | x-ms-policy-signer

### <a name="common-outgoing-claims-across-all-attestation-types"></a>Häufige ausgehende Ansprüche für alle Nachweistypen

Die folgenden Ansprüche werden vom [IETF-JWT](https://tools.ietf.org/html/rfc7519) definiert und können von Azure Attestation im Antwortobjekt verwendet werden:

- **Anspruch „jti“ (JWT-ID)**
- **Anspruch „iss“ (Aussteller)**
- **Anspruch „iat“ (Ausgestellt um)**
- **Anspruch „exp“ (Ablaufzeit)**
- **Anspruch „nbf“ (Nicht vor)**

Die folgenden Ansprüche werden vom [IETF-EAT](https://tools.ietf.org/html/draft-ietf-rats-eat-03#page-9) definiert und können von Azure Attestation im Antwortobjekt verwendet werden:
- **Anspruch „Nonce“ (Nonce)**

## <a name="claims-specific-to-sgx-enclaves"></a>Spezifische Ansprüche für SGX-Enklaven

### <a name="incoming-claims-specific-to-sgx-attestation"></a>Spezifische eingehende Ansprüche für SGX-Nachweis

Die folgenden Ansprüche werden vom Dienst für den SGX-Nachweis generiert und können zum Definieren von Autorisierungsregeln in einer benutzerdefinierten Richtlinie verwendet werden:
- **x-ms-sgx-is-debuggable**: ein boolescher Wert, der angibt, ob Debugging für die Enclave aktiviert ist oder nicht
- **x-ms-sgx-product-id**
- **x-ms-sgx-mrsigner**: Hexadezimal codierter Wert des Felds „mrsigner“ des Angebots
- **x-ms-sgx-mrenclave**: Hexadezimal codierter Wert des Felds „mrenclave“ des Angebots
- **x-ms-sgx-svn**: Im Angebot codierte Sicherheitsversionsnummer 

### <a name="outgoing-claims-specific-to-sgx-attestation"></a>Spezifische ausgehende Ansprüche für SGX-Nachweis

Die unten angegebenen Ansprüche werden vom Dienst generiert und in das Antwortobjekt für den SGX-Nachweis eingefügt:
- **x-ms-sgx-is-debuggable**: ein boolescher Wert, der angibt, ob Debugging für die Enclave aktiviert ist oder nicht
- **x-ms-sgx-product-id**
- **x-ms-ver**
- **x-ms-sgx-mrsigner**: Hexadezimal codierter Wert des Felds „mrsigner“ des Angebots
- **x-ms-sgx-mrenclave**: Hexadezimal codierter Wert des Felds „mrenclave“ des Angebots
- **x-ms-sgx-svn**: Im Angebot codierte Sicherheitsversionsnummer 
- **x-ms-sgx-ehd**: In Enklave enthaltene Daten im Format „BASE64URL(Enclave Held Data)“
- **x-ms-sgx-collateral**: JSON-Objekt mit der Beschreibung der Begleitdaten für die Durchführung des Nachweisvorgangs. Der Wert für den Anspruch „x-ms-sgx-collateral“ ist ein geschachteltes JSON-Objekt mit den folgenden Schlüssel-Wert-Paaren:
    - **qeidcertshash**: SHA256-Wert der QE-Identität, von der Zertifikate ausgestellt werden
    - **qeidcrlhash**: SHA256-Wert der QE-Identität, von der Zertifikate (Zertifikatssperrliste) ausgestellt werden
    - **qeidhash**: SHA256-Wert der Begleitdaten der QE-Identität
    - **quotehash**: SHA256-Wert des ausgewerteten Angebots
    - **tcbinfocertshash**: SHA256-Wert der TCB-Informationen zum Ausstellen von Zertifikaten
    - **tcbinfocrlhash**: SHA256-Wert der TCB-Informationen zum Ausstellen von Zertifikaten (Zertifikatssperrliste)
    - **tcbinfohash**: JSON-Objekt mit der Beschreibung der Begleitdaten für die Durchführung des Nachweisvorgangs

Die unten angegebenen Ansprüche werden als veraltet eingestuft, aber sie werden vollständig unterstützt und sind auch in Zukunft noch vorhanden. Wir empfehlen Ihnen, die nicht als veraltet eingestuften Anspruchsnamen zu verwenden.

Veralteter Anspruch | Empfohlener Anspruch
--- | --- 
$is-debuggable | x-ms-sgx-is-debuggable
$sgx-mrsigner | x-ms-sgx-mrsigner
$sgx-mrenclave | x-ms-sgx-mrenclave
$product-id | x-ms-sgx-product-id
$svn | x-ms-sgx-svn
$tee | x-ms-attestation-type
maa-ehd | x-ms-sgx-ehd
aas-ehd | x-ms-sgx-ehd
maa-attestationcollateral | x-ms-sgx-collateral

## <a name="claims-issued-specific-to-trusted-platform-module-tpm-attestation"></a>Spezifisch ausgestellte Ansprüche für den TPM-Nachweis (Trusted Platform Module)

### <a name="incoming-claims-can-also-be-used-as-outgoing-claims"></a>Eingehende Ansprüche (können auch als ausgehende Ansprüche verwendet werden)

- **aikValidated**:  Ein boolescher Wert, der Informationen dazu enthält, ob das Zertifikat des Nachweisidentitätsschlüssels (Attestation Identity Key, AIK) überprüft wurde
- **aikPubHash**:  Eine Zeichenfolge mit base64 (SHA256 (öffentlicher AIK-Schlüssel im DER-Format))
- **tpmVersion**:   Ein ganzzahliger Wert, der die Hauptversion des Trusted Platform Module (TPM) enthält
- **secureBootEnabled**: Ein boolescher Wert, der angibt, ob der sichere Start aktiviert ist
- **iommuEnabled**:  Ein boolescher Wert, der angibt, ob die Input-output memory management unit (Iommu) aktiviert ist
- **bootDebuggingDisabled**: Ein boolescher Wert, der angibt, ob das Startdebugging deaktiviert ist
- **notSafeMode**:  Ein boolescher Wert, der angibt, wenn das Fenster nicht im abgesicherten Modus ausgeführt wird
- **notWinPE**:  Ein boolescher Wert, der angibt, wenn das Fenster nicht im WinPE-Modus ausgeführt wird
- **vbsEnabled**:  Ein boolescher Wert, der angibt, ob VBS aktiviert ist
- **vbsReportPresent**:  Ein boolescher Wert, der angibt, ob der VBS-Enclavebericht verfügbar ist
- **enclaveAuthorId**:  Ein Zeichenfolgenwert, der den Base64Url-codierten Wert der Enclave-Autor-ID enthält – den Autorenbezeichner des primären Moduls für die Enclave
- **enclaveImageId**:  Ein Zeichenfolgenwert, der den Base64Url-codierten Wert der Enclave-Image-ID enthält – den Imagebezeichner des primären Moduls für die Enclave
- **enclaveOwnerId**:  Ein Zeichenfolgenwert, der den Base64Url-codierten Wert der Enclave-Besitzer-ID enthält – den Bezeichner des Besitzers des primären Moduls für die Enclave
- **enclaveFamilyId**:  Ein Zeichenfolgenwert, der den Base64Url-codierten Wert der Enclave-Familien-ID enthält – der Familienbezeichner des primären Moduls für die Enclave
- **enclaveSvn**:  Ein ganzzahliger Wert, der die Sicherheitsversionsnummer des primären Moduls für die Enclave enthält
- **enclavePlatformSvn**:  Ein ganzzahliger Wert, der die Sicherheitsversionsnummer der Plattform enthält, die die Enclave hostet
- **enclaveFlags**:  Der enclaveFlags-Anspruch ist ein ganzzahliger Wert, der Flags enthält, die die Runtimerichtlinie für die Enclave beschreiben
  
### <a name="outgoing-claims-specific-to-tpm-attestation"></a>Spezifische ausgehende Ansprüche für TPM-Nachweis

- **policy_hash**:  Zeichenfolgenwert, der den SHA256-Hash des Richtlinientexts enthält, der von BASE64URL(SHA256(BASE64URL(UTF8(Policy text)))) berechnet wurde
- **policy_signer**:  Enthält ein JWK mit dem öffentlichen Schlüssel oder der Zertifikatkette, die im Header der signierten Richtlinie vorhanden ist.
- **ver (Version)** :  Zeichenfolgenwert, der eine Version des Berichts enthält. Derzeit 1.0.
- **Anspruch „cnf“ (Bestätigung)** :  Der Anspruch „cnf“ wird verwendet, um den Eigentumsnachweisschlüssel zu identifizieren. Bestätigungsansprüche wie in RFC 7800 definiert, enthält den öffentlichen Teil des nachgewiesenen Enclaveschlüssels, der als JSON Web Key-Objekt (JWK) dargestellt ist (RFC 7517).
- **rp_data (Daten der vertrauenden Seite)** :  Die Daten der vertrauenden Seite, sofern in der Anforderung angegeben, die von der vertrauenden Seite als Nonce verwendet wird, um sicherzustellen, dass der Bericht aktuell ist
- **Anspruch „jti“ (JWT-ID)** : Der Anspruch "jti" (JWT-ID) stellt einen eindeutigen Bezeichner für das JWT bereit. Der Bezeichnerwert wird auf eine Art und Weise zugewiesen, die sicherstellt, dass eine geringe Wahrscheinlichkeit besteht, dass der gleiche Wert versehentlich einem anderen Datenobjekt zugewiesen ist.

### <a name="property-claims"></a>Eigenschaftsansprüche

- **report_validity_in_minutes**: Eine ganzzahliger Anspruch, der angibt, wie lange das Token gültig ist
  - **Default value(time)** : Ein Tag in Minuten
  - **Maximum value(time)** : Ein Jahr in Minuten
- **omit_x5c**: Boolescher Wert, der angibt, ob Azure Attestation das Zertifikat auslassen soll, mit dem die Dienstauthentizität nachgewiesen werden soll. Wenn dieser Wert wahr ist, wird x5t zum Nachweistoken hinzugefügt. Wenn dieser Wert falsch ist (Standard), wird x5t zum Nachweistoken hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen und Signieren einer Nachweisrichtlinie](author-sign-policy.md)
- [Einrichten von Azure Attestation mithilfe von PowerShell](quickstart-powershell.md)
