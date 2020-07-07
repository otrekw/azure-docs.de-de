---
title: Installieren von Update 5.1 auf einem Gerät der StorSimple 8000-Serie | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Update 5.1 für die StorSimple 8000-Serie auf Ihrem StorSimple-Gerät der 8000-Serie installieren.
services: storsimple
documentationcenter: NA
author: priestlg
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/05/2020
ms.author: v-grpr
ms.openlocfilehash: fb2123bdb0e12571a58a32e414a610665935ebb6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85508213"
---
# <a name="install-update-51-on-your-storsimple-device"></a>Installieren von Update 5.1 auf Ihrem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In diesem Tutorial wird beschrieben, wie Sie Update 5.1 auf einem StorSimple-Gerät mit einer früheren Softwareversion im Azure-Portal ausführen. <!--The hotfix method is used when you are trying to install Update 5.1 on a device running pre-Update 3 versions. The hotfix method is also used when a gateway is configured on a network interface other than DATA 0 of the StorSimple device and you are trying to update from a pre-Update 1 software version.-->

Update 5.1 umfasst Sicherheitsupdates, die unterbrechungsfrei ausgeführt werden. Die unterbrechungsfreien und regulären Updates können über das Azure-Portal angewendet werden. <!--or by the hotfix method-->erforderlich.

> [!IMPORTANT]
>
> * Update 5.1 ist ein obligatorisches Update und sollte sofort installiert werden. Weitere Informationen finden Sie in den [Versionshinweisen zu Update 5.1](storsimple-update51-release-notes.md).
> * Vor der Installation wird ein Satz manueller und automatischer Vorabprüfungen durchgeführt, mit denen die Geräteintegrität in Bezug auf Hardwarestatus und Netzwerkkonnektivität ermittelt wird. Diese Vorabprüfungen werden nur ausgeführt, wenn Sie die Updates aus dem Azure-Portal ausführen.
> * Wenn Sie sie mithilfe der Hotfix-Methode installieren möchten, wenden Sie sich an den [Microsoft-Support](mailto:support@microsoft.com).

<!--
> * We strongly recommend that when updating a device running versions prior to Update 3, you install the updates using hotfix method. If you encounter any issues, [log a support ticket](storsimple-8000-contact-microsoft-support.md). 


> * We recommend that you install the software and other regular updates via the Azure portal. You should only go to the Windows PowerShell interface of the device (to install updates) if the pre-update gateway check fails in the portal. Depending upon the version you are updating from, the updates may take 4 hours (or greater) to install. The maintenance mode updates must be installed through the Windows PowerShell interface of the device. As maintenance mode updates are disruptive updates, these result in a down time for your device.


> * If running the optional StorSimple Snapshot Manager, ensure that you have upgraded your Snapshot Manager version to Update 5.1 prior to updating the device.
-->

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-51-through-the-azure-portal"></a>Installieren von Update 5.1 über das Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihr Gerät auf [Update 5.1](storsimple-update51-release-notes.md) zu aktualisieren.

> [!NOTE]
> Microsoft ruft zusätzliche Diagnoseinformationen vom Gerät ab. Dadurch können wir besser Informationen auf dem Gerät sammeln und Probleme diagnostizieren, wenn unser Betriebsteam Geräte mit Problemen ermittelt.

#### <a name="to-install-an-update-from-the-azure-portal"></a>So installieren Sie ein Update über das Azure-Portal

1. Wählen Sie auf der Seite des StorSimple-Diensts Ihr Gerät aus.

    ![Gerät auswählen](./media/storsimple-8000-install-update-51/update1.png)

2. Navigieren Sie zu **Geräteeinstellungen** > **Geräteupdates**.

    ![Klicken Sie auf „Geräteupdates“.](./media/storsimple-8000-install-update-51/update2.png)

3. Eine Benachrichtigung wird angezeigt, wenn neue Updates verfügbar sind. Alternativ können Sie auf dem Blatt **Geräteupdates** auf **Updates scannen** klicken. Ein Auftrag für die Suche nach verfügbaren Updates wird erstellt. Sie werden benachrichtigt, wenn der Auftrag erfolgreich abgeschlossen wurde.

    ![Klicken Sie auf „Geräteupdates“.](./media/storsimple-8000-install-update-51/update3.png)

4. Es empfiehlt sich, die Versionshinweise zu lesen, bevor Sie ein Update auf Ihr Gerät anwenden. Klicken Sie zum Anwenden von Updates auf **Updates installieren**. Überprüfen Sie auf dem Blatt **Reguläre Updates bestätigen** die Voraussetzungen, bevor Sie Updates anwenden. Aktivieren Sie das Kontrollkästchen, um anzugeben, dass Sie bereit sind, das Gerät zu aktualisieren, und klicken Sie dann auf **Installieren**.

    ![Klicken Sie auf „Geräteupdates“.](./media/storsimple-8000-install-update-51/update4.png)

5. Eine Reihe von Voraussetzungsüberprüfungen wird gestartet. Diese Prüfungen umfassen:
   
   * **Controller Integritätsprüfungen** , um sicherzustellen, dass die beiden Gerätecontroller fehlerfrei und online sind.
   * **Integritätsprüfungen für Hardware-Komponente** , um zu überprüfen, ob alle Hardwarekomponenten auf dem StorSimple-Gerät fehlerfrei sind.
   * **DATA 0-Prüfungen** , um sicherzustellen, dass DATA 0 auf Ihrem Gerät aktiviert ist. Wenn diese Schnittstelle nicht aktiviert ist, müssen Sie sie aktivieren, und es dann erneut versuchen.

     Das Update wird nur heruntergeladen und installiert, wenn alle Überprüfungen erfolgreich abgeschlossen wurden. Sie werden benachrichtigt, wenn die Prüfungen ausgeführt werden. Wenn bei den Vorüberprüfungen Fehler auftreten, werden die Fehlergründe angegeben. Beheben Sie diese Probleme, und wiederholen Sie den Vorgang anschließend. Möglicherweise müssen Sie den Microsoft Support kontaktieren, wenn Sie diese Probleme nicht selbst beheben können.

7. Wenn die Vorüberprüfungen erfolgreich abgeschlossen wurden, wird ein Updateauftrag erstellt. Sie werden benachrichtigt, wenn der Updateauftrag erfolgreich erstellt wurde.
   
    ![Erstellen eines Updateauftrags](./media/storsimple-8000-install-update-51/update6.png)
   
    Das Update wird dann auf Ihr Gerät angewendet.

9. Das Update dauert einige Stunden. Durch Auswählen des Updateauftrags und Klicken auf **Details** können Sie jederzeit die Details des Auftrags anzeigen.

    ![Erstellen eines Updateauftrags](./media/storsimple-8000-install-update-51/update8.png)

     Sie können den Fortschritt des Updateauftrags auch über **Geräteeinstellungen > Aufträge** überwachen. Auf dem Blatt **Aufträge** sehen Sie den Updatestatus.

     ![Erstellen eines Updateauftrags](./media/storsimple-8000-install-update-51/update7.png)

10. Nachdem der Auftrag abgeschlossen ist, navigieren Sie zu **Geräteeinstellungen > Geräteupdates**. Die Softwareversion sollte jetzt aktualisiert sein.


Vergewissern Sie sich, dass auf Ihrem Gerät **Update 5.1 der StorSimple 8000-Serie (6.3.9600.17885)** ausgeführt wird. Das **Datum der letzten Aktualisierung** sollte sich ändern.
<!-- 5.1 - KB 4542887-->

<!--You will now see that the Maintenance mode updates are available (this message might continue to be displayed for up to 24 hours after you install the updates). The steps to install maintenance mode update are detailed in the next section.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## Install Update 5.1 as a hotfix

The software versions that can be upgraded using the hotfix method are:

* Update 0.1, 0.2, 0.3
* Update 1, 1.1, 1.2
* Update 2, 2.1, 2.2
* Update 3, 3.1
* Update 4
* Update 5

> [!NOTE]
> The recommended method to install Update 5.1 is through the Azure portal when trying to update from Update 3 and later version. When updating a device running versions prior to Update 3, use this procedure. You can also use this procedure if you fail the gateway check when trying to install the updates through the Azure portal. The check fails when you have a gateway assigned to a non-DATA 0 network interface and your device is running a software version earlier than Update 1.

The hotfix method involves the following three steps:

1. Download the hotfixes from the Microsoft Update Catalog.
2. Install and verify the regular mode hotfixes.
3. Install and verify the maintenance mode hotfix.

#### Download updates for your device

You must download and install the following hotfixes in the prescribed order and the suggested folders:

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Software update<br> Download both _HcsSoftwareUpdate.exe_ and _CisMSDAgent.exe_ |Regular <br></br>Non-disruptive |~ 25 mins |FirstOrderUpdate|

If updating from a device running Update 4, you only need to install the OS cumulative updates as second order updates.

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |OS cumulative updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|

If installing from a device running Update 3 or earlier, install the following in addition to the cumulative updates.

| Order | KB | Description | Update type | Install time |Install in folder|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI driver and firmware updates <br> USM firmware update (version 3.38) |Regular <br></br>Non-disruptive |~ 3 hrs <br> (includes 2A. + 2B. + 2C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |OS security updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |OS updates package <br> Download Windows Server 2012 R2 version |Regular <br></br>Non-disruptive |- |SecondOrderUpdate|



You may also need to install disk firmware updates on top of all the updates shown in the preceding tables. You can verify whether you need the disk firmware updates by running the `Get-HcsFirmwareVersion` cmdlet. If you are running these firmware versions: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, then you do not need to install these updates.

| Order | KB | Description | Update type | Install time | Install in folder|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Disk firmware |Maintenance <br></br>Disruptive |~ 30 mins | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * If updating from Update 4, the total install time is close to 4 hours.
> * Before using this procedure to apply the update, make sure that both the device controllers are online and all the hardware components are healthy.

Perform the following steps to download and install the hotfixes.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]
-->
<!--
[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]
-->

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Versionshinweise zu Update 5.1](storsimple-update51-release-notes.md).
