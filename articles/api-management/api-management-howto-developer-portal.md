---
title: Übersicht über das Entwicklerportal in Azure API Management
titleSuffix: Azure API Management
description: Informieren Sie sich über das Entwicklerportal in API Management, eine anpassbare Website, auf der API-Consumer Ihre APIs untersuchen können.
services: api-management
documentationcenter: API Management
author: mikebudzynski
ms.service: api-management
ms.topic: article
ms.date: 04/15/2021
ms.author: apimpm
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 3f5da5ed6c828278eb25a9fa4fa5cfb90e16f8f4
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107815957"
---
# <a name="overview-of-the-developer-portal"></a>Übersicht über das Entwicklerportal

Das Entwicklerportal ist eine automatisch generierte, vollständig anpassbare Website mit der Dokumentation Ihrer APIs. Dort können API-Consumer ihre APIs ermitteln, erfahren, wie sie verwendet werden, Zugriff anfordern und die APIs ausprobieren.

Wie in diesem Artikel beschrieben, können Sie das Entwicklerportal für Ihre spezifischen Szenarien anpassen und erweitern. 

![Entwicklerportal für API Management](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="migration-from-the-legacy-portal"></a>Migration vom Legacyportal

> [!IMPORTANT]
> Das Legacyentwicklerportal ist mittlerweile veraltet, und es werden nur noch Sicherheitsupdates angewendet. Sie können die Anwendung bis Oktober 2023 wie gewohnt weiterhin verwenden, daraufhin wird sie aus allen API Management-Diensten entfernt.

Die Migration zum neuen Entwicklerportal ist in einem [eigenen Artikel in der Dokumentation](developer-portal-deprecated-migration.md) beschrieben.

## <a name="customization-and-styling"></a>Anpassung und Formatierung

Das Entwicklerportal kann mit dem integrierten, visuellen Drag & Drop-Editor angepasst und formatiert werden. Weitere Einzelheiten finden Sie in [diesem Tutorial](api-management-howto-developer-portal-customize.md).

## <a name="extensibility"></a><a name="managed-vs-self-hosted"></a> Erweiterbarkeit

Der API Management-Dienst umfasst ein integriertes **verwaltetes** Entwicklerportal, das laufend aktualisiert wird. Sie können über die Benutzeroberfläche des Azure-Portals darauf zugreifen.

Wenn Sie es mit benutzerdefinierter Logik erweitern müssen, was standardmäßig nicht unterstützt wird, können Sie die entsprechende Codebasis ändern. Die Codebasis des Portals ist [in einem GitHub-Repository verfügbar](https://github.com/Azure/api-management-developer-portal). Beispielsweise können Sie ein neues Widget implementieren, das in das Supportsystem eines Drittanbieters integriert ist. Wenn Sie neue Funktionen implementieren, können Sie aus folgenden Optionen wählen:

- **Selbsthosting** des resultierenden Portals außerhalb Ihres API Management-Diensts. Wenn Sie das Portal selbst hosten, sind Sie der Maintainer und selbst für dessen Upgrades verantwortlich. Die Unterstützung durch den Azure-Support ist auf die [grundlegende Einrichtung selbstgehosteter Portale](developer-portal-self-host.md) beschränkt.
- Öffnen Sie einen Pull Request, damit das API Management-Team neue Funktionen mit der Codebasis des **verwalteten** Portals zusammenführen kann.

Details und Anweisungen zur Erweiterbarkeit finden Sie im [GitHub-Repository](https://github.com/Azure/api-management-developer-portal) und im [Tutorial zum Implementieren eines Widgets](developer-portal-implement-widgets.md). Das [Tutorial zum Anpassen des verwalteten Portals](api-management-howto-developer-portal-customize.md) führt Sie Schritt für Schritt durch den Verwaltungsbereich des Portals, der sowohl in der **verwalteten** als auch in der **selbstgehosteten** Version zur Verfügung steht.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum neuen Entwicklerportal:

- [Zugreifen auf das neue Entwicklerportal und Verwalten des Portals](api-management-howto-developer-portal-customize.md)
- [Einrichten einer selbstgehosteten Version des Portals](developer-portal-self-host.md)
- [Implementieren Ihres eigenen Widgets](developer-portal-implement-widgets.md)

Weitere Ressourcen:

- [GitHub-Repository mit dem Quellcode](https://github.com/Azure/api-management-developer-portal)
- [Häufig gestellte Fragen zum Entwicklerportal](developer-portal-faq.md)
