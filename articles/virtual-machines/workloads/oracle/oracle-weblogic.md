---
title: 'Oracle WebLogic Server: Azure-Anwendungen | Microsoft-Dokumentation'
description: Erfahren Sie, wie Oracle WebLogic Server in Microsoft Azure ausgeführt wird.
services: virtual-machines-linux
documentationcenter: ''
author: edburns
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2020
ms.author: edburns
ms.openlocfilehash: 9ba4e3a66ae8ef71b39aa26fd54048381237c2fa
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83664048"
---
# <a name="oracle-weblogic-server-azure-applications"></a>Oracle WebLogic Server: Azure-Anwendungen

## <a name="oracle-weblogic-server-is-a-scalable-enterprise-ready-java-ee-application-server"></a>Oracle WebLogic Server ist ein skalierbarer Java EE-Anwendungsserver für den Unternehmenseinsatz.

Oracle WebLogic Server ist der weltweit erste cloudbasierte Anwendungsserver für Unternehmen auf der Java-Plattform zum Entwickeln und Bereitstellen verteilter Unternehmensanwendungen mit mehreren Schichten. Die Azure WebLogic Server-Angebote ermöglichen Ihnen die Nutzung von Cloud Computing durch mehr Wahlmöglichkeiten und Flexibilität für die WebLogic-Migration, einschließlich Lift-&-Shift Ihrer Java EE-Anwendungen in die Azure Cloud mit geringstmöglichem Aufwand und bestem Ergebnis. Mit den Angeboten kommen Ihre Geschäftsanwendungen schnell in Gang, denn virtuelles Netzwerk, Speicher- und Linux-Ressourcen werden automatisch bereitgestellt, WebLogic Server installiert, außerdem Sicherheit mit einer Netzwerksicherheitsgruppe, Lastenausgleich mit Azure-App Gateway, Authentifizierung mit Azure Active Directory eingerichtet, und schließlich werden Datenbankverbindungen erleichtert.

Es stehen vier Angebote für verschiedene Szenarien zur Verfügung: Einzelknoten ohne Verwaltungsserver, Einzelknoten mit Verwaltungsserver, Cluster und dynamischer Cluster.  Zögern Sie nicht, sie auszuprobieren, denn die Angebote sind kostenlos verfügbar.

_Für diese Angebote gilt „Bring-Your-Own-License“_ . Es wird davon ausgegangen, dass Sie die entsprechenden Lizenzen bereits bei Oracle erworben haben und mit ordnungsgemäßen Lizenzen ausgestattet sind, um Angebote in Microsoft Azure auszuführen.

_Wenn Sie mit dem Ingenieurteam, das diese Angebote entwickelt, eng an Ihren Migrationsszenarien zusammenarbeiten möchten, klicken Sie einfach auf die Schaltfläche [KONTAKTFORMULAR](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)_ im [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview). Programm-Manager, Architekten und Techniker setzen sich dann in Kürze mit Ihnen in Verbindung und leiten die Zusammenarbeit ein!

### <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server-Einzelknoten

Dieses Angebot stellt einen einzelnen virtuellen Computer bereit und installiert Oracle WebLogic Server auf ihm. Es wird weder eine Domäne erstellt noch der Verwaltungsserver gestartet. Dies empfiehlt sich für Szenarien mit einer stark angepassten Domänenkonfiguration.

### <a name="oracle-weblogic-server-with-admin-server"></a>Oracle WebLogic Server mit Verwaltungsserver

Dieses Angebot stellt einen einzelnen virtuellen Computer bereit und installiert Oracle WebLogic Server auf ihm. Es erstellt eine Domäne und startet den Verwaltungsserver, was Ihnen die Verwaltung der Domäne ermöglicht.

### <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server-Cluster

Bei diesem Angebot wird ein hoch verfügbarer Cluster aus virtuellen Oracle WebLogic Server-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet, was Ihnen die Verwaltung der Domäne ermöglicht.

### <a name="oracle-weblogic-server-dynamic-cluster"></a>Dynamischer Oracle WebLogic Server-Cluster

Bei diesem Angebot wird ein hoch verfügbarer und skalierbarer dynamischer Cluster aus virtuellen Oracle WebLogic Server-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet, was Ihnen die Verwaltung der Domäne ermöglicht.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Angebote im Azure Marketplace.

> [!div class="nextstepaction"]
> [Oracle WebLogic Server-Einzelknoten](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server mit Verwaltungsserver](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server-Cluster](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Dynamischer Oracle WebLogic Server-Cluster](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)
