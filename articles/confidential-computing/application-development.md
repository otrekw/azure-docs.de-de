---
title: Entwicklungstools für Azure Confidential Computing
description: Verwendung von Tools und Bibliotheken zum Entwickeln von Anwendungen für Confidential Computing
services: virtual-machines
author: JBCook
ms.service: virtual-machines
ms.subservice: confidential-computing
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: JenCook
ms.openlocfilehash: 0ba6ee92111da66a2118ba4c490b94e5bc9449e0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102551384"
---
# <a name="application-development-on-intel-sgx"></a>Anwendungsentwicklung unter Intel SGX 


Die Infrastruktur für Confidential Computing erfordert spezielle Tools und Software. Auf dieser Seite werden Konzepte im Zusammenhang mit der Anwendungsentwicklung für virtuelle Azure Confidential Computing-Computer erläutert, die unter Intel SGX ausgeführt werden. [Lesen Sie die Einführung in virtuelle Intel SGX-Computer und -Enklaven](confidential-computing-enclaves.md), bevor Sie diese Seite lesen. 

Für die Nutzung der Leistungsfähigkeit von Enklaven und isolierten Umgebungen müssen Sie Tools verwenden, die Confidential Computing unterstützen. Es gibt unterschiedliche Tools, die die Entwicklung von Enklavenanwendungen unterstützen. Beispielsweise können Sie die folgenden Open-Source-Frameworks verwenden: 

- [Das Open Enclave Software Development Kit (OE SDK)](#oe-sdk)
- [Confidential Consortium Framework (CCF)](#ccf)

## <a name="overview"></a>Übersicht

Eine für Enklaven erstellte Anwendung wird auf zwei Arten partitioniert:

1. In eine „nicht vertrauenswürdige“ Komponente (Host)
1. In eine „vertrauenswürdige“ Komponente (Enklave)


![App-Entwicklung](media/application-development/oe-sdk.png)


**Der Host** ist eine nicht vertrauenswürdige Umgebung, in der Ihre Enklavenanwendung ausgeführt wird. Auf den auf dem Host bereitgestellten Enklavencode kann der Host nicht zugreifen. 

**Die Enklave** ist der Ort, an dem der Anwendungscode und seine zwischengespeicherten Daten bzw. der Arbeitsspeicher ausgeführt werden. Rechenvorgänge mit hohen Sicherheitsanforderungen sollten in der Enklave durchgeführt werden, um dafür zu sorgen, dass Geheimnisse und vertrauliche Daten geschützt bleiben. 


Während des Anwendungsentwurfs ist es wichtig, zu identifizieren und zu bestimmen, welcher Teil der Anwendung in den Enklaven ausgeführt werden muss. Der Code, den Sie in der vertrauenswürdigen Komponente anordnen, wird vom restlichen Teil Ihrer Anwendung isoliert. Nachdem die Enklave initialisiert und der Code in den Arbeitsspeicher geladen wurde, kann dieser Code nicht mehr von den nicht vertrauenswürdigen Komponenten gelesen oder geändert werden. 

## <a name="open-enclave-software-development-kit-oe-sdk"></a>Open Enclave Software Development Kit (OE SDK) <a id="oe-sdk"></a>

Verwenden Sie eine Bibliothek oder ein Framework, die bzw. das von Ihrem Anbieter unterstützt wird, falls Sie Code für die Ausführung in einer Enklave schreiben möchten. Das [Open Enclave SDK](https://github.com/openenclave/openenclave) (OE SDK) ist ein Open-Source-SDK, das die Abstraktion über unterschiedliche Confidential Computing-fähige Hardware ermöglicht. 

Das OE SDK ist so konzipiert, dass es als einzelne Abstraktionsschicht für beliebige Hardware und CSPs genutzt wird. Das OE SDK kann zusätzlich zu virtuellen Azure Confidential Computing-Computern verwendet werden, um Anwendungen basierend auf Enklaven zu erstellen und auszuführen.

## <a name="confidential-consortium-framework-ccf"></a>Confidential Consortium Framework (CCF) <a id="ccf"></a>

Das [CCF](https://github.com/Microsoft/CCF) ist ein verteiltes Netzwerk von Knoten, die jeweils eigene Enklaven ausführen. Mit dem Netzwerk vertrauenswürdiger Knoten können Sie einen Distributed Ledger ausführen. Der Ledger stellt sichere, zuverlässige Komponenten für das zu verwendende Protokoll bereit. 

![CCF-Knoten](media/application-development/ccf.png)

Dieses Open-Source-Framework ermöglicht eine differenzierte Vertraulichkeit mit hohem Durchsatz und Konsortium-Governance für Blockchain. Mit jedem Knoten, der TEEs verwendet, können Sie für einen sicheren Konsens und eine sichere Transaktionsverarbeitung sorgen.


## <a name="next-steps"></a>Nächste Schritte 
- [Bereitstellen eines virtuellen Confidential Computing-Computers der DCsv2-Serie](quick-create-portal.md)
- [Herunterladen und Installieren des OE SDK und Beginnen mit der Entwicklung von Anwendungen](https://github.com/openenclave/openenclave)