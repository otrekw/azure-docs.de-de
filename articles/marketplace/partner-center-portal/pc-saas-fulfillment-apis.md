---
title: SaaS-Fulfillment-APIs im kommerziellen Microsoft-Marketplace
description: Hier finden Sie eine Einführung in die Fulfillment-APIs, mit denen Sie Ihre SaaS-Angebote in Microsoft AppSource und in den Azure Marketplace integrieren können.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: ba1b158bc529b148a8e3138d122c13ead19e073e
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858090"
---
# <a name="saas-fulfillment-apis-in-microsoft-commercial-marketplace"></a>SaaS-Fulfillment-APIs im kommerziellen Microsoft-Marketplace

Mit den SaaS-Fulfillment-APIs können unabhängige Softwarehersteller (Independent Software Vendors, ISVs) ihre SaaS-Anwendungen in Microsoft AppSource und in den Azure Marketplace integrieren. Mit diesen APIs können ISVs für ihre Anwendungen alle Handelskanäle nutzen: direkt, über Partner (Reseller) und über den Vertrieb. Sie sind erforderlich, um transaktionsfähige SaaS-Angebote in Microsoft AppSource und im Azure Marketplace listen zu können.

> [!WARNING]
> Die aktuelle Version dieser API ist Version 2, die für alle neuen SaaS-Angebote verwendet werden sollte.  Version 1 der API ist veraltet und wird zur Unterstützung vorhandener Angebote noch gepflegt.

## <a name="business-model-support"></a>Geschäftsmodellunterstützung

Diese API unterstützt die folgenden Geschäftsmodellfunktionen für folgende Aufgaben:

* Definieren mehrerer Pläne für ein Angebot. Diese Pläne haben unterschiedliche Funktionen und werden möglicherweise unterschiedlich berechnet.
* Bereitstellen eines Angebots auf Website- oder Benutzer-Abrechnungsbasis.
* Bereitstellen monatlicher und jährlicher (Vorauszahlung) Abrechnungsoptionen.
* Bereitstellen eines privaten Preismodells für einen Kunden auf der Grundlage einer ausgehandelten Geschäftsvereinbarung.


## <a name="next-steps"></a>Nächste Schritte

Wenn nicht bereits geschehen, registrieren Sie Ihre SaaS-Anwendung im [Azure-Portal](https://ms.portal.azure.com) wie unter [Registrieren einer Azure AD-Anwendung](./pc-saas-registration.md) erklärt.  Verwenden Sie anschließend für die Entwicklung die aktuelle Version dieser Schnittstelle: [SaaS Fulfillment API Version 2](./pc-saas-fulfillment-api-v2.md).
