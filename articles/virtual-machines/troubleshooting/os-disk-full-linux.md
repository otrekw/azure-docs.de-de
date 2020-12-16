---
title: Probleme mit einem vollen Betriebssystemdatenträger auf einer Linux-VM
description: Beheben von Problemen mit einem vollen Betriebssystemdatenträger auf einer Linux-VM
author: v-miegge
ms.author: tibasham
manager: dcscontentpm
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: troubleshooting
ms.date: 11/20/2020
ms.openlocfilehash: 2e350fe297b2aaf89ac302baaefb29092cfe2532
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523307"
---
# <a name="issues-with-a-full-os-disk-on-a-linux-virtual-machine"></a>Probleme mit einem vollen Betriebssystemdatenträger auf einer Linux-VM

Wenn der Betriebssystemdatenträger einer Linux-VM (Virtual Machine, virtueller Computer) voll ist, kann dies zu Problemen beim ordnungsgemäßen Betrieb der VM führen.

## <a name="symptom"></a>Symptom

Wenn Sie versuchen, eine neue Datei zu erstellen, erhalten Sie folgende Meldung:

```
username@AZUbuntu1404:~$ touch new 
touch: cannot touch “new”: No space left on device 
username@AZUbuntu1404:~$
```

Mehrere Daemons geben dann an, dass sie während der Startsitzung keine temporären Dateien erstellen können.

```
ERROR:IOError: [Errno 28] No space left on device: '/var/lib/waagent/events/1474306860983232.tmp' 
    
OSError: [Errno 28] No space left on device: '/var/lib/cloud/data/tmpDZCq0g'
```
    
## <a name="cause"></a>Ursache

Es gibt verschiedene Gründe für das Auftreten dieser Fehlermeldung:

1. Der Datenträger ist möglicherweise voll.
1. Das Dateisystem verfügt möglicherweise nicht mehr über freie I-Knoten.
1. Ein Datenträger wurde möglicherweise über ein vorhandenes Verzeichnis eingebunden, sodass Dateien ausgeblendet sind.
1. Dateien wurden von einem Prozess geöffnet und dann gelöscht.

## <a name="solution"></a>Lösung

### <a name="process-overview"></a>Übersicht über den Prozess

1. Erstellen und Aufrufen einer Reparatur-VM.
1. Freigeben von Speicherplatz auf dem Datenträger.
1. Erstellen Sie den virtuellen Computer neu.

> [!NOTE]
> Wenn dieser Fehler auftritt, ist das Gastbetriebssystem nicht funktionsfähig. Behandeln Sie dieses Problem im Offlinemodus, um dieses Problem zu beheben.

### <a name="create-and-access-a-repair-vm"></a>Erstellen und Aufrufen einer Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie per SSH eine Verbindung mit der Reparatur-VM her.

### <a name="free-up-space-on-the-disk"></a>Freigeben von Speicherplatz auf der Festplatte

So beheben Sie das Problem:

- Ändern Sie die Größe des Datenträgers auf bis zu 1 TB, wenn er die maximale Größe von 1 TB noch nicht erreicht hat.
- Geben Sie Speicherplatz auf dem Datenträger frei.

1. Überprüfen Sie, ob der Datenträger voll ist. Wenn die Größe des Datenträgers unter 1 TB liegt, erweitern Sie ihn mit der [Azure-Befehlszeilenschnittstelle](../linux/expand-disks.md) auf maximal 1 TB.
1. Wenn der Datenträger bereits 1 TB groß ist, müssen Sie Speicherplatz auf dem Datenträger freigeben.
1. Nachdem Größenanpassung und Bereinigung abgeschlossen sind, fahren Sie mit dem erneuten Erstellen der VM fort.

### <a name="rebuild-the-vm"></a>Neuerstellen der VM

Verwenden Sie [Schritt 5 der VM-Reparaturbefehle](./repair-linux-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example), um die VM neu zu erstellen.
