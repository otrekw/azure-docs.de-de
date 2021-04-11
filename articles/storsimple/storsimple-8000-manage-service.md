---
title: Bereitstellen des StorSimple-Geräte-Manager-Diensts in Azure | Microsoft-Dokumentation
description: Hier werden die erforderlichen Schritte zum Erstellen, Löschen und Migrieren des Diensts und die Verwaltung des Dienstsregistrierungsschlüssels beschrieben.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: 85d66b5e4f7e95d114cbf0b4c681d73eebb93a67
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106076563"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Bereitstellen des StorSimple-Geräte-Manager-Diensts für Geräte der StorSimple 8000-Serie

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Übersicht

Der StorSimple-Geräte-Manager-Dienst wird in Microsoft Azure ausgeführt. Er verfügt über Verbindungen mit mehreren StorSimple-Geräten. Nachdem Sie den Dienst erstellt haben, können Sie alle Geräte, die mit den StorSimple-Geräte-Manager-Dienst verbunden sind, von einem zentralen Standort aus verwalten und damit den Verwaltungsaufwand minimieren.

In diesem Tutorial werden die erforderlichen Schritte zum Erstellen, Löschen und Migrieren des Diensts und die Verwaltung des Dienstsregistrierungsschlüssels beschrieben. Die Informationen in diesem Artikel gelten nur für Geräte der StorSimple 8000-Serie. Weitere Informationen zu StorSimple Virtual Arrays finden Sie unter [Bereitstellen eines StorSimple-Geräte-Manager-Diensts für das StorSimple Virtual Array](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  Das Azure-Portal unterstützt Geräte mit ausgeführtem Update 5.0 oder höher. Wenn Ihr Gerät nicht auf dem neuesten Stand ist, installieren Sie Update 5 sofort. Weitere Informationen finden Sie unter [Installieren von Update 5](storsimple-8000-install-update-5.md). 
> - Wenn Sie eine StorSimple Cloud Appliance (8010/8020) verwenden, können Sie dieses Gerät nicht aktualisieren. Verwenden Sie die neueste Version der Software, um ein neues Cloudgerät mit Update 5.0 zu erstellen und dann ein Failover auf das neu erstellte Cloudgerät auszuführen. 
> - Alle Geräte, auf denen Update 4.0 oder eine frühere Version ausgeführt wird, weisen verminderte Verwaltungsfunktionalität auf. 

## <a name="create-a-service"></a>Erstellen von Diensten
Um einen StorSimple-Geräte-Manager-Dienst zu erstellen, benötigen Sie Folgendes:

* Ein Abonnement mit einem Enterprise Agreement
* Ein aktives Microsoft Azure-Speicherkonto
* Die Abrechnungsinformationen für die Zugriffsverwaltung

Nur Abonnements mit Enterprise Agreement sind zulässig. Sie können beim Erstellen des Diensts auch ein Standardspeicherkonto generieren.

Mit einem Dienst können mehrere Geräte verwaltet werden. Ein Gerät kann jedoch nicht mehrere Dienste umfassen. Große Unternehmen können mit mehreren Dienstinstanzen mit verschiedenen Abonnements, Organisationen oder sogar Bereitstellungsstandorten arbeiten. 

> [!NOTE]
> Sie benötigen separate Instanzen des StorSimple-Geräte-Manager-Diensts, um StorSimple-Geräte der 8000er-Serie und StorSimple Virtual Arrays zu verwalten.

Führen Sie die folgenden Schritte aus, um einen Dienst zu erstellen.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Für jeden StorSimple-Geräte-Manager-Dienst gibt es die folgenden Attribute:

* **Name:** Name, der dem StorSimple-Geräte-Manager-Dienst bei der Erstellung zugewiesen wurde. **Der Dienstname kann nicht mehr geändert werden, nachdem der Dienst erstellt wurde. Dies gilt auch für andere Entitäten wie z B. Geräte, Volumes, Volumecontainer und Sicherungsrichtlinien, die im Azure-Portal nicht umbenannt werden können.**
* **Status**: der Status des Diensts mit den möglichen Werten **Aktiv**, **Wird erstellt** oder **Online**.
* **Standort:** der geografische Standort, an dem das StorSimple-Gerät bereitgestellt wird.
* **Abonnement:** das Abonnement für die Abrechnung, das mit Ihrem Dienst verbunden ist.

## <a name="delete-a-service"></a>Löschen von Diensten

Bevor Sie einen Dienst löschen, stellen Sie sicher, dass er von keinen verbundenen Geräten verwendet wird. Wenn der Dienst verwendet wird, deaktivieren Sie die verbundenen Geräte. Der deaktivierte Vorgang trennt die Verbindung zwischen dem Gerät und dem Dienst, behält aber die Gerätedaten in der Cloud bei.

> [!IMPORTANT]
> Das Löschen eines Diensts kann nicht rückgängig gemacht werden. Jedes Gerät, das den Dienst verwendet hat, muss auf die Werkseinstellungen zurückgesetzt werden, bevor es mit einem anderen Dienst verwendet werden kann. In diesem Szenario gehen sowohl die lokalen Daten auf dem Gerät als auch die Konfiguration verloren.

Führen Sie die folgenden Schritte aus, um einen Dienst zu löschen.

### <a name="to-delete-a-service"></a>So löschen Sie einen Dienst

1. Suchen Sie den Dienst, den Sie löschen möchten. Klicken Sie auf das Symbol **Ressourcen**, und geben Sie dann die entsprechenden Suchbegriffe ein. Klicken Sie in den Suchergebnissen auf den Dienst, den Sie löschen möchten.

    ![Suchen des zu löschenden Diensts](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Sie gelangen zum Blatt „StorSimple-Geräte-Manager-Dienst“. Klicken Sie auf **Löschen**.

    ![Suchdienst löschen](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klicken Sie in der Bestätigungsbenachrichtigung auf **Ja** . Es dauert einige Minuten, bis der Dienst gelöscht wird.

    ![Löschvorgang bestätigen](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Abrufen des Dienstregistrierungsschlüssels

Nachdem Sie einen Dienst erstellt haben, müssen Sie Ihr StorSimple-Gerät bei dem Dienst registrieren. Um Ihr erstes StorSimple-Gerät zu registrieren, benötigen Sie den Dienstregistrierungsschlüssel. Um zusätzliche Geräte bei einem vorhandenen StorSimple-Dienst zu registrieren, benötigen Sie den Registrierungsschlüssel und den Verschlüsselungsschlüssel für Dienstdaten (die während der Registrierung auf dem ersten Gerät generiert werden). Weitere Informationen zum Verschlüsselungsschlüssel für Dienstdaten finden Sie unter [StorSimple-Sicherheit](storsimple-8000-security.md). Sie können den Registrierungsschlüssel auf dem Blatt „StorSimple-Geräte-Manager“ unter **Schlüssel** abrufen.

Führen Sie die folgenden Schritte durch, um den Dienstregistrierungsschlüssel abzurufen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Bewahren Sie den Dienstregistrierungsschlüssel an einem sicheren Ort auf. Sie benötigen diesen Schlüssel sowie den Verschlüsselungsschlüssel für Dienstdaten, um zusätzliche Geräte bei diesem Dienst zu registrieren. Nach dem Abrufen des Dienstregistrierungsschlüssels müssen Sie das Gerät mithilfe von Windows PowerShell für StorSimple konfigurieren.

Ausführliche Informationen zur Verwendung dieses Registrierungsschlüssels finden Sie unter [Schritt 3: Konfigurieren und Registrieren des Geräts über Windows PowerShell für StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Neugenerieren des Dienstregistrierungsschlüssels
Sie müssen den Dienstregistrierungsschlüssel neu generieren, wenn Sie die Schlüsselrotation durchführen müssen oder wenn sich die Liste der Dienstadministratoren geändert hat. Wenn Sie den Schlüssel neu generieren, wird der neue Schlüssel nur für die Registrierung nachfolgender Geräte verwendet. Bereits registrierte Geräte sind von diesem Vorgang nicht betroffen.

Führen Sie die folgenden Schritte durch, um den Dienstregistrierungsschlüssel neu zu generieren.

### <a name="to-regenerate-the-service-registration-key"></a>So generieren Sie den Dienstregistrierungsschlüssel neu
1. Wechseln Sie auf dem Blatt **StorSimple-Geräte-Manager** zu **Management &gt;** **Schlüssel**.
    
    ![Navigieren zum Blatt „Schlüssel“](./media/storsimple-8000-manage-service/regenregkey2.png)

2. Klicken Sie auf dem Blatt **Schlüssel** auf **Neu generieren**.

    ![Klicken Sie auf „Neu generieren“.](./media/storsimple-8000-manage-service/regenregkey3.png)
3. Überprüfen Sie auf dem Blatt **Dienstregistrierungsschlüssel erneut generieren**, welche Aktion erforderlich ist, wenn die Schlüssel neu generiert werden. Alle weiteren Geräte, die bei diesem Dienst angemeldet werden, verwenden den neuen Registrierungsschlüssel. Klicken Sie zum Bestätigen auf **Regenerieren**. Sie werden benachrichtigt, sobald die Registrierung abgeschlossen ist.

    ![Bestätigen des erneuten Generierens](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Ein neuer Dienstregistrierungsschlüssel wird angezeigt.

5. Kopieren Sie diesen Schlüssel, und speichern Sie ihn für die Registrierung neuer Geräte bei diesem Dienst.



## <a name="change-the-service-data-encryption-key"></a>Ändern des Verschlüsselungsschlüssels für Dienstdaten
[!INCLUDE [storage-files-migration-generate-key](../../includes/storage-files-migration-generate-key.md)]

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Unterstützte Vorgänge auf Geräten mit einer Version vor Update 5.0
Im Azure-Portal werden nur die StorSimple-Geräte mit ausgeführtem Update 5.0 und höher unterstützt. Die Geräte mit früheren Versionen werden eingeschränkt unterstützt. In der folgenden Tabelle sind die Vorgänge aufgeführt, die auf Geräten mit Softwareversionen vor Update 5.0 unterstützt werden, nachdem die Umstellung auf das Azure-Portal erfolgt ist.

| Vorgang                                                                                                                       | Unterstützt      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Registrieren eines Geräts                                                                                                               | Ja            |
| Konfigurieren von Geräteeinstellungen (allgemein, Netzwerk und Sicherheit)                                                                | Ja            |
| Suchen nach, Herunterladen und Installieren von Updates                                                                                             | Ja            |
| Deaktivieren des Geräts                                                                                                               | Ja            |
| Löschen des Geräts                                                                                                                   | Ja            |
| Erstellen, Ändern und Löschen eines Volumecontainers                                                                                   | Nein             |
| Erstellen, Ändern und Löschen eines Volumes                                                                                             | Nein             |
| Erstellen, Ändern und Löschen einer Sicherungsrichtlinie                                                                                      | Nein             |
| Erstellen einer manuellen Sicherung                                                                                                            | Nein             |
| Erstellen einer geplanten Sicherung                                                                                                         | Nicht verfügbar |
| Wiederherstellen eines Sicherungssatzes                                                                                                        | Nein             |
| Klonen eines Gerät mit ausgeführtem Update 3.0 und höher <br> Auf dem Quellgerät wird eine Version vor Update 3.0 ausgeführt.                                | Ja            |
| Klonen eines Gerät mit ausgeführter Version vor Update 3.0                                                                          | Nein             |
| Failover als Quellgerät <br> (von einem Gerät mit Version vor Update 3.0 auf ein Gerät mit Update 3.0 und höher)                                                               | Ja            |
| Failover als Zielgerät <br> (auf ein Gerät mit Softwareversion vor Update 3.0)                                                                                   | Nein             |
| Erstellen einer Warnung                                                                                                                  | Ja            |
| Anzeigen von im klassischen Portal erstellten Sicherungsrichtlinien, Sicherungskatalogen, Volumes, Volumecontainern, Überwachungsdiagrammen, Aufträgen und Warnungen | Ja            |
| Aktivieren und Deaktivieren von Gerätecontrollern                                                                                              | Ja            |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [StorSimple-Bereitstellungsprozess](storsimple-8000-deployment-walkthrough-u2.md)
* Weitere Informationen über das [Verwalten des StorSimple-Speicherkontos](storsimple-8000-manage-storage-accounts.md)
* Weitere Informationen über das [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple-Geräte-Manager-Diensts](storsimple-8000-manager-service-administration.md)
