---
title: Erstellen der technischen Ressourcen für Ihre Azure-VM
description: Hier erfahren Sie, wie Sie die technischen Ressourcen für ein VM-Angebot für Azure Marketplace erstellen und konfigurieren.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/13/2020
ms.openlocfilehash: 532a2ce429899d3674a8f6d8308188e9b1f737b1
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407947"
---
# <a name="create-your-azure-virtual-machine-technical-assets"></a>Erstellen der technischen Ressourcen für Ihre Azure-VM

In diesem Artikel wird beschrieben, wie Sie die technischen Ressourcen für ein VM-Angebot für Azure Marketplace erstellen und konfigurieren. Eine VM enthält zwei Komponenten: die virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Betriebssystem und optionale zugeordnete Datenträger-VHDs:

* **Betriebssystem-VHD**: Enthält das Betriebssystem und die Lösung, die mit Ihrem Angebot bereitgestellt wird. Der Prozess zur Vorbereitung der VHD unterscheidet sich abhängig davon, ob es sich um eine Linux-basierte, Windows-basierte oder benutzerdefinierte VM handelt.
* **Datenträger-VHDs**: Dedizierter, beständiger Speicher für eine VM. Verwenden Sie nicht die Betriebssystem-VHD (z. B. das Laufwerk C), um beständige Informationen zu speichern.

Ein VM-Image enthält einen Betriebssystemdatenträger und bis zu 16 Datenträger. Verwenden Sie eine VHD pro Datenträger, auch wenn der Datenträger leer ist.

> [!NOTE]
> Fügen Sie unabhängig vom verwendeten Betriebssystem immer nur die von der Lösung benötigte Mindestanzahl von Datenträgern hinzu. Kunden können Datenträger, die Teil eines Images sind, zum Zeitpunkt der Bereitstellung nicht entfernen. Sie haben aber immer die Möglichkeit, während oder nach der Bereitstellung Datenträger hinzuzufügen.

> [!IMPORTANT]
> Jedes VM-Image in einem Plan muss die gleiche Anzahl von Datenträgern aufweisen.

## <a name="fundamental-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure-Plattform und die Technologien erforderlich, die verwendet werden, um das Angebot zu erstellen. Zusätzlich zur Vertrautheit mit Ihrer Lösungsdomäne sollte sich Ihr Engineeringteam mit den folgenden Microsoft-Technologien auskennen:

* Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/)
* [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
* Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) und das [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking)
* Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* Erweiterte [JSON](https://www.json.org/)-Grundkenntnisse

## <a name="suggested-tools--optional"></a>Empfohlene Tools (optional)

Verwenden Sie ggf. eine der folgenden Skriptumgebungen für die Verwaltung von VMs und VHDs:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)
* [Azure-Befehlszeilenschnittstelle](https://code.visualstudio.com/)

Fügen Sie Ihrer Entwicklungsumgebung ggf. auch die folgenden Tools hinzu:

* [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Visual Studio Code](https://code.visualstudio.com/)
  * Erweiterung: [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  * Erweiterung: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  * Erweiterung: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Sehen Sie sich die verfügbaren Tools auf der Seite [Azure-Entwicklertools](https://azure.microsoft.com/product-categories/developer-tools/) und bei Verwendung von Visual Studio den [Visual Studio Marketplace](https://marketplace.visualstudio.com/) an.

## <a name="create-a-vm-image-using-an-approved-base"></a>Erstellen eines VM-Images anhand eines genehmigten Basisimages

> [!NOTE]
> Wenn Sie die technischen Ressourcen für Ihre VM anhand eines Images erstellen möchten, das Sie lokal erstellt haben, gehen Sie wie unter [Erstellen einer VM anhand Ihres eigenen Images](#create-a-vm-using-your-own-image) beschrieben vor.

In diesem Abschnitt werden verschiedene Aspekte der Verwendung eines genehmigten Basisimages beschrieben, z. B. die Verwendung des Remotedesktopprotokolls (RDP), die Auswahl einer Größe für die VM, die Installation der aktuellen Windows-Updates und die Generalisierung des VHD-Images.

In den folgenden Abschnitten geht es hauptsächlich um Windows-basierte VHDs. Weitere Informationen zum Erstellen von Linux-basierten VHDs finden Sie unter [Von Azure unterstützte Distributionen von Linux](../../virtual-machines/linux/endorsed-distros.md).

> [!WARNING]
> Folgen Sie der Anleitung in diesem Artikel, um mit Azure eine VM mit einem vorkonfigurierten, unterstützten Betriebssystem zu erstellen. Falls dies nicht mit Ihrer Lösung kompatibel ist, können Sie eine lokale VM mit einem genehmigten Betriebssystem erstellen und konfigurieren. Diesen können Sie für den Upload vorbereiten, wie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](../../virtual-machines/windows/prepare-for-upload-vhd-image.md) beschrieben.

### <a name="select-an-approved-base"></a>Auswählen eines genehmigten Basisimages

Wählen Sie entweder Windows oder Linux als Basisimage aus.

#### <a name="windows"></a>Windows

Die Betriebssystem-VHD für Ihr Windows-basiertes VM-Image muss auf einem von Azure genehmigten Basisimage basieren, das Windows Server oder SQL Server enthält. Erstellen Sie zunächst eine VM aus einem der folgenden Images im Azure-Portal:

* Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://www.microsoft.com/cloud-platform/windows-server-pricing), [2012 Datacenter](https://www.microsoft.com/cloud-platform/windows-server-pricing), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
* [SQL Server 2014](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md) (Enterprise, Standard, Web)
* [SQL Server 2012 SP2](../../virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md) (Enterprise, Standard, Web)

> [!NOTE]
> Wenn Sie das aktuelle Azure-Portal oder Azure PowerShell verwenden, sind ab dem 8. September 2014 veröffentlichte Windows Server-Images genehmigt.

#### <a name="linux"></a>Linux

Azure bietet eine Reihe von genehmigten Linux-Distributionen. Eine aktuelle Liste finden Sie unter [Linux auf von Azure unterstützten Distributionen](../../virtual-machines/linux/endorsed-distros.md).

### <a name="create-vm-in-the-azure-portal"></a>Erstellen der VM im Azure-Portal

Gehen Sie wie folgt vor, um das VM-Basisimage im [Azure-Portal](https://ms.portal.azure.com/) zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) mit dem Microsoft-Konto an, das dem Azure-Abonnement zugeordnet ist, das Sie zum Veröffentlichen Ihres VM-Angebots verwenden möchten.
2. Erstellen Sie eine neue Ressourcengruppe, und geben Sie den **Ressourcengruppennamen**, das **Abonnement** und den **Ressourcengruppenstandort** an. Ausführliche Informationen finden Sie unter [Verwalten von Ressourcen](../../azure-resource-manager/resource-group-portal.md).
3. Wählen Sie auf der linken Seite **Virtuelle Computer** aus, um die Seite mit den VM-Details anzuzeigen.
4. Wählen Sie **+ Hinzufügen** aus, um die Seite **Virtuellen Computer erstellen** zu öffnen.
5. Wählen Sie das Image in der Dropdownliste aus, oder klicken Sie auf **Alle öffentlichen und privaten Images durchsuchen**, um nach allen verfügbaren VM-Images zu suchen.
6. Wählen Sie anhand der folgenden Empfehlungen die Größe der VM aus, die Sie bereitstellen möchten:
    * Wenn Sie vorhaben, die VHD lokal zu entwickeln, spielt die Größe keine Rolle. Die Verwendung eines kleineren virtuellen Computers wird empfohlen.
    * Wenn Sie vorhaben, das Image in Azure zu entwickeln, sollten Sie eine der empfohlenen VM-Größen für das ausgewählte Image verwenden.

7. Erweitern Sie unter **Datenträger** den Abschnitt **Erweitert**, und legen Sie die Option **Verwaltete Datenträger verwenden** auf **Nein** fest.
8. Geben Sie die anderen Details ein, die zum Erstellen der VM erforderlich sind.
9. Wählen Sie **Bewerten + erstellen** aus, um Ihre Auswahl zu überprüfen. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Azure beginnt mit der Bereitstellung der von Ihnen angegebenen VM. Sie können den Fortschritt nachverfolgen, indem Sie auf der linken Seite die Registerkarte **Virtuelle Computer** auswählen. Nachdem die VM erstellt wurde, ändert sich der Status in **Wird ausgeführt**.

Falls beim Erstellen Ihrer neuen Azure-basierten VHD Probleme auftreten, lesen Sie den Artikel [Häufige Probleme bei der VHD-Erstellung (Häufig gestellte Fragen)](common-issues-during-vhd-creation.md).

### <a name="connect-to-your-azure-vm"></a>Herstellen einer Verbindung mit der Azure-VM

In diesem Abschnitt wird beschrieben, wie Sie eine Verbindung mit der in Azure erstellten VM herstellen und sich bei ihr anmelden. Nachdem Sie die Verbindung erfolgreich hergestellt haben, können Sie so mit der VM arbeiten, als wären Sie lokal auf dem Hostserver angemeldet.

#### <a name="connect-to-a-windows-based-vm"></a>Herstellen einer Verbindung mit einem Windows-basierten virtuellen Computer

Verwenden Sie den Remotedesktopclient, um eine Verbindung mit der in Azure gehosteten Windows-basierten VM herzustellen. Die meisten Versionen von Windows verfügen über native Unterstützung des Remotedesktopprotokolls (RDP). Weitere Informationen zu Clients für andere Betriebssysteme finden Sie unter [Remotedesktopclients](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

In diesem Artikel wird ausführlich beschrieben, wie Sie die integrierte Windows-RDP-Unterstützung verwenden, um eine Verbindung mit Ihrer VM herzustellen: [Gewusst wie: Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](../../virtual-machines/windows/connect-logon.md).

> [!TIP]
> Beim Herstellen der Verbindung werden möglicherweise Sicherheitswarnungen angezeigt, z. B. Warnungen mit dem Hinweis, dass die RDP-Datei von einem unbekannten Herausgeber stammt oder dass Ihre Benutzeranmeldeinformationen nicht verifiziert werden können. Sie können diese Warnungen ignorieren.

#### <a name="connect-to-a-linux-based-vm"></a>Herstellen einer Verbindung mit einem Linux-basierten virtuellen Computer

Zum Herstellen einer Verbindung mit einer Linux-basierten VM benötigen Sie einen SSH-Client (mit dem Secure Shell-Protokoll). In den folgenden Schritten wird das kostenlose SSH-Terminal [PuTTY](https://www.ssh.com/ssh/putty/) verwendet.

1. Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com/).
2. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.
3. Wählen Sie den virtuellen Computer aus, mit dem Sie eine Verbindung herstellen möchten.
4. Starten Sie die VM, falls sie noch nicht ausgeführt wird.
5. Wählen Sie den Namen der VM aus, um die zugehörige Seite **Übersicht** zu öffnen.
6. Notieren Sie sich die öffentliche IP-Adresse und den DNS-Namen Ihrer VM (wenn diese Werte nicht festgelegt sind, müssen Sie eine [Netzwerkschnittstelle erstellen](../../virtual-network/virtual-network-network-interface.md#create-a-network-interface)).
7. Öffnen Sie die Anwendung PuTTY.
8. Geben Sie im Dialogfeld für die PuTTY-Konfiguration die IP-Adresse oder den DNS-Namen Ihrer VM ein.

    :::image type="content" source="media/avm-putty.png" alt-text="Einstellungen für das PuTTY-Terminal. Die Felder für den Hostnamen (oder die IP-Adresse) und den Port sind hervorgehoben.":::

9. Wählen Sie **Öffnen** aus, um ein PuTTY-Terminal zu öffnen.
10. Geben Sie den Kontonamen und das Kennwort Ihres Linux-VM-Kontos ein, wenn Sie dazu aufgefordert werden.

Falls Verbindungsprobleme auftreten, hilft Ihnen die Dokumentation für Ihren SSH-Client weiter, beispielsweise [Kapitel 10 mit den häufigen Fehlermeldungen](https://www.ssh.com/ssh/putty/putty-manuals).

Ausführliche Informationen, z. B. zum Hinzufügen eines Desktops zu einer bereitgestellten Linux-VM, finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../../virtual-machines/linux/use-remote-desktop.md).

## <a name="create-a-vm-using-your-own-image"></a>Erstellen einer VM anhand Ihres eigenen Images

In diesem Abschnitt wird beschrieben, wie Sie ein vom Benutzer bereitgestelltes VM-Image erstellen und bereitstellen. Dazu können Sie Betriebssystem- und Datenträger-VHD-Images von einer in Azure bereitgestellten VHD bereitstellen.

> [!NOTE]
> Folgen Sie den Anweisungen unter [Erstellen eines VM-Images anhand eines genehmigten Basisimages](#create-a-vm-image-using-an-approved-base), um optional ein genehmigtes Basisimage zu verwenden.

1. Laden Sie Ihre Images in ein Azure-Speicherkonto hoch.
2. Stellen Sie das VM-Image bereit.
3. Erfassen Sie das VM-Image.

### <a name="upload-your-images-to-an-azure-storage-account"></a>Hochladen Ihrer Images in ein Azure-Speicherkonto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Laden Sie Ihre generalisierten Betriebssystem- und Datenträger-VHDs in Ihr Azure-Speicherkonto hoch.

### <a name="deploy-your-image"></a>Bereitstellen Ihres Images

Erstellen Sie Ihr Image im Azure-Portal oder mit Azure PowerShell.

#### <a name="deploy-using-the-azure-portal"></a>Bereitstellen über das Azure-Portal

1. Wählen Sie auf der Startseite **Ressource erstellen** aus, suchen Sie nach „Vorlagenbereitstellung“, und wählen Sie **Erstellen** aus.
2. Wählen Sie **Eigene Vorlage im Editor erstellen** aus.

    :::image type="content" source="media/avm-custom-deployment.png" alt-text="Seite „Benutzerdefinierte Bereitstellung“.":::

3. Fügen Sie diese [JSON-Vorlage](../partner-center-portal/azure-vm-image-certification.md) in den Editor ein, und wählen Sie **Speichern** aus.
4. Geben Sie die Parameterwerte für die angezeigten Eigenschaftenseiten für die **benutzerdefinierte Bereitstellung** ein.

    | Parameter | BESCHREIBUNG |
    | ------------ | ------------- |
    | Name des Speicherkontos des Benutzers | Inhalt aus Zelle 2 |
    | Name des Speichercontainers des Benutzers | Name des Speicherkontos, in dem sich die generalisierte VHD befindet |
    | DNS-Name für öffentliche IP-Adresse | DNS-Name der öffentlichen IP-Adresse. Definieren Sie nach der Bereitstellung des Angebots im Azure-Portal den DNS-Namen für die öffentliche IP-Adresse. |
    | Administratorbenutzername | Der Benutzername des Administratorkontos für die neue VM |
    | Administratorkennwort | Das Kennwort des Administratorkontos für die neue VM |
    | Betriebssystemtyp | VM-Betriebssystem: Windows oder Linux |
    | Abonnement-ID | Bezeichner des ausgewählten Abonnements |
    | Standort | Geografischer Standort der Bereitstellung |
    | Größe des virtuellen Computers | [Azure-VM-Größe](../../virtual-machines/windows/sizes.md), z. B. „Standard_A2“ |
    | Öffentliche IP-Adresse | Name der öffentlichen IP-Adresse |
    | VM-Name | Name des neuen virtuellen Computers |
    | Name des virtuellen Netzwerks | Name des virtuellen Netzwerks, das von der VM verwendet wird |
    | NIC-Name | Name der Netzwerkschnittstellenkarte, über die das virtuelle Netzwerk ausgeführt wird |
    | VHD-URL | Vollständige URL der VHD mit dem Betriebssystemdatenträger |
    |  |  |

5. Nachdem Sie diese Werte angegeben haben, wählen Sie **Kaufen** aus.

Azure startet die Bereitstellung. Im angegebenen Speicherkontopfad wird eine neue VM mit der angegebenen nicht verwalteten VHD erstellt. Sie können den Fortschritt im Azure-Portal nachverfolgen, indem Sie auf der linken Seite **Virtuelle Computer** auswählen. Nachdem die VM erstellt wurde, ändert sich der Status von „Wird gestartet“ in „Wird ausgeführt“.

#### <a name="deploy-using-azure-powershell"></a>Bereitstellen mit Azure PowerShell

```powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

### <a name="capture-the-vm-image"></a>Erfassen des VM-Images

Folgen Sie den Anweisungen, die Ihrem Ansatz entsprechen:

* Azure PowerShell: [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../../virtual-machines/windows/capture-image-resource.md)
* Azure CLI: [Vorgehensweise zum Erstellen eines Image von einem virtuellen Computer oder einer VHD](../../virtual-machines/linux/capture-image.md)
* API: [VMs – Erfassen](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

## <a name="configure-the-virtual-machine"></a>Konfigurieren der VM

In diesem Abschnitt wird beschrieben, wie Sie die Größe einer Azure-VM anpassen, sie aktualisieren und generalisieren. Diese Schritte sind erforderlich, um Ihre VM für die Bereitstellung in Azure Marketplace vorzubereiten.

### <a name="sizing-the-vhds"></a>Festlegen der Größe von VHDs

Wenn Sie eine der VMs ausgewählt haben, die mit einem Betriebssystem (und optional weiteren Diensten) vorkonfiguriert sind, verfügen Sie bereits über eine Azure-VM mit einer Standardgröße. Ein Starten Ihrer Lösung mit einem vorkonfigurierten Betriebssystem ist die empfohlene Vorgehensweise. Wenn Sie ein Betriebssystem aber manuell installieren, müssen Sie die Größe Ihrer primären VHD in Ihrem VM-Image festlegen:

* Für Windows sollte die Betriebssystem-VHD als VHD mit 127 bis 128 GB und festem Format erstellt werden.
* Für Linux sollte diese VHD als VHD mit 30 bis 50 GB und festem Format erstellt werden.

Beträgt die physische Größe weniger als 127 bis 128 GB, sollte die VHD erweiterbar sein (geringe Dichte/dynamisch). Die bereitgestellten Windows- und SQL Server-Images erfüllen diese Anforderungen. Ändern Sie daher weder das Format noch die Größe der VHD.

Datenträger können bis zu 1 TB groß sein. Bedenken Sie bei der Auswahl der Größe, dass Kunden die Größe der VHDs in einem Image bei der Bereitstellung nicht ändern können. Datenträger-VHDs sollten als VHDs mit festem Format erstellt werden. Sie sollten ebenfalls erweiterbar sein (geringe Dichte/dynamisch). Datenträger können anfänglich leer sein oder Daten enthalten.

### <a name="install-the-most-current-updates"></a>Installieren der aktuellsten Updates

Die Basisimages von Betriebssystem-VMs müssen die aktuellen Updates bis zum Veröffentlichungsdatum enthalten. Bevor Sie die von Ihnen erstellte Betriebssystem-VHD veröffentlichen, müssen Sie daher das Betriebssystem und alle installierten Dienste mit sämtlichen aktuellen Sicherheits- und Wartungspatches aktualisieren.

Führen Sie für Windows Server den Befehl **Nach Updates suchen** aus.

Für Linux-Distributionen werden Updates normalerweise mit einem Befehlszeilentool oder grafischen Hilfsprogramm heruntergeladen und installiert. Unter Ubuntu Linux können beispielsweise der Befehl [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) und das Tool [Update Manager](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) zum Aktualisieren des Betriebssystems verwendet werden.

### <a name="perform-additional-security-checks"></a>Durchführen von zusätzlichen Sicherheitsüberprüfungen

Stellen Sie sicher, dass Ihre Lösungsimages im Azure Marketplace über ein hohes Maß an Sicherheit verfügen. Im folgenden Artikel finden Sie eine Checkliste zu den Sicherheitskonfigurationen und -verfahren, die Ihnen dabei hilft: [Sicherheitsempfehlungen für Azure Marketplace-Images](../../security/security-recommendations-azure-marketplace-images.md). Einige dieser Empfehlungen gelten speziell für Linux-basierte Images, aber die meisten sind für beliebige VM-Images geeignet.

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Durchführen der benutzerdefinierten Konfiguration und von geplanten Aufgaben

Falls eine zusätzliche Konfiguration erforderlich ist, können Sie eine geplante Aufgabe einrichten, die beim Start ausgeführt wird. So ist es möglich, nach der Bereitstellung der VM letzte Änderungen vorzunehmen. Beachten Sie auch die folgenden Empfehlungen:

* Wenn es sich um eine Aufgabe mit einmaliger Ausführung handelt, sollte sie nach der erfolgreichen Ausführung automatisch gelöscht werden.
* Für Konfigurationen sollten keine anderen Laufwerke als C oder D verwendet werden, da nur für diese beiden Laufwerke garantiert ist, dass sie immer vorhanden sind (Laufwerk C ist der Betriebssystemdatenträger, und Laufwerk D ist der temporäre lokale Datenträger).

Weitere Informationen zu Linux-Anpassungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Linux](../../virtual-machines/extensions/features-linux.md).

## <a name="generalize-the-image"></a>Generalisieren des Images

Alle Images im Azure Marketplace müssen allgemein wiederverwendbar sein. Um dies zu erreichen, muss die Betriebssystem-VHD generalisiert werden. Dies ist ein Vorgang, bei dem alle instanzspezifischen Bezeichner und Softwaretreiber von einer VM entfernt werden.

### <a name="windows"></a>Windows

Windows-Betriebssystemdatenträger werden mit dem [Sysprep-Tool](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) generalisiert. Wenn Sie das Betriebssystem später dann aktualisieren oder neu konfigurieren, müssen Sie Sysprep erneut ausführen.

> [!WARNING]
> Fahren Sie die VM nach der Ausführung von Sysprep bis zur Bereitstellung herunter, da u. U. automatisch Updates ausgeführt werden. Dadurch verhindern Sie, dass durch nachfolgende Updates instanzspezifische Änderungen am Betriebssystem oder den installierten Diensten vorgenommen werden. Weitere Informationen zum Ausführen von Sysprep finden Sie unter [Schritte zum Generalisieren einer VHD](../../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep).

### <a name="linux"></a>Linux

Mit dem folgenden Prozess wird eine Linux-VM generalisiert und als separate VM erneut bereitgestellt. Ausführliche Informationen finden Sie unter [Vorgehensweise zum Erstellen eines Images von einem virtuellen Computer oder einer VHD](../../virtual-machines/linux/capture-image.md). Für Sie sind in diesem Fall nur die Informationen bis zum Abschnitt „Bereitstellen eines virtuellen Computers anhand des erfassten Images“ relevant.

1. **Entfernen des Azure Linux-Agents**

    1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrer Linux-VM her.
    2. Geben Sie im SSH-Fenster den folgenden Befehl ein: `sudo waagent -deprovision+user`
    3. Geben Sie **Y** ein, um fortzufahren (Sie können dem vorherigen Befehl den Parameter **-force** hinzufügen, um diesen Bestätigungsschritt zu umgehen).
    d. Geben Sie nach der Ausführung dieses Befehls **Exit** ein, um den SSH-Client zu schließen.

2. **Beenden der VM**

    1. Wählen Sie im Azure-Portal Ihre Ressourcengruppe (RG) aus, und heben Sie die Zuordnung der VM auf.
    2. Ihre VHD ist jetzt generalisiert, und Sie können damit eine neue VM erstellen.

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Erstellen Ihrer neuen Azure-basierten VHD Probleme auftreten, lesen Sie den Artikel [Häufige Probleme bei der VHD-Erstellung](common-issues-during-vhd-creation.md).

Andernfalls:

* In [Testen eines über eine VHD bereitgestellten virtuellen Computers (VM)](azure-vm-image-certification.md) wird erläutert, wie Sie ein VM-Image für die Azure Marketplace-Zertifizierung testen und übermitteln. Außerdem erfahren Sie, wo Sie das *Certification Test Tool for Azure Certified* erhalten und wie Sie es zum Zertifizieren Ihres VM-Images verwenden.
