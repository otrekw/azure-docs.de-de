---
title: Azure Security Center und Azure Container Registry
description: Erfahren Sie mehr über das Überprüfen Ihrer Containerregistrierungen mit Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 1335b1034304b7efe2b113f7ff2d2927fea41638
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90977362"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>Azure Container Registry-Imageprüfung durch Security Center

Azure Container Registry (ACR) ist ein verwalteter, privater Docker-Registrierungsdienst, der Ihre Containerimages für Azure-Bereitstellungen in einer zentralen Registrierung speichert und verwaltet. Er basiert auf der Open-Source-Docker-Registrierung 2.0.

Aktivieren Sie **Azure Defender für Containerregistrierungen**, um tiefere Einblicke in die Sicherheitsrisiken der Images in Ihren Azure Resource Manager-basierten Registrierungen zu erhalten. Sie können den Plan auf Abonnementebene aktivieren oder deaktivieren, um alle Registrierungen in einem Abonnement abzudecken. Die Verwendung dieses Features wird wie auf der [Tarifseite](security-center-pricing.md) gezeigt pro Image in Rechnung gestellt. Durch Aktivierung von Azure Defender wird sichergestellt, dass Security Center bereit ist, Images zu überprüfen, die an die Registrierung gepusht werden. 


## <a name="when-are-images-scanned"></a>Wann werden Images überprüft?

Immer, wenn ein Image an Ihre Registrierung gepusht wird, überprüft Security Center es automatisch. Zum Auslösen der Überprüfung eines Images müssen Sie es also an Ihr Repository pushen.

Wenn die Überprüfung abgeschlossen ist (in der Regel nach ungefähr zwei Minuten, sie kann aber auch bis zu 15 Minuten dauern), sind die Ergebnisse als Security Center-Empfehlungen wie diese verfügbar:

[![Beispiel einer Azure Security Center-Empfehlung über in einem gehosteten Azure Container Registry-Image (ACR) erkannte Sicherheitsrisiken](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Vorteile der Integration

Security Center identifiziert Azure Resource Manager-basierte ACR-Registrierungen in Ihrem Abonnement und bietet nahtlos Folgendes:

* **Azure-native Sicherheitsrisikoüberprüfung** für alle gepushten Linux-Images. Security Center überprüft das Image mithilfe eines Scanners von Qualys, dem branchenführenden Hersteller von Anwendungen zur Sicherheitsrisikoüberprüfung. Diese native Lösung ist standardmäßig nahtlos integriert.

* **Sicherheitsempfehlungen** für Linux-Images mit bekannten Sicherheitsrisiken. Security Center bietet Details zu den einzelnen gemeldeten Sicherheitsrisiken und eine Schweregradklassifizierung. Außerdem erhalten Sie Anleitungen zur Behebung der spezifischen Sicherheitsrisiken, die für jedes in die Registrierung gepushte Image gefunden wurden.

![Allgemeine Übersicht zu Azure Security Center und Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Häufig gestellte Fragen zum Überprüfen von Azure Container Registry-Images

### <a name="how-does-security-center-scan-an-image"></a>Wie überprüft Security Center ein Image?
Das Image wird aus der Registrierung gepullt. Anschließend wird es in einer isolierten Sandbox mit dem Qualys-Scanner ausgeführt, der eine Liste bekannter Sicherheitsrisiken extrahiert.

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.

### <a name="how-often-does-security-center-scan-my-images"></a>Wie häufig überprüft Security Center meine Images?
Imagescans werden bei jedem Push ausgelöst.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Kann ich die Scanergebnisse über die REST-API abrufen?
Ja. Die Ergebnisse befinden sich unter [Sub-Assessments Rest API](/rest/api/securitycenter/subassessments/list/) (Unterbewertungen-REST-API). Außerdem können Sie Azure Resource Graph (ARG) verwenden, die Kusto-ähnliche API für alle Ihre Ressourcen: Mit einer Abfrage kann ein bestimmter Scan abgerufen werden.
 
### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>Welche Typen von Registrierungen werden überprüft? Welche Typen werden abgerechnet?
Im Abschnitt zur Verfügbarkeit sind die Typen von Containerregistrierungen aufgelistet, die von Azure Defender für Containerregistrierungen unterstützt werden. 

Wenn Registrierungen, die nicht unterstützt werden, mit Ihrem Azure-Abonnement verbunden sind, werden sie nicht überprüft, und sie werden Ihnen nicht in Rechnung gestellt.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)


