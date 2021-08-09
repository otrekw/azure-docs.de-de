---
title: Details zu Azure Firewall-DNS-Proxys
description: Informationen zur Funktionsweise von Azure Firewall DNS-Proxys
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 05/26/2021
ms.author: victorh
ms.openlocfilehash: f6f17823b51200c21ee0975f50487ba1e3f58ced
ms.sourcegitcommit: 9ad20581c9fe2c35339acc34d74d0d9cb38eb9aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110547675"
---
# <a name="azure-firewall-dns-proxy-details"></a>Details zu Azure Firewall-DNS-Proxys

Sie haben auch die Möglichkeit, Azure Firewall als DNS-Proxy zu konfigurieren. Ein DNS-Proxy ist ein Vermittler für DNS-Anforderungen von virtuellen Clientcomputern an einen DNS-Server.

Im Folgenden werden einige Implementierungsdetails für Azure Firewall DNS-Proxy beschrieben.

## <a name="fqdns-with-multiple-a-records"></a>FQDNs mit mehreren A-Records

Azure Firewall fungiert als DNS-Standardclient. Wenn mehrere A-Records in der Antwort enthalten sind, speichert die Firewall alle Records im Cache. Wenn es einen Record pro Antwort gibt, speichert die Firewall nur einen einzelnen Record. Ein Client kann nie im Voraus wissen, ob er einen oder mehrere A-Records in Antworten erwarten sollte.

## <a name="fqdn-time-to-live-ttl"></a>FQDN-Lebensdauer (TTL, Time to Live)

Kurz bevor eine FQDN-TTL abläuft, werden Records gemäß ihren TTLs zwischengespeichert und verworfen. Es wird kein Prefetching verwendet, sodass die Firewall vor Ablauf der TTL keinen Lookup ausführt, um den Record zu aktualisieren.

## <a name="clients-not-configured-to-use-the-firewall-dns-proxy"></a>Nicht für die Verwendung des Firewall-DNS-Proxys konfigurierte Clients

Wenn ein Clientcomputer für die Verwendung eines anderen DNS-Servers als dem DNS-Proxy der Firewall konfiguriert ist, können die Ergebnisse unvorhersehbar sein.

Ein Beispiel: Angenommen, eine Clientworkload befindet sich in „USA, Osten“ und verwendet einen primären DNS-Server, der in „USA, Osten“ gehostet wird. Die Azure Firewall DNS-Servereinstellungen sind für einen sekundären DNS-Server konfiguriert, der in „USA, Westen“ gehostet wird. Der DNS-Server der Firewall, der in „USA, Westen“ gehostet wird, sendet eine andere Antwort als der Client in der Region „USA, Osten“.

Dieses häufig auftretende Szenario ist der Grund dafür, dass Clients die DNS-Proxyfunktionalität der Firewall verwenden sollten. Wenn Sie FQDNs in Netzwerkregeln verwenden, sollten Clients die Firewall als Auflöser verwenden. Durch Clients und die Firewall selbst können sie eine konsistente IP-Adressauflösung sicherstellen.

Wenn in diesem Beispiel ein FQDN in den Netzwerkregeln konfiguriert ist, löst die Firewall den FQDN als IP1 (IP-Adresse 1) auf und aktualisiert die Netzwerkregeln, um den Zugriff auf IP1 zu ermöglichen. Wenn der Client den gleichen FQDN aufgrund eines Unterschieds in der DNS-Antwort als IP2 auflöst, entspricht der Verbindungsversuch nicht den Regeln der Firewall und wird verweigert. 

Für HTTP/S-FQDNs in Anwendungsregeln liest die Firewall den FQDN aus dem Host- oder SNI-Header, löst ihn auf und stellt dann eine Verbindung mit dieser IP-Adresse her. Die Ziel-IP-Adresse, mit der der Client eine Verbindung herstellen wollte, wird ignoriert.

## <a name="next-steps"></a>Nächste Schritte

- [DNS-Einstellungen für Azure Firewall](dns-settings.md)