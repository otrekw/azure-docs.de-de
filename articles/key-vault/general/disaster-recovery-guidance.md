---
title: 'Azure Key Vault: Verfügbarkeit und Redundanz | Microsoft-Dokumentation'
description: Informationen zur Verfügbarkeit und Redundanz in Azure Key Vault
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: sudbalas
ms.openlocfilehash: aea5f0428fe55c0dae3734e196008cbc26a974b9
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576218"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault: Verfügbarkeit und Redundanz

Azure Key Vault-Features bieten mehrere Redundanzebenen, um sicherzustellen, dass Ihre Schlüssel bzw. geheimen Schlüssel Ihrer Anwendung auch dann zur Verfügung stehen, wenn einzelne Komponenten des Dienstes ausfallen sollten.

> [!NOTE]
> Diese Anleitung gilt für Tresore. Pools verwalteter HSMs verwenden ein anderes Modell für Hochverfügbarkeit und Notfallwiederherstellung. Weitere Informationen finden Sie unter [Notfallwiederherstellung für „Verwaltetes HSM“](../managed-hsm/disaster-recovery-guide.md).

Der Inhalt Ihres Schlüsseltresors wird innerhalb der Region sowie in eine sekundäre Region repliziert, die mindestens 240 km entfernt ist (jedoch innerhalb des gleichen Gebiets liegt), um eine hohe Dauerhaftigkeit Ihrer Schlüssel und Geheimnisse zu gewährleisten. Einzelheiten zu spezifischen Regionspaaren finden Sie im Dokument [Azure-Regionspaare](../../best-practices-availability-paired-regions.md). Eine Ausnahme vom Modell mit Regionspaaren ist die Region „Brasilien, Süden“. Sie bietet nur die Möglichkeit, Daten in „Brasilien, Süden“ zu speichern. „Brasilien, Süden“ nutzt lokal redundanten Speicher (LRS), um Ihre Daten dreimal innerhalb des einzelnen Standorts/der einzelnen Region zu replizieren.   

Wenn einzelne Komponenten innerhalb des Key Vault-Diensts ausfallen, springen andere Komponenten in der Region ein, um Ihre Anforderung zu erfüllen, sodass die Funktionalität nicht beeinträchtigt wird. Sie müssen keine Maßnahmen ergreifen, um diesen Prozess zu starten. Dieser wird automatisch initiiert und ist für Sie transparent.

In dem seltenen Fall, dass eine gesamte Azure-Region nicht verfügbar sein sollte, werden die Anforderungen an den Azure Key Vault in dieser Region automatisch an eine sekundäre Region weitergeleitet (was als *Failover* bezeichnet wird). Wenn die primäre Region wieder verfügbar ist, werden Anforderungen wieder zurück an die primäre Region geleitet (was als *Failback* bezeichnet wird). Sie müssen wiederum keine Maßnahmen ergreifen, da diese Schritte automatisch erfolgen.

Durch diesen Entwurf für Hochverfügbarkeit erfordert Azure Key Vault keine Ausfallzeiten für Wartungsarbeiten.

Beachten Sie folgende Einschränkungen:

* Beim Failover einer Region kann es einige Minuten dauern, bis das Failover des Diensts erfolgt. Anforderungen während dieser Zeit vor dem Failover können fehlschlagen.
* Wenn Sie die Verbindung mit dem Schlüsseltresor über einen privaten Link herstellen, kann es im Falle eines Failovers bis zu 20 Minuten dauern, bis die Verbindung wiederhergestellt wird. 
* Während eines Failovers befindet sich der Schlüsseltresor im schreibgeschützten Modus. Folgende Anforderungen werden in diesem Modus unterstützt:
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
