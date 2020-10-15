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
ms.openlocfilehash: 42960c25c4124203b64646fdc5cbca833b246e21
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81683163"
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
export SSL_CERT_DIR=/opt/ssl/certs
```

- OPENSSLDIR ist `/etc/pki/tls` (wie bei RHEL-/CentOS-basierten Systemen). Es ist ein Unterverzeichnis vom Typ `certs` mit einer Zertifikatpaketdatei vorhanden, z. B. `ca-bundle.crt`.
Legen Sie die Umgebungsvariable `SSL_CERT_FILE` so fest, dass sie auf diese Datei verweist, bevor Sie ein Programm ausführen, das das Speech SDK verwendet. Beispiel:
```bash
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt
```
> [!NOTE]
> Beachten Sie auch, dass in einigen Linux-Distributionen keine TMP- oder TMPDIR-Umgebungsvariable definiert ist. Dies führt dazu, dass das Speech SDK die Zertifikatsperrliste (Certificate Revocation List, CRL) jedes Mal herunterlädt, statt diese Liste zur Wiederverwendung bis zum Ablauf auf einem Datenträger zwischenzuspeichern. Um die Leistung bei der Verbindungsherstellung zu verbessern, können Sie [eine Umgebungsvariable namens TMPDIR erstellen und auf den Pfad des von Ihnen ausgewählten temporären Verzeichnisses festlegen](https://help.ubuntu.com/community/EnvironmentVariables).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Speech SDK](speech-sdk.md)
