---
title: Onboardinganforderungen für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Onboardinganforderungen für SAP HANA in Azure (große Instanzen).
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
ms.date: 05/14/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 899fd32a60f4938f42dc03b5ac836ff809c5c61f
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579275"
---
# <a name="onboarding-requirements"></a>Onboardinganforderungen

In diesem Artikel werden die Anforderungen für die Ausführung von SAP HANA in Azure (große Instanzen) aufgeführt (auch als BareMetal Infrastructure-Instanzen bekannt).

## <a name="microsoft-azure"></a>Microsoft Azure

- Ein Azure-Abonnement, das mit SAP HANA in Azure (große Instanzen) verknüpft werden kann.
- Microsoft Premier Support-Vertrag. Spezifische Informationen im Zusammenhang mit der Ausführung von SAP in Azure finden Sie im [SAP-Supporthinweis 2015553 – SAP in Microsoft Azure: Supportvoraussetzungen](https://launchpad.support.sap.com/#/notes/2015553). Wenn Sie Einheiten von HANA (große Instanz) mit 384 und mehr CPUs verwenden, muss der Premier Support-Vertrag Azure Rapid Response beinhalten.
- Kenntnis der [SKUs von HANA (große Instanz)](hana-available-skus.md), die Sie nach dem Abschluss eines [Dimensionierungsschritts](hana-sizing.md) mit SAP benötigen.

## <a name="network-connectivity"></a>Netzwerkkonnektivität

- ExpressRoute zwischen lokalen Systemen und Azure: Um Ihr lokales Rechenzentrum mit Azure zu verbinden, benötigen Sie mindestens eine 1-GBit/s-Verbindung von Ihrem Internetdienstanbieter (ISP). Für die Verbindungen zwischen HANA (große Instanzen) und Azure wird ebenfalls ExpressRoute-Technologie verwendet. Diese ExpressRoute-Verbindung zwischen HANA (große Instanzen) und Azure ist im Preis für HANA (große Instanzen) enthalten. Der Preis umfasst auch alle Gebühren für den ein- und ausgehenden Datenverkehr für diese bestimmte ExpressRoute-Leitung. Somit entstehen Ihnen keine zusätzlichen Kosten über Ihre ExpressRoute-Verbindung zwischen der lokalen Umgebung und Azure hinaus.

## <a name="operating-system"></a>Betriebssystem

- Lizenzen für SUSE Linux Enterprise Server 12 für SAP-Anwendungen.

   > [!NOTE] 
   > Das von Microsoft bereitgestellte Betriebssystem ist nicht bei SUSE registriert. Es ist nicht mit einer Subscription Management Tool-Instanz verbunden.

- Bereitstellung von SUSE Linux Subscription Management Tool in Azure auf einer VM. Dieses Tool bietet die Möglichkeit, SAP HANA in Azure (große Instanzen) zu registrieren und entsprechend von SUSE aktualisieren zu lassen. (Im Rechenzentrum für HANA (große Instanz) besteht kein Internetzugriff.) 
- Lizenzen für Red Hat Enterprise Linux 6.7 oder 7.x für SAP HANA.

   > [!NOTE]
   > Das von Microsoft bereitgestellte Betriebssystem ist nicht bei Red Hat registriert. Es ist nicht mit einer Red Hat Subscription Manager-Instanz verbunden.

- Bereitstellung von Red Hat Subscription Manager in Azure auf einer VM. Der Red Hat Subscription Manager bietet die Möglichkeit, SAP HANA in Azure (große Instanzen) zu registrieren und entsprechend von Red Hat aktualisieren zu lassen. (In dem Mandanten, der im Azure-Umfeld der großen Instanz bereitgestellt wird, besteht kein direkter Internetzugriff.)
- SAP setzt voraus, dass Sie auch einen Support-Vertrag mit Ihrem Linux-Anbieter haben. Diese Anforderung gilt auch bei der Verwendung von HANA (große Instanz) und trotz der Tatsache, dass Sie Linux in Azure ausführen. Anders als bei einigen der Images im Linux Azure-Katalog ist die Servicegebühr *nicht* im Lösungsangebot von HANA (große Instanz) enthalten. Es liegt in Ihrer Verantwortung, die Anforderungen von SAP bezüglich der Supportverträge mit dem Linux-Vertreiber zu erfüllen. 
   - Die Anforderungen bezüglich der Supportverträge für SUSE Linux finden Sie in [SAP-Hinweis 1984787 – SUSE Linux Enterprise Server 12: Installationshinweise](https://launchpad.support.sap.com/#/notes/1984787) und [SAP-Hinweis 1056161 – SUSE-Prioritätssupport für SAP-Anwendungen](https://launchpad.support.sap.com/#/notes/1056161).
   - Für Red Hat Linux benötigen Sie die richtigen Abonnementebenen, die Support und Dienstupdates für die Betriebssysteme von HANA (große Instanz) umfassen. Red Hat empfiehlt das Red Hat Enterprise Linux-Abonnement für SAP-Lösungen. Lesen Sie https://access.redhat.com/solutions/3082481. 

Die Supportmatrix der anderen SAP HANA-Versionen mit den verschiedenen Linux-Versionen finden Sie im [SAP-Hinweis 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Die Kompatibilitätsmatrix des Betriebssystems und der HLI-Firmware-/Treiberversionen finden Sie unter [Operating System Upgrade](os-upgrade-hana-large-instance.md) (Betriebssystemupgrade).


> [!IMPORTANT] 
> Derzeit wird für Einheiten vom Typ II nur die Betriebssystemversion SLES 12 SP2 unterstützt. 


## <a name="database"></a>Datenbank

- Lizenzen und Softwareinstallationskomponenten für SAP HANA (Plattform oder Enterprise Edition).

## <a name="applications"></a>Anwendungen

- Lizenzen und Softwareinstallationskomponenten für alle SAP-Anwendungen, die eine Verbindung mit SAP HANA herstellen, sowie zugehörige SAP-Supportverträge.
- Lizenzen und Softwareinstallationskomponenten für alle Nicht-SAP-Anwendungen, die mit Umgebungen für SAP HANA in Azure (große Instanzen) verwendet werden, sowie zugehörige Supportverträge.

## <a name="skills"></a>Fähigkeiten

- Erfahrung und Kenntnisse in Bezug auf IaaS und die zugehörigen Komponenten
- Erfahrung und Kenntnisse in Bezug auf die Bereitstellung von SAP-Workloads in Azure
- Für die SAP HANA-Installation zertifizierte Mitarbeiter.
- Kenntnisse der SAP-Architektur zum Entwerfen der Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA

## <a name="sap"></a>SAP

- Es wird vorausgesetzt, das Sie ein SAP-Kunde sind und über einen Supportvertrag mit SAP verfügen.
- Insbesondere für Implementierungen der Typ-II-Klasse-SKUs von HANA (große Instanz) sollten Sie SAP bezüglich der SAP HANA-Versionen und möglichen Konfigurationen auf großer, zentral hochskalierter Hardware zurate ziehen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Verwenden von SAP HANA Data Tiering und Extension Nodes.

> [!div class="nextstepaction"]
> [Verwenden von SAP HANA Data Tiering und Extension Nodes](hana-data-tiering-extension-nodes.md)
