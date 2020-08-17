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
ms.date: 08/02/2020
ms.author: memildin
ms.openlocfilehash: 718f9a29b70dab34269c959ccd62452e56a32d72
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056600"
---
# <a name="azure-container-registry-image-scanning-by-security-center"></a>Azure Container Registry-Imageprüfung durch Security Center

Azure Container Registry (ACR) ist ein verwalteter, privater Docker-Registrierungsdienst, der Ihre Containerimages für Azure-Bereitstellungen in einer zentralen Registrierung speichert und verwaltet. Er basiert auf der Open-Source-Docker-Registrierung 2.0.

Wenn Sie den Standardtarif von Azure Security Center verwenden, können Sie das Paket „Containerregistrierungen“ hinzufügen. Dieses optionale Feature bietet Ihnen tiefere Einblicke in die Sicherheitsrisiken der Images in Ihren Azure Resource Manager-basierten Registrierungen. Sie können das Paket auf Abonnementebene aktivieren oder deaktivieren, um alle Registrierungen in einem Abonnement abzudecken. Die Verwendung dieses Features wird wie auf der [Tarifseite](security-center-pricing.md) gezeigt pro Image in Rechnung gestellt. Durch Aktivierung des Pakets „Containerregistrierungen“ wird sichergestellt, dass Security Center bereit ist, Images zu überprüfen, die an die Registrierung gepusht werden. 

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemeine Verfügbarkeit|
|Preise:|Standard-Tarif|
|Unterstützte Registrierungen und Images:|![Ja](./media/icons/yes-icon.png) Von Linux gehostete ACR-Registrierungen, die über das öffentliche Internet zugänglich sind und den Shellzugriff ermöglichen.<br>![Nein](./media/icons/no-icon.png) Von Windows gehostete ACR-Registrierungen.<br>![Nein](./media/icons/no-icon.png) „Private“ Registrierungen – Security Center erfordert, dass Ihre Registrierungen über das öffentliche Internet zugänglich sind. Security Center kann derzeit keine Verbindung mit Registrierungen herstellen oder diese überprüfen, deren Zugriff durch eine Firewall, einen Dienstendpunkt oder private Endpunkte wie Azure Private Link beschränkt ist.<br>![No](./media/icons/no-icon.png) Extrem minimalistische Images wie [Docker-Scratch](https://hub.docker.com/_/scratch/)-Images oder Images ohne Distribution, die nur eine Anwendung und deren Laufzeitabhängigkeiten ohne Paket-Manager, Shell oder Betriebssystem enthalten.|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsleseberechtigter** und [Azure Container Registry-Leser](https://docs.microsoft.com/azure/container-registry/container-registry-roles)|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Nein](./media/icons/no-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||




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
Der [Abschnitt zur Verfügbarkeit](#availability) listet die Typen von Containerregistrierungen auf, die durch das Paket „Containerregistrierungen“ unterstützt werden. 

Wenn Registrierungen, die nicht unterstützt werden, mit Ihrem Azure-Abonnement verbunden sind, werden sie nicht überprüft, und sie werden Ihnen nicht in Rechnung gestellt.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* [Schützen von Computern und Anwendungen in Azure Security Center](security-center-virtual-machine-protection.md) – Beschreibt Empfehlungen des Security Center.
