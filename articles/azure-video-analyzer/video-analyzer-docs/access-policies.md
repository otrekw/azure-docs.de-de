---
title: Azure Video Analyzer-Zugriffsrichtlinien
description: Dieser Artikel erklärt, wie Azure Video Analyzer JWT-Tokens in Zugriffsrichtlinien verwendet, um Videos zu sichern.
ms.topic: reference
ms.date: 06/01/2021
ms.openlocfilehash: 3cf450249567d07bf6855d115a0e39640074eeb0
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114604185"
---
# <a name="access-policies"></a>Zugriffsrichtlinien

Die Zugriffsrichtlinien bestimmen die Berechtigungen und die Dauer des Zugriffs auf eine bestimmten Azure Video Analyzer-Videoressource. Diese Zugriffsrichtlinien ermöglichen eine größere Kontrolle und Flexibilität, indem sie JWT-Tokens von 3-Anbietern (Nicht-AAD-Clients) erlauben, die Autorisierung für Client-APIs zu ermöglichen: 

- Zugriff auf Video-Metadaten. 
- Zugriff auf Video-Streaming. 

## <a name="access-policy-definition"></a>Definition der Azure-Zugriffsrichtlinie

```json
"name": "accesspolicyname1", 
"properties": { 
    "role": "Reader", 
    "authentication": { 
        "@type": "#Microsoft.VideoAnalyzer.JwtAuthentication", 
        "issuers": [ 
            "issuer1", 
            "issuer2" 
        ], 
        "audiences": [ 
            "audience1" 
        ], 
        "claims": [ 
            { 
                "name":"claimname1", 
                "value":"claimvalue1" 
            }, 
            { 
                "name":"claimname2", 
                "value":"claimvalue2" 
            } 
        ], 
        "keys": [ 
            { 
                "@type": "#Microsoft.VideoAnalyzer.RsaTokenKey", 
                "alg": "RS256", 
                "kid": "123", 
                "n": "YmFzZTY0IQ==", 
                "e": "ZLFzZTY0IQ==" 
            }, 
            { 
                "@type": "#Microsoft.VideoAnalyzer.EccTokenKey", 
                "alg": "ES256", 
                "kid": "124", 
                "x": "XX==", 
                "y": "YY==" 
            } 
        ] 
    } 
} 
```

> [!NOTE] 
> Es wird nur ein Tastentyp benötigt. 

### <a name="roles"></a>Rollen

Derzeit wird nur die Leserrolle unterstützt.

### <a name="issuer-matching-rules"></a>Übereinstimmungsregeln für Zertifikatsaussteller

In der Richtlinie können mehrere Ausgaben angegeben werden, im Token kann ein einzelner Aussteller angegeben werden.  Der Aussteller stimmt überein, wenn der Token-Aussteller sich unter den in der Richtlinie angegebenen Ausstellern befindet.

### <a name="audience-matching-rules"></a>Regeln für den Abgleich mit der Zielgruppe

Wenn der Zielgruppenwert ${System.Runtime.BaseResourceUrlPattern} für die Videoressource ist, muss die im JWT-Token angegebene Zielgruppe mit der URL der Basisressource übereinstimmen. Wenn nicht, dann muss die Token-Zielgruppe mit der Zielgruppe aus der Zugriffsrichtlinie übereinstimmen.

### <a name="claims-matching-rules"></a>Regeln für den Abgleich von Ansprüchen

In der Zugriffsrichtlinie und im JWT-Token können mehrere Ansprüche angegeben werden.  Alle Ansprüche aus einer Zugriffsrichtlinie müssen im Token enthalten sein, um die Validierung zu bestehen. Das JWT-Token kann jedoch zusätzliche Ansprüche enthalten, die nicht in der Zugriffsrichtlinie aufgeführt sind.

### <a name="keys"></a>Schlüssel

Es werden zwei Arten von Schlüsseln unterstützt, nämlich die RSA- und die ECC-Schlüssel.

[RSA](https://wikipedia.org/wiki/RSA_(cryptosystem))

* @type- \#Microsoft.VideoAnalyzer.RsaTokenKey
* alg – Algorithmus.  Kann 256, 384 oder 512 sein 
* kid – Schlüssel-ID
* n – Modulus
* e – Öffentlicher Exponent 

[ECC](https://wikipedia.org/wiki/Elliptic-curve_cryptography)        

* @type- \#Microsoft.VideoAnalyzer.EccTokenKey
* alg – Algorithmus.  Kann 256, 384 oder 512 sein
* kid – Schlüssel-ID
* x – Koordinatenwert.
* y – Koordinatenwert.

### <a name="token-validation-process"></a>Token-Validierungsvorgang

Kunden müssen ihre eigenen JWT-Tokens erstellen und sie werden anhand der folgenden Methode validiert:

- Aus der Liste der Richtlinien, die mit der Schlüssel-ID übereinstimmen, validieren wir:
  - Tokensignatur
  - Tokenablauf
  - Aussteller
  - Zielgruppe
  - Zusätzliche Ansprüche

### <a name="policy-audience-and-token-matching-examples"></a>Beispiele für die Azure-Richtlinien-Zielgruppe und Token-Abgleich:

| **Richtlinien-Zielgruppe**                      | Angeforderte URL                         | Token-URL                            | Ergebnis |
| ---------------------------------------- | ------------------------------------- | ------------------------------------ | ------ |
| (Beliebiges Literal)                            | (JEGLICHES)                                 | (Übereinstimmung)                              | Erteilen  |
| (Beliebiges Literal)                            | (JEGLICHES)                                 | (Keine Übereinstimmung)                          | Verweigern   |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos                   | https://fqdn/videos/*                | Erteilen  |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos                   | https://fqdn/videos/{videoName}      | Verweigern   |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{videoName}       | https://fqdn/vid*                    | Erteilen  |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{videoName}       | https://fqdn/videos/*                | Erteilen  |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{videoName}       | https://fqdn/videos/{baseVideoName}* | Erteilen  |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{videoName}       | https://fqdn/videos/{videoName}      | Erteilen  |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{videoName}Suffix | https://fqdn/videos/{videoName}      | Verweigern   |
| ${System.Runtime.BaseResourceUrlPattern} | https://fqdn/videos/{otherVideoName}  | https://fqdn/videos/{videoName}      | Verweigern   |

> [!NOTE]  
> Azure Video Analyzer unterstützt maximal 20 Richtlinien.  ${System.Runtime.BaseResourceUrlPattern} ermöglicht eine größere Flexibilität beim Zugriff auf bestimmte Ressourcen durch die Verwendung einer Zugriffsrichtlinie und mehrerer Token.  Diese Token ermöglichen wiederum den Zugriff auf verschiedene Azure Video Analyzer-Ressourcen, je nach Zielgruppe. 

## <a name="creating-an-access-policy"></a>Erstellen einer Azure-Zugriffs-Richtlinie

Es gibt zwei Möglichkeiten, eine Zugriffsrichtlinie zu erstellen.

### <a name="in-the-azure-portal"></a>Im Azure-Portal

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrer Ressourcengruppe, in der sich Ihr Azure Video Analyzer-Konto befindet.
2. Wählen Sie die Azure Video Analyzer-Ressource aus.
3. Wählen Sie unter Azure Video Analyzer die Option Zugriffsrichtlinien aus

   :::image type="content" source="./media/access-policies/access-policies-menu.png" alt-text="Zugriff auf das Richtlinien-Menü im Azure-Portal":::
4. Klicken Sie auf neu, und geben Sie Folgendes ein:

   - Name der Zugriffsrichtlinie – beliebiger Name
   - Aussteller – muss mit dem JWT-Token-Aussteller übereinstimmen 
   - Zielgruppe – Zielgruppe für das JWT-Token -- ${System.Runtime.BaseResourceUrlPattern} ist die Standardeinstellung. 
   - Schlüsseltyp – kty 
   - Algorithmus – alg
   - Schlüssel-ID – kid 
   - N- / X-Wert 
   - E- / Y- Wert 

   :::image type="content" source="./media/access-policies/access-policies-portal.png" alt-text="Zugriffsrichtlinie im Azure-Portal":::
5. Klicken Sie auf `Save`.

### <a name="create-access-policy-via-api"></a>Erstellen Sie eine Zugriffsrichtlinie via API

Siehe Azure Resource Manager (ARM) API 

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](overview.md)
