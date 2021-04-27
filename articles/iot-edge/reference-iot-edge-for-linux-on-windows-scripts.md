---
title: PowerShell-Skripts für Azure IoT Edge für Linux unter Windows | Microsoft-Dokumentation
description: Referenzinformationen zu Azure IoT Edge für Linux unter Windows PowerShell-Skripts zum Bereitstellen und Abrufen des Status von IoT Edge für Linux auf virtuellen Windows-Computern.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 02/16/2021
ms.topic: reference
ms.service: iot-edge
services: iot-edge
monikerRange: =iotedge-2018-06
ms.openlocfilehash: a24b39107d8f78c049afa230fe678ec92852eeb0
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105959686"
---
# <a name="powershell-scripts-for-iot-edge-for-linux-on-windows"></a>PowerShell-Skripts für IoT Edge für Linux unter Windows

[!INCLUDE [iot-edge-version-201806](../../includes/iot-edge-version-201806.md)]

Informieren Sie sich über die PowerShell-Skripts, mit denen Sie Ihr IoT Edge für Linux auf einem virtuellen Windows-Computer bereitstellen und seinen Status abrufen können.

Die in diesem Artikel beschriebenen Befehle stammen aus der Datei `AzureEFLOW.psm1`, die Sie auf Ihrem System in Ihrem Verzeichnis `WindowsPowerShell` unter `C:\Program Files\WindowsPowerShellModules\AzureEFLOW` finden können.

## <a name="deploy-eflow"></a>Deploy-Eflow

Der Befehl **Deploy-Eflow** ist die Hauptbereitstellungsmethode. Der Bereitstellungsbefehl erstellt den virtuellen Computer und stellt Dateien sowie das IoT Edge-Agentmodul bereit. Obwohl keiner der Parameter erforderlich ist, kann damit während der Bereitstellung Ihr IoT Edge-Gerät bereitgestellt werden, und es können während der Erstellung Einstellungen für den virtuellen Computer geändert werden. Eine vollständige Liste können Sie mit dem Befehl `Get-Help Deploy-Eflow -full` anzeigen.  

>[!NOTE]
>Im Hinblick auf den Bereitstellungstyp bezieht sich **X509** zurzeit exklusiv auf die X509-Bereitstellung mithilfe eines [Azure IoT Hub Device Provisioning-Diensts](../iot-dps/about-iot-dps.md). Die manuelle X509-Bereitstellungsmethode wird zurzeit nicht unterstützt.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| eflowVhdxDir | Verzeichnispfad | Verzeichnis, in dem die Bereitstellung die VHDX-Datei für die VM speichert. |
| provisioningType | **manuell**, **TPM**, **X509** oder **symmetrisch** |  Definiert den Bereitstellungstyp, den Sie für Ihr IoT Edge-Gerät verwenden möchten. |
| devConnString | Die Geräteverbindungszeichenfolge eines vorhandenen IoT Edge-Geräts | Geräteverbindungszeichenfolge zum manuellen Bereitstellen eines IoT Edge-Geräts (**manuell**). |
| symmKey | Der Primärschlüssel für eine vorhandene DPS-Registrierung oder der Primärschlüssel eines vorhandenen IoT Edge-Geräts, das mithilfe von symmetrischen Schlüsseln registriert wurde | Symmetrischer Schlüssel für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| scopeId | Die Bereichs-ID für eine vorhandene DPS-Instanz. | Bereichs-ID für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| registrationId | Die Registrierungs-ID eines vorhandenen IoT Edge-Geräts | Registrierungs-ID für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| identityCertLocVm | Verzeichnispfad; muss sich in einem Ordner befinden, dessen Besitzer der `iotedge`-Dienst sein kann | Absoluter Zielpfad des Identitätszertifikats auf Ihrem virtuellen Computer für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| identityCertLocWin | Verzeichnispfad | Absoluter Quellpfad des Identitätszertifikats in Windows für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| identityPkLocVm | Verzeichnispfad; muss sich in einem Ordner befinden, dessen Besitzer der `iotedge`-Dienst sein kann | Absoluter Zielpfad des privaten Identitätsschlüssels auf Ihrem virtuellen Computer für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| identityPkLocWin | Verzeichnispfad | Absoluter Quellpfad des privaten Identitätsschlüssels in Windows für die Bereitstellung eines IoT Edge-Geräts (**X509** oder **symmetrisch**). |
| vmSizeDefintion | Nicht länger als 30 Zeichen | Definition der Anzahl von Kernen und des verfügbaren RAM für den virtuellen Computer. **Standardwert**: Standard_K8S_v1. |
| vmDiskSize | Zwischen 8 GB und 256 GB | Maximale Datenträgergröße der dynamisch erweiterbaren virtuellen Festplatte. **Standardwert**: 16 GB. |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |
| vnetType | **Transparent** oder **ICS** | Der Typ des virtuellen Netzwerks. **Standardwert**: Transparent. Transparent bezieht sich auf einen externen Switch, während ICS sich auf einen internen Switch bezieht. |
| vnetName | Nicht länger als 64 Zeichen | Der Name des virtuellen Netzwerks. **Standardwert**: Extern. |
| enableVtpm | Keine | **Switch-Parameter:** Erstellen Sie den virtuellen Computer mit aktiviertem oder deaktiviertem TPM. |
| mobyPackageVersion | Nicht länger als 30 Zeichen |  Version des Moby-Pakets, das auf dem virtuellen Computer überprüft oder installiert werden soll.  **Standardwert:** 19.03.11. |
| iotedgePackageVersion | Nicht länger als 30 Zeichen | Version des IoT Edge-Pakets, das auf dem virtuellen Computer überprüft oder installiert werden soll. **Standardwert:** 1.1.0. |
| installPackages | Keine | **Switch-Parameter:** Beim Umschalten versucht das Skript, die Pakete „Moby“ und „IoT Edge“ zu installieren, statt zu überprüfen, ob sie vorhanden sind. |

>[!NOTE]
>Wenn der Prozess keinen externen Switch mit dem Namen`External` findet, sucht er standardmäßig nach einem vorhandenen externen Switch, über den er eine IP-Adresse beziehen kann. Wenn kein externer Schalter vorhanden ist, sucht es nach einem internen Schalter. Wenn kein interner Switch verfügbar ist, wird es versuchen, den Standard-Switch zu erstellen, über den eine IP-Adresse bezogen werden kann.

## <a name="verify-eflowvm"></a>Verify-EflowVm

Der Befehl **Verify-EflowVm** ist eine verfügbar gemachte Funktion zur Überprüfung, ob der virtuelle IoT Edge für Linux unter Windows-Computer erstellt wurde. Es werden dafür nur allgemeine Parameter verwendet, und wenn der virtuelle Computer erstellt wurde, wird **true** zurückgegeben, andernfalls **false**. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Verify-EflowVm -full`.

## <a name="provision-eflowvm"></a>Provision-EflowVm

Der Befehl **Provision-EflowVm** fügt die Bereitstellungsinformationen für Ihr IoT Edge-Gerät der IoT Edge-Datei `config.yaml` des virtuellen Computers hinzu. Die Bereitstellung kann auch während der Bereitstellungsphase durch Festlegen von Parametern im Befehl **Deploy-Eflow** erfolgen. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Provision-EflowVm -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |
| provisioningType | **manuell**, **TPM**, **X509** oder **symmetrisch** |  Definiert den Bereitstellungstyp, den Sie für Ihr IoT Edge-Gerät verwenden möchten. |
| devConnString | Die Geräteverbindungszeichenfolge eines vorhandenen IoT Edge-Geräts | Geräteverbindungszeichenfolge zum manuellen Bereitstellen eines IoT Edge-Geräts (**manuell**). |
| symmKey | Der Primärschlüssel für eine vorhandene DPS-Registrierung oder der Primärschlüssel eines vorhandenen IoT Edge-Geräts, das mithilfe von symmetrischen Schlüsseln registriert wurde | Symmetrischer Schlüssel für die Bereitstellung eines IoT Edge-Geräts (**DPS**, **symmetrisch**). |
| scopeId | Die Bereichs-ID für eine vorhandene DPS-Instanz. | Bereichs-ID für die Bereitstellung eines IoT Edge-Geräts (**DPS**). |
| registrationId | Die Registrierungs-ID eines vorhandenen IoT Edge-Geräts | Registrierungs-ID für die Bereitstellung eines IoT Edge-Geräts (**DPS**). |
| identityCertLocVm | Verzeichnispfad; muss sich in einem Ordner befinden, dessen Besitzer der `iotedge`-Dienst sein kann | Absoluter Zielpfad des Identitätszertifikats auf Ihrem virtuellen Computer für die Bereitstellung eines IoT Edge-Geräts (**DPS**, **X509**). |
| identityCertLocWin | Verzeichnispfad | Absoluter Quellpfad des Identitätszertifikats in Windows für die Bereitstellung eines IoT Edge-Geräts (**DPS**, **X509**). |
| identityPkLocVm  | Verzeichnispfad; muss sich in einem Ordner befinden, dessen Besitzer der `iotedge`-Dienst sein kann | Absoluter Zielpfad des privaten Identitätsschlüssels auf Ihrem virtuellen Computer für die Bereitstellung eines IoT Edge-Geräts (**DPS**, **X509**). |
| identityPkLocWin | Verzeichnispfad | Absoluter Quellpfad des privaten Identitätsschlüssels in Windows für die Bereitstellung eines IoT Edge-Geräts (**DPS**, **X509**). |

## <a name="get-eflowvmname"></a>Get-EflowVmName

Mit dem Befehl **Get-EflowVmName** wird der aktuelle Hostname des virtuellen Computers abgefragt. So wird die Tatsache berücksichtigt, dass sich der Windows-Hostname im Laufe der Zeit ändern kann. Bei diesem Befehl werden nur allgemeine Parameter verwendet, und der aktuelle Hostname des virtuellen Computers wird zurückgegeben. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmName -full`.

## <a name="get-eflowlogs"></a>Get-EflowLogs

Mit dem Befehl **Get-EflowLogs** werden Protokolle aus der Bereitstellung von IoT Edge für Linux unter Windows erfasst und gebündelt. Die gebündelten Protokolle werden in Form eines `.zip`-Ordners ausgegeben. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowLogs -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmtpmprovisioninginfo"></a>Get-EflowVmTpmProvisioningInfo

Mit dem Befehl **Get-EflowVmTpmProvisioningInfo** werden die vTPM-Bereitstellungsinformationen des virtuellen Computers erfasst und angezeigt. Wenn der virtuelle Computer ohne vTPM erstellt wurde, gibt der Befehl zurück, dass die TPM-Bereitstellungsinformationen nicht gefunden werden konnten. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmTpmProvisioningInfo -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmaddr"></a>Get-EflowVmAddr

Mit dem Befehl **Get-EflowVmAddr** werden die IP- und MAC-Adresse des virtuellen Computers gesucht und angezeigt. Es werden dafür nur allgemeine Parameter verwendet. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmAddr -full`.

## <a name="get-eflowvmsysteminformation"></a>Get-EflowVmSystemInformation

Mit dem Befehl **Get-EflowVmSystemInformation** werden Systeminformationen aus dem virtuellen Computer erfasst und angezeigt, z. B. die Arbeitsspeicher- und Speicherauslastung. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmSystemInformation -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmedgeinformation"></a>Get-EflowVmEdgeInformation

Mit dem Befehl **Get-EflowVmEdgeInformation** werden IoT Edge-Informationen aus dem virtuellen Computer erfasst und angezeigt, z. B. die Version von IoT Edge, auf dem der virtuelle Computer ausgeführt wird. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmEdgeInformation -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmedgemodulelist"></a>Get-EflowVmEdgeModuleList

Mit dem Befehl **Get-EflowVmEdgeModuleList** wird die Liste von IoT Edge-Modulen, die auf dem virtuellen Computer ausgeführt werden, abgefragt und angezeigt. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmEdgeModuleList -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmedgestatus"></a>Get-EflowVmEdgeStatus

Mit dem Befehl **Get-EflowVmEdgeStatus** wird der Status von IoT Edge-Runtime auf dem virtuellen Computer abgefragt und angezeigt. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmEdgeStatus -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="get-eflowvmusername"></a>Get-EflowVmUserName

Mit dem Befehl **Get-EflowVmUserName** können Sie den Benutzernamen des virtuellen Computers abfragen und anzeigen, mit dem der virtuelle Computer aus der Registrierung erstellt wurde. Bei diesem Befehl werden nur allgemeine Parameter verwendet. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmUserName -full`.

## <a name="get-eflowvmsshkey"></a>Get-EflowVmSshKey

Mit dem Befehl **Get-EflowVmSshKey** wird der vom virtuellen Computer verwendete SSH-Schlüssel abgefragt und angezeigt. Bei diesem Befehl werden nur allgemeine Parameter verwendet. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Get-EflowVmSshKey -full`.

## <a name="ssh-eflowvm"></a>Ssh-EflowVm

Mit dem Befehl **Ssh-EflowVm** wird eine SSH-Anmeldung am virtuellen Computer durchgeführt. Das einzige Konto, das zum Herstellen eines SSH mit dem virtuellen Computer zulässig ist, ist der Benutzer, der es erstellt. Wenn Sie zusätzliche Informationen benötigen, verwenden Sie den Befehl `Get-Help Ssh-EflowVm -full`.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| vmUser | Nicht länger als 30 Zeichen | Benutzername für die Anmeldung am virtuellen Computer. |

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung dieser Befehle finden Sie im folgenden Artikel:

* [Installieren von Azure IoT Edge für Linux unter Windows](./how-to-install-iot-edge-windows-on-windows.md)

* Alle Befehle, die über PowerShell zur Verfügung stehen, finden Sie in der [Skriptreferenz zu IoT Edge für Linux in Windows PowerShell](reference-iot-edge-for-linux-on-windows-scripts.md#deploy-eflow).
