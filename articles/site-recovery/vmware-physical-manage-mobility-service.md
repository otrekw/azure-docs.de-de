---
title: Verwalten des Mobilität-Agent für VMware-/physische Server mit Azure Site Recovery
description: Verwalten Sie den Mobilitäts-Agent, um mit dem Azure Site Recovery-Dienst eine Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure auszuführen.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 9be758c286e072b0fbefc5f8b20b7accc4e6741b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228850"
---
# <a name="manage-the-mobility-agent"></a>Verwalten des Mobilitäts-Agent 

Sie richten einen Mobilitäts-Agent auf Ihrem Server ein, wenn Sie Azure Site Recovery für die Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure verwenden. Der Mobilitäts-Agent koodiniert die Kommunikation zwischen dem geschützten Computer und dem Konfigurationsserver/Prozessserver für horizontales Hochskalieren und verwaltet die Datenreplikation. In diesem Artikel werden häufige Aufgaben zur Verwaltung des Mobilitäts-Agents nach dessen Bereitstellung zusammengefasst.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="update-mobility-service-from-azure-portal"></a>Aktualisieren des Mobilitätsdiensts über das Azure-Portal

1. Stellen Sie vor dem Start sicher, dass der Konfigurationsserver, der Prozessserver für horizontales Skalieren und alle Masterzielserver, die Teil der Bereitstellung sind, aktualisiert werden, bevor Sie mit der Aktualisierung von Mobility Service auf den geschützten Computern beginnen.
2. Öffnen Sie im Portal den Tresor > **Replizierte Elemente**.
3. Wenn der Konfigurationsserver bereits die neueste Version verwendet, wird eine Benachrichtigung angezeigt, dass ein neues Update für den Site Recovery-Replikations-Agent verfügbar ist. Sie können es per Klick installieren.

     ![Fenster „Replizierte Elemente“](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Klicken Sie auf die Benachrichtigung, und wählen Sie in **Agent-Update** den Computer, auf dem Sie den Mobilitätsdienst aktualisieren möchten. Klicken Sie dann auf **OK**.

     ![VM-Liste mit replizierten Elementen](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. Der Aktualisierungsauftrag für Mobility Service wird für jeden ausgewählten Computer gestartet.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Aktualisieren des Mobilitätsdiensts über ein PowerShell-Skript unter Windows Server

Stellen Sie vor dem Start sicher, dass der Konfigurationsserver, der Prozessserver für horizontales Skalieren und alle Masterzielserver, die Teil der Bereitstellung sind, aktualisiert werden, bevor Sie mit der Aktualisierung von Mobility Service auf den geschützten Computern beginnen.

Verwenden Sie das folgende Skript zum Aktualisieren des Mobilitätsdiensts auf einem Server über das PowerShell-Cmdlet:

```azurepowershell
Update-AzRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-mobility-service-manually-on-each-protected-server"></a>Manuelles Aktualisieren des Mobilitätsdiensts auf den einzelnen geschützten Servern

1. Stellen Sie vor dem Start sicher, dass der Konfigurationsserver, der Prozessserver für horizontales Skalieren und alle Masterzielserver, die Teil der Bereitstellung sind, aktualisiert werden, bevor Sie mit der Aktualisierung von Mobility Service auf den geschützten Computern beginnen.

2. [Suchen Sie das Agent-Installationsprogramm](vmware-physical-mobility-service-overview.md#locate-installer-files) basierend auf dem Betriebssystem des Servers.

>[!IMPORTANT]
> Wenn Sie virtuelle Azure-IaaS-Computer von einer Azure-Region in eine andere replizieren, sollten Sie diese Methode nicht verwenden. Weitere Informationen zu allen verfügbaren Optionen finden Sie in [unserem Leitfaden](azure-to-azure-autoupdate.md).

3. Kopieren Sie die Installationsdatei auf den geschützten Computer, und führen Sie sie aus, um den Mobilitäts-Agent zu aktualisieren.

## <a name="update-account-used-for-push-installation-of-mobility-service"></a>Aktualisieren des für die Pushinstallation des Mobilitätsdiensts verwendeten Kontos

Wenn Sie Site Recovery eingesetzt haben, um die Pushinstallation des Mobilitätsdiensts zu aktivieren, haben Sie ein Konto angegeben, über das der Site Recovery-Prozessserver auf die Computer zugreift und den Dienst installiert, wenn die Replikation für den Computer aktiviert ist. Wenn Sie die Anmeldeinformationen für dieses Konto aktualisieren möchten, führen Sie [diese Anweisungen](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) aus.

## <a name="uninstall-mobility-service"></a>Deinstallieren des Mobilitätsdiensts

### <a name="on-a-windows-machine"></a>Auf einem Windows-Computer

Die Deinstallation erfolgt über die Benutzeroberfläche oder eine Eingabeaufforderung.

- **Über die Benutzeroberfläche**: Wählen Sie in der Systemsteuerung des Computers **Programme** aus. Wählen Sie **Microsoft Azure Site Recovery Mobility Service/Masterzielserver** > **Deinstallieren**.
- **Über eine Eingabeaufforderung**: Öffnen Sie als Administrator ein Eingabeaufforderungsfenster auf dem Computer. Führen Sie den folgenden Befehl aus: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

### <a name="on-a-linux-machine"></a>Auf einem Linux-Computer
1. Melden Sie sich auf dem Linux-Computer als ein **root**-Benutzer an.
2. Navigieren Sie in einem Terminal zu „/usr/local/ASR“.
3. Führen Sie den folgenden Befehl aus:
    ```
    uninstall.sh -Y
   ```
   
## <a name="install-site-recovery-vss-provider-on-source-machine"></a>Installieren des Site Recovery-VSS-Anbieters auf dem Quellcomputer

Auf dem Quellcomputer ist der Azure Site Recovery-VSS-Anbieter erforderlich, um Anwendungskonsistenzpunkte zu generieren. Wenn die Pushinstallation des Anbieters nicht erfolgreich war, befolgen Sie die unten aufgeführten Richtlinien, um ihn manuell zu installieren.

1. Öffnen Sie das Administratorbefehlsfenster.
2. Navigieren Sie zum Mobilitätsdienst-Installationsspeicherort. (D.h. C:\Programme (x86)\Microsoft Azure Site Recovery\agent)
3. Führen Sie das Skript InMageVSSProvider_Uninstall.cmd aus. Dadurch wird der Dienst deinstalliert, wenn er bereits vorhanden ist.
4. Führen Sie das Skript InMageVSSProvider_Install. cmd aus, um den VSS-Anbieter manuell zu installieren.

## <a name="next-steps"></a>Nächste Schritte

- [Einrichten der Notfallwiederherstellung für VMware-VMs](vmware-azure-tutorial.md)
- [Einrichten der Notfallwiederherstellung für physische Server](physical-azure-disaster-recovery.md)
