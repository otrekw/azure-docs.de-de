---
title: Neuerungen im Agent für Azure Arc-fähige Server (Vorschau)
description: Dieser Artikel enthält Versionshinweise für den Agent für Azure Arc-fähige Server (Vorschau). Für viele der zusammengefassten Issues werden Links zu weiteren Informationen bereitgestellt.
ms.topic: conceptual
ms.date: 08/31/2020
ms.openlocfilehash: 0f5060189d6f9cac620b49f414e0e27bed3e356e
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89236545"
---
# <a name="whats-new-with-azure-arc-enabled-servers-preview-agent"></a>Neuerungen im Agent für Azure Arc-fähige Server (Vorschau)

Der Connected Machine-Agent für Azure Arc-fähige Server (Vorschau) wird ständig verbessert. Damit Sie bezüglich der neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern

## <a name="august-2020"></a>August 2020

Version: 0,11

- Unterstützung für Ubuntu 20.04.

- Verbesserungen bei der Zuverlässigkeit von Erweiterungsbereitstellungen.

### <a name="known-issues"></a>Bekannte Probleme

Wenn Sie eine ältere Version des Linux-Agents verwenden und diesen für die Verwendung eines Proxyservers konfiguriert haben, müssen Sie die Proxyservereinstellung nach dem Upgrade neu konfigurieren. Führen Sie hierzu `sudo azcmagent_proxy add http://proxyserver.local:83` aus.

## <a name="next-steps"></a>Nächste Schritte

Bevor Sie Server mit Arc-Unterstützung (Vorschauversion) auf mehreren Hybridcomputern auswerten oder aktivieren, lesen Sie [Übersicht über den Connected Machine-Agent](agent-overview.md), um die Anforderungen, technischen Details zum Agent und Bereitstellungsmethoden zu verstehen.