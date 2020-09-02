---
title: Installieren von Language Packs auf Windows 10-VMs in Windows Virtual Desktop – Azure
description: Hier erfahren Sie, wie Sie Language Packs für Windows 10-VMs (mehrere Sitzungen) in Windows Virtual Desktop installieren.
author: Heidilohr
ms.topic: how-to
ms.date: 08/21/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: de495d18220500e5aa5653e89776c2634d5b1c85
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88719143"
---
# <a name="add-language-packs-to-a-windows-10-multi-session-image"></a>Hinzufügen von Language Packs zu einem Image für Windows 10 (mehrere Sitzungen)

Windows Virtual Desktop ist ein Dienst, den Ihre Benutzer jederzeit und überall bereitstellen können. Daher ist es wichtig, dass die Benutzer die jeweilige Sprache, die in dem Image für Windows 10 Enterprise (mehrere Sitzungen) angezeigt wird, anpassen können.

Es gibt zwei Möglichkeiten, die Sprachanforderungen Ihrer Benutzer zu erfüllen:

- Erstellen Sie dedizierte Hostpools mit einem angepassten Image für jede Sprache.
- Fügen Sie Benutzer mit unterschiedlichen Sprach- und Lokalisierungsanforderungen demselben Hostpool hinzu, aber passen Sie deren Images an, um sicherzustellen, dass sie die gewünschte Sprache auswählen können.

Die letztere Methode ist sehr viel effizienter und kostengünstiger. Sie können aber entscheiden, welche Methode Ihren Anforderungen am besten entspricht. In diesem Artikel wird gezeigt, wie Sie die Sprachen für Ihre Images anpassen.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen folgende Komponenten, um die Images für Windows 10 Enterprise (mehrere Sitzungen) so anzupassen, dass mehrere Sprachen hinzugefügt werden können:

- Einen virtuellen Azure-Computer (VM) mit Windows 10 Enterprise (mehrere Sitzungen), Version 1903 oder höher

- Language ISO und Feature on Demand (FOD) Disk 1 für die vom Image verwendete Betriebssystemversion. Diese können Sie hier herunterladen: 
     
     - Language ISO:
        - [Windows 10, Version 1903 oder 1909, Language Pack ISO](https://software-download.microsoft.com/download/pr/18362.1.190318-1202.19h1_release_CLIENTLANGPACKDVD_OEM_MULTI.iso)
        - [Windows 10, Version 2004, Language Pack ISO](https://software-download.microsoft.com/download/pr/19041.1.191206-1406.vb_release_CLIENTLANGPACKDVD_OEM_MULTI.iso)

     - FOD Disk 1 ISO:
        - [Windows 10, Version 1903 oder 1909, FOD Disk 1 ISO](https://software-download.microsoft.com/download/pr/18362.1.190318-1202.19h1_release_amd64fre_FOD-PACKAGES_OEM_PT1_amd64fre_MULTI.iso)
        - [Windows 10, Version 2004, FOD Disk 1 ISO](https://software-download.microsoft.com/download/pr/19041.1.191206-1406.vb_release_amd64fre_FOD-PACKAGES_OEM_PT1_amd64fre_MULTI.iso)

- Eine Azure Files-Freigabe oder eine Dateifreigabe auf einer Windows-Dateiserver-VM

>[!NOTE]
>Auf die Dateifreigabe (Repository) muss von der Azure-VM aus zugegriffen werden können, die zum Erstellen des benutzerdefinierten Images verwendet werden soll.

## <a name="create-a-content-repository-for-language-packages-and-features-on-demand"></a>Erstellen eines Inhaltsrepositorys für Sprachpakete und Features bei Bedarf (FODs)

Zum Erstellen des Inhaltsrepositorys für Sprachpakete und FODs führen Sie die folgenden Schritte aus:

1. Laden Sie auf einer Azure-VM die Windows 10-ISO und -FODs (mehrere Sprachen) für Images für Windows 10 Enterprise (mehrere Sitzungen), Version 1903, 1909 und 2004, über die Links unter [Voraussetzungen](#prerequisites) herunter.

2. Öffnen Sie die ISO-Dateien auf der VM, und binden Sie sie ein.

3. Wechseln Sie zur Language Pack-ISO, kopieren Sie den Inhalt aus den Ordnern **LocalExperiencePacks** und **x64\\langpacks**, und fügen Sie dann den Inhalt in die Dateifreigabe ein.

4. Wechseln Sie zur **FOD-ISO-Datei**, kopieren Sie den gesamten Inhalt, und fügen Sie ihn in die Dateifreigabe ein.

     >[!NOTE]
     > Wenn Sie mit begrenztem Speicherplatz arbeiten, kopieren Sie nur die Dateien für die Sprachen, von denen Sie wissen, dass Ihre Benutzer sie benötigen. Sie können die Dateien anhand der Sprachcodes in den Dateinamen voneinander unterscheiden. Beispielsweise enthält die Datei für Französisch den Code „fr-FR“ im Namen. Eine vollständige Liste der Sprachcodes für alle verfügbaren Sprachen finden Sie unter [Verfügbare Sprachen für Windows](/windows-hardware/manufacture/desktop/available-language-packs-for-windows).

     >[!IMPORTANT]
     > Einige Sprachen erfordern zusätzliche Schriftarten, die in Satellitenpaketen mit anderen Benennungskonventionen enthalten sind. Beispielsweise enthalten die Namen der Dateien für japanische Schrift die Zeichenfolge „Jpan“.
     >
     > [!div class="mx-imgBorder"]
     > ![Beispiel der Sprachpakete für Japanisch mit der Sprachkennung „Jpan“ im Dateinamen](media/language-pack-example.png)

5. Legen Sie die Berechtigungen für die Freigabe des Inhaltsrepositorys für Sprachen so fest, dass Sie über Lesezugriff von der VM aus verfügen, die Sie zum Erstellen des benutzerdefinierten Images verwenden möchten.

## <a name="create-a-custom-windows-10-enterprise-multi-session-image-manually"></a>Manuelles Erstellen eines Images für Windows 10 Enterprise (mehrere Sitzungen)

Führen Sie die folgenden Schritte aus, um ein Image für Windows 10 Enterprise (mehrere Sitzungen) manuell zu erstellen:

1. Stellen Sie eine Azure-VM bereit. Dann navigieren Sie zum Azure-Katalog, und wählen Sie die von Ihnen verwendete aktuelle Version von Windows 10 Enterprise (mehrere Sitzungen) aus.
2. Nachdem Sie die VM bereitgestellt haben, stellen Sie mithilfe von RDP als lokaler Administrator eine Verbindung her.
3. Vergewissern Sie sich, dass die VM über alle aktuellen Windows-Updates verfügt. Laden Sie bei Bedarf die Updates herunter, und starten Sie die VM neu.
4. Stellen Sie eine Verbindung mit dem Repository für Sprachpakete und FOD-Dateifreigaben her, und stellen Sie es auf einem Laufwerk (z. B. Laufwerk E) bereit.

## <a name="create-a-custom-windows-10-enterprise-multi-session-image-automatically"></a>Automatisches Erstellen eines Images für Windows 10 Enterprise (mehrere Sitzungen)

Wenn Sie Sprachen lieber über einen automatisierten Prozess installieren möchten, können Sie ein Skript in PowerShell einrichten. Mithilfe des folgenden Skriptbeispiels können Sie die Sprachpakete für Spanisch (Spanien), Französisch (Frankreich) und Chinesisch (VRC) sowie die Satellitenpakete für Windows 10 Enterprise (mehrere Sitzungen), Version 2004, installieren. Mit dem Skript werden das Benutzeroberflächen-Sprachpaket und alle erforderlichen Satellitenpakete in das Image integriert. Sie können Sie dieses Skript aber auch abändern, um andere Sprachen zu installieren. Stellen Sie lediglich sicher, dass Sie das Skript in einer PowerShell-Sitzung mit erhöhten Rechten ausführen, da es andernfalls nicht funktioniert.

```powershell
########################################################
## Add Languages to running Windows Image for Capture##
########################################################

##Disable Language Pack Cleanup##
Disable-ScheduledTask -TaskPath "\Microsoft\Windows\AppxDeploymentClient\" -TaskName "Pre-staged app cleanup"

##Set Language Pack Content Stores##
[string]$LIPContent = "E:"

##Spanish##
Add-AppProvisionedPackage -Online -PackagePath $LIPContent\es-es\LanguageExperiencePack.es-es.Neutral.appx -LicensePath $LIPContent\es-es\License.xml
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Client-Language-Pack_x64_es-es.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Basic-es-es-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Handwriting-es-es-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-OCR-es-es-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Speech-es-es-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-TextToSpeech-es-es-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-NetFx3-OnDemand-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-MSPaint-FoD-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Notepad-FoD-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-PowerShell-ISE-FOD-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Printing-WFS-FoD-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-StepsRecorder-Package~31bf3856ad364e35~amd64~es-es~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-WordPad-FoD-Package~31bf3856ad364e35~amd64~es-es~.cab
$LanguageList = Get-WinUserLanguageList
$LanguageList.Add("es-es")
Set-WinUserLanguageList $LanguageList -force

##French##
Add-AppProvisionedPackage -Online -PackagePath $LIPContent\fr-fr\LanguageExperiencePack.fr-fr.Neutral.appx -LicensePath $LIPContent\fr-fr\License.xml
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Client-Language-Pack_x64_fr-fr.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Basic-fr-fr-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Handwriting-fr-fr-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-OCR-fr-fr-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Speech-fr-fr-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-TextToSpeech-fr-fr-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-NetFx3-OnDemand-Package~31bf3856ad364e35~amd64~fr-fr~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-MSPaint-FoD-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Notepad-FoD-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-PowerShell-ISE-FOD-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Printing-WFS-FoD-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-StepsRecorder-Package~31bf3856ad364e35~amd64~fr-FR~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-WordPad-FoD-Package~31bf3856ad364e35~amd64~fr-FR~.cab
$LanguageList = Get-WinUserLanguageList
$LanguageList.Add("fr-fr")
Set-WinUserLanguageList $LanguageList -force

##Chinese(PRC)##
Add-AppProvisionedPackage -Online -PackagePath $LIPContent\zh-cn\LanguageExperiencePack.zh-cn.Neutral.appx -LicensePath $LIPContent\zh-cn\License.xml
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Client-Language-Pack_x64_zh-cn.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Basic-zh-cn-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Fonts-Hans-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Handwriting-zh-cn-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-OCR-zh-cn-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-Speech-zh-cn-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-LanguageFeatures-TextToSpeech-zh-cn-Package~31bf3856ad364e35~amd64~~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-NetFx3-OnDemand-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-MSPaint-FoD-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Notepad-FoD-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-PowerShell-ISE-FOD-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-Printing-WFS-FoD-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-StepsRecorder-Package~31bf3856ad364e35~amd64~zh-cn~.cab
Add-WindowsPackage -Online -PackagePath $LIPContent\Microsoft-Windows-WordPad-FoD-Package~31bf3856ad364e35~amd64~zh-cn~.cab
$LanguageList = Get-WinUserLanguageList
$LanguageList.Add("zh-cn")
Set-WinUserLanguageList $LanguageList -force
```

>[!IMPORTANT]
>Für die Windows 10 Enterprise-Versionen 1903 und 1909 ist die Paketdatei `Microsoft-Windows-Client-Language-Pack_x64_<language-code>.cab` nicht erforderlich.

Je nach Anzahl der Sprachen, die installiert werden müssen, kann die Ausführung des Skripts einige Zeit in Anspruch nehmen.

Sobald die Skriptausführung abgeschlossen ist, vergewissern Sie sich, dass die Sprachpakete ordnungsgemäß installiert wurden. Dazu navigieren Sie zu **Start** > **Einstellungen** > **Uhrzeit und Sprache** > **Sprache**. Wenn die Sprachdateien vorhanden sind, ist die Einrichtung abgeschlossen.

Denken Sie anschließend daran, die Freigabe wieder zu trennen.

## <a name="finish-customizing-your-image"></a>Abschließen der Imageanpassung

Nachdem Sie die Language Packs installiert haben, können Sie weitere Software installieren, die Sie dem angepassten Image hinzufügen möchten.

Sobald Sie die Anpassung des Images abgeschlossen haben, müssen Sie das Tools für die Systemvorbereitung (sysprep) ausführen.

Zum Ausführen von Sysprep gehen Sie folgendermaßen vor:

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um das Image zu generalisieren:  
   
     ```cmd
     C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown
     ```

2. Fahren Sie die VM herunter, und erfassen Sie sie in einem verwalteten Image. Folgen Sie dazu den Anweisungen unter [Erstellen eines verwalteten Images eines generalisierten virtuellen Computers in Azure](../virtual-machines/windows/capture-image-resource.md).

3. Sie können jetzt das angepasste Image zum Bereitstellen eines Windows Virtual Desktop-Hostpools verwenden. Informationen dazu, wie Sie einen Hostpool bereitstellen, finden Sie im [Tutorial: Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md).

## <a name="enable-languages-in-windows-settings-app"></a>Aktivieren von Sprachen in der App für Windows-Einstellungen

Zum Schluss müssen Sie die Sprache der Sprachliste jedes Benutzers hinzufügen, damit er seine bevorzugte Sprache im Menü „Einstellungen“ auswählen kann.

Um sicherzustellen, dass die Benutzer die von Ihnen installierten Sprachen auswählen können, melden Sie sich als Benutzer an, und führen Sie dann das folgende PowerShell-Cmdlet aus, um die installierten Language Packs zum Menü „Sprachen“ hinzuzufügen. Sie können dieses Skript auch als automatisierte Aufgabe einrichten, die aktiviert wird, wenn sich der Benutzer bei seiner Sitzung anmeldet.

```powershell
$LanguageList = Get-WinUserLanguageList
$LanguageList.Add("es-es")
$LanguageList.Add("fr-fr")
$LanguageList.Add("zh-cn")
Set-WinUserLanguageList $LanguageList -force
```

Nachdem ein Benutzer seine Spracheinstellungen geändert hat, muss er sich von seiner Windows Virtual Desktop-Sitzung abmelden und sich erneut anmelden, damit die Änderungen wirksam werden. 

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie etwas über bekannte Probleme bei Language Packs erfahren möchten, finden Sie weitere Informationen unter [Hinzufügen von Sprachpaketen in Windows 10, Version 1803 und höher: Bekannte Probleme](/windows-hardware/manufacture/desktop/language-packs-known-issue).

Wenn Sie weitere Fragen zu Windows 10 Enterprise (mehrere Sitzungen) haben, sehen Sie sich die Informationen unter [Häufig gestellte Fragen](windows-10-multisession-faq.md) an.
