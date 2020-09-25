---
title: Servicepläne und Dienstkontingente für Azure Spring Cloud
description: Erfahren Sie mehr über Dienstkontingente und Servicepläne für Azure Spring Cloud.
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 7c4c832819f61d208d0722823d0a74354960f182
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904261"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Kontingente und Servicepläne für Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

Für die Ressourcen und Funktionen aller Azure-Dienste gelten festgelegte Standardlimits und -kontingente.   Azure Spring Cloud bietet zwei Tarife: Basic und Standard. In diesem Artikel werden die Grenzwerte für beide Tarife erläutert.

## <a name="azure-spring-cloud-service-tiers-and-limits"></a>Dienstebenen und Grenzwerte für Azure Spring Cloud

| Ressource | Basic | Standard
------- | ------- | -------
vCPU | 1 pro Dienstinstanz | 4 pro Dienstinstanz
Arbeitsspeicher | 2 GB pro Dienstinstanz | 8 GB pro Dienstinstanz
Azure Spring Cloud-Dienstinstanzen pro Region und Abonnement | 10 | 10
Gesamtanzahl der App-Instanzen pro Azure Spring Cloud-Dienstinstanz | 25 | 500
Persistente Volumes | 1 GB/App × 10 Apps | 50 GB/App × 10 Apps

## <a name="next-steps"></a>Nächste Schritte

Einige Standardgrenzwerte können erhöht werden. Sollte für Ihr Setup eine Erhöhung erforderlich sein, [erstellen Sie eine Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).
