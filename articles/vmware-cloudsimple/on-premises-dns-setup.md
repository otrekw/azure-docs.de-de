---
title: 'Azure VMware Solutions (AVS): Konfigurieren von DNS für eine private AVS-Cloud'
description: Beschreibt das Einrichten der DNS-Namensauflösung für den Zugriff auf vCenter-Server in einer privaten AVS-Cloud über lokale Arbeitsstationen
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 12c4362ae1b075af132d5971f4fe0461c9d91733
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77083083"
---
# <a name="configure-dns-for-name-resolution-for-avs-private-cloud-vcenter-access-from-on-premises-workstations"></a>Konfigurieren von DNS für die Namensauflösung für den vCenter-Zugriff in der privaten AVS-Cloud über lokale Arbeitsstationen

Für den Zugriff auf den vCenter-Server in einer privaten AVS-Cloud über lokale Arbeitsstationen müssen Sie die DNS-Adressauflösung konfigurieren, damit der vCenter-Server sowohl über den Hostnamen als auch über die IP-Adresse angesprochen werden kann.

## <a name="obtain-the-ip-address-of-the-dns-server-for-your-avs-private-cloud"></a>Abrufen der IP-Adresse des DNS-Servers für die private AVS-Cloud

1. Melden Sie sich beim [AVS-Portal](access-cloudsimple-portal.md) an.

2. Navigieren Sie zu **Resources** > **AVS Private Clouds** (Ressourcen > Private AVS-Clouds), und wählen Sie die private AVS-Cloud aus, mit der Sie eine Verbindung herstellen möchten.

3. Kopieren Sie auf der Seite **Summary** (Zusammenfassung) der privaten AVS-Cloud unter **Basic Info** (Grundlegende Informationen) die IP-Adresse des DNS-Servers für die private AVS-Cloud.

    ![DNS-Server in der privaten AVS-Cloud](media/private-cloud-dns-server.png)


Verwenden Sie eine dieser Optionen für die DNS-Konfiguration.

* [Erstellen einer Zone auf dem DNS-Server für *.cloudsimple.io](#create-a-zone-on-a-microsoft-windows-dns-server)
* [Erstellen einer bedingten Weiterleitung auf dem lokalen DNS-Server zum Auflösen von *.cloudsimple.io](#create-a-conditional-forwarder)

## <a name="create-a-zone-on-the-dns-server-for-cloudsimpleio"></a>Erstellen einer Zone auf dem DNS-Server für *.cloudsimple.io

Sie können eine Zone als Stubzone einrichten und auf die DNS-Server in der privaten Cloud für Namensauflösung verweisen. Dieser Abschnitt enthält Informationen zur Verwendung eines BIND-DNS-Servers oder eines Microsoft Windows-DNS-Servers.

### <a name="create-a-zone-on-a-bind-dns-server"></a>Erstellen einer Zone auf einem BIND-DNS-Server

Welche Datei und welche Parameter konfiguriert werden müssen, hängt vom jeweiligen DNS-Setup ab.

Bearbeiten Sie z. B für die BIND-Standardserverkonfiguration die Datei „/etc/named.conf“ auf Ihrem DNS-Server, und fügen Sie die folgenden Zoneninformationen hinzu.

```
zone "az.cloudsimple.io"
{
    type stub;
    masters { IP address of DNS servers; };
    file "slaves/cloudsimple.io.db";
};
```

### <a name="create-a-zone-on-a-microsoft-windows-dns-server"></a>Erstellen einer Zone auf einem Microsoft Windows-DNS-Server

1. Klicken Sie mit der rechten Maustaste auf den DNS-Server, und wählen Sie dann **New Zone** (Neue Zone) aus. 
  
    ![Neue Zone](media/DNS01.png)
2. Wählen Sie **Stub Zone** (Stubzone) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Neue Zone](media/DNS02.png)
3. Wählen Sie abhängig von Ihrer Umgebung die entsprechende Option aus, und klicken Sie dann auf **Next** (Weiter).

    ![Neue Zone](media/DNS03.png)
4. Wählen Sie **Forward lookup zone** (Forward-Lookupzone) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Neue Zone](media/DNS01.png)
5. Geben Sie den Zonennamen ein, und klicken Sie auf **Next** (Weiter).

    ![Neue Zone](media/DNS05.png)
6. Geben Sie die IP-Adressen der DNS-Server für die private AVS-Cloud ein, die Sie aus dem AVS-Portal abgerufen haben.

    ![Neue Zone](media/DNS06.png)
7. Klicken Sie bei Bedarf auf **Next** (Weiter), um das Setup des Assistenten abzuschließen.

## <a name="create-a-conditional-forwarder"></a>Erstellen einer bedingten Weiterleitung

Bei einer bedingten Weiterleitung werden alle DNS-Namensauflösungsanforderungen an den vorgesehenen Server weitergeleitet. Mit diesem Setup werden alle Anforderungen an *.cloudsimple.io an die DNS-Server in der privaten AVS-Cloud weitergeleitet. In den folgenden Beispielen wird gezeigt, wie Weiterleitungen für verschiedene DNS-Servertypen eingerichtet werden.

### <a name="create-a-conditional-forwarder-on-a-bind-dns-server"></a>Erstellen einer bedingten Weiterleitung auf einem BIND-DNS-Server

Welche Datei und welche Parameter konfiguriert werden müssen, hängt vom jeweiligen DNS-Setup ab.

Bearbeiten Sie z. B für die BIND-Standardserverkonfiguration die Datei „/etc/named.conf“ auf Ihrem DNS-Server, und fügen Sie die folgenden Informationen zur bedingten Weiterleitung hinzu.

```
zone "az.cloudsimple.io" {
    type forward;
    forwarders { IP address of DNS servers; };
};
```

### <a name="create-a-conditional-forwarder-on-a-microsoft-windows-dns-server"></a>Erstellen einer bedingten Weiterleitung auf einem Microsoft Windows-DNS-Server

1. Öffnen Sie den DNS-Manager auf dem DNS-Server.
2. Klicken Sie mit der rechten Maustaste auf **Conditional Forwarders** (Bedingte Weiterleitungen), und wählen Sie dann die Option zum Hinzufügen einer neuen bedingten Weiterleitung aus.

    ![Bedingte Weiterleitung 1 Windows-DNS](media/DNS08.png)
3. Geben Sie die DNS-Domäne und die IP-Adresse der DNS-Server in der privaten AVS-Cloud ein, und klicken Sie auf **OK**.
