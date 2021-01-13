---
title: Azure VMware Solution by CloudSimple – Firewalltabellen
description: Hier erfahren Sie mehr über Firewalltabellen und Firewallregeln einer privaten CloudSimple-Cloud, u a. über Standardregeln, die für jede Firewalltabelle erstellt werden.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8c44c39f66a0a0161eea8a7e9656bbe0e3d1015c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88140869"
---
# <a name="firewall-tables-overview"></a>Übersicht über Firewalltabellen

In einer Firewalltabelle sind die Regeln aufgeführt, mit denen Netzwerkdatenverkehr an und von Ressourcen einer privaten Cloud gefiltert werden. Sie können Firewalltabellen auf ein VLAN oder ein Subnetz anwenden. Die Regeln steuern den Netzwerkdatenverkehr zwischen einem Quellnetzwerk oder einer Quell-IP-Adresse und einem Zielnetzwerk oder einer Ziel-IP-Adresse.

## <a name="firewall-rules"></a>Firewallregeln

In der folgenden Tabelle sind die Parameter beschrieben, die es in einer Firewallregel gibt.

| Eigenschaft | Details |
| ---------| --------|
| **Name** | Ein Name, der die Firewallregel und deren Zweck eindeutig kennzeichnet. |
| **Priority** | Eine Zahl zwischen 100 und 4096, wobei 100 die höchste Priorität bedeutet. Regeln werden in der Reihenfolge ihrer Priorität verarbeitet. Wenn beim Datenverkehr eine Regelübereinstimmung gefunden wird, wird die Regelverarbeitung beendet. Daher werden Regeln mit niedrigeren Prioritäten, die dieselben Attribute aufweisen wie Regeln mit höheren Prioritäten, nicht verarbeitet.  Achten Sie darauf, sich blockierende Regeln zu vermeiden. |
| **Zustandsnachverfolgung** | Nachverfolgung kann zustandslos (private Cloud, Internet oder VPN) oder zustandsbehaftet (öffentliche IP-Adresse) sein.  |
| **Protokoll** | Zu den Optionen gehören „Any“ (Beliebig), „TCP“ und „UDP“. Wenn Sie ICMP benötigen, verwenden Sie „Any“. |
| **Richtung** | Gibt an, ob die Regel für ein- oder ausgehenden Datenverkehr gilt. |
| **Aktion** | Zulassen oder ablehnen für den Typ von Datenverkehr, der in der Regel definiert ist. |
| **Quelle** | Eine IP-Adresse, ein CIDR-Block (Classless Inter-Domain Routing, z. B. 10.0.0.0/24) oder „Any“.  Durch das Angeben eines Bereichs, eines Diensttags oder einer Anwendungssicherheitsgruppe haben Sie die Möglichkeit, weniger Sicherheitsregeln zu erstellen. |
| **Quellport** | Port, von dem der Netzwerkdatenverkehr stammt.  Sie können einen einzelnen Port oder einen Portbereich, etwa 443 oder 8000-8080, angeben. Das Angeben von Bereichen ermöglicht Ihnen die Erstellung von weniger Sicherheitsregeln. |
| **Ziel** | Eine IP-Adresse, ein CIDR-Block (Classless Inter-Domain Routing, z. B. 10.0.0.0/24) oder „Any“.  Durch das Angeben eines Bereichs, eines Diensttags oder einer Anwendungssicherheitsgruppe haben Sie die Möglichkeit, weniger Sicherheitsregeln zu erstellen.  |
| **Zielport** | Der Port, an den der Netzwerkdatenverkehr gesendet wird.  Sie können einen einzelnen Port oder einen Portbereich, etwa 443 oder 8000-8080, angeben. Das Angeben von Bereichen ermöglicht Ihnen die Erstellung von weniger Sicherheitsregeln.|

### <a name="stateless"></a>Zustandslos

Mit einer zustandslosen Regel werden nur einzelne Pakete analysiert, und diese werden entsprechend der Regel gefiltert.  
Möglicherweise sind weitere Regeln für Datenverkehr in umgekehrter Richtung erforderlich.  Verwenden Sie zustandslose Regeln für Datenverkehr zwischen den folgenden Punkten:

* Subnetze von privaten Clouds
* Ein lokales Subnetz und ein Subnetz in einer privaten Cloud
* Internetdatenverkehr aus den privaten Clouds

### <a name="stateful"></a>Zustandsbehaftet

 Mit einer zustandsbehafteten Regel werden die Verbindungen überwacht, die ihr zugeordnet sind. Für vorhandene Verbindungen wird ein Flussdatensatz erstellt. Die Kommunikation wird basierend auf dem Verbindungszustand der Flussdatensätze zugelassen oder verweigert.  Verwenden Sie Regeln dieses Typs für öffentliche IP-Adressen, um Datenverkehr aus dem Internet zu filtern.

### <a name="default-rules"></a>Standardregeln

Die folgenden Standardregeln werden in jeder Firewalltabelle erstellt.

|Priority|Name|Zustandsnachverfolgung|Direction|Datenverkehrstyp|Protocol|`Source`|Quellport|Destination|Zielport|Action|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|allow-all-to-internet|Zustandsbehaftet|Ausgehend|Öffentliche IP-Adresse oder Internetdatenverkehr|All|Any|Any|Any|Any|Allow|
|65001|deny-all-from-internet|Zustandsbehaftet|Eingehend|Öffentliche IP-Adresse oder Internetdatenverkehr|All|Any|Any|Any|Any|Verweigern|
|65002|allow-all-to-intranet|Zustandslos|Ausgehend|Datenverkehr in privater Cloud oder VPN-Datenverkehr|All|Any|Any|Any|Any|Allow|
|65003|allow-all-from-intranet|Zustandslos|Eingehend|Datenverkehr in privater Cloud oder VPN-Datenverkehr|All|Any|Any|Any|Any|Allow|

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Firewalltabellen und -regeln](firewall.md)
