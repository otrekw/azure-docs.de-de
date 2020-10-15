---
title: Azure VMware Solution by CloudSimple – öffentliche IP-Adresse
description: Erfahren Sie mehr über öffentliche IP-Adressen und deren Vorteile für Azure VMware Solution by CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 147ab6b5ace4493e1a0b303c320cb2e81d829b29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "77024975"
---
# <a name="cloudsimple-public-ip-address-overview"></a>Öffentliche CloudSimple-IP-Adresse – Übersicht

Eine öffentliche IP-Adresse ermöglicht Internetressourcen die eingehende Kommunikation mit Ressourcen in einer privaten Cloud über eine private IP-Adresse. Die private IP-Adresse gehört ist entweder ein virtueller Computer oder ein softwarebasiertes Lastenausgleichsmodul im vCenter Ihrer privaten Cloud. Über die öffentliche IP-Adresse können Sie Dienste, die in Ihrer privaten Cloud ausgeführt werden, für das Internet verfügbar machen.

Die öffentliche IP-Adresse ist solange der privaten IP-Adresse zugewiesen, bis Sie diese Zuweisung aufgehoben haben. Eine öffentliche IP-Adresse kann nur einer privaten IP-Adresse zugewiesen werden.

Für eine Ressource, die mit einer öffentlichen IP-Adresse verknüpft ist, wird für Internetzugriff immer die öffentliche IP-Adresse verwendet. Standardmäßig ist nur ausgehender Internetzugriff für eine öffentliche IP-Adresse zulässig.  Eingehender Datenverkehr über die öffentliche IP-Adresse wird abgelehnt.  Um eingehenden Datenverkehr zuzulassen, erstellen Sie eine Firewallregel für die öffentliche IP-Adresse am entsprechenden Port.

## <a name="benefits"></a>Vorteile

Das Verwenden einer öffentlichen IP-Adresse für die eingehende Kommunikation bietet:

* Schutz vor DDoS-Angriffen (verteilte Denial-of-Service-Angriffe). Dieser Schutz wird automatisch für die öffentliche IP-Adresse aktiviert.
* Ununterbrochene Datenverkehrsüberwachung sowie Echtzeit-Risikominderung von häufig vorkommenden Angriffen auf Netzwerkebene. Diese Schutzmaßnahmen sind mit denen identisch, die von Microsoft Online Services verwendet werden.
* Die gesamte Skalierung des globalen Azure-Netzwerks. Das Netzwerk kann verwendet werden, um den aus Angriffen resultierenden Datenverkehr über Regionen zu verteilen und zu minimieren.  

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Zuordnen einer öffentlichen IP-Adresse](public-ips.md).