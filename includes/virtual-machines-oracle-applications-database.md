---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/01/2020
ms.author: danlep
ms.openlocfilehash: b9e275cbdaf264df3d0a40f032693fd93553e520
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84317510"
---
### <a name="database-tier"></a>Datenbankschicht

Die Datenbankschicht enthält die Datenbankinstanzen für die Anwendung. Bei der Datenbank kann es sich um ein Oracle DB-, Oracle RAC- oder Oracle Exadata Database-System handeln. 

Wenn die Wahl auf Oracle DB fällt, kann die Datenbankinstanz in Azure über die Oracle DB-Images bereitgestellt werden, die auf dem Azure Marketplace verfügbar sind. Alternativ können Sie auch die Verbindung zwischen Azure und der OCI verwenden, um Oracle DB in einem PaaS-Modell in der OCI bereitzustellen.

Für Oracle RAC können Sie OCI in einem PaaS-Modell verwenden. Wir empfehlen Ihnen, ein RAC-System mit zwei Knoten zu verwenden. Oracle RAC kann zwar in einem IaaS-Modell in Azure CloudSimple bereitgestellt werden, dies ist jedoch keine von Oracle unterstützte Konfiguration. Weitere Informationen finden Sie unter [Oracle-Programme, die für autorisierte Cloudumgebungen zulässig sind](http://www.oracle.com/us/corporate/pricing/authorized-cloud-environments-3493562.pdf).

Verwenden Sie für Exadata-Systeme die OCI-Verbindung, und stellen Sie das Exadata-System in der OCI bereit. Im obigen Architekturdiagramm ist ein Exadata-System dargestellt, das in der OCI in zwei Subnetzen bereitgestellt wurde.

Stellen Sie für Produktionsszenarien mehrere Instanzen der Datenbank in zwei Verfügbarkeitszonen (bei Bereitstellung in Azure) bzw. zwei Verfügbarkeitsdomänen (in der OCI) bereit. Nutzen Sie Oracle Active Data Guard, um die primäre Datenbank und die Standbydatenbank zu synchronisieren.

Die Datenbankebene empfängt nur Anforderungen von der mittleren Ebene. Wir empfehlen Ihnen, eine Netzwerksicherheitsgruppe einzurichten (Sicherheitsliste bei Bereitstellung der Datenbank in der OCI), um nur Anforderungen über Port 1521 von der mittleren Ebene und Port 22 vom Bastionsserver zu Verwaltungszwecken zuzulassen.

Für in der OCI bereitgestellte Datenbanken muss ein separates virtuelles Cloudnetzwerk mit einem Gateway für dynamisches Routing (Dynamic Routing Gateway, DRG) eingerichtet werden, das mit Ihrer FastConnect-Leitung verbunden ist.