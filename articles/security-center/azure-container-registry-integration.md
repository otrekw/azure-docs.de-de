---
title: Azure Security Center und Azure Container Registry
description: Erfahren Sie mehr über die Integration von Azure Security Center in Azure Container Registry.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: f3ef633ff0271d74eea7320faadf17685976d3b6
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970466"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Integration von Security Center in Azure Container Registry

Azure Container Registry (ACR) ist ein verwalteter, privater Docker-Registrierungsdienst, der Ihre Containerimages für Azure-Bereitstellungen in einer zentralen Registrierung speichert und verwaltet. Er basiert auf der Open-Source-Docker-Registrierung 2.0.

Wenn Sie den Standardtarif von Azure Security Center verwenden, können Sie das Paket „Containerregistrierungen“ hinzufügen. Dieses optionale Feature bietet Ihnen tiefere Einblicke in die Sicherheitsrisiken der Images in Ihren ARM-basierten Registrierungen. Sie können das Paket auf Abonnementebene aktivieren oder deaktivieren, um alle Registrierungen in einem Abonnement abzudecken. Die Verwendung dieses Features wird wie auf der [Tarifseite](security-center-pricing.md) gezeigt pro Image in Rechnung gestellt. Durch Aktivierung des Pakets „Containerregistrierungen“ wird sichergestellt, dass Security Center bereit ist, Images zu überprüfen, die an die Registrierung gepusht werden. 


## <a name="availability"></a>Verfügbarkeit

- Status des Release: **Allgemeine Verfügbarkeit**
- Erforderliche Rollen: **Sicherheitsleseberechtigter** und [Azure Container Registry-Leser](https://docs.microsoft.com/azure/container-registry/container-registry-roles)
- Clouds: 
    - ✔ Kommerzielle Clouds
    - ✘ US Government-Cloud
    - ✘ China Government-Cloud, andere Government-Clouds


## <a name="when-are-images-scanned"></a>Wann werden Images überprüft?

Immer, wenn ein Image an Ihre Registrierung gepusht wird, überprüft Security Center es automatisch. Zum Auslösen der Überprüfung eines Images müssen Sie es also an Ihr Repository pushen.

Wenn die Überprüfung abgeschlossen ist (in der Regel nach ungefähr zehn Minuten, sie kann aber auch bis zu 40 Minuten dauern), sind die Ergebnisse als Security Center-Empfehlungen wie die folgende verfügbar:

[![Beispiel einer Azure Security Center-Empfehlung über in einem gehosteten Azure Container Registry-Image (ACR) erkannte Sicherheitsrisiken](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Vorteile der Integration

Security Center identifiziert ARM-basierte ACR-Registrierungen in Ihrem Abonnement und bietet nahtlos Folgendes:

* **Azure-native Sicherheitsrisikoüberprüfung** für alle gepushten Linux-Images. Security Center überprüft das Image mithilfe eines Scanners von Qualys, dem branchenführenden Hersteller von Anwendungen zur Sicherheitsrisikoüberprüfung. Diese native Lösung ist standardmäßig nahtlos integriert.

* **Sicherheitsempfehlungen** für Linux-Images mit bekannten Sicherheitsrisiken. Security Center bietet Details zu den einzelnen gemeldeten Sicherheitsrisiken und eine Schweregradklassifizierung. Außerdem erhalten Sie Anleitungen zur Behebung der spezifischen Sicherheitsrisiken, die für jedes in die Registrierung gepushte Image gefunden wurden.

![Allgemeine Übersicht zu Azure Security Center und Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="acr-with-security-center-faq"></a>Häufig gestellte Fragen zu ACR mit Security Center

### <a name="what-types-of-images-can-azure-security-center-scan"></a>Welche Imagetypen kann Azure Security Center scannen?
Security Center scannt auf Linux-Betriebssystemen basierende Images, die Shellzugriff bereitstellen. 

Der Qualys-Scanner unterstützt keine extrem minimalistischen Images wie [Docker-Scratch](https://hub.docker.com/_/scratch/)-Images oder Images ohne Distribution, die nur Ihre Anwendung und deren Laufzeitabhängigkeiten ohne Paket-Manager, Shell oder Betriebssystem enthalten.

### <a name="how-does-azure-security-center-scan-an-image"></a>Wie scannt Azure Security Center ein Image?
Das Image wird aus der Registrierung gepullt. Anschließend wird es in einer isolierten Sandbox mit dem Qualys-Scanner ausgeführt, der eine Liste bekannter Sicherheitsrisiken extrahiert.

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.

### <a name="how-often-does-azure-security-center-scan-my-images"></a>Wie häufig scannt Azure Security Center meine Images?
Imagescans werden bei jedem Push ausgelöst.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Kann ich die Scanergebnisse über die REST-API abrufen?
Ja. Die Ergebnisse befinden sich unter [Sub-Assessments Rest API](/rest/api/securitycenter/subassessments/list/) (Unterbewertungen-REST-API). Außerdem können Sie Azure Resource Graph (ARG) verwenden, die Kusto-ähnliche API für alle Ihre Ressourcen: Mit einer Abfrage kann ein bestimmter Scan abgerufen werden.
 



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* [Schützen von Computern und Anwendungen in Azure Security Center](security-center-virtual-machine-protection.md) – Beschreibt Empfehlungen des Security Center.
