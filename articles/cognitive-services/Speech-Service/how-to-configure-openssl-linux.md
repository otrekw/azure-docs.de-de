---
title: Konfigurieren von OpenSSL für Linux
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie OpenSSL für Linux konfigurieren.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: ff8772f7c3c3213c010b0bdbd0d0aa8897404bac
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119982"
---
# <a name="configure-openssl-for-linux"></a>Konfigurieren von OpenSSL für Linux

Bei der Verwendung einer beliebigen Speech SDK-Version vor 1.9.0 wird [OpenSSL](https://www.openssl.org) dynamisch für die Version des Hostsystems konfiguriert. In späteren Versionen des Speech SDKs ist OpenSSL (Version [1.1.1b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)) statisch mit der Kernbibliothek des Speech SDKs verknüpft.

Um Konnektivität sicherzustellen, überprüfen Sie, ob OpenSSL-Zertifikate auf Ihrem System installiert wurden. Ausführen eines Befehls:
```bash
openssl version -d
```

Die Ausgabe auf Ubuntu/Debian-basierten Systemen sollte wie folgt aussehen:
```
OPENSSLDIR: "/usr/lib/ssl"
```

Überprüfen Sie, ob ein Unterverzeichnis `certs` unter OPENSSLDIR vorhanden ist. Im Beispiel oben wäre das `/usr/lib/ssl/certs`.

* Wenn `/usr/lib/ssl/certs` vorhanden ist und viele einzelne Zertifikatdateien (mit der Erweiterung `.crt`-oder `.pem`) enthält, ist keine weitere Aktion erforderlich.

* Wenn OPENSSLDIR etwas anderes als `/usr/lib/ssl` ist und/oder eine einzelne Zertifikatpaketdatei anstatt vieler einzelner Dateien vorhanden ist, müssen Sie eine geeignete SSL-Umgebungsvariable festlegen, um anzugeben, wo die Zertifikate zu finden sind.

## <a name="examples"></a>Beispiele

- OPENSSLDIR ist `/opt/ssl`. Es gibt ein Unterverzeichnis `certs` mit vielen `.crt`-oder `.pem`-Dateien.
Legen Sie die Umgebungsvariable `SSL_CERT_DIR` so fest, dass sie auf `/opt/ssl/certs` verweist, bevor Sie ein Programm ausführen, das das Speech SDK verwendet. Beispiel:
```bash
SSL_CERT_DIR=/opt/ssl/certs ./helloworld
```

- OPENSSLDIR ist `/etc/pki/tls`. Es ist eine Zertifikatpaketdatei vorhanden, z. B. `ca-bundle.pem` oder `ca-bundle.crt`.
Legen Sie die Umgebungsvariable `SSL_CERT_FILE` so fest, dass sie auf `/etc/pki/tls/ca-bundle.pem` verweist, bevor Sie ein Programm ausführen, das das Speech SDK verwendet. Beispiel:
```bash
SSL_CERT_FILE=/etc/pki/tls/ca-bundle.pem ./helloworld
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Speech SDK](speech-sdk.md)
