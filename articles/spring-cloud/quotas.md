---
title: Servicepläne und Dienstkontingente für Azure Spring Cloud
description: Erfahren Sie mehr über Dienstkontingente und Servicepläne für Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: b02ccb3acb4546e08e7d58159ab9d85bca2d0eed
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877660"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Kontingente und Servicepläne für Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

Für die Ressourcen und Funktionen aller Azure-Dienste gelten festgelegte Standardlimits und -kontingente.   Azure Spring Cloud bietet zwei Tarife: Basic und Standard. In diesem Artikel werden die Grenzwerte für beide Tarife erläutert.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Dienstebenen und Grenzwerte für Azure Spring Cloud

| Resource | `Scope` | Basic | Standard
------- | ------- | -------
vCPU | pro App-Instanz | 1 | 4
Arbeitsspeicher | pro App-Instanz | 2 GB | 8 GB
Azure Spring Cloud-Dienstinstanzen | pro Region und Abonnement | 10 | 10
Gesamte App-Instanzen | pro Azure Spring Cloud-Dienstinstanz | 25 | 500
Benutzerdefinierte Domänen | pro Azure Spring Cloud-Dienstinstanz | 0 | 25 
Persistente Volumes | pro Azure Spring Cloud-Dienstinstanz | 1 GB/App × 10 Apps | 50 GB/App × 10 Apps

> [!TIP]
> Die für die Gesamtzahl der App-Instanzen pro Dienstinstanz aufgeführten Grenzwerte gelten für Apps und Bereitstellungen in beliebigem Zustand, einschließlich des beendeten Zustands. Löschen Sie Apps oder Bereitstellungen, die nicht verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Einige Standardgrenzwerte können erhöht werden. Sollte für Ihr Setup eine Erhöhung erforderlich sein, [erstellen Sie eine Supportanfrage](../azure-portal/supportability/how-to-create-azure-support-request.md).
