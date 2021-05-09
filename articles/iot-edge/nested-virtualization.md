---
title: Geschachtelte Virtualisierung für Azure IoT Edge für Linux unter Windows | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die geschachtelte Virtualisierung in Azure IoT Edge für Linux unter Windows steuern.
author: fcabrera
manager: kgremban
ms.author: fcabrera
ms.date: 2/24/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
monikerRange: =iotedge-2018-06
ms.openlocfilehash: 7c50e813655603101da15931a6b3056d431f47b8
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129529"
---
# <a name="nested-virtualization-for-azure-iot-edge-for-linux-on-windows"></a>Geschachtelte Virtualisierung für Azure IoT Edge für Linux unter Windows
Es gibt zwei Formen der geschachtelten Virtualisierung, die mit Azure IoT Edge für Linux unter Windows kompatibel sind. Die Benutzer können wählen, ob die Bereitstellung über einen lokalen virtuellen Computer oder über einen virtuellen Azure-Computer erfolgen soll. Anhand dieses Artikels können sich Benutzer einen Überblick darüber verschaffen, welche Option für ihr Szenario am besten geeignet ist. Zudem erhalten sie Informationen zu den Konfigurationsanforderungen.

> [!NOTE]
>
> Stellen Sie sicher, dass Sie eine [Netzwerkoption](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#networking-options) für die geschachtelte Virtualisierung aktivieren. Wenn Sie dies nicht tun, treten bei der Installation von EFLOW Fehler auf. 

## <a name="deployment-on-local-vm"></a>Bereitstellung auf einem lokalen virtuellen Computer
Hierbei handelt es sich um einen grundlegenden Ansatz für alle virtuellen Windows-Computer, auf denen Azure IoT Edge für Linux unter Windows gehostet wird. In diesem Fall muss zunächst eine geschachtelte Virtualisierung aktiviert werden, bevor mit der Bereitstellung begonnen wird. Weitere Informationen zum Konfigurieren dieses Szenarios finden Sie unter [Ausführen von Hyper-V auf einem virtuellen Computer mit geschachtelter Virtualisierung](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).

Wenn Sie Windows Server verwenden, müssen Sie die [Hyper-V-Rolle installieren](/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server).

## <a name="deployment-on-azure-vms"></a>Bereitstellung auf virtuellen Azure-Computern
Wenn Sie sich für die Bereitstellung auf virtuellen Computern entscheiden, müssen Sie beachten, dass standardmäßig kein externer Switch vorhanden ist. Zudem ist Azure IoT Edge für Linux unter Windows auch nicht kompatibel mit einem virtuellen Azure-Computer, auf dem die Server-SKU aufgeführt wird, es sei denn, es wird ein spezielles Skript ausgeführt, mit dem ein Standardswitch bereitgestellt wird. Weitere Informationen zum Bereitstellen eines Standardswitches finden Sie weiter unten im Abschnitt [Windows Server](#windows-server). 

> [!NOTE]
>
> Ein virtueller Azure-Computer, auf dem EFLOW gehostet werden soll, muss die [geschachtelte Virtualisierung unterstützen](../virtual-machines/acu.md).


## <a name="limited-use-of-external-switch"></a>Eingeschränkte Nutzung eines externen Switches
In einem Szenario, in dem der virtuelle Computer keine IP-Adresse über einen externen Switch abrufen kann, wird von der Bereitstellungsfunktion automatisch der interne Standardswitch für die Bereitstellung verwendet. Das bedeutet, dass die Verwaltung des virtuellen Computers mit Azure IoT Edge für Linux nur auf dem Zielgerät selbst ausgeführt werden kann (d. h., mit dem virtuellen Computer mit Azure IoT Edge für Linux kann über die PowerShell SSH-Erweiterung WAC nur auf Localhost eine Verbindung hergestellt werden).

## <a name="windows-server"></a>Windows Server
Benutzer von Windows Server müssen beachten, dass der Standardswitch von Azure IoT Edge für Linux unter Windows nicht automatisch unterstützt wird.

* Folgen für den lokalen virtuellen Computer: Sie müssen sicherstellen, dass der virtuelle EFLOW-Computer eine IP-Adresse über den externen Switch abrufen kann.

* Folgen für den virtuellen Azure-Computer: Da auf virtuellen Azure-Computern kein externer Switch vorhanden ist, kann EFLOW erst bereitgestellt werden, nachdem auf dem Server ein interner Switch eingerichtet wurde.

Bei Server-SKUs ist standardmäßig kein Standardswitch vorhanden (ganz gleich, ob die Server-SKU auf einem virtuellen Azure-Computer ausgeführt wird oder nicht). Bei der Bereitstellung auf einem virtuellen Azure-Computer, auf dem der externe Switch nicht verwendet werden kann, muss vor der Bereitstellung von Azure IoT Edge für Linux unter Windows der Standardswitch manuell eingerichtet und konfiguriert werden. Das ist mit unserer Bereitstellungsfunktion möglich, da sie eine IP-Konfiguration für den internen Switch, eine NAT-Konfiguration sowie die Installation und Konfiguration eines DHCP-Servers verlangt. Für unsere Bereitstellungsfunktion in Public Preview ist angegeben, dass damit diese Einstellungen nicht verändert, sodass die Netzwerkkonfiguration bei produktiven Bereitstellungen nicht beeinträchtigt wird.

* Relevante Informationen zum manuellen Einrichten des Standardswitches finden Sie unter [Aktivieren der geschachtelten Virtualisierung auf Azure Virtual Machines](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).
* Die Dokumentation zum Einrichten eines DHCP-Servers für dieses Szenario finden Sie unter [Bereitstellen von DHCP mithilfe von Windows PowerShell](/windows-server/networking/technologies/dhcp/dhcp-deploy-wps).