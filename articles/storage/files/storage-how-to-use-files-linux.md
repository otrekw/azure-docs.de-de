---
title: Verwenden von Azure Files mit Linux | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Linux eine Azure-Dateifreigabe über SMB einbinden. Lesen Sie die Liste der Voraussetzungen. Überprüfen Sie die Sicherheitsüberlegungen zu SMB auf Linux-Clients.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/05/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 7e02d85fe5385b8918fbfdb037382aeeef444267
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110088352"
---
# <a name="use-azure-files-with-linux"></a>Verwenden von Azure Files mit Linux
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können mithilfe des [SMB-Kernelclients](https://wiki.samba.org/index.php/LinuxCIFS) in Linux-Distributionen eingebunden werden.

Es wird empfohlen, zum Einbinden einer Azure-Dateifreigabe unter Linux SMB 3.1.1 zu verwenden. Standardmäßig müssen Daten bei Azure Files während der Übertragung verschlüsselt werden. Dies wird nur von SMB 3.0+ unterstützt. Azure Files unterstützt auch die Version 2.1 von SMB, bei der allerdings keine Verschlüsselung während der Übertragung möglich ist. Deshalb können Sie mit SMB 2.1 aus Sicherheitsgründen keine Azure-Dateifreigaben aus einer anderen Region oder lokal einbinden. Verwenden Sie SMB 3.1.1, sofern Ihre Anwendung nicht ausdrücklich SMB 2.1 erfordert.

| Distribution | SMB 3.1.1 | SMB 3.0 |
|-|-----------|---------|
| Linux-Kernelversion | <ul><li>Basic 3.1.1-Unterstützung: 4.17</li><li>Standard-Bereitstellung: 5.0</li><li>AES-128-GCM-Verschlüsselung: 5.3</li></ul> | <ul><li>Basic 3.0-Unterstützung: 3.12</li><li>AES-128-CCM-Verschlüsselung: 4.11</li></ul> |
| [Ubuntu](https://wiki.ubuntu.com/Releases) | AES-128-GCM-Verschlüsselung: 18.04.5 LTS+ | AES-128-CCM-Verschlüsselung: 16.04.4 LTS+ |
| [Red Hat Enterprise Linux (RHEL)](https://access.redhat.com/articles/3078) | <ul><li>Basic: 8.0+</li><li>Standard-Bereitstellung: 8.2+</li><li>AES-128-GCM-Verschlüsselung: 8.2+</li></ul> | 7.5 und höher |
| [Debian](https://www.debian.org/releases/) | Basic: 10+ | AES-128-CCM-Verschlüsselung: 10+ |
| [SUSE Linux Enterprise Server](https://www.suse.com/support/kb/doc/?id=000019587) | AES-128-GCM-Verschlüsselung: 15 SP2+ | AES-128-CCM-Verschlüsselung: 12 SP2+ |

Wenn Ihre Linux-Distribution in der vorstehenden Tabelle nicht aufgeführt ist, können Sie die Linux-Kernelversion mit dem Befehl `uname` überprüfen:

```bash
uname -r
```

> [!Note]  
> Zur Linux-Kernelversion 3.7 wurde Unterstützung für SMB 2.1 hinzugefügt. Wenn Sie eine Version des Linux-Kernels nach 3.7 verwenden, sollte sie SMB 2.1 unterstützen.

## <a name="prerequisites"></a>Voraussetzungen
<a id="smb-client-reqs"></a>

* <a id="install-cifs-utils"></a>**Vergewissern Sie sich, dass das cifs-utils-Paket installiert ist.**  
    Das Paket „cifs-utils“ kann mithilfe des Paket-Managers für die Linux-Distribution Ihrer Wahl installiert werden. 

    Verwenden Sie unter **Ubuntu** und **Debian** den Paket-Manager `apt`:

    ```bash
    sudo apt update
    sudo apt install cifs-utils
    ```

    Verwenden Sie unter **Red Hat Enterprise Linux 8+** den Paket-Manager `dnf`:

    ```bash
    sudo dnf install cifs-utils
    ```

    Verwenden Sie unter älteren Versionen von **Red Hat Enterprise Linux** den Paket-Manager `yum`:

    ```bash
    sudo yum install cifs-utils 
    ```

    Verwenden Sie unter **SUSE Linux Enterprise Server** den Paket-Manager `zypper`:

    ```bash
    sudo zypper install cifs-utils
    ```

    Verwenden Sie bei anderen Distributionen den entsprechenden Paket-Manager, oder [kompilieren Sie den Quellcode](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Die neueste Version der Azure-Befehlszeilenschnittstelle (CLI).** Informationen zum Installieren der Azure CLI finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli) im Abschnitt zu Ihrem Betriebssystem. Wenn Sie lieber das Azure PowerShell-Modul in PowerShell 6+ und höher verwenden möchten, können Sie dies tun. Allerdings beziehen sich die Anleitungen in diesem Artikel auf die Azure CLI.

* **Prüfen Sie, ob Port 445 geöffnet ist**: SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.  Ersetzen Sie `<your-resource-group>` und `<your-storage-account>`, und führen Sie dann das folgende Skript aus:

    ```bash
    resourceGroupName="<your-resource-group>"
    storageAccountName="<your-storage-account>"
    
    # This command assumes you have logged in with az login
    httpEndpoint=$(az storage account show \
        --resource-group $resourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | tr -d '"')
    smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))
    fileHost=$(echo $smbPath | tr -d "/")

    nc -zvw3 $fileHost 445
    ```

    Wenn erfolgreich eine Verbindung hergestellt werden konnte, wird in etwa die folgende Ausgabe angezeigt:

    ```ouput
    Connection to <your-storage-account> 445 port [tcp/microsoft-ds] succeeded!
    ```

    Wenn Sie Port 445 nicht in Ihrem Unternehmensnetzwerk öffnen können oder der Internetdienstanbieter dies nicht zulässt, können Sie eine VPN-Verbindung oder ExpressRoute verwenden, um Port 445 zu umgehen. Weitere Informationen finden Sie unter [Überlegungen zum Netzwerk für den direkten Zugriff auf Azure-Dateifreigaben](storage-files-networking-overview.md).

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Bedarfsgesteuertes Einbinden der Azure-Dateifreigabe mit „Einbinden“
Wenn Sie eine Dateifreigabe unter einem Linux-Betriebssystem einbinden, wird Ihre Remotedateifreigabe in Ihrem lokalen Dateisystem als Ordner dargestellt. Sie können Dateifreigaben an einem beliebigen Ort in Ihrem System einbinden. Im folgenden Beispiel wird die Dateifreigabe unter dem Pfad `/mount` eingebunden. Dies können Sie in Ihren bevorzugten Pfad ändern, indem Sie die Variable `$mntRoot` ändern.

Ersetzen Sie `<resource-group-name>`, `<storage-account-name>` und `<file-share-name>` durch die entsprechenden Informationen für Ihre Umgebung:

```bash
resourceGroupName="<resource-group-name>"
storageAccountName="<storage-account-name>"
fileShareName="<file-share-name>"

mntRoot="/mount"
mntPath="$mntRoot/$storageAccountName/$fileShareName"

sudo mkdir -p $mntPath
```

Binden Sie die Dateifreigabe als Nächstes mit dem Befehl `mount` ein. Im folgenden Beispiel wird der Befehl `$smbPath` mit dem vollqualifizierten Domänennamen für den Dateiendpunkt des Speicherkontos aufgefüllt und `$storageAccountKey` mit dem Speicherkontoschlüssel. 

# <a name="smb-311"></a>[SMB 3.1.1](#tab/smb311)
> [!Note]  
> Ab der Linux-Kernelversion 5.0 ist SMB 3.1.1 das ausgehandelte Standardprotokoll. Wenn Sie eine Version des Linux-Kernels verwenden, die älter als 5.0 ist, geben Sie in der Liste der Bereitstellungsoptionen `vers=3.1.1` an.  

```bash
# This command assumes you have logged in with az login
httpEndpoint=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "primaryEndpoints.file" | tr -d '"')
smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

sudo mount -t cifs $smbPath $mntPath -o username=$storageAccountName,password=$storageAccountKey,serverino
```

# <a name="smb-30"></a>[SMB 3.0](#tab/smb30)
```bash
# This command assumes you have logged in with az login
httpEndpoint=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "primaryEndpoints.file" | tr -d '"')
smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,serverino
```

# <a name="smb-21"></a>[SMB 2.1](#tab/smb21)
```bash
# This command assumes you have logged in with az login
httpEndpoint=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "primaryEndpoints.file" | tr -d '"')
smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

sudo mount -t cifs $smbPath $mntPath -o vers=2.1,username=$storageAccountName,password=$storageAccountKey,serverino
```

---

Zum Festlegen von Berechtigungen können Sie in den Einbindungsoptionen für den Befehl `mount` `uid`/`gid` oder `dir_mode` und `file_mode` verwenden. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [UNIX numeric notation](https://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation) (Numerische UNIX-Notation) auf Wikipedia.

Falls gewünscht, können Sie dieselbe Azure-Dateifreigabe auch in mehrere Bereitstellungspunkte einbinden. Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, heben Sie deren Einbindung mit `sudo umount $mntPath` auf.

## <a name="automatically-mount-file-shares"></a>Automatisches Einbinden von Dateifreigaben
Wenn Sie eine Dateifreigabe unter einem Linux-Betriebssystem einbinden, wird Ihre Remotedateifreigabe in Ihrem lokalen Dateisystem als Ordner dargestellt. Sie können Dateifreigaben an einem beliebigen Ort in Ihrem System einbinden. Im folgenden Beispiel wird die Dateifreigabe unter dem Pfad `/mount` eingebunden. Dies können Sie in Ihren bevorzugten Pfad ändern, indem Sie die Variable `$mntRoot` ändern.

```bash
mntRoot="/mount"
sudo mkdir -p $mntRoot
```

Verwenden Sie zum Einbinden einer Azure-Dateifreigabe unter Linux den Speicherkontonamen als Benutzernamen der Dateifreigabe und den Speicherkontoschlüssel als Kennwort. Da sich die Anmeldeinformationen für das Speicherkonto im Laufe der Zeit ändern können, sollten Sie sie für das Speicherkonto getrennt von der Bereitstellungskonfiguration speichern. 

Im folgenden Beispiel wird gezeigt, wie Sie eine Datei zum Speichern der Anmeldeinformationen erstellen können. Ersetzen Sie `<resource-group-name>` und `<storage-account-name>` durch die entsprechenden Angaben für Ihre Umgebung.

```bash
resourceGroupName="<resource-group-name>"
storageAccountName="<storage-account-name>"

# Create a folder to store the credentials for this storage account and
# any other that you might set up.
credentialRoot="/etc/smbcredentials"
sudo mkdir -p "/etc/smbcredentials"

# Get the storage account key for the indicated storage account.
# You must be logged in with az login and your user identity must have 
# permissions to list the storage account keys for this command to work.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroupName \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# Create the credential file for this individual storage account
smbCredentialFile="$credentialRoot/$storageAccountName.cred"
if [ ! -f $smbCredentialFile ]; then
    echo "username=$storageAccountName" | sudo tee $smbCredentialFile > /dev/null
    echo "password=$storageAccountKey" | sudo tee -a $smbCredentialFile > /dev/null
else 
    echo "The credential file $smbCredentialFile already exists, and was not modified."
fi

# Change permissions on the credential file so only root can read or modify the password file.
sudo chmod 600 $smbCredentialFile
```

Zum automatischen Einbinden einer Dateifreigabe haben Sie die Wahl zwischen einem statischen Einbinden über das Hilfsprogramm `/etc/fstab` oder einem dynamischen Einbinden über das Hilfsprogramm `autofs`. 

### <a name="static-mount-with-etcfstab"></a>Statisches Einbinden mit „/etc/fstab“
Erstellen Sie in der früheren Umgebung unter Ihrem Bereitstellungsordner einen Ordner für Ihr Speicherkonto bzw. Ihre Dateifreigabe. Ersetzen Sie `<file-share-name>` durch den entsprechenden Namen Ihrer Azure-Dateifreigabe.

```bash
fileShareName="<file-share-name>"

mntPath="$mntRoot/$storageAccountName/$fileShareName"
sudo mkdir -p $mntPath
```

Erstellen Sie abschließend einen Datensatz in der Datei `/etc/fstab` für Ihre Azure-Dateifreigabe. Im nachstehenden Befehl werden Linux-Datei- und -Ordnerberechtigungen mit dem Standardwert „0755“ verwendet. Dadurch erhalten Besitzer Lese-, Schreib- und Ausführungsberechtigungen (basierend auf dem Linux-Besitzer der Datei bzw. des Verzeichnisses). Benutzer in Besitzergruppen erhalten Lese- und Ausführungsberechtigungen und alle anderen Benutzer des Systems Lese- und Ausführungsberechtigungen. Möglicherweise möchten Sie für die Einbindung alternative `uid`- und `gid`- oder `dir_mode`- und `file_mode`-Berechtigungen nach Wunsch festlegen. Weitere Informationen zum Festlegen von Berechtigungen finden Sie unter [UNIX numeric notation](https://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation) (Numerische UNIX-Notation) auf Wikipedia.

```bash
httpEndpoint=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "primaryEndpoints.file" | tr -d '"')
smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

if [ -z "$(grep $smbPath\ $mntPath /etc/fstab)" ]; then
    echo "$smbPath $mntPath cifs nofail,credentials=$smbCredentialFile,serverino" | sudo tee -a /etc/fstab > /dev/null
else
    echo "/etc/fstab was not modified to avoid conflicting entries as this Azure file share was already present. You may want to double check /etc/fstab to ensure the configuration is as desired."
fi

sudo mount -a
```

> [!Note]  
> Ab der Linux-Kernelversion 5.0 ist SMB 3.1.1 das ausgehandelte Standardprotokoll. Sie können alternative Protokollversionen mithilfe der Bereitstellungsoption `vers` angeben (Protokollversionen sind `3.1.1`, `3.0` und `2.1`).

### <a name="dynamically-mount-with-autofs"></a>Dynamisches Einbinden mit „autofs“
Wenn Sie eine Dateifreigabe mit dem Hilfsprogramm `autofs` dynamisch einbinden möchten, installieren Sie sie mithilfe des Paket-Managers in der Linux-Distribution Ihrer Wahl.  

Verwenden Sie bei **Ubuntu**- und **Debian**-Distributionen den Paket-Manager `apt`:

```bash
sudo apt update
sudo apt install autofs
```

Verwenden Sie unter **Red Hat Enterprise Linux 8+** den Paket-Manager `dnf`:
```bash
sudo dnf install autofs
```

Verwenden Sie unter älteren Versionen von **Red Hat Enterprise Linux** den Paket-Manager `yum`:

```bash
sudo yum install autofs 
```

Verwenden Sie unter **SUSE Linux Enterprise Server** den Paket-Manager `zypper`:
```bash
sudo zypper install autofs
```

Aktualisieren Sie als Nächstes die Konfigurationsdateien `autofs`. 

```bash
fileShareName="<file-share-name>"

httpEndpoint=$(az storage account show \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --query "primaryEndpoints.file" | tr -d '"')
smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

echo "$fileShareName -fstype=cifs,credentials=$smbCredentialFile :$smbPath" > /etc/auto.fileshares

echo "/fileshares /etc/auto.fileshares --timeout=60" > /etc/auto.master
```

Der letzte Schritt ist ein Neustart des Diensts `autofs`.

```bash
sudo systemctl restart autofs
```

## <a name="securing-linux"></a>Sichern von Linux
Zum Einbinden einer Azure-Dateifreigabe mit SMB muss Port 445 zugänglich sein. Viele Organisationen blockieren Port 445 aufgrund von mit SMB 1 verbundenen Sicherheitsrisiken. SMB 1, auch bekannt als CIFS (Common Internet File System) ist ein Legacydateisystemprotokoll, das in vielen Linux-Distributionen enthalten ist. SMB 1 ist ein veraltetes, ineffizientes und vor allem unsicheres Protokoll. Azure Files unterstützt SMB 1 nicht und ab der Linux-Kernelversion 4.18 ist es möglich, SMB 1 unter Linux zu deaktivieren. Es wird [dringend empfohlen](https://aka.ms/stopusingsmb1), SMB 1 auf Ihren Linux-Clients zu deaktivieren, bevor Sie SMB-Dateifreigaben in der Produktion verwenden.

Ab der Linux-Kernelversion 4.18 stellt das SMB-Kernelmodul (`cifs` genannt, weil es sich um eine Legacyversion handelt) einen neuen Modulparameter (häufig in verschiedenen Dokumentationen als *parm*, hier aber als `disable_legacy_dialects` bezeichnet) zur Verfügung. Obwohl dies mit der Linux-Kernelversion 4.18 eingeführt wurde, haben einige Anbieter diese Änderung auf ältere von ihnen unterstützte Kernelversionen zurückportiert. In der folgenden Tabelle wird die Verfügbarkeit dieses Modulparameters für allgemeine Linux-Distributionen ausführlich erläutert.

| Distribution | SMB 1 deaktivierbar |
|--------------|-------------------|
| Ubuntu 14.04–16.04 | Nein |
| Ubuntu 18.04 | Ja |
| Ubuntu 19.04 und höher | Ja |
| Debian 8–9 | Nein |
| Debian 10 und höher | Ja |
| Fedora 29 und höher | Ja |
| CentOS 7 | Nein | 
| CentOS 8 und höher | Ja |
| Red Hat Enterprise Linux 6.x–7.x | Nein |
| Red Hat Enterprise Linux 8 und höher | Ja |
| openSUSE Leap 15.0 | Nein |
| openSUSE Leap 15.1 und höher | Ja |
| openSUSE Tumbleweed | Ja |
| SUSE Linux Enterprise 11.x–12.x | Nein |
| SUSE Linux Enterprise 15 | Nein |
| SUSE Linux Enterprise 15.1 | Nein |

Mithilfe des folgenden Befehls können Sie überprüfen, ob Ihre Linux-Distribution den Modulparameter `disable_legacy_dialects` unterstützt.

```bash
sudo modinfo -p cifs | grep disable_legacy_dialects
```

Es sollte die folgende Meldung ausgegeben werden:

```output
disable_legacy_dialects: To improve security it may be helpful to restrict the ability to override the default dialects (SMB2.1, SMB3 and SMB3.02) on mount with old dialects (CIFS/SMB1 and SMB2) since vers=1.0 (CIFS/SMB1) and vers=2.0 are weaker and less secure. Default: n/N/0 (bool)
```

Bevor Sie SMB 1 deaktivieren, müssen Sie sich vergewissern, dass das SMB-Modul zum aktuellen Zeitpunkt nicht auf Ihrem System geladen ist. Wenn Sie eine SMB-Freigabe eingebunden haben, wird dies automatisch überprüft. Ansonsten können Sie den folgenden Befehl verwenden, der keine Ausgabe zurückgeben sollte, wenn SMB nicht geladen ist:

```bash
lsmod | grep cifs
```

Wenn Sie das Modul entladen möchten, entfernen Sie zuerst alle SMB-Freigaben (wie oben beschrieben über den Befehl `umount`). Mithilfe des folgenden Befehls können Sie alle eingebunden SMB-Freigaben auf Ihrem System identifizieren:

```bash
mount | grep cifs
```

Nachdem Sie alle SMB-Dateifreigaben entfernt haben, ist es sicher, das Modul zu entladen. Hierfür können Sie den Befehl `modprobe` verwenden:

```bash
sudo modprobe -r cifs
```

Sie können das Modul mithilfe des Befehls `modprobe` manuell laden, wenn SMB 1 entladen ist:

```bash
sudo modprobe cifs disable_legacy_dialects=Y
```

Zum Schluss können Sie überprüfen, ob das SMB-Modul mit dem Parameter geladen wurde, indem Sie sich die geladenen Parameter in `/sys/module/cifs/parameters` ansehen:

```bash
cat /sys/module/cifs/parameters/disable_legacy_dialects
```

Wenn Sie SMB 1 für Ubuntu- und Debian-basierte Distributionen dauerhaft aktivieren möchten, müssen Sie eine neue Datei mit dem Namen `/etc/modprobe.d/local.conf` und der entsprechenden Einstellung erstellen (wenn Sie noch nicht über benutzerdefinierte Optionen für andere Module verfügen). Dafür können Sie den folgenden Befehl verwenden:

```bash
echo "options cifs disable_legacy_dialects=Y" | sudo tee -a /etc/modprobe.d/local.conf > /dev/null
```

Wenn Sie das SMB-Modul laden, können Sie überprüfen, ob der Vorgang erfolgreich war:

```bash
sudo modprobe cifs
cat /sys/module/cifs/parameters/disable_legacy_dialects
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Files finden Sie unter diesen Links:

* [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
* [Häufig gestellte Fragen](./storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-linux-file-connection-problems.md)
