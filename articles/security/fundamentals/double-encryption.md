---
title: Doppelte Verschlüsselung in Microsoft Azure
description: In diesem Artikel wird beschrieben, wie Microsoft Azure doppelte Verschlüsselung für ruhende Daten und Daten während der Übertragung bereitstellt.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2020
ms.author: terrylan
ms.openlocfilehash: 020e8249f57ccb1a14da798a717a00dcc3962389
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88226858"
---
# <a name="double-encryption"></a>Doppelte Verschlüsselung
Bei der doppelten Verschlüsselung werden zwei oder mehr unabhängige Verschlüsselungsebenen aktiviert, um vor der Kompromittierung einer der Verschlüsselungsebenen zu schützen. Durch die Verwendung von zwei Verschlüsselungsebenen werden Bedrohungen verringert, die sich beim Verschlüsseln von Daten ergeben. Beispiel:

- Konfigurationsfehler bei der Datenverschlüsselung
- Implementierungsfehler im Verschlüsselungsalgorithmus
- Kompromittierung eines einzelnen Verschlüsselungsschlüssels

Azure bietet doppelte Verschlüsselung für ruhende Daten und Daten während der Übertragung.

## <a name="data-at-rest"></a>Ruhende Daten
Der Ansatz von Microsoft zum Aktivieren zweier Verschlüsselungsebenen für ruhende Daten ist:

- **Datenträgerverschlüsselung mithilfe kundenseitig verwalteter Schlüssel**. Sie stellen Ihren eigenen Schlüssel für die Datenträgerverschlüsselung bereit. Sie können Ihre eigenen Schlüssel in Ihren Key Vault mitbringen (BYOK: Bring Your Own Key) oder neue Schlüssel in Azure Key Vault generieren, um die gewünschten Ressourcen zu verschlüsseln.
- **Infrastrukturverschlüsselung mithilfe plattformseitig verwalteter Schlüssel**.  Standardmäßig werden Datenträger automatisch im Ruhezustand mit plattformseitig verwalteten Verschlüsselungsschlüsseln verschlüsselt.

## <a name="data-in-transit"></a>Daten während der Übertragung
Der Ansatz von Microsoft zum Aktivieren zweier Verschlüsselungsebenen für Daten während der Übertragung ist:

- **Übertragungsverschlüsselung mithilfe von TLS 1.2 (Transport Layer Security) zum Schutz von Daten bei ihrer Übertragung zwischen den Clouddiensten und Ihnen**. Der gesamte Datenverkehr, der ein Rechenzentrum verlässt, wird während der Übertragung verschlüsselt, auch wenn das Ziel des Datenverkehrs ein anderer Domänencontroller in derselben Region ist. TLS 1.2 ist das Standardsicherheitsprotokoll, das verwendet wird. TLS bietet strenge Authentifizierung, Datenschutz von Nachrichten und Integrität (ermöglicht die Erkennung von Manipulation, Abfangen und Fälschung von Nachrichten), Interoperabilität, Algorithmusflexibilität sowie einfache Bereitstellung und Verwendung.
- **Zusätzliche Verschlüsselungsebene, die auf der Infrastrukturebene bereitgestellt wird**. Es wird eine Verschlüsselungsmethode für die Daten-Netzzugangsschicht mit dem Standard IEEE 802.1AE MAC Security (auch MACsec genannt) von Punkt zu Punkt auf der zugrunde liegenden Netzwerkhardware angewendet. Immer wenn Datenverkehr von Azure-Kunden zwischen Rechenzentren fließt (außerhalb von physischen Grenzen, die nicht von (oder im Auftrag von) Microsoft kontrolliert werden), werden die Pakete auf den Geräten vor dem Senden verschlüsselt und entschlüsselt. Dadurch werden physische Man-in-the-Middle-Angriffe und Spionage-/Abhörangriffe verhindert. Da diese Technologie in die Netzwerkhardware selbst integriert ist, bietet sie Verschlüsselung auf der Netzwerkhardware mit der Leitungsrate ohne eine messbar höhere Verbindungslatenz. Diese MACsec-Verschlüsselung ist standardmäßig für den gesamten Azure-Datenverkehr aktiviert, der innerhalb einer Region oder zwischen Regionen fließt, und der Kunde muss keine Aktion zum Aktivieren ausführen.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie [Verschlüsselung in Azure verwendet wird](encryption-overview.md).
