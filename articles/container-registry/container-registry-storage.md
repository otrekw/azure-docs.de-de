---
title: Speichern von Containerimages
description: Details darüber, wie Ihre Docker-Containerimages in Azure Container Registry gespeichert werden, sowie über Sicherheit, Redundanz und Kapazität.
ms.topic: article
ms.date: 06/18/2020
ms.openlocfilehash: d51014e9e0769091aba42682cce3a6a01cfa19de
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85214059"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Speichern von Containerimages in Azure Container Registry

Jede Azure Container Registry vom Typ [Basic, Standard und Premium](container-registry-skus.md) profitiert von erweiterten Azure-Speicherfunktionen wie Verschlüsselung ruhender Daten für die Imagedatensicherheit und Georedundanz für den Imagedatenschutz. Die folgenden Abschnitte beschreiben sowohl die Funktionen als auch die Imagespeichergrenzen in Azure Container Registry (ACR).

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Alle Containerimages in Ihrer Registrierung werden im Ruhezustand verschlüsselt. Azure verschlüsselt ein Image automatisch, bevor es gespeichert wird, und entschlüsselt es dynamisch, sobald Sie oder Ihre Anwendungen und Dienste einen Pull für das Image ausführen. Wenden Sie optional eine zusätzliche Verschlüsselungsebene mit einem [kundenseitig verwalteten Schlüssel](container-registry-customer-managed-keys.md) an.

## <a name="geo-redundant-storage"></a>Georedundanter Speicher

Azure verwendet ein georedundantes Speicherschema, um den Verlust Ihrer Containerimages zu verhindern. Azure Container Registry repliziert die Containerimages automatisch in mehrere geografisch entfernte Rechenzentren und verhindert deren Verlust im Falle eines regionalen Speicherausfalls.

## <a name="geo-replication"></a>Georeplikation

Für Szenarien, die eine noch zuverlässigere Hochverfügbarkeit erfordern, sollten Sie das Feature [Georeplikation](container-registry-geo-replication.md) der Premium-Registrierungen nutzen. Die Georeplikation schützt vor dem Verlust des Zugriffs auf Ihre Registrierung im Falle eines *Totalausfalls* der Region, nicht nur eines Speicherausfalls. Zudem bietet die Georeplikation weitere Vorteile, wie z.B. netzwerknahe Imagespeicher für schnellere Push- und Pullvorgänge in verteilten Entwicklungs- oder Bereitstellungsszenarien.

## <a name="scalable-storage"></a>Skalierbarer Speicher

Mit Azure Container Registry können Sie beliebig viele benötigte Repositorys, Images, Ebenen oder Tags erstellen, bis zum [Registrierungsspeicherlimit](container-registry-skus.md#service-tier-features-and-limits). 

Eine hohe Anzahl von Repositorys und Tags können die Leistung Ihrer Registrierung beeinträchtigen. Löschen Sie regelmäßig unbenutzte Repositorys, Tags und Images als Teil der Wartungsroutine für Ihre Registrierung, und legen Sie optional eine [Aufbewahrungsrichtlinie](container-registry-retention-policy.md) für nicht mit Tags versehene Manifeste fest. Gelöschte Registrierungsressourcen wie Repositorys, Images und Tags können nach dem Löschen *nicht* wiederhergestellt werden. Weitere Informationen zum Löschen von Registrierungsressourcen finden Sie unter [Löschen von Containerimages in Azure Container Registry](container-registry-delete.md).

## <a name="storage-cost"></a>Speicherkosten

Ausführliche Informationen zu den Preisen finden Sie unter [Containerregistrierung – Preise][pricing].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containerregistrierungen „Basic“, „Standard“ und „Premium“ finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: https://aka.ms/acr/pricing

<!-- LINKS - Internal -->
