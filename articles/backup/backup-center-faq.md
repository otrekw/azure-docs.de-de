---
title: 'Backup Center: FAQ'
description: In diesem Artikel finden Sie häufig gestellte Fragen zu Backup Center.
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: b10a9e73e65cf12c43ce28b429a8f12e0b960a76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993452"
---
# <a name="backup-center---frequently-asked-questions"></a>Backup Center: häufig gestellte Fragen (FAQ)

## <a name="management"></a>Verwaltung

### <a name="can-backup-center-be-used-across-tenants"></a>Kann Backup Center mandantenübergreifend verwendet werden?

Ja. Wenn Sie [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/overview) verwenden und den Zugriff auf Abonnements über verschiedene Mandanten delegiert haben, können Sie Backup Center als zentrale Benutzeroberfläche für die mandantenübergreifende Verwaltung von Sicherungen verwenden.

### <a name="can-backup-center-be-used-to-manage-both-recovery-services-vaults-and-backup-vaults"></a>Kann Backup Center zum Verwalten von Recovery Services-Tresoren und Sicherungstresoren verwendet werden?

Ja, Backup Center kann zum Verwalten sowohl von [Recovery Services-Tresoren](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) als auch von [Sicherungstresoren](backup-vault-overview.md) verwendet werden.

### <a name="is-there-a-delay-before-data-surfaces-in-backup-center"></a>Gibt es eine Verzögerung, bevor Daten in Backup Center angezeigt werden?

Backup Center dient zum Bereitstellen von Echtzeitinformationen. Zwischen dem Zeitpunkt, zu dem eine Entität auf dem Bildschirm eines einzelnen Tresors angezeigt wird, und dem Zeitpunkt, zu dem dieselbe Entität in Backup Center angezeigt wird, kann es zu einer Verzögerung von einigen Sekunden kommen.

## <a name="configuration"></a>Konfiguration

### <a name="do-i-need-to-configure-anything-to-see-data-in-backup-center"></a>Ist eine Konfiguration erforderlich, um Daten in Backup Center anzuzeigen?

Nein Backup Center ist sofort einsatzbereit. Sie müssen jedoch die Berichterstellung für Ihre Tresore konfigurieren, um [Sicherungsberichte](https://docs.microsoft.com/azure/backup/configure-reports) in Backup Center anzuzeigen.

### <a name="do-i-need-to-have-any-special-permissions-to-use-backup-center"></a>Benötige ich für die Verwendung von Backup Center besondere Berechtigungen?

Für Backup Center sind keine neuen Berechtigungen erforderlich. Wenn Sie über die richtige RBAC-Zugriffsebene für die von Ihnen verwalteten Ressourcen verfügen, können Sie Backup Center für diese Ressourcen verwenden. Wenn Sie z. B. Informationen zu Ihren Sicherungen anzeigen möchten, benötigen Sie Zugriff auf Ihre Tresore als **Leser**. Um die Sicherung zu konfigurieren und andere Aktionen im Zusammenhang mit der Sicherung auszuführen, benötigen Sie eine der Rollen **Sicherungsmitwirkender** oder **Sicherungsoperator**. Erfahren Sie mehr über [RBAC-Rollen für Azure Backup](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="pricing"></a>Preise

### <a name="do-i-need-to-pay-anything-extra-to-use-backup-explorer"></a>Muss ich für die Verwendung des Sicherungs-Explorers Zusatzkosten bezahlen?

Derzeit entstehen keine zusätzlichen Kosten (abgesehen von Ihren Sicherungskosten) für die Verwendung von Backup Center. Wenn Sie jedoch [Sicherungsberichte](https://docs.microsoft.com/azure/backup/configure-reports) in Backup Center verwenden, [entstehen Kosten](https://azure.microsoft.com/pricing/details/monitor/) für die Verwendung von Azure Monitor-Protokollen für Sicherungsberichte.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die anderen häufig gestellten Fragen:

* [Häufige Fragen zu Recovery Services-Tresoren](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq)
* [Häufige Fragen zu Azure-VM-Sicherungen](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq)
