---
title: Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Konfigurieren Sie die benötigte Verbindungsinfrastruktur, um SAP HANA in Azure (große Instanzen) zu verwenden.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/1/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f8fc161f83bcdb070ca4be1c1da3be84f1086c93
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111412545"
---
# <a name="sap-hana-large-instances-deployment"></a>Bereitstellung von SAP HANA (große Instanzen) 

In diesem Artikel werden die Informationen aufgeführt, die Sie zum Bereitstellen von SAP HANA (große Instanzen, auch als BareMetal-Infrastrukturinstanzen bekannt) benötigen. Informieren Sie sich zunächst über die Hintergründe:
- [Gängige Begriffe bezüglich HANA (große Instanzen)](hana-know-terms.md)
-  [SKUs für HANA (große Instanzen)](hana-available-skus.md)

## <a name="required-information"></a>Erforderliche Informationen

Sie haben SAP HANA in Azure (große Instanzen) von Microsoft erworben und möchten es bereitstellen. Microsoft benötigt die folgenden Informationen von Ihnen:

- Kundenname.
- Kontaktinformationen für geschäftliche Fragen (einschließlich E-Mail-Adresse und Telefonnummer).
- Kontaktinformationen für technische Fragen (einschließlich E-Mail-Adresse und Telefonnummer).
- Kontaktinformationen für technische Fragen in Bezug auf das Netzwerk (einschließlich E-Mail-Adresse und Telefonnummer).
- Azure-Bereitstellungsregion (z.B. „USA, Westen“, „Australien, Osten“ oder „Europa, Norden“).
- SKU für SAP HANA in Azure (große Instanzen) (Konfiguration).
- Für jede Azure-Bereitstellungsregion:
    - Ein /29-IP-Adressbereich für ER-P2P-Verbindungen, über die virtuelle Azure-Netzwerke mit HANA (große Instanzen) verbunden werden.
    - Ein /24-CIDR-Block, der für den HANA (große Instanzen)-Server-IP-Pool verwendet wird.
    - Optional bei Verwendung von [ExpressRoute Global Reach](../../../expressroute/expressroute-global-reach.md): Reservieren Sie einen weiteren /29-IP-Adressbereich. Der zusätzliche Bereich ermöglicht das direkte Routing vom lokalen Rechenzentrum zu Einheiten von HANA (große Instanzen). Der zusätzliche Bereich ermöglicht auch das Routing zwischen Einheiten von HANA (große Instanzen) in verschiedenen Azure-Regionen. Dieser bestimmte Bereich darf sich mit keinem zuvor von Ihnen definierten IP-Adressbereichen überlappen.
- Die IP-Adressbereichswerte, die im Attribut für den „Adressraum des virtuellen Netzwerks“ jedes virtuellen Azure-Netzwerks verwendet werden, das Verbindungen mit HANA (große Instanzen) herstellt.
- Daten für jedes HANA (große Instanzen)-System:
  - Der gewünschte Hostname – idealerweise mit vollqualifiziertem Domänennamen.
  - Die gewünschte IP-Adresse für die Einheit von HANA (große Instanzen) aus dem Adressbereich des Server-IP-Pools. (Die ersten 30 IP-Adressen im Adressbereich des Server-IP-Pools sind für die interne Nutzung in HANA (große Instanzen) reserviert.)
  - SAP HANA-SID-Name für die SAP HANA-Instanz (zum Erstellen der benötigten SAP HANA-bezogenen Datenträgervolumes erforderlich). Microsoft benötigt die HANA-SID für die Erstellung der Berechtigungen für „sidadm“ auf den NFS-Volumes. Diese Volumes werden an die Einheit von HANA (große Instanzen) angefügt. Außerdem wird die HANA-SID als eine der Namenskomponenten der Datenträgervolumes verwendet, die bereitgestellt werden. Wenn Sie auf der Einheit mehr als eine HANA-Instanz ausführen möchten, sollten Sie mehrere HANA-SIDs auflisten. Für jede Instanz wird jeweils ein separater Volumesatz zugewiesen.
  - Unter dem Linux-Betriebssystem verfügt der Benutzer „sidadm“ über eine Gruppen-ID. Diese ID wird benötigt, um die erforderlichen SAP HANA-bezogenen Datenträgervolumes zu erstellen. Bei einer SAP HANA-Installation wird die Gruppe „sapsys“ in der Regel mit der Gruppen-ID „1001“ erstellt. Der Benutzer „sidadm“ gehört zu dieser Gruppe.
  - Unter dem Linux-Betriebssystem verfügt der Benutzer „sidadm“ über eine Benutzer-ID. Diese ID wird benötigt, um die erforderlichen SAP HANA-bezogenen Datenträgervolumes zu erstellen. Listen Sie alle sidadm-Benutzer auf, wenn Sie auf der Einheit mehrere HANA-Instanzen ausführen. 
- Die ID des Azure-Abonnements, mit dem SAP HANA in Azure HANA (große Instanzen) direkt verbunden wird. Diese Abonnement-ID verweist auf das Azure-Abonnement, für das die Einheiten von HANA (große Instanzen) berechnet werden.

Nachdem Sie die obigen Informationen angegeben haben, wird SAP HANA in Azure (große Instanzen) von Microsoft bereitgestellt. Microsoft sendet Ihnen die Informationen, mit denen Sie Ihre virtuellen Azure-Netzwerke mit HANA (große Instanzen) verknüpfen können. Sie können auch auf die Einheiten vom Typ „HANA (große Instanzen)“ zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie folgende Anleitungen nacheinander, um eine Verbindung mit HANA (große Instanzen) herzustellen, nachdem Microsoft es bereitgestellt hat:

1. [Verbinden von Azure-VMs mit HANA (große Instanzen)](hana-connect-azure-vm-large-instances.md)
2. [Herstellen einer ExpressRoute-Verbindung zwischen einem VNET und HANA (große Instanzen)](hana-connect-vnet-express-route.md)
3. [Weitere Netzwerkanforderungen (optional)](hana-additional-network-requirements.md)
