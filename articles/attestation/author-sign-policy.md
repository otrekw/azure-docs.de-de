---
title: Erstellen und Signieren einer Azure Attestation-Richtlinie
description: Hier wird erläutert, wie Sie eine Nachweisrichtlinie erstellen und signieren.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: a3afb12ac831d87b03d0bb16d1b7ef553f1bb906
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "90006818"
---
# <a name="how-to-author-and-sign-an-attestation-policy"></a>Erstellen und Signieren einer Nachweisrichtlinie

Bei einer Nachweisrichtlinie handelt es sich um eine Datei, die in Microsoft Azure Attestation hochgeladen wird. Mit Azure Attestation kann eine Richtlinie in einem nachweisspezifischen Richtlinienformat hochgeladen werden. Alternativ kann auch eine mit JSON Web Signature codierte Version der Richtlinie hochgeladen werden. Der Richtlinienadministrator ist für das Schreiben der Nachweisrichtlinie verantwortlich. In den meisten Nachweisszenarien fungiert die vertrauende Seite als Richtlinienadministrator. Der Client, der den Nachweisaufruf durchführt, sendet einen Nachweisbeweis. Dieser wird vom Dienst analysiert und in eingehende Ansprüche konvertiert (verschiedene Eigenschaften, Wert). Anschließend verarbeitet der Dienst die Ansprüche basierend auf der Definition in der Richtlinie und gibt das berechnete Ergebnis zurück.

Die Richtlinie enthält Regeln, die die Autorisierungskriterien, Eigenschaften und den Inhalt des Nachweistokens bestimmen. Eine Beispielrichtliniendatei sieht wie folgt aus:

```
version=1.0;
authorizationrules
{
   c:[type="secureBootEnables", issuer=="AttestationService"]=> permit()
};

issuancerules
{
  c:[type="secureBootEnables", issuer=="AttestationService"]=> issue(claim=c)
  c:[type="notSafeMode", issuer=="AttestationService"]=> issue(claim=c)
};
```
 
Eine Richtliniendatei enthält drei Segmente, wie oben gezeigt:

- **version**:  Die Version ist die Versionsnummer der befolgten Grammatik. 

    ```
    version=MajorVersion.MinorVersion   
    ```

    Die einzige derzeit unterstützte Version lautet „1.0“.

- **authorizationrules**: Eine Sammlung von Anspruchsregeln, die zuerst geprüft werden, um zu bestimmen, ob Azure Attestation mit **issuancerules** fortfahren soll. Die Anspruchsregeln werden in der Reihenfolge angewendet, in der sie definiert wurden.

- **issuancerules**: Eine Sammlung von Anspruchsregeln, die ausgewertet werden, um dem Nachweisergebnis gemäß der Definition in der Richtlinie weitere Informationen hinzuzufügen. Die Anspruchsregeln werden in der Reihenfolge angewendet, in der sie definiert wurden, und sind ebenfalls optional.

Weitere Informationen finden Sie unter [Anspruch und Anspruchsregeln](claim-rule-grammar.md).
   
## <a name="drafting-the-policy-file"></a>Entwerfen der Richtliniendatei

1. Erstellen Sie eine neue Datei.
1. Fügen Sie der Datei eine Version hinzu.
1. Fügen Sie Abschnitte für **authorizationrules** und **issuancerules** hinzu.

  ```
  version=1.0;
  authorizationrules
  {
  =>deny();
  };
  
  issuancerules
  {
  };
  ```

  Die Autorisierungsregeln enthalten die Aktion „deny()“ ohne jegliche Bedingung. So wird sichergestellt, dass keine Ausstellungsregeln verarbeitet werden. Alternativ kann die Autorisierungsregel auch die Aktion „permit()“ enthalten, um die Verarbeitung von Ausstellungsregeln zuzulassen.
  
4. Hinzufügen von Anspruchsregeln zu den Autorisierungsregeln

  ```
  version=1.0;
  authorizationrules
  {
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules
  {
  };
  ```

  Wenn der eingehende Anspruchssatz einen Anspruch enthält, der mit Typ, Wert und Aussteller übereinstimmt, weist die Aktion „permit()“ die Richtlinien-Engine an, **issuancerules** zu verarbeiten.
  
5. Fügen Sie **issuancerules** Anspruchsregeln hinzu.

  ```
  version=1.0;
  authorizationrules
  {
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules
  {
  => issue(type="SecurityLevelValue", value=100);
  };
  ```
  
  Der ausgehende Anspruchssatz enthält einen Anspruch mit folgenden Angaben:

  ```
  [type="SecurityLevelValue", value=100, valueType="Integer", issuer="AttestationPolicy"]
  ```

  Komplexe Richtlinien können auf ähnliche Weise erstellt werden. Weitere Informationen finden Sie unter [Beispiele für eine Nachweisrichtlinie](policy-examples.md).
  
6. Speichern Sie die Datei .

## <a name="creating-the-policy-file-in-json-web-signature-format"></a>Erstellen der Richtliniendatei im JSON Web Signature-Format

Führen Sie nach der Erstellung einer Richtliniendatei die folgenden Schritte aus, um eine Richtlinie im JWS-Format hochzuladen:

1. Generieren Sie die JWS-Datei (RFC 7515) mit der Richtlinie (UTF-8-codiert) als Nutzlast.
     - Die Nutzlast-ID für die Base64Url-codierte Richtlinie muss „AttestationPolicy“ lauten.
     
     JWT-Beispiel:
     ```
     Header: {"alg":"none"}
     Payload: {"AttestationPolicy":" Base64Url (policy)"}
     Signature: {}

     JWS format: eyJhbGciOiJub25lIn0.XXXXXXXXX.
     ```

2. (Optional) Signieren Sie die Richtlinie. Azure Attestation unterstützt die folgenden Algorithmen:
     - **Keine:** Signieren Sie die Richtliniennutzlast nicht.
     - **RS256**: Unterstützter Algorithmus zum Signieren der Richtliniennutzlast

3. Laden Sie die JWS-Datei hoch, und überprüfen Sie die Richtlinie.
     - Enthält die Richtliniendatei keine Syntaxfehler, wird sie vom Dienst akzeptiert.
     - Enthält die Richtliniendatei Syntaxfehler, wird sie vom Dienst abgelehnt.

## <a name="signing-the-policy"></a>Signieren der Richtlinie

Nachfolgend sehen Sie ein Python-Beispielskript zum Ausführen eines Richtliniensignaturvorgangs:

```python
from OpenSSL import crypto
import jwt
import getpass
       
def cert_to_b64(cert):
              cert_pem = crypto.dump_certificate(crypto.FILETYPE_PEM, cert)
              cert_pem_str = cert_pem.decode('utf-8')
              return ''.join(cert_pem_str.split('\n')[1:-2])
       
print("Provide the path to the PKCS12 file:")
pkcs12_path = str(input())
pkcs12_password = getpass.getpass("\nProvide the password for the PKCS12 file:\n")
pkcs12_bin = open(pkcs12_path, "rb").read()
pkcs12 = crypto.load_pkcs12(pkcs12_bin, pkcs12_password.encode('utf8'))
ca_chain = pkcs12.get_ca_certificates()
ca_chain_b64 = []
for chain_cert in ca_chain:
   ca_chain_b64.append(cert_to_b64(chain_cert))
   signing_cert_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM, pkcs12.get_privatekey())
signing_cert_b64 = cert_to_b64(pkcs12.get_certificate())
ca_chain_b64.insert(0, signing_cert_b64)

print("Provide the path to the policy text file:")
policy_path = str(input())
policy_text = open(policy_path, "r").read()
encoded = jwt.encode({'text': policy_text }, signing_cert_pkey, algorithm='RS256', headers={'x5c' : ca_chain_b64})
print("\nAttestation Policy JWS:")
print(encoded.decode('utf-8'))
```

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten von Azure Attestation mithilfe von PowerShell](quickstart-powershell.md)
- [Erbringen eines Nachweises für eine SGX-Enclave mithilfe von Codebeispielen](https://docs.microsoft.com/samples/browse/?expanded=azure&terms=attestation)
