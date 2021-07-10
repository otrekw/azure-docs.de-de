---
title: Beschreibung von DHCP und DNS in Azure VMware Solution
description: Beschreibung von DHCP und DNS in Azure VMware Solution.
ms.topic: include
ms.date: 05/28/2021
ms.openlocfilehash: 585f195c36dd02e28cb744142080bee7dab91f6a
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110788080"
---
<!-- Used in tutorial-network-checklist.md and configure-dhcp-azure-vmware-solution.md -->

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste zum Nachschlagen und für die IP-Adresszuweisung. Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich. Sie können einen virtuellen Computer konfigurieren, um diese Dienste in Ihrer privaten Cloudumgebung bereitzustellen.  

Verwenden Sie den in NSX integrierten DHCP-Dienst oder einen lokalen DHCP-Server in der privaten Cloud, anstatt DHCP-Broadcastdatenverkehr über das WAN in die lokale Umgebung zurückzuleiten.
