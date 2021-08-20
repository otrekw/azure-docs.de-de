---
title: Beschreibung von DHCP und DNS in Azure VMware Solution
description: Beschreibung von DHCP und DNS in Azure VMware Solution.
ms.topic: include
ms.date: 05/28/2021
author: shortpatti
ms.author: v-patsho
ms.service: azure-vmware
ms.openlocfilehash: 98f2fd71cc4657a3837278c3b7a761678b8a1005
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "121750101"
---
<!-- Used in tutorial-network-checklist.md and configure-dhcp-azure-vmware-solution.md -->

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste zum Nachschlagen und für die IP-Adresszuweisung. Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich. Sie können einen virtuellen Computer konfigurieren, um diese Dienste in Ihrer privaten Cloudumgebung bereitzustellen.  

Verwenden Sie den in NSX integrierten DHCP-Dienst oder einen lokalen DHCP-Server in der privaten Cloud, anstatt DHCP-Broadcastdatenverkehr über das WAN in die lokale Umgebung zurückzuleiten.
