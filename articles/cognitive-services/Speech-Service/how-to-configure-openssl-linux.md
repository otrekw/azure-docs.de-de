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
ms.openlocfilehash: cadf31dede8ee81323076013d00b9431f597bda6
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156487"
---
# <a name="configure-openssl-for-linux"></a>Konfigurieren von OpenSSL für Linux

Bei der Verwendung einer beliebigen Speech SDK-Version vor 1.9.0 wird [OpenSSL](https://www.openssl.org) dynamisch für die Version des Hostsystems konfiguriert. In späteren Versionen des Speech SDKs ist OpenSSL (Version [1.1.1b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)) statisch mit der Kernbibliothek des Speech SDKs verknüpft.

## <a name="troubleshoot-connectivity"></a>Behandeln von Konnektivitätsproblemen

Wenn es bei der Verwendung der Version 1.9.0 des Speech SDK zu Verbindungsfehlern kommt, stellen Sie sicher, dass das Verzeichnis `ssl/certs` im Verzeichnis `/usr/lib` – das sich im Linux-Dateisystem befindet – vorhanden ist. Wenn das Verzeichnis `ssl/certs` *nicht vorhanden ist*, überprüfen Sie mit dem folgenden Befehl, wo OpenSSL in Ihrem System installiert ist:

```bash
which openssl
```

Suchen Sie dann das OpenSSL-Verzeichnis `certs`, und kopieren Sie den Inhalt dieses Verzeichnisses in das Verzeichnis `/usr/lib/ssl/certs`. Versuchen Sie als nächstes erneut zu prüfen, ob die Verbindungsprobleme behoben sind.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Speech SDK](speech-sdk.md)