---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 2/20/2020
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: a142de62f1ab7046d47c29753863b16864d97536
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075575"
---
Dienstdaten-Verschlüsselungsschlüssel werden verwendet, um vertrauliche Kundendaten zu verschlüsseln, beispielsweise Anmeldeinformationen für das Speicherkonto, die vom StorSimple Manager-Dienst zum StorSimple-Gerät gesendet werden. Sie müssen diese Schlüssel regelmäßig ändern, wenn Ihre IT-Organisation über eine Richtlinie zur Schlüsselrotation für Speichergeräte verfügt. Das Verfahren zur Schlüsseländerung kann variieren, je nachdem, ob ein einzelnes Gerät oder mehrere Geräte vom StorSimple Manager-Dienst verwaltet werden. Weitere Informationen finden Sie unter [StorSimple-Sicherheit und -Datenschutz](../articles/storsimple/storsimple-8000-security.md).

Das Ändern den Verschlüsselungsschlüssel für Dienstdaten wird in drei Schritten vollzogen:

1. Autorisieren Sie mithilfe von Windows PowerShell-Skripts für Azure Resource Manager ein Gerät, um den Verschlüsselungsschlüssel für Dienstdaten zu ändern.
2. Sie verwenden Windows PowerShell für StorSimple, um die Änderung des Verschlüsselungsschlüssels für Dienstdaten zu initialisieren.
3. Wenn Sie über mehrere StorSimple-Geräte verfügen, aktualisieren Sie den Verschlüsselungsschlüssel für Dienstdaten auf den anderen Geräten.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Schritt 1: Autorisieren eines Geräts mithilfe eines Windows PowerShell-Skripts, um den Verschlüsselungsschlüssel für Dienstdaten zu ändern
Üblicherweise fordert der Geräteadministrator den Dienstadministrator auf, ein Gerät zu autorisieren, um den Verschlüsselungsschlüssel für Dienstdaten zu ändern. Der Dienstadministrator autorisiert dann das Gerät, um den Schlüssel zu ändern.

Dieser Schritt wird mithilfe des Azure Resource Manager-basierten Skripts ausgeführt. Der Dienstadministrator kann ein Gerät auswählen, das für die Autorisierung geeignet ist. Das Gerät wird dann autorisiert und der Vorgang zur Änderung des Verschlüsselungsschlüssels für Dienstdaten wird gestartet. 

Weitere Informationen zum Verwenden des Skripts finden Sie unter [Authorize-ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1).

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Welche Geräte können zur Änderung des Verschlüsselungsschlüssels für Dienstdaten autorisiert werden?
Ein Gerät muss die folgenden Kriterien erfüllen, bevor es für die Initiierung des Änderungsvorgangs des Verschlüsselungsschlüssels für Dienstdaten autorisiert werden kann:

* Für die Autorisierung zum Ändern des Verschlüsselungsschlüssels für Dienstdaten muss das Gerät online sein.
* Wurde die Änderung des Schlüssels nicht initiiert, können Sie dasselbe Gerät nach 30 Minuten erneut autorisieren.
* Wurde die Änderung des Schlüssel nicht durch das zuvor autorisierte Gerät initialisiert, können Sie auch ein anderes Gerät autorisieren. Nachdem das neue Gerät autorisiert wurde, kann die Änderung nicht durch das alte Gerät initiiert werden.
* Während des Rollover-Prozesses des Verschlüsselungsschlüssel für Dienstdaten können Sie kein Gerät autorisieren.
* Eine Geräte-Autorisierung ist möglich, sobald einige der beim Dienst registrierten Geräte den Verschlüsselungs-Rollover abgeschlossen haben. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Schritt 2: Verwenden von Windows PowerShell für StorSimple, um die Änderung des Verschlüsselungsschlüssels für Dienstdaten zu initialisieren
Dieser Schritt wird in der Windows PowerShell für StorSimple-Schnittstelle auf dem autorisierten StorSimple-Gerät ausgeführt.

> [!NOTE]
> Bis zum Abschluss des Schlüsselrollovers kann im Azure-Portal des StorSimple Managers kein Vorgang erfolgen.


Wenn Sie die Verbindung zur Windows PowerShell-Schnittstelle über die serielle Gerätekonsole herstellen, führen Sie die folgenden Schritte durch.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Initiierung der Änderung des Verschlüsselungsschlüssels für Dienstdaten
1. Wählen Sie Option 1, um sich mit Vollzugriff anzumelden.
2. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Nachdem das Cmdlet erfolgreich abgeschlossen wurde, erhalten Sie einen neuen Verschlüsselungsschlüssel für Dienstdaten. Kopieren und speichern Sie diesen Schlüssel, da Sie ihn für Schritt 3 dieses Prozesses benötigen. Dieser Schlüssel wird zur Aktualisierung aller Geräte benötigt, die beim StorSimple Manager-Dienst registriert sind.
   
   > [!NOTE]
   > Dieser Prozess muss innerhalb von vier Stunden nach der Autorisierung eines StorSimple-Geräts initiiert werden.
   > 
   > 
   
   Der neue Schlüssel wird dann an den Dienst gesendet und an alle bei diesem Dienst registrierten Geräte übermittelt. Im Dienst-Dashboard wird eine Warnung angezeigt. Der Dienst deaktiviert alle Vorgänge auf den registrierten Geräten. Dann muss der Geräteadministrator den Verschlüsselungsschlüssel für Dienstdaten auf den anderen Geräten aktualisieren. E/A-Vorgänge (von Hosts an die Cloud übermittelte Daten) werden jedoch nicht unterbrochen.
   
   Wenn Sie nur ein Gerät beim Dienst registriert haben, ist der Rollover-Prozess nun abgeschlossen und Sie können den nächsten Schritt überspringen. Wenn Sie mehrere Geräte beim Dienst registriert haben, fahren Sie mit Schritt 3 fort.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Schritt 3: Aktualisieren des Verschlüsselungsschlüssels für Dienstdaten auf anderen StorSimple-Geräten
Wenn Sie mehrere Geräte bei Ihrem StorSimple Manager-Dienst registriert haben, müssen Sie die folgenden Schritte in der Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts durchführen. Der Schlüssel, den in Schritt 2 abgerufen haben, muss zum Aktualisieren aller verbleibenden StorSimple-Geräte genutzt werden, die beim StorSimple Manager-Dienst registriert sind.

Führen Sie die folgenden Schritte aus, um den Verschlüsselungsschlüssel für Dienstdaten auf Ihrem Gerät zu aktualisieren.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>So aktualisieren Sie den Dienstdatenverschlüsselungs-Schlüssel auf physischen Geräten
1. Verwenden Sie Windows PowerShell für StorSimple für die Verbindung mit der Konsole. Wählen Sie Option 1, um sich mit Vollzugriff anzumelden.
2. Geben Sie an der Eingabeaufforderung Folgendes ein: `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`.
3. Geben Sie den Dienstdatenverschlüsselungs-Schlüssel an, den Sie in [Schritt2: Verwenden von Windows PowerShell für StorSimple, um die Änderung des Verschlüsselungsschlüssels für Dienstdaten zu initialisieren](#to-initiate-the-service-data-encryption-key-change) abgerufen haben.

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>So aktualisieren Sie den Dienstdatenverschlüsselungs-Schlüssel auf allen 8010/8020 Cloud Appliances
1. Laden Sie das PowerShell-Skript [Update-CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) herunter, und richten Sie es ein. 
2. Öffnen Sie PowerShell, und geben Sie an der Eingabeaufforderung Folgendes ein: `Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Mit diesem Skript wird sichergestellt, dass der Dienstdatenverschlüsselungs-Schlüssel in allen 8010/8020 Cloud Appliances unter dem Device Manager festgelegt wird.