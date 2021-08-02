---
title: 'Virtuellen Computer bei Verbindung starten: Azure'
description: Vorgehensweise beim Konfigurieren des Features zum Starten eines virtuellen Computers bei Verbindung.
author: Heidilohr
ms.topic: how-to
ms.date: 05/21/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 7e4ca9a6cfc87844bf74131b145c19aecd964554
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111752133"
---
# <a name="start-virtual-machine-on-connect-preview"></a>VM bei Verbindung starten (Vorschau)

> [!IMPORTANT]
> Das Feature „VM bei Verbindung starten“ befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Feature „VM bei Verbindung starten (Vorschau)“ können Sie Kosten sparen, indem Sie Endbenutzern erlauben, ihre virtuellen Computer (VMs) nur dann zu aktivieren, wenn sie sie benötigen. Nicht benötigte VMs können Sie dann deaktivieren.

>[!NOTE]
>Dieses Feature wird von Azure Virtual Desktop (klassisch) nicht unterstützt.

## <a name="requirements-and-limitations"></a>Anforderungen und Einschränkungen

Sie können das Feature „VM bei Verbindung starten“ für persönliche oder gepoolte Hostpools mithilfe von PowerShell und dem Azure-Portal aktivieren.

Die folgenden Remotedesktopclients unterstützen das Feature „VM bei Verbindung starten“:

- [Der Webclient](connect-web.md)
- [Der Windows-Client (Version 1.2748 oder höher)](connect-windows-7-10.md)
- [Der Android-Client (Version 10.0.10 oder höher)](connect-android.md)
- [Der macOS-Client (Version 10.6.4 oder höher)](connect-macos.md)

Sie können im [Tech Community-Forum](https://aka.ms/wvdtc) nach Ankündigungen zu Updates und zur Clientunterstützung suchen.

## <a name="create-a-custom-role-for-start-vm-on-connect"></a>Erstellen einer benutzerdefinierten Rolle für „VM bei Verbindung starten“

Bevor Sie das Feature „VM bei Verbindung starten“ konfigurieren können, müssen Sie Ihrem virtuellen Computer eine benutzerdefinierte RBAC-Rolle (Role-Based Access Control, rollenbasierte Zugriffssteuerung) zuweisen. Diese Rolle ermöglicht Azure Virtual Desktop die Verwaltung der VMs in Ihrem Abonnement. Sie können diese Rolle auch verwenden, um VMs zu aktivieren, ihren Status zu überprüfen und Diagnoseinformationen zu melden. Wenn Sie mehr über die Aufgaben der einzelnen Rollen erfahren möchten, lesen Sie den Artikel [Benutzerdefinierte Azure-Rollen](../role-based-access-control/custom-roles.md).

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So weisen Sie eine benutzerdefinierte Rolle für „VM bei Verbindung starten“ im Azure-Portal zu:

1. Navigieren Sie im Azure-Portal zu **Abonnements**.

2. Wechseln Sie zu **Zugriffssteuerung (IAM)** , und wählen Sie **Benutzerdefinierte Rolle hinzufügen** aus.

    > [!div class="mx-imgBorder"]
    > ![Ein Screenshot eines Dropdownmenüs der Schaltfläche „Hinzufügen“ in „Zugriffssteuerung (IAM)“. „Benutzerdefinierte Rolle hinzufügen“ ist rot hervorgehoben.](media/add-custom-role.png)

3. Benennen Sie als Nächstes die benutzerdefinierte Rolle, und fügen Sie eine Beschreibung hinzu. Wir empfehlen Ihnen die Benennung „VM bei Verbindung starten“.

4. Fügen Sie auf der Registerkarte **Berechtigungen** dem Abonnement, dem Sie die Rolle zuweisen, die folgenden Berechtigungen hinzu: 
 
   - Microsoft.Compute/virtualMachines/start/action
   - Microsoft.Compute/virtualMachines/read

5. Klicken Sie auf **OK**, wenn Sie fertig sind.

Danach müssen Sie die Rolle zuweisen, um Azure Virtual Desktop Zugriff zu gewähren.

So weisen Sie die benutzerdefinierten Rolle zu:

1. Wählen Sie auf der Registerkarte **Zugriffssteuerung (IAM)** die Option **Rollenzuweisungen hinzufügen** aus.

2. Wählen Sie die Rolle aus, die Sie gerade erstellt haben.

3. Geben Sie in der Suchleiste **Azure Virtual Desktop** ein, und wählen Sie diese Eingabe aus.

      >[!NOTE]
      >Wenn Sie Azure Virtual Desktop (klassisch) bereitgestellt haben, werden möglicherweise zwei Apps angezeigt. Weisen Sie die Rolle beiden Apps zu, die Sie sehen.
      >
      > [!div class="mx-imgBorder"]
      > ![Ein Screenshot der Registerkarte „Zugriffssteuerung (IAM)“. In der Suchleiste ist sowohl Azure Virtual Desktop als auch Azure Virtual Desktop (klassisch) rot hervorgehoben.](media/add-role-assignment.png)

### <a name="create-a-custom-role-with-a-json-file-template"></a>Erstellen einer benutzerdefinierten Rolle mit einer JSON-Dateivorlage

Wenn Sie eine JSON-Datei verwenden, um die benutzerdefinierte Rolle zu erstellen, können Sie die im folgenden Beispiel gezeigte grundlegende Vorlage verwenden. Stellen Sie sicher, dass Sie den Wert der Abonnement-ID durch die Abonnement-ID ersetzen, der Sie die Rolle zuweisen möchten.

```json
{
    "properties": {
        "roleName": "start VM on connect",
        "description": "Friendly description.",
        "assignableScopes": [
            "/subscriptions/<SubscriptionID>"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Compute/virtualMachines/start/action",
                    "Microsoft.Compute/virtualMachines/read"
                ],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ]
    }
}
```

## <a name="configure-the-start-vm-on-connect-feature"></a>Konfigurieren des Features „VM bei Verbindung starten“

Nachdem Sie Ihrem Abonnement die Rolle zugewiesen haben, können Sie das Feature „VM bei Verbindung starten“ konfigurieren.

### <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung 

„VM bei Verbindung starten“ ist eine Hostpooleinstellung. Wenn nur eine ausgewählte Gruppe von Benutzern dieses Feature verwenden soll, stellen Sie sicher, dass Sie nur den Benutzern, die Sie hinzufügen möchten, die erforderliche Rolle zuweisen.

Bei persönlichen Desktops aktiviert das Feature nur einen vorhandenen virtuellen Computer, den der Dienst bereits zugewiesen hat oder einem Benutzer zuweisen wird. In einem Szenario mit gepooltem Hostpool aktiviert der Dienst nur dann einen virtuellen Computer, wenn keine aktiviert sind. Das Feature aktiviert nur dann zusätzliche virtuelle Computer, wenn der erste virtuelle Computer den Sitzungsgrenzwert erreicht.

>[!IMPORTANT]
> Dieses Feature kann nur in vorhandenen Hostpools konfiguriert werden. Wenn Sie einen neuen Hostpool erstellen, ist dieses Feature nicht verfügbar.

### <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwenden Sie das Azure-Portal zum Konfigurieren von „VM bei Verbindung starten“:

1. Öffnen Sie Ihren Browser, und wechseln Sie zum [Azure-Portal](https://portal.azure.com).

2. Wechseln Sie im Azure-Portal zu **Azure Virtual Desktop**.

3. Wählen Sie **Hostpools** aus, und wechseln Sie dann zum Hostpool, in dem Sie die Einstellung aktivieren möchten.

4. Wählen Sie im Hostpool **Eigenschaften** aus. Wählen Sie unter **VM bei Verbindung starten** die Option **Ja** und dann **Speichern** aus, damit die Einstellung sofort angewendet wird.

    > [!div class="mx-imgBorder"]
    > ![Screenshot des Fensters „Eigenschaften“. Die Option „VM bei Verbindung starten“ ist rot hervorgehoben.](media/properties-start-vm-on-connect.png)

### <a name="use-powershell"></a>Verwenden von PowerShell

Um diese Einstellung mit PowerShell zu konfigurieren, müssen Sie sicherstellen, dass Sie über die Namen der Ressourcengruppe und der Hostpools verfügen, die Sie konfigurieren möchten. Außerdem müssen Sie das [Azure PowerShell-Modul (Version 2.1.0 oder höher)](https://www.powershellgallery.com/packages/Az.DesktopVirtualization/2.1.0) installieren.

So konfigurieren Sie „VM bei Verbindung starten“ mit PowerShell:

1. Öffnen Sie ein PowerShell-Befehlsfenster.

2. Führen Sie das folgende Cmdlet aus, um „VM bei Verbindung starten“ zu aktivieren:

    ```powershell
    Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -StartVMOnConnect:$true
    ```

3. Führen Sie das folgende Cmdlet aus, um „VM bei Verbindung starten“ zu deaktivieren:

    ```powershell
    Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -StartVMOnConnect:$false
    ```

## <a name="user-experience"></a>Benutzererfahrung

In normalen Sitzungen nimmt die Zeit zu, die ein Benutzer zum Herstellen der Verbindung mit einer VM benötigt, deren Zuordnung aufgehoben wurde, da das erneute Einschalten der VM ähnlich wie das Einschalten eines physischen Computers eine Weile dauert. Der Remotedesktopclient hat einen Indikator, der dem Benutzer mitteilt, dass der PC eingeschaltet wird, während er die Verbindung herstellt.

## <a name="troubleshooting"></a>Problembehandlung

Wenn bei dem Feature Probleme auftreten, sollten Sie mit dem [Diagnosefeature](diagnostics-log-analytics.md) von Azure Virtual Desktop nach der Ursache suchen. Wenn Sie eine Fehlermeldung erhalten, beachten Sie deren Inhalt genau, und notieren Sie sich den Fehlernamen.

Sie können auch [Azure Monitor für Azure Virtual Desktop](azure-monitor.md) verwenden, um Vorschläge zum Beheben von Problemen zu erhalten.

Wenn sich der virtuelle Computer nicht aktivieren lässt, müssen Sie die Integrität des virtuellen Computers überprüfen, den Sie zu aktivieren versuchen, bevor Sie andere Schritte unternehmen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Probleme auftreten, die mithilfe der Dokumentation zur Problembehandlung oder des Diagnosefeatures nicht gelöst werden konnten, besuchen Sie die [FAQ zu „VM bei Verbindung starten“](start-virtual-machine-connect-faq.md).
