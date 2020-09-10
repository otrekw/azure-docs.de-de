---
title: Erstellen der technischen Ressourcen für Azure-VMs
description: Hier erfahren Sie, wie Sie die technischen Ressourcen für ein VM-Angebot für Azure Marketplace erstellen und konfigurieren.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 08/14/2020
ms.openlocfilehash: 496cd4aeb96ca1849e950331658014d91dc6d6ba
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89143890"
---
# <a name="create-azure-virtual-machine-technical-assets"></a>Erstellen der technischen Ressourcen für Azure-VMs

Beim Veröffentlichen Ihrer Images virtueller Computer (VM) im Azure Marketplace überprüft das Azure-Team das VM-Image, um seine Startfähigkeit, Sicherheit und Kompatibilität mit Azure sicherzustellen. Wird einer der hochwertigen Tests nicht bestanden, schlägt die Veröffentlichung fehl, und eine Meldung mit dem Fehler und möglichen [Schritten zur Korrektur](https://docs.microsoft.com/azure/marketplace/partner-center-portal/vm-certification-issues-solutions) wird ausgegeben.

In diesem Artikel wird beschrieben, wie Sie die technischen Ressourcen für ein VM-Angebot für Azure Marketplace erstellen und konfigurieren. Eine VM enthält zwei Komponenten: die virtuelle Festplatte (Virtual Hard Disk, VHD) mit dem Betriebssystem und optionale zugeordnete Datenträger-VHDs:

1. **Betriebssystem-VHD**: Enthält das Betriebssystem und die Lösung, die mit Ihrem Angebot bereitgestellt wird. Der Prozess zur Vorbereitung der VHD unterscheidet sich in Abhängigkeit davon, ob es sich um eine Linux-basierte, Windows-basierte oder benutzerdefinierte VM handelt.

2. **Datenträger-VHDs**: Dedizierter, beständiger Speicher für eine VM. Verwenden Sie nicht die Betriebssystem-VHD (z. B. das Laufwerk C), um beständige Informationen zu speichern.

Ein VM-Image enthält einen Betriebssystemdatenträger und bis zu 16 Datenträger. Verwenden Sie eine VHD pro Datenträger, auch wenn der Datenträger leer ist.

> [!NOTE]
> Fügen Sie unabhängig vom verwendeten Betriebssystem immer nur die von der Lösung benötigte Mindestanzahl von Datenträgern hinzu. Kunden können Datenträger, die Teil eines Images sind, zum Zeitpunkt der Bereitstellung nicht entfernen. Sie haben aber immer die Möglichkeit, während oder nach der Bereitstellung Datenträger hinzuzufügen.

> [!IMPORTANT]
> Jedes VM-Image in einem Plan muss die gleiche Anzahl von Datenträgern aufweisen.

## <a name="fundamental-technical-knowledge"></a>Grundlegende technische Kenntnisse

Das Entwerfen, Erstellen und Testen dieser Ressourcen dauert lange, und es sind technische Kenntnisse in Bezug auf die Azure-Plattform und die Technologien erforderlich, die verwendet werden, um das Angebot zu erstellen. Zusätzlich zur Vertrautheit mit Ihrer Lösungsdomäne sollte sich Ihr Engineeringteam mit den folgenden Microsoft-Technologien auskennen:

- Grundkenntnisse in Bezug auf [Azure-Dienste](https://azure.microsoft.com/services/)
- [Entwerfen und Erstellen der Architektur für Azure-Anwendungen](https://azure.microsoft.com/solutions/architecture/)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) und [Azure-Netzwerk](https://azure.microsoft.com/services/?filter=networking)
- Erweiterte Grundkenntnisse in Bezug auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
- Erweiterte [JSON](https://www.json.org/)-Grundkenntnisse

### <a name="optional-suggested-tools"></a>Optionale empfohlene Tools

Verwenden Sie ggf. eine der folgenden Skriptumgebungen für die Verwaltung von VMs und VHDs:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [Azure-Befehlszeilenschnittstelle](https://code.visualstudio.com/)

Fügen Sie Ihrer Entwicklungsumgebung ggf. auch die folgenden Tools hinzu:

- [Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-vm-image-using-an-approved-base"></a>Erstellen eines VM-Images anhand eines genehmigten Basisimages

Wenn Sie die technischen Ressourcen für Ihre VM anhand eines Images erstellen möchten, das Sie lokal erstellt haben, finden Sie weitere Informationen in [Erstellen eines VM-Images anhand eines genehmigten Basisimages](#create-a-vm-image-using-an-approved-base) weiter unten.

In diesem Abschnitt werden verschiedene Aspekte der Verwendung eines genehmigten Basisimages beschrieben, z. B. die Verwendung des Remotedesktopprotokolls (RDP), die Auswahl einer Größe für die VM, die Installation der aktuellen Windows-Updates und die Generalisierung des VHD-Images.

Folgen Sie der Anleitung in diesem Artikel, um mit Azure eine VM mit einem vorkonfigurierten, unterstützten Betriebssystem zu erstellen. Falls dies nicht mit Ihrer Lösung kompatibel ist, können Sie eine lokale VM mit einem genehmigten Betriebssystem erstellen und konfigurieren. Diesen können Sie für den Upload vorbereiten, wie unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) beschrieben.

### <a name="select-an-approved-base-image"></a>Auswählen eines genehmigten Basisimages

Wählen Sie entweder Windows oder Linux als Basisimage aus.

Die Betriebssystem-VHD für Ihr VM-Image muss auf einem von Azure genehmigten Basisimage basieren, das Windows Server oder SQL Server enthält.

Wenn Sie eine Anforderung zur erneuten Veröffentlichung des Images mit Updates einreichen, schlägt der Testfall für die Überprüfung der Teilenummer möglicherweise fehl. In diesem Fall wird das Image nicht genehmigt.

Dieser Fehler tritt auf, wenn Sie ein Basisimage verwendet haben, das einem anderen Herausgeber gehört, und Sie haben das Image aktualisiert. In diesem Fall können Sie das Image nicht veröffentlichen.

Rufen Sie zum Beheben des Problems das aktuelle Image aus Azure Marketplace ab, und nehmen Sie Änderungen an diesem Image vor. Im Folgenden wird beschrieben, wie Sie genehmigte Basisimages nach Ihrem Image durchsuchen können:

#### <a name="windows"></a>Windows

- Windows Server ([2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 R2 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
- SQL Server 2019 ([Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview), [Standard](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview), [Web](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview))
- SQL Server 2014 ([Enterprise](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Standard](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Web](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance))
- SQL Server 2012 SP2 ([Enterprise](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Standard](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance), [Web](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance))

#### <a name="linux"></a>Linux

Azure bietet eine Reihe von genehmigten Linux-Distributionen. Eine aktuelle Liste finden Sie unter [Linux auf von Azure unterstützten Distributionen](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).

### <a name="create-vm-on-the-azure-portal"></a>Erstellen der VM im Azure-Portal

Gehen Sie wie folgt vor, um das VM-Basisimage im [Azure-Portal](https://ms.portal.azure.com/) zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) mit dem Microsoft-Konto an, das dem Azure-Abonnement zugeordnet ist, das Sie zum Veröffentlichen Ihres VM-Angebots verwenden möchten.
2. Erstellen Sie eine neue Ressourcengruppe, und geben Sie den **Ressourcengruppennamen**, das **Abonnement** und den **Ressourcengruppenstandort** an. Ausführliche Informationen finden Sie unter [Verwalten von Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

    :::image type="content" source="media/vm/create-resource-group.png" alt-text="Zeigt die ersten Schritte beim Erstellen einer Ressourcengruppe.":::

3. Wählen Sie im linken Navigationsbereich **Virtuelle Computer** aus, um die Seite mit den VM-Details anzuzeigen.
4. Wählen Sie **+ Hinzufügen** aus, um die Seite **Virtuellen Computer erstellen** zu öffnen.
5. Wählen Sie das Image in der Dropdownliste oder **Alle öffentlichen und privaten Images durchsuchen** aus, um nach allen verfügbaren VM-Images zu suchen. Beispiel:

    :::image type="content" source="media/vm/create-resource-group-example.png" alt-text="Zeigt ein Beispiel für ein VM-Image.":::

6. Wählen Sie anhand der folgenden Empfehlungen die Größe der VM aus, die Sie bereitstellen möchten:
    1. Wenn Sie vorhaben, die VHD lokal zu entwickeln, spielt die Größe keine Rolle. Die Verwendung eines kleineren virtuellen Computers wird empfohlen.
    2. Wenn Sie vorhaben, das Image in Azure zu entwickeln, sollten Sie eine der empfohlenen VM-Größen für das ausgewählte Image verwenden.

    :::image type="content" source="media/vm/create-virtual-machine.png" alt-text="Zeigt die Auswahl der VM-Größe.":::

7. Erweitern Sie unter **Datenträger** den Abschnitt **Erweitert**, und legen Sie die Option **Verwaltete Datenträger verwenden** auf **Nein** fest.

    :::image type="content" source="media/vm/use-managed-disks.png" alt-text="Zeigt eine Option zum Verwenden verwalteter Datenträger.":::

8. Geben Sie die anderen Details ein, die zum Erstellen der VM erforderlich sind.
9. Wählen Sie **Bewerten + erstellen** aus, um Ihre Auswahl zu überprüfen. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Azure beginnt mit der Bereitstellung der von Ihnen angegebenen VM. Sie können den Fortschritt nachverfolgen, indem Sie auf der linken Seite die Registerkarte **Virtuelle Computer** auswählen. Nachdem die VM erstellt wurde, ändert sich der Status in **Wird ausgeführt**.

### <a name="create-a-generation-2-vm-on-the-azure-portal"></a>Erstellen einer VM der 2. Generation im Azure-Portal

Erstellen Sie eine VM der 2. Generation (Gen2) im Azure-Portal.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com/) beim Azure-Portal an.
2. Wählen Sie **Ressource erstellen**.
3. Wählen Sie im Azure Marketplace links **Alle anzeigen** aus.
4. Wählen Sie ein Image aus, das Gen2 unterstützt.
5. Klicken Sie auf **Erstellen**.
6. Wählen Sie auf der Registerkarte **Erweitert** im Abschnitt **VM-Generation** die Option **Gen 2** aus.
7. Wechseln Sie auf der Registerkarte **Grundlagen** unter **Instanzdetails** zu **Größe**, und öffnen Sie das Blatt **VM-Größe auswählen**.
8. Wählen Sie eine empfohlene Größe der [unterstützten VMs der Generation 2](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2#generation-2-vm-sizes) und eine Größe aus.
9. Durchlaufen Sie den [Erstellungsflow im Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal), um die Erstellung des virtuellen Computers abzuschließen.

    :::image type="content" source="media/vm/vm-generation.png" alt-text="Zeigt die Option zum Auswählen der VM-Generation.":::

## <a name="connect-to-your-azure-vm"></a>Herstellen einer Verbindung mit der Azure-VM

In diesem Abschnitt wird beschrieben, wie Sie eine Verbindung mit der in Azure erstellten VM herstellen und sich bei ihr anmelden. Nachdem Sie die Verbindung erfolgreich hergestellt haben, können Sie so mit der VM arbeiten, als wären Sie lokal auf dem Hostserver angemeldet.

### <a name="connect-to-a-windows-based-vm"></a>Herstellen einer Verbindung mit einem Windows-basierten virtuellen Computer

Verwenden Sie den Remotedesktopclient, um eine Verbindung mit der in Azure gehosteten Windows-basierten VM herzustellen. Die meisten Versionen von Windows verfügen über native Unterstützung des Remotedesktopprotokolls (RDP). Weitere Informationen zu Clients für andere Betriebssysteme finden Sie unter [Remotedesktopclients](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

In diesem Artikel wird ausführlich beschrieben, wie Sie die integrierte Windows-RDP-Unterstützung verwenden, um eine Verbindung mit Ihrer VM herzustellen: [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden bei diesem Computer](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

> [!TIP]
> Beim Herstellen der Verbindung werden möglicherweise Sicherheitswarnungen angezeigt, z. B. Warnungen mit dem Hinweis, dass die RDP-Datei von einem unbekannten Herausgeber stammt oder dass Ihre Benutzeranmeldeinformationen nicht verifiziert werden können. Sie können diese Warnungen ignorieren.

### <a name="connect-to-a-linux-based-vm"></a>Herstellen einer Verbindung mit einem Linux-basierten virtuellen Computer

Zum Herstellen einer Verbindung mit einer Linux-basierten VM benötigen Sie einen SSH-Client (mit dem Secure Shell-Protokoll). In den folgenden Schritten wird das kostenlose SSH-Terminal [PuTTY](https://www.ssh.com/ssh/putty/) verwendet.

1. Öffnen Sie das [Azure-Portal](https://ms.portal.azure.com/).
2. Suchen Sie nach Virtuelle Computer, und wählen Sie diese Option aus.
3. Wählen Sie den virtuellen Computer aus, mit dem Sie eine Verbindung herstellen möchten.
4. Starten Sie die VM, falls sie noch nicht ausgeführt wird.
5. Wählen Sie den Namen der VM aus, um die zugehörige Seite Übersicht zu öffnen.
6. Notieren Sie sich die öffentliche IP-Adresse und den DNS-Namen Ihrer VM (wenn diese Werte nicht festgelegt sind, müssen Sie eine [Netzwerkschnittstelle erstellen](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#create-a-network-interface)).
7. Öffnen Sie die Anwendung PuTTY.
8. Geben Sie im Dialogfeld für die PuTTY-Konfiguration die IP-Adresse oder den DNS-Namen Ihrer VM ein.

    :::image type="content" source="media/vm/putty-configuration.png" alt-text="Veranschaulicht die Einstellungen für das PuTTY-Terminal. Die Felder für den Hostnamen und den Port sind hervorgehoben.":::

9. Wählen Sie **Öffnen** aus, um ein PuTTY-Terminal zu öffnen.
10. Geben Sie den Kontonamen und das Kennwort Ihres Linux-VM-Kontos ein, wenn Sie dazu aufgefordert werden.

## <a name="configure-the-virtual-machine"></a>Konfigurieren der VM

In diesem Abschnitt wird beschrieben, wie Sie die Größe einer Azure-VM anpassen, sie aktualisieren und generalisieren. Diese Schritte sind erforderlich, um Ihre VM für die Bereitstellung in Azure Marketplace vorzubereiten.

### <a name="sizing-the-vhds"></a>Festlegen der Größe von VHDs

Die folgenden Regeln gelten für Einschränkungen der Betriebssystem-Datenträgergröße. Stellen Sie beim Senden von Anforderungen sicher, dass die Betriebssystem-Datenträgergröße innerhalb der Grenzen für Linux bzw. Windows liegt.

| OS | Empfohlene VHD-Größe |
| --- | --- |
| Linux | 30 bis 1023 GB |
| Windows | 30 bis 250 GB |

Da VMs den Zugriff auf das zugrunde liegende Betriebssystem zulassen, vergewissern Sie sich, dass die Größe der VHD ausreichend ist. Da Datenträger nicht ohne Downtime erweitert werden können, sollten Sie eine Datenträgergröße zwischen 30 und 50&nbsp;GB verwenden.

| VHD-Größe | Tatsächlich belegte Größe | Lösung |
| --- | --- | --- |
| > 500 TB | – | Wenden Sie sich für eine Ausnahmegenehmigung an das Supportteam. |
| 250–500 TB | > 200 GB Unterschied zur Blobgröße | Wenden Sie sich für eine Ausnahmegenehmigung an das Supportteam. |

### <a name="install-the-most-current-updates"></a>Installieren der aktuellsten Updates

Die Basisimages von Betriebssystem-VMs müssen die aktuellen Updates bis zum Veröffentlichungsdatum enthalten. Bevor Sie die von Ihnen erstellte Betriebssystem-VHD veröffentlichen, müssen Sie daher das Betriebssystem und alle installierten Dienste mit sämtlichen aktuellen Sicherheits- und Wartungspatches aktualisieren.

- Führen Sie für Windows Server den Befehl Nach Updates suchen aus.
- Für Linux-Distributionen werden Updates normalerweise mit einem Befehlszeilentool oder grafischen Hilfsprogramm heruntergeladen und installiert. Unter Ubuntu Linux können beispielsweise der Befehl [apt-get](https://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) und das Tool [Update Manager](https://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) zum Aktualisieren des Betriebssystems verwendet werden.

#### <a name="perform-additional-security-checks"></a>Durchführen von zusätzlichen Sicherheitsüberprüfungen

Stellen Sie sicher, dass Ihre Lösungsimages im Azure Marketplace über ein hohes Maß an Sicherheit verfügen. Eine Checkliste zu den Sicherheitskonfigurationen und -verfahren finden Sie unter [Sicherheitsempfehlungen für Azure Marketplace-Images](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images). Einige dieser Empfehlungen gelten speziell für Linux-basierte Images, aber die meisten sind für beliebige VM-Images geeignet.

#### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Durchführen der benutzerdefinierten Konfiguration und von geplanten Aufgaben

Falls eine zusätzliche Konfiguration erforderlich ist, können Sie eine geplante Aufgabe einrichten, die beim Start ausgeführt wird. So ist es möglich, nach der Bereitstellung der VM letzte Änderungen vorzunehmen. Berücksichtigen Sie außerdem folgende Punkte:

- Wenn es sich um eine Aufgabe mit einmaliger Ausführung handelt, sollte sie nach der erfolgreichen Ausführung automatisch gelöscht werden.
- Für Konfigurationen sollten keine anderen Laufwerke als C oder D verwendet werden, da nur für diese beiden Laufwerke garantiert ist, dass sie immer vorhanden sind (Laufwerk C ist der Betriebssystemdatenträger, und Laufwerk D ist der temporäre lokale Datenträger).

Weitere Informationen zu Linux-Anpassungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux).

## <a name="generalize-the-image"></a>Generalisieren des Images

Alle Images im Azure Marketplace müssen allgemein wiederverwendbar sein. Um dies zu erreichen, muss die Betriebssystem-VHD generalisiert werden. Dies ist ein Vorgang, bei dem alle instanzspezifischen Bezeichner und Softwaretreiber von einer VM entfernt werden.

### <a name="for-windows"></a>Für Windows

Windows-Betriebssystemdatenträger werden mit dem [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)-Tool generalisiert. Wenn Sie das Betriebssystem später aktualisieren oder neu konfigurieren, müssen Sie Sysprep erneut ausführen.

> [!WARNING]
> Fahren Sie die VM nach der Ausführung von Sysprep bis zur Bereitstellung herunter, da u. U. automatische Updates ausgeführt werden. Dadurch verhindern Sie, dass durch nachfolgende Updates instanzspezifische Änderungen am Betriebssystem oder den installierten Diensten vorgenommen werden. Weitere Informationen zum Ausführen von Sysprep finden Sie unter [Schritte zum Generalisieren einer VHD](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource#generalize-the-windows-vm-using-sysprep).

### <a name="for-linux"></a>Für Linux

Mit dem folgenden Prozess wird eine Linux-VM generalisiert und als separate VM erneut bereitgestellt. Ausführliche Informationen finden Sie unter [Vorgehensweise zum Erstellen eines Images von einem virtuellen Computer oder einer VHD](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image). Für Sie sind in diesem Fall nur die Informationen bis zum Abschnitt „Bereitstellen eines virtuellen Computers anhand des erfassten Images“ relevant.

1. Entfernen des Azure Linux-Agents

    1. Stellen Sie mit einem SSH-Client eine Verbindung mit Ihrer Linux-VM her.
    2. Geben Sie im SSH-Fenster den folgenden Befehl ein: `sudo waagent –deprovision+user`
    3. Geben Sie Y ein, um fortzufahren (Sie können dem vorherigen Befehl den Parameter -force hinzufügen, um diesen Bestätigungsschritt zu umgehen).
    4. Geben Sie nach der Ausführung dieses Befehls Exit ein, um den SSH-Client zu schließen.

2. Beenden des virtuellen Computers

    1. Wählen Sie im Azure-Portal Ihre Ressourcengruppe (RG) aus, und heben Sie die Zuordnung der VM auf.
    2. Ihre VHD ist jetzt generalisiert, und Sie können damit eine neue VM erstellen.

## <a name="next-steps"></a>Nächste Schritte

- Wenn beim Erstellen Ihrer neuen Azure-basierten VHD Probleme auftreten, lesen Sie den Artikel [Häufige Probleme bei der VHD-Erstellung](common-issues-during-vhd-creation.md).
- Falls nicht, wird in [Testen eines über eine VHD bereitgestellten virtuellen Computers (VM)](azure-vm-image-certification.md) erläutert, wie Sie ein VM-Image für die Azure Marketplace-Zertifizierung testen und übermitteln. Außerdem erfahren Sie, wo Sie das Certification Test Tool for Azure Certified erhalten und wie Sie es zum Zertifizieren Ihres VM-Images verwenden.
