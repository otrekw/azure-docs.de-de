---
title: Erweitern eines vorhandenen Hostpools in Windows Virtual Desktop (klassisch) mit neuen Sitzungshosts – Azure
description: Hier finden Sie Informationen dazu, wie Sie einen vorhandenen Hostpool in Windows Virtual Desktop (klassisch) um neue Sitzungshosts erweitern.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 61cf28b0f1ebee6a0312ec3f23f22b01c6c4919e
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009170"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts-in-windows-virtual-desktop-classic"></a>Erweitern eines vorhandenen Hostpools in Windows Virtual Desktop (klassisch) mit neuen Sitzungshosts

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../expand-existing-host-pool.md) weiter.

Wenn Sie die Nutzung in Ihrem Hostpool erhöhen, müssen Sie den vorhandenen Pool möglicherweise um neue Sitzungshosts erweitern, um die neue Last zu verarbeiten.

In diesem Artikel wird beschrieben, wie Sie einen vorhandenen Hostpool um neue Sitzungshosts erweitern.

## <a name="what-you-need-to-expand-the-host-pool"></a>Voraussetzungen für die Erweiterung des Hostpools

Bevor Sie beginnen, stellen Sie sicher, dass der Hostpool und die Sitzungshost-VMs mit einer der folgenden Methoden erstellt wurden:

- [Angebot im Azure Marketplace](create-host-pools-azure-marketplace-2019.md)
- [Azure Resource Manager-Vorlage in GitHub](create-host-pools-arm-template.md)
- [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell-2019.md)

Sie benötigen außerdem folgende Informationen aus dem Vorgang der ersten Erstellung des Hostpools und der Sitzungshost-VMs:

- Größe, Image und Namenspräfix der VM
- Informationen zum Domänenbeitritt und Anmeldeinformationen für den Windows Virtual Desktop-Mandantenadministrator
- Name des virtuellen Netzwerks und des Subnetzes

In den nächsten Abschnitten werden die drei Methoden beschrieben, die Sie zum Erweitern des Hostpools verwenden können. Sie können jede dieser Methoden mit dem von Ihnen bevorzugten Bereitstellungstool verwenden.

>[!NOTE]
>Während der Bereitstellungsphase werden Fehlermeldungen für Ressourcen der vorherigen Sitzungshost-VMs angezeigt, wenn diese derzeit herunterfahren sind. Diese Fehler treten auf, weil Azure die PowerShell-DSC-Erweiterung nicht ausführen kann, um zu überprüfen, ob die Sitzungshost-VMs ordnungsgemäß bei Ihrem vorhandenen Hostpool registriert sind. Sie können diese Fehler getrost ignorieren. Wenn Sie die Fehler vermeiden möchten, starten Sie alle Sitzungshost-VMs im vorhandenen Hostpool, bevor Sie den Bereitstellungsprozess starten.

## <a name="redeploy-from-azure"></a>Erneutes Bereitstellen aus Azure

Wenn Sie bereits mithilfe des [Angebots im Azure Marketplace](create-host-pools-azure-marketplace-2019.md) oder der [Azure Resource Manager-Vorlage in GitHub](create-host-pools-arm-template.md) einen Hostpool und Sitzungshost-VMs erstellt haben, können Sie dieselbe Vorlage aus dem Azure-Portal erneut bereitstellen. Bei einer erneuten Bereitstellung der Vorlage werden alle von Ihnen in der ursprünglichen Vorlage angegebenen Informationen erneut eingegeben, mit Ausnahme von Kennwörtern.

So stellen Sie die Azure Resource Manager-Vorlage erneut bereit, um einen Hostpool zu erweitern:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Suchen Sie in der Suchleiste oben im Azure-Portal nach **Ressourcengruppen**, und wählen Sie das Element unter **Dienste** aus.
3. Suchen Sie die Ressourcengruppe, die Sie beim Erstellen des Hostpools erstellt haben, und wählen Sie diese aus.
4. Wählen Sie im linken Bereich des Browsers die Option **Bereitstellungen** aus.
5. Wählen Sie die geeignete Bereitstellung für die Erstellung Ihres Hostpools aus:
     - Wenn Sie den ursprünglichen Hostpool mithilfe des Azure Marketplace-Angebots erstellt haben, wählen Sie die Bereitstellung aus, die mit **rds.wvd-provision-host-pool** beginnt.
     - Wenn Sie den ursprünglichen Hostpool mithilfe der Azure Resource Manager-Vorlage in GitHub erstellt haben, wählen Sie die Bereitstellung namens **Microsoft.Template** aus.
6. Wählen Sie **Erneut bereitstellen** aus.

     >[!NOTE]
     >Wenn die Vorlage bei Auswahl von **Erneut bereitstellen** nicht automatisch erneut bereitgestellt wird, klicken Sie im linken Bereich Ihres Browsers auf **Vorlage** und wählen **Bereitstellen** aus.

7. Wählen Sie die Ressourcengruppe aus, die die aktuellen Sitzungshost-VMs im vorhandenen Hostpool enthält.

     >[!NOTE]
     >Wenn Sie in einer Fehlermeldung aufgefordert werden, eine andere Ressourcengruppe auszuwählen, obwohl die von Ihnen eingegebene Gruppe richtig ist, wählen Sie zunächst eine andere Ressourcengruppe und danach die ursprüngliche Ressourcengruppe aus.

8. Geben Sie für *_artifactsLocation* folgende URL ein: `https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/`
9. Geben Sie unter *Remotedesktop-Sitzungshosts – Anzahl von Instanzen* die gewünschte neue Gesamtzahl von Sitzungshosts ein. Ein Beispiel: Wenn Sie Ihren Hostpool von fünf auf acht Sitzungshosts erweitern möchten, geben Sie **8** ein.
10. Geben Sie dasselbe vorhandene Domänenkennwort ein, dass Sie für den vorhandenen Benutzerprinzipalnamen in der Domäne verwendet haben. Ändern Sie den Benutzernamen nicht, da dies beim Ausführen der Vorlage zu einem Fehler führt.
11. Geben Sie dasselbe Kennwort für den Mandantenadministrator ein, das Sie für den Benutzernamen oder die Anwendungs-ID verwendet haben, den/die Sie unter *Mandantenadministrator-UPN oder Anwendungs-ID* eingegeben haben. Denken Sie daran: Ändern Sie den Benutzernamen nicht.
12. Schließen Sie die Übermittlung ab, um Ihren Hostpool zu erweitern.

## <a name="run-the-azure-marketplace-offering"></a>Ausführen des Azure Marketplace-Angebots

Befolgen Sie die Anweisungen unter [Erstellen eines Hostpools mit dem Azure Marketplace](create-host-pools-azure-marketplace-2019.md), bis Sie zu [Ausführen des Azure Marketplace-Angebots zum Bereitstellen eines neuen Hostpools](create-host-pools-azure-marketplace-2019.md#run-the-azure-marketplace-offering-to-provision-a-new-host-pool) gelangen. Dort müssen Sie auf den jeweiligen Registerkarten folgende Informationen eingeben:

### <a name="basics"></a>Grundlagen

Alle Werte in diesem Abschnitt müssen den Informationen entsprechen, die Sie beim ersten Erstellen des Hostpools und der Sitzungshost-VMs angegeben haben, mit Ausnahme von *Standarddesktopbenutzer*:

1.    Wählen Sie unter *Abonnement* das Abonnement aus, in dem Sie den Hostpool anfänglich erstellt haben.
2.    Wählen Sie als *Ressourcengruppe* die Ressourcengruppe aus, in der sich die Sitzungshost-VMs des vorhandenen Hostpools befinden.
3.    Wählen Sie unter *Region* die Region aus, in der sich die Sitzungshost-VMs des vorhandenen Hostpools befinden.
4.    Geben Sie unter *Hostpoolname* den Namen des vorhandenen Hostpools ein.
5.    Wählen Sie als *Desktoptyp* den Typ aus, der mit dem vorhandenen Hostpool übereinstimmt.
6.    Geben Sie für *Standarddesktopbenutzer* eine mit Trennzeichen getrennte Liste mit allen zusätzlichen Benutzern ein, denen Sie erlauben möchten, sich nach Abschluss des Azure Marketplace-Angebots bei Windows Virtual Desktop-Clients anzumelden und auf einen Desktop zuzugreifen. Geben Sie also beispielsweise „user3@contoso.com,user4@contoso.com“ ein, um user3@contoso.com und user4@contoso.com Zugriff zu gewähren.
7.    Wählen Sie **Weiter: Virtuellen Computer konfigurieren** aus.

>[!NOTE]
>Mit Ausnahme von *Standarddesktopbenutzer* müssen alle Felder exakt mit den Informationen übereinstimmen, die im vorhandenen Hostpool konfiguriert sind. Jegliche Abweichung führt zur Erstellung eines neuen Hostpools.

### <a name="configure-virtual-machines"></a>Konfigurieren virtueller Computer

Alle Parameterwerte in diesem Abschnitt müssen den Informationen entsprechen, die Sie beim ersten Erstellen des Hostpools und der Sitzungshost-VMs angegeben haben, mit Ausnahme der Gesamtzahl von VMs. Die Anzahl von VMs, die Sie hier eingeben, entspricht der Anzahl von VMs in Ihrem erweiterten Hostpool:

1. Wählen Sie die VM-Größe aus, die den vorhandenen Sitzungshost-VMs entspricht.

    >[!NOTE]
    >Wird die gewünschte VM-Größe nicht in der VM-Größenauswahl angezeigt, liegt das daran, dass sie noch nicht ins Azure Marketplace-Tool aufgenommen wurde. Wenn Sie eine VM-Größe anfordern möchten, erstellen Sie im [Windows Virtual Desktop-UserVoice-Forum](https://windowsvirtualdesktop.uservoice.com/forums/921118-general) eine Anforderung, oder stimmen Sie dort für eine vorhandene Anforderung ab.

2. Passen Sie die Parameter *Nutzungsprofil*, *Gesamtanzahl Benutzer* und *Anzahl von virtuellen Computern* an, um die Gesamtzahl von Sitzungshosts auszuwählen, die in Ihrem Hostpool enthalten sein sollen. Ein Beispiel: Wenn Sie Ihren Hostpool von fünf auf acht Sitzungshosts erweitern, konfigurieren Sie diese Optionen so, dass Sie acht virtuelle Computer erhalten.
3. Geben Sie ein Namenspräfix für die virtuellen Computer ein. Wenn Sie also beispielsweise „prefix“ eingeben, heißen die virtuellen Computer „prefix-0“, „prefix-1“ usw.
4. Wählen Sie **Weiter: Virtual machine settings** (Weiter: VM-Einstellungen) aus.

### <a name="virtual-machine-settings"></a>Einstellungen des virtuellen Computers

Alle Parameterwerte in diesem Abschnitt müssen den Informationen entsprechen, die Sie beim ersten Erstellen des Hostpools und der Sitzungshost-VMs angegeben haben:

1. Geben Sie für *Imagequelle* und *Betriebssystemversion des Images* dieselben Informationen ein, die Sie beim ersten Erstellen des Hostpools angegeben haben.
2. Geben Sie für *UPN für AD-Domänenbeitritt* und die zugehörigen Kennwörter dieselben Informationen ein, die Sie beim ersten Erstellen des Hostpools angegeben haben, um die VMs in die Active Directory-Domäne einzubinden. Mit diesen Anmeldeinformationen wird ein lokales Konto auf Ihren virtuellen Computern erstellt. Sie können diese lokalen Konten später zurücksetzen, um die Anmeldeinformationen zu ändern.
3. Wählen Sie dasselbe virtuelle Netzwerk und Subnetz aus, in dem sich die Sitzungshost-VMs des vorhandenen Hostpools befinden.
4. Wählen Sie **Weiter: Informationen zu Windows Virtual Desktop konfigurieren** aus.

### <a name="windows-virtual-desktop-information"></a>Informationen zu Windows Virtual Desktop

Alle Parameterwerte in diesem Abschnitt müssen den Informationen entsprechen, die Sie beim ersten Erstellen des Hostpools und der Sitzungshost-VMs angegeben haben:

1. Geben Sie in *Name der Windows Virtual Desktop-Mandantengruppe* den Namen für die Mandantengruppe ein, die Ihren Mandanten enthält. Lassen Sie den Standardnamen, es sei denn, Ihnen wurde ein spezieller Mandantengruppenname bereitgestellt.
2. Geben Sie für *Name des Windows Virtual Desktop-Mandanten* den Namen des Mandanten ein, in dem Sie diesen Hostpool erstellen möchten.
3. Geben Sie dieselben Anmeldeinformationen an, die Sie beim ersten Erstellen des Hostpools und der Sitzungshost-VMs verwendet haben. Wenn Sie einen Dienstprinzipal verwenden, geben Sie die ID der Azure Active Directory-Instanz ein, in der sich der Dienstprinzipal befindet.
4. Wählen Sie **Weiter: Überprüfen + erstellen**.

## <a name="run-the-github-azure-resource-manager-template"></a>Ausführen der Azure Resource Manager-Vorlage aus GitHub

Befolgen Sie die Anweisungen unter [Ausführen der Azure Resource Manager-Vorlage zum Bereitstellen eines neuen Hostpools](create-host-pools-arm-template.md#run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool), und geben Sie überall dieselben Parameterwerte an, mit Ausnahme von *Remotedesktop-Sitzungshosts – Anzahl von Instanzen*. Geben Sie die Anzahl von Sitzungshost-VMs an, die nach Ausführung der Vorlage im Hostpool vorhanden sein sollen. Ein Beispiel: Wenn Sie Ihren Hostpool von fünf auf acht Sitzungshosts erweitern möchten, geben Sie **8** ein.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren vorhandenen Hostpool erweitert haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um die Einstellungen im Rahmen einer Benutzersitzung zu testen. Sie können jeden der folgenden Clients verwenden, um eine Verbindung mit einer Sitzung herzustellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](connect-windows-7-10-2019.md)
- [Herstellen einer Verbindung mit dem Webclient](connect-web-2019.md)
- [Herstellen einer Verbindung mit dem Android-Client](connect-android-2019.md)
- [Herstellen einer Verbindung mit dem macOS-Client](connect-macos-2019.md)
- [Herstellen einer Verbindung mit dem iOS-Client](connect-ios-2019.md)
