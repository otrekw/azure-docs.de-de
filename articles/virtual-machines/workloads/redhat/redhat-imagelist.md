---
title: Red Hat Enterprise Linux-Images in Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über Red Hat Enterprise Linux-Images in Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 32df17ffed400af80eadadbdeaafbaa1e3e1dbce
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941703"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Verfügbare RHEL-Images (Red Hat Enterprise Linux) in Azure
Azure bietet eine Vielzahl von RHEL-Images für verschiedene Anwendungsfälle.

## <a name="list-of-rhel-images"></a>Liste der RHEL-Images
Die folgende Liste enthält die in Azure verfügbaren RHEL-Images. Sofern nicht anders angegeben, sind alle Images mit LVM partitioniert und an reguläre RHEL-Repositorys angefügt (nicht EUS, nicht E4S). Die folgenden Images sind derzeit zur allgemeinen Verwendung verfügbar:

Angebot| SKU | Partitionierung | Bereitstellung | Notizen
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | RAW    | Linux-Agent |
|             | 6,8      | RAW    | Linux-Agent |
|             | 6.9      | RAW    | Linux-Agent |
|             | 6.10     | RAW    | Linux-Agent |
|             | 7-RAW    | RAW    | Linux-Agent | RHEL 7.x-Imagefamilie. <br> Standardmäßig mit regulären Repositorys verknüpft (nicht EUS).
|             | 7-LVM    | LVM    | Linux-Agent | RHEL 7.x-Imagefamilie. <br> Standardmäßig mit regulären Repositorys verknüpft (nicht EUS).
|             | 7-RAW-CI | RAW-CI | Cloud-Init  | RHEL 7.x-Imagefamilie. <br> Standardmäßig mit regulären Repositorys verknüpft (nicht EUS).
|             | 7.2      | RAW    | Linux-Agent |
|             | 7.3      | RAW    | Linux-Agent |
|             | 7.4      | RAW    | Linux-Agent | Ab April 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7,5      | RAW    | Linux-Agent | Ab Juni 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7.6      | RAW    | Linux-Agent | Ab Mai 2019 standardmäßig mit EUS-Repositorys verknüpft.
|             | 7,7      | LVM    | Linux-Agent | Standardmäßig mit EUS-Repositorys verknüpft.
|             | 8        | LVM    | Linux-Agent | RHEL 8.x-Imagefamilie
|             | 8-gen2   | LVM    | Linux-Agent | Hyper-V Generation 2 – RHEL 8.x-Imagefamilie.
RHEL-SAP      | 7.4      | LVM    | Linux-Agent | RHEL 7.4 for SAP HANA und Geschäfts-Apps. An E4S-Repositorys angefügt, Premium-Berechnung für SAP und RHEL sowie Grundgebühr für Computekapazität.
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 for SAP HANA und Geschäfts-Apps. An E4S-Repositorys angefügt, Premium-Berechnung für SAP und RHEL sowie Grundgebühr für Computekapazität.
|             | 7.6      | LVM    | Linux-Agent | RHEL 7.5 for SAP HANA und Geschäfts-Apps. An E4S-Repositorys angefügt, Premium-Berechnung für SAP und RHEL sowie Grundgebühr für Computekapazität.
|             | 7,7      | LVM    | Linux-Agent | RHEL 7.5 for SAP HANA und Geschäfts-Apps. An E4S-Repositorys angefügt, Premium-Berechnung für SAP und RHEL sowie Grundgebühr für Computekapazität.
RHEL-SAP-HANA | 6.7      | RAW    | Linux-Agent | RHEL 6.7 for SAP HANA. Veraltet zugunsten der RHEL-SAP-Images.
|             | 7.2      | LVM    | Linux-Agent | RHEL 7.2 for SAP HANA. Veraltet zugunsten der RHEL-SAP-Images.
|             | 7.3      | LVM    | Linux-Agent | RHEL 7.3 for SAP HANA. Veraltet zugunsten der RHEL-SAP-Images.
RHEL-SAP-APPS | 6,8      | RAW    | Linux-Agent | RHEL 6.8 for SAP Business Applications. Veraltet zugunsten der RHEL-SAP-Images.
|             | 7.3      | LVM    | Linux-Agent | RHEL 7.3 for SAP Business Applications. Veraltet zugunsten der RHEL-SAP-Images.
RHEL-Hochverfügbarkeit       | 7.4      | LVM    | Linux-Agent | RHEL 7.4 mit Hochverfügbarkeits-Add-On. Premium-Berechnung für HANA und RHEL zusätzlich zur Grundgebühr für Computekapazität.
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 mit Hochverfügbarkeits-Add-On. Premium-Berechnung für HANA und RHEL zusätzlich zur Grundgebühr für Computekapazität.
|             | 7.6      | LVM    | Linux-Agent | RHEL 7.6 mit Hochverfügbarkeits-Add-On. Premium-Berechnung für HANA und RHEL zusätzlich zur Grundgebühr für Computekapazität.
RHEL-SAP-HA   | 7.4      | LVM    | Linux-Agent | RHEL 7.4 für SAP mit Hochverfügbarkeits-Add-On. An E4S-Repositorys angefügt. Premium-Berechnung für SAP- und Hochverfügbarkeitsrepositorys sowie RHEL zusätzlich zur Grundgebühr für Computekapazität.
|             | 7,5      | LVM    | Linux-Agent | RHEL 7.5 für SAP mit Hochverfügbarkeits-Add-On. An E4S-Repositorys angefügt. Premium-Berechnung für SAP- und Hochverfügbarkeitsrepositorys sowie RHEL zusätzlich zur Grundgebühr für Computekapazität.
|             | 7.6      | LVM    | Linux-Agent | RHEL 7.6 für SAP mit Hochverfügbarkeits-Add-On. An E4S-Repositorys angefügt. Premium-Berechnung für SAP- und Hochverfügbarkeitsrepositorys sowie RHEL zusätzlich zur Grundgebühr für Computekapazität.
rhel-byos     |rhel-lvm74| LVM    | Linux-Agent | Bei RHEL 7.4 BYOS-Images, die nicht an eine Quelle für Updates angefügt sind, erfolgt keine Premium-Berechnung für RHEL.
|             |rhel-lvm75| LVM    | Linux-Agent | Bei RHEL 7.5 BYOS-Images, die nicht an eine Quelle für Updates angefügt sind, erfolgt keine Premium-Berechnung für RHEL.
|             |rhel-lvm76| LVM    | Linux-Agent | Bei RHEL 7.6 BYOS-Images, die nicht an eine Quelle für Updates angefügt sind, erfolgt keine Premium-Berechnung für RHEL.
|             |rhel-lvm77| LVM    | Linux-Agent | Bei RHEL 7.7 BYOS-Images, die nicht an eine Quelle für Updates angefügt sind, erfolgt keine Premium-Berechnung für RHEL.
|             |rhel-lvm8 | LVM    | Linux-Agent | Bei RHEL 8-BYOS-Images (die RHEL-Nebenversion wird im Wert der Imageversion angezeigt), die nicht an eine Quelle für Updates angefügt sind, erfolgt keine Premium-Berechnung für RHEL.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Red Hat-Images in Azure](./redhat-images.md).
* Erfahren Sie mehr über die [Red Hat-Updateinfrastruktur](./redhat-rhui.md).
* Erfahren Sie mehr über das [RHEL-BYOS-Angebot](./byos.md).
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).
