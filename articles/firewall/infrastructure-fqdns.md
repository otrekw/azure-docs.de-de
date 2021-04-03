---
title: Infrastruktur-FQDN für Azure Firewall
description: Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: d4de81bad1ddda2f5b54f47c1be78925ad4cdbf3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "74168983"
---
# <a name="infrastructure-fqdns"></a>Infrastruktur-FQDNs

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. 

Die folgenden Dienste sind in der integrierten Regelsammlung enthalten:

- Computezugriff auf das Speicher-PIR (Platform Image Repository, PIR)
- Speicherzugriff auf den Status verwalteter Datenträger
- Azure-Diagnose und -Protokollierung (MDS)

## <a name="overriding"></a>Überschreiben 

Diese integrierte Infrastrukturregelsammlung kann außer Kraft gesetzt werden. Erstellen Sie hierzu eine Anwendungsregelsammlung vom Typ „Alle ablehnen“, die ganz zum Schluss verarbeitet wird. Sie wird immer vor der Infrastrukturregelsammlung verarbeitet. Alles, was sich nicht in der Infrastrukturregelsammlung befindet, wird standardmäßig abgelehnt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [Azure Firewall bereitstellen und konfigurieren](tutorial-firewall-deploy-portal.md).