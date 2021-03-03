---
title: Microsoft Connected Cache in einer Azure IoT Edge for Industrial IoT-Konfiguration | Microsoft-Dokumentation
description: 'Tutorial: Microsoft Connected Cache in einer Azure IoT Edge for Industrial IoT-Konfiguration'
author: andyriv
ms.author: andyriv
ms.date: 2/16/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: 0d70ed8b906c171c001c5bda81a79ca9b65febac
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658743"
---
# <a name="microsoft-connected-cache-preview-deployment-scenario-sample-microsoft-connected-cache-within-an-azure-iot-edge-for-industrial-iot-configuration"></a>Bereitstellungsszenariobeispiel für Microsoft Connected Cache Vorschau: Microsoft Connected Cache in einer Azure IoT Edge for Industrial IoT-Konfiguration

Fertigungsnetzwerke sind häufig in hierarchischen Ebenen nach dem [Purdue-Netzwerkmodell](https://en.wikipedia.org/wiki/Purdue_Enterprise_Reference_Architecture) organisiert (in den [ISA 95](https://en.wikipedia.org/wiki/ANSI/ISA-95)- und [ISA 99](https://www.isa.org/standards-and-publications/isa-standards/isa-standards-committees/isa99)-Standards enthalten). In diesen Netzwerken verfügt nur die oberste Ebene über Konnektivität mit der Cloud, und die niedrigeren Ebenen in der Hierarchie können nur mit der Ebene direkt darüber bzw. darunter kommunizieren.

Dieses GitHub-Beispiel [Azure IoT Edge für Industrial IoT](https://github.com/Azure-Samples/iot-edge-for-iiot) stellt Folgendes bereit:

* Simuliertes Purdue-Netzwerk in Azure
* Industrieanlagen 
* Hierarchie von Azure IoT Edge-Gateways
  
Diese Komponenten werden verwendet, um Industriedaten abzurufen und sicher in die Cloud hochzuladen, ohne die Sicherheit des Netzwerks zu beeinträchtigen. Microsoft Connected Cache kann bereitgestellt werden, um das Herunterladen von Inhalten auf allen Ebenen innerhalb des mit ISA 95 kompatiblen Netzwerks zu unterstützen.

Der Schlüssel zum Konfigurieren von Microsoft Connected Cache-Bereitstellungen in einem mit ISA 95 kompatiblen Netzwerk besteht darin, den OT-Proxy *und* den Upstreamhost auf dem L3 IoT Edge-Gateway zu konfigurieren.

1. Konfigurieren von Microsoft Connected Cache-Bereitstellungen auf den Ebenen L5 und L4, wie im Beispiel „Two-Level Nested IoT Edge gateway“ (Geschaltetes IoT Edge-Gateway mit zwei Ebenen) beschrieben 
2. Bei der Bereitstellung auf dem L3 IoT Edge-Gateway muss Folgendes angegeben werden:
   
   * UPSTREAM_HOST: IP/FQDN des L4 IoT Edge-Gateways, von dem der L3 Microsoft Connected Cache Inhalt anfordert.
   * UPSTREAM_PROXY: IP/FQDN:PORT des OT-Proxyservers.

3. Der OT-Proxy muss der Zulassungsliste den L4-MCC-FQDN bzw. die IP-Adresse hinzufügen.

Um zu überprüfen, ob Microsoft Connected Cache ordnungsgemäß funktioniert, führen Sie den folgenden Befehl im Terminal des IoT Edge-Geräts, das das Modul hostet, oder eines beliebigen Geräts im Netzwerk aus.

```bash
    wget "http://<L3 IoT Edge Gateway IP>/mscomtest/wuidt.gif?cacheHostOrigin=au.download.windowsupdate.com
```