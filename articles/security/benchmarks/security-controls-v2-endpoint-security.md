---
title: 'Azure-Sicherheitsvergleichstest V2: Endpunktsicherheit'
description: Azure-Sicherheitsvergleichstest V2 für Endpunktsicherheit
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 48b22ba913370b27cd01319a14a2a627d7589ce4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051513"
---
# <a name="security-control-v2-endpoint-security"></a>Sicherheitskontrolle V2: Endpunktsicherheit

Endpunktsicherheit umfasst Steuerelemente im Zusammenhang mit Endpunkterkennung und -antwort. Dies beinhaltet die Verwendung von Endpunkterkennung und -antwort (Endpoint Detection and Response, EDR) und des Antischadsoftwarediensts für Endpunkte in Azure-Umgebungen.

Die entsprechende integrierte Azure Policy finden Sie unter [Details zur integrierten Initiative zur Einhaltung der gesetzlichen Bestimmungen gemäß Azure-Sicherheitsvergleichstest: Endpunktsicherheit](../../governance/policy/samples/azure-security-benchmark.md#endpoint-security).

## <a name="es-1-use-endpoint-detection-and-response-edr"></a>ES-1: Verwenden von Endpunkterkennung und -antwort (Endpoint Detection and Response, EDR)

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| ES-1 | 8.1 | SI-2, SI-3, SC-3 |

Aktivieren Sie EDR-Funktionen (Endpoint Detection and Response, Endpunkterkennung und -antwort) für Server und Clients, und implementieren Sie eine Integration für SIEM- und Security Operations-Prozesse.

Microsoft Defender für Endpunkt bietet EDR-Funktionen im Rahmen einer Endpunktsicherheitsplattform für Unternehmen, um komplexe Bedrohungen zu vermeiden, zu erkennen, zu untersuchen und darauf zu reagieren.

- [Microsoft Defender für Endpunkt – Übersicht](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

- [Microsoft Defender für Endpunkt für Windows-Server](/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)

- [Microsoft Defender für Endpunkt für Nicht-Windows-Server](/windows/security/threat-protection/microsoft-defender-atp/configure-endpoints-non-windows)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security)

- [Threat Intelligence](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Statusverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-2-use-centrally-managed-modern-anti-malware-software"></a>ES-2: Verwenden von moderner, zentral verwalteter Antischadsoftware

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| ES-2 | 8.1 | SI-2, SI-3, SC-3 |

Verwenden Sie eine zentral verwaltete Antischadsoftwarelösung für Endpunkte, mit der Echtzeitüberprüfungen und periodische Überprüfungen durchgeführt werden können.

Azure Security Center kann automatisch die Verwendung verschiedener gängiger Antischadsoftwarelösungen für Ihre virtuellen Computer erkennen, den Ausführungsstatus des Endpunktschutzes melden und Empfehlungen abgeben. 

Microsoft Antimalware für Azure Cloud Services ist die Standard-Antischadsoftware für virtuelle Windows-Computer (virtual machines, VMs). Verwenden Sie für virtuelle Linux-Computer eine Antischadsoftware eines Drittanbieters. Mithilfe der Azure Security Center-Bedrohungserkennung für Datendienste können Sie auch Schadsoftware erkennen, die in Azure Storage-Konten hochgeladen wurde. 

- [Konfigurieren von Microsoft Antimalware für Cloud Services und Virtual Machines](../fundamentals/antimalware.md)

- [Unterstützte Endpoint Protection-Lösungen](../../security-center/security-center-services.md?tabs=features-windows#supported-endpoint-protection-solutions-)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security)

- [Threat Intelligence](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Statusverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="es-3-ensure-anti-malware-software-and-signatures-are-updated"></a>ES-3: Sicherstellen der Aktualisierung von Antischadsoftware und Signaturen

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| ES-3 | 8,2 | SI-2, SI-3 |

Stellen Sie sicher, dass Antischadsoftwaresignaturen schnell und konsistent aktualisiert werden.

Befolgen Sie die Empfehlungen im Azure Security Center unter „Compute und Apps“, um sicherzustellen, dass alle Endpunkte mit den neuesten Signaturen auf dem aktuellen Stand sind. Mit Microsoft Antimalware werden standardmäßig die neuesten Signaturen und Engine-Updates automatisch installiert. Stellen Sie für Linux sicher, dass die Signaturen in der Antischadsoftware-Lösung des Drittanbieters aktualisiert werden.

- [Bereitstellen von Microsoft Antimalware für Azure Cloud Services und Virtual Machines](../fundamentals/antimalware.md)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security)

- [Threat Intelligence](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Statusverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Endpoint Protection: Bewertung und Empfehlungen in Azure Security Center](../../security-center/security-center-endpoint-protection.md)