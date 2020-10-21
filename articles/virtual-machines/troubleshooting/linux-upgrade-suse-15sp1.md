---
title: Upgrade einer Azure-VM mit SUSE Linux Enterprise Server auf SUSE 15 SP1 | Microsoft-Dokumentation
description: In diesem Artikel finden Sie allgemeine Schritte für die Verwendung des Systems für die Migration der SUSE-Distribution zum Durchführen eines Upgrades von SUSE Linux Enterprise Server auf SUSE 15 SP1 für einen virtuellen Azure-Computer.
services: virtual-machines-linux
documentationcenter: ''
author: amkarmak
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 05/21/2020
ms.author: arremana
ms.openlocfilehash: 7ed355f82b88f460ff4b372484a690f166a15550
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91359510"
---
# <a name="upgrade-azure-vm-with-sles-12-to-sles-15-sp1"></a>Upgrade einer Azure-VM mit SLES 12 auf SLES 15 SP1

In diesem Artikel finden Sie allgemeine Schritte zum Durchführen eines Upgrades von SLES 12 (SUSE Linux Enterprise Server) auf SLES 15 SP1 für eine Azure-VM. Weitere Informationen finden Sie unter [Verwenden des Systems für die Migration der SUSE-Distribution](https://documentation.suse.com/suse-distribution-migration-system/1.0/single-html/distribution-migration-system/index.html) und in der [Anleitung zum Upgrade von SUSE Linux Enterprise Server 15 SP1](https://documentation.suse.com/sles/15-SP1/single-html/SLES-upgrade/index.html#sec-update-preparation-update).

## <a name="supported-upgrade-paths"></a>Unterstützte Upgradepfade
Sie benötigen mindestens SLES 12 SP4 oder 12 SP5, bevor Sie ein Upgrade auf SLES 15 SP1 durchführen können.

![Screenshot zum unterstützten Upgradepfad](./media/linux-upgrade-suse-15sp1/upgrade-path.png)

## <a name="prerequisites"></a>Voraussetzungen

- Planen Sie die Migration nach dem Zeitfenster für genehmigte Ausfallzeiten. Der Grund dafür besteht darin, dass die VM während der Migration neu gestartet wird.
- Erstellen Sie vor der Migration eine vollständige Sicherung der VM.
- Wenn keine Sicherung konfiguriert ist, erstellen Sie eine Momentaufnahmesicherung des Betriebssystemdatenträgers.
- [Überprüfen Sie, ob es sich um eine VM der Generation V1 oder der Generation V2 handelt](#check-the-generation-version-for-a-vm).

## <a name="upgrade-from-suse-12-sp4-or-sp5-to-suse-15-sp1"></a>Upgrade von SUSE 12 SP4 oder SP5 auf SUSE 15 SP1

1. Installieren Sie das neueste Paket für die VM:

    ```
    zypper clean --all
    zypper refresh
    zypper update
    ```

2. Starten Sie die VM nach der Installation neu.

3. Überprüfen Sie die Kernel- und Betriebssystemversion. Stellen Sie sicher, dass die Version SUSE 12 SP4 oder SUSE 12 SP5 vorliegt.

    ```
    uname -a
    cat /etc/os-release
    ```

4. Installieren Sie das Paket **suse-migration-sle15-activation**. Bei der Installation des Pakets **suse-migration-sle15-activation** wird automatisch das Paket **SLES15-Migration** als Abhängigkeitspaket installiert. 

   ```
   zypper install suse-migration-sle15-activation
   ```

5. Führen Sie nach der Installation den Befehl `reboot` aus, um die VM neu zu starten.

6. Öffnen Sie das [Azure-Portal](https://portal.azure.com), wählen Sie die VM aus, und wählen Sie dann die Option **Serielle Konsole** aus. Sie werden feststellen, dass das System bei „reboot: Restarting system“ anhält. Dieser Prozess sollte etwa 15 bis 45 Minuten dauern. Bei VMs der Generation V2 hält der Prozess möglicherweise bei der Anzeige „reboot: Restarting system“ an. Warten Sie in diesem Fall 45 Minuten. Wenn der Prozess weiterhin nicht fortgesetzt wird, rufen Sie die Seite **Übersicht** der VM im Azure-Portal auf, beenden Sie die VM, und starten Sie diese dann neu.

     ![Screenshot zu den Meldungen in der seriellen Konsole.](./media/linux-upgrade-suse-15sp1/reboot-message.png)

8. Sobald das System mit dem neuen Kernel neu gestartet wurde, wird Ihnen die folgende Meldung angezeigt.

     ![Screenshot zu den Meldungen in der seriellen Konsole, nachdem das System mit dem neuen Kernel neu gestartet wurde.](./media/linux-upgrade-suse-15sp1/output-message.png)
9. Überprüfen Sie die Kernel- und Betriebssystemversion, um in Erfahrung zu bringen, ob das Systemupgrade erfolgreich durchgeführt wurde.

    ```
    uname -a
    cat /etc/os-release
    ```

## <a name="check-the-generation-version-for-a-vm"></a>Überprüfen der Version einer VM

Sie können eine der folgenden Methoden zum Überprüfen der Version verwenden:

- Führen Sie den Befehl `dmidecode | grep -i hyper` im SLES-Terminal aus. Wenn es sich um eine VM der Generation V1 handelt, wird keine Ausgabe zurückgegeben. Bei VMs der Generation V2 wird die folgende Ausgabe zurückgegeben:

     ![Screenshot: Ausgabe für VMs der Generation V2](./media/linux-upgrade-suse-15sp1/output-gen2.png)
- Öffnen Sie im [Azure-Portal](https://portal.azure.com) die **Eigenschaften** der VM, und überprüfen Sie dann das Feld **VM-Generation**.
