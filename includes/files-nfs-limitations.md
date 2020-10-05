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
ms.openlocfilehash: 8901f54c4802766eb3f12830d98d29fb47337ae5
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90705213"
---
In der Vorschau weist NFS die folgenden Einschränkungen auf:

- NFS 4.1 unterstützt derzeit nur die obligatorischen Features aus der [Protokollspezifikation](https://tools.ietf.org/html/rfc5661). Optionale Features wie Delegierungen und Rückrufe aller Art, Upgrades und Downgrades von Sperren sowie Kerberos-Authentifizierung und -Verschlüsselung werden nicht unterstützt.
- Wenn sich die meisten Ihrer Anforderungen schwerpunktmäßig auf Metadaten beziehen, ist die Wartezeit im Vergleich zu Lese-/Schreib-/Aktualisierungsvorgängen schlechter.
- Es muss ein neues Speicherkonto erstellt werden, um eine NFS-Freigabe zu erstellen.
- Nur die REST-APIs der Verwaltungsebene werden unterstützt. REST-APIs der Datenebene sind nicht verfügbar, was bedeutet, dass Tools wie Speicher-Explorer nicht mit NFS-Freigaben funktionieren und Sie auch nicht in der Lage sind, NFS-Freigabedaten im Azure-Portal zu durchsuchen.
- Nur für den Premium-Tarif verfügbar.
- Derzeit nur mit lokal redundantem Speicher verfügbar.

### <a name="azure-storage-features-not-yet-supported"></a>Noch nicht unterstützte Azure Storage-Funktionen

Auch die folgenden Features von Azure Files sind bei NFS-Freigaben nicht verfügbar:

- Identitätsbasierte Authentifizierung
- Azure Backup-Unterstützung
- Momentaufnahmen
- Vorläufiges Löschen
- Vollständige Unterstützung der Verschlüsselung während der Übertragung (Details finden Sie unter [NFS-Sicherheit](../articles/storage/files/storage-files-compare-protocols.md#security))
- Azure-Dateisynchronisierung (nur für Windows-Clients verfügbar, die von NFS 4.1 nicht unterstützt werden)