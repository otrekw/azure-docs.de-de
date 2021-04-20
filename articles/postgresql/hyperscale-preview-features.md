---
title: Previewfunktionen in Azure Database for PostgreSQL – Hyperscale (Citus)
description: Aktualisierte Liste der Features, die sich derzeit in der Vorschauphase befinden
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: overview
ms.date: 04/07/2021
ms.openlocfilehash: 5f0552e09464a6b571a665cfe3895f48c3aa8c41
ms.sourcegitcommit: 20f8bf22d621a34df5374ddf0cd324d3a762d46d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107258451"
---
# <a name="preview-features-for-postgresql---hyperscale-citus"></a>Previewfunktionen für PostgreSQL – Hyperscale (Citus)

Azure Database for PostgreSQL Hyperscale (Citus) bietet Vorschauversionen für noch nicht veröffentlichte Features. Previewversionen werden ohne Vereinbarung zum Servicelevel bereitgestellt und sind nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.  Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="features-currently-in-preview"></a>Funktionen, die sich derzeit in der Vorschauphase befinden

Nachfolgend sind die Funktionen aufgeführt, die derzeit als Previewfunktionen verfügbar sind:

* **[Tarif „Basic“:](concepts-hyperscale-tiers.md)** Führen Sie eine Servergruppe nur mithilfe eines Koordinatorknotens und ohne Workerknoten aus. Bietet eine kostengünstige Möglichkeit, erste Tests und Entwicklungsschritte durchzuführen und kleine Produktionsworkloads zu verarbeiten.
* **[PostgreSQL 12 und 13:](concepts-hyperscale-versions.md)**
  Verwenden Sie die neueste Datenbankversion in Ihrer Servergruppe.
* **[Citus 10:](concepts-hyperscale-versions.md#citus-and-other-extension-versions)**
  Wird automatisch für Servergruppen installiert, auf denen PostgreSQL 13 ausgeführt wird.
* **[Spaltenbasierter Speicher:](concepts-hyperscale-columnar.md)**
  Speichern Sie die Spalten (anstelle von Zeilen) ausgewählter Tabellen zusammenhängend auf dem Datenträger. Unterstützt die Komprimierung auf dem Datenträger. Eignet sich gut für Analyse- und Data Warehousing-Workloads.
* **[Lesereplikate](howto-hyperscale-read-replicas-portal.md)** (derzeit nur in derselben Region): Alle Änderungen an der primären Servergruppe werden in ihrem Replikat widergespiegelt, und Abfragen für das Replikat führen nicht zu einer zusätzlichen Auslastung des ursprünglichen Replikats.
  Replikate sind ein nützliches Tool zur Verbesserung der Leistung für schreibgeschützte Workloads.
* **[Verwaltete PgBouncer-Instanz:](concepts-hyperscale-limits.md#managed-pgbouncer-preview)**
  Ein Verbindungspoolfunktion, die gleichzeitige Verbindungen zahlreicher Clients mit der Servergruppe ermöglicht und gleichzeitig die Anzahl aktiver Verbindungen begrenzt. Sie erfüllt Verbindungsanforderungen und sorgt gleichzeitig für eine reibungslose Ausführung des Koordinatorknotens.
* **[pgAudit:](concepts-hyperscale-audit.md)** Bietet eine detaillierte Sitzungs- und Objektüberwachungsprotokollierung über die standardmäßige PostgreSQL-Protokollierungseinheit. Die Erweiterung generiert Überwachungsprotokolle, die zum Bestehen bestimmter Behörden-, Finanz- oder ISO-Zertifizierungsaudits erforderlich sind.

### <a name="available-regions-for-preview-features"></a>Verfügbare Regionen für Previewfunktionen

Die pgAudit-Erweiterung ist in allen [von Hyperscale (Citus) unterstützten Regionen verfügbar](concepts-hyperscale-configuration-options.md#regions).
Die anderen Previewfunktionen sind nur in der Region **USA, Osten** verfügbar.

## <a name="does-my-server-group-have-access-to-preview-features"></a>Hat meine Servergruppe Zugriff auf Previewfunktionen?

Navigieren Sie im Azure-Portal zur Seite **Übersicht**, um zu ermitteln, ob für Ihre Hyperscale (Citus)-Servergruppe Previewfunktionen aktiviert sind.
Wird die Eigenschaft **Ebene: Basic (Vorschau)** oder **Ebene: Standard (Vorschau)** angezeigt, hat Ihre Servergruppe Zugriff auf Previewfunktionen.

### <a name="how-to-get-access"></a>Erhalten von Zugriff

Aktivieren Sie beim Erstellen einer neuen Hyperscale (Citus)-Servergruppe das Kontrollkästchen **Enable preview features** (Previewfunktionen aktivieren).

## <a name="contact-us"></a>Kontakt

Berichten Sie uns von Ihrer Erfahrung mit Previewfunktionen, indem Sie eine E-Mail an [Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) senden.
(Bei dieser E-Mail-Adresse handelt es sich nicht um einen Kanal für den technischen Support. Öffnen Sie bei technischen Problemen eine [Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).)
