---
title: Was ist Oracle WebLogic Server in Azure?
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
ms.openlocfilehash: fe247e75040f658beb94a66176f802993268a7d8
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87851856"
---
# <a name="what-is-oracle-weblogic-server-on-azure"></a>Was ist Oracle WebLogic Server in Azure?

Auf dieser Seite werden Lösungen zum Ausführen von WebLogic Server (WLS) auf virtuellen Azure-Computern beschrieben.  Diese Lösungen werden gemeinsam von Oracle und Microsoft entwickelt.

Oracle WebLogic Server ist der weltweit erste cloudbasierte Anwendungsserver für Unternehmen auf der Java-Plattform zum Entwickeln und Bereitstellen verteilter Unternehmensanwendungen mit mehreren Schichten. Die Azure WebLogic Server-Angebote ermöglichen Ihnen die Nutzung von Cloud Computing.  Sie erhalten mehr Auswahlmöglichkeiten und Flexibilität für die WebLogic-Migration, einschließlich Lift-&-Shift Ihrer Java EE-Anwendungen in die Azure-Cloud.   WLS in Azure bietet große Wirkung mit geringem Aufwand. Mit den Angeboten kommen Ihre branchenspezifischen Anwendungen (LOB) schnell in Gang.  Jedes Angebot stellt automatisch ein virtuelles Netzwerk, Speicher- und Linux-Ressourcen bereit.  WebLogic Server wird ohne den geringsten Aufwand installiert.  WLS in Azure richtet Sicherheit mit einer Netzwerksicherheitsgruppe, Lastenausgleich mit Azure App Gateway und Authentifizierung mit Azure Active Directory ein und stellt automatisch eine Verbindung mit Ihrer vorhandenen Datenbank her.

:::image type="content" source="media/oracle-weblogic/wls-on-azure.gif" alt-text="Bereitstellung von WebLogic Server in Azure über das Azure-Portal":::

Es stehen vier Angebote für verschiedene Szenarien zur Verfügung: Einzelknoten ohne Verwaltungsserver, Einzelknoten mit Verwaltungsserver, Cluster und dynamischer Cluster.  Probieren Sie die Angebote aus. Diese sind kostenlos erhältlichen.

_Für diese Angebote gilt „Bring-Your-Own-License“_ . Es wird davon ausgegangen, dass Sie bereits über die entsprechenden Lizenzen bei Oracle verfügen und mit ordnungsgemäßen Lizenzen ausgestattet sind, um Angebote in Microsoft Azure auszuführen.

_Wenn Sie mit dem Ingenieurteam, das diese Angebote entwickelt, eng an Ihren Migrationsszenarien zusammenarbeiten möchten, klicken Sie auf die Schaltfläche [KONTAKTFORMULAR](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)_ im [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview). Programm-Manager, Architekten und Techniker setzen sich dann in Kürze mit Ihnen in Verbindung und beginnen mit der Zusammenarbeit!

## <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server-Einzelknoten

Dieses Angebot stellt einen einzelnen virtuellen Computer bereit und installiert WLS darauf. Es wird weder eine Domäne erstellt noch der Verwaltungsserver gestartet. Ein Einzelknoten empfiehlt sich für Szenarien mit einer stark angepassten Domänenkonfiguration.

## <a name="oracle-weblogic-server-with-admin-server"></a>Oracle WebLogic Server mit Verwaltungsserver

Dieses Angebot stellt einen einzelnen virtuellen Computer bereit und installiert WLS darauf. Es erstellt eine Domäne und startet den Verwaltungsserver, was Ihnen die Verwaltung der Domäne ermöglicht.

## <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server-Cluster

Bei diesem Angebot wird ein hoch verfügbarer Cluster von virtuellen WLS-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet, was Ihnen die Verwaltung der Domäne ermöglicht.

## <a name="oracle-weblogic-server-dynamic-cluster"></a>Dynamischer Oracle WebLogic Server-Cluster

Bei diesem Angebot wird ein hoch verfügbarer und skalierbarer dynamischer Cluster von virtuellen WLS-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet, was Ihnen die Verwaltung der Domäne ermöglicht.

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
