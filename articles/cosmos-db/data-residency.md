---
title: 'Vorgehensweise: Erfüllen der Datenresidenzanforderungen in Azure Cosmos DB'
description: Erfahren Sie, wie Sie die Datenresidenzanforderungen in Azure Cosmos DB erfüllen, damit Ihre Daten und Sicherungen in einer einzelnen Region verbleiben.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/05/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: c5f8a4361774368e3934d1e2b16c8311876f5caa
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491864"
---
# <a name="how-to-meet-data-residency-requirements-in-azure-cosmos-db"></a>Vorgehensweise: Erfüllen der Datenresidenzanforderungen in Azure Cosmos DB

In Azure Cosmos DB können Sie Ihre Daten und Sicherungen so konfigurieren, dass sie in einer einzelnen Region verbleiben, um die [Residenzanforderungen](https://azure.microsoft.com/en-us/global-infrastructure/data-residency/) zu erfüllen.

## <a name="residency-requirements-for-data"></a>Residenzanforderungen für Daten

In Azure Cosmos DB müssen Sie die regionsübergreifende Datenreplikation explizit konfigurieren. Erfahren Sie, wie Sie die Georeplikation mit [Azure-Portal](how-to-manage-database-account.md#addremove-regions-from-your-database-account) und [Azure CLI](scripts/cli/common/regions.md) konfigurieren. Um die Anforderungen an die Datenresidenz zu erfüllen, können Sie eine Azure-Richtlinie erstellen, die bestimmten Regionen ermöglicht, die Datenreplikation in unerwünschten Regionen zu verhindern.

## <a name="residency-requirements-for-backups"></a>Residenzanforderungen für Sicherungen

**Sicherungen im fortlaufenden Modus**: Diese Sicherungen sind standardmäßig resident, da sie entweder in lokal redundantem oder zonenredundantem Speicher gespeichert werden. Weitere Informationen finden Sie im Artikel [Konfigurieren und Verwalten von fortlaufender Sicherung und Zeitpunktwiederherstellung (Vorschau) – Verwenden des Azure-Portals](continuous-backup-restore-portal.md).

**Sicherungen im periodischen Modus**: Standardmäßig werden Kontosicherungen im periodischen Modus im georedundanten Speicher gespeichert. Für Sicherungen in periodischen Modi können Sie die Datenredundanz auf Kontoebene konfigurieren. Es gibt drei Redundanzoptionen für den Sicherungsspeicher. Dabei handelt es sich um lokale Redundanz, Zonenredundanz oder Georedundanz. Informieren Sie sich über das [Ändern des Sicherungsintervalls und des Aufbewahrungszeitraums](configure-periodic-backup-restore.md#configure-backup-interval-retention) im Portal.

## <a name="use-azure-policy-to-enforce-the-residency-requirements"></a>Erzwingen der Residenzanforderungen mit Azure Policy

Bei Datenresidenzanforderungen, laut denen Sie Ihre gesamten Daten in einer einzelnen Azure-Region speichern müssen, können Sie zonenredundante oder lokal redundante Sicherungen für Ihr Konto mit Azure Policy erzwingen.  Sie können auch eine Richtlinie erzwingen, dass die Azure Cosmos DB-Konten nicht in anderen Regionen georepliziert werden.

Azure Policy ist ein Dienst, mit dem Sie Richtlinien zum Anwenden von Regeln auf Azure-Ressourcen erstellen, zuweisen und verwalten können. Azure Policy hilft Ihnen, die Konformität dieser Ressourcen mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel sicherzustellen. Erfahren Sie, wie Sie mit [Azure Policy](policy.md) Governance und Kontrollen für Azure Cosmos DB-Ressourcen implementieren.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurieren und Verwalten der periodischen Sicherung mit dem [Azure-Portal](configure-periodic-backup-restore.md)
* Konfigurieren und verwalten Sie die fortlaufende Sicherung über das [Azure-Portal](continuous-backup-restore-portal.md) oder aber mithilfe von [PowerShell](continuous-backup-restore-powershell.md), [CLI](continuous-backup-restore-command-line.md) oder [Azure Resource Manager](continuous-backup-restore-template.md).
