---
title: 'Azure-VMware-Lösungen (AVS): Einrichten von Workload-DNS und -DHCP für private AVS-Clouds'
description: Beschreibt das Einrichten von DNS und DHCP für Anwendungen und Workloads, die in ihrer privaten AVS-Cloudumgebung ausgeführt werden
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2a5cae868f2d8f3689f05dd9d466715ab2008a3
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024686"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-avs-private-cloud"></a>Einrichten von DNS- und DHCP-Anwendungen und -Workloads in Ihrer privaten AVS-Cloud

Anwendungen und Workloads, die in einer privaten AVS-Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste zum Nachschlagen und für die IP-Adresszuweisung. Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich. Sie können eine VM konfigurieren, um diese Dienste in Ihrer privaten AVS-Cloudumgebung bereitzustellen. 

## <a name="prerequisites"></a>Voraussetzungen

* Eine verteilte Portgruppe mit konfiguriertem VLAN
* Routeneinrichtung zu lokalen oder internetbasierten DNS-Servern
* Vorlage für virtuelle Maschinen oder ISO zum Erstellen eines virtuellen Computers

## <a name="linux-based-dns-server-setup"></a>Setup für Linux-basiertes DNS-Server

Linux bietet verschiedene Pakete für das Einrichten von DNS-Servern. Dies ist ein [Beispielsetup von DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) mit Anleitungen zu Einrichten eines Open-Source-BIND-DNS-Servers.

## <a name="windows-based-setup"></a>Windows-basiertes Setup

In diesen Microsoft-Themen wird beschrieben, wie Sie einen Windows-Server als DNS-Server und als DHCP-Server einrichten.

* [Windows-Server als DNS-Server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)
* [Windows-Server als DHCP-Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)
