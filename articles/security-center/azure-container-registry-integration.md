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
ms.date: 11/19/2019
ms.author: memildin
ms.openlocfilehash: 069ce6ca1e76a9bd954031708702c973387abbaa
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2020
ms.locfileid: "78205992"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Integration von Security Center in Azure Container Registry

Azure Container Registry (ACR) ist ein verwalteter, privater Docker-Registrierungsdienst, der Ihre Containerimages für Azure-Bereitstellungen in einer zentralen Registrierung speichert und verwaltet. Er basiert auf der Open-Source-Docker-Registrierung 2.0.

Wenn Sie den Standardtarif von Azure Security Center verwenden, können Sie das Paket „Containerregistrierungen“ hinzufügen. Dieses optionale Feature bietet Ihnen tiefere Einblicke in die Sicherheitsrisiken der Images in Ihren ARM-basierten Registrierungen. Sie können das Paket auf Abonnementebene aktivieren oder deaktivieren, um alle Registrierungen in einem Abonnement abzudecken. Die Verwendung dieses Features wird wie auf der [Tarifseite](security-center-pricing.md) gezeigt pro Image in Rechnung gestellt. Durch Aktivierung des Pakets „Containerregistrierungen“ wird sichergestellt, dass Security Center bereit ist, Images zu überprüfen, die an die Registrierung gepusht werden. 

Immer, wenn ein Image an Ihre Registrierung gepusht wird, überprüft Security Center es automatisch. Zum Auslösen der Überprüfung eines Images müssen Sie es also an Ihr Repository pushen.

Wenn die Überprüfung abgeschlossen ist (in der Regel nach ungefähr 10 Minuten), sind die Ergebnisse in Security Center-Empfehlungen wie folgt verfügbar:

[![Beispiel einer Azure Security Center-Empfehlung über in einem gehosteten Azure Container Registry-Image (ACR) erkannte Sicherheitsrisiken](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>Vorteile der Integration

Security Center identifiziert ARM-basierte ACR-Registrierungen in Ihrem Abonnement und bietet nahtlos Folgendes:

* **Azure-native Sicherheitsrisikoüberprüfung** für alle gepushten Linux-Images. Security Center überprüft das Image mithilfe eines Scanners von Qualys, dem branchenführenden Hersteller von Anwendungen zur Sicherheitsrisikoüberprüfung. Diese native Lösung ist standardmäßig nahtlos integriert.

* **Sicherheitsempfehlungen** für Linux-Images mit bekannten Sicherheitsrisiken. Security Center bietet Details zu den einzelnen gemeldeten Sicherheitsrisiken und eine Schweregradklassifizierung. Außerdem erhalten Sie Anleitungen zur Behebung der spezifischen Sicherheitsrisiken, die für jedes in die Registrierung gepushte Image gefunden wurden.

![Allgemeine Übersicht zu Azure Security Center und Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

* [Containersicherheit in Security Center](container-security.md)

* [Integration mit Azure Kubernetes Service](azure-kubernetes-service-integration.md)

* [Schützen von Computern und Anwendungen in Azure Security Center](security-center-virtual-machine-protection.md) – Beschreibt Empfehlungen des Security Center.
