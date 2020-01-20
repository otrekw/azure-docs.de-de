---
title: 'Windows Virtual Desktop: MSIX-Feature zum Anfügen von Apps – Azure'
description: Einrichten des MSIX-Features zum Anfügen von Apps für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/14/2019
ms.author: helohr
ms.openlocfilehash: 8e8eec8af81832992a27206efcd7b7e7051a83b8
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772549"
---
# <a name="set-up-msix-app-attach"></a>Einrichten des MSIX-Features zum Anfügen von Apps

> [!IMPORTANT]
> Das MSIX-Features zum Anfügen von Apps befindet sich zurzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In diesem Thema wird erläutert, wie Sie das MSIX-Feature zum Anfügen von Apps in einer Windows Virtual Desktop-Umgebung einrichten.

## <a name="requirements"></a>Requirements (Anforderungen)

Bevor Sie beginnen, müssen Sie Folgendes für das MSIX-Feature zum Anfügen von Apps konfigurieren:

- Zugriff auf das Windows-Insider-Portal, um die Version von Windows 10 mit Unterstützung für die APIs für das MSIX-Feature zum Anfügen von Apps abzurufen.
- Eine funktionierende Windows Virtual Desktop-Bereitstellung. Weitere Informationen finden Sie unter [Erstellen eines Mandanten in Windows Virtual Desktop](tenant-setup-azure-active-directory.md).
- Das MSIX-Pakettool
- Eine Netzwerkfreigabe in der Windows Virtual Desktop-Bereitstellung, in der das MSIX-Paket gespeichert wird

## <a name="get-the-os-image"></a>Abrufen des Betriebssystemimages

Zunächst müssen Sie das Betriebssystemimage abrufen, das Sie für die MSIX-App verwenden. So rufen Sie das Betriebssystemimage ab:

1. Öffnen Sie das [Windows-Insider-Portal](https://www.microsoft.com/software-download/windowsinsiderpreviewadvanced?wa=wsignin1.0), und melden Sie sich an.

     >[!NOTE]
     >Sie müssen Mitglied des Windows-Insider-Programms sein, um auf das Windows-Insider-Portal zugreifen zu können. Weitere Informationen zum Windows-Insider-Programm finden Sie in unserer [Dokumentation zu Windows-Insider](https://docs.microsoft.com/windows-insider/at-home/).

2. Scrollen Sie nach unten zum Abschnitt **Edition auswählen**, und wählen Sie **Windows 10 Insider Preview Enterprise (FAST) – Build 19035** aus.

3. Wählen Sie **Bestätigen** aus, und wählen Sie dann die gewünschte Sprache aus. Wählen Sie anschließend **Bestätigen** erneut aus.
    
     >[!NOTE]
     >Zurzeit ist Englisch die einzige Sprache, die mit dem Feature getestet wurde. Sie können andere Sprachen auswählen, aber sie werden möglicherweise nicht wie vorgesehen angezeigt.
    
4. Wenn der Downloadlink generiert wird, wählen Sie den **64-Bit-Download** aus, und speichern Sie ihn auf der lokalen Festplatte.

## <a name="prepare-the-vhd-image-for-azure"></a>Vorbereiten des VHD-Images für Azure 

Bevor Sie beginnen, müssen Sie ein VHD-Masterimage erstellen. Wenn Sie das VHD-Masterimage noch nicht erstellt haben, navigieren Sie zu [Vorbereiten und Anpassen eines VHD-Masterimages](set-up-customize-master-image.md), und befolgen Sie die dort beschriebenen Anweisungen. 

Nachdem Sie das VHD-Masterimage erstellt haben, müssen Sie automatische Updates für das MSIX-Feature zum Anfügen von Apps deaktivieren. Um automatische Updates zu deaktivieren, müssen Sie an einer Eingabeaufforderung mit erhöhten Rechten die folgenden Befehle ausführen:

```cmd
rem Disable Store auto update:

reg add HKLM\Software\Policies\Microsoft\WindowsStore /v AutoDownload /t REG_DWORD /d 0 /f
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Automatic app update" /Disable
Schtasks /Change /Tn "\Microsoft\Windows\WindowsUpdate\Scheduled Start" /Disable

rem Disable Content Delivery auto download apps that they want to promote to users:

reg add HKCU\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v PreInstalledAppsEnabled /t REG_DWORD /d 0 /f

reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager\Debug /v ContentDeliveryAllowedOverride /t REG_DWORD /d 0x2 /f

rem Disable Windows Update:

sc config wuauserv start=disabled
```

Bereiten Sie im nächsten Schritt die VM-VHD für Azure vor, und laden Sie den sich ergebenden VHD-Datenträger in Azure hoch. Weitere Informationen finden Sie unter [Vorbereiten und Anpassen eines VHD-Masterimages](set-up-customize-master-image.md).

Nachdem Sie die VHD in Azure hochgeladen haben, erstellen Sie einen Hostpool, der auf diesem neuen Image basiert, indem Sie die Anweisungen im Tutorial [Erstellen eines Hostpools mithilfe von Azure Marketplace](create-host-pools-azure-marketplace.md) befolgen.

## <a name="prepare-the-application-for-msix-app-attach"></a>Vorbereiten der Anwendung für das MSIX-Feature zum Anfügen von Apps 

Wenn Sie bereits über ein MSIX-Paket verfügen, fahren Sie mit [Konfigurieren der Windows Virtual Desktop-Infrastruktur](#configure-windows-virtual-desktop-infrastructure) fort. Wenn Sie Legacyanwendungen testen möchten, befolgen Sie die Anweisungen unter [Erstellen eines MSIX-Pakets aus einem Desktopinstaller auf einem virtuellen Computer](https://docs.microsoft.com/windows/msix/packaging-tool/create-app-package-msi-vm), um die Legacyanwendung in ein MSIX-Paket zu konvertieren.

## <a name="generate-a-vhd-or-vhdx-package-for-msix"></a>Generieren eines VHD- oder VHDX-Pakets für MSIX

Pakete liegen im VHD- oder VHDX-Format vor, um die Leistung zu optimieren. MSIX benötigt VHD- oder VHDX-Pakete, um ordnungsgemäß zu funktionieren.

So generieren Sie ein VHD- oder VHDX-Paket für MSIX:

1. [Laden Sie das msixmgr-Tool herunter](https://aka.ms/msixmgr), und speichern Sie den ZIP-Ordner in einem Ordner auf der Sitzungshost-VM.

2. Entpacken Sie den ZIP-Ordner des msixmgr-Tools.

3. Speichern Sie das MSIX-Quellpaket im gleichen Ordner, in dem Sie das msixmgr-Tool entpackt haben.

4. Führen Sie das folgende Cmdlet in PowerShell aus, um eine VHD zu erstellen:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    >Stellen Sie sicher, dass die VHD groß genug ist, um das erweiterte MSIX-Paket zu speichern.*

5. Führen Sie das folgende Cmdlet aus, um die neu erstellte VHD-Datei einzubinden:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

6. Führen Sie dieses Cmdlet aus, um die VHD zu initialisieren:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

7. Führen Sie dieses Cmdlet aus, um eine neue Partition zu erstellen:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

8. Führen Sie dieses Cmdlet aus, um die Partition zu formatieren:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

9. Erstellen Sie einen übergeordneten Ordner auf der eingebundenen VHD. Dieser Schritt ist obligatorisch, da für das MSIX-Feature zum Anfügen von Apps ein übergeordneter Ordner erforderlich ist. Sie können dem übergeordneten Ordner einen beliebigen Namen geben.

### <a name="expand-msix"></a>Erweitern von MSIX

Danach müssen Sie das MSIX-Image durch Entpacken „erweitern“. So entpacken Sie das MSIX-Image:

1. Öffnen Sie eine Eingabeaufforderung als Administrator, und navigieren Sie zu dem Ordner, den Sie zum Herunterladen und Entzippen des msixmgr-Tools verwendet haben.

2. Führen Sie das folgende Cmdlet aus, um das MSIX-Paket in die VHD zu entpacken, die Sie im vorherigen Abschnitt erstellt und eingebunden haben.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    Nachdem das Entpacken abgeschlossen ist, sollte die folgende Meldung angezeigt werden:

    `Successfully unpacked and applied ACLs for package: <package name>.msix`

    >[!NOTE]
    > Bei der Verwendung von Paketen aus Microsoft Store for Business (oder Education) in Ihrem Netzwerk oder auf Geräten, die nicht mit dem Internet verbunden sind, müssen Sie die Paketlizenzen aus dem Store abrufen und installieren, um die App erfolgreich ausführen zu können. Weitere Informationen finden Sie unter [Offlineverwendung von Paketen](#use-packages-offline).

3. Navigieren Sie zur eingebundenen VHD, öffnen Sie den App-Ordner, und vergewissern Sie sich, dass Paketinhalt vorhanden ist.

4. Heben Sie die Einbindung der VHD auf.

## <a name="configure-windows-virtual-desktop-infrastructure"></a>Konfigurieren der Windows Virtual Desktop-Infrastruktur

Entwurfsbedingt kann ein einzelnes erweitertes MSIX-Paket (die VHD, die Sie im vorherigen Abschnitt erstellt haben) von mehreren Sitzungshost-VMs gemeinsam genutzt werden, da die VHDs im schreibgeschützten Modus angefügt werden.

Bevor Sie beginnen, stellen Sie sicher, dass Ihre Netzwerkfreigabe die folgenden Anforderungen erfüllt:

- Die Freigabe ist SMB-kompatibel.
- Die VMs, die Teil des Sitzungshostpools sind, verfügen über NTFS-Berechtigungen für die Freigabe.

### <a name="set-up-an-msix-app-attach-share"></a>Einrichten einer Freigabe für das Feature zum Anfügen von Apps 

Erstellen Sie in Ihrer Windows Virtual Desktop-Umgebung eine Netzwerkfreigabe, und verschieben Sie das Paket dorthin.

>[!NOTE]
> Die bewährte Methode zum Erstellen von MSIX-Netzwerkfreigaben besteht darin, die Netzwerkfreigabe mit schreibgeschützten NTFS-Berechtigungen einzurichten.

## <a name="install-certificates"></a>Installieren von Zertifikaten

Wenn Ihre App ein Zertifikat verwendet, das nicht öffentlich vertrauenswürdig ist oder selbst signiert wurde, wird dieses wie folgt installiert:

1. Klicken Sie mit der rechten Maustaste auf das Paket, und wählen Sie **Eigenschaften** aus.
2. Wählen Sie im angezeigten Fenster die Registerkarte **Digitale Signaturen** aus. Es darf nur ein Element in der Liste auf der Registerkarte vorhanden sein, wie in der folgenden Abbildung gezeigt. Wählen Sie dieses Element aus, um das Element zu markieren, und wählen Sie dann **Details** aus.
3. Wenn das Fenster mit den Details zur digitalen Signatur angezeigt wird, wählen Sie die Registerkarte **Allgemein** und dann **Zertifikat installieren** aus.
4. Wenn das Installationsprogramm geöffnet wird, wählen Sie **Lokaler Computer** als Speicherort aus, und klicken Sie dann auf **Weiter**.
5. Wenn Sie vom Installationsprogramm gefragt werden, ob Sie zulassen möchten, dass die App Änderungen an Ihrem Gerät vornimmt, wählen Sie **Ja** aus.
6. Wählen Sie **Alle Zertifikate in folgendem Speicher speichern** und dann **Durchsuchen** aus.
7. Wenn das Fenster zum Auswählen des Zertifikatspeichers angezeigt wird, wählen Sie **Vertrauenswürdige Personen** aus, und klicken Sie dann auf **OK**.
8. Wählen Sie **Fertig stellen** aus.

## <a name="prepare-powershell-scripts-for-msix-app-attach"></a>Vorbereiten von PowerShell-Skripts für das MSIX-Feature zum Anfügen von Apps

Das MSIX-Feature zum Anfügen von Apps weist vier verschiedene Phasen auf, die in der folgenden Reihenfolge ausgeführt werden müssen:

1. Phase
2. Register
3. Registrierung aufheben
4. Staging aufheben

In jeder Phase wird ein PowerShell-Skript erstellt. Die Beispielskripts für die einzelnen Phasen sind [hier](https://github.com/Azure/RDS-Templates/tree/master/msix-app-attach) verfügbar.

### <a name="stage-the-powershell-script"></a>Staging des PowerShell-Skripts

Bevor Sie die PowerShell-Skripts aktualisieren, stellen Sie sicher, dass Sie über die Volume-GUID des Volumes auf der VHD verfügen. So rufen Sie die Volume-GUID ab:

1.  Öffnen Sie die Netzwerkfreigabe, in der sich die VHD innerhalb des virtuellen Computers befindet, auf dem Sie das Skript ausführen möchten.

2.  Klicken Sie mit der rechten Maustaste auf die VHD, und wählen Sie **Einbinden** aus. Dadurch wird die VHD in einen Laufwerkbuchstaben eingebunden.

3.  Nachdem Sie die VHD eingebunden haben, wird das Fenster **Datei-Explorer** geöffnet. Erfassen des übergeordneten Ordners und Aktualisieren der Variable **$parentFolder**

    >[!NOTE]
    >Wenn kein übergeordneter Ordner angezeigt wird, bedeutet dies, dass MSIX nicht ordnungsgemäß erweitert wurde. Wiederholen Sie den vorherigen Abschnitt, und versuchen Sie es noch mal.

4.  Öffnen Sie den übergeordneten Ordner. Wenn das Paket ordnungsgemäß erweitert wurde, wird ein Ordner mit dem gleichen Namen wie das Paket angezeigt. Aktualisieren Sie die Variable **$packageName**, sodass sie mit dem Namen dieses Ordners übereinstimmt.

    Beispiel: `VSCodeUserSetup-x64-1.38.1_1.38.1.0_x64__8wekyb3d8bbwe`.

5.  Öffnen Sie eine Eingabeaufforderung, und geben Sie **mountvol** ein. Mit diesem Befehl wird eine Liste von Volumes und deren GUIDs angezeigt. Kopieren Sie die GUID des Volumes, für das der Laufwerkbuchstabe mit dem Laufwerk übereinstimmt, in das Sie die VHD in Schritt 2 eingebunden haben.

    Wenn Sie in diesem Beispiel Ihre VHD in Laufwerk C: eingebunden haben, sollten Sie z.B. in dieser Beispielausgabe für den Befehl „mountvol“ den Wert über `C:\` kopieren:

    ```cmd
    Possible values for VolumeName along with current mount points are:

    \\?\Volume{a12b3456-0000-0000-0000-10000000000}\
    *** NO MOUNT POINTS ***

    \\?\Volume{c78d9012-0000-0000-0000-20000000000}\
        E:\

    \\?\Volume{d34e5678-0000-0000-0000-30000000000}\
        C:\

    ```


6.  Aktualisieren Sie die Variable **$volumeGuid** mit der soeben kopierten Volume-GUID.

7. Öffnen Sie eine PowerShell-Eingabeaufforderung mit Administratorrechten, und aktualisieren Sie das folgende PowerShell-Skript mit den Variablen, die für Ihre Umgebung gelten.

    ```powershell
    #MSIX app attach staging sample

    #region variables

    $vhdSrc="<path to vhd>"

    $packageName = "<package name>"

    $parentFolder = "<package parent folder>"

    $parentFolder = "\" + $parentFolder + "\"

    $volumeGuid = "<vol guid>"

    $msixJunction = "C:\temp\AppAttach\"

    #endregion

    #region mountvhd

    try

    {

    Mount-Diskimage -ImagePath $vhdSrc -NoDriveLetter -Access ReadOnly

    Write-Host ("Mounting of " + $vhdSrc + " was completed!") -BackgroundColor Green

    }

    catch

    {

    Write-Host ("Mounting of " + $vhdSrc + " has failed!") -BackgroundColor Red

    }

    #endregion

    #region makelink

    $msixDest = "\\?\Volume{" + $volumeGuid + "}\"

    if (!(Test-Path $msixJunction))

    {

    md $msixJunction

    }

    $msixJunction = $msixJunction + $packageName

    cmd.exe /c mklink /j $msixJunction $msixDest

    #endregion

    #region stage

    [Windows.Management.Deployment.PackageManager,Windows.Management.Deployment,ContentType=WindowsRuntime]
    | Out-Null

    Add-Type -AssemblyName System.Runtime.WindowsRuntime

    $asTask = ([System.WindowsRuntimeSystemExtensions].GetMethods() | Where {
    $_.ToString() -eq 'System.Threading.Tasks.Task`1[TResult]
    AsTask[TResult,TProgress](Windows.Foundation.IAsyncOperationWithProgress`2[TResult,TProgress])'})[0]

    $asTaskAsyncOperation =
    $asTask.MakeGenericMethod([Windows.Management.Deployment.DeploymentResult],
    [Windows.Management.Deployment.DeploymentProgress])

    $packageManager = [Windows.Management.Deployment.PackageManager]::new()

    $path = $msixJunction + $parentFolder + $packageName # needed if we do the pbisigned.vhd

    $path = ([System.Uri]$path).AbsoluteUri

    $asyncOperation = $packageManager.StagePackageAsync($path, $null, "StageInPlace")

    $task = $asTaskAsyncOperation.Invoke($null, @($asyncOperation))

    $task

    #endregion
    ```

### <a name="register-powershell-script"></a>Registrieren eines PowerShell-Skripts

Um das Registrierungsskript auszuführen, führen Sie die folgenden PowerShell-Cmdlets mit den Platzhalterwerten aus, die durch Werte ersetzt wurden, die für Ihre Umgebung gelten.

```powershell
#MSIX app attach registration sample

#region variables

$packageName = "<package name>"

$path = "C:\Program Files\WindowsApps\" + $packageName + "\AppxManifest.xml"

#endregion

#region register

Add-AppxPackage -Path $path -DisableDevelopmentMode -Register

#endregion
```

### <a name="deregister-powershell-script"></a>Aufheben der Registrierung eines PowerShell-Skripts

Ersetzen Sie in diesem Skript den Platzhalter für **$packageName** durch den Namen des Pakets, das Sie testen.

```powershell
#MSIX app attach deregistration sample

#region variables

$packageName = "<package name>"

#endregion

#region deregister

Remove-AppxPackage -PreserveRoamableApplicationData $packageName

#endregion
```

### <a name="destage-powershell-script"></a>Aufheben des Stagings des PowerShell-Skripts

Ersetzen Sie in diesem Skript den Platzhalter für **$packageName** durch den Namen des Pakets, das Sie testen.

```powershell
#MSIX app attach de staging sample

#region variables

$packageName = "<package name>"

$msixJunction = "C:\temp\AppAttach\"

#endregion

#region deregister

Remove-AppxPackage -AllUsers -Package $packageName

cd $msixJunction

rmdir $packageName -Force -Verbose

#endregion
```

## <a name="set-up-simulation-scripts-for-the-msix-app-attach-agent"></a>Einrichten von Simulationsskripts für den Agent für das Feature zum Anfügen von Apps

Nachdem Sie die Skripts erstellt haben, können Benutzer diese manuell ausführen oder sie so einrichten, dass sie automatisch als Skripts zum Starten, Anmelden, Abmelden und Herunterfahren ausgeführt werden. Weitere Informationen zu diesen Arten von Skripts finden Sie unter [Verwenden von Skripts zum Starten, Herunterfahren, Anmelden und Abmelden in der Gruppenrichtlinie](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn789196(v=ws.11)).

Jedes dieser automatischen Skripts führt eine Phase der Skripts zum Anfügen von Apps aus:

- Das Startskript führt das Stagingskript aus.
- Das Anmeldeskript führt das Registrierungsskript aus.
- Das Abmeldeskript führt das Skript zum Aufheben der Registrierung aus.
- Das Skript zum Herunterfahren führt das Skript zum Aufheben des Stagings aus.

## <a name="use-packages-offline"></a>Offlineverwendung von Paketen

Bei der Verwendung von Paketen aus [Microsoft Store for Business](https://businessstore.microsoft.com/) oder [Microsoft Store for Education](https://educationstore.microsoft.com/) in Ihrem Netzwerk oder auf Geräten, die nicht mit dem Internet verbunden sind, müssen Sie die Paketlizenzen aus dem Microsoft Store abrufen und auf Ihrem Gerät installieren, um die App erfolgreich ausführen zu können. Wenn Ihr Gerät online ist und eine Verbindung mit Microsoft Store for Business herstellen kann, sollten die erforderlichen Lizenzen automatisch heruntergeladen werden. Wenn Sie jedoch offline sind, müssen Sie die Lizenzen manuell einrichten. 

Um die Lizenzdateien zu installieren, müssen Sie ein PowerShell-Skript verwenden, das die MDM_EnterpriseModernAppManagement_StoreLicenses02_01-Klasse im WMI-Bridge-Anbieter aufruft.  

So richten Sie die Lizenzen für die Offlineverwendung ein: 

1. Laden Sie das App-Paket, die Lizenzen und die erforderlichen Frameworks aus Microsoft Store for Business herunter. Sie benötigen sowohl die codierten als auch die nicht codierten Lizenzdateien. Ausführliche Downloadanweisungen finden Sie [hier](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).
2. Aktualisieren Sie die folgenden Variablen im Skript für Schritt 3:
      1. `$contentID` ist der ContentID-Wert aus der nicht codierten Lizenzdatei (XML-Datei). Sie können die Lizenzdatei in einem Text-Editor Ihrer Wahl öffnen.
      2. `$licenseBlob` ist die vollständige Zeichenfolge für das Lizenzierungsblob in der codierten Lizenzdatei (BIN-Datei). Sie können die codierte Lizenzdatei in einem Text-Editor Ihrer Wahl öffnen. 
3. Führen Sie das folgende Skript an einer PowerShell-Eingabeaufforderung mit erhöhten Rechten aus. Ein guter Zeitpunkt, um die Lizenzinstallation durchzuführen, ist am Ende des [Stagingskripts](#stage-the-powershell-script), das ebenfalls an einer Eingabeaufforderung mit Administratorberechtigungen ausgeführt werden muss.

```powershell
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_EnterpriseModernAppManagement_StoreLicenses02_01"
$methodName = "AddLicenseMethod"
$parentID = "./Vendor/MSFT/EnterpriseModernAppManagement/AppLicenses/StoreLicenses"

#TODO - Update $contentID with the ContentID value from the unencoded license file (.xml)
$contentID = "{‘ContentID’_in_unencoded_license_file}"

#TODO - Update $licenseBlob with the entire String in the encoded license file (.bin)
$licenseBlob = "{Entire_String_in_encoded_license_file}"

$session = New-CimSession 

#The final string passed into the AddLicenseMethod should be of the form <License Content="encoded license blob" />
$licenseString = '<License Content='+ '"' + $licenseBlob +'"' + ' />' 

$params = New-Object Microsoft.Management.Infrastructure.CimMethodParametersCollection
$param = [Microsoft.Management.Infrastructure.CimMethodParameter]::Create("param",$licenseString ,"String", "In")
$params.Add($param) 


try
{
   $instance = New-CimInstance -Namespace $namespaceName -ClassName $className -Property @{ParentID=$parentID;InstanceID=$contentID}
   $session.InvokeMethod($namespaceName, $instance, $methodName, $params)

}
catch [Exception]
{
     write-host $_ | out-string
}  
```

## <a name="next-steps"></a>Nächste Schritte

Dieses Feature wird zurzeit nicht unterstützt, aber Sie können der Community in der [Windows Virtual Desktop TechCommunity](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) Fragen stellen.

Sie können Feedback zu Windows Virtual Desktop auch auf dem [Windows Virtual Desktop-Feedback-Hub](https://aka.ms/MRSFeedbackHub) oder für die MSIX-App und das Pakettool auf dem [Feedback-Hub für das Feature zum Anfügen von Apps](https://aka.ms/msixappattachfeedback) und dem [Feedback-Hub für das MSIX-Pakettool](https://aka.ms/msixtoolfeedback) hinterlassen.
