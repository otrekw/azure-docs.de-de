---
title: Liste der Systemabfragen
description: Hier erfahren Sie mehr über Systemabfragen, die Sie verwenden können, um Analysedaten zu Ihren Angeboten im kommerziellen Marketplace von Microsoft programmgesteuert abzurufen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: f2b5f7eb559e349947f88067a3d2ea53d99b7cbf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102583500"
---
# <a name="list-of-system-queries"></a>Liste der Systemabfragen

Die folgenden Systemabfragen können mit `QueryId` direkt in der [API zum Erstellen von Berichten](analytics-programmatic-access.md#create-report-api) verwendet werden. Die Systemabfragen sind wie die Exportberichte in Partner Center für einen Berechnungszeitraum von 6 Monaten (6M) konzipiert.

Weitere Informationen zu den Spaltennamen, Attributen und Beschreibungen finden Sie in den folgenden Artikeln:

- [Dashboard „Kunden“ in Analysen für den kommerziellen Marketplace](customer-dashboard.md#customer-details-table)
- [Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace](orders-dashboard.md#orders-details-table)
- [Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace](usage-dashboard.md#usage-details-table)
- [Dashboard „Marketplace-Erkenntnisse“ in Analysen für den kommerziellen Marketplace](insights-dashboard.md#marketplace-insights-details-table)

In den folgenden Abschnitten werden Berichtsabfragen für verschiedene Berichte bereitgestellt.

## <a name="customers-report-query"></a>Abfrage des Berichts zu Kunden

**Berichtsbeschreibung**: Bericht zu Kunden über die letzten 6 Monate

**Abfrage-ID**: `b9df4929-073f-4795-b0cb-a2c81b11e28d`

**Berichtsabfrage**:

`SELECT MarketplaceSubscriptionId,DateAcquired,DateLost,ProviderName,ProviderEmail,FirstName,LastName,Email,CustomerCompanyName,CustomerCity,CustomerPostalCode,CustomerCommunicationCulture,CustomerCountryRegion,AzureLicenseType,PromotionalCustomers,CustomerState,CommerceRootCustomer,CustomerId,BillingAccountId,ID  FROM ISVCustomer TIMESPAN LAST_6_MONTHS`

## <a name="orders-report-query"></a>Abfrage des Berichts zu Bestellungen

**Berichtsbeschreibung**: Bericht zu Bestellungen über die letzten 6 Monate

**Abfrage-ID**: `fd0f299c-5a1c-4929-9f48-bfc6cc44355d`

**Berichtsabfrage**:

`SELECT MarketplaceSubscriptionId,MonthStartDate,OfferType,AzureLicenseType,MarketplaceLicenseType,Sku,CustomerCountry,IsPreviewSKU,OrderId,OrderQuantity,CloudInstanceName,IsNewCustomer,OrderStatus,OrderCancelDate,CustomerCompanyName,CustomerName,OrderPurchaseDate,OfferName,TrialEndDate,CustomerId,BillingAccountId FROM ISVOrder TIMESPAN LAST_6_MONTHS`

## <a name="usage-report-queries"></a>Abfrage des Berichts zur Nutzung

**Berichtsbeschreibung**: Bericht zur normalisierten VM-Nutzung über die letzten 6 Monate

**Abfrage-ID**: `2c6f384b-ad52-4aed-965f-32bfa09b3778`

**Berichtsabfrage**:

`SELECT MarketplaceSubscriptionId,MonthStartDate,OfferType,AzureLicenseType,MarketplaceLicenseType,SKU,CustomerCountry,IsPreviewSKU,SKUBillingType,IsInternal,VMSize,CloudInstanceName,ServicePlanName,OfferName,DeploymentMethod,CustomerName,CustomerCompanyName,UsageDate,IsNewCustomer,CoreSize,TrialEndDate,CustomerCurrencyCC,PriceCC,PayoutCurrencyPC,EstimatedPricePC,UsageReference,UsageUnit,CustomerId,BillingAccountId,NormalizedUsage,EstimatedExtendedChargeCC,EstimatedExtendedChargePC FROM ISVUsage WHERE OfferType IN ('vm core image', 'Virtual Machine Licenses', 'multisolution') TIMESPAN LAST_6_MONTHS`

**Berichtsbeschreibung**: Bericht zur tatsächlichen VM-Nutzung über die letzten 6 Monate

**Abfrage-ID**: `3f19fb95-5bc4-4ee0-872e-cedd22578512`

**Berichtsabfrage**:

`SELECT MarketplaceSubscriptionId,MonthStartDate,OfferType,AzureLicenseType,MarketplaceLicenseType,SKU,CustomerCountry,IsPreviewSKU,SKUBillingType,IsInternal,VMSize,CloudInstanceName,ServicePlanName,OfferName,DeploymentMethod,CustomerName,CustomerCompanyName,UsageDate,IsNewCustomer,CoreSize,TrialEndDate,CustomerCurrencyCC,PriceCC,PayoutCurrencyPC,EstimatedPricePC,UsageReference,UsageUnit,CustomerId,BillingAccountId,RawUsage,EstimatedExtendedChargeCC,EstimatedExtendedChargePC FROM ISVUsage WHERE OfferType IN ('vm core image', 'Virtual Machine Licenses', 'multisolution') TIMESPAN LAST_6_MONTHS`

**Berichtsbeschreibung**: Bericht zur gemessenen Nutzung über die letzten 6 Monate

**Abfrage-ID**: `f0c4927f-1f23-4c99-be4a-1371a5a9a086`

**Berichtsabfrage**:

`SELECT MarketplaceSubscriptionId,MonthStartDate,OfferType,AzureLicenseType,MarketplaceLicenseType,SKU,CustomerCountry,IsPreviewSKU,SKUBillingType,IsInternal,VMSize,CloudInstanceName,ServicePlanName,OfferName,DeploymentMethod,CustomerName,CustomerCompanyName,UsageDate,IsNewCustomer,CoreSize,TrialEndDate,CustomerCurrencyCC,PriceCC,PayoutCurrencyPC,EstimatedPricePC,UsageReference,UsageUnit,CustomerId,BillingAccountId,MeteredUsage,EstimatedExtendedChargeCC,EstimatedExtendedChargePC FROM ISVUsage WHERE OfferType IN ('SaaS', 'Azure Applications') TIMESPAN LAST_6_MONTHS`

## <a name="marketplace-insights-report-query"></a>Abfrage des Berichts zu Marketplace-Erkenntnissen

**Berichtsbeschreibung**: Bericht zu Marketplace-Erkenntnissen über die letzten 6 Monate

**Abfrage-ID**: `6fd7624b-aa9f-42df-a61d-67d42fd00e92`

**Berichtsabfrage**:

`Date,OfferName,ReferralDomain,CountryName,PageVisits,GetItNow,ContactMe,TestDrive,FreeTrial FROM ISVMarketplaceInsights TIMESPAN LAST_6_MONTHS`

## <a name="next-steps"></a>Nächste Schritte

- [APIs für den Zugriff auf kommerzielle Marketplace-Analysedaten](analytics-available-apis.md)
- [Beispielanwendung für den Zugriff auf kommerzielle Marketplace-Analysedaten](analytics-sample-application.md)
