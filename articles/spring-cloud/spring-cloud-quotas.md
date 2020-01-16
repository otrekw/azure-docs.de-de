---
title: Servicepläne und Dienstkontingente für Azure Spring Cloud
description: Erfahren Sie mehr über Dienstkontingente und Servicepläne für Azure Spring Cloud.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: a5352b371c5754672e668e53eb5e4211de9c46cc
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75891494"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Kontingente und Servicepläne für Azure Spring Cloud

Für die Ressourcen und Funktionen aller Azure-Dienste gelten festgelegte Standardlimits und -kontingente.  Während der Vorschauphase bietet Azure Spring Cloud nur einen Serviceplan.

In diesem Artikel werden die während der aktuellen Vorschauphase angebotenen Dienstkontingente ausführlich erläutert.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Dienstebenen und -kontingente für Azure Spring Cloud

Während der Vorschauphase bietet Azure Spring Cloud nur eine Dienstebene.

Resource | Amount
------- | -------
vCPU | 4 pro Dienstinstanz
Arbeitsspeicher | 8 GB pro Dienstinstanz
Azure Spring Cloud-Dienstinstanzen pro Region und Abonnement | 10
Gesamtanzahl der App-Instanzen pro Azure Spring Cloud-Dienstinstanz | 500
Gesamtanzahl der App-Instanzen pro Spring-Anwendung | 20
Persistente Volumes | 10 × 50 GB

Wenn Sie ein Kontingent erreichen, erhalten Sie einen 400-Fehler mit folgendem Inhalt: „Das Kontingent überschreitet den Grenzwert für das Abonnement *Ihr Abonnement* in der Region *Region, in der Ihr Azure Spring Cloud-Dienst erstellt wurde*.“

## <a name="next-steps"></a>Nächste Schritte

Bestimmte Standardlimits und Kontingente können erhöht werden. Wenn für Ihre Ressource eine Erhöhung erforderlich ist, [erstellen Sie eine Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).
