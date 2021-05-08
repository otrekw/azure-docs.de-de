---
title: Bereitstellen von BareMetal für Oracle
description: Erfahren Sie mehr über die Bereitstellung Ihrer BareMetal-Infrastruktur für Oracle.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: 45ca2bf8bfc61c3820b4b14daa998e2e82e972fa
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107558673"
---
# <a name="provision-baremetal-for-oracle"></a>Bereitstellen von BareMetal für Oracle

In diesem Artikel erfahren Sie, wie Sie Ihre BareMetal Infrastructure-Instanzen für Oracle-Workloads bereitstellen. 

Der erste Schritt zum Bereitstellen Ihrer BareMetal-Instanzen besteht darin, mit Ihrem Microsoft CSA zu arbeiten. Diese unterstützen Sie basierend auf Ihren spezifischen Workload-Anforderungen und der Architektur, die Sie bereitstellen, unabhängig davon, ob es um eine einzelne Instanz, eine RAC mit einem Knoten oder um eine RAC-Instanz geht. Weitere Informationen zu diesen Topologien finden Sie unter [Architektur der BareMetal-Infrastruktur für Oracle](oracle-baremetal-architecture.md).

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * Ein aktives Azure-Abonnement
> * Microsoft Premier Support-Vertrag
> * Lizenzen für Red Hat Enterprise Linux 7.6
> * Oracle-Supportvertrag 
> * Lizenzen und Softwareinstallationskomponenten für Oracle
> * **Lokale ExpressRoute-Verbindung mit Azure** (**Konfigurieren Sie optional** ExpressRoute-Global Reach für die direkte Konnektivität von der lokalen Oracle Database)   
> * Virtuelles Netzwerk
> * Gatewayerstellung
> * Virtuelle Computer (VMs) im virtuellen Netzwerk (Jumpboxen)

## <a name="information-to-provide-microsoft-operations"></a>Informationen zur Bereitstellung von Microsoft-Vorgängen

Sie müssen die folgenden Informationen in Ihrem CSA angeben:

1. Adressraum des virtuellen Netzwerks. Dieser Bereich muss das /24-Subnetz sein. Beispiel: 10.11.0.0/24.
2. P2P-Bereich. Dieser Bereich muss das /29-Subnetz sein. Beispiel: 10.12.0.0/29.
3. IP-Adresspool des Servers. Der empfohlene Bereich ist /24. Beispiel: 10.13.0.0/24.
4. IP-Adresse des Servers. Wählen Sie eine IP-Adresse aus dem Server-IP-Adresspool aus.

    > [!Note] 
    > Die ersten 30 IP-Adressen sind für die Konfiguration der Microsoft-Infrastruktur reserviert. In diesem Beispiel wäre Ihre erste verfügbare IP-Adresse für ein Blatt 10.13.0.30.

5. Die erforderliche Azure-Region. Beispiel: „USA, Westen, 2“.
6. Die BareMetal-Infrastruktur-SKU ist erforderlich. Beispiel: S192 (zwei Computer).

## <a name="storage-requirements"></a>Speicheranforderungen

Arbeiten Sie während der Bereitstellungsanforderung mit Ihrem CSA-Vertreter für Ihre Speicheranforderungen zusammen, einschließlich der erwarteten Speicheranforderungen basierend auf zukünftigem Wachstum. Hinzugefügter Speicher wird in Schritten von je 1 TB hinzugefügt.

Für Volumes befolgen wir den [OfA-Standard (Optimal Flexible Architecture)](https://docs.oracle.com/en/database/oracle/oracle-database/19/ladbi/about-the-optimal-flexible-architecture-standard.html#GUID-6619CDB7-9667-426E-8471-5A996707D093) von Oracle mit Basic-Ebene und Enterprise-Konfiguration. Wenn Sie andere benutzerdefinierte Speicheranforderungen als die standardmäßige „T-Shirt-Größe“ haben, stellen Sie Ihre benutzerdefinierte Anforderung über Ihr CSA.

| Grundlegende Konfiguration (POC/Testing) | BESCHREIBUNG | Klein | Medium | Groß |
| --- | --- | --- | --- | --- |
| /u01 | Oracle-Binärdateien | 500 GB | 500 GB | 500 GB |
| /u02 | Leseintensiv/Admin | 500 GB | 1 TB | 5 TB |
| /u03 | Schreibintensiv/Protokolle | 500 GB | 1 TB | 5 TB |
| /u09 | Backup | 5 TB | 10 TB | 15 TB |

| Unternehmenskonfiguration | BESCHREIBUNG | Klein | Medium | Groß | Sehr groß |
| --- | --- | --- | --- | --- | --- |
| /u01 | Oracle-Binärdateien | 500 GB | 500 GB | 500 GB | 500 GB |
| /u02 | Admin | 100 GB | 100 GB | 100 GB | 100 GB |
| /u10 bis /u59 | Leseintensiv | 500 GB | 5 TB | 10 TB | 20 TB |
| /u60 bis /u89 | Schreibintensiv | 500 GB | 5 TB | 10 TB | 20 TB |
| /u90 bis /u91 | Wiederholungsprotokolle | 500 GB | 500 GB | 1 TB | 1 TB |
| /u95 | Archivieren | 10 TB | 10 TB | 20 TB | 20 TB |
| /u98 | Backup | 25 TB | 25 TB | 50 TB | 50 TB |

## <a name="next-step"></a>Nächster Schritt

Erfahren Sie mehr über BareMetal Infrastructure für Oracle.

> [!div class="nextstepaction"]
> [Worum handelt es sich bei der BareMetal-Infrastruktur in Azure?](../../concepts-baremetal-infrastructure-overview.md)
