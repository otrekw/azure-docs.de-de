---
title: 'Azure VMware Solutions (AVS): Sicherheit für AVS-Dienste'
description: Hier finden Sie eine Beschreibung der Modelle für gemeinsame Verantwortung, mit denen für die Sicherheit der AVS-Dienste gesorgt wird.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d2d45f827d4165175a4a5429f0b9393a55e2ff1e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024992"
---
# <a name="avs-security-overview"></a>AVS-Sicherheitsübersicht

Dieser Artikel enthält eine Übersicht darüber, wie die Sicherheit für den Azure VMware Solution by AVS-Dienst, die Infrastruktur und das Rechenzentrum implementiert wird. Sie erhalten Informationen zum Schutz und zur Sicherheit von Daten, zur Netzwerksicherheit und zur Verwaltung von Sicherheitsrisiken und Patches.

## <a name="shared-responsibility"></a>Gemeinsame Verantwortung

Für Azure VMware Solution by AVS wird aus Sicherheitsgründen ein Modell mit der Bezeichnung „Gemeinsame Verantwortung“ genutzt. Die vertrauenswürdige Sicherheit in der Cloud wird über die gemeinsame Verantwortung von Kunden und Microsoft als Dienstanbieter erreicht. Mit dieser Matrix aus Zuständigkeiten wird für eine höhere Sicherheit gesorgt, und Single Points of Failure werden beseitigt.

## <a name="azure-infrastructure"></a>Azure-Infrastruktur

Zu den Aspekten in Bezug auf die Sicherheit der Azure-Infrastruktur gehören die Datencenter und der Standort der Ausrüstung.

### <a name="datacenter-security"></a>Sicherheit im Datencenter

Microsoft verfügt über eine komplette Abteilung, die sich mit der Entwicklung, der Erstellung und dem Betrieb der Einrichtungen zur Unterstützung von Azure befasst. Dieses Team hat die Aufgabe, die physische Sicherheit auf dem neuesten Stand zu halten. Ausführlichere Informationen zur physischen Sicherheit finden Sie unter [Azure-Einrichtungen, Gelände und physische Sicherheit](../security/azure-physical-security.md).

### <a name="equipment-location"></a>Standort der Ausrüstung

Die Bare-Metal-Hardwareausrüstung, mit der Ihre privaten AVS-Clouds ausgeführt werden, wird an Standorten von Azure-Rechenzentren gehostet. Für den Zugang zu den Käfigen, in denen sich die Ausrüstung befindet, ist eine biometriebasierte zweistufige Authentifizierung erforderlich.

## <a name="dedicated-hardware"></a>Dedizierte Hardware

Im Rahmen des AVS-Diensts erhalten alle AVS-Kunden dedizierte Bare-Metal-Hosts mit angefügten lokalen Datenträgern, die von der anderen Mandantenhardware physisch isoliert sind. Ein ESXi-Hypervisor mit vSAN wird auf jedem Knoten ausgeführt. Die Knoten werden per VMware vCenter und NSX dediziert für Kunden verwaltet. Indem die Hardware von Mandanten nicht gemeinsam genutzt wird, wird eine zusätzliche Ebene der Isolation und des Schutzes geschaffen.

## <a name="data-security"></a>Datensicherheit

Kunden behalten die Kontrolle über ihre Daten und sind deren Besitzer. Der Kunde ist selbst für Data Steward-Aufgaben verantwortlich.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Schutz von ruhenden Daten und während der Übertragung in internen Netzwerken

Für ruhende Daten in der privaten AVS-Cloudumgebung können Sie die vSAN-Verschlüsselung verwenden. Die vSAN-Verschlüsselung funktioniert mit VMware-zertifizierten externen Schlüsselverwaltungsservern (Key Management Servers, KMS) in Ihrem eigenen virtuellen Netzwerk oder lokal. Sie steuern die Datenverschlüsselungsschlüssel selbst. Für Daten in der privaten AVS-Cloud während der Übertragung unterstützt vSphere die Verschlüsselung während dieses Übertragungsvorgangs für den gesamten VMkernel-Datenverkehr (einschließlich vMotion-Datenverkehr).

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Schutz von Daten, die über öffentliche Netzwerke übertragen werden müssen

Zum Schützen von Daten, die über öffentliche Netzwerke übertragen werden, können Sie IPsec- und SSL-VPN-Tunnel für Ihre private AVS-Clouds erstellen. Die gängigen Verschlüsselungsmethoden werden unterstützt, z. B. 128-Byte- und 256-Byte-AES. Daten während der Übertragung (einschließlich Authentifizierung, Administratorzugriff und Kundendaten) werden mit Standardmechanismen (SSH, TLS 1.2 und Secure RDP) verschlüsselt. Für die Kommunikation, bei der vertrauliche Informationen übermittelt werden, werden die Standardmechanismen für die Verschlüsselung genutzt.

### <a name="secure-disposal"></a>Sicheres Löschen

Wenn Ihr AVS-Dienst abläuft oder beendet wird, sind Sie für das Entfernen bzw. Löschen Ihrer Daten verantwortlich. AVS arbeitet mit Ihnen zusammen, wenn es um das Löschen oder Zurückgeben aller Kundendaten gemäß Kundenvereinbarung geht. Eine Ausnahme bilden personenbezogene Daten (teilweise oder gesamt), zu deren Aufbewahrung AVS gesetzlich verpflichtet ist. Falls personenbezogene Daten aufbewahrt werden müssen, werden diese von AVS archiviert, und es werden angemessene Maßnahmen getroffen, um für die Kundendaten eine weitere Verarbeitung zu verhindern.

### <a name="data-location"></a>Speicherort der Daten

Beim Einrichten Ihrer privaten AVS-Clouds wählen Sie die Azure-Region aus, in der diese bereitgestellt werden. Die Daten von virtuellen VMware-Computern werden nur aus dem physischen Datencenter verschoben, wenn Sie eine Datenmigration oder eine Sicherung der Daten an einem anderen Standort durchführen. Sie können für das Hosten von Workloads und Speichern von Daten auch mehrere Azure-Regionen nutzen, falls dies für Sie erforderlich ist.

Kundendaten, die sich auf hyperkonvergenten Knoten der privaten AVS-Cloud befinden, durchlaufen Standorte nur, wenn vom Mandantenadministrator eine entsprechende explizite Aktion durchgeführt wird. Sie sind dafür verantwortlich, Ihre Workloads so zu implementieren, dass für Hochverfügbarkeit gesorgt ist.

### <a name="data-backups"></a>Datensicherungen

AVS führt keine Sicherung oder Archivierung von Kundendaten durch. Bei AVS werden regelmäßige Sicherungen von vCenter- und NSX-Daten erstellt, um Hochverfügbarkeit für Verwaltungsserver zu ermöglichen. Vor der Sicherung werden alle Daten mit VMware-APIs auf der vCenter-Quelle verschlüsselt. Die verschlüsselten Daten werden per Azure-Blob transportiert und gespeichert. Verschlüsselungsschlüssel für Sicherungen werden in einem äußerst sicheren verwalteten AVS-Tresor gespeichert, der im virtuellen AVS-Netzwerk in Azure ausgeführt wird.

## <a name="network-security"></a>Netzwerksicherheit

Für die AVS-Lösung werden mehrere Ebenen der Netzwerksicherheit verwendet.

### <a name="azure-edge-security"></a>Azure-Edge-Sicherheit

Grundlage der AVS-Dienste ist die Basisnetzwerksicherheit, die über Azure bereitgestellt wird. Azure verfügt über Defense-in-Depth-Verfahren zur Erkennung und rechtzeitigen Reaktion auf netzwerkbasierte Angriffe mit anomalen Mustern bei ein- oder ausgehendem Datenverkehr und auf DDoS-Angriffe (Distributed Denial-of-Service). Diese Sicherheitskontrolle gilt für private AVS-Cloudumgebungen und die von AVS entwickelte Software für die Steuerungsebene.

### <a name="segmentation"></a>Segmentierung

Der AVS-Dienst verfügt über logisch getrennte Layer 2-Netzwerke, mit denen der Zugriff auf Ihre eigenen privaten Netzwerke in Ihrer privaten AVS-Cloudumgebung eingeschränkt wird. Mit einer Firewall können Sie Ihre privaten AVS-Cloudnetzwerke zusätzlich schützen. Über das AVS-Portal können Sie EW- und NS-Steuerungsregeln für den gesamten Netzwerkdatenverkehr definieren, z. B. für den Datenverkehr innerhalb einer privaten AVS-Cloud und zwischen privaten AVS-Clouds, für den allgemeinen Datenverkehr mit dem Internet sowie für den Netzwerkdatenverkehr in die lokale Umgebung über eine IPsec-VPN- oder ExpressRoute-Verbindung.

## <a name="vulnerability-and-patch-management"></a>Verwaltung von Sicherheitsrisiken und Patches

AVS ist für die regelmäßige Anwendung von Sicherheitspatches für verwaltete VMware-Software (ESXi, vCenter und NSX) verantwortlich.

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

Kunden können die Authentifizierung für ihr Azure-Konto (in Azure AD) durchführen, indem sie je nach Bedarf die mehrstufige Authentifizierung oder einmaliges Anmelden (SSO) verwenden. Im Azure-Portal können Sie das AVS-Portal starten, ohne dass Sie die Anmeldeinformationen neu eingeben müssen.

AVS unterstützt die optionale Konfiguration einer Identitätsquelle für die vCenter-Instanz der privaten AVS-Cloud. Sie können eine [lokale Identitätsquelle](set-vcenter-identity.md), eine neue Identitätsquelle für die private AVS-Cloud oder [Azure AD](azure-ad.md) verwenden.

Standardmäßig erhalten Kunden die Berechtigungen, die für den täglichen Betrieb von vCenter in der privaten AVS-Cloud benötigt werden. Diese Berechtigungsebene umfasst keinen Administratorzugriff auf vCenter. Falls vorübergehend Administratorzugriff erforderlich ist, können Sie [Ihre Berechtigungen für kurze Zeit erhöhen](escalate-private-cloud-privileges.md), während Sie die administrativen Aufgaben durchführen.
