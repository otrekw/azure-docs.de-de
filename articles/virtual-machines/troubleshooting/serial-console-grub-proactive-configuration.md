---
title: Proaktive GRUB-Konfiguration der seriellen Azure-Konsole | Microsoft-Dokumentation
description: Konfigurieren Sie GRUB übergreifend in verschiedenen Distributionen, um den Zugriff von Einzelbenutzern und den Zugriff im Wiederherstellungsmodus auf virtuellen Azure-Computern zu ermöglichen.
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/10/2019
ms.author: mimckitt
ms.openlocfilehash: 573bd0797e63fc512e59b0e0882c718e4569111c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262892"
---
# <a name="proactively-ensuring-you-have-access-to-grub-and-sysrq-could-save-you-lots-of-down-time"></a>Wenn Sie proaktiv sicherstellen, dass Sie Zugriff auf GRUB und SysRq haben, kann Ihnen das viele Ausfallzeiten ersparen

Der Zugang zur seriellen Konsole und zu GRUB verbessert in den meisten Fällen die Wiederherstellungszeiten Ihres virtuellen Linux-IaaS-Computers. GRUB bietet Wiederherstellungsoptionen für Ihre VM, die andernfalls mehr Zeit beanspruchen würden. 


Es gibt viele Gründe, eine VM-Wiederherstellung durchzuführen, und oftmals können sie Szenarien wie den folgenden zugeschrieben werden:

   - Beschädigte Dateisysteme/Kernel/MBR (Master Boot Record)
   - Fehler bei Upgrades des Kernels
   - Falsche GRUB-Kernelparameter
   - Falsche fstab-Konfigurationen
   - Firewallkonfigurationen
   - Verlorenes Kennwort
   - Beschädigte sshd-Konfigurationsdateien
   - Netzwerkkonfigurationen

 Viele weitere Szenarien, wie [hier](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux#common-scenarios-for-accessing-the-serial-console) ausführlich beschrieben

Stellen Sie sicher, dass Sie auf GRUB-und die serielle Konsole auf ihren in Azure bereitgestellten virtuellen Computern zugreifen können. 

Wenn Sie noch nicht mit der seriellen Konsole vertraut sind, klicken Sie auf [diesen Link](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux/).

> [!TIP]
> Achten Sie darauf, Sicherungskopien von Dateien zu erstellen, bevor Sie Änderungen vornehmen.

Sehen Sie das Video unten an, um zu erfahren, wie Sie Ihre Linux-VM schnell wiederherstellen können, sobald Sie Zugriff auf GRUB haben.

[Video zum Wiederherstellen von Linux-VMs](https://youtu.be/KevOc3d_SG4)

Es gibt eine Reihe von Methoden, mit denen die Wiederherstellung von Linux-VMs unterstützt werden kann. In Cloudumgebungen war dieser Vorgang bisher schwierig.
Die Entwicklung von Tools und Funktionen, die sicherstellen, dass Dienste schnell wiederhergestellt werden können, schreitet ständig fort.

Mit der seriellen Azure-Konsole können Sie mit Ihrer Linux-VM interagieren, als säßen Sie an der Systemkonsole.

Sie können viele Konfigurationsdateien bearbeiten, auch Dateien, die das Verhalten des Kernels beim Systemstart beeinflussen. 

Erfahrenere Linux/Unix-Systemadministratoren werden den **Einzelbenutzer**- und den **Notfallmodus** zu schätzen wissen, auf die über die serielle Azure-Konsole zugegriffen werden kann, was den Datenträgeraustausch und die Löschung von VMs in vielen Wiederherstellungsszenarien erübrigt.

Die Wiederherstellungsmethode hängt vom aufgetretenen Problem ab, beispielsweise kann ein verlorenes oder verlegtes Kennwort über die Option > **Kennwort zurücksetzen** im Azure-Portal zurückgesetzt werden. Die Funktion **Kennwort zurücksetzen** ist als Erweiterung bekannt und kommuniziert mit dem Linux-Gast-Agent.

Es stehen andere Erweiterungen zur Verfügung, wie etwa Custom Script, jedoch muss für diese Optionen der Linux-**waagent** ausgeführt werden und in fehlerfreiem Zustand sein, was nicht immer der Fall ist.

![Agentstatus](./media/virtual-machines-serial-console/agent-status.png)


Das Sicherstellen des Zugriffs auf die serielle Azure-Konsole und GRUB bedeutet, dass eine Kennwortänderung oder eine fehlerhafte Konfiguration in Minuten statt in Stunden berichtigt werden können. Sie können in einem Szenario, in dem der primäre Kernel beschädigt ist, in der VM sogar das Starten eines alternativen Kernels erzwingen, sollten Sie über mehrere Kernel auf dem Datenträger verfügen.

![Mehrere Kernel](./media/virtual-machines-serial-console/more-kernel.png)

## <a name="suggested-order-of-recovery-methods"></a>Empfohlene Reihenfolge der Wiederherstellungsmethoden:

- Serielle Azure-Konsole

- Austausch des Datenträgers – kann durch folgende Verfahren automatisiert werden:

   - [PowerShell-Wiederherstellungsskripts](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
   - [Bash-Wiederherstellungsskripts](https://github.com/sribs/azure-support-scripts)

- Legacyverfahren

## <a name="disk-swap-video"></a>Video zum Austausch eines Datenträgers:

Wenn Sie keinen Zugriff auf GRUB haben, sehen Sie sich [dieses](https://youtu.be/m5t0GZ5oGAc) Video an, und sehen Sie, wie Sie den Austausch des Datenträgers einfach automatisieren können, um Ihre VM wiederherzustellen.

## <a name="challenges"></a>Herausforderungen:

Nicht alle virtuellen Azure-Linux-Computer sind standardmäßig für den GRUB-Zugriff konfiguriert, noch sind sie alle für Interrupts über den Befehl SysRq konfiguriert. Einige ältere Distributionen wie SLES 11 sind nicht für die Anzeige der Anmeldeaufforderung in der seriellen Azure-Konsole konfiguriert.

In diesem Artikel behandeln wir verschiedene Linux-Distributionen und dokumentieren Konfigurationen, mit deren Hilfe GRUB verfügbar gemacht wird.




## <a name="how-to-configure-linux-vm-to-accept-sysrq-keys"></a>Konfigurieren der Linux-VM für die Annahme von SysRq-Tasten
Die SysRq-Taste ist in einigen neueren Linux-Distributionen standardmäßig aktiviert, in anderen aber möglicherweise nur für die Annahme von Werten für bestimmte SysRq-Funktionen konfiguriert.
Bei älteren Distributionen ist sie möglicherweise vollständig deaktiviert.

Die SysRq-Funktion ist zum erneuten Starten einer abgestürzten oder nicht reagierenden VM direkt an der seriellen Azure-Konsole nützlich, aber ebenso nützlich beim Erlangen von Zugriff auf das GRUB-Menü. Beim alternativen Neustart einer VM über ein anderes Portalfenster oder eine SSH-Sitzung kann die Verbindung mit Ihrer aktuellen Konsole verloren gehen, sodass GRUB-Timeouts ablaufen, die zum Anzeigen des GRUB-Menüs erforderlich sind.
Die VM muss dafür konfiguriert werden, den Wert 1 als Kernelparameter zu akzeptieren, wodurch alle Funktionen von SysRq oder 128 aktiviert werden, was Neustart/Herunterfahren ermöglicht


[Video zum Aktivieren von SysRq](https://youtu.be/0doqFRrHz_Mc)


Um die VM für das Akzeptieren eines Neustarts über SysRq-Befehle im Azure-Portal zu konfigurieren, müssen Sie für den Kernelparameter „kernel.sysrq“ den Wert 1 festlegen.

Damit diese Konfiguration auch nach einem Neustart erhalten bleibt, fügen Sie der Datei **sysctl.conf** einen Eintrag hinzu.

`echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Dynamisches Konfigurieren des Kernelparameters

`sysctl -w kernel.sysrq=1`

Wenn Sie nicht über **Stammzugriff** verfügen oder sudo defekt ist, ist es nicht möglich, SysRq an der Shell-Eingabeaufforderung zu konfigurieren.

In diesem Szenario können Sie SysRq mithilfe des Azure-Portals aktivieren. Diese Methode kann hilfreich sein, wenn die **sudoers.d/waagent**-Datei beschädigt oder gelöscht wurde.

Für die Verwendung der Funktion „Vorgänge“ > „Befehl ausführen“ > „ShellScript ausführen“ im Azure-Portal muss die waagent-Prozessdatei fehlerfrei sein. In diesem Fall können Sie diesen Befehl einfügen, um SysRq zu aktivieren:

`sysctl -w kernel.sysrq=1 ; echo kernel.sysrq = 1 >> /etc/sysctl.conf`

Wie hier dargestellt: ![Aktivieren von sysrq 2](./media/virtual-machines-serial-console/enabling-sysrq-2.png)

Nachdem dieser Vorgang abgeschlossen ist, können Sie versuchen, auf **SysRq** zuzugreifen und sollten dabei feststellen, dass ein Neustart möglich ist.

![Aktivieren von Sysrq 3](./media/virtual-machines-serial-console/enabling-sysrq-3.png)

Wählen Sie **Neustart** und den Befehl **SysRq senden** aus.

![Aktivieren von Sysrq 4](./media/virtual-machines-serial-console/enabling-sysrq-4.png)

Das System sollte eine Rücksetzungsnachricht wie diese protokollieren:

![Aktivieren von Sysrq 5](./media/virtual-machines-serial-console/enabling-sysrq-5.png)


## <a name="ubuntu-grub-configuration"></a>Ubuntu GRUB-Konfiguration

Standardmäßig sollten Sie auf GRUB zugreifen können, indem Sie während des Starts der VM die **ESC**-Taste gedrückt halten. Wenn das GRUB-Menü nicht angezeigt wird, können Sie in der seriellen Azure-Konsole die dauernde Anzeige des GRUB-Menüs mithilfe einer dieser Optionen erzwingen.

**Option 1**: Erzwingt die Anzeige von GRUB auf dem Bildschirm. 

Aktualisieren Sie die Datei „/etc/default/grub.d/50-cloudimg-settings.cfg“, um das GRUB-Menü für den angegebenen TIMEOUT auf dem Bildschirm zu halten.
Sie brauchen **ESC** nicht zu drücken, da GRUB unmittelbar angezeigt wird.

```
GRUB_TIMEOUT=0

change to

GRUB_TIMEOUT=5
```

**Option 2**: Ermöglicht das Drücken von **ESC-** vor dem Starten.

Ein ähnliches Verhalten kann durch Änderungen an der Datei „/etc/default/grub“ und Einhalten eines Timeouts von 3 Sekunden zum Drücken von **ESC** erreicht werden.


Kommentieren Sie diese zwei Zeilen aus:

```
#GRUB_HIDDEN_TIMEOUT=0
#GRUB_HIDDEN_TIMEOUT_QUIET=true
```
Fügen Sie diese Zeile hinzu:

```
GRUB_TIMEOUT_STYLE=countdown
```


## <a name="ubuntu-1204"></a>Ubuntu 12\.04

Ubuntu 12.04 erlaubt den Zugriff auf die serielle Konsole, bietet aber keine Interaktionsmöglichkeit. Es wird keine Aufforderung zur **Anmeldung:** angezeigt.


So erhalten Sie in 12.04 eine **Anmeldeaufforderung**:
1. Erstellen Sie eine Datei mit dem Namen „/etc/init/ttyS0.conf“, die folgenden Text enthält:

    ```
    # ttyS0 - getty
    #
    # This service maintains a getty on ttyS0 from the point the system is
    # started until it is shut down again.
    start on stopped rc RUNLEVEL=[12345]
    stop on runlevel [!12345]
    
    respawn
    exec /sbin/getty -L 115200 ttyS0 vt102
    ```    

2. Fordern Sie upstart auf, getty abzurufen:     
    ```
    sudo start ttyS0
    ```
 
Die erforderlichen Einstellungen zum Konfigurieren der seriellen Konsole für Ubuntu-Versionen finden Sie [hier](https://help.ubuntu.com/community/SerialConsoleHowto)

## <a name="ubuntu-recovery-mode"></a>Ubuntu-Wiederherstellungsmodus

Für Ubuntu stehen über GRUB zusätzliche Wiederherstellungs- und Bereinigungsoptionen zur Verfügung, jedoch ist der Zugriff auf diese Einstellungen nur möglich, wenn die Kernelparameter entsprechend konfiguriert sind.
Wenn diese Kernel-Startparameter nicht konfiguriert werden, würde ein Senden des Wiederherstellungsmenüs an Azure Diagnostics statt an die serielle Azure-Konsole erzwungen.
Sie können mit diesen Schritten Zugriff auf das Wiederherstellungsmenü von Ubuntu erhalten:

Unterbrechen Sie den Startvorgangs, und greifen Sie auf das GRUB-Menü zu

Wählen Sie „Erweiterte Optionen für Ubuntu“ aus, und drücken Sie die EINGABETASTE.

![ubunturec1](./media/virtual-machines-serial-console/ubunturec1.png)

Wählen Sie die Zeile aus, die *(Wiederherstellungsmodus)* enthält. Drücken Sie nicht EINGABE, sondern E.

![ubunturec2](./media/virtual-machines-serial-console/ubunturec2.png)

Suchen Sie die Zeile, die den Kernel lädt, und ersetzen Sie den letzten Parameter **nomodeset** durch das Ziel **console=ttyS0**

```
linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery nomodeset

change to

linux /boot/vmlinuz-4.15.0-1023-azure root=UUID=21b294f1-25bd-4265-9c4e-d6e4aeb57e97 ro recovery console=ttyS0
```

![ubunturec3](./media/virtual-machines-serial-console/ubunturec3.png)

Drücken Sie **STRG-x**, um den Kernel zu starten und zu laden.
Wenn alles funktioniert, werden diese zusätzlichen Optionen angezeigt, mit denen Sie weitere Wiederherstellungsoptionen ausführen können.

![ubunturec4](./media/virtual-machines-serial-console/ubunturec4.png)


## <a name="red-hat-grub-configuration"></a>Red Hat GRUB-Konfiguration

## <a name="red-hat-74-grub-configuration"></a>Red Hat 7\.4\+ GRUB-Konfiguration
Die Standardkonfiguration „/etc/default/grub“ in diesen Versionen ist angemessen konfiguriert.

```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL="serial console"
GRUB_SERIAL_COMMAND="serial"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```

Aktivieren des SysRq-Schlüssels

```
sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq
```

## <a name="red-hat-72-and-73-grub-configuration"></a>Red Hat 7\.2 und 7\.3 GRUB-Konfiguration
Die zu ändernde Datei ist „/etc/default/grub“ – eine Standardkonfiguration sieht wie in diesem Beispiel aus:

```
GRUB_TIMEOUT=1
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```

Ändern Sie in „/etc/default/grub“ die folgenden Zeilen:

```
GRUB_TIMEOUT=1 

to

GRUB_TIMEOUT=5
```


```
GRUB_TERMINAL_OUTPUT="console"

to

GRUB_TERMINAL="serial console"
```

Fügen Sie außerdem diese Zeile hinzu:

```
GRUB_SERIAL_COMMAND=”serial –speed=115200 –unit=0 –word=8 –parity=no –stop=1″
```

„/etc/default/grub“ sollte nun in etwa wie in diesem Beispiel aussehen:

```
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
GRUB_DISABLE_RECOVERY="true"
```
 
Vervollständigen und Aktualisieren Sie die GRUB-Konfiguration mithilfe von:

`grub2-mkconfig -o /boot/grub2/grub.cfg`

Legen Sie den SysRq-Kernelparameter fest:

`sysctl -w kernel.sysrq = 1;echo kernel.sysrq = 1 >> /etc/sysctl.conf;sysctl -a | grep -i sysrq`

Alternativ können Sie GRUB und SysRq mit einer einzelnen Zeile entweder in der Shell oder über den „Befehl ausführen“ konfigurieren. Sichern Sie Ihre Dateien, bevor Sie diesen Befehl ausführen:


`cp /etc/default/grub /etc/default/grub.bak; sed -i 's/GRUB_TIMEOUT=1/GRUB_TIMEOUT=5/g' /etc/default/grub; sed -i 's/GRUB_TERMINAL_OUTPUT="console"/GRUB_TERMINAL="serial console"/g' /etc/default/grub; echo "GRUB_SERIAL_COMMAND=\"serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1\"" >> /etc/default/grub;grub2-mkconfig -o /boot/grub2/grub.cfg;sysctl -w kernel.sysrq=1;echo kernel.sysrq = 1 /etc/sysctl.conf;sysctl -a | grep -i sysrq`


## <a name="red-hat-6x-grub-configuration"></a>Red Hat 6\.x GRUB-Konfiguration
Die zu ändernde Datei ist „/boot/grub/grub.conf“. Der `timeout`-Wert bestimmt, wie lange GRUB angezeigt wird.

```
#boot=/dev/vda
default=0
timeout=15
splashimage=(hd0,0)/grub/splash.xpm.gz
#hiddenmenu
serial --unit=0 --speed=9600
terminal serial
terminal --timeout=5 serial console
```


Die letzte Zeile – *terminal –-timeout=5 serial console* verlängert den Timeoutwert von **GRUB** durch Hinzufügen einer 5 Sekunden langen Aufforderung **Press any key to continue** (Zum Fortfahren beliebige Taste drücken).

![rh6-1](./media/virtual-machines-serial-console/rh6-1.png)

Das GRUB-Menü sollte am Bildschirm für den konfigurierten Timeout von 15 s angezeigt werden, ohne dass ein Druck auf ESC erforderlich ist. Achten Sie darauf, im Browser auf die Konsole zu klicken, um das Menü zu aktivieren und den erforderlichen Kernel auszuwählen.

![rh6-2](./media/virtual-machines-serial-console/rh6-2.png)

## <a name="suse"></a>SuSE

## <a name="sles-12-sp1"></a>SLES 12 sp1
Verwenden Sie den YaST Bootloader gemäß der offiziellen [Dokumentation](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#grub-access-in-suse-sles).

Oder fügen Sie „/etc/default/grub“ die folgenden Parameter hinzu, bzw. ändern Sie sie:

```
GRUB_TERMINAL=serial
GRUB_TIMEOUT=5
GRUB_SERIAL_COMMAND="serial --unit=0 --speed=9600 --parity=no"

```
Vergewissern Sie sich, dass in GRUB_CMDLINE_LINUX oder GRUB_CMDLINE_LINUX_DEFAULT ttys0 verwendet wird.

```
GRUB_CMDLINE_LINUX_DEFAULT="console=ttyS0,9600n"
```

Erstellen Sie „grub.cfg“ neu.

`grub2-mkconfig -o /boot/grub2/grub.cfg`


## <a name="sles-11-sp4"></a>SLES 11 SP4 
Die serielle Konsole erscheint und zeigt Startnachrichten an, jedoch wird keine **Anmeldeaufforderung** angezeigt.

Öffnen Sie eine SSH-Sitzung zur VM, und aktualisieren Sie die Datei **/etc/inittab**, indem Sie die Auskommentierung dieser Zeile aufheben:

```
#S0:12345:respawn:/sbin/agetty -L 9600 ttyS0 vt102
```

Führen Sie als nächstes diesen Befehl aus: 

`telinit q`

Zum Aktivieren von GRUB sollten die folgenden Änderungen an „/boot/grub/menu.lst“ vorgenommen werden. 

```
timeout 5
serial --unit=0 --speed=9600 --parity=no
terminal --timeout=5 serial console

root (hd0,0)
kernel /boot/vmlinuz-3.0.101-108.74-default root=/dev/disk/by-uuid/ab6b62bb--
1a8c-45eb-96b1-1fbc535b9265 disk=/dev/sda  USE_BY_UUID_DEVICE_NAMES=1 earlyprinttk=ttyS0 console=ttyS0 rootdelay=300  showopts vga=0x314
```

 Mit dieser Konfiguration wird die Meldung **Press any key to continue** (Zum Fortfahren beliebige Taste drücken) 5 Sekunden lang in der Konsole angezeigt. 

Anschließend wird das GRUB-Menü weitere 5 Sekunden lang angezeigt – durch Drücken der NACH-UNTEN-TASTE unterbrechen Sie den Zähler und wählen den zu startenden Kernel aus. Sie können das Schlüsselwort **single** für den Einzelbenutzermodus anfügen, für den kein Stammkennwort festgelegt werden muss.

Durch Anfügen des Befehls **init=/bin/bash** wird der Kernel geladen und zugleich sichergestellt, dass das init-Programm durch eine Bash-Shell ersetzt wird.

Sie erhalten Zugriff auf eine Shell, ohne ein Kennwort eingeben zu müssen. Anschließend können Sie damit fortfahren, Kennwörter für Linux-Konten zu aktualisieren oder andere Konfigurationsänderungen vorzunehmen.


## <a name="force-the-kernel-to-a-bash-prompt"></a>Erzwingen einer Bash-Eingabeaufforderung im Kernel
Der Zugriff auf GRUB ermöglicht Ihnen, den Initialisierungsvorgang zu unterbrechen; diese Interaktion ist für viele Wiederherstellungsverfahren nützlich.
Wenn Sie nicht über das Stammkennwort verfügen und für den Einzelbenutzermodus ein Kennwort erforderlich ist, können Sie beim Starten des Kernels das init-Programm durch eine Bash-Eingabeaufforderung ersetzen – dieser Interrupt kann durch Anfügen von „init=/bin/bash“ an die Startzeile des Kernels erreicht werden.

![bash1](./media/virtual-machines-serial-console/bash1.png)

Binden Sie Ihr /(root)-Dateisystem mit Lese-/Schreibzugriff mit diesem Befehl erneut ein:

`mount -o remount,rw /`

![bash2](./media/virtual-machines-serial-console/bash2.png)


Nun können Sie eine Änderung des Stammkennworts oder viele andere Linux-Konfigurationsänderungen durchführen.

![bash3](./media/virtual-machines-serial-console/bash3.png)

Starten Sie die VM mit diesem Befehl neu: 

`/sbin/reboot -f`




## <a name="single-user-mode"></a>Einzelbenutzermodus

Alternativ können Sie ggf. im Einzelbenutzer- oder Notfallmodus auf die VM zugreifen. Wählen Sie den zu startenden oder zu unterbrechenden Kernel mithilfe der Pfeiltasten aus.
Wechseln Sie in den gewünschten Modus, indem Sie das Schlüsselwort **single** oder **1** an die Startzeile des Kernels anfügen. Auf RHEL-Systemen können Sie auch **rd.break** anfügen.

Weitere Informationen zum Zugreifen auf den Einzelbenutzermodus finden Sie in [diesem Dokument](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-grub-single-user-mode#general-single-user-mode-access). 


![single_user_ubuntu](./media/virtual-machines-serial-console/single-user-ubuntu.png)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur [seriellen Azure-Konsole]( https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)
