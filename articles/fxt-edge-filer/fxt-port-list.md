---
title: Portliste für Azure FXT Edge Filer
description: Liste der von FXT-Clusterumgebungen verwendeten TCP/UDP-Ports
author: ekpgh
ms.author: v-erkel
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 05/20/2021
ms.openlocfilehash: 5067e60a45025ae6c37af63df4e1392e9ae52709
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2021
ms.locfileid: "111358488"
---
# <a name="required-network-ports"></a>Erforderliche Netzwerkports

Diese Liste zeigt TCP/UDP-Ports, die für FXT-Clusterumgebungen erforderlich sind. Konfigurieren Sie Firewalls, die Sie verwenden, um sicherzustellen, dass auf diese Ports zugegriffen werden kann.

Die genauen Anforderungen Ihres Systemen variieren und hängen davon ab, welche Art Back-End-Speicher Sie verwenden.

Wenn Sie weitere Informationen erhalten möchten, wenden Sie sich an den Kundenservice und -support von Microsoft.

## <a name="api-ports"></a>API-Ports

| Direction | Typ | Portnummer | Protocol |
|-----------|------|-------------|----------|
| Eingehend   | TCP  | 22          | SSH      |
| Ausgehend  | TCP  | 80          | HTTP     |
| Eingehend und Ausgehend  | TCP  | 443         | HTTPS    |

## <a name="nfs-ports"></a>NFS-Ports

Eingehende NFS-Ports:

| Typ    | Portnummer | Dienst  |
|---------|-------------|----------|
| TCP/UDP | 111         | RPCBIND  |
| TCP/UDP | 2049        | NFS      |
| TCP/UDP | 4045        | NLOCKMGR |
| TCP/UDP | 4046        | MOUNTD   |
| TCP/UDP | 4047        | STATUS   |

Ausgehende NFS-Ports:

| Typ    | Portnummer | Dienst  |
|---------|-------------|----------|
| TCP/UDP | 111         | RPCBIND  |
| TCP/UDP | 2049        | NFS      |

Der Datenverkehr über ausgehende NFS-Ports variiert und hängt von der Art des Speichers ab, der als Kernspeichereinheit verwendet wird. (Einige Systeme verwenden Port 2049 nicht, er wird aber häufig genug verwendet, um hier aufgeführt werden zu können. Alle Cluster müssen auf Port 111 zugreifen können.) Sehen Sie sich die Dokumentation für Speichersysteme an, oder verwenden Sie die unten in [Zusätzliche Portanforderungen](#additional-port-requirements) beschriebene Abfragemethodik.

Für bestimmten ausgehenden NFS-Datenverkehr von FXT-Knoten werden kurzlebige Ports verwendet. Ausgehender FXT-Datenverkehr über bekannte Ports sollte auf Transportebene nicht eingeschränkt werden.

## <a name="smb-ports"></a>SMB-Ports

| Direction | Typ | Portnummer | Protocol |
|-----------|------|-------------|----------|
| Eingehend und Ausgehend  | UDP  | 137         | NetBIOS  |
| Eingehend   | UDP  | 138         | NetBIOS  |
| Eingehend und Ausgehend  | TCP  | 139         | SMB      |
| Eingehend und Ausgehend  | TCP  | 445         | SMB      |

<!--| Outbound  | UDP  | 137         | NetBIOS  | 
| Outbound  | TCP  | 139         | SMB      |
| Outbound  | TCP  | 445         | SMB      |
-->

## <a name="general-traffic-ports"></a>Allgemeine Datenverkehrsports

| Direction | Typ    | Portnummer | Protocol |
|-----------|---------|-------------|----------|
| Ausgehend  | TCP/UDP | 53          | DNS      |
| Ausgehend  | TCP/UDP | 88          | Kerberos |
| Ausgehend  | UDP     | 123         | NTP      |
| Ausgehend  | TCP/UDP | 389         | LDAP     |
| Ausgehend  | TCP     | 686         | LDAPS    |

## <a name="additional-port-requirements"></a>Zusätzliche Portanforderungen

Ihre Kernspeichereinheiten benötigen möglicherweise Zugriff auf weitere Ports. Diese Anforderung variiert und hängt davon ab, welche Art Speicher verwendet wird.

Sie können den Befehl `rpcinfo` verwenden, um zu ermitteln, welche Ports von einem bestimmten Server verwendet werden. Führen Sie diesen Befehl in einem Clientsystem aus, das über keine Firewall verfügt:

`rpcinfo -p <server_IP_address>`
