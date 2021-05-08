---
title: Verwenden von DFS-N mit Azure Files
description: Allgemeine DFS-N-Anwendungsfälle mit Azure Files
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 3/02/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: f571fa6e04d19412db856a42232cb2d151ceb6ab
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108141485"
---
# <a name="how-to-use-dfs-namespaces-with-azure-files"></a>Verwenden von DFS-Namespaces mit Azure Files
[DFS-Namespaces (Distributed File Systems Namespaces)](/windows-server/storage/dfs-namespaces/dfs-overview) oder DFS-N bezeichnet eine Windows Server-Serverrolle, die vielfach verwendet wird, um die Bereitstellung und Wartung von SMB-Dateifreigaben in der Produktion zu vereinfachen. DFS-Namespaces ist eine Technologie für die Virtualisierung von Speichernamespaces, mit deren Hilfe Sie eine indirekte Ebene zwischen dem UNC-Pfad Ihrer Dateifreigaben und den eigentlichen Dateifreigaben bereitstellen können. DFS-Namespaces funktionieren mit SMB-Dateifreigaben, wobei es keine Rolle spielt, wo diese Dateifreigaben gehostet werden: Sie können SMB-Freigaben verwenden, die auf einem lokalen Windows-Dateiserver mit oder ohne Azure-Dateisynchronisierung gehostet werden, direkte Azure-Dateifreigaben, SMB-Dateifreigaben mit Hosting in Azure NetApp Files oder anderen Drittanbieterangeboten oder sogar Dateifreigaben, die in anderen Clouds gehostet werden. 

Im Kern bietet DFS-Namespaces eine Zuordnung zwischen einem benutzerfreundlichen UNC-Pfad wie `\\contoso\shares\ProjectX` und dem zugrunde liegenden UNC-Pfad der SMB-Freigabe, z. B. `\\Server01-Prod\ProjectX` oder `\\storageaccount.file.core.windows.net\projectx`. Wenn der Endbenutzer zu seiner Dateifreigabe navigieren möchte, gibt er den benutzerfreundlichen UNC-Pfad ein, aber der zugehörige SMB-Client greift auf den zugrunde liegenden SMB-Pfad der Zuordnung zu. Es ist zudem möglich, dieses grundlegende Konzept zu erweitern, um einen Namen eines vorhandenen Dateiservers wie beispielsweise `\\MyServer\ProjectX` zu übernehmen. Sie können diese Funktion verwenden, um die folgenden Szenarien zu unterstützen:

- Geben Sie einen migrationssicheren Namen für einen logischen Satz von Daten an. In diesem Beispiel verfügen Sie über eine Zuordnung wie `\\contoso\shares\Engineering`, die `\\OldServer\Engineering` zugeordnet ist. Wenn Sie die Migration zu Azure Files durchführen, können Sie Ihre Zuordnung ändern, damit Ihr benutzerfreundlicher UNC-Pfad auf `\\storageaccount.file.core.windows.net\engineering` zeigt. Wenn ein Endbenutzer auf den benutzerfreundlichen UNC-Pfad zugreift, wird er nahtlos an den Azure-Dateifreigabepfad umgeleitet.

- Richten Sie einen allgemeinen Namen für einen logischen Satz von Daten ein, der auf mehrere Server an verschiedenen physischen Standorten verteilt wird, etwa über die Azure-Dateisynchronisierung. In diesem Beispiel wird ein Name wie `\\contoso\shares\FileSyncExample` mehreren UNC-Pfaden wie beispielsweise `\\FileSyncServer1\ExampleShare`, `\\FileSyncServer2\DifferentShareName`, `\\FileSyncServer3\ExampleShare` zugeordnet. Wenn der Benutzer auf den benutzerfreundlichen UNC-Pfad zugreift, wird ihm eine Liste möglicher UNC-Pfade angezeigt, und er kann basierend auf Windows Server Active Directory-Standortdefinitionen (AD) den nächstgelegenen Standort auswählen.

- Erweitern Sie einen logischen Satz von Daten über Größen-, E/A- oder andere Schwellenwerte für die Skalierung hinweg. Dies ist häufig bei Benutzerverzeichnissen der Fall, bei denen jeder Benutzer einen eigenen Ordner auf einer Freigabe erhält, oder bei temporären Freigaben, bei denen Benutzer beliebigen Speicherplatz erhalten, um temporäre Datenanforderungen zu erfüllen. Mit DFS-Namespaces werden mehrere Ordner zu einem kohäsiven Namespace miteinander verbunden. Beispielsweise wird `\\contoso\shares\UserShares\user1` zu `\\storageaccount.file.core.windows.net\user1` zugeordnet, `\\contoso\shares\UserShares\user2` zu `\\storageaccount.file.core.windows.net\user2` usw.  

Ein Beispiel für die Verwendung von DFS-Namespaces mit Ihrer Azure Files-Bereitstellung finden Sie in der folgenden Videoübersicht.

[![Demo zur Einrichtung von DFS-N mit Azure Files: Klicken Sie, um das Video abzuspielen!](./media/files-manage-namespaces/video-snapshot-dfsn.png)](https://www.youtube.com/watch?v=jd49W33DxkQ)
> [!NOTE]  
> Wechseln Sie zu Position 10:10 im Video, um zu erfahren, wie DFS-Namespaces eingerichtet wird.

Wenn Sie bereits über DFS-Namespace verfügen, sind für die Verwendung mit Azure Files und die Dateisynchronisierung keine besonderen Schritte erforderlich. Wenn Sie lokal auf Ihre Azure-Dateifreigabe zugreifen, gelten die üblichen Überlegungen zum Netzwerkbetrieb. Weitere Informationen finden Sie unter [Azure Files – Überlegungen zum Netzwerkbetrieb](./storage-files-networking-overview.md).

## <a name="namespace-types"></a>Namespacetypen
DFS-Namespaces bietet zwei grundlegende Arten von Namespaces:

- **Domänenbasierter Namespace**: Ein Namespace, der als Teil Ihrer Windows Server AD-Domäne gehostet wird. Namespaces, die als Teil von AD gehostet werden, umfassen einen UNC-Pfad, der den Namen Ihrer Domäne enthält. Dieser lautet z. B. `\\contoso.com\shares\myshare`, wenn Ihre Domäne `contoso.com` heißt. Domänenbasierte Namespaces unterstützen höhere Skalierungsgrenzwerte und integrierte Redundanz über AD. Es darf sich nicht um eine Clusterressource in einem Failovercluster handeln. 
- **Eigenständiger Namespace**: Ein Namespace, der nicht als Teil der Windows Server AD-Instanz, sondern auf einem einzelnen Server gehostet wird. Der Name eines eigenständige Namespaces basiert auf dem Namen des eigenständigen Servers. Er kann z. B. `\\MyStandaloneServer\shares\myshare` lauten, wenn Ihr eigenständiger Server den Namen `MyStandaloneServer` hat. Eigenständige Namespaces unterstützen niedrigere Skalierungsziele als domänenbasierte Namespaces, können aber als Clusterressource in einem Failovercluster gehostet werden.

## <a name="requirements"></a>Anforderungen
Um DFS-Namespaces mit Azure Files und Dateisynchronisierung verwenden zu können, benötigen Sie die folgenden Ressourcen:

- Eine Active Directory-Domäne. Diese kann an einem beliebigen Ort gehostet werden, z. B. lokal, auf einer Azure-VM oder sogar in einer anderen Cloud.
- Eine Windows Server-Instanz, die den Namespace hosten kann. Ein gängiges Muster für die Bereitstellung von DFS-Namespaces ist die Verwendung des Active Directory-Domänencontrollers zum Hosten der Namespaces. Die Namespaces können jedoch von jedem Server mit installierter DFS-Namespaces-Serverrolle eingerichtet werden. DFS-Namespaces ist für alle unterstützten Windows Server-Versionen verfügbar.
- Eine SMB-Dateifreigabe, die in einer Umgebung mit Domäneneinbindung gehostet wird, z. B. eine Azure-Dateifreigabe, die über ein in die Domäne eingebundenes Speicherkonto gehostet wird, oder eine Dateifreigabe, die auf einem in die Domäne eingebundenen Windows-Dateiserver mithilfe der Azure-Dateisynchronisierung gehostet wird. Weitere Informationen zum Domänenbeitritt für Ihr Speicherkonto finden Sie im Artikel über die [identitätsbasierte Authentifizierung](storage-files-active-directory-overview.md). Windows-Dateiserver werden in gleicher Weise in die Domäne eingebunden, unabhängig von der Verwendung der Azure-Dateisynchronisierung.
- Die SMB-Dateifreigaben, die Sie mit DFS-Namespaces verwenden möchten, müssen über Ihre lokalen Netzwerke erreichbar sein. Dies ist in erster Linie für Azure-Dateifreigaben von Bedeutung, gilt aber technisch gesehen für alle Dateifreigaben, die in Azure oder einer anderen Cloud gehostet werden. Weitere Informationen finden Sie unter [Azure Files – Überlegungen zum Netzwerkbetrieb](storage-files-networking-overview.md).

## <a name="install-the-dfs-namespaces-server-role"></a>Installieren der DFS-Namespaces-Serverrolle
Wenn Sie bereits DFS-Namespaces verwenden oder DFS-Namespaces auf Ihrem Domänencontroller einrichten möchten, können Sie diese Schritte überspringen.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Öffnen Sie den Server-Manager auf Ihrem Server, um die DFS-Namespaces-Serverrolle zu installieren. Klicken Sie auf **Verwalten**, und wählen Sie anschließend **Rollen und Features hinzufügen** aus. Der daraufhin geöffnete Assistent führt Sie durch die Installation der erforderlichen Windows-Komponenten zum Ausführen und Verwalten von DFS-Namespaces. 

Aktivieren Sie im Abschnitt **Installationstyp** des Installations-Assistenten das Kontrollkästchen **Rollen- oder featurebasierte Installation**, und klicken Sie auf **Weiter**. Wählen Sie im Abschnitt **Serverauswahl** die gewünschten Server aus, auf denen Sie die DFS-Namespaces-Serverrolle installieren möchten, und klicken Sie auf **Weiter**. 

Aktivieren Sie im Abschnitt **Serverrollen** in der Rollenliste die Rolle **DFS-Namespaces**. Sie finden diese Einstellung unter **Datei- und Speicherdienste** > **Datei- und iSCSI-Dienste**. Wenn Sie die DFS-Namespaces-Serverrolle auswählen, werden möglicherweise weitere erforderliche unterstützende Serverrollen oder Features hinzugefügt, die Sie noch nicht installiert haben.

![Screenshot: Assistent **Rollen und Features hinzufügen** mit ausgewählter Rolle **DFS-Namespaces**](./media/files-manage-namespaces/dfs-namespaces-install.png)

Nachdem Sie die Rolle **DFS-Namespaces** aktiviert haben, können Sie auf allen nachfolgenden Bildschirmen auf **Weiter** klicken und **Installieren** auswählen, sobald der Assistent die Schaltfläche aktiviert. Wenn die Installation abgeschlossen ist, können Sie Ihren Namespace konfigurieren.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Führen Sie in einer PowerShell-Sitzung mit erhöhten Rechten (oder mithilfe von PowerShell-Remoting) die folgenden Befehle aus.

```PowerShell
Install-WindowsFeature -Name "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```
---

## <a name="take-over-existing-server-names-with-root-consolidation"></a>Übernehmen vorhandener Servernamen mit Stammkonsolidierung
Eine wichtige Verwendung für DFS-Namespaces besteht darin, den Namen eines vorhandenen Servers zu übernehmen, um die physische Anordnung der Dateifreigaben anzupassen. Angenommen, Sie möchten im Rahmen einer Modernisierungsmigration die Dateifreigaben mehrerer alter Dateiserver auf einem einzelnen Dateiserver konsolidieren. Traditionell werden die Möglichkeiten für die Konsolidierung von Dateifreigaben von unterschiedlichen Dateiservern auf einem Host durch Faktoren wie Benutzerfreundlichkeit und Limits für die Dokumentverknüpfung eingeschränkt. Dank der DFS-Namespaces-Funktion für die Stammkonsolidierung können Sie jedoch einen einzelnen Server für mehrere Servernamen einrichten und an den entsprechenden Freigabenamen umleiten.

Die Stammkonsolidierung eignet sich für verschiedene Szenarien für die Rechenzentrumsmigration, ist aber aus den folgenden Gründen insbesondere für die Einführung cloudnativer Azure-Dateifreigaben von Vorteil:

- Azure-Dateifreigaben ermöglichen es Ihnen nicht, vorhandene lokale Servernamen beizubehalten.
- Der Zugriff auf Azure-Dateifreigaben muss über den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Speicherkontos erfolgen. Beispielsweise wird auf eine Azure-Dateifreigabe mit dem Namen `share` im Speicherkonto `storageaccount` immer über den UNC-Pfad `\\storageaccount.file.core.windows.net\share` zugegriffen. Dies kann für Endbenutzer verwirrend sein, die einen Kurznamen wie etwa `\\MyServer\share` oder als Namen eine Unterdomäne des Domänennamens der Organisation erwarten (z. B. `\\MyServer.contoso.com\share`).

Die Stammkonsolidierung kann nur mit eigenständigen Namespaces verwendet werden. Wenn Sie bereits über einen domänenbasierten Namespace für Ihre Dateifreigaben verfügen, müssen Sie keinen konsolidierten Stammnamespace erstellen.

### <a name="enabling-root-consolidation"></a>Aktivieren der Stammkonsolidierung
Die Stammkonsolidierung kann durch Festlegen der folgenden Registrierungsschlüssel aus einer PowerShell-Sitzung mit erhöhten Rechten (oder mithilfe von PowerShell-Remoting) aktiviert werden.

```PowerShell
New-Item `
    -Path "HKLM:SYSTEM\CurrentControlSet\Services\Dfs" `
    -Type Registry `
    -ErrorAction SilentlyContinue
New-Item `
    -Path "HKLM:SYSTEM\CurrentControlSet\Services\Dfs\Parameters" `
    -Type Registry `
    -ErrorAction SilentlyContinue
New-Item `
    -Path "HKLM:SYSTEM\CurrentControlSet\Services\Dfs\Parameters\Replicated" `
    -Type Registry `
    -ErrorAction SilentlyContinue
Set-ItemProperty `
    -Path "HKLM:SYSTEM\CurrentControlSet\Services\Dfs\Parameters\Replicated" `
    -Name "ServerConsolidationRetry" `
    -Value 1
```

### <a name="creating-dns-entries-for-existing-file-server-names"></a>Erstellen von DNS-Einträgen für vorhandene Dateiservernamen
Damit DFS-Namespaces auf vorhandene Dateiservernamen antworten können, erstellen Sie Aliaseinträge (CNAME) für Ihre vorhandenen Dateiserver, die auf den Servernamen der DFS-Namespaces verweisen. Das genaue Verfahren zum Aktualisieren Ihrer DNS-Einträge hängt davon ab, welche Server Ihre Organisation verwendet und ob Ihre Organisation benutzerdefinierte Tools verwendet, um die DNS-Verwaltung zu automatisieren. Die folgenden Schritte gelten für den DNS-Server, der in Windows Server enthalten ist und automatisch von Windows AD verwendet wird.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Öffnen Sie auf einem Windows DNS-Server die DNS-Verwaltungskonsole. Klicken Sie hierzu auf die Schaltfläche **Start**, und geben Sie **DNS** ein. Navigieren Sie zur Forward-Lookupzone für Ihre Domäne. Wenn Ihre Domäne beispielsweise `contoso.com` lautet, befindet sich die Forward-Lookupzone in der Verwaltungskonsole unter **Forward-Lookupzonen** >  **`contoso.com`** . Die genaue Hierarchie, die in diesem Dialogfeld angezeigt wird, hängt von der DNS-Konfiguration für Ihr Netzwerk ab.

Klicken Sie mit der rechten Maustaste auf Ihre Forward-Lookupzone, und wählen Sie **Neuer Alias (CNAME)** aus. Geben Sie im nun angezeigten Dialogfeld den Kurznamen für den Dateiserver ein, den Sie ersetzen. Der FQDN wird automatisch in das Textfeld **Vollqualifizierter Domänenname** eingefügt. Geben Sie im Textfeld **Vollqualifizierter Domänenname (FQDN) für Zielhost** den Namen des eingerichteten DFS-N-Servers ein. Sie können die Schaltfläche **Durchsuchen** verwenden, um bei Bedarf nach dem Server zu suchen. Klicken Sie auf **OK**, um den CNAME-Eintrag für Ihren Server zu erstellen.

![Screenshot: **Neuer Ressourceneintrag** für einen CNAME-DNS-Eintrag](./media/files-manage-namespaces/root-consolidation-cname.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Öffnen Sie auf einem Windows DNS-Server eine PowerShell-Sitzung (oder verwenden Sie PowerShell-Remoting), um die folgenden Befehle auszuführen. Geben Sie für `$oldServer` und `$dfsnServer` die relevanten Werte für Ihre Umgebung ein. `$domain` wird automatisch mit dem Domänennamen aufgefüllt, aber Sie können den Wert auch manuell eingeben.

```PowerShell
# Variables
$oldServer = "MyServer"
$domain = Get-CimInstance -ClassName "Win32_ComputerSystem" | `
    Select-Object -ExpandProperty Domain
$dfsnServer = "CloudDFSN.$domain"

# Create CNAME record
Import-Module -Name DnsServer
Add-DnsServerResourceRecordCName `
    -Name $oldServer `
    -HostNameAlias $dfsnServer `
    -ZoneName $domain
```

---

## <a name="create-a-namespace"></a>Erstellen eines Namespace
Die grundlegende Verwaltungseinheit für DFS-Namespaces ist der Namespace. Der Namespacestamm bzw. -name ist der Ausgangspunkt des Namespace. Beispielsweise lautet im UNC-Pfad `\\contoso.com\Public\` der Namespacestamm `Public`. 

Wenn Sie DFS-Namespaces verwenden, um einen vorhandenen Servernamen per Stammkonsolidierung zu übernehmen, muss der Namespacename dem zu übernehmenden Servernamen einschließlich dem Präfix `#` entsprechen. Wenn Sie beispielsweise einen vorhandenen Server mit dem Namen `MyServer` übernehmen möchten, erstellen Sie einen DFS-N-Namespace namens `#MyServer`. Der folgende PowerShell-Abschnitt sorgt dafür, dass das Zeichen `#` vorangestellt wird. Wenn Sie den Namespace jedoch über die DFS-Verwaltungskonsole erstellen, müssen Sie das Rautezeichen selbst einfügen. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Öffnen Sie die Konsole für die **DFS-Verwaltung**, um einen neuen Namespace zu erstellen. Klicken Sie hierzu auf die Schaltfläche **Start**, und geben Sie **DFS-Verwaltung** ein. Die daraufhin geöffnete Verwaltungskonsole umfasst die zwei Abschnitte **Namespaces** und **Replikation**, die sich respektive auf DFS-Namespaces und DFS-Replikation (DFS-R) beziehen. Die Azure-Dateisynchronisierung bietet einen modernen Replikations- und Synchronisierungsmechanismus, der anstelle von DFS-R verwendet werden kann, wenn auch eine Replikation gewünscht wird.

Wählen Sie den Abschnitt **Namespaces** aus, und klicken Sie dann auf die Schaltfläche **Neuer Namespace**. Sie können alternativ auch mit der rechten Maustaste auf den Abschnitt **Namespaces** klicken. Der daraufhin geöffnete **Assistent für neue Namespaces** führt Sie durch den Vorgang zum Erstellen eines Namespace. 

Im ersten Abschnitt des Assistenten müssen Sie den DFS-Namespaceserver zum Hosten des Namespace auswählen. Ein Namespace kann durch mehrere Server gehostet werden, aber Sie müssen DFS-Namespaces mit jeweils einem Server einrichten. Geben Sie den Namen des gewünschten DFS-Namespaceservers ein, und klicken Sie auf **Weiter**. Im Abschnitt **Namespacename und -einstellungen** können Sie den gewünschten Namen Ihres Namespace eingeben. Klicken Sie dann auf **Weiter**. 

Im Abschnitt **Namespacetyp** können Sie zwischen **Domänenbasierter Namespace** und **Eigenständiger Namespace** auswählen. Wenn Sie DFS-Namespaces verwenden möchten, um einen vorhandenen Dateiserver-/NAS-Gerätenamen beizubehalten, müssen Sie die Option „Eigenständiger Namespace“ auswählen. Für alle anderen Szenarien sollten Sie einen domänenbasierten Namespace auswählen. Weitere Informationen zur Auswahl des geeigneten Namespacetyps finden Sie im Abschnitt [Namespacetypen](#namespace-types) weiter oben.

![Screenshot: Auswahl zwischen einem domänenbasierten Namespace und einem eigenständigen Namespace im **Assistenten für neue Namespaces**](./media/files-manage-namespaces/dfs-namespace-type.png)

Wählen Sie den gewünschten Namespacetyp für Ihre Umgebung aus, und klicken Sie auf **Weiter**. Der Assistent zeigt eine Zusammenfassung des zu erstellenden Namespace an. Klicken Sie auf **Erstellen**, um den Namespace zu erstellen, und auf **Schließen**, wenn der Vorgang abgeschlossen wurde.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Führen Sie in einer PowerShell-Sitzung auf dem DFS-Namespaceserver die folgenden PowerShell-Befehle aus, und geben Sie für `$namespace`, `$type` und `$takeOverName` die relevanten Werte für Ihre Umgebung ein.

```PowerShell
# Variables
$namespace = "Public"
$type = "DomainV2" # "Standalone"
$takeOverName = $false # $true

$namespace = if ($takeOverName -and $type -eq "Standalone" -and $namespace[0] -ne "#") { 
    "#$namespace" 
} else { $namespace }
$dfsnServer = $env:ComputerName
$namespaceServer = if ($type -eq "DomainV2") { 
    Get-CimInstance -ClassName "Win32_ComputerSystem" | `
    Select-Object -ExpandProperty Domain
} else { $dfsnServer }

# Create share for DFS-N namespace
$smbShare = "C:\DFSRoots\$namespace"
if (!(Test-Path -Path $smbShare)) { New-Item -Path $smbShare -ItemType Directory }
New-SmbShare -Name $namespace -Path $smbShare -FullAccess Everyone

# Create DFS-N namespace
Import-Module -Name DFSN
$namespacePath = "\\$namespaceServer\$namespace"
$targetPath = "\\$dfsnServer\$namespace"
New-DfsnRoot -Path $namespacePath -TargetPath $targetPath -Type $type
```
---

## <a name="configure-folders-and-folder-targets"></a>Konfigurieren von Ordnern und Ordnerzielen
Damit ein Namespace nützlich ist, muss er Ordner und Ordnerziele enthalten. Jeder Ordner kann eines oder mehrere Ordnerziele umfassen, bei denen es sich um Zeiger auf die SMB-Dateifreigaben handelt, die diesen Inhalt hosten. Wenn Benutzer einen Ordner mit Ordnerzielen durchsuchen, erhält der Clientcomputer einen Verweis, über den der Clientcomputer transparent an eines der Ordnerziele umgeleitet wird. Sie können auch Ordner ohne Ordnerziele verwenden, um dem Namespace Struktur und Hierarchie hinzuzufügen.

Sie können sich DFS-Namespaceordner wie Dateifreigaben vorstellen. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Wählen Sie in der DFS-Verwaltungskonsole den Namespace aus, den Sie gerade erstellt haben, und klicken Sie auf **Neuer Ordner**. Im daraufhin angezeigten Dialogfeld **Neuer Ordner** können Sie sowohl den Ordner als auch die zugehörigen Ziele erstellen.

![Screenshot: Dialogfeld **Neuer Ordner**](./media/files-manage-namespaces/dfs-folder-targets.png)

Geben Sie im Textfeld **Name** den Namen des Ordners an. Wählen Sie **Hinzufügen...** aus, um Ordnerziele für diesen Ordner hinzuzufügen. Das daraufhin geöffnete Dialogfeld **Ordnerziel hinzufügen** enthält ein Textfeld mit der Bezeichnung **Pfad zum Ordnerziel**, in dem Sie den UNC-Pfad zum gewünschten Ordner angeben können. Klicken Sie im Dialogfeld **Ordnerziel hinzufügen** auf **OK**. Wenn Sie einer Azure-Dateifreigabe einen UNC-Pfad hinzufügen, erhalten Sie möglicherweise eine Meldung, dass der Server `storageaccount.file.core.windows.net` nicht kontaktiert werden kann. Dies ist das erwartete Verhalten. Klicken Sie auf **Ja**, um den Vorgang fortzusetzen. Klicken Sie abschließend im Dialogfeld **Neuer Ordner** auf **OK**, um den Ordner und die Ordnerziele zu erstellen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```PowerShell
# Variables
$shareName = "MyShare"
$targetUNC = "\\storageaccount.file.core.windows.net\myshare"

# Create folder and folder targets
$sharePath = "$namespacePath\$shareName"
New-DfsnFolder -Path $sharePath -TargetPath $targetUNC
```

---

Nachdem Sie nun einen Namespace, einen Ordner und ein Ordnerziel erstellt haben, sollten Sie Ihre Dateifreigabe über DFS-Namespaces einbinden können. Wenn Sie einen domänenbasierten Namespace verwenden, sollte der vollständige Pfad für Ihre Freigabe `\\<domain-name>\<namespace>\<share>` lauten. Bei Verwendung eines eigenständigen Namespace sollte der vollständige Pfad für Ihre Freigabe `\\<DFS-server>\<namespace>\<share>` lauten. Wenn Sie einen eigenständigen Namespace mit Stammkonsolidierung verwenden, können Sie direkt über Ihren alten Servernamen zugreifen, z. B. `\\<old-server>\<share>`.

## <a name="see-also"></a>Weitere Informationen
- Bereitstellen einer Azure-Dateifreigabe: [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md) und [Erstellen einer Azure-Dateifreigabe](storage-how-to-create-file-share.md)
- Konfigurieren des Zugriffs auf Dateifreigaben: [Identitätsbasierte Authentifizierung](storage-files-active-directory-overview.md) und [Azure Files – Überlegungen zum Netzwerkbetrieb](storage-files-networking-overview.md)
- [Übersicht über DFS-Namespaces](/windows-server/storage/dfs-namespaces/dfs-overview)