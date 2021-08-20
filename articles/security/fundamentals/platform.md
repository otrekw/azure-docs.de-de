---
title: 'Azure Plattformintegrität und -sicherheit: Azure Security'
description: Technische Übersicht über die Integrität und Sicherheit der Azure-Plattform.
author: yosharm
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 06/24/2021
ms.openlocfilehash: e0522d6dcb02571a1ed197d60734906ee8131ab4
ms.sourcegitcommit: 5be51a11c63f21e8d9a4d70663303104253ef19a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2021
ms.locfileid: "112893958"
---
# <a name="platform-integrity-and-security-overview"></a>Übersicht über Plattformintegrität und -sicherheit
Die Azure-Flotte besteht aus Millionen von Servern (Hosts), zu denen täglich Tausende hinzukommen. Tausende von Hosts durchlaufen zudem täglich Wartungen durch Neustarts, Aktualisierungen des Betriebssystems oder Reparaturen. Bevor ein Host der Flotte beitreten und mit der Annahme von Kundenworkloads beginnen kann, überprüft Microsoft, ob sich der Host in einem sicheren und vertrauenswürdigen Zustand befindet. Durch diese Überprüfung wird sichergestellt, dass während Lieferketten- oder Wartungsworkflows keine schädlichen oder unbeabsichtigten Änderungen an den Startsequenzkomponenten vorgenommen wurden.

## <a name="securing-azure-hardware-and-firmware"></a>Sichern von Azure-Hardware und -Firmware
In dieser Artikelreihe wird beschrieben, wie Microsoft die Integrität und Sicherheit von Hosts in verschiedenen Phasen ihres Lebenszyklus gewährleistet, von der Fertigung bis hin zum Ablauf. Die Artikel behandeln folgende Themen:
 
- [Firmwaresicherheit](firmware.md)
- [Codeintegrität auf der Azure-Plattform](code-integrity.md)
- [UEFI Secure Boot](secure-boot.md)
- [Gemessener Start- und Hostnachweis](measured-boot-host-attestation.md)
- [Project Cerberus](project-cerberus.md)
- [Verschlüsselung ruhender Daten](encryption-atrest.md)
- [Hypervisor-Sicherheit](hypervisor.md)
 
## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Microsoft innerhalb des Cloudhardware-Ökosystems aktiv Partnerschaften eingeht, um kontinuierliche [Firmware-Sicherheitsverbesserungen](firmware.md) voranzutreiben.

- Informieren Sie sich über die [gemeinsame Verantwortung in der Cloud](shared-responsibility.md).