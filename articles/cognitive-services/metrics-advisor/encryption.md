---
title: Metrics Advisor-Dienstverschlüsselung
titleSuffix: Azure Cognitive Services
description: Verschlüsselung für ruhende Daten des Metrics Advisor-Diensts
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 9d6a2f8a69d41d29e635b03425ed738484d6f408
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931273"
---
# <a name="metrics-advisor-service-encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten des Metrics Advisor-Diensts

Mit dem Metrics Advisor-Dienst werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Durch die Verschlüsselung des Metrics Advisor-Diensts werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Vom Kunden verwaltete Schlüssel sind nur im Tarif E0 verfügbar. Wenn Sie die Möglichkeit haben möchten, kundenseitig verwaltete Schlüssel zu verwenden, füllen Sie das [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den Metrics Advisor-Dienst](https://aka.ms/cogsvc-cmk) aus, und reichen Sie es ein. Nach ca. 3–5 Werktagen erhalten Sie eine Rückmeldung zum Status Ihrer Anforderung. Je nach Bedarf können Sie in einer Warteschlange platziert und genehmigt werden, sobald Platz verfügbar ist. Nachdem Ihre Verwendung von CMK mit dem Metrics Advisor-Dienst genehmigt wurde, müssen Sie eine neue Metrics Advisor-Ressource erstellen und als Tarif „E0“ auswählen. Nachdem die Metrics Advisor-Ressource mit dem Tarif „E0“ erstellt wurde, können Sie mit Azure Key Vault Ihre verwaltete Identität einrichten.

[!INCLUDE [cognitive-services-cmk](../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den Metrics Advisor-Dienst](https://aka.ms/cogsvc-cmk)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
