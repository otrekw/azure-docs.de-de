---
title: Remotedesktopverbindung mit Azure Virtual Machines aufgrund einer statischen IP-Adresse nicht möglich | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Problembehandlung für ein RDP-Problem durchführen, das in Microsoft Azure aufgrund einer statischen IP-Adresse verursacht wird. | Microsoft-Dokumentation
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/08/2018
ms.author: genli
ms.openlocfilehash: 49f3f44c7de8c700d0093c5eb6f166a1dffb34a4
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087247"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>Remotedesktopverbindung mit Azure Virtual Machines aufgrund einer statischen IP-Adresse nicht möglich

In diesem Artikel wird ein Problem beschrieben, bei dem Sie keine Remotedesktopverbindung mit Azure Windows Virtual Machines (VMs) herstellen können, nachdem auf der VM eine statische IP-Adresse konfiguriert wurde.


## <a name="symptoms"></a>Symptome

Wenn Sie eine RDP-Verbindung mit einer VM in Azure herstellen, erhalten Sie die folgende Fehlermeldung:

**Remotedesktop kann aus einem der folgenden Gründe keine Verbindung mit dem Remotecomputer herstellen:**

1. **Der Remotezugriff auf den Server ist nicht aktiviert.**

2. **Der Remotecomputer ist ausgeschaltet.**

3. **Der Remotecomputer ist im Netzwerk nicht verfügbar.**

**Stellen Sie sicher, dass der Remotecomputer eingeschaltet und mit dem Netzwerk verbunden ist und dass der Remotezugriff aktiviert ist.**

Im Screenshot im Azure-Portal unter [Startdiagnose](../troubleshooting/boot-diagnostics.md) ist zu erkennen, dass die VM normal gestartet und auf dem Anmeldebildschirm auf die Anmeldeinformationen gewartet wird.

## <a name="cause"></a>Ursache

Die VM verfügt über eine statische IP-Adresse, die auf der Netzwerkschnittstelle in Windows definiert wird. Diese IP-Adresse unterscheidet sich von der Adresse, die im Azure-Portal definiert ist.

## <a name="solution"></a>Lösung

Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des betroffenen virtuellen Computers als Sicherung, bevor Sie die unten angegebenen Schritte ausführen. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Datenträgers](../windows/snapshot-copy-managed-disk.md).

Verwenden Sie zum Beheben dieses Problems die serielle Konsole, um DHCP zu aktivieren oder die [Netzwerkschnittstelle für die VM zurückzusetzen](reset-network-interface.md).

### <a name="use-serial-control"></a>Verwenden der seriellen Konsole

1. Stellen Sie eine Verbindung mit der [seriellen Konsole her, und öffnen Sie eine CMD-Instanz](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Wenn die serielle Konsole auf Ihrer VM nicht aktiviert ist, helfen Ihnen die Informationen unter [Zurücksetzen der Netzwerkschnittstelle](reset-network-interface.md) weiter.
2. Überprüfen Sie, ob DHCP für die Netzwerkschnittstelle deaktiviert ist:

    ```console
    netsh interface ip show config
    ```

3. Wenn DHCP deaktiviert ist, sollten Sie die Konfiguration Ihrer Netzwerkschnittstelle so ändern, dass DHCP verwendet wird:

    ```console
    netsh interface ip set address name="<NIC Name>" source=dhc
    ```

    Führen Sie beispielsweise den folgenden Befehl aus, wenn die Netzwerkschnittstelle den Namen „Ethernet 2“ hat:

    ```console
    netsh interface ip set address name="Ethernet 2" source=dhc
    ```

4. Fragen Sie die IP-Konfiguration erneut ab, um sicherzustellen, dass die Netzwerkschnittstelle jetzt richtig eingerichtet ist. Die neue IP-Adresse sollte mit der IP-Adresse übereinstimmen, die von Azure bereitgestellt wird.

    ```console
    netsh interface ip show config
    ```

    Sie müssen die VM an diesem Punkt nicht neu starten. Die VM ist jetzt wieder erreichbar.

Falls Sie jetzt die statische IP-Adresse für die VM konfigurieren möchten, helfen Ihnen die Informationen unter [Konfigurieren von privaten IP-Adressen für einen virtuellen Computer über das Azure-Portal](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md) weiter.