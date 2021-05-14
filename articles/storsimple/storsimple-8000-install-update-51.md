---
title: Installieren von Update 5.1 auf einem Gerät der StorSimple 8000-Serie | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Update 5.1 für die StorSimple 8000-Serie auf Ihrem StorSimple-Gerät der 8000-Serie installieren.
services: storsimple
documentationcenter: NA
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/21/2021
ms.author: alkohli
ms.openlocfilehash: 289ffbbd3dc23be0060ee5dbe488bf80323214e2
ms.sourcegitcommit: 19dcad80aa7df4d288d40dc28cb0a5157b401ac4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107897215"
---
# <a name="install-update-51-on-your-storsimple-device"></a>Installieren von Update 5.1 auf Ihrem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In dieser Anleitung wird erklärt, wie Sie Update 5.1 auf einem StorSimple-Gerät mit einer früheren Softwareversion über das Azure-Portal oder die Hotfix-Methode installieren.

Update 5.1 enthält nicht-unterbrechende Sicherheitsaktualisierungen, die über das Azure-Portal oder über die Hotfix-Methode angewendet werden können.

Wenn Sie Update 5.1 über das Azure-Portal anwenden, werden manuelle und automatische Vorprüfungen durchgeführt, um den Gerätezustand in Bezug auf den Hardwarezustand und die Netzwerkkonnektivität zu ermitteln. Diese Vorprüfungen finden nur statt, wenn Sie die Aktualisierungen über das Azure-Portal anwenden.

Wenn Sie es bevorzugen, die Hotfix-Methode zu verwenden, empfehlen wir Ihnen dringend, zuerst Update 5 zu installieren, indem Sie die Anweisungen unter [Update 5 auf Ihrem StorSimple-Gerät installieren befolgen](storsimple-8000-install-update-5.md). Führen Sie dann die Schritte unter [Update 5.1 als Hotfix](#install-update-51-as-a-hotfix) installieren, untenstehend, aus, um Update 5.1 zu installieren.

Die Installation der Sicherheitsaktualisierungen in Update 5.1 nimmt etwa 30 Minuten in Anspruch.

> [!IMPORTANT]
> * Update 5.1 ist ein obligatorisches Update und sollte sofort installiert werden. Weitere Informationen finden Sie in den [Versionshinweisen zu Update 5.1](storsimple-update51-release-notes.md).
> * Bei Update 5 handelt es sich um eine minimal unterstützte Version.

> [!NOTE]
> * Wir empfehlen, die Software und anderen regulären Updates über das Azure-Portal zu installieren. <!--You should only go to the Windows PowerShell interface of the device (to install updates) if the pre-update gateway check fails in the portal. Depending upon the version you are updating from, the updates may take 4 hours (or greater) to install. The maintenance mode updates must be installed through the Windows PowerShell interface of the device. As maintenance mode updates are disruptive updates, these result in a down time for your device. - N/A FOR 5.1? No maintenance mode updates, and the security updates go quickly.-->
> * Wenn Sie die Installation mit der Hotfix-Methode planen, müssen Sie den [Microsoft-Support](mailto:support@microsoft.com) kontaktieren, bevor Sie mit der Installation beginnen.

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-51-through-the-azure-portal"></a>Installieren von Update 5.1 über das Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihr Gerät auf [Update 5.1](storsimple-update51-release-notes.md) zu aktualisieren.

> [!NOTE]
> Microsoft ruft zusätzliche Diagnoseinformationen vom Gerät ab. Dadurch können wir besser Informationen auf dem Gerät sammeln und Probleme diagnostizieren, wenn unser Betriebsteam Geräte mit Problemen ermittelt.

#### <a name="to-install-an-update-from-the-azure-portal"></a>So installieren Sie ein Update über das Azure-Portal

1. Wählen Sie auf der Seite des StorSimple-Diensts Ihr Gerät aus.

    ![Gerät auswählen](./media/storsimple-8000-install-update-51/update1.png)

2. Navigieren Sie zu **Geräteeinstellungen** > **Geräteupdates**.

    ![Screenshot: Blatt „Einstellungen“ mit der hervorgehobenen Option „Geräteupdates“](./media/storsimple-8000-install-update-51/update2.png)

3. Eine Benachrichtigung wird angezeigt, wenn neue Updates verfügbar sind. Alternativ können Sie auf dem Blatt **Geräteupdates** auf **Updates scannen** klicken. Ein Auftrag für die Suche nach verfügbaren Updates wird erstellt. Sie werden benachrichtigt, wenn der Auftrag erfolgreich abgeschlossen wurde.

    ![Screenshot: Blatt „Einstellungen“ mit der hervorgehobenen Option „Geräteupdates“ und Blatt „Geräteupdates“ mit der hervorgehobenen Meldung „Es sind neue reguläre Updates verfügbar.“](./media/storsimple-8000-install-update-51/update3.png)

4. Es empfiehlt sich, die Versionshinweise zu lesen, bevor Sie ein Update auf Ihr Gerät anwenden. Klicken Sie zum Anwenden von Updates auf **Updates installieren**. Überprüfen Sie auf dem Blatt **Reguläre Updates bestätigen** die Voraussetzungen, bevor Sie Updates anwenden. Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie bereit sind, das Gerät zu aktualisieren, und klicken Sie dann auf **Installieren**.

    ![Screenshot: Blatt „Geräteupdates“, auf dem die Option „Updates installieren“ hervorgehoben ist, und Blatt „Reguläre Updates bestätigen“, auf dem die Zustimmungsoption sowie die Option „Installieren“ hervorgehoben sind](./media/storsimple-8000-install-update-51/update4.png)

5. Eine Reihe von Voraussetzungsüberprüfungen wird gestartet. Diese Prüfungen umfassen:
   
   * **Controller Integritätsprüfungen** , um sicherzustellen, dass die beiden Gerätecontroller fehlerfrei und online sind.
   * **Integritätsprüfungen für Hardware-Komponente** , um zu überprüfen, ob alle Hardwarekomponenten auf dem StorSimple-Gerät fehlerfrei sind.
   * **DATA 0-Prüfungen** , um sicherzustellen, dass DATA 0 auf Ihrem Gerät aktiviert ist. Wenn diese Schnittstelle nicht aktiviert ist, müssen Sie sie aktivieren, und es dann erneut versuchen.

     Das Update wird nur heruntergeladen und installiert, wenn alle Überprüfungen erfolgreich abgeschlossen wurden. Sie werden benachrichtigt, wenn die Prüfungen ausgeführt werden. Wenn bei den Vorüberprüfungen Fehler auftreten, werden die Fehlergründe angegeben. Beheben Sie diese Probleme, und wiederholen Sie den Vorgang anschließend. Möglicherweise müssen Sie den Microsoft Support kontaktieren, wenn Sie diese Probleme nicht selbst beheben können.

6. Wenn die Vorüberprüfungen erfolgreich abgeschlossen wurden, wird ein Updateauftrag erstellt. Sie werden benachrichtigt, wenn der Updateauftrag erfolgreich erstellt wurde.
   
    ![Screenshot: Benachrichtigung „Der Auftrag für Softwareupdates wird auf dem Gerät "{0}" gestartet.“](./media/storsimple-8000-install-update-51/update6.png)
   
    Das Update wird dann auf Ihr Gerät angewendet.

7. Das Update dauert einige Stunden. Durch Auswählen des Updateauftrags und Klicken auf **Details** können Sie jederzeit die Details des Auftrags anzeigen.

    ![Screenshot: Blatt „Geräteupdates“ mit hervorgehobener Option „Die Softwareupdates werden heruntergeladen und installiert“ und Blatt „Updates installieren“](./media/storsimple-8000-install-update-51/update8.png)

     Sie können den Fortschritt des Updateauftrags auch über **Geräteeinstellungen > Aufträge** überwachen. Auf dem Blatt **Aufträge** sehen Sie den Updatestatus.

     ![Screenshot: Blatt „Einstellungen“ mit hervorgehobener Option „Aufträge“ und Blatt „Aufträge“ mit dem Status des Updates](./media/storsimple-8000-install-update-51/update7.png)

8. Nachdem der Auftrag abgeschlossen ist, navigieren Sie zu **Geräteeinstellungen > Geräteupdates**. Die Softwareversion sollte jetzt aktualisiert sein.

   Stellen Sie sicher, dass auf Ihrem Gerät das **StorSimple 8000 Series Update 5.1 (6.3.9600.17885)** läuft und das **Datum der letzten Aktualisierung** das heutige Datum ist.

## <a name="install-update-51-as-a-hotfix"></a>Installieren Sie Update 5.1 als Hotfix

Wenn Sie Update 5.1 als Hotfix installieren möchten, führen Sie die folgenden Schritte aus, bevor Sie mit der Installation beginnen:

* Installieren Sie Update 5, bevor Sie Update 5.1 installieren. Anweisungen finden Sie unter [Installieren von Update 5 auf Ihrem StorSimple Gerät](storsimple-8000-install-update-5.md).
* Bevor Sie mit der Hotfix-Installation beginnen, kontaktieren Sie den [Microsoft-Support](mailto:support@microsoft.com).

Das Hotfixverfahren umfasst die folgenden Schritte:

1. Laden Sie die Hotfixes aus dem Microsoft-Aktualisierungskatalog herunter.
2. Installieren und überprüfen Sie die Hotfixes für den normalen Modus.
3. Installieren und überprüfen Sie den Hotfix für den Wartungsmodus.

#### <a name="download-updates-for-your-device"></a>Herunterladen von Updates für Ihr Gerät

Sie müssen die folgenden Hotfixes in die vorgeschlagenen Ordner in der vorgeschriebenen Reihenfolge herunterladen.

| Auftrag | KB       | Beschreibung | Updatetyp | Installationszeit |Installationsordner|
|-------|----------|------------ |-------------|--------------|----- |
|1.     |.KB4542887|Softwareupdate<br>Laden Sie _HcsSoftwareUpdate.exe_ und _CisMSDAgent.exe_ herunter. |Regulär <br></br>Unterbrechungsfrei |ca. 25 Min. |FirstOrderUpdate|
|3.<sup>1, 2</sup>     |KB4037263|Datenträgerfirmware|Wartung <br></br>Mit Unterbrechung|~ 30 Min.|ThirdOrderUpdate|

<sup>1</sup> In Update 5.1 sind keine Aktualisierungen zweiter Ordnung enthalten.

<sup>2</sup> Installieren Sie die Aktualisierungen dritter Ordnung, wenn Sie die Datenträgerfirmware-Aktualisierungen nicht zusätzlich zu den Hotfix-Aktualisierungen für Update 5 installiert haben.

Führen Sie die folgenden Schritte aus, um diese Datei herunterzuladen und die Hotfixes zu installieren.

#### <a name="download-hotfixes"></a>Laden Sie die Hotfixes herunter

Informationen zum Herunterladen der Hotfixes finden Sie unter [So laden Sie Hotfixes herunter.](storsimple-8000-install-update-5.md#to-download-hotfixes)

#### <a name="install-and-verify-device-updates"></a>Installieren und Überprüfen von Geräte-Aktualisierungen

Installieren Sie die Geräte-Aktualisierungen in KB4542887, indem Sie die Schritte in [So installieren und verifizieren Sie Hotfixes](storsimple-8000-install-update-5.md#to-install-and-verify-regular-mode-hotfixes)in **Installieren Sie Update 5 auf Ihrem StorSimple-Gerät** befolgen.

> [!IMPORTANT]
> Wenn Sie den [Microsoft-Support](mailto:support@microsoft.com)noch nicht kontaktiert haben, müssen Sie das jetzt tun, bevor Sie die Hotfixes installieren.

Folgen Sie den Schritten, um Aktualisierungen der ersten Ordnung zu installieren. In Update 5.1 sind keine Aktualisierungen zweiter Ordnung enthalten.

Für Update 5.1 suchen Sie nach der Installation nach diesen Softwareversionen:

 * FriendlySoftwareVersion: StorSimple 8000 Series Update 5.1
 * HcsSoftwareVersion: 6.3.9600.17885
 * CisAgentVersion: 1.0.9777.0
 * MdsAgentVersion: 35.2.2.0
 * Lsisas2Version: 2.0.78.00

#### <a name="install-and-verify-disk-firmware-updates"></a>Installieren und Überprüfen Sie die Datenträgerfirmware-Aktualisierungen

Wenn Sie bei der Installation von Update 5 keine Aktualisierungen der Datenträgerfirmware installiert haben, installieren Sie die Aktualisierungen der Datenträgerfirmware in KB4037263, indem Sie die Schritte unter [So installieren und überprüfen Sie Hotfixes](storsimple-8000-install-update-5.md#to-install-and-verify-maintenance-mode-hotfixes) für den regulären Modus in **Installieren von Update 5 auf Ihrem StorSimple-Gerät** ausführen.

Sie müssen keine Datenträgerfirmware-Aktualisierungen installieren, wenn Sie diese Firmwareversionen ausführen: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`.

Sie können überprüfen, ob Sie Aktualisierungen für die Datenträgerfirmware benötigen, indem Sie das Cmdlet `Get-HcsFirmwareVersion` ausführen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Versionshinweise zu Update 5.1](storsimple-update51-release-notes.md).
