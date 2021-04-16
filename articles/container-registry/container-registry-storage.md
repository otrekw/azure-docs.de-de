---
title: Speichern von Containerimages
description: Hier erhalten Sie ausführliche Informationen dazu, wie Ihre Containerimages und andere Artefakte in Azure Container Registry gespeichert werden, und erfahren u. a. mehr über Sicherheit, Redundanz und Kapazität.
ms.topic: conceptual
ms.date: 03/24/2021
ms.custom: references_regions
ms.openlocfilehash: a9c8ec877ddb17603e82b763223278a2e5e36714
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047744"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Speichern von Containerimages in Azure Container Registry

Jede Azure-Containerregistrierung vom Typ [Basic, Standard und Premium](container-registry-skus.md) profitiert von erweiterten Azure-Speicherfunktionen, einschließlich Verschlüsselung ruhender Daten. Die folgenden Abschnitte beschreiben die Funktionen und die Imagespeichergrenzen in Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Containerimages und anderen Artefakte in Ihrer Registrierung werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch, bevor es gespeichert wird, und entschlüsselt es dynamisch, sobald Sie oder Ihre Anwendungen und Dienste einen Pull für das Image ausführen. Wenden Sie optional eine zusätzliche Verschlüsselungsebene mit einem [kundenseitig verwalteten Schlüssel](container-registry-customer-managed-keys.md) an.

## <a name="regional-storage"></a>Regionaler Speicher

Azure Container Registry speichert Daten in der Region, in der die Registrierung erstellt wurde, um Kunden bei der Erfüllung der Anforderungen an Datenresidenz und Compliance zu unterstützen. In allen Regionen mit Ausnahme von „Brasilien, Süden“ und „Asien, Südosten“ kann Azure auch Registrierungsdaten in einem Regionspaar in derselben Region speichern. In den Regionen „Brasilien, Süd“ und „Asien, Südosten“ sind Registrierungsdaten immer auf die Region beschränkt, um die Anforderungen an die Datenresidenz für diese Regionen zu erfüllen.

Wenn ein regionaler Ausfall auftritt, sind die Registrierungsdaten möglicherweise nicht mehr verfügbar und werden nicht automatisch wieder hergestellt. Kunden, die Ihre Daten in mehreren Regionen speichern möchten, um die Leistung für verschiedene geografische Regionen zu verbessern, oder die bei einem regionalen Ausfall Resilienz wünschen, sollten [Georeplikation](container-registry-geo-replication.md) aktivieren.

## <a name="geo-replication"></a>Georeplikation

Für Szenarien, die zuverlässige Hochverfügbarkeit erfordern, sollten Sie das Feature [Georeplikation](container-registry-geo-replication.md) der Premium-Registrierungen nutzen. Die Georeplikation schützt vor dem Verlust des Zugriffs auf Ihre Registrierung im Falle eines regionalen Ausfalls. Zudem bietet die Georeplikation weitere Vorteile, wie z.B. netzwerknahe Imagespeicher für schnellere Push- und Pullvorgänge in verteilten Entwicklungs- oder Bereitstellungsszenarien.

## <a name="zone-redundancy"></a>Zonenredundanz

Zum Erstellen einer resilienten und hochverfügbaren Azure-Containerregistrierung aktivieren Sie optional [Zonenredundanz](zone-redundancy.md) in ausgewählten Azure-Regionen. Zonenredundanz ist eine Funktion der Dienstebene „Premium“ und nutzt Azure-[Verfügbarkeitszonen](../availability-zones/az-overview.md), um Ihre Registrierung in mindestens drei separaten Zonen in jeder aktivierten Region zu replizieren. Kombinieren Sie Georeplikation und Zonenredundanz, um sowohl die Zuverlässigkeit als auch die Leistung einer Registrierung zu verbessern. 

## <a name="scalable-storage"></a>Skalierbarer Speicher

Mit Azure Container Registry können Sie beliebig viele benötigte Repositorys, Images, Ebenen oder Tags erstellen, bis zum [Registrierungsspeicherlimit](container-registry-skus.md#service-tier-features-and-limits). 

Eine hohe Anzahl von Repositorys und Tags kann die Leistung Ihrer Registrierung beeinträchtigen. Löschen Sie regelmäßig unbenutzte Repositorys, Tags und Images als Teil der Wartungsroutine für Ihre Registrierung, und legen Sie optional eine [Aufbewahrungsrichtlinie](container-registry-retention-policy.md) für nicht mit Tags versehene Manifeste fest. Gelöschte Registrierungsressourcen wie Repositorys, Images und Tags können nach dem Löschen *nicht* wiederhergestellt werden. Weitere Informationen zum Löschen von Registrierungsressourcen finden Sie unter [Löschen von Containerimages in Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Speicherkosten

Ausführliche Informationen zu den Preisen finden Sie unter [Containerregistrierung – Preise][pricing].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containerregistrierungen „Basic“, „Standard“ und „Premium“ finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
