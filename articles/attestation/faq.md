---
title: Häufig gestellte Fragen
description: Antworten auf häufig gestellte Fragen zu Microsoft Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: reference
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: 39f628845bdc9d54b48b1c8037f4a506a9d5c00a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89236533"
---
# <a name="frequently-asked-questions-for-microsoft-azure-attestation"></a>Häufig gestellte Fragen zu Microsoft Azure Attestation

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zu [Azure Attestation](overview.md).

Wenn Ihr Azure-Problem in diesem Artikel nicht behandelt wird, können Sie auf der [Azure-Supportseite](https://azure.microsoft.com/support/options/) auch eine Azure-Supportanfrage einreichen.

## <a name="what-is-azure-pck-caching-service-and-its-role-in-enclave-attestation"></a>Was ist der Azure PCK-Cachedienst, und welche Rolle spielt er im Enklavennachweis?

Der Azure PCK Cachedienst definiert die Azure-Sicherheitsbaseline für die [ACC-Knoten (Azure Confidential Computing)](../confidential-computing/overview.md) von Intel und speichert die Daten zwischen. Die zwischengespeicherten Informationen werden außerdem von Azure Attestation bei der Validierung von TEEs (Trusted Execution Environments) verwendet.  

Azure PCK-Cachedienst:
   - Bietet Hochverfügbarkeit 
   - Reduziert Abhängigkeiten von extern gehosteten Diensten und Internetkonnektivität.
   - Ruft die aktuellen Versionen von Intel-Zertifikaten, -Zertifikatsperrlisten, TCB-Informationen (Trusted Computing Base) und die Quoting Enclave-Identität der ACC-Knoten von Intel ab. Der Dienst bestätigt daher bei der Überprüfung der TEEs die Azure-Sicherheitsbaseline, auf die Azure Attestation verweisen soll, und reduziert so die Nachweisfehler aufgrund von Invalidierung oder Sperrung von Intel-Zertifikaten erheblich.  

## <a name="is-sgx-attestation-supported-by-azure-attestation-in-non-azure-environments"></a>Wird der SGX-Nachweis von Azure Attestation in Nicht-Azure-Umgebungen unterstützt?

Azure Attestation hängt von der vom Azure PCK-Cachedienst festgelegten Sicherheitsbaseline ab, um die TEEs zu validieren. Der Azure PCK-Cachedienst ist zurzeit nur für die Unterstützung von Azure Confidential-Computeknoten konzipiert. 

## <a name="what-validations-does-azure-attestation-perform-for-attesting-sgx-enclaves"></a>Welche Überprüfungen werden von Azure Attestation als Nachweis von SGX-Enklaven durchgeführt?

Azure Attestation ist ein einheitliches Framework für den Remotenachweis verschiedener Arten von TEEs. Azure Attestation:

   - Überprüft, ob der vertrauenswürdige Stamm einer signierten Enclave Quote zu Intel gehört.
   - Überprüft, ob die Enclave Quote die Azure-Sicherheitsbaseline wie vom Azure PCK-Cachedienst definiert erfüllt.
   - Überprüft, ob der SHA256-Hash der EHD (Enclave Held Data) im Nachweisanforderungsobjekt mit den ersten 32 Bytes des reportData-Felds in der Enclave Quote übereinstimmt.
   - Ermöglicht Kunden das Erstellen eines Nachweisanbieters und das Konfigurieren einer benutzerdefinierten Richtlinie. Zusätzlich zu den oben aufgeführten Überprüfungen wertet Azure Attestation die Enclave Quote anhand der Richtlinie aus. Richtlinien definieren Autorisierungsregeln für die Enklave und geben auch Ausstellungsregeln zum Generieren des Nachweistokens vor. Um zu bestätigen, dass beabsichtigte Software in einer Enklave ausgeführt wird, können Kunden Autorisierungsregeln hinzufügen, um zu überprüfen, ob die Felder **mrsigner** und **mrenclave** in der Enclave Quote mit den Werten von Kundenbinärdateien übereinstimmen.

## <a name="how-can-a-verifier-obtain-the-collateral-for-sgx-attestation-supported-by-azure-attestation"></a>Wie kann ein Verifizierer die Sicherheiten für den durch Azure Attestation unterstützten SGX-Nachweis abrufen?

Bei den Nachweismodellen, bei denen Intel der Stamm der Vertrauensstellung ist, kommuniziert der Nachweisclient im Allgemeinen mit Enklaven-APIs, um die Enklavennachweise abzurufen. Enklaven-APIs rufen den Intel PCK-Cachedienst intern auf, um Intel-Zertifikate des nachzuweisenden Knotens abzurufen. Die Zertifikate werden verwendet, um den Enklavennachweis zu signieren und so eine remote nachweisbare Sicherheit zu generieren.  

Der gleiche Prozess kann für Azure Attestation implementiert werden. Wenn Sie jedoch die Vorteile nutzen möchten, die der Azure PCK-Cachedienst bietet, empfiehlt es sich, nach der Installation des virtuellen ACC-Computers die [Azure DCAP-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.DCAP) zu installieren. Basierend auf der Vereinbarung mit Intel werden bei der Installation der Azure DCAP-Bibliothek die Anforderungen zum Erstellen von Enklavennachweisen vom Intel PCK-Cachedienst an den Azure PCK-Cachedienst umgeleitet. Die Azure DCAP-Bibliothek wird in Windows- und Linux-basierten Umgebungen unterstützt.

## <a name="how-to-shift-to-azure-attestation-from-other-attestation-models"></a>Vorgehensweise beim Verlagern von anderen Nachweismodellen zu Azure Attestation

- Installieren Sie nach der Installation des virtuellen Azure Confidential-Computers die Azure DCAP-Bibliothek ([Windows/](https://www.nuget.org/packages/Microsoft.Azure.DCAP/) [Linux](https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/a/az-dcap-client/)), um die Vorteile des Azure PCK-Cachediensts zu nutzen.
- Der Remotenachweisclient muss erstellt werden, mit dem die Enklavennachweise abgerufen und Anforderungen an Azure Attestation gesendet werden können. Weitere Informationen finden Sie unter [Codebeispiele](/samples/browse/?expanded=azure&terms=attestation). 
- Nachweisanforderungen können an den REST-API-Endpunkt von Standardanbietern oder benutzerdefinierten Nachweisanbietern gesendet werden. 
- Azure Attestation-APIs werden durch Azure AD-Authentifizierung geschützt. Daher muss der Client, der die Nachweis-APIs aufruft, in der Lage sein, in der Nachweisanforderung ein gültiges Azure AD-Zugriffstoken abzurufen und zu übergeben. 

## <a name="how-can-the-relying-party-verify-the-integrity-of-attestation-token"></a>Wie kann die vertrauende Seite die Integrität des Nachweistokens überprüfen?

Das von Azure Attestation generierte Nachweistoken wird mit einem selbstsignierten Zertifikat signiert. Die Zertifikate werden über einen [OpenID-Metadatenendpunkt](/rest/api/attestation/metadataconfiguration/get) verfügbar gemacht. Die vertrauende Seite kann die Signaturzertifikate von diesem Endpunkt abrufen und die Signaturüberprüfung des Nachweistokens durchführen. Die Gültigkeitsdauer des Nachweistokens beträgt 8 Stunden. 

## <a name="how-to-identify-the-certificate-to-be-used-for-signature-verification-from-the-openid-metadata-endpoint"></a>Identifizieren des Zertifikats, das für die Signaturüberprüfung vom OpenID-Metadatenendpunkt verwendet werden soll

Mehrere Zertifikate, die im OpenID-Metadatenendpunkt verfügbar gemacht werden, entsprechen verschiedenen Anwendungsfällen (z. B. SGX-Nachweis), die von Azure Attestation unterstützt werden. Gemäß den Standards, die durch [RFC 7515](https://tools.ietf.org/html/rfc7515) angegeben werden, muss das Zertifikat mit der Schlüssel-ID (kid), die mit dem *kid*-Parameter im Header des Nachweistokens übereinstimmt, für die Signaturüberprüfung verwendet werden. Wenn keine übereinstimmende **kid** gefunden wird, wird erwartet, dass alle Zertifikate ausprobiert werden, die vom OpenID-Metadatenendpunkt bereitgestellt werden.

## <a name="is-it-possible-for-the-relying-party-to-share-secrets-with-the-validated-trusted-execution-environments-tees"></a>Ist es möglich, dass die vertrauende Seite geheime Schlüssel mit den überprüften TEEs (Trusted Execution Environments) gemeinsam nutzt?

Ein innerhalb einer Enklave generierter öffentlicher Schlüssel kann in der EHD-Eigenschaft (Enclave Held Data) des Nachweisanforderungsobjekts ausgedrückt werden, das vom Client an Azure Attestation gesendet wird. Nachdem bestätigt wurde, dass der SHA256-Hash von EHD im reportData-Feld der Quote ausgedrückt wird, schließt Azure Attestation EHD in das Nachweistoken ein. Die vertrauende Seite kann die EHD aus der verifizierten Nachweisantwort verwenden, um die geheimen Schlüssel zu verschlüsseln und mit der Enklave zu teilen. Weitere Informationen finden Sie unter [Grundlegende Konzepte von Azure Attestation](basic-concepts.md).
