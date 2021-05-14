---
title: 'Azure VMware Solution by CloudSimple: Einrichten von Workload-DNS und -DHCP für die private Cloud'
description: Beschreibt das Einrichten von DNS und DHCP für Anwendungen und Workloads, die in ihrer privaten CloudSimple-Cloudumgebung ausgeführt werden.
author: shortpatti
ms.author: v-patsho
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4b8e5cddb8616cc346cce16cc64c6770699a94c6
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108176898"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>Einrichten von DNS- und DHCP-Anwendungen und -Workloads in der privaten CloudSimple-Cloud

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste für Nachschlagen und IP-Adresszuweisung.  Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich.  Sie können einen virtuellen Computer konfigurieren, um diese Dienste in Ihrer privaten Cloudumgebung bereitzustellen.  

## <a name="prerequisites"></a>Voraussetzungen

* Eine verteilte Portgruppe mit konfiguriertem VLAN
* Routeneinrichtung zu lokalen oder internetbasierten DNS-Servern
* Vorlage für virtuelle Maschinen oder ISO zum Erstellen eines virtuellen Computers

## <a name="linux-based-dns-server-setup"></a>Setup für Linux-basiertes DNS-Server

Linux bietet verschiedene Pakete für das Einrichten von DNS-Servern.  Dies ist ein [Beispielsetup von DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) mit Anleitungen zu Einrichten eines Open-Source-BIND-DNS-Servers.

## <a name="windows-based-setup"></a>Windows-basiertes Setup

In diesen Microsoft-Themen wird beschrieben, wie Sie einen Windows-Server als DNS-Server und als DHCP-Server einrichten.

* [Windows-Server als DNS-Server](/windows-server/networking/dns/dns-top)
* [Windows-Server als DHCP-Server](/windows-server/networking/technologies/dhcp/dhcp-top)
