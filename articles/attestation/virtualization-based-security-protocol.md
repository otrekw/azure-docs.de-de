---
title: VBS-Protokoll (Virtualization-based Security) für Azure Attestation
description: VBS-Nachweisprotokoll
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: e5cc3b5fb7ca38df196119de12d346f5d0346b58
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91339325"
---
# <a name="virtualization-based-security-vbs-attestation-protocol"></a>VBS-Nachweisprotokoll (Virtualization-based Security) 

Damit Microsoft Azure Attestation strenge Sicherheitsgarantien bieten kann, dass die gemeldeten Daten authentisch sind, ist es erforderlich, eine Vertrauenskette von der Firmware bis zum Hypervisorstart und dem Start des sicheren Kernels zu erstellen. Dazu muss Azure Attestation den Startstatus des Computers nachweisen, bevor eine Vertrauensstellung zur sicheren Enclave hergestellt werden kann. Die Binärdateien für Betriebssystem, Hypervisor und sicheren Kernel müssen von den richtigen offiziellen Microsoft-Stellen signiert und sicher konfiguriert werden. Sobald eine Vertrauensstellung zwischen dem Trusted Platform Module (TPM) und der Hypervisorintegrität hergestellt wurde, können die VSM-IDKs im Protokoll für kontrollierten Start als vertrauenswürdig eingestuft werden. So ist es möglich, zu überprüfen, ob ein Schlüsselpaar von der Enclave erstellt wurde, und Sie können einen Nachweisbericht erstellen, der die Vertrauensstellung für diesen Schlüssel bindet und weitere Ansprüche wie die Sicherheitsebene und Eigenschaften des Startnachweises enthält.

## <a name="protocol-messages"></a>Protokollnachrichten

### <a name="init-message"></a>Init-Nachricht

#### <a name="direction"></a>Direction

Client -> Azure Attestation

#### <a name="payload"></a>Nutzlast

```
{
  "type": "aikcert"
}
```

„type“ (ASCII-Zeichenfolge): steht für den Typ des angeforderten Nachweises. Derzeit wird nur „aikcert“ unterstützt.

### <a name="challenge-message"></a>Challenge-Nachricht

#### <a name="direction"></a>Direction

Azure Attestation -> Client

#### <a name="payload"></a>Nutzlast

```
{

  "challenge": "<BASE64URL(CHALLENGE)>",
  
  "service_context": "<BASE64URL(SERVICECONTEXT)>"
  
}
```

**challenge** (BASE64URL(OCTETS)): Hierbei handelt es sich um einen zufällig vom Dienst ausgegeben Wert.

**service_context** (BASE64URL(OCTETS)): Opaker, nicht verschlüsselter vom Dienst erstellter Kontext, der u. a. den challenge-Wert sowie eine Ablaufzeit für diesen Wert umfasst.


### <a name="request-message"></a>Request-Nachricht

#### <a name="direction"></a>Direction

Client -> Azure Attestation 

#### <a name="payload"></a>Nutzlast

```
{

  "request": "<JWS>"
  
}
```

**request** (JWS): „request“ besteht aus einer JWS-Struktur (JSON Web Signature). Der geschützte JWS-Header und die JWS-Payload werden unten gezeigt. Wie bei jeder JWS-Struktur besteht der endgültige Wert aus Folgendem:

BASE64URL(UTF8(geschützter JWS-Header)) || '.' ||

BASE64URL(JWS-Payload) || '.' ||

BASE64URL(JWS-Signatur)

##### <a name="jws-protected-header"></a>Geschützter JWS-Header

```
{
  "alg": "PS256",
  "typ": "attReq"
  // no "kid" parameter as the key specified by attest_key MUST sign this JWS to prove possession.
}
```

##### <a name="jws-payload"></a>JWS-Payload

Die JWS-Payload kann als Wert „basic“ oder „VBS“ aufweisen. „Basic“ wird verwendet, wenn der Beweis für einen Nachweis keine VBS-Daten enthält.

Einfaches Beispiel

``` 
{
  "att_type": "basic",
  "att_data": {
    "rp_id": "<URL>",
    "rp_data": "<BASE64URL(RPCUSTOMDATA)>",
    "challenge": "<BASE64URL(CHALLENGE)>",
    "tpm_att_data": {
      "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>",
      "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>",
      "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>",
      "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>",
      "aik_cert": "<BASE64URL(AIKCERTIFICATE)>",
      // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517).
      "aik_pub": {
        "kty": "RSA",
        "n": "<Base64urlUInt(MODULUS)>",
        "e": "<Base64urlUInt(EXPONENT)>"
      },
      "current_claim": "<BASE64URL(CURRENTCLAIM)>",
      "boot_claim": "<BASE64URL(BOOTCLAIM)>"
    },
    // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517).
    "attest_key": {
      "kty": "RSA",
      "n": "<Base64urlUInt(MODULUS)>",
      "e": "<Base64urlUInt(EXPONENT)>"
    },
    "custom_claims": [
      {
        "name": "<name>",
        "value": "<value>",
        "value_type": "<value_type>"
      },
      {
        "name": "<name>",
        "value": "<value>",
        "value_type": "<value_type>"
      }
    ],
    "service_context": "<BASE64URL(SERVICECONTEXT)>"
  }
}
```

VBS-Beispiel

``` 
{
  "att_type": "vbs",
  "att_data": {
    "report_signed": {
      "rp_id": "<URL>",
      "rp_data": "<BASE64URL(RPCUSTOMDATA)>",
      "challenge": "<BASE64URL(CHALLENGE)>",
      "tpm_att_data": {
        "srtm_boot_log": "<BASE64URL(SRTMBOOTLOG)>",
        "srtm_resume_log": "<BASE64URL(SRTMRESUMELOG)>",
        "drtm_boot_log": "<BASE64URL(DRTMBOOTLOG)>",
        "drtm_resume_log": "<BASE64URL(DRTMRESUMELOG)>",
        "aik_cert": "<BASE64URL(AIKCERTIFICATE)>",
        // aik_pub is represented as a JSON Web Key (JWK) object (RFC 7517).
        "aik_pub": {
          "kty": "RSA",
          "n": "<Base64urlUInt(MODULUS)>",
          "e": "<Base64urlUInt(EXPONENT)>"
        },
        "current_claim": "<BASE64URL(CURRENTCLAIM)>",
        "boot_claim": "<BASE64URL(BOOTCLAIM)>"
      },
      // attest_key is represented as a JSON Web Key (JWK) object (RFC 7517).
      "attest_key": {
        "kty": "RSA",
        "n": "<Base64urlUInt(MODULUS)>",
        "e": "<Base64urlUInt(EXPONENT)>"
      },
      "custom_claims": [
        {
          "name": "<name>",
          "value": "<value>",
          "value_type": "<value_type>"
        },
        {
          "name": "<name>",
          "value": "<value>",
          "value_type": "<value_type>"
        }
      ],
      "service_context": "<BASE64URL(SERVICECONTEXT)>"
    },
    "vbs_report": "<BASE64URL(REPORT)>"
  }
}
``` 

**rp_id** (StringOrURI): Hierbei handelt es sich um den Bezeichner der vertrauenden Seite. Dieser wird vom Dienst bei der Berechnung des Anspruchs der Computer-ID verwendet.

**rp_data** (BASE64URL(OCTETS)): Hierbei handelt es sich um opake Daten, die von der vertrauenden Seite übergeben werden. Die Verwendung erfolgt in der Regel als Nonce durch die vertrauende Seite, um die Aktualität eines Berichts zu gewährleisten.

**challenge** (BASE64URL(OCTETS)): Hierbei handelt es sich um einen zufällig vom Dienst ausgegeben Wert.

**tpm_att_data:** Hierbei handelt es sich um TPM-Nachweisdaten.

- **srtm_boot_log (BASE64URL(OCTETS)):** Hierbei handelt es sich um das SRTM-Startprotokoll, wie es von der Funktion Tbsi_Get_TCG_Log_Ex mit log type = TBS_TCGLOG_SRTM_BOOT abgerufen wurde.

- **srtm_resume_log (BASE64URL(OCTETS)):** Hierbei handelt es sich um das SRTM-Wiederaufnahmeprotokoll, wie es von der Funktion Tbsi_Get_TCG_Log_Ex mit log type = TBS_TCGLOG_SRTM_RESUME abgerufen wurde.

- **drtm_boot_log (BASE64URL(OCTETS)):** Hierbei handelt es sich um das DRTM-Startprotokoll, wie es von der Funktion Tbsi_Get_TCG_Log_Ex mit log type = TBS_TCGLOG_DRTM_BOOT abgerufen wurde.

- **drtm_resume_log (BASE64URL(OCTETS)):** Hierbei handelt es sich um das DRTM-Wiederaufnahmeprotokoll, wie es von der Funktion Tbsi_Get_TCG_Log_Ex mit log type = TBS_TCGLOG_DRTM_RESUME abgerufen wurde.

- **aik_cert (BASE64URL(OCTETS)):** Hierbei handelt es sich um das für AIK von der Funktion NCryptGetProperty mit der Eigenschaft = NCRYPT_CERTIFICATE_PROPERTY zurückgegebene X.509-Zertifikat.

- **aik_pub:** Hierbei handelt es sich um den öffentlichen als JWK-Objekt (JSON Web Key, RFC 7517) dargestellten AIK-Teil.

- **current_claim (BASE64URL(OCTETS)):** Hierbei handelt es sich um den Nachweisanspruch für den aktuellen PCR-Status, wie er von der Funktion NCryptCreateClaim mit dwClaimType = NCRYPT_CLAIM_PLATFORM und mit so festgelegtem Parameter NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK zurückgegeben wird, dass alle PCRs eingeschlossen werden. Der vom Dienst gesendete challenge-Wert sollte auch für die Berechnung dieses Anspruchs verwendet werden.

- **boot_claim (BASE64URL(OCTETS)):** Hierbei handelt es sich um den Nachweisanspruch für den PCR-Status beim Start, wie er von der Funktion NCryptCreateClaim mit dwClaimType = NCRYPT_CLAIM_PLATFORM und mit so festgelegtem Parameter NCRYPTBUFFER_TPM_PLATFORM_CLAIM_PCR_MASK zurückgegeben wird, dass alle PCRs eingeschlossen werden.

**vbs report** (BASE64URL(OCTETS)): Hierbei handelt es sich um den Nachweisbericht der VBS-Enclave, wie er von der Funktion EnclaveGetAttestationReport zurückgegeben wird. Beim EnclaveData-Parameter muss es sich um den SHA-512-Hash oder den Wert von report_signed (einschließlich der öffnenden und schließenden Klammer) handeln. Die Hashfunktionseingabe ist UTF8(report_signed).

**attest_key:** Hierbei handelt es sich um den öffentlichen als JWK-Objekt (JSON Web Key, RFC 7517) dargestellten Teil des Enclaveschlüssels.

**custom_claims:** Hierbei handelt es sich um ein Array benutzerdefinierter Enclaveansprüche, die an den Dienst gesendet werden und die von der Richtlinie ausgewertet werden können. Der Anspruch

- **name (Zeichenfolge):** Hierbei handelt es sich um den Namen des Anspruchs. Dieser Name wird an eine URL angefügt, die vom Attestation-Dienst bestimmt wurde (um Konflikte zu vermeiden). Die verkettete Zeichenfolge wird zum Typ des Anspruchs, der in der Richtlinie verwendet werden kann.

- **value (Zeichenfolge):** Hierbei handelt es sich um den Wert des Anspruchs.

- **value_type (Zeichenfolge):** Hierbei handelt es sich um den Datentyp des Werts des Anspruchs.

**service_context** (BASE64URL(OCTETS)): Opaker, nicht verschlüsselter vom Dienst erstellter Kontext, der u. a. den challenge-Wert sowie eine Ablaufzeit für diesen Wert umfasst.

### <a name="report-message"></a>Report-Nachricht

#### <a name="direction"></a>Direction

Azure Attestation -> Client

#### <a name="payload"></a>Nutzlast

```
{
  "report": "<JWT>"
}
```

**report** (JWT): Hierbei handelt es sich um den Nachweisbericht im JWT-Format (JSON Web Token, RFC 7519).
