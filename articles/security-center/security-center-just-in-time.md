---
title: Just-In-Time-VM-Zugriff in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie mit Just-In-Time-VM-Zugriff in Azure Security Center den Zugriff auf die virtuellen Azure-Computer steuern können.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: cc4e267c6912b8938db1ba5497a27f9c0026bd79
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887332"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>Sichern Ihrer Verwaltungsports mit Just-in-Time-Zugriff (JIT)

Wenn Sie den Security Center-Tarif „Standard“ verwenden (siehe [Preise](/azure/security-center/security-center-pricing)), können Sie den eingehenden Datenverkehr auf Ihren virtuellen Azure-Computern mit Just-in-Time-VM-Zugriff sperren. Dies verringert die Angriffsfläche und bietet bei Bedarf einfachen Zugriff zum Herstellen einer Verbindung mit VMs.

> [!NOTE]
> Der durch das Security Center gesteuerte Just-In-Time-Zugriff auf virtuelle Computer unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Weitere Informationen über das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/management/deployment-models.md).

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>Konfigurieren von JIT auf einem virtuellen Computer

Es gibt drei Möglichkeiten, eine JIT-Richtlinie auf einem virtuellen Computer zu konfigurieren:

- [Konfigurieren des JIT-Zugriffs in Azure Security Center](#jit-asc)
- [Konfigurieren des JIT-Zugriffs auf einer Azure VM-Seite](#jit-vm)
- [Programmgesteuertes Konfigurieren einer JIT-Richtlinie auf einem virtuellen Computer](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>Konfigurieren des JIT-Zugriffs in Azure Security Center

In Security Center können Sie eine JIT-Richtlinie konfigurieren und mithilfe einer JIT-Richtlinie den Zugriff auf einen virtuellen Computer anfordern.

### <a name="configure-jit-access-on-a-vm-in-security-center"></a>Konfigurieren des JIT-Zugriffs auf einen virtuellen Computer in Security Center <a name="jit-asc"></a>

1. Öffnen Sie das Dashboard **Security Center**.

1. Wählen Sie im linken Bereich die Option **JIT-VM-Zugriff** aus.

    ![Kachel „JIT-VM-Zugriff“](./media/security-center-just-in-time/just-in-time.png)

    Das Fenster **JIT-VM-Zugriff** mit Informationen zum Status Ihrer virtuellen Computer wird geöffnet:

    - **Konfiguriert**: Virtuelle Computer, die für die Unterstützung von Just-In-Time-Zugriff konfiguriert wurden. Die dargestellten Daten betreffen die letzte Woche und enthalten für jeden virtuellen Computer die Anzahl der genehmigten Anforderungen, Datum und Uhrzeit des letzten Zugriffs sowie den letzten Benutzer.
    - **Empfohlen**: Virtuelle Computer, die Just-In-Time-VM-Zugriff unterstützen, jedoch nicht entsprechend konfiguriert wurden. Es wird empfohlen, die Just-In-Time-VM-Zugriffssteuerung für diese virtuellen Computer zu aktivieren.
    - **Keine Empfehlung** – Mögliche Gründe, dass ein virtueller Computer nicht empfohlen wird:
      - Fehlende NSG: Die Just-In-Time-Lösung erfordert, dass eine NSG festgelegt ist.
      - Klassischer virtueller Computer: Der durch das Security Center gesteuerte Just-In-Time-VM-Zugriff unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Klassische Bereitstellungen werden durch die Just-In Time-Lösung nicht unterstützt. 
      - Andere: Ein virtueller Computer fällt in diese Kategorie, wenn die Just-In-Time-Lösung in der Sicherheitsrichtlinie des Abonnements oder der Ressourcengruppe deaktiviert ist oder wenn der virtuelle Computer über keine öffentliche IP-Adresse und über keine NSG verfügt.

1. Wählen Sie die Registerkarte **Empfohlen** aus.

1. Klicken Sie unter **VIRTUELLER COMPUTER** auf die virtuellen Computer, die Sie aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.

      ![Aktivieren des Just-in-Time-Zugriffs](./media/security-center-just-in-time/enable-just-in-time.png)

1. Klicken Sie auf **JIT auf VMs aktivieren**. In einem Bereich werden die von Azure Security Center empfohlenen Standardports angezeigt:
    - 22 – SSH
    - 3389 – RDP
    - 5985 – WinRM 
    - 5986 – WinRM
1. Optional können Sie der Liste benutzerdefinierte Ports hinzufügen:

      1. Klicken Sie auf **Hinzufügen**. Das Fenster **Add port configuration** (Portkonfiguration hinzufügen) wird geöffnet.
      1. Sie können für jeden zu konfigurierenden Port (sowohl Standard- als auch benutzerdefinierter Port) die folgenden Einstellungen anpassen:
            - **Protokolltyp**: Das Protokoll, das auf diesem Port zulässig ist, wenn eine Anforderung genehmigt wird.
            - **Zulässige Quell-IP-Adressen**: Die IP-Bereiche, die für diesen Port zulässig sind, wenn eine Anforderung genehmigt wird.
            - **Maximale Anforderungsdauer**: Das maximale Zeitfenster, in dem ein bestimmter Port geöffnet werden kann.

     1. Klicken Sie auf **OK**.

1. Klicken Sie auf **Speichern**.

> [!NOTE]
>Wenn der JIT-VM-Zugriff für einen virtuellen Computer aktiviert ist, erstellt Azure Security Center Regeln zum Ablehnen von sämtlichem eingehenden Datenverkehr für die ausgewählten Ports in den ihr zugeordneten Netzwerksicherheitsgruppen und der Azure Firewall. Wurden für die ausgewählten Ports andere Regeln erstellt, so haben die bestehenden Regeln Vorrang vor den neuen Regeln zum Ablehnen von sämtlichem eingehenden Datenverkehr. Wenn es für die ausgewählten Ports keine Regeln gibt, dann haben die neuen Regeln zum Ablehnen von sämtlichem eingehenden Datenverkehr in den Netzwerksicherheitsgruppen und der Azure Firewall höchste Priorität.


## <a name="request-jit-access-via-security-center"></a>Anfordern des JIT-Zugriffs über Security Center

So fordern Sie den Zugriff auf einen virtuellen Computer in Security Center an:

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.

1. Klicken Sie unter **Virtueller Computer** auf die virtuellen Computer, für die Sie Zugriff anfordern möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.

    - Das Symbol in der Spalte **Verbindungsdetails** gibt an, ob JIT in der Netzwerksicherheitsgruppe oder der Firewall aktiviert ist. Wenn JIT für beide aktiviert ist, wird nur das Firewallsymbol angezeigt.

    - Die Spalte **Verbindungsdetails** enthält die Informationen, die dazu erforderlich sind, den virtuellen Computer zu verbinden, und enthält dessen geöffnete Ports.

      ![Anfordern des Just-In-Time-Zugriffs](./media/security-center-just-in-time/request-just-in-time-access.png)

1. Klicken Sie auf **Zugriff anfordern**. Das Fenster **Zugriff anfordern** wird geöffnet.

      ![JIT-Details](./media/security-center-just-in-time/just-in-time-details.png)

1. Konfigurieren Sie unter **Zugriff anfordern** für jeden virtuellen Computer die zu öffnenden Ports und die Quell-IP-Adressen, für die der jeweilige Port geöffnet wird, und das Zeitfenster, in dem der Port geöffnet wird. Es kann nur Zugriff auf die Ports angefordert werden, die in der Just-In-Time-Richtlinie konfiguriert sind. Für jeden Port gilt eine maximal zulässige Zeitspanne, die aus der Just-In-Time-Richtlinie abgeleitet ist.

1. Klicken Sie auf **Open ports** (Offene Ports).

> [!NOTE]
> Wenn sich ein Benutzer, der Zugriff anfordert, hinter einem Proxy befindet, funktioniert die Option **Meine IP-Adresse** möglicherweise nicht. Möglicherweise müssen Sie den vollständigen IP-Adressenbereich der Organisation definieren.



## <a name="edit-a-jit-access-policy-via-security-center"></a>Bearbeiten einer JIT-Zugriffsrichtlinie über Security Center

Sie können die vorhandene Just-In-Time-Richtlinie eines virtuellen Computers ändern, indem Sie einen neuen Port, der für diesen virtuellen Computer geschützt werden soll, hinzufügen und konfigurieren oder eine beliebige andere Einstellung eines bereits geschützten Ports ändern.

So bearbeiten Sie eine vorhandene Just-In-Time-Richtlinie eines virtuellen Computers:

1. Wählen Sie auf der Registerkarte **Konfiguriert** unter **VMs** einen virtuellen Computer aus, dem ein Port hinzugefügt werden soll, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken. 

1. Wählen Sie **Bearbeiten** aus.

1. Unter **JIT-VM-Zugriffskonfiguration** können Sie die vorhandenen Einstellungen eines bereits geschützten Ports bearbeiten oder einen neuen benutzerdefinierten Port hinzufügen. 
  ![JIT-VM-Zugriff](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>Überwachen einer JIT-Zugriffsaktivität über Security Center

Mit der Protokollsuche erhalten Sie Einblicke in VM-Aktivitäten. So zeigen Sie Protokolle an:

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.
2. Wählen Sie unter **VMs** einen virtuellen Computer aus, zu dem Informationen angezeigt werden sollen, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken und im Menü **Aktivitätsprotokoll** auswählen. Das **Aktivitätsprotokoll** wird geöffnet.

   ![Aktivitätsprotokoll auswählen](./media/security-center-just-in-time/select-activity-log.png)

   **Aktivitätsprotokoll** enthält eine gefilterte Ansicht der früheren Vorgänge für diesen virtuellen Computer zusammen mit der Uhrzeit, dem Datum und dem Abonnement.

Sie können die Protokollinformationen herunterladen, indem Sie **Klicken Sie hier, um alle Elemente als CSV herunterzuladen** auswählen.

Ändern Sie die Filter, und klicken Sie auf **Anwenden**, um eine Suche und ein Protokoll zu erstellen.



## <a name="configure-jit-access-from-an-azure-vms-page"></a>Konfigurieren des JIT-Zugriffs auf der Seite eines virtuellen Azure-Computers <a name="jit-vm"></a>

Zur Vereinfachung können Sie direkt von der Seite des virtuellen Computers in Security Center über JIT eine Verbindung mit einem virtuellen Computer herstellen.

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>Konfigurieren des JIT-Zugriffs auf einen virtuellen Computer über die Seite des virtuellen Azure-Computers

Um den Rollout des Just-In-Time-Zugriffs auf Ihre virtuellen Computer zu vereinfachen, können Sie einen virtuellen Computer so einrichten, dass er nur den direkten Just-In-Time-Zugriff innerhalb des virtuellen Computers gestattet.

1. Suchen Sie im [Azure-Portal](https://ms.portal.azure.com) nach **Virtuelle Computer**, und wählen Sie die Option aus. 
2. Wählen Sie den virtuellen Computer aus, den Sie auf den Just-In-Time-Zugriff beschränken möchten.
3. Wählen Sie im Menü die Option **Konfiguration** aus.
4. Wählen Sie unter **Just-in-Time-Zugriff** die Option **Just-in-Time aktivieren** aus. 

Dies ermöglicht den Just-In-Time-Zugriff auf den virtuellen Computer mit den folgenden Einstellungen:

- Windows-Server:
    - RDP-Port 3389
    - Zugriff bis zu maximal drei Stunden
    - Zulässige Quell-IP-Adressen auf „Beliebige“ festgelegt
- Linux-Server:
    - SSH-Port 22
    - Zugriff bis zu maximal drei Stunden
    - Zulässige Quell-IP-Adressen auf „Beliebige“ festgelegt
     
Wenn für einen virtuellen Computer bereits Just-In-Time aktiviert ist, können Sie auf der Konfigurationsseite sehen, dass Just-In-Time aktiviert ist. Zudem können Sie über den Link die Richtlinie im Azure Security Center öffnen, um die Einstellungen anzuzeigen und zu ändern.

![JIT-Konfiguration im virtuellen Computer](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>Anfordern des JIT-Zugriffs auf einen virtuellen Computer über die Seite eines virtuellen Azure-Computers

Wenn Sie im Azure-Portal versuchen, eine Verbindung mit einem virtuellen Computer herzustellen, überprüft Azure, ob Sie eine Just-In-Time-Zugriffsrichtlinie auf diesem virtuellen Computer konfiguriert haben.

- Wenn Sie eine JIT-Richtlinie auf dem virtuellen Computer konfiguriert haben, können Sie auf **Zugriff anfordern** klicken, um entsprechend der JIT-Richtlinie, die für den virtuellen Computer festgelegt ist, Zugriff zu gewähren. 

  >![JIT-Anforderung](./media/security-center-just-in-time/jit-request.png)

  Der Zugriff wird mit den folgenden Standardparametern angefordert:

  - **Quell-IP**: „Beliebige“ (*) (kann nicht geändert werden)
  - **Zeitbereich**: Drei Stunden (kann nicht geändert werden) <!--Isn't this set in the policy-->
  - **Portnummer**: RDP-Port 3389 für Windows bzw. Port 22 für Linux (kann geändert werden)

    > [!NOTE]
    > Nachdem eine Anforderung für einen durch Azure Firewall geschützten virtuellen Computer genehmigt wurde, stellt Security Center dem Benutzer die richtigen Verbindungsinformationen (die Portzuordnung aus der DNAT-Tabelle) zum Herstellen einer Verbindung mit dem virtuellen Computer zur Verfügung.

- Wenn Sie keine JIT-Zugriffsrichtlinie auf einem virtuellen Computer konfiguriert haben, werden Sie aufgefordert, dies zu tun.

  ![JIT-Eingabeaufforderung](./media/security-center-just-in-time/jit-prompt.png)

## <a name="configure-a-jit-policy-on-a-vm-programmatically"></a>Programmgesteuertes Konfigurieren einer JIT-Richtlinie auf einem virtuellen Computer <a name="jit-program"></a>

Sie können Just-In-Time über REST-APIs und über PowerShell einrichten.

### <a name="jit-vm-access-via-rest-apis"></a>Just-In-Time-VM-Zugriff über REST-APIs

Das Feature Just-In-Time-VM-Zugriff kann über die Azure Security Center-API verwendet werden. Sie können über diese API u.a. Informationen über konfigurierte virtuelle Computer erhalten, neue hinzufügen und Zugriff auf einen virtuellen Computer anfordern. Unter [Richtlinien für den Jit-Netzwerkzugriff](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) finden Sie weitere Informationen zur Just-In-Time-REST-API.

### <a name="jit-vm-access-via-powershell"></a>Just-In-Time-VM-Zugriff über PowerShell

Um die Lösung für den Just-In-Time-VM-Zugriff mit PowerShell zu verwenden, verwenden Sie die offiziellen Azure Security Center-PowerShell-Cmdlets und insbesondere `Set-AzJitNetworkAccessPolicy`.

Im folgenden Beispiel werden auf einer bestimmten VM eine Richtlinie für den Just-In-Time-VM-Zugriff sowie die folgenden Einstellungen festgelegt:

1.    Schließen Sie die Ports 22 und 3389.

2.    Legen Sie für die Ports ein maximales Zeitfenster von drei Stunden fest, damit sie bei genehmigter Anforderung geöffnet werden können.
3.    Damit wird dem Benutzer, der Zugriff anfordert, ermöglicht, den Zugriff auf die Quell-IP-Adressen zu steuern und bei genehmigter Just-In-Time-Zugriffsanforderung eine Sitzung herzustellen.

Führen Sie hierfür Folgendes in PowerShell aus:

1.    Weisen Sie eine Variable zu, die die Just-In-Time-VM-Zugriffsrichtlinie für eine VM enthält:

        $JitPolicy = (@{    id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"   ports=(@{        number=22;        protocol="\*";        allowedSourceAddressPrefix=@("\*");        maxRequestAccessDuration="PT3H"},        @{        number=3389;        protocol="\*";        allowedSourceAddressPrefix=@("\*");        maxRequestAccessDuration="PT3H"})})

2.    Fügen Sie einem Array die Just-In-Time-VM-Zugriffsrichtlinie hinzu:
    
        $JitPolicyArr=@($JitPolicy)

3.    Konfigurieren Sie die Just-In-Time-VM-Zugriffsrichtlinie auf der ausgewählten VM:
    
        Set-AzJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>Anfordern des Zugriffs auf einen virtuellen Computer über PowerShell

Im folgenden Beispiel können Sie eine Just-In-Time-VM-Zugriffsanforderung für eine bestimmte VM sehen, in der das Öffnen von Port 22 für eine bestimmte IP-Adresse und für einen bestimmten Zeitraum angefordert wird:

Führen Sie die folgenden Schritte in PowerShell aus:
1.    Konfigurieren Sie die Eigenschaften für die VM-Zugriffsanforderung.

        $JitPolicyVm1 = (@{     id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"   ports=(@{      number=22;      endTimeUtc="2018-09-17T17:00:00.3658798Z";      allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.    Fügen Sie die Parameter für die VM-Zugriffsanforderung zu einem Array hinzu:

        $JitPolicyArr=@($JitPolicyVm1)
3.    Senden Sie die Zugriffsanforderung (mit der Ressourcen-ID, die Sie in Schritt 1 abgerufen haben).

        Start-AzJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Weitere Informationen finden Sie in der Dokumentation zum [PowerShell-Cmdlet](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview).


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>Automatische Bereinigung von redundanten JIT-Regeln 

Jedes Mal, wenn Sie eine JIT-Richtlinie aktualisieren, wird automatisch ein Bereinigungstool ausgeführt, um die Gültigkeit Ihres gesamten Regelsatzes zu überprüfen. Das Tool sucht nach Konflikten zwischen den Regeln in Ihrer Richtlinie und den Regeln in der NSG. Wenn das Bereinigungstool einen Konflikt feststellt, ermittelt es die Ursache und entfernt integrierte Regeln, die nicht mehr benötigt werden, sofern dies sicher ist. Der Cleaner löscht niemals Regeln, die Sie erstellt haben.

Beispiele für Szenarien, in denen der Cleaner eine integrierte Regel entfernen könnte:

- Wenn zwei Regeln mit identischen Definitionen vorhanden sind, und eine Regel eine höhere Priorität als die andere aufweist (d. h. die Regel mit der niedrigeren Priorität wird niemals verwendet)
- Wenn eine Regelbeschreibung den Namen einer VM enthält, die nicht mit der Ziel-IP in der Regel übereinstimmt 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde Ihnen vermittelt, wie Sie mit Just-In-Time-VM-Zugriff im Security Center den Zugriff auf die virtuellen Azure-Computer steuern können.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- Im Microsoft Learn-Modul [Schützen Ihrer Server und VMs vor Brute-Force- und Schadsoftwareangriffen mit dem Azure Security Center](https://docs.microsoft.com/learn/modules/secure-vms-with-azure-security-center/)
- [Festlegen von Sicherheitsrichtlinien:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
