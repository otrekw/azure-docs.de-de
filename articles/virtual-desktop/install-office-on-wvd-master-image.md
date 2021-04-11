---
title: Installieren von Office für ein VHD-Masterimage – Azure
description: Erfahren Sie, wie Sie Office auf einem Windows Virtual Desktop-Masterimage in Azure installieren und anpassen.
author: Heidilohr
ms.topic: how-to
ms.date: 05/02/2019
ms.author: helohr
manager: femila
ms.openlocfilehash: ce4eb3d556b6dff833129d11b26d439066b63f2e
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106446841"
---
# <a name="install-office-on-a-master-vhd-image"></a>Installieren von Office für ein VHD-Masterimage

In diesem Artikel erfahren Sie, wie Sie Microsoft 365 Apps for Enterprise, OneDrive und andere gängige Anwendungen auf einem VHD-Masterimage (virtuelle Festplatte) für den Upload in Azure installieren. Wenn Ihre Benutzer auf bestimmte Branchenanwendungen zugreifen müssen, wird empfohlen, die Installation durchzuführen, nachdem Sie die Anweisungen in diesem Artikel befolgt haben.

In diesem Artikel wird vorausgesetzt, dass Sie bereits eine VM (virtueller Computer) erstellt haben. Wenn Sie das noch nicht getan haben, führen Sie die Anweisungen unter [Vorbereiten und Anpassen eines VHD-Masterimages](set-up-customize-master-image.md#create-a-vm) aus.

In diesem Artikel wird außerdem davon ausgegangen, dass Sie über erhöhte Rechte für die VM verfügen, unabhängig davon, ob diese in Azure oder Hyper-V-Manager bereitgestellt wird. Wenn das nicht der Fall ist, führen Sie die Anweisungen unter [Erhöhen der Zugriffsrechte zum Verwalten aller Azure-Abonnements und Verwaltungsgruppen](../role-based-access-control/elevate-access-global-admin.md) aus.

>[!NOTE]
>Diese Anweisungen beziehen sich auf eine spezielle Konfiguration für Windows Virtual Desktop, die mit den in Ihrer Organisation vorhandenen Prozessen verwendet werden kann.

## <a name="install-office-in-shared-computer-activation-mode"></a>Installieren von Office im Modus „Aktivierung gemeinsam genutzter Computer“

Mit der Aktivierung gemeinsam genutzter Computer können Sie Microsoft 365 Apps for Enterprise auf einem Computer in Ihrem Unternehmen bereitstellen, auf den mehrere Benutzer zugreifen. Weitere Informationen zur Aktivierung gemeinsam genutzter Computer finden Sie im [Überblick über die Aktivierung gemeinsam genutzter Computer für Microsoft 365 Apps](/deployoffice/overview-shared-computer-activation).

Verwenden Sie das [Office-Bereitstellungstool](https://www.microsoft.com/download/details.aspx?id=49117), um Office zu installieren. Windows 10 Enterprise (mehrere Sitzungen) unterstützt nur die folgenden Versionen von Office:

   - Microsoft 365 Apps for Enterprise
   - Microsoft 365 Apps for Business mit einem Microsoft 365 Business Premium-Abonnement

Für das Office-Bereitstellungstool wird eine XML-Konfigurationsdatei benötigt. Informationen zum Anpassen des folgenden Beispiels finden Sie unter [Konfigurationsoptionen für das Office-Bereitstellungstool](/deployoffice/configuration-options-for-the-office-2016-deployment-tool/).

Mit dieser von uns bereitgestellten XML-Beispieldatei für die Konfiguration wird Folgendes durchgeführt:

   - Installieren von Office und Bereitstellen von Updates, jeweils über den monatlichen Enterprise-Kanal
   - Verwenden der X64-Architektur
   - Deaktivieren automatischer Updates
   - Entfernen aller vorhandenen Installationen von Office und Migrieren der zugehörigen Einstellungen
   - Aktivierung gemeinsam genutzter Computer

>[!NOTE]
>Die Schablonensuchfunktion von Visio funktioniert unter Windows Virtual Desktop möglicherweise nicht wie erwartet.

Folgendes wird dagegen von dieser XML-Beispielkonfigurationsdatei nicht erledigt:

   - Installieren von Skype for Business
   - Installieren von OneDrive im Modus „Pro Benutzer“ Weitere Informationen finden Sie unter [Installieren von OneDrive im Modus „Pro Computer“](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Die Aktivierung gemeinsam genutzter Computer kann über Gruppenrichtlinienobjekte (GPOs) oder Registrierungseinstellungen eingerichtet werden. Das Gruppenrichtlinienobjekt befindet sich unter **Computerkonfiguration\\Richtlinien\\Administrative Vorlagen\\Microsoft Office 2016 (Computer)\\Lizenzierungseinstellungen**.

Das Office-Bereitstellungstool enthält die Datei „setup.exe“. Führen Sie den folgenden Befehl auf einer Befehlszeile aus, um Office zu installieren:

```cmd
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Beispieldatei „configuration.xml“

Im folgenden XML-Beispiel wird das Release aus dem monatlichen Enterprise-Kanal installiert.

```xml
<Configuration>
  <Add OfficeClientEdition="64" Channel="MonthlyEnterprise">
    <Product ID="O365ProPlusRetail">
      <Language ID="en-US" />
      <Language ID="MatchOS" />
      <ExcludeApp ID="Groove" />
      <ExcludeApp ID="Lync" />
      <ExcludeApp ID="OneDrive" />
      <ExcludeApp ID="Teams" />
    </Product>
  </Add>
  <RemoveMSI/>
  <Updates Enabled="FALSE"/>
  <Display Level="None" AcceptEULA="TRUE" />
  <Logging Level="Standard" Path="%temp%\WVDOfficeInstall" />
  <Property Name="FORCEAPPSHUTDOWN" Value="TRUE"/>
  <Property Name="SharedComputerLicensing" Value="1"/>
</Configuration>
```

>[!NOTE]
>Das Office-Team empfiehlt die Verwendung der 64-Bit-Installation für den Parameter **OfficeClientEdition**.

Nach der Installation von Office können Sie das Standardverhalten von Office aktualisieren. Führen Sie die folgenden Befehle einzeln oder in einer Batchdatei aus, um das Verhalten zu aktualisieren.

```cmd
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

## <a name="install-onedrive-in-per-machine-mode"></a>Installieren von OneDrive im Modus „Pro Computer“

Normalerweise wird OneDrive im Modus „Pro Benutzer“ installiert. In dieser Umgebung sollte die Anwendung pro Computer installiert werden.

Hier ist beschrieben, wie Sie OneDrive im Modus „Pro Computer“ installieren:

1. Erstellen Sie zunächst einen Speicherort zum Bereitstellen des OneDrive-Installationsprogramms. Hierfür ist ein lokaler Datenträgerordner oder ein Speicherort vom Typ [\\\\unc] (file://unc) geeignet.

2. Laden Sie die Datei „OneDriveSetup.exe“ mit diesem Link an Ihren bereitgestellten Speicherort herunter: <https://aka.ms/OneDriveWVD-Installer>.

3. Wenn Sie durch Auslassen von **\<ExcludeApp ID="OneDrive" /\>** Office mit OneDrive installiert haben, deinstallieren Sie alle vorhandenen OneDrive-Einzelbenutzerinstallationen durch Ausführen des folgenden Befehls in einer Eingabeaufforderung mit erhöhten Rechten:

    ```cmd
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Führen Sie diesen Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus, um den Registrierungswert **AllUsersInstall** festzulegen:

    ```cmd
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Führen Sie diesen Befehl aus, um OneDrive im Modus „Pro Computer“ zu installieren:

    ```cmd
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Führen Sie diesen Befehl aus, um OneDrive mit der Anmeldung für alle Benutzer zu starten:

    ```cmd
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Aktivieren Sie die Option **Silently configure user account** (Benutzerkonto im Hintergrund konfigurieren), indem Sie den folgenden Befehl ausführen.

    ```cmd
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Sie können bekannte Windows-Ordner umleiten und in OneDrive verschieben, indem Sie den folgenden Befehl ausführen.

    ```cmd
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="microsoft-teams-and-skype-for-business"></a>Microsoft Teams und Skype for Business

Windows Virtual Desktop unterstützt Skype for Business nicht.

Hilfe bei der Installation von Microsoft Teams finden Sie unter [Verwenden von Microsoft Teams in Windows Virtual Desktop](teams-on-wvd.md). Die Medienoptimierung für Microsoft Teams in Windows Virtual Desktop ist in der Vorschauversion verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Office auf dem Image installiert haben, können Sie damit fortfahren, Ihr VHD-Masterimage anzupassen. Informationen dazu finden Sie unter [Vorbereiten und Anpassen eines VHD-Masterimages](set-up-customize-master-image.md).
