---
title: Sicherheit in Azure Database for PostgreSQL – Einzelserver
description: Eine Übersicht über die Sicherheitsfeatures in Azure Database for PostgreSQL– Einzelserver.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 02dc9e1ad9ee46b1a400e44b6ef737e70571a17a
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75972586"
---
# <a name="security-in-azure-database-for-postgresql---single-server"></a>Sicherheit in Azure Database for PostgreSQL – Einzelserver

Zum Schutz der Daten auf Ihrem Azure Database for PostgreSQL-Server stehen mehrere Sicherheitsebenen zur Verfügung. In diesem Artikel werden diese Sicherheitsoptionen beschrieben.

## <a name="information-protection-and-encryption"></a>Informationsschutz und -verschlüsselung

### <a name="in-transit"></a>Während der Übertragung
Mit Azure Database for PostgreSQL werden Ihre Daten durch Verschlüsselung während der Übertragung mit Transport Layer Security geschützt. Die Verschlüsselung (SSL/TLS) wird standardmäßig erzwungen.

### <a name="at-rest"></a>Ruhende Daten
Der Azure Database for PostgreSQL-Dienst nutzt das FIPS 140-2-zertifizierte Kryptografiemodul für die Speicherverschlüsselung ruhender Daten. Daten, einschließlich Sicherungen, werden auf dem Datenträger verschlüsselt (mit Ausnahme von temporären Dateien, die während der Ausführung von Abfragen erstellt werden). Der Dienst verwendet das in der Azure Storage-Verschlüsselung enthaltene AES-256-Bit-Verschlüsselungsverfahren, und die Schlüssel werden vom System verwaltet. Die Speicherverschlüsselung ist immer aktiviert und kann nicht deaktiviert werden.


## <a name="network-security"></a>Netzwerksicherheit
Verbindungen mit einem Azure Database for PostgreSQL-Server werden zunächst über ein regionales Gateway geleitet. Das Gateway verfügt über eine öffentlich zugängliche IP-Adresse, während die IP-Adressen des Servers geschützt sind. Weitere Informationen zum Gateway finden Sie im Artikel [Verbindungsarchitektur](concepts-connectivity-architecture.md).  

Ein neu erstellter Azure Database for PostgreSQL-Server verfügt über eine Firewall, die alle externen Verbindungen blockiert. Sie erreichen zwar das Gateway, dürfen aber keine Verbindung mit dem Server herstellen. 

### <a name="ip-firewall-rules"></a>IP-Firewallregeln
IP-Firewallregeln gewähren Serverzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung. Weitere Informationen finden Sie in der [Übersicht über Firewallregeln](concepts-firewall-rules.md).

### <a name="virtual-network-firewall-rules"></a>Firewallregeln für virtuelle Netzwerke
Mit VNET-Dienstendpunkten wird die Konnektivität virtueller Netzwerke über den Azure-Backbone erweitert. Mithilfe von VNET-Regeln können Sie Ihren Azure Database for PostgreSQL-Server so konfigurieren, dass Verbindungen von ausgewählten Subnetzen in einem virtuellen Netzwerk zugelassen werden. Weitere Informationen finden Sie in der [Übersicht über VNET-Dienstendpunkte](concepts-data-access-and-security-vnet.md).

### <a name="private-ip"></a>Private IP-Adresse
Private Link ermöglicht das Herstellen von Verbindungen mit Ihrem Azure Database for PostgreSQL-Einzelserver in Azure über einen privaten Endpunkt. Im Prinzip macht Azure Private Link Azure-Dienste in Ihrem privaten virtuellen Netzwerk (VNET) verfügbar. Der Zugriff auf die PaaS-Ressourcen erfolgt wie bei anderen Ressourcen im VNET über die private IP-Adresse. Weitere Informationen finden Sie in der [Übersicht über Private Link](concepts-data-access-and-security-private-link.md).


## <a name="access-management"></a>Zugriffsverwaltung

Beim Erstellen des Azure Database for PostgreSQL-Servers geben Sie Anmeldeinformationen für eine Administratorrolle ein. Diese Administratorrolle kann verwendet werden, um zusätzliche [PostgreSQL-Rollen](https://www.postgresql.org/docs/current/user-manag.html) zu erstellen.

Mithilfe von [Azure Active Directory (AAD)-Authentifizierung](concepts-aad-authentication.md) können Sie auch eine Verbindung mit dem Server herstellen.


## <a name="threat-protection"></a>Bedrohungsschutz

Sie können [Advanced Threat Protection](concepts-data-access-and-security-threat-protection.md) verwenden. Diese Lösung erkennt anormale Aktivitäten, die darauf hindeuten, dass auf ungewöhnliche und potenziell schädliche Weise versucht wird, auf Server zuzugreifen oder diese auszunutzen.

Aktivitäten in Ihren Datenbanken können mithilfe der [Überwachungsprotokollierung](concepts-audit.md) nachverfolgt werden. 


## <a name="next-steps"></a>Nächste Schritte
- Aktivieren von Firewallregeln für [IP-Adressen](concepts-firewall-rules.md) oder [virtuelle Netzwerke](concepts-data-access-and-security-vnet.md)
- Informieren Sie sich über [Azure Active Directory-Authentifizierung](concepts-aad-authentication.md) in Azure Database for PostgreSQL.
