---
title: Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault – Azure Key Vault| Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/24/2020
ms.author: sudbalas
ms.openlocfilehash: e4364c3c5bcba1a04837f3f9e7a0576579211c72
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88796594"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault: Verfügbarkeit und Redundanz

Azure Key Vault-Features bieten mehrere Redundanzebenen, um sicherzustellen, dass Ihre Schlüssel bzw. geheimen Schlüssel Ihrer Anwendung auch dann zur Verfügung stehen, wenn einzelne Komponenten des Dienstes ausfallen sollten.

Der Inhalt Ihres Schlüsseltresors wird innerhalb der Region sowie in eine sekundäre Region repliziert, die mindestens 240 km entfernt ist (jedoch innerhalb des gleichen Gebiets liegt), um eine hohe Dauerhaftigkeit Ihrer Schlüssel und Geheimnisse zu gewährleisten. Einzelheiten zu spezifischen Regionspaaren finden Sie im Dokument [Azure-Regionspaare](../../best-practices-availability-paired-regions.md).

Wenn einzelne Komponenten innerhalb des Key Vault-Diensts ausfallen, springen andere Komponenten in der Region ein, um Ihre Anforderung zu erfüllen, sodass die Funktionalität nicht beeinträchtigt wird. Sie müssen keine Maßnahmen ergreifen, um diesen Prozess zu starten. Dieser wird automatisch initiiert und ist für Sie transparent.

In dem seltenen Fall, dass eine gesamte Azure-Region nicht verfügbar sein sollte, werden die Anforderungen an den Azure Key Vault in dieser Region automatisch an eine sekundäre Region weitergeleitet (was als *Failover* bezeichnet wird). Wenn die primäre Region wieder verfügbar ist, werden Anforderungen wieder zurück an die primäre Region geleitet (was als *Failback* bezeichnet wird). Sie müssen wiederum keine Maßnahmen ergreifen, da diese Schritte automatisch erfolgen.

Durch diesen Entwurf für Hochverfügbarkeit erfordert Azure Key Vault keine Ausfallzeiten für Wartungsarbeiten.

Beachten Sie folgende Einschränkungen:

* Beim Failover einer Region kann es einige Minuten dauern, bis das Failover des Diensts erfolgt. Anforderungen während dieser Zeit vor dem Failover können fehlschlagen.
* Während eines Failovers befindet sich der Schlüsseltresor im schreibgeschützten Modus. Folgende Anforderungen werden in diesem Modus unterstützt:
  * Auflisten von Schlüsseltresoren
  * Abrufen von Eigenschaften von Schlüsseltresoren
  * Auflisten von Zertifikaten
  * Abrufen von Zertifikaten
  * Auflisten geheimer Schlüssel
  * Abrufen geheimer Schlüssel
  * Auflisten von Schlüsseln
  * Abrufen (von Eigenschaften) von Schlüsseltresoren
  * Verschlüsseln
  * Entschlüsseln
  * Umschließen
  * Aufheben der Umschließung
  * Überprüfen
  * Signieren
  * Backup
* Während des Failovers können Sie keine Änderungen an den Schlüsseltresoreigenschaften vornehmen. Sie können Konfigurationen und Einstellungen für Zugriffsrichtlinien oder Firewalls nicht ändern.
* Sobald auf ein Failover ein Failback erfolgt ist, stehen alle Anforderungstypen (einschließlich Lese- *und* Schreibanforderungen) wieder zur Verfügung.

