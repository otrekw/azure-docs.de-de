---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/15/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 10177dd949ac531027e13cf633b11c16674fd4ab
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2020
ms.locfileid: "94386523"
---
In der Vorschau weist NFS die folgenden Einschränkungen auf:

- NFS 4.1 unterstützt derzeit nur die obligatorischen Features aus der [Protokollspezifikation](https://tools.ietf.org/html/rfc5661). Optionale Features wie Delegierungen und Rückrufe aller Art, Upgrades und Downgrades von Sperren sowie Kerberos-Authentifizierung und -Verschlüsselung werden nicht unterstützt.
- Wenn sich die meisten Ihrer Anforderungen schwerpunktmäßig auf Metadaten beziehen, ist die Wartezeit im Vergleich zu Lese-/Schreib-/Aktualisierungsvorgängen schlechter.
- Es muss ein neues Speicherkonto erstellt werden, um eine NFS-Freigabe zu erstellen.
- Nur die REST-APIs der Verwaltungsebene werden unterstützt. REST-APIs der Datenebene sind nicht verfügbar. Das bedeutet, dass Tools wie der Speicher-Explorer mit NFS-Freigaben nicht funktionieren und Sie keine NFS-Freigabedaten im Azure-Portal durchsuchen können.
- Nur für den Premium-Tarif verfügbar.
- Derzeit nur mit lokal redundantem Speicher (LRS) verfügbar.

### <a name="azure-storage-features-not-yet-supported"></a>Noch nicht unterstützte Azure Storage-Funktionen

Auch die folgenden Features von Azure Files sind bei NFS-Freigaben nicht verfügbar:

- Identitätsbasierte Authentifizierung
- Azure Backup-Unterstützung
- Momentaufnahmen
- Vorläufiges Löschen
- Vollständige Unterstützung der Verschlüsselung während der Übertragung (Details finden Sie unter [NFS-Sicherheit](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure-Dateisynchronisierung (nur für Windows-Clients verfügbar, die von NFS 4.1 nicht unterstützt werden)
