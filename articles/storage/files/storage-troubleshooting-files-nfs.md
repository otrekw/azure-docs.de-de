---
title: Behandeln von Problemen mit Azure NFS-Dateifreigaben – Azure Files
description: Behandeln von Problemen mit Azure NFS-Dateifreigaben
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: jeffpatt
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 661cfd5bb410a714bc42e0cd9676ac2ec08f8a45
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90708410"
---
# <a name="troubleshoot-azure-nfs-file-shares"></a>Behandeln von Problemen mit Azure NFS-Dateifreigaben

In diesem Artikel sind einige allgemeine Probleme aufgeführt, die im Zusammenhang mit Azure NFS-Dateifreigaben auftreten können. Er enthält mögliche Ursachen und Problemumgehungen, wenn diese Probleme auftreten.

## <a name="unable-to-create-an-nfs-share"></a>Es kann keine NFS-Freigabe erstellt werden.

### <a name="cause-1-subscription-is-not-enabled"></a>Ursache 1: Das Abonnement ist nicht aktiviert.

Möglicherweise wurde Ihr Abonnement nicht für die Azure Files-NFS-Vorschau registriert. Sie müssen einige zusätzliche Cmdlets von der Cloud Shell oder einem lokalen Terminal ausführen, um die Funktion zu aktivieren.

> [!NOTE]
> Es kann bis zu 30 Minuten dauern, bis die Registrierung abgeschlossen ist.


#### <a name="solution"></a>Lösung

Verwenden Sie das folgende Skript, um die Funktion und den Ressourcenanbieter zu registrieren, und ersetzen Sie `<yourSubscriptionIDHere>` vor dem Ausführen des Skripts:

```azurepowershell
Connect-AzAccount

#If your identity is associated with more than one subscription, set an active subscription
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context

Register-AzProviderFeature -FeatureName AllowNfsFileShares - ProviderNamespace Microsoft.Storage

Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="cause-2-unsupported-storage-account-settings"></a>Ursache 2: Nicht unterstützte Speicherkontoeinstellungen

NFS ist nur für Speicherkonten mit der folgenden Konfiguration verfügbar:

- Ebene – Premium
- Kontoart – FileStorage
- Redundanz – LRS
- Regionen – USA, Osten; USA, Osten 2; Vereinigtes Königreich, Süden; Asien, Südosten

#### <a name="solution"></a>Lösung

Befolgen Sie die Anweisungen in unserem Artikel: [Erstellen einer NFS-Freigabe](storage-files-how-to-create-nfs-shares.md)

### <a name="cause-3-the-storage-account-was-created-prior-to-registration-completing"></a>Ursache 3: Das Speicherkonto wurde vor Abschluss der Registrierung erstellt.

Damit die Funktion von einem Speicherkonto verwendet werden kann, muss sie erstellt werden, sobald die NFS-Registrierung für das Abonnement abgeschlossen ist. Es kann bis zu 30 Minuten dauern, bis die Registrierung abgeschlossen ist.

#### <a name="solution"></a>Lösung

Nachdem die Registrierung abgeschlossen ist, befolgen Sie die Anweisungen in unserem Artikel: [Erstellen einer NFS-Freigabe](storage-files-how-to-create-nfs-shares.md)

## <a name="cannot-connect-to-or-mount-an-azure-nfs-file-share"></a>Es ist nicht möglich, eine Verbindung mit einer Azure NFS-Dateifreigabe herzustellen bzw. diese einzubinden.

### <a name="cause-1-request-originates-from-a-client-in-an-untrusted-networkuntrusted-ip"></a>Ursache 1: Eine Anforderung stammt von einem Client in einem nicht vertrauenswürdigen Netzwerk/mit einer nicht vertrauenswürdigen IP-Adresse.

NFS unterstützt im Gegensatz zu SMB keine benutzerbasierte Authentifizierung. Die Authentifizierung für eine Freigabe basiert auf der Konfiguration Ihrer Netzwerksicherheitsregeln. Aus diesem Grund müssen Sie entweder den Dienstendpunkt oder private Endpunkte verwenden, um sicherzustellen, dass nur sichere Verbindungen mit der NFS-Freigabe hergestellt werden. Wenn Sie lokal auf Freigaben zugreifen möchten, müssen Sie zusätzlich zu privaten Endpunkten ein VPN oder ExpressRoute einrichten. IP-Adressen, die der Firewall-Zulassungsliste des Speicherkontos hinzugefügt wurden, werden ignoriert. Sie müssen eine der folgenden Methoden verwenden, um den Zugriff auf eine NFS-Freigabe einzurichten:


- [Dienstendpunkt](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - Zugriff über den öffentlichen Endpunkt
    - Nur in derselben Region verfügbar
    - VNET-Peering unterstützt keinen Zugriff auf die Freigabe.
    - Jedes virtuelle Netzwerk oder Subnetz muss der Zulassungsliste einzeln hinzugefügt werden.
    - Für den lokalen Zugriff können Dienstendpunkte mit ExpressRoute, Point-to-Site- und Site-to-Site-VPNs verwendet werden, es wird jedoch empfohlen, einen privaten Endpunkt zu verwenden, da dieser sicherer ist.

Im folgenden Diagramm wird die Konnektivität bei Verwendung öffentlicher Endpunkte dargestellt.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg" alt-text="Diagramm der Verbindungen über öffentliche Endpunkte" lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg":::

- [Privater Endpunkt](storage-files-networking-endpoints.md#create-a-private-endpoint)
    - Der Zugriff ist sicherer als der über den Dienstendpunkt.
    - Der Zugriff auf die NFS-Freigabe über Private Link ist innerhalb und außerhalb der Azure-Region des Speicherkontos verfügbar (regionsübergreifend, lokal).
    - Das Peering virtueller Netzwerke mit virtuellen Netzwerken, die im privaten Endpunkt gehostet werden, gewährt den Clients in virtuellen Netzwerken mit Peering Zugriff auf NFS-Freigaben.
    - Private Endpunkte können mit ExpressRoute, Point-to-Site- und Site-to-Site-VPNs verwendet werden.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg" alt-text="Diagramm der Verbindungen über öffentliche Endpunkte" lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg":::

### <a name="cause-2-secure-transfer-required-is-enabled"></a>Ursache 2: „Sichere Übertragung erforderlich“ ist aktiviert.

Die doppelte Verschlüsselung wird für NFS-Freigaben noch nicht unterstützt. Azure bietet mit MACSec eine Verschlüsselungsschicht für alle Daten, die zwischen Azure-Rechenzentren übertragen werden. Auf NFS-Freigaben kann nur von vertrauenswürdigen virtuellen Netzwerken und über VPN-Tunnel zugegriffen werden. Auf NFS-Freigaben ist keine zusätzliche Verschlüsselung der Transportschicht verfügbar.

#### <a name="solution"></a>Lösung

Deaktivieren Sie auf dem Blatt „Konfiguration“ des Speicherkontos die Option „Sichere Übertragung erforderlich“.

:::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Diagramm der Verbindungen über öffentliche Endpunkte":::

### <a name="cause-3-nfs-common-package-is-not-installed"></a>Ursache 3: Das Paket „nfs-common“ ist nicht installiert.
Installieren Sie das Paket, bevor Sie den mount-Befehl ausführen, indem Sie den unten angegebenen distributionsspezifischen Befehl ausführen.

Führen Sie `rpm qa | grep nfs-utils` aus, um zu überprüfen, ob das NFS-Paket installiert ist.

#### <a name="solution"></a>Lösung

Wenn das Paket nicht installiert ist, installieren Sie das Paket in Ihrer Distribution.

##### <a name="ubuntu-or-debian"></a>Ubuntu oder Debian

```
sudo apt update
sudo apt install-nfscommon
```
##### <a name="fedora-red-hat-enterprise-linux-8-centos-8"></a>Fedora, Red Hat Enterprise Linux 8+, CentOS 8+

Verwenden Sie den DNF-Paket-Manager `sudo dnf install nfs-common`.

Verwenden Sie unter älteren Versionen von Red Hat Enterprise Linux und CentOS den YUM-Paket-Manager `sudo yum install nfs-common`.

##### <a name="opensuse"></a>openSUSE

Verwenden Sie den Zypper-Paket-Manager `sudo zypper install-nfscommon`.

### <a name="cause-4-firewall-blocking-port-2049"></a>Ursache 4: Die Firewall blockiert Port 2049.

Das NFS-Protokoll kommuniziert über Port 2049 mit dem Server. Stellen Sie sicher, dass dieser Port für das Speicherkonto (den NFS-Server) geöffnet ist.

#### <a name="solution"></a>Lösung

Überprüfen Sie, ob Port 2049 auf dem Client geöffnet ist, indem Sie den folgenden Befehl ausführen: `telnet <storageaccountnamehere>.file.core.windows.net 2049`. Wenn der Port nicht geöffnet ist, öffnen Sie ihn.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.