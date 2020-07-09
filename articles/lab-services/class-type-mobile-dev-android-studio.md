---
title: Einrichten eines Labs zum Vermitteln der Entwicklung mobiler Anwendungen mit Android Studio
titleSuffix: Azure Lab Services
description: Erfahren Sie, wie Sie ein Lab einrichten, in dem Sie einer Klasse mit Android Studio die Entwicklung von mobilen Datenanwendungen vermitteln.  Im Artikel werden außerdem die Anpassungen erläutert, die bei der Verwendung von Android Studio auf einem virtuellen Computer in Azure vorgenommen werden müssen.
author: emaher
ms.topic: article
ms.date: 06/26/2020
ms.author: enewman
ms.openlocfilehash: ff996a096cddbb85f1e1c84cd051c18a7ab4ad79
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85444980"
---
# <a name="set-up-a-lab-to-teach-data-mobile-application-development-with-android-studio"></a>Einrichten eines Labs zum Vermitteln der Entwicklung mobiler Datenanwendungen mit Android Studio

In diesem Artikel wird gezeigt, wie Sie eine Einführungsklasse für die Entwicklung mobiler Anwendungen einrichten.  Diese Klasse befasst sich mit mobilen Android-Anwendungen, die im [Google Play Store](https://play.google.com/store/apps) veröffentlicht werden können.  Die Kursteilnehmer lernen, wie Anwendungen mit [Android Studio](https://developer.android.com/studio) erstellt werden.  Für das lokale Testen der Anwendung wird der [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) verwendet.

## <a name="lab-configuration"></a>Labkonfiguration

Zum Einrichten dieses Labs benötigen Sie zunächst ein Azure-Abonnement und ein Lab-Konto. Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/), bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie ein neues Lab-Konto in Azure Lab Services erstellen. Weitere Informationen zum Erstellen eines neuen Labkontos finden Sie im [Tutorial zum Einrichten eines Labkontos](tutorial-setup-lab-account.md).  Sie können auch ein vorhandenes Lab-Konto verwenden.

Absolvieren Sie das [Tutorial zum Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md), um ein neues Lab zu erstellen, und wenden Sie dann die folgenden Einstellungen an:

| Größe des virtuellen Computers | Image |
| -------------------- | ----- |
| Mittel (geschachtelte Virtualisierung) | Windows Server 2019 Datacenter |

## <a name="template-machine-configuration"></a>Konfiguration des Vorlagencomputers

Nachdem der Vorlagencomputer erstellt wurde, [starten Sie den Computer, und stellen Sie eine Verbindung mit ihm her](how-to-create-manage-template.md#update-a-template-vm), um die folgenden Aufgaben auszuführen:

1. Hinzufügen einer Hyper-V-Rolle
2. Herunterladen und Installieren von Java  
3. Herunterladen und Installieren des Visual Studio-Emulators für Android
4. Herunterladen und Installieren von Android Studio
5. Konfigurieren des Visual Studio-Emulators für Android Studio

## <a name="add-hyper-v-role"></a>Hinzufügen einer Hyper-V-Rolle

Für eine erfolgreiche Installation des Visual Studio-Emulators für Android muss Hyper-V aktiviert werden.  Befolgen Sie die Anweisungen im Artikel [Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer](how-to-enable-nested-virtualization-template-vm.md).

## <a name="install-java"></a>Installieren von Java

Android Studio erfordert Java.  Führen Sie die folgenden Schritte aus, um die neueste Java-Version herunterzuladen.

1. Navigieren Sie zur [Java-Downloadseite](https://www.java.com/download/). Klicken Sie auf die Schaltfläche **Java Download**.
2. Klicken Sie auf der Webseite von Java für 64-Bit-Windows auf die Schaltfläche **Agree and Start Free Download** (Zustimmen und kostenlosen Download starten).
3. Wenn das Installationsprogramm des **Java Setup** angezeigt wird, klicken Sie auf **Install** (Installieren).
4. Warten Sie, bis der Titel des Installationsprogramms zu **Java Setup – Complete** (Java-Setup abgeschlossen) wechselt.  Klicken Sie auf die Schaltfläche **Close** (Schließen).

## <a name="install-visual-studio-emulator-for-android"></a>Installieren des Visual Studio-Emulators für Android

Zum lokalen Testen einer Android-Anwendung muss eine virtualisierte Version eines Android-Geräts verwendet werden.  Es sind einige Android-Emulatoren verfügbar, mit deren Hilfe Entwickler ihre Anwendungen auf ihren Computern testen können.  Wir verwenden den Visual Studio-Emulator für Android, da es sich um einen Emulator handelt, der geschachtelte Virtualisierungen unterstützt.  Da es sich bei der Labdienst-VM bereits um einen virtuellen Computer handelt, benötigen wir einen Emulator, der geschachtelte Virtualisierungen unterstützt.  Der integrierte Emulator für Android Studio unterstützt die geschachtelte Virtualisierung nicht.  Informationen dazu, welche Emulatoren geschachtelte Virtualisierungen unterstützen, finden Sie unter [Hardwarebeschleunigung für verbesserte Leistung des Emulators (Hyper-V und HAXM)](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration).

Befolgen Sie die folgenden Anweisungen, um den Visual Studio-Emulator für Android herunterzuladen und zu installieren.

1. Navigieren Sie zur Homepage des [Visual Studio-Emulators für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/).
2. Klicken Sie auf den die Schaltfläche **Download the Emulator** (Emulator herunterladen).
3. Wenn „vs_emulatorsetup.exe“ heruntergeladen wurde, führen Sie die ausführbare Datei aus.
4. Wenn das Visual Studio-Setupdialogfeld angezeigt wird, klicken Sie auf die Schaltfläche **Installieren**.
5. Warten Sie, bis die Installation abgeschlossen ist.  Klicken Sie auf die Schaltfläche **Jetzt neu starten**, um den Computer neu zu starten und die Installation abzuschließen.

Starten Sie den Emulator zuerst, bevor Sie die Anwendung mit Android Studio bereitstellen.  Weitere Informationen zum Visual Studio-Emulator für Android finden Sie in der [Dokumentation des Visual Studio-Emulators für Android](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-emulator-for-android).

## <a name="install-android-studio"></a>Installieren von Android Studio

Befolgen Sie die nachstehenden Anweisungen, um [Android Studio](https://developer.android.com/studio) herunterzuladen und zu installieren.

1. Navigieren Sie zur [Android Studio-Downloadseite](https://developer.android.com/studio#downloads).  
    > [!NOTE]
    > Internet Explorer wird von dieser Website nicht unterstützt.
2. Klicken Sie auf das ausführbare Android Studio-Paket für Windows (64 Bit).
3. Lesen Sie die im Popup angezeigten rechtlichen Bedingungen.  Wenn Sie fortfahren möchten, aktivieren Sie das Kontrollkästchen **I have read and agree with the above terms and conditions** (Ich habe die oben angegebenen Nutzungsbedingungen gelesen und stimme ihnen zu), und klicken Sie auf die Schaltfläche **Download Android Studio for Windows** (Android Studio für Windows herunterladen).
4. Nachdem die ausführbare Setupdatei für Android Studio heruntergeladen wurde, führen Sie sie aus.
5. Klicken Sie auf der Seite **Welcome to Android Studio Setup** (Willkommen beim Android Studio-Setup) von **Android Studio Setup** auf **Next** (Weiter).
6. Klicken Sie auf der Seite **Configuration Settings** (Konfigurationseinstellungen) auf **Next** (Weiter).
7. Klicken Sie auf der Seite **Choose Start Menu Folder** (Ordner des Startmenüs auswählen) auf **Install** (Installieren).
8. Warten Sie, bis das Setup abgeschlossen ist.
9. Klicken Sie auf der Seite **Installation Complete** (Installation abgeschlossen) auf **Next** (Weiter).
10. Gehen Sie auf der Seite **Completing Android Studio Setup** (Android Studio-Setup fertigstellen) wie folgt vor.  Klicken Sie auf **Fertig stellen**.
11. Android Studio wird nach Abschluss des Setups automatisch gestartet.
12. Wählen Sie im Dialogfeld **Import Android Settings From** (Android-Einstellungen importieren von) die Option **Do not import settings** (Keine Einstellungen importieren) aus. Klicken Sie auf **OK**.
13. Klicken Sie auf der Seite **Welcome** (Willkommen) des **Android Studio-Setup-Assistenten** auf **Next** (Weiter).
14. Wählen Sie auf der Seite **Install Type** (Installationstyp) die Option **Standard** aus. Klicken Sie auf **Weiter**.
15. Wählen Sie auf der Seite **Select UI Theme** (Benutzeroberflächendesign auswählen) das gewünschte Design aus. Klicken Sie auf **Weiter**.
16. Klicken Sie auf der Seite **Verify Settings** (Einstellungen überprüfen) auf **Next** (Weiter).
17. Warten Sie auf der Seite **Downloading Components** (Komponenten werden heruntergeladen), bis alle Komponenten heruntergeladen wurden.  Klicken Sie auf **Fertig stellen**.

    > [!IMPORTANT]
    > Es ist davon auszugehen, dass bei der HAXM-Installation ein Fehler auftritt.  HAXM unterstützt keine geschachtelten Virtualisierungen, daher wurde weiter oben in diesem Artikel der Visual Studio-Emulator für Android installiert.

18. Wenn der Setup-Assistent abgeschlossen ist, wird das Dialogfeld **Welcome to Android Studio** (Willkommen bei Android Studio) angezeigt.

## <a name="configure-android-studio-and-visual-studio-emulator-for-android"></a>Konfigurieren von Android Studio und dem Visual Studio-Emulator für Android Studio

Android Studio ist nun beinahe einsatzbereit.  Wir müssen noch dem Visual Studio-Emulator für Android mitteilen, wo das Android SDK installiert ist.  Dadurch werden alle in Visual Studio für Android ausgeführten Emulatoren als Bereitstellungsziele zum Debuggen in Android Studio angezeigt.

Wir müssen einen bestimmten Registrierungsschlüssel angeben, um dem Visual Studio-Emulator für Android mitzuteilen, wo sich das Android SDK befindet.  Führen Sie das nachstehende Skript aus, um den benötigten Registrierungsschlüssel festzulegen.  Im nachstehenden PowerShell-Skript wird davon ausgegangen, dass das Android SDK am Standardspeicherort installiert wurde.  Wenn Sie das Android SDK an einem anderen Speicherort installiert haben, ändern Sie den Wert von `$androidSdkPath` vor dem Ausführen des Skripts.

```powershell
$androidSdkPath = Resolve-Path $(Join-Path "$($env:APPDATA)" "../Local/Android/Sdk")

$registryKeyPath = "HKLM:Software\WOW6432NODE\Android Sdk Tools"
New-Item -Path $registryKeyPath
New-ItemProperty -Path $registryKeyPath -Name Path -PropertyType String -Value $androidSdkPath
```

> [!IMPORTANT]
> Starten Sie den Visual Studio-Emulator für Android und Android Studio neu, damit die neue Einstellung verwendet wird.

Starten Sie die Version, die Sie im Visual Studio-Emulator benötigen.  Sie wird als Bereitstellungsziel für Ihre Android-App in Android Studio angezeigt.  Das Android Studio-Projekt muss mindestens die Version unterstützen, die im Visual Studio-Emulator für Android ausgeführt wird.  Nun können Sie Projekte mithilfe von Android Studio und dem Visual Studio-Emulator für Android erstellen und debuggen.  Wenn Probleme auftreten, finden Sie weitere Informationen in den Problembehandlungshinweisen zum Android-Emulator.

## <a name="cost"></a>Kosten

Das nachstehende Beispiel dient der Schätzung der Labkosten.
Für einen Kurs mit 25 Teilnehmern, 20 planmäßigen Kursstunden und 10 Stunden Hausaufgaben bzw. Arbeitsaufträgen entstehen folgende Kosten für das Lab:  

25 Kursteilnehmer \* (20 geplante Stunden + 10 Kontingentstunden) × 55 Lab-Einheiten × 0,01 USD pro Stunde = 412,50 USD

Weitere Informationen zu den Preisen finden Sie unter [Azure Lab Services-Preise](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Einrichtungsschritte sind für alle Labs gleich.

- [Erstellen und Verwalten einer Vorlage](how-to-create-manage-template.md)
- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Registrierungslinks per E-Mail an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users)
