---
title: 'Azure VMware Solutions (AVS): VPN-Gateways'
description: Hier erfahren Sie mehr über die AVS-Konzepte für Site-to-Site- und Point-to-Site-VPNs.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 73171e2c46bdf6c934db5777efe36ba51153a686
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024856"
---
# <a name="vpn-gateways-overview"></a>Übersicht über VPN-Gateways

Ein VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem AVS-Regionsnetzwerk und einem lokalen Standort oder einem Computer über das öffentliche Internet zu senden. Jede Region kann über ein VPN-Gateway verfügen, das mehrere Verbindungen unterstützen kann. Wenn Sie mehrere Verbindungen mit dem gleichen VPN-Gateway herstellen, wird die für das Gateway zur Verfügung stehende Bandbreite auf alle VPN-Tunnel aufgeteilt.

AVS stellt zwei Arten von VPN-Gateways bereit:

* Site-to-Site-VPN-Gateway
* Point-to-Site-VPN Gateway

## <a name="site-to-site-vpn-gateway"></a>Site-to-Site-VPN-Gateway

Ein Site-to-Site-VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem AVS-Regionsnetzwerk und einem lokalen Rechenzentrum zu senden. Verwenden Sie diese Verbindung, um die Subnetze oder den CIDR-Bereich für Netzwerkdatenverkehr zwischen Ihrem lokalen Netzwerk und dem AVS-Regionsnetzwerk zu definieren.

Das VPN-Gateway ermöglicht es Ihnen, Dienste vom lokalen Standort in Ihrer privaten AVS-Cloud und Dienste in Ihrer privaten AVS-Cloud aus dem lokalen Netzwerk zu nutzen. AVS stellt einen richtlinienbasierten VPN-Server bereit, über den Sie die Verbindung aus Ihrem lokalen Netzwerk herstellen können.

Anwendungsfälle für ein Site-to-Site-VPN:

* Zugriff auf die vCenter-Instanz Ihrer privaten AVS-Cloud von jeder Arbeitsstation in Ihrem lokalen Netzwerk aus
* Verwenden Ihres lokalen Active Directory Domain Services als vCenter-Identitätsquelle.
* Einfaches Übertragen von VM-Vorlagen, ISO-Dateien und anderen Dateien aus Ihren lokalen Ressourcen in die vCenter-Instanz Ihrer privaten AVS-Cloud
* Zugriff auf Workloads, die in Ihrer privaten AVS-Cloud ausgeführt werden, aus dem lokalen Netzwerk

![Topologie einer Site-to-Site-VPN-Verbindung](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>Kryptografische Parameter

Für eine Site-to-Site-VPN-Verbindung werden die folgenden kryptografischen Standardparameter verwendet, um eine sichere Verbindung herzustellen. Wenn Sie eine Verbindung von Ihrem lokalen VPN-Gerät aus herstellen, können Sie jeden der folgenden Parameter verwenden, die von Ihrem lokalen VPN-Gateway unterstützt werden.

#### <a name="phase-1-proposals"></a>Vorschläge in Phase 1

| Parameter | Vorschlag 1 | Vorschlag 2 | Vorschlag 3 |
|-----------|------------|------------|------------|
| IKE-Version | IKEv1 | IKEv1 | IKEv1 |
| Verschlüsselung | AES 128 | AES 256 | AES 256 |
| Hashalgorithmus| SHA 256 | SHA 256 | SHA 1 |
| Diffie-Hellman-Gruppe (DH-Gruppe) | 2 | 2 | 2 |
| Lebensdauer | 28.800 Sekunden | 28.800 Sekunden | 28.800 Sekunden |
| Datengröße | 4 GB | 4 GB | 4 GB |

#### <a name="phase-2-proposals"></a>Vorschläge in Phase 2

| Parameter | Vorschlag 1 | Vorschlag 2 | Vorschlag 3 |
|-----------|------------|------------|------------|
| Verschlüsselung | AES 128 | AES 256 | AES 256 |
| Hashalgorithmus| SHA 256 | SHA 256 | SHA 1 |
| Perfect Forward Secrecy-Gruppe (PFS-Gruppe) | Keine | Keine | Keine |
| Lebensdauer | 1\.800 Sekunden | 1\.800 Sekunden | 1\.800 Sekunden |
| Datengröße | 4 GB | 4 GB | 4 GB |


> [!IMPORTANT]
> Legen Sie TCP MSS-Clamping für das VPN-Gerät auf 1200 fest. Wenn Ihre VPN-Geräte MSS-Clamping nicht unterstützen, können Sie stattdessen auch den MTU-Wert der Tunnelschnittstelle auf 1240 Bytes festlegen.

## <a name="point-to-site-vpn-gateway"></a>Point-to-Site-VPN Gateway

Ein Point-to-Site-VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem AVS-Regionsnetzwerk und einem Clientcomputer zu senden. Ein Point-to-Site-VPN ist die einfachste Möglichkeit, auf das Netzwerk Ihrer privaten AVS-Cloud zuzugreifen, einschließlich vCenter- und Workload-VMs Ihrer privaten AVS-Cloud. Nutzen Sie die Point-to-Site-VPN-Konnektivität, wenn Sie eine Remoteverbindung mit der privaten AVS-Cloud herstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten eines VPN-Gateways](vpn-gateway.md)
