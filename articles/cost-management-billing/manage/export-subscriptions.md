---
title: Exportieren von Informationen zu Ihrem Azure-Abonnement auf oberster Ebene | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie alle mit Ihrem Konto verknüpften Azure-Abonnement-IDs anzeigen können.
keywords: ''
author: bandersmsft
ms.reviewer: matrive
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 83b6a9db0107cb35448aec491c81c2630e87ddd7
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199720"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exportieren und Anzeigen von Informationen zu Ihrem Abonnement auf oberster Ebene
Wenn Sie den Satz der Abonnement-IDs anzeigen müssen, die Ihren Benutzeranmeldeinformationen zugeordnet sind, [laden Sie eine JSON-Datei mit Informationen zu Ihrem Abonnement aus dem Azure-Kontocenter herunter](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

Die heruntergeladene JSON-Datei enthält die folgenden Informationen:
- E-Mail: Die Ihrem Konto zugeordnete E-Mail-Adresse
- PUID: Der eindeutige Bezeichner, der Ihrem Abrechnungskonto zugeordnet ist
- Abonnement-IDs: Eine Liste der Abonnements, die zu Ihrem Konto gehören (aufgelistet nach Abonnement-ID)

### <a name="subscriptionsjson-sample"></a>JSON-Beispieldatei für ein Abonnement (subscriptions.json)

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
